---
title: mysql操作
date: 2018-10-31 15:15:28
tags:
---
1、mysql的基本操作，增、删、减、改、查
2、mysql备份
3、myql用户管理
4、mysql事务

<!---more--->
数据库的操作无非就是增删改查，增删前需要新建数据库和数据表
## 1、 新建create
#### 新建数据库
CREATE DATABASE用来新建数据库
语法：

```
CREATE DATABASE database_name；
```
#### 新建数据数据表
CREATE TABLE用来新建数据表
mysql语法：

```
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
在新建数据表时对字段会用到一些约束
1、数据类型
2、NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。
3、UNIQE
```
UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。

PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。

请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束
```
4、PRIMARY KEY 约束唯一标识数据库表中的每条记录。
5、FORIMARY KEY  一个表中的 FOREIGN KEY 指向另一个表中的 UNIQUE KEY(唯一约束的键)。
6、CHECK  约束用于限制列中的值的范围。
7、ENGINE
8、DEFAULT CHARSET
10、DEFAULT  约束用于向列中插入默认值。如果没有规定其他的值，那么会将默认值添加到所有的新记录。
```
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
```
11、AUTO_INCREMENT 


#### 新建索引
CREATE INDEX 语句用于在表中创建索引。
在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。
注释：更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。
语法
```
CREATE INDEX index_name  ON table_name (column_name)
```
在Persion表中给LastName创建索引
```
CREATE INDEX PIndex ON Persons (LastName)
```

#### 新建视图
CREATE VIEW 
视图是可视化的表。
在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。
您可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。


## 2、增(即数据插入表操作)
#### INSERT INTO
INSERT INTO 语句用于向表中插入新记录。
INSERT INTO 语法:

```
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...),(value1,value2,value3,...),(value1,value2,value3,...),....;
```
#### INSERT INTO SELECT 语句
INSERT INTO SELECT 语句从一个表复制数据，然后把数据插入到一个已存在的表中。目标表中任何已存在的行都不会受影响。
SQL INSERT INTO SELECT 语法
我们可以从一个表中复制所有的列插入到另一个已存在的表中：
```
INSERT INTO table2
SELECT * FROM table1;
```
或者我们可以只复制希望的列插入到另一个已存在的表中：
```
INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
```
## 3、 删
1、DELETE 
DELETE 语句用于删除表中的行。
SQL DELETE 语法：
```
DELETE FROM table_name WHERE some_column=some_value;
```

2、TRUNCATE 
如果我们仅仅需要删除表内的数据，但并不删除表本身
```
TRUNCATE TABLE table_name
```
3、DROP
通过使用 DROP 语句，可以轻松地删除索引、表和数据库。
1）DROP TABLE 语句

```
DROP TABLE table_name
```
2）DROP DATABASE 语句

```
DROP DATABASE database_name
```
3）DROP INDEX 语句
用于 MySQL 的 DROP INDEX 语法：
```
ALTER TABLE table_name DROP INDEX index_name；
```

## 4、 改
1、ALTER
 ALTER TABLE 语句用于在已有的表中添加、删除或修改列。
 1）如需在表中添加列，请使用下面的语法:
```
ALTER TABLE table_name ADD column_name datatype
```
2）如需删除表中的列，请使用下面的语法

```
ALTER TABLE table_name DROP COLUMN column_name
```
3）要改变表中列的数据类型，请使用下面的语法

```
ALTER TABLE table_name MODIFY COLUMN column_name datatype
```
2、UPDATE
 UPDATE语句用于更新表中已存在的记录
 SQL UPDATE 语法：
```
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```
例子：

```
UPDATE Websites 
SET alexa='5000', country='USA' 
WHERE name='菜鸟教程';
```
注意：
在更新记录时要格外小心！在上面的实例中，如果我们省略了 WHERE 子句，会将 Websites 表中所有数据的 alexa 改为 5000，country 改为 USA。

## 5、查（表查询select）
数据操作的大部分在于查，其中用于查的条件有
1、DISTNCT
2、WHERE
3、AND & OR
4、ORDER BY  
5、LIKE
6、IN
7、BETWEEN
8、JOIN

```
INNER JOIN：如果表中有至少一个匹配，则返回行
LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行
RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行
FULL JOIN：只要其中一个表中存在匹配，则返回行
```
9、HAVING WHERE 关键字无法与聚合函数一起使用。HAVING 子句可以让我们筛选分组后的各组数据。
10、GROUP BY 语句根据一个或多个列对结果集进行分组。。
11、LIMIT
12、 AS给查的列取别名
13、UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

## 6、 mysql表结构，数据库配置数据等查询和修改
#### show语句
1、数据库信息查询
|命令|                         	描述|
|----|----|
|SELECT VERSION( )	 |         服务器版本信息|
|SELECT DATABASE( )	 |    当前数据库名 (或者返回空)|
|SELECT USER( )	  |             当前用户名|
|SHOW STATUS	|服务器状态|
|SHOW VARIABLES|	服务器配置变量|
|SHOW COLUMNS FROM table_name|查询表结构信息|
|DESCRIBE table_name|查询表结构信息|
|SELECT CREATE TABLE table_name|查询表结构信息|


2、修改数据库变量信息
有两种方法可以将值分配给用户定义的变量。
- 第一种方法是使用SET语句如下：

```
SET @variable_name = value;
```
- 第二种方法是使用SELECT语句

```
SELECT @variable_name := value;
```

## 7、SQL 函数
SQL 拥有很多可用于计数和计算的内建函数。

#### SQL Aggregate 函数
SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。

有用的 Aggregate 函数：

AVG() - 返回平均值
COUNT() - 返回行数
FIRST() - 返回第一个记录的值
LAST() - 返回最后一个记录的值
MAX() - 返回最大值
MIN() - 返回最小值
SUM() - 返回总和
#### SQL Scalar 函数
SQL Scalar 函数基于输入值，返回一个单一的值。
有用的 Scalar 函数：

UCASE() - 将某个字段转换为大写
LCASE() - 将某个字段转换为小写
MID() - 从某个文本字段提取字符，MySql 中使用
SubString(字段，1，end) - 从某个文本字段提取字符
LEN() - 返回某个文本字段的长度
ROUND() - 对某个数值字段进行指定小数位数的四舍五入
NOW() - 返回当前的系统日期和时间
FORMAT() - 格式化某个字段的显示方式

## 8、mysql用户管理
#### 新建用户
 CREATE USER语句创建用户帐户
```
CREATE USER dbadmin@192.168.1.100 
IDENTIFIED BY 'pwd123';
```
#### 查看用户访问权限

```
SHOW GRANTS FOR remote_user;
SHOW GRANTS FOR 'api@localhost';
```

#### 用户授权
创建新的用户帐户后，用户没有任何权限，如要向用户帐户授予权限，请使用GRANT语句。
GRANT语句的语法：

```
GRANT privilege,[privilege],.. ON privilege_level 
TO user [IDENTIFIED BY password]
[REQUIRE tsl_option]
[WITH [GRANT_OPTION | resource_option]];
```
可指定用户是否必须通过安全连接(如SSL，X059等)连接到数据库服务器。可选的WITH GRANT OPTION子句允许此用户授予其他用户或从其他用户删除您拥有的权限。此外，可以使用WITH子句来分配MySQL数据库服务器的资源，例如，设置用户每小时可以使用多少个连接或语句。这在MySQL共享托管等共享环境中非常有用。
例如：

```
GRANT ALL ON *.* TO 'super'@'localhost' WITH GRANT OPTION;
```
ON *.*子句表示MySQL中的所有数据库和所有对象。WITH GRANT OPTION允许super@localhost向其他用户授予权限。

#### 撤销用户某权限
要从用户帐户撤销权限，您可以使用MySQL REVOKE语句。

```
REVOKE   privilege_type [(column_list)]      
        [, priv_type [(column_list)]]...
ON [object_type] privilege_level
FROM user [, user]...
```
- 首先，在REVOKE关键字之后指定要从用户撤销的权限列表，需要用逗号分隔权限。
- 其次，在ON子句中指定要撤销权限的权限级别。
- 第三，在FROM子句中指定要撤销的权限的用户帐户

例如：

```
REVOKE UPDATE, DELETE ON yiibaidb.*  FROM rfc;
```

#### 删除用户
要删除一个或多个用户帐户，请按如下所示使用DROP USER语句：

```
DROP USER [IF EXISTS] user, [user],...;
```
除了删除用户帐户之外，DROP USER语句还会从所有授权表中删除所有权限。

#### MySQL角色
通常，MySQL数据库拥有多个相同权限集合的用户。以前，向多个用户授予和撤销权限的唯一方法是单独更改每个用户的权限，假如用户数量比较多的时候，这是非常耗时的。为了用户权限管理更容易，MySQL提供了一个名为role的新对象，它是一个命名的特权集合。如果要向多个用户授予相同的权限集，
则应如下所示：
- 首先，创建新的角色。
- 第二，授予角色权限。
- 第三，授予用户角色。

1、创建角色
```
CREATE ROLE IF NOT EXISTS 'crm_dev', 'crm_read', 'crm_write';
```
2、授予角色权限
```
GRANT SELECT ON crmdb.* TO crm_read;
```
3、将角色分配给用户帐户
```
GRANT crm_dev TO crm_dev1@localhost;

GRANT crm_read TO crm_read1@localhost;

GRANT crm_read, crm_write TO crm_write1@localhost, crm_write2@localhost;
```
4、设置默认角色
在向用户帐户授予角色时，当用户帐户连接到数据库服务器时，它不会自动使角色变为活动状态，要在每次用户帐户连接到数据库服务器时指定哪些角色应该处于活动状态，请使用SET DEFAULT ROLE语句。
```
SET DEFAULT ROLE ALL TO crm_read1@localhost;
```
5、撤销角色的权限
```
REVOKE INSERT, UPDATE, DELETE ON crmdb.* FROM crm_write;
```
6、删除角色
```
DROP ROLE role_name, role_name, ...;
```
如果要更改用户的权限，则需要仅更改授权角色的权限。这些更改角色的权限将对授予角色的所有用户生效。
## 9、维护MySQL数据库表
#### 分析表语句

```
ANALYZE TABLE table_name;
```
#### 优化表语句
MySQL提供了一个语句，允许您优化表以避免此碎片整理问题。 以下说明如何优化表：
```
OPTIMIZE TABLE table_name;
```
#### 检查表语句
MySQL允许您使用CHECK TABLE语句检查数据库表的完整性。以下说明CHECK TABLE语句的语法：
```
CHECK TABLE table_name;
```
#### 修复表语句
REPAIR TABLE语句允许您修复数据库表中发生的一些错误。 MySQL不保证REPAIR TABLE语句可以修复表所有可能的错误。

```
REPAIR TABLE table_name;
```
## 10、MySQL使用mysqldump备份数据库
#### 如何备份MySQL数据库
通过执行以下命令，所有数据库结构和数据将导出到一个[dump_file.sql]转储文件中。
```
mysqldump -u [username] –p[password] [database_name] > [dump_file.sql]
```
#### 如何仅备份MySQL数据库结构 

```
mysqldump -u [username] –p[password] –no-data [database_name] > [dump_file.sql]
```
#### 如何仅备份MySQL数据库数据

```
mysqldump –u root –p123456 –no-create-info yiibaidb > D:\worksp\bakup\backup003.sql
```
#### 如何将多个MySQL数据库备份到一个文件中
如果要通过[database_name]中的命令来备份多个数据库，只需单独的数据库名称即可。 如果要备份数据库服务器中的所有数据库，请使用选项-all-database。
```
mysqldump -u [username] –p[password]  [dbname1,dbname2,…] > [all_dbs_dump_file.sql]

mysqldump -u [username] –p[password] –all-database > [all_dbs_dump_file.sql]
```
## 11、mysql数据导入
#### mysql 命令导入

```
mysql -u用户名    -p密码    <  要导入的数据库数据(runoob.sql)
```
#### source 命令导入

```
mysql> source /home/abc/abc.sql  # 导入备份数据库
```
#### 使用 LOAD DATA 导入数据

```
mysql> LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;
```
#### 使用 mysqlimport 导入数据

```
$ mysqlimport -u root -p --local database_name dump.txt
password *****
```
- mysqlimport的常用选项介绍

|  选项| 功能 |
|---|---|
|-d or --delete	|新数据导入数据表中之前删除数据数据表中的所有信息|
|-f or --force	|不管是否遇到错误，mysqlimport将强制继续插入数据
|-i or --ignore	|mysqlimport跳过或者忽略那些有相同唯一 关键字的行， 导入文件中的数据将被忽略。
|-l or -lock-tables	|数据被插入之前锁住表，这样就防止了， 你在更新数据库时，用户的查询和更新受到影响。
|-r or -replace|	这个选项与－i选项的作用相反；此选项将替代 表中有相同唯一关键字的记录。
|--fields-enclosed- by= char	|指定文本文件中数据的记录时以什么括起的， 很多情况下 数据以双引号括起。 默认的情况下数据是没有被字符括起的。
|--fields-terminated- by=char|	指定各个数据的值之间的分隔符，在句号分隔的文件中， 分隔符是句号。您可以用此选项指定数据之间的分隔符。 默认的分隔符是跳格符（Tab）
|--lines-terminated- by=str|	此选项指定文本文件中行与行之间数据的分隔字符串 或者字符。 默认的情况下mysqlimport以newline为行分隔符。 您可以选择用一个字符串来替代一个单个的字符： 一个新行或者一个回车。
mysqlimport命令常用的选项还有-v 显示版本（version）， -p 提示输入密码（password）等。


## 12、mysql事务
#### 特性
1、原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

2、一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。

3、隔离性：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。

4、持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。
#### 事务控制语句：
- BEGIN或START TRANSACTION；显式地开启一个事务；

- COMMIT；也可以使用COMMIT WORK，不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改成为永久性的；

- ROLLBACK；有可以使用ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；

- SAVEPOINT identifier；SAVEPOINT允许在事务中创建一个保存点，一个事务中可以有多个SAVEPOINT；

- RELEASE SAVEPOINT identifier；删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；

- ROLLBACK TO identifier；把事务回滚到标记点；

- SET TRANSACTION；用来设置事务的隔离级别。InnoDB存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ和SERIALIZABLE。
#### MYSQL 事务处理主要有两种方法：
1、用 BEGIN, ROLLBACK, COMMIT来实现

BEGIN 开始一个事务
ROLLBACK 事务回滚
COMMIT 事务确认
2、直接用 SET 来改变 MySQL 的自动提交模式:

SET AUTOCOMMIT=0 禁止自动提交
SET AUTOCOMMIT=1 开启自动提交

## 13、 mysql以及sql注入
如果您通过网页获取用户输入的数据并将其插入一个MySQL数据库，那么就有可能发生SQL注入安全的问题。
本节将为大家介绍如何防止SQL注入，并通过脚本来过滤SQL中注入的字符。
所谓SQL注入，就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。
我们永远不要信任用户的输入，我们必须认定用户输入的数据都是不安全的，我们都需要对用户输入的数据进行过滤处理。
以下实例中，输入的用户名必须为字母、数字及下划线的组合，且用户名长度为 8 到 20 个字符之间：

```
if (preg_match("/^\w{8,20}$/", $_GET['username'], $matches))
{
   $result = mysqli_query($conn, "SELECT * FROM users 
                          WHERE username=$matches[0]");
}
 else 
{
   echo "username 输入异常";
}
```
让我们看下在没有过滤特殊字符时，出现的SQL情况：

```
// 设定$name 中插入了我们不需要的SQL语句
$name = "Qadir'; DELETE FROM users;";
 mysqli_query($conn, "SELECT * FROM users WHERE name='{$name}'");
```
