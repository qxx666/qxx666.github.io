---
title: mysql的一些记录
date: 2018-09-02 11:43:45
tags:
---
mysql学习的一些记录
1、数据备份
2、MySQL优化
3、MySQL状态的一些查询

<!--more-->


### mysql数据备份
###### 数据备份类型
1、完全备份
2、部分备份

 - 增量备份
 - 差异备份
###### mysql数据备份的方式
1、热备份：数据在执行备份时，读写不影响
2、温备份：数据库在执行备份时，读可以执行，写不可以执行
3、冷备份：数据库在备份时，读写都不可以执行
###### mysql支持的存储引擎
1、myisam：支持温备份和冷备份
2、innodb：三种备份方式都支持

###### 备份方式
1、物理备份：通过tar、cp等直接打包复制数据库文件
2、逻辑备份：通过特定的工具，从数据库中导出数据并另存备份（逻辑备份会丢失数据精度）

###### 备份的数据
1、数据
2、二进制日志，innodb日志
3、代码（存储过程，存储函数，触发器，事件调度器）
4、服务器配置文件

###### 备份工具
1、mysqldum ：逻辑备份工具
2、cp，tar 等归档复制工具
3、lvm2，snapshot 肌肤热备，借助文件系统管理工具进行备份
4、mysqlhotcopy 几乎冷备，仅支持mysiam
5、xtrabackup

 - 数据量小用第二种方式对数据进行备份
 - 数据量中等用第一中方式，先用mysqldump对数据库进行完全备份，然后定期被封binary_log达到增量备份
 - 数据量一般而又不过分影响业务进行，使用第三种备份方式
 - 数据量大，可以运用第五种方式对数据进行备份




### mysql优化
1、调优的几个参数

```
[client]
port = 3306
socket = /data/3306/mysql.sock

[mysql]
no-auto-rehash

[mysqld]
user = mysql
port = 3306
socket = /data/3306/mysql.sock
basedir = /usr/local/mysql
datadir = /data/3306/data
open_files_limit = 10240
back_log = 600
max_connections = 3000
max_connect_errors = 6000
table_cache = 614
external-locking = FALSE
max_allowed_packet = 32M

sort_buffer_size = 2M

Sort_Buffer_Size 是一个connection级参数，在每个connection（session）第一次需要使用这个buffer的时候，一次性分配设置的内存。
Sort_Buffer_Size 并不是越大越好，由于是connection级的参数，过大的设置+高并发可能会耗尽系统内存资源。例如：500个连接将会消耗 500*sort_buffer_size(8M)=4G内存
Sort_Buffer_Size 超过2KB的时候，就会使用mmap() 而不是 malloc() 来进行内存分配，导致效率降低。

join_buffer_size = 2M
thread_cache_size = 300
thread_concurrency = 8
query_cache_size = 64M
query_cache_limit = 4M
query_cache_min_res_unit = 2k
default-storage-engine = MyISAM
default_table_type = InnoDB
thread_stack = 192K
transaction_isolation = READ-COMMITTED
tmp_table_size = 256M
max_heap_table_size = 256M
long_query_time = 2
log_long_format
log-slow-queries=/data/3306/slow-log.log
binlog_cache_size = 4M
max_binlog_cache_size = 8M
max_binlog_size = 512M
expire_logs_days = 7
key_buffer_size = 2048M
read_buffer_size = 1M
read_rnd_buffer_size = 16M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_max_extra_sort_file_size = 10G
myisam_repair_threads = 1
skip-name-resolve
lower_case_table_names = 1
server-id = 1
innodb_additional_mem_pool_size = 16M
innodb_buffer_pool_size = 2048M
innodb_data_file_path = ibdata1:1024M:autoextend
innodb_file_io_threads = 4
innodb_thread_concurrency = 8
innodb_flush_log_at_trx_commit = 2
innodb_log_file_size = 128M
nnodb_log_buffer_size = 16M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120
innodb_file_per_table = 0
wait_timeout = 10
独享表空间（关闭）

[mysqldump]
quick
max_allowed_packet = 32M

[mysqld_safe]
log-error=/data/3306/mysql_oldboy.err
pid-file=/data/3306/mysqld.pid

```

2、软件优化

 - 选择合适的存储引擎
 - 正确使用索引
 - 避免使用select *
 - 字段尽量设置为NOT NULL

3、硬件优化

 - linux内核用内存开缓存存放数据
 - 增加应用缓存
 - 用ssd代替机械硬盘
 - 用ssd+sata混合存储

4、架构优化

 - 分表
 - 读写分离：读维护一些机器，写维护一些机器，二进制文件的主从复制，延迟解决方案，
 - 分库

5、SQL慢查询分析，调参数
慢查询指执行超过一定时间的SQL查询，语句记录到慢查询日志，方便开发人员看日志
找问题：

```
long_quary_time          //定义慢查询时间
slow_query_log          //设置慢查询开关
slow_query_log_file     //设置慢查询日志文件路径
```
设置方法1:命令式

```
set log_query_time=1
set slow_query_log=on       //设置慢查询开关
set slow_query_log_file='/data/slow_log'     //设置慢查询日志文件路径
```
设置方法2：修改配置文件参数

```
vim /etc/my.cnf
[mysqld]
log_query_time=1
slow_query_log=on       //设置慢查询开关
slow_query_log_file='/data/slow_log'
```


##### MYSQL的一些状态查询
###### 状态或参数查询
1、show status;
2、show variables;
show variables like '%character%';    //查看mysql字符集
3、show processlist;       //查看mysql正在执行的操作
###### 参数修改
1、set  global  variables=？
例如： set global general_log = on;
