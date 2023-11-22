# SQL基础命令

> 学习教程地址：<https://www.bilibili.com/video/BV1Kr4y1i7ru/?p=8&spm_id_from=333.788.top_right_bar_window_history.content.click>

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

### 数字类型

> 无符号指的是没有负数的数值范围。

- 年龄字段的类型定义，由于年龄没有负数这里使用无符号类型：`age TINYINT UNSIGNED`
- 精度：整段数字的长度列如：`123.45`则长度为：5
- 标度：小数后面的长度列如：`123.45`则长度为：2
- 分数字段的类型定义，由于分数最多出现1位小数，整体最长为4位数：`score DOUBLE(4,1)`

|类型|大小（字节）|有符号（SIGNED）范围|无符号（UNSIGNED）范围|描述|
|---|---|---|---|---|
|`TINYINT`|1 byte|(-128,127)|(0,255)|小整数值|
|`SMALLINT`|2 bytes|(-32768,32767)|(0,65535)|大整数值|
|`MEDIUMINT`|3 bytes|(-8388608,8388607)|(0,16777215)|大整数值|
|`INT`或`INTEGER`|4 bytes|(-2 147 483 648，2 147 483 647)|(0，4 294 967 295)|大整数值|
|`BIGINT`|8 bytes|(-2 147 483 648，2 147 483 647)|(0，4 294 967 295)|极大整数值|
|`FLOAT`|4 bytes|(-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38)|0，(1.175 494 351 E-38，3.402 823 466 E+38)|单精度浮点数值|
|`DOUBLE`|8 bytes|(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)|0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)|双精度浮点数值|
|`DECIMAL`|对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2|依赖于M和D的值|依赖于M和D的值|小数值（精确定点数）M：精度。D：标度|

### 字符串类型

> 带`TEXT`的就是存文本的，带`MBLOB`是存二进制数据的
> 二进制数据比如：音频，视频等这些数据。**一般不用这种方式存储，性能不佳，不方便管理**

`CHAR(10)`：表示只能存储10个字符的长度，超过则报错，定长字符如果存储不到10个字符则系统默认在后面补上空格填补后面的字符
`VARCHAR(10)`：表示只能存储10个字符的长度，不同于定长的是你存储2个字符它只占用2个字符的长度

`CHAR`:性能相对与`VARCHAR`较好。**因为：**`VARCHAR`还需要去计算你存储的字符的长度。

**列子：**

- 用户名：`username VARCHAR(50)`，因为用户可能输入的是1位或者是10多位。所以使用变长字符类型
- 性别：`gender CHAR(1)`，由于性别只有一位数，不是男就是女。这里使用定长字符类型

|类型|大小（字节）|描述|
|---|---|---|
|`CHAR`|0-255 bytes|定长字符串|
|`VARCHAR`|0-65535 bytes|变长字符串|
|`TINYBLOB`|0-255 bytes|不超过 255 个字符的二进制字符串|
|`TINYTEXT`|0-255 bytes|短文本字符串|
|`BLOB`|0-65 535 bytes|二进制形式的长文本数据|
|`TEXT`|0-65 535 bytes|长文本数据|
|`MEDIUMBLOB`|0-16 777 215 bytes|二进制形式的中等长度文本数据|
|`MEDIUMTEXT`|0-16 777 215 bytes|中等长度文本数据|
|`LONGBLOB`|0-4 294 967 295 bytes|二进制形式的极大文本数据|
|`LONGTEXT`|0-4 294 967 295 bytes|极大文本数据|

### 日期和时间类型

**例子：**

记录生日：`birthday DATE`，因为记录生日只需要年月日即可

|类型|大小(bytes)|范围|格式|描述|
|---|---|---|---|---|
|`DATE`|3|1000-01-01/9999-12-31|YYYY-MM-DD|日期值|
|`TIME`|3|'-838:59:59'/'838:59:59'|HH:MM:SS|时间值或持续时间|
|`YEAR`|1|1901/2155|YYYY|年份值|
|`DATETIME`|8|'1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'|YYYY-MM-DD hh:mm:ss|混合日期和时间值|
|`TIMESTAMP`|4|'1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 2147483647 秒，北京时间 2038-1-19 11:14:07，格林尼治时间 2038年1月19日 凌晨 03:14:07|YYYY-MM-DD hh:mm:ss|混合日期和时间值，时间戳|

## DDL操作表-删除&修改&添加

- 新增： `ALTER TABLE 表名 add 字段名 类型 comment '描述';`
- 修改数据类型：`ALTER TABLE 表名 MODIFY 字段名 新类型;`
- 修改字段名和字段类型：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) comment '描述' [约束];`

```shell
# 新增
mysql> alter table staff add nickname varchar(20) comment '昵称';
# 修改
mysql> alter table staff modify age tinyint unsigned;
# 修改字段名和字段类型：
mysql> alter table staff change nickname realname char(4) comment '真实名字';
```
