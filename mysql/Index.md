## Hash Index

哈希索引基于哈希表实现，只有精确匹配索引所有列的查询才有效。

### 哈希索引的限制

1. 哈希索引只包含哈希值和行指针，而不存储字段值，所以不能使用索引中的值来避免读取行。
1. 哈希索引数据并不是按找索引值顺序存储的，所以也就无法用于排序。
1. 哈希索引也不支持部分索引列匹配查找，因为哈希索引始终是使用索引列的全部内容来计算哈希值的。
1. 哈希索引只支持等职比较查询，包括=、IN、<=>、。也不支持任何范围查询。
1. 当出现哈希冲突的时候，存储引擎必须遍历链表中所有的行指针，逐行进行比较，知道找到所有符合条件的行。
1. 如果哈希冲入很多的化，一些索引维护操作的代价也会很高。

### 示例

1. 建表：creata table testhash ( fname varchar(50) not null, lname varchar(50) not null, key using hash(fname))engine=memoory;
1. 表中包含数据[(Arjen, Lentz), (Baron, Schwartz), (Perter Zaitev), (Vadim, Tkachenko)]
1. 假设索引使用家乡的哈希函数f()，它返回下面的值：f('Arjen')=2323; f('Baron')=7437; f('Peter')=8784; f(Vadim')=2458;
1. 则哈希索引的数据结构如下：[(2323, ptr_1), (2458, ptr_4), (7437, ptr_2), (8784, ptr_3)]
1. 每个槽的编号的顺序的，但是数据行不是顺序的。下面来看一句查询：select lname from testhash where fname='Perter';
1. MySQL先计算'Perter'的哈希值，并使用该值寻找对应的记录指针。因为f('Perter')=8784，所以MySQL在索引中查找8784，可以找到指向第3行的指针，最后一步是比较第三行的值是否为'Perter'，以确保就是要查找的行。