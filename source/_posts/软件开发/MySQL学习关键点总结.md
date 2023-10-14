---
title: MySQL学习关键点总结
excerpt: MySQL
categories:
  - 编程
tags:
  - MySQL
  - Database
  - DBMS
date: 2019-01-01
updated: 2020-07-16
toc: true
toc_sticky: true
---

## errors

### user.primary

导入数据时，不断报出主键重复的错误，可能的原因：

1. 旧表中的索引并没有从系统中删除，导致导入的新表与旧表中的索引记录冲突
1. 数据库中旧表曾经使用过与新表相同的名称，然后旧表改名后，索引名称未能正确修改，导致旧表与新表的索引冲突

解决方案：

1. 重新建立一个数据库，导入数据不会再出现这种问题
1. 重新建立一个新的数据表，表的名字是以前没有使用过的，导入数据不会再出现这种问题，然后再把表改回自己需要的表名即可。

### timezone

- MySQL 8.0 加入对 UTC 支持后，连接会出错，可以使用 Custom 方式输入连接串，在连接串的最后加入 `?serverTimezone=UTC` 即可
    - 完整连接串：`jdbc: mysql://localhost:3306/mysql?serverTimezone=UTC`

### 1206

1206 - The total number of locks exceeds the lock table size

问题：因为数据量太大，无法完成数据库操作

解决办法 1：

进入 MySQL 的 Command Line

输入 `show variables like "%_buffer%";`

默认的 `innodb_buffer_pool_size=8388608` 8M

修改为 `SET GLOBAL innodb_buffer_pool_size=1073741824` 1G

解决办法 2：

前面的修改是临时的，如果需要永久修改，可以去 my.ini 文件中修改

可以通过 services.msc 找到 my.ini 的位置，打开后直接修改需要的目标数据，然后重启服务就可以了。

【注】：在 Windows 下不要使用 Notepad 修改，否则文件格式会变成 UTF-8 with BOM 格式，格式改变后会导致服务无法正常启动！

### 1130

ERROR 1130: Host is not allowed to connect to this MySQL server

问题：访问别的机器的 MySQL Server 被拒绝

解决办法：

```SQL
CREATE USER 'vScopeUserName'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'vScopeUserName'@'localhost' WITH GRANT OPTION;
CREATE USER 'vScopeUserName'@'vScopeServerIP' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'vScopeUserName'@'vScopeServerIP' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

代码说明

- vScopeUserName：希望创建的超级用户的名称，与最初的 `root` 名称不同。
- localhost：就是本地地址。
- vScopeServerIP：希望远程访问服务器的 IP 地址
- password：希望使用的密码

注：本质就是对 MySQL 的 `user` 表增加一个用户，并且赋予权限，熟悉的话，可以直接对 `user` 表进行数据操作，操作完成后需要重启数据库才能有效。

例子：

```SQL
create user 'admin'@'localhost' identified by 'admin';
grant all privileges on *.* to 'admin'@'localhost' with grant option;
create user 'admin'@'192.168.31.3' identified by 'admin';
grant all privileges on *.* to 'admin'@'192.168.31.3' with grant option;
FLUSH PRIVILEGES;
```

## Server

### Table Fragmentation

MySQL 表碎片化：因为数据被删除时，MySQL只对其进行标记，并没有真正进行物理删除，反复进行添加和删除后就会出现文件碎片。查看哪些表出现文件碎片及碎片的严重情况可以使用：

```mysql
SELECT * FROM `TABLES` WHERE TABLE_SCHEMA='tencent';
SELECT * FROM `TABLES` WHERE TABLE_NAME='click_log_all';
```

如果 `DATA_FREE` 字段值较大时，就说明这张表的碎片化有点严重，需要清理。

清理表中碎片的方法

```mysql
OPTIMIZE TABLE cick_log_all;
```

以上方法对于不同的表格式效果不同

- 对于 MyISAM，`OPTIMIZE TABLE` 的作用
    - 如果表中的行被删除或者拆分过，则修复这张表
    - 对于没有排序的索引页进行排序
    - 更新表的统计信息为最新状态
- 对于 InnoDB，`OPTIMIZE TABLE` 的作用
    - 等价于 `ALTER TABLE ... FORCE`，将会重建表、索引和统计信息，并且释放聚簇索引中未使用的空间

### Modify Password

[修改用户密码](https://dev.mysql.com/doc/refman/8.0/en/set-password.html)

设置 `root` 的密码为 `123456`

```SQL
SET PASSWORD FOR 'root'@'localhost' = '123456';
```

### Startup & Shutdown

使用 Windows Service 启动 和 停止

```bat
REM mysql80 是服务名称
net start mysql80
net stop mysql80
```

使用 Command Line 启动 和 停止

```bat
cd C: \Program Files\MySQL\MySQL Server 8.0\bin
mysqld --defaults-file="C: \ProgramData\MySQL\MySQL Server 8.0\my.ini"
mysqladmin shutdown -u root -p
REM mysqladmin 可以查看帮助文件
```

### Data Transfer

使用文件拷贝的方式就不用再尝试了，可能以前的版本学可以，后面因为安全的需要只会越来越难。建议使用 Navicat 工具完成数据迁移。

### Storage Engine

MySQL 常用的有三种存储引擎([详情参考](https://blog.csdn.net/ybdesire/article/details/83154312))：

1. MyISAM：选择密集型（筛选大量数据时非常迅速）、插入密集型的表（管理邮件或Web服务器日志数据）；
1. MEMORY：数据量小，且被频繁访问；
1. InnoDB：需要事务支持，并且有较高的并发读取频率。
1. 其他可参考 [MySQL 的文档](https://dev.mysql.com/doc/refman/8.0/en/storage-engines.html)

### Row_Format

MySQL 常用的Row_Format有两种：

1. FIXED：静态表[static/fixed ]，表里面不存在varchar、text以及其变形、blob以及其变形的字段，即每条记录所占用的字节一样。

- 优点读取快，缺点浪费额外一部分空间。
- fixed--->dynamic: 这会导致CHAR变成VARCHAR

1. DYNAMIC：动态表，表的字段类型有很多种

- 优点是节省空间,缺点是读取的时间的开销
- dynamic--->fixed: 这会导致VARCHAR变成CHAR

### Collation

mysql的collation大致的意思就是字符序。首先字符本来是不分大小的，那么对字符的>, = , < 操作就需要有个字符序的规则。collation做的就是这个事情，可以对表进行字符序的设置，也可以单独对某个字段进行字符序的设置。一个字符类型，它的字符序有多个。mysql的字符序遵从命名惯例。以_ci(表示大小写不敏感)，以_cs(表示大小写敏感)，以_bin(表示用编码值进行比较)。

## Performance

### `Count(*)`

如果数据表使用 InnoDB 引擎存储，则 `Count(*)`速度会变慢，因为 InnoDB 引擎是把数据全部读出来进行 `Count(*)` 的。

## SQL

### Aggregate Function Descriptions

[MySQL 支持的聚合函数](https://dev.mysql.com/doc/refman/5.7/en/aggregate-functions.html)
