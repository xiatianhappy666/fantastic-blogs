## Redis分布式缓存/分布式锁
PS:
1. 缓存带来的性能提升可以采用Jmeter进行压测(Http请求)
2. Redisson: Redisson是Redis官方推荐的Java版的Redis客户端，功能十分强大（以上的锁通过提供看门狗解决），提供了各种分布式锁（可重入锁（Reentrant Lock）、公平锁、联锁（MultiLock）、红锁（RedLock）），redis分布式集群操作，和分布式服务等.

### Redis分布式锁
在秒杀场景中为避免超卖/少卖问题：采用分布式锁(独占排他锁)
(无法用JVM锁解决)
知识点：
0. 缓存操作需要具有原子性(因为redis是单线程模型)，不能分为2条命令操作
1. setex key value(random value) expireTime(random time), value保存在threadLocal
2. 解锁时，不能简单del key(避免解错锁). 需要delete if equals value, 可以使用lua脚本保证原子性
3. 新建线程给锁续期(If key exists)，线程设为Daemon线程(lifecycle和main线程一致)
```
new Thread

int time = ttl lockKey
if (time != -2)
	expire lockKey (time + 5)
```

### 高并发系统的常见缓存问题 & solution
引子：思考以下code在高并发场景有什么问题
```
// code sample about redis cache, but has some problems
Object redisObj = redisUtil.get(key);
// hit cache
if (redisObj != null)
	return (Order)redisObj;
try {
	// hit mysql
	Order order = orderMapper.selectById(key);
	if (order != null) {
		redisUtil.setex(key, order, 10, TimeUnit.MINUTES);
		return order;
	}
} finally {
	// something
}
return null;
```

#### 缓存击穿: DB有，缓存没有相应的数据
(可能是这条数据没人访问过，或者刚好过期)
solution1: 
大促前，服务器预热，即预先把秒杀商品加载进缓存（启动时+运行时）
solution2:
采用本地缓存, Capacity固定+淘汰策略LRU -> 保留热点数据(Guava)
solution3: 
对热点数据/秒杀数据，加分布式锁(独占排他锁)，则10000个请求只有1个能打进DB，代码改为如下：
```
// code sample about cache
Object redisObj = redisUtil.get(key);
// hit cache
if (redisObj != null)
	return (Order)redisObj;
//--- add here ---
redisUtil.lock(key); // 阻塞式的请求分布式锁
try {
	// re-query redis one more time
	Object redisObj = redisUtil.get(key);
	// re-hit cache, avoid hit DB
	if (redisObj != null)
		return (Order)redisObj;
//--- add here ---
	// hit mysql
	Order order = orderMapper.selectById(key);
	if (order != null) {
		redisUtil.setex(key, order, 10, TimeUnit.MINUTES);
		return order;
	}
} finally {
	// something
//--- add here ---
	redisUtil.unlock(key);
//--- add here ---
}
return null;
```

#### 缓存穿透：缓存和DB都没有相应的数据
solution:
1. 缓存空对象  代码简单  效果不好 （因为大量非法数据还是会打到缓存和DB）
2. bloom过滤器 代码复杂  效果很好
    - bloomFIlter放在JVM中，即增加以下代码(with Guava lib)，缺点：保存在内存中，不便于更新维护；不是分布式
    ```
    if (!bloomFilter.mightContain(key))
        throw new Exception("Invalid visit ID!");
    ```
    - bloomFilter放在redis中，利用redis中的bit位图操作(setbit key 100 1). 维护：利用定时任务更新
    - JVM vs redis对比：
        * 位数组  21亿==256M  JVM内存    数据不会进行持久化  
        * redis  42亿==512M   redis内存  redis的持久化数据  分布式


#### 缓存雪崩: when redis is down / 大部分数据过期失效
prevention：
1. redis搭建HA cluster
2. 错开数据过期时间
solution:
降级、熔断


### 缓存和数据库一致性问题
[先更新哪一个？采用延时双删方案](https://github.com/xiatianhappy666/fantastic-blogs/blob/master/doc/Distributed/Consistency_redis_DB.docx)