# MySQL基本操作

SQL 包含以下 4 部分：

#### 1）数据定义语言（Data Definition Language，DDL）

用来创建或删除数据库以及表等对象，主要包含以下几种命令：

- DROP：删除数据库和表等对象
- CREATE：创建数据库和表等对象
- ALTER：修改数据库和表等对象的结构

#### 2）数据操作语言（Data Manipulation Language，DML）

用来变更表中的记录，主要包含以下几种命令：

- SELECT：查询表中的数据
- INSERT：向表中插入新数据
- UPDATE：更新表中的数据
- DELETE：删除表中的数据

#### 3）数据查询语言（Data Query Language，DQL）

用来查询表中的记录，主要包含 SELECT 命令，来查询表中的数据。

#### 4）数据控制语言（Data Control Language，DCL）

用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对数据库中的用户设定权限。主要包含以下几种命令：

- GRANT：赋予用户操作权限
- REVOKE：取消用户的操作权限
- COMMIT：确认对数据库中的数据进行的变更
- ROLLBACK：取消对数据库中的数据进行的变更



## 查看或显示数据库

```
SHOW DATABASES [LIKE '数据库名'];
```

语法说明如下：

- LIKE 从句是可选项，用于匹配指定的数据库名称。LIKE 从句可以部分匹配，也可以完全匹配。
- 数据库名由单引号`''`包围。

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

- information_schema：主要存储了系统中的一些数据库对象信息，比如用户表信息、列信息、权限信息、字符集信息和分区信息等。
- mysql：MySQL 的核心数据库，类似于 SQL Server 中的 master 表，主要负责存储数据库用户、用户访问权限等 MySQL 自己需要使用的控制和管理信息。常用的比如在 mysql 数据库的 user 表中修改 root 用户密码。
- performance_schema：主要用于收集数据库服务器性能参数。

- sys：MySQL 5.7 安装完成后会多一个 sys 数据库。sys 数据库主要提供了一些视图，数据都来自于 performation_schema，主要是让开发者和使用者更方便地查看性能问题。

### 创建并查看数据库

```
mysql> CREATE DATABASE test_db;
Query OK, 1 row affected (0.00 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)

```

### 使用LIKE从句

```
1、完全匹配
mysql> SHOW DATABASES LIKE 'test_db';
+--------------------+
| Database (test_db) |
+--------------------+
| test_db            |
+--------------------+
1 row in set (0.00 sec)

2、查看名字以 db 结尾的数据库
mysql> SHOW DATABASES LIKE '%db';
+----------------+
| Database (%db) |
+----------------+
| test_db        |
+----------------+
1 row in set (0.00 sec)

3、查看名字以 test 开头的数据库
mysql> SHOW DATABASES LIKE 'test%';
+------------------+
| Database (test%) |
+------------------+
| test_db          |
+------------------+
1 row in set (0.00 sec)

4、查看名字中包含 ysq 的数据库
mysql> SHOW DATABASES LIKE '%ysq%';
+------------------+
| Database (%ysq%) |
+------------------+
| mysql            |
+------------------+
1 row in set (0.00 sec)
```



## 创建数据库

语法

```
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];
```

`[]`中的内容是可选的。

详情：

- <数据库名>：创建数据库的名称。MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，不能以数字开头，尽量要有实际意义。注意在 MySQL 中不区分大小写。
- IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时才能执行操作。此选项可以用来避免数据库已经存在而重复创建的错误。
- [DEFAULT] CHARACTER SET：指定数据库的字符集。指定字符集的目的是为了避免在数据库中存储的数据出现乱码的情况。如果在创建数据库时不指定字符集，那么就使用系统的默认字符集。
- [DEFAULT] COLLATE：指定字符集的默认校对规则。

注意：

- MySQL 的字符集（CHARACTER）和校对规则（COLLATION）是两个不同的概念。字符集是用来定义 MySQL 存储字符串的方式，校对规则定义了比较字符串的方式。

例子：

```
1、最简单的创建MySQL数据库的语句
mysql> CREATE DATABASE test_db;
Query OK, 1 row affected (0.00 sec)

当再次输入时
mysql> CREATE DATABASE test_db;
ERROR 1007 (HY000): Can't create database 'test_db'; database exists
提示不能创建test_db数据库，数据库已存在。
** 重点：MySQL不允许在同一系统下创建两个相同名称的数据库。

可以加上IF NOT EXISTS从句，就可以避免类似错误
mysql> CREATE DATABASE IF NOT EXISTS test_db;
Query OK, 1 row affected, 1 warning (0.00 sec)

2、创建MySQL数据库时指定字符集和校对规则
mysql> CREATE DATABASE IF NOT EXISTS test_db_char 
	-> DEFAULT CHARACTER SET utf8 
	-> DEFAULT COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

此时再查看数据库
mysql> SHOW CREATE DATABASE test_db_char;
+--------------+-----------------------------------------------------------------------+
| Database     | Create Database                                                       |
+--------------+-----------------------------------------------------------------------+
| test_db_char | CREATE DATABASE `test_db_char` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+--------------+-----------------------------------------------------------------------+
1 row in set (0.00 sec)
```



## 修改数据库

语法

```
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
```

详情：

- ALTER DATABASE 用于更改数据库的全局特性。
- 使用 ALTER DATABASE 需要获得数据库 ALTER 权限。
- 数据库名称可以忽略，此时语句对应于默认数据库。
- CHARACTER SET 子句用于更改默认的数据库字符集。

例子：

```
先查看原来test_db数据库的定义声明
mysql> SHOW CREATE DATABASE test_db;
+----------+--------------------------------------------------------------------+
| Database | Create Database                                                    |
+----------+--------------------------------------------------------------------+
| test_db  | CREATE DATABASE `test_db` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+--------------------------------------------------------------------+
1 row in set (0.00 sec)

进行修改
mysql> ALTER DATABASE test_db
    -> DEFAULT CHARACTER SET utf8
    -> DEFAULT COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

mysql> SHOW CREATE DATABASE test_db;
+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| test_db  | CREATE DATABASE `test_db` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.00 sec)
```



## 删除数据库

语法

```
DROP DATABASE [ IF EXISTS ] <数据库名>
```

详情：

- <数据库名>：指定要删除的数据库名。
- IF EXISTS：用于防止当数据库不存在时发生错误。
- DROP DATABASE：删除数据库中的所有表格并同时删除数据库。使用此语句时要非常小心，以免错误删除。如果要使用 DROP DATABASE，需要获得数据库 DROP 权限。

注意：

- MySQL 安装后，系统会自动创建名为 information_schema 和 mysql 的两个系统数据库，系统数据库存放一些和数据库相关的信息，如果删除了这两个数据库，MySQL 将不能正常工作。

例子：

```
创建测试数据库
mysql> CREATE DATABASE test_db_del;
Query OK, 1 row affected (0.00 sec)

查看所有数据库
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
| test_db_char       |
| test_db_del        |
+--------------------+
7 rows in set (0.00 sec)

删除该库
mysql> DROP DATABASE test_db_del;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
| test_db_char       |
+--------------------+
6 rows in set (0.00 sec)

再次删除该库
mysql> DROP DATABASE test_db_del;
ERROR 1008 (HY000): Can't drop database 'test_db_del'; database doesn't exist

使用 IF EXISTS从句
mysql> DROP DATABASE IF EXISTS test_db_del;
Query OK, 0 rows affected, 1 warning (0.00 sec)

注意：使用 DROP DATABASE 命令时要非常谨慎，在执行该命令后，MySQL 不会给出任何提示确认信息。DROP DATABASE 删除数据库后，数据库中存储的所有数据表和数据也将一同被删除，而且不能恢复。因此最好在删除数据库之前先将数据库进行备份。
```



## 选择数据库

语法：

```
USE <数据库名>
```

注意：该语句可以通知 MySQL 把`<数据库名>`所指示的数据库作为当前数据库。该数据库保持为默认数据库，直到语段的结尾，或者直到遇见一个不同的 USE 语句。 只有使用 USE 语句来指定某个数据库作为当前数据库之后，才能对该数据库及其存储的数据对象执行操作。

例子：

```
mysql> USE test_db;
Database changed
注意：在执行选择数据库语句时，如果出现“Database changed”提示，则表示选择数据库成功。

mysql> USE test_db_s;
ERROR 1049 (42000): Unknown database 'test_db_s'
如果没有该库会报错
```

