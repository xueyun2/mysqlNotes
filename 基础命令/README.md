# SQL基础命令

## SQL分类

- `DDL`：数据定义语言，用来定义数据库对象（数据库，表，字段）
- `DML`：数据操作语言，用来对数据库表中的数据进行增删改查。
- `DQL`：数据查询语言，用来查询数据库中表的记录。
- `DCL`：数据控制语言，用来创建数据库用户，控制数据库的访问权限。

## DDL数据库操作

> 在命令后面要加上`;`

**查询：**

|语法|说明|
|---|---|
|`SHOW DATABASES;`|查询所有数据库 |
|`SELECT DATABASE();`|查询当前数据库|

查看所有数据库：

```shell
mysql> SHOW DATABASES; 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydatabase         |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

查看当前数据库：

```shell
mysql> SELECT DATABASE(); 
+------------+
| DATABASE() |
+------------+
| mydatabase |
+------------+
1 row in set (0.00 sec)
```

**创建：**

|语法|说明|参数|
|---|---|---|
|`CREATE DATABASE`|创建数据库 |`IF NOT EXISTS`：创建不存在的数据库，`DEFAULT CHARSET`：设置字符集一般设置：`utf8mb4`，`COLLATE`：排序规则|

创建名为：`myDataBase`的数据库。

```shell
mysql> CREATE DATABASE myDataBase;
Query OK, 1 row affected (0.01 sec)
```

**删除：**

|语法|说明|参数|
|---|---|---|
|`DROP DATABASE`|删除指定数据库|`IF EXISTS`：判断是否存在|

删除名为：`mydatabase`的数据库。

```shell
mysql>  DROP DATABASE IF EXISTS mydatabase;
Query OK, 0 rows affected (0.01 sec)
```

**使用：**

`USE`：数据库名

使用：`mydatabase`数据库。

```shell
mysql> USE mydatabase;
Database changed
```

## DDL操作表

- `SHOW TABLES`：查询当前数据库所有表
- `DESC 表名;` ：查询表结构
- `SHOW CREATE TABLE 表名;` ：查询指定表的建表语句
- `CREATE TABLE 表名（字段1 字段1类型[COMMENT 字段注释]）[COMMENT 表注释],`：创建表，**注意：最后一个字段没有逗号**

**创建表：**

```shell
mysql> CREATE TABLE TB_USER(
    -> id int COMMENT '编号',
    -> name varchar(50) COMMENT '姓名',
    -> age int COMMENT '年龄',
    -> gender varchar(50) COMMENT '性别'
    -> ) COMMENT '用户表';
Query OK, 0 rows affected (0.04 sec)
```

**查询当前使用数据库所有表：**

```shell
mysql> SHOW TABLES;
+----------------------+
| Tables_in_mydatabase |
+----------------------+
| tb_user              |
+----------------------+
1 row in set (0.00 sec)
```

**查询表结构：**

```shell
mysql> DESC tb_user;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| id     | int         | YES  |     | NULL    |       |
| name   | varchar(50) | YES  |     | NULL    |       |
| age    | int         | YES  |     | NULL    |       |
| gender | varchar(50) | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

**查询指定表的建表语句：**

```shell
mysql> SHOW CREATE TABLE tb_user;
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table   | Create Table


                                                            |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tb_user | CREATE TABLE `tb_user` (
  `id` int DEFAULT NULL COMMENT '编号',
  `name` varchar(50) DEFAULT NULL COMMENT '姓名',
  `age` int DEFAULT NULL COMMENT '年龄',
  `gender` varchar(50) DEFAULT NULL COMMENT '性别'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci COMMENT='用户 表' |
+---------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)
```

## DDL操作表-类型