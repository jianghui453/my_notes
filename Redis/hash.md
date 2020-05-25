# hash

与 Java 中的 HashMap 类似，Redis 中的 Hash 比 Java 中的更高级一些。

Redis 本身就是 KV 服务器，除了 Redis 本身数据库之外，字典也是哈希键的底层实现。

字典的数据结构如下所示：

```
typedef struct dict{
      dictType *type;
    void *privdata;
    dictht ht[2];
    int trehashidx;
}


typedef struct dictht{
    //哈希表数组
    dectEntrt **table;
    //哈希表大小
    unsigned long size;
    //
    unsigned long sizemask;
    //哈希表已有节点数量
    unsigned long used;
}
```

重要的两个字段是 dictht 和 trehashidx，这两个字段与 rehash 有关，下面重点介绍 rehash。

## Rehash

学过 Java 的朋友都应该知道 HashMap 是如何 rehash 的，在此处我就不过多赘述，下面介绍 Redis 中 Rehash 的过程。

由上段代码，我们可知 dict 中存储了一个 dictht 的数组，长度为 2，表明这个数据结构中实际存储着两个哈希表 ht\[0\] 和 ht\[1\]，为什么要存储两张 hash 表呢？

当然是为了 Rehash，Rehash 的过程如下：

为 ht\[1\] 分配空间。如果是扩容操作，ht\[1\] 的大小为第一个大于等于 ht\[0\].used*2 的 2^n；如果是缩容操作，ht\[1\] 的大小为第一个大于等于 ht\[0\].used 的 2^n。
将 ht\[0\] 中的键值 Rehash 到 ht\[1\] 中。
当 ht\[0\] 全部迁移到 ht\[1\] 中后，释放 ht\[0\]，将 ht\[1\] 置为 ht\[0\]，并为 ht\[1\] 创建一张新表，为下次 Rehash 做准备。

## 渐进式 Rehash

rehash是在hash table的大小不能满足需求，造成过多hash碰撞后需要进行的扩容hash table的操作。

由于 Redis 的 Rehash 操作是将 ht\[0\] 中的键值全部迁移到 ht\[1\]，如果数据量小，则迁移过程很快。但如果数据量很大，一个 Hash 表中存储了几万甚至几百万几千万的键值时，迁移过程很慢并会影响到其他用户的使用。

为了避免 Rehash 对服务器性能造成影响，Redis 采用了一种渐进式 Rehash 的策略，分多次、渐进的将 ht\[0\] 中的数据迁移到 ht\[1\] 中。

前一过程如下：

为 ht\[1\] 分配空间，让字典同时拥有 ht\[0\] 和 ht\[1\] 两个哈希表。
字典中维护一个 rehashidx，并将它置为 0，表示 Rehash 开始。
在 Rehash 期间，每次对字典操作时，程序还顺便将 ht\[0\] 在 rehashidx 索引上的所有键值对 rehash 到 ht\[1\] 中，当 Rehash 完成后，将 rehashidx 属性+1。当全部 rehash 完成后，将 rehashidx 置为 -1，表示 rehash 完成。
注意，由于维护了两张 Hash 表，所以在 Rehash 的过程中内存会增长。另外，在 Rehash 过程中，字典会同时使用 ht\[0\] 和 ht\[1\]。

所以在删除、查找、更新时会在两张表中操作，在查询时会现在第一张表中查询，如果第一张表中没有，则会在第二张表中查询。但新增时一律会在 ht\[1\] 中进行，确保 ht\[0\] 中的数据只会减少不会增加。