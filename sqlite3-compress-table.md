# SQLite3 压缩表空间方法

步骤：
1. 首先分析一下是哪里占用了空间
2. 对占用空间大的表进行清理
3. 压缩对应的表


这一次我们是对drone的DB进行压缩。

## 分析空间使用情况

从 [SQLite Download Page](https://www.sqlite.org/download.html) 下载 sqlite-tools，下载后解压，有一个叫做 sqlite3_analyzer 的工具。

```
# drone.sqlite 是数据库文件名
./sqlite3_analyzer drone.sqlite

# 运行部分结果如下

/** Disk-Space Utilization Report For drone.sqlite

Page size in bytes................................ 1024
Pages in the whole file (measured)................ 2082712
Pages in the whole file (calculated).............. 2082711
Pages that store data............................. 2082711    100.000%
Pages on the freelist (per header)................ 0            0.0%
Pages on the freelist (calculated)................ 1            0.0%
Pages of auto-vacuum overhead..................... 0            0.0%
Number of tables in the database.................. 16
Number of indices................................. 24
Number of defined indices......................... 10
Number of implied indices......................... 14
Size of the file in bytes......................... 2132697088
Bytes of user payload stored...................... 2118441888  99.33%

*** Page counts for all tables with their indices *****************************

LOGS.............................................. 2066980     99.24%
BUILDS............................................ 7747         0.37%
PROCS............................................. 6627         0.32%
CONFIG............................................ 1025         0.049%
PERMS............................................. 141          0.007%
REPOS............................................. 122          0.006%
SECRETS........................................... 27           0.001%
USERS............................................. 12           0.0%
SQLITE_MASTER..................................... 9            0.0%
MIGRATIONS........................................ 6            0.0%
FILES............................................. 4            0.0%
REGISTRY.......................................... 3            0.0%
SENDERS........................................... 3            0.0%
AGENTS............................................ 2            0.0%
TASKS............................................. 2            0.0%
SQLITE_SEQUENCE................................... 1            0.0%
```

可以看出来是LOGS表占用空间非常大。


## 清理表里的内容

```
sqlite3 drone.sqlite
# sqlite> 是命令提示符，非命令的一部分
sqlite> .help
# 查看数据库
sqlite> .databases
# 查看表
sqlite> .tables
# 查看表结构
sqlite> .schema logs
# 查看数据
sqlite> select * from logs order by log_id desc limit 1;
# 删除数据
sqlite> delete from logs where log_id < 40000;
```

## 压缩表空间

```
sqlite> VACUUM logs;
```

