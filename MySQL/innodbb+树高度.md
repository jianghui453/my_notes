### 如何计算b+树的阶数

mysql 5.7 之后的page大小默认的16k。这个数值可以配置。聚集索引是一棵b+树，非叶子节点由主键索引 + 子节点指针组成（6个字节）。如果主键索引的为bigint类型，即8个字节。那么一页可以保存 16 * 1024 / (8 + 6) = 1170 个键值对，即b+树为1170阶（实际由于每个页还保存了LSN等数据，b+树的阶数会小于这个值）。那么树高为3时可以保存 1170 * 1170 * 16 = 21902400条记录。

### 引用

[为什么生产环境中B+树的高度总是3-4层？](https://zhuanlan.zhihu.com/p/86137284)
[mysql Innodb B+树索引的层高是多少，每个节点能存多少数据](https://www.bloghome.com.cn/post/innodbde-b-shu-suo-yin-dao-di-neng-cun-duo-shao-shu-ju-cheng-xu-yuan-da-ben-ying.html)