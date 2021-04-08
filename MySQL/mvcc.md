# MVCC

InnoDB is a multi-versioned storage engine: it keeps information about old versions of changed rows, to support transactional features such as concurrency and rollback. This Information is stored in the tablespace in data structure called a rollback segment (agter an analogous data structure in Oracle).

InnoDB uses the information in the rollback segment to perform the undo operations needed in a transaction rollback. It also uses the information to build earlier versions of a row for a consistent read.

Internally, InnoDB adds three fields to each row stored in the database

- A 6-byte DB\_TRX\_ID field indicates the transaction identifier for the last transaction that inserted or updated the row. Also, a deletion is treated internally as an update where a special bit in the row is set to mark it as deleted.

- Each row also contains a  7-byte DB\_ROLL\_PTR field called the roll pointer. The roll pointer points to an **undo log** record written to the roolback segment. If the row was updated, the undo log record contains the information necessary to rebuild the content of the row before it was updated.

- A 6-byte DB\_ROW\_ID field contains a row ID that increase monotonically as new rows are inserted. If InnoDB generates a clustered index automatically, the index contains row ID values. Otherwise, the DB\_ROW\_ID column does not appear in any index.

In the InnoDB multi-versioning scheme, a row is not physically removed from the database immediately when you delete it with an SQL statement. InnoDB only physically removes the corresponding row and its index records when it discards the update undo log record written for the deletion. This removal operation is called a purge, and itis quite fast, usually taking the same order of time as the SQL statement that did the deletion.

InnoDB multi-version concurrency control (MVCC) treats secondary indexs differently than clustered indexes.
- Records in a clustered index are updated in-place, and their hidden system columns point undo log entries from which earlier from which earlier versions of records can be reconstructed.
- When a secondary index column is updated, old secondary index records are delete-marked, new records are inserted, and delete-marked records are eventually purged. When a secondary index record is delete-marked or the secondary index page is updated by newer transacton, InnoDB looks up the database record in the clustered index. In the clustered index, the record`s is DB\_TRX\_ID is checked, and the correct version od the record is retrieved from the undo log if record was modified after the reading transaction was initiated.
- If secondary index record is marked for deletion or the secondary index page is updated by a newer transaction, the covering index technique is not used. Instead of returing values from the index structure, InnoDB looks up the record in the clustered index.

### 引用

1. [MySQL Document: InnoDB Multi-Versioning](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)