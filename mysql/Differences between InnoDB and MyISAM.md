|  | InnoDB | MyISAM |
| --- | --- | --- |
| ACID | yes | no |
| Concurrency | supports a quite deep level of concurrency | multi-reader, single write |
| Locking | row-level | table-level|
| Crash Recovery | InnoDB's transaction system guarantees recovery of commited and written transactions after crash. | somewhat unpredicatable and messy. |
| Buffer pool | indexes and base-table data managed by the storage manager internally. | indexes are managed by the storage manager, while base-table data is just disk files. |
| Primary Key | InnoDB is index-organized, which means its base table structure is effectively a primary index organized around the table's primary key | MyISAM uses a secondary index to implement primary keys, which means access to them is slower |
| Full Index | yes | yes |