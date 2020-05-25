### 5.5（2010）

默认存储引擎使用InnoDB；
提升多核可扩展性；

### 5.6（2013）

- 安全性加强
    - 不明文保存用户信息；
    - 使用SH-256加密用户密码；
- InnoDB加强
    - undo log可独立出系统表空间；
    - redo log最大可增长到512G；
    - 可设置 page size 为 8k 或者 4k；
- 优化器加强
    - explain 支持 delete, insert, replace, update；
    - 优化了子查询的效率；

### 5.7（2015）

- 安全性加强：
    - 加强管理员权限，如：可以加锁或者解锁用户的登录权限；
- InnoDB 加强：
    - 加强缓冲池的清理和加载；
    - 可配置缓存池大小；
- 支持原生分区；
- 支持存储 JSON 类型数据；

### 8.0

- InnoDB 加强
    - no wait 语句
- 安全和账户管理加强
    - grant 表使用 InnoDB 表，保证授权的用户操作的原子性；
    - 支持用户角色；
    - 维护用户密码变更记录；
- JSON 加强
    - 提供更多的函数，如： JSON_PRETTY 输出一个更易读的格式；
- 优化器
    - 支持利用索引进行倒序排序

## 引用
1. [MySQL Document: What Is New in MySQL version](https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html)