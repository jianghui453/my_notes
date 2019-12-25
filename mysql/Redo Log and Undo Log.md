# Redo

The redo log is a disk-based data structure used during crash recouvery to correct data written by incomplete transactions.

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
