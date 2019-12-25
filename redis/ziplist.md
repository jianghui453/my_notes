# ziplist

压缩列表 ziplist 是为 Redis 节约内存而开发的，是列表键和字典键的底层实现之一。

当元素个数较少时，Redis 用 ziplist 来存储数据，当元素个数超过某个值时，链表键中会把 ziplist 转化为 linkedlist，字典键中会把 ziplist 转化为 hashtable。

ziplist 是由一系列特殊编码的连续内存块组成的顺序型的数据结构，ziplist 中可以包含多个 entry 节点，每个节点可以存放整数或者字符串。

![](../Images/Redis/ziplist/1.jpg)

由于内存是连续分配的，所以遍历速度很快。