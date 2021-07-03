## Point-in-Time Recovery

To recover an InnoDB database to the present from the time at which the physical backup was made, you must run MySQL server with binary logging enabled, even before taking the backup. To achieve point-in-tim recovery after restoring a backup, you can apply changes from the binary log that accurred after the backup was made.

## Recovery from Data Corruption or Disk Failure

If your database becomes corrupted or disk failure occurs, you must perform the recovery using a backup. In the case corruption, first find a backup that is not corrupted. After restoring the base backup, do a point-in-tim revocery from the binary log files using mysqlbinlog and mysql to restore the changes that occurred after the backup was made.

## InnoDB Crash Recovery

To recover from a MySQL server crash, the only requirement is to restart the MySQL server. InnoDB automatically checkos the logs and performs a roll-forward of the database to present. InnoDB automatically rools back uncommitted transactions that were present at the time of the crash. 

## InnoDB crash recovery consists of serveral steps:

1. Tablespace discovery: Tablespace discovery is the process that InnoDB uses to identify tablespaces that 	require redo log application.
1. Redo log application: Redo log application is performed during initialization, before accepting any 			connections. If all changes are flushed from the buffer pool to the tablespaces(ibdata* and *.ibd files) at the time of the shutdown or crash, redo log application is skipped. InnoDB also skips redo log application if redo log files are missing at startup.
    1. The current maximum auto-increment counter value is written to the redo log each time the value changes, which makes it crash-safe. During recovery, InnoDB scans the redo log to collect counter value changes and applies the changes to the in-memory table object.
    1. When encountering index tree corruption, InnoDB writes a corruption flag to the redo log, which makes the corruption crash-safe, InnoDb also writes in-memory corruption flag data to an engine-private system table on each checkpoint. During recovery, InnoDB reads corruption flags from both locations and merges results before marking in-memory table and index objects as corrupt.
    1. Removing redo logs to speed up recovery is not recommended, even if some data loss is acceptable. Removing redo logs should only be considered after a clean shutdown, with innodb_fast_shutdown set to 0 or 1.
1. Roll back of incomplete transactions: incomplete transactions are any transactions that were active at the time of crash or fast shutdown. The time it takes to roll back an incomplete transaction can be three or four times the amount of time atransaction is active before it is interrupted, depending on server load.</br>
You cannot cancel transactions that are being rooled back. In extreme cases, when rolling back transactions is expected to take an exceptionally long time, it may be faster to start InnoDB with an innodb_force_recovery setting of 3 or greater.
1. Change buffer merge: Applying changes from the change buffer (part of the system tablespace) to leaf pages of secondary indexes, as the index pages are read to the buffer pool.
1. Purge: Deleteing delete-marked records that are no loner visible to active transactions.
