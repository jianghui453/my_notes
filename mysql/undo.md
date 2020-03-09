# Undo

1. An undo log record contains information about how to undo the latest change by a transaction to a clustered index record.
1. If another transaction needs to see the original data as part of a consistent read operation, the unmodified data is retrieved from undo log records.
1. Undo logs exist within undo log segments, which are contained within rollback segments. Rollback segments reside in undo tablespaces and in the global temporary tablespace.
    1. Undo logs that reside in the global temporary tablespace are used for transactions that modify data in user-defined temporary tables.
    1. These undo logs are not redo-logged, as they are not required for crash recovery.
    1. They are used only for rollback while the server is running. This type of undo log benefits performance by avoiding redo logging I/O.
1. Undo logs in the rollback segment are divided into insert and update undo logs. 
    1. Insert undo logs are needed only in transaction rollback and can be discarded as soon as the transaction commits.
    1. Update undo logs are used also in consistent reads, but they can be discarded only agfter there is no transaction present for which InnoDB has assigned a snapshot that in a consistent read could need the information in the update undo log to build an earlier version of a database row.

## 基本概念

### 主要作用

1. 提供回滚
1. 多个行版本控制(MVCC)

在数据修改的时候，不仅记录了redo，还记录了相对应的undo，如果因为某些原因导致事务失败或回滚了，可以借助该undo进行回滚。

undo log和redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的update记录。

当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。有时候应用到行版本控制的时候，也是通过undo log来实现的：当读取的某一行被其他事务锁定时，它可以从undo log中分析出该行记录以前的数据是什么，从而提供该行版本信息，让用户实现非锁定一致性读取。

undo log是采用段(segment)的方式来记录的，每个undo操作在记录的时候占用一个undo log segment。

另外，undo log也会产生redo log，因为undo log也要实现持久性保护。
