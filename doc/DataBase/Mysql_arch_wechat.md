### InnonDB原理解析+技巧
1. [MyISAM和InnoDB的主要区别](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961428&idx=1&sn=31a9eb967941d888fbd4bb2112e9602b&chksm=bd2d0d888a5a849e7ebaa7756a8bc1b3d4e2f493f3a76383fc80f7e9ce7657e4ed2f6c01777d&scene=21#wechat_redirect)
2. [MyISAM与InnoDB的索引差异](https://mp.weixin.qq.com/s/FUXPXKfKyjxAvMUFHZm9UQ)
3. [数据库为什么要用B+树作为索引](https://mp.weixin.qq.com/s/YMbRJwyjutGMD1KpI_fS0A)
4. [InnoDB并发高的原因：锁，事务模型, MVCC, 快照读](https://mp.weixin.qq.com/s/R3yuitWpHHGWxsUcE0qIRQ)
5. [或许你不知道的12条SQL技巧](https://mp.weixin.qq.com/s/ap9tkaEuWDi39u0NFxnACA)


### InnoDB的锁控制
- [引子1](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961431&idx=1&sn=4f46fbada3d99ca6cf74b305d06c1ac6&chksm=bd2d0d8b8a5a849d8cb5a616c957abde7a6485cd2624372b84a5459eed081bd95429a09572f8&scene=21#wechat_redirect)
- [引子2](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961451&idx=1&sn=1bac366be5ad2dc721f79c9cb8e65e34&chksm=bd2d0db78a5a84a101e05a02e337fe91c3fd179132bced897156e1f34f0d0ba7e48dc89a1b95&scene=21#wechat_redirect)
- [自增锁](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961455&idx=1&sn=4c26a836cff889ff749a1756df010e0e&chksm=bd2d0db38a5a84a53db91e97c7be6295185abffa5d7d1e88fd6b8e1abb3716ee9748b88858e2&scene=21#wechat_redirect)
- [共享/排他锁，意向锁，插入意向锁](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961461&idx=1&sn=b73293c71d8718256e162be6240797ef&chksm=bd2d0da98a5a84bfe23f0327694dbda2f96677aa91fcfc1c8a5b96c8a6701bccf2995725899a&scene=21#wechat_redirect)
- [记录锁、间隙锁、临键锁](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961471&idx=1&sn=da257b4f77ac464d5119b915b409ba9c&chksm=bd2d0da38a5a84b5fc1417667fe123f2fbd2d7610b89ace8e97e3b9f28b794ad147c1290ceea&scene=21#wechat_redirect)
- [各种SQL在不同隔离级别下分别用到了什么锁？](https://mp.weixin.qq.com/s/wGOxro3uShp2q5w97azx5A)


### 数据库4种事务的隔离级别，以及InnoDB中的SQL实现
- [数据库4种事务的隔离级别，InnoDB如何巧妙实现?  各种SQL在不同隔离级别下分别用到了什么锁？](https://mp.weixin.qq.com/s/x_7E2R2i27Ci5O7kLQF0UA)