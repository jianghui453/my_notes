### 叶子节点如何组织数据

b+树叶子节点是链表。而且叶子节点的数据并不限于一页，可以有多张页连起来。所以查询的时候通过索引找到叶子节点，然后使用二分法查询具体的索引。

### 引用

[mysql Innodb B+树索引的层高是多少，每个节点能存多少数据](https://www.bloghome.com.cn/post/innodbde-b-shu-suo-yin-dao-di-neng-cun-duo-shao-shu-ju-cheng-xu-yuan-da-ben-ying.html)