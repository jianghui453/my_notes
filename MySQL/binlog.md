# binlog

## 作用

1. 复制：MySQL Replication在Master端开启binlog，Master把它的二进制日志传递给slaves并回放来达到master-slave数据一致的目的
1. 数据恢复：通过mysqlbinlog工具恢复数据
1. 增量备份

## binlog 主备复制

![](images/binlog/1.jpg)
