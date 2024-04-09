# SQL基础命令

- [SQL基础命令](#sql基础命令)
  - [SQL分类](#sql分类)
  - [DDL操作（数据库，表，字段）](#ddl操作数据库表字段)
    - [数据库操作](#数据库操作)
      - [查询数据库](#查询数据库)
      - [创建数据库](#创建数据库)
      - [删除数据库](#删除数据库)
      - [使用数据库](#使用数据库)
    - [操作表](#操作表)
      - [查询表](#查询表)
      - [创建表](#创建表)
    - [字段操作](#字段操作)
      - [删除\&修改\&新增](#删除修改新增)
  - [DML-数据操作](#dml-数据操作)
    - [新增](#新增)
    - [修改](#修改)
    - [删除](#删除)
  - [DQL基础查询](#dql基础查询)
    - [基础查询](#基础查询)
    - [条件查询(WHERE)](#条件查询where)
    - [聚合函数](#聚合函数)
    - [分组查询(GROUP BY)](#分组查询group-by)
    - [排序查询(ORDER BY)](#排序查询order-by)
    - [分页查询(LIMIT)](#分页查询limit)
    - [DQL执行顺序](#dql执行顺序)
    - [DCL-用户，访问，权限](#dcl-用户访问权限)

## SQL分类

- `DDL`：数据定义语言，用来定义数据库对象（数据库，表，字段）
- `DML`：数据操作语言，用来对数据库表中的数据进行增删改查。
- `DQL`：数据查询语言，用来查询数据库中表的记录。
- `DCL`：数据控制语言，用来创建数据库用户，控制数据库的访问权限。

## DDL操作（数据库，表，字段）

> 在部分命令后面要加上`;`

### 数据库操作

#### 查询数据库

**语法：**

```SQL
SHOW DATABASES; #查询所有数据库
SELECT DATABASE(); #查询当前数据库
```

#### 创建数据库

**语法：**

```SQL
CREATE DATABASE 数据库名称 IF NOT EXISTS 条件 DEFAULT CHARSET 设置字符集一般设置utf8mb4,COLLATE 排序规则
```

```SQL
CREATE DATABASE myDataBase; #创建myDataBase数据库
```

#### 删除数据库

**语法：**

```SQL
DROP DATABASE 数据库名称 IF EXISTS 条件判断是否存在
```

```SQL
DROP DATABASE IF EXISTS mydatabase; #myDataBase
```

#### 使用数据库

**语法：**

```SQL
USE mydatabase; #使用mydatabase数据库
```

### 操作表

- 查询使用的数据库所有表：`SHOW TABLES`
- 查询表结构：`DESC 表名;`
- 查询指定表的建表语句：`SHOW CREATE TABLE 表名;`
- 创建表：`CREATE TABLE 表名（字段1 字段1类型[COMMENT 字段注释]）[COMMENT 表注释];`

#### 查询表

```SQL
DESC tb_user; #查询表结构
SHOW TABLES; #查询使用的数据库所有表
SHOW CREATE TABLE tb_user #查询指定表的建表语句
```

#### 创建表

> **注意：** 最后一个字段没有逗号

```SQL
CREATE TABLE TB_USER(
 id int COMMENT '编号',
 name varchar(50) COMMENT '姓名',
 age int COMMENT '年龄',
 gender varchar(50) COMMENT '性别'
 ) COMMENT '用户表';
```

### 字段操作

#### 删除&修改&新增

**语法：**

```SQL
#新增：
ALTER TABLE 表名 ADD 字段名 类型 COMMENT '描述';
#修改数据类型
ALTER TABLE 表名 MODIFY 字段名 新类型;
#修改字段名和字段类型
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) COMMENT '描述' 约束;
```

```SQL
# 新增nickname字段，类型VARCHAR(20)，描述：昵称
ALTER TABLE staff ADD nickname VARCHAR(20) COMMENT '昵称';
# 修改表中的age类型为TINYINT UNSIGNED
ALTER TABLE staff MODIFY age TINYINT UNSIGNED;
# 修改字段名nickname为realname，类型：char(4)，描述：'真实名字'
ALTER TABLE staff CHANGE nickname realname char(4) COMMENT '真实名字';
```

## DML-数据操作

- 新增：`INSERT INTO  表名(字段1,字段2) VALUES(值1,值2,...);`
- 修改：`UPDATE 表名 SET 字段1=值1，字段2=值2...[ WHERE 条件 ];`
- 删除：`DELETE FROM 表名 [WHERE 条件];`

### 新增

> **注意：** 新增数据时，指定的字段顺序需要与值的顺序时一一对应的。字符串和日期类型数据应该包含再引号中，新增的数据大小，应该再字段的规定范围内。

**语法：**

```SQL
#1.给表中所有字段添加值
INSERT INTO staff VALUES(value1,value2,value3,...);
#2.给指定字段添加值，即：column1字段对应value1值。
INSERT INTO table_name (column1,column2,column3,...) 
VALUES(value1,value2,value3,...);
#3.批量添加数据
INSERT INTO staff VALUES(value1,value2,value3,...),(value1,value2,value3,...),(value1,value2,value3,...);
#3.1批量添加对应字段数据
INSERT INTO table_name (column1,column2,column3,...) 
VALUES(value1,value2,value3,...),(value1,value2,value3,...),(value1,value2,value3,...);
```

### 修改

**语法：**

```SQL
UPDATE 表名 SET 字段名=值,字段名=值 WHERE 条件;
```

> **注意：** 修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据

```SQL
# 修改staff表中nickname和age,WHERE id=1:表示修改ID=1的这一条数据。
UPDATE staff SET nickname='张三',age=24 WHERE id=1;
```

### 删除

**语法：**

```SQL
DELETE FROM 表名 WHERE 条件;
```

> **注意：** 如果没有添加条件则删除整张表的数据，DELETE无法删除单个字段值如果要删除单个字段值可以使用UPDATE把这个字段值设置为null。

```SQL
# 删除staff表中ID=1的这条数据
DELETE FROM staff WHERE id=1;
```

## DQL基础查询

- 查询关键字：`SELECT`

### 基础查询

**语法：**

```SQL
SELECT 字段列表
FROM 表名列表
WHERE 条件列表
GROUP BY 分组字段列表
HAVING 分组后条件列表
ORDER BY 排序字段列表
LIMIT 分页参数
```

```SQL
SELECT 字段1,字段2,字段3... FROM 表名;
SELECT * FROM 表名;
SELECT 字段1 AS 别名1,字段2 AS 别名2,字段3 AS 别名3,... FROM 表名;
```

> 设置别名为可选参数，在设置别名时可以省略`AS`

```SQL
# 查询多个字段
SELECT nickname FROM staff;
# 查询所有
SELECT * FROM staff;
# 查询去除重复信息DISTINCT
SELECT DISTINCT city FROM staff;
# 设置别名
SELECT nickname AS 昵称 FROM staff;
# 省略AS查询
SELECT nickname 昵称 FROM staff;
```

### 条件查询(WHERE)

**语法：**

```SQL
SELECT 字段列表 FROM 表名 WHERE 条件;
```

```SQL
# 查询没有年龄的数据
SELECT * FROM staff WHERE age IS NULL;
# 查询有年龄的数据
SELECT * FROM staff WHERE age IS NOT NULL;
# 查询年龄在15到20之间的信息BETWEEN前为最小值AND后为最大值。
SELECT * FROM staff WHERE age BETWEEN 15 AND 20;
# 查询年龄等于18或20或40
SELECT * FROM staff WHERE age in(18,20,40);
# 查询姓名两个字的信息，查询两个字符设置两个下划线
SELECT * FROM staff WHERE name LIKE '__';
# 查询身份证号最后一位是X的员工信息，%表示前面多少位数无关，但是最后一位必须是X
SELECT * FROM staff WHERE name LIKE '%X';
```

### 聚合函数

将一列数据最为一个整体，进行纵向计算。

**语法：**

```SQL
SELECT 聚合函数(字段列表) FROM 表名;
```

> 所有`NULL`值的字段是不参与聚合函数计算

- `count`：统计数量
- `max`：最大值
- `min`：最小值
- `avg`：平均值
- `sum`：求和

```SQL
# 统计整张表的数量
SELECT COUNT(*) FROM staff;
# 查询年龄最大值
SELECT MXA(age) FROM staff;
# 统计西安地区员工的年龄之和
SELECT SUM(age) FROM staff WHERE workaddress = '西安';
```

### 分组查询(GROUP BY)

**语法：**

```SQL
SELECT 字段列表 FROM 表名 WHERE 条件 GROUP BY 分组字段名 HAVING 分组后过滤条件;
```

**WHERE与HAVING区别：**

- 执行时机不同：`WHERE`是分组之前进行过滤，不满足`WHERE`条件，不参与分组;而`HAVING`是分组之后对结果进行过滤。
- 判断条件不同：`WHERE`不能对聚合函数进行判断，而`HAVING`可以。

```SQL
# 根据性别分组，统计男性员工和女性员工的数量，gender,count(*) 查询同时把性别字段显示
SELECT gender,count(*) FROM staff GROUP BY gender;
# 根据性别分组，统计男性员工和女性员工平均年龄
SELECT gender,AVG(age) FROM staff GROUP BY gender;
# 查询年龄小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址
SELECT workaddress,count(*) FROM staff where age < 45 GROUP BY workaddress HAVING count(*) >= 3;
```

### 排序查询(ORDER BY)

**语法：**

```SQL
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2 排序方式2;
```

- `ASC`：升序（默认值）
- `DESC`：降序

> **注意:** 如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序,

``` SQL
# 已年龄字段进行降序查询
SELECT * FROM staff ORDER BY age DESC;
# 按照年龄字段降序查询如果年龄相同则按照时间字段升序查询
SELECT * FROM staff ORDER BY age DESC,time ASC
```

### 分页查询(LIMIT)

**语法：**

```SQL
SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
```

- 起始索引从0开始，起始索引 = （查询页码-1）*每页显示记录数。
- 分页查询是数据库的方言，不同的数据库有不同的实现，`MySQL`中是`LIMIT`。
- 如果查询的是第一页数据，起始索引可以省略，直接简写为`LIMIT 10`。

```SQL
# 查询第一页的数据，每页展示10条
SELECT * FROM staff LIMIT 0,10;
# 简写：第一页可以省略不写
SELECT * FROM staff LIMIT 10;
# 查询第二页数据，每页展示10条
SELECT * FROM staff LIMIT 10,10;
```

### DQL执行顺序

```sql
1.FROM  #表列表
2.WHERE #条件
3.GROUP BY #分组字段
4.SELECT #字段列表
5.ORDER BY #排序字段列表
6.LIMIT #分页参数
```

### DCL-用户，访问，权限

用来管理数据库用户，控制数据库的访问 权限。

**管理用户：**

主机名：`localhost`表示只能在当前主机下访问，设置`%`号即可在任意主机下访问。

```sql

# 查询用户

USE mysql;
SELECT * FROM user;

# 创建用户

CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

# 修改用户
# mysql_native_password 是mysql中的一个加密方式

CREATE USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';

# 删除用户

DROP USER '用户名'@'主机名';
```

**权限控制：**

常用权限表，其他权限可参考官方文档

|权限|说明|
|---|---|
|ALL,ALLPRIVILEGES|所有权限|
|SELECT|查询数据|
|INSERT|插入数据|
|UPDATE|修改数据|
|DELETE|删除数据|
|ALTER|修改表|
|DROP|删除数据库/表/视图|
|CREATE|创建数据库/表|

> 多个权限之间，使用逗号分隔

```sql
# 1.查询权限
SHOW GRANTS FOR '用户名'@'主机名';
# 2.授予权限,如果授予所有数据库和表可以设置*号
GRANT 权限列表 ON 数据库名,表名 TO '用户名'@'主机名';
# 3.撤销权限
REVOKE 权限列表 ON 数据库名,表名 FROM '用户名'@'主机名';
```
