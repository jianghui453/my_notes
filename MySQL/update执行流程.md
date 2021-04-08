# update 执行流程

![update执行流程](images/update执行流程/update执行流程.jpg)

1. 连接器。
1. 分析器，词法/语法分析。
1. 优化器。
1. 执行器。
1. 写undo log。
1. 更新索引。
1. 写redo log。
1. 写 binlog。
1. 提交事务。

## 引用

1. [面试官：一条MySQL更新语句是如何执行的？](https://zhuanlan.zhihu.com/p/146968292)
