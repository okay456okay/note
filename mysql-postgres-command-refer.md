# MSQL Postgresql命令对照表

因工作中需要用到 Postgresql ，但对 Postgresql 又不太熟悉，因此记录一下，方便以后对照使用。

mysql                                      |postgresql                                 |note
-------------------------------------------|-------------------------------------------|--------------------------
mysql -u user -h host -P 3306 dbname -p    | psql -U user -h host -p 5432 dbname       | connect database command
SHOW DATABASES;                            | \l                                        |
USE db-name;                               | \c db-name                                |
SHOW TABLES;                               | \dt                                       |
SELECT * FROM table-name;                  | select * from table-name;                 |
DESC table-name;                           | \d table-name                             |
SHOW PROCESSLIST;                          | SELECT * FROM pg_stat_activity;           |
SELECT now()\G                             | \x  # 打开和关闭类似\G功能                   |
SELECT * from table-name limit 5;          | SELECT * from table-name limit 5;         |
SOURCE /path.sql                           | \i /path.sql                              |
LOAD DATA INFILE ...                       | \copy ...                                 |
\h                                         | \?                                        | help
exit or quit                               | \q                                        |
   




## 参考来源
>
* [PostgreSQL 与 MySQL 常用命令对照 - 代码天地](https://www.codetd.com/article/4159747)
* [MySQL & PostgreSQL 小命令对比-mysql教程-PHP中文网](https://www.php.cn/mysql-tutorials-133332.html)
* [PostgreSQL与MySQL常用命令比较[转] - 企业宝 - 博客园](https://www.cnblogs.com/qiyebao/p/4749146.html)

