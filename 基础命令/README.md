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
  - [函数](#函数)
    - [字符串函数](#字符串函数)
    - [数值函数](#数值函数)
    - [日期函数](#日期函数)
    - [流程函数](#流程函数)
  - [约束](#约束)
  - [外键约束](#外键约束)
    - [外键约束-删除更新操作](#外键约束-删除更新操作)
  - [多表查询](#多表查询)
    - [内连接](#内连接)
    - [外连接](#外连接)
    - [自连接](#自连接)
    - [联合查询（UNION UNION ALL）](#联合查询union-union-all)
    - [子查询](#子查询)
      - [标量子查询](#标量子查询)
      - [列子查询](#列子查询)
      - [行子查询](#行子查询)
      - [表子查询](#表子查询)
      - [相关子查询](#相关子查询)
      - [EXISTS 子查询](#exists-子查询)
  - [事务](#事务)
    - [事务的ACID属性](#事务的acid属性)
    - [使用事务的步骤](#使用事务的步骤)
    - [并发事务问题](#并发事务问题)
    - [事务隔离级别](#事务隔离级别)

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
# 判断字段名是否存在，存在则删除该字段
ALTER TABLE 表名 DROP 字段名 IF EXISTS 字段名;;
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

## DCL-用户，访问，权限

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

ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';

# 删除用户

DROP USER '用户名'@'主机名';
```

**权限控制：**

常用权限表，其他权限可参考官方文档

| 权限              | 说明               |
| ----------------- | ------------------ |
| ALL,ALLPRIVILEGES | 所有权限           |
| SELECT            | 查询数据           |
| INSERT            | 插入数据           |
| UPDATE            | 修改数据           |
| DELETE            | 删除数据           |
| ALTER             | 修改表             |
| DROP              | 删除数据库/表/视图 |
| CREATE            | 创建数据库/表      |

> 多个权限之间，使用逗号分隔

```sql
# 1.查询权限
SHOW GRANTS FOR '用户名'@'主机名';
# 2.授予权限,如果授予所有数据库和表可以设置*号
GRANT 权限列表 ON 数据库名,表名 TO '用户名'@'主机名';
# 3.撤销权限
REVOKE 权限列表 ON 数据库名,表名 FROM '用户名'@'主机名';
```

## 函数

- 字符串函数
- 数值函数
- 日期函数
- 流程函数

### 字符串函数

- `CONCAT(s1,s2..sn)`：将多个字符串拼接成一个字符串
- `LOWER(str)`：将字符串全部转换成小写
- `UPPER(str)`：将字符串全部转换成大写
- `LENGTH(str)`：获取字符串长度
- `LPAD(str,n,pad)`：用pad在str左边填充，到n个字符长度
- `RPAD(str,n,pad)`：用pad在str右边填充，到n个字符长度
- `TRIM(str)`：去掉字符串左右两边的空格
- `SUBTRING(str,start,len)`：从str字符串中截取从start位置开始，len个长度的字符串

```sql
# 字符串拼接
SELECT CONCAT('hello','mysql');
# hellomysql
# 字符串转小写
SELECT LOWER('HELLO');
# hello
# 字符串转大写
SELECT UPPER('hello');
# HELLO
# 获取字符串长度
SELECT LENGTH('hello');
# 5
# 左填充
SELECT LPAD('hello',10,'*');
# *****hello
# 右填充
SELECT RPAD('hello',10,'*');
# hello*****
# 去掉左右空格
SELECT TRIM('  he llo  ');
# he llo
# 截取字符串
SELECT SUBSTRING('hello',2,3);
# ell

```

修改`article`表中的`serial`字段，将其填充到3位，不足3位的在左边填充0

```sql
update article set serial=lpad(serial,3,'0');
```

### 数值函数

- `CEIL(z)`：向上取整
- `FLOOR(z)`：向下取整
- `MOD(x/y)`：返回x/y的模
- `RAND()`：返回0-1内的随机数
- `ROUND(x,y)`：求参数x的四舍五入的值，保留y位小数

```sql
# 向上取整
SELECT CEIL(1.1);
# 2
# 向下取整
SELECT FLOOR(1.9);
# 1
# 求模（求余数）
SELECT MOD(10,3);
# 1
# 求随机数
SELECT RAND();
# 0.123456789
# 求四舍五入，保留2位小数
SELECT ROUND(1.12345,2);
# 生成一个六位数的验证码
# 1.生成一个0-1之间的随机数
# 2.乘以1000000，生成一个0-1000000之间的随机数
# 3.四舍五入，保留0位小数
# 4.不足6位数，向左填充0保证是6位数
SELECT LPAD(ROUND(RAND()*1000000,0),6,'0');
# 299140
```

### 日期函数

- `CURDATE();` 获取当前日期
- `CURTIME();` 获取当前时间
- `NOW();` 获取当前日期和时间
- `YEAR(date);` 获取date年
- `MONTH(date);` 获取date月
- `DAY(date);` 获取date日
- `DATE_ADD(date,INTERVAL expr type);` 给当前日期添加一个间隔，expr表示时间间隔多少，type表示时间间隔类型，如：
  `INTERVAL 70 DAY` 表示70天,`INTERVAL`为固定值。

- `DATEDIFF(date1,date2);` 返回起始时间date1和结束时间date2之间的天数

```sql
# 查询article表中的所有文章，按照文章发表日期于当前日期间隔进行排序，并显示文章标题和发表天数
# 由于使用到`datediff(curdate(),created_at)`太长给该函数设置一个别名方便后续排序使用。
# 正序
select title,datediff(curdate(),created_at) as 'entrydays' from article order by entrydays;
# 倒叙
select title,datediff(curdate(),created_at) as 'entrydays' from article order by entrydays desc;
```

### 流程函数

- `IF(value,t,f)`：如果value为true，则返回t，否则返回f
- `IFNULL(value1,value2)`：如果value1不为空，则返回value1，否则返回value2
- `CASE WHEN [val1] THEN [res1]...ELSE [default] END`：如果val1为true，则返回res1，否则返回default
- `CASE [expr] WHEN [val1] THEN [res1]...ELSE [default] END`：如果expr等于val1，则返回res1，否则返回default

```sql
# 如果workaddress为北京,上海则返回一线城市，否则返回二线城市
select
name
( case workaddress when'北京'then'一线城市'when'上海'then'一线城市'else'二线城市'end )as'工作地址'
from emp;
```

## 约束

- `NOT NULL`：限制该字段不能为null
- `UNIQUE`：限制该字段不能重复
- `PRIMARY KEY`：主键，限制该字段不能重复，并且不能为空，一个表只能有一个主键
- `DEFAULT`：保存数据时，如果没有指定该字段值，则采用默认值
- `FOREIGN KEY`：用来让两张表的数据之间建立连接，保证数据的一致性和完整性。
- `CHECK`：保证字段值满足某一个条件。**注意：在8.0.16之后支持**

```sql
create table user(
  # 自增主键，自动增长
  id int primary key auto_increment comment '主键',
  # 该字段不能空，并且不能重复，并且长度不能超过10个字符
  name varchar(10) not null unique comment '姓名',
  # 该字段值必须大于0并且小于等于120
  age int check(age>0&&age<=120) comment '年龄',
  # 如果该字段没有值则使用默认值1
  status char(1) default '1' comment '状态',
  gender char(1) comment '性别'
  ) comment '用户表';
```

## 外键约束

```sql
CREATE TABLE 表名(
  字段名 类型,
  ...
  CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名)
)
# 给现有的表添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
# 删除外键约束
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

例子：

```shell
# dept表
+----+--------+
| id | name   |
+----+--------+
|  1 | 研发部 |
|  2 | 市场部 |
|  3 | 财务部 |
|  4 | 销售部 |
|  5 | 总经办 |
+----+--------+
# emp表
+----+--------+------+----------+--------+------------+-----------+---------+
| id | name   | age  | job      | salary | entrydate  | managerid | dept_id |
+----+--------+------+----------+--------+------------+-----------+---------+
|  1 | 王锤   |   23 | 后端     |  20000 | 2024-10-31 |         6 |       1 |
|  2 | 张大侠 |   14 | 销售     |   3000 | 2024-10-31 |         7 |       4 |
|  3 | 张三   |   29 | 市场调研 |  10000 | 2024-10-31 |         9 |       3 |
|  4 | 李小龙 |   24 | 前端     |  20000 | 2024-10-31 |         8 |       1 |
+----+--------+------+----------+--------+------------+-----------+---------+
```

给以上emp表添加外键约束，dept_id字段关联dept表的id字段。

```sql
# 给emp表的dept_id字段添加外键约束
ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id);
# 删除外键约束
ALTER TABLE emp DROP FOREIGN KEY fk_emp_dept_id;
```

### 外键约束-删除更新操作

| 行为                    | 说明                                                                                                       |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| `NO ACTION`、`RESTRICT` | 当在父表中删除/更新对应记录时，首先检查该记录是否又对应的外键，如果有则不允许删除/更新。（与RESTRICT一致） |
| `CASCADE`               | 当父表中的记录被删除或更新时，子表中的对应记录也会自动删除或更新。                                         |
| `SET NULL`              | 当父表中的记录被删除或更新时，子表中的对应记录的外键字段会自动设置为NULL。                                 |
| `SET DEFAULT`           | 父表有变更时，子表将外键列设置成一个默认的值（innodb不支持）                                               |

```sql
# 给emp表的dept_id字段添加外键约束，当删除/更新dept表中的记录时，emp表中的对应记录也会自动删除或更新。 ON DELETE 删除后执行的操作 ON UPDATE 更新后执行操作

ALTER TABLE emp ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id) ON UPDATE CASCADE ON DELETE CASCADE;
```

## 多表查询

查询多张表时以逗号分隔

```sql
# 多表查询，筛选emp表中dept_id等于dept表中的id的数据;
SELECT * FROM emp,dept WHERE emp.dept_id=dept.id;

# 查询emp和dept表中的name字段,emp.dept_id = dept.id的数据。
SELECT emp.name,dept.name FROM emp,dept WHERE emp.dept_id = dept.id;

# 给表添加别名,在表名后面空格加上别名，在查询时使用别名。
SELECT a.name,b.name FROM emp a,dept b WHERE a.dept_id = b.id;

```

### 内连接

查询A,B交集的部分数据。可以省略`INNER`关键字。

```sql
# 语法
SELECT 列名
FROM 表1
INNER JOIN 表2 ON 表1.列 = 表2.列;
```

显示内连接：

```sql
# 内连接查询emp和dept表中的name字段,emp.dept_id = dept.id的数据。
SELECT a.name,b.name FROM emp a INNER JOIN dept b ON a.dept_id = b.id;
```

### 外连接

左外连接：查询左表所有数据，包含右表交集的部分。
右外连接：查询右表所有数据，包含左表交集的部分。

```sql
# 左连接
SELECT 列名
FROM 表1
LEFT JOIN 表2 ON 表1.列 = 表2.列;
# 右连接
SELECT 列名
FROM 表1
RIGHT JOIN 表2 ON 表1.列 = 表2.列;
```

查询emp表中所有数据，包含dept表交集的部分数据。

```sql
select emp.name,dept.name from emp left join dept on emp.dept_id = dept.id
```

### 自连接

当前表与自身连接查询，**自连接必须使用别名**。

```sql
# 语法
SELECT 列名
FROM 表 别名1
JOIN 表 别名2 ON 别名1.列 = 别名2.列;
```

查询emp表中所有员工的姓名和上级领导的姓名。

```sql
SELECT a.name,b.name FROM emp a,emp b WHERE a.managerid = b.id;
SELECT a.name,b.name FROM emp a JOIN emp b ON a.managerid = b.id;
```

### 联合查询（UNION UNION ALL）

- UNION ：去除重复数据
- UNION ALL ：包含重复数据

> 查询多张表时的列数必须保持一致，字段类型也要保持一致

```sql
SELECT 列名
FROM 表1
LEFT JOIN 表2 ON 表1.列 = 表2.列
UNION
SELECT 列名
FROM 表1
RIGHT JOIN 表2 ON 表1.列 = 表2.列;
```

查询emp表中年龄小于20岁的员工和薪资小于4000的员工。`UNION` 去除重复数据

```sql
SELECT * FROM emp WHERE emp.age<20
UNION 
SELECT * FROM emp WHERE  emp.salary<4000;
```

### 子查询

#### 标量子查询

标量子查询返回单个值（一个列值），通常用于 `WHERE` 子句或 `SELECT` 子句中。

```sql
SELECT 列名
FROM 表
WHERE 列名 = (SELECT 单一列 FROM 表 WHERE 条件);
```

**示例：**

查询article表中的作为王锤的文章标题和作者名。
`(SELECT name FROM emp WHERE emp.name='王锤')`该条语句子返回一个用户名。

```sql
SELECT title 文章标题,author 作者名 FROM article WHERE article.author = (SELECT name FROM emp WHERE emp.name='王锤')
```

#### 列子查询

列子查询返回一个列的值，通常用于 `IN`、`ALL` 或 `ANY` 操作符。

- `in` ：列表中的任意一个值
- `all` ：列表中的所有值
- `any|some` ：列表中的任意一个值

```sql
SELECT 列名
FROM 表
WHERE 列名 IN (SELECT 单列 FROM 表 WHERE 条件);
```

查询emp表中部门为研发部或市场部的员工姓名和职位，子查询返回研发部，市场部的ID，和emp表中的dept_id进行比较。

```sql

SELECT name,job FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE dept.name='研发部' OR dept.name='市场部');
```

#### 行子查询

行子查询返回多列的数据，用于匹配多列条件，通常用于 `WHERE` 子句中，并结合 `IN` 操作符。

```sql
SELECT 列名
FROM 表
WHERE (列1, 列2) IN (SELECT 列1, 列2 FROM 表 WHERE 条件);
```

查询与王锤的薪资和经理ID相同的员工信息。

```sql
SELECT * FROM emp WHERE (salary, managerid) = (SELECT salary, managerid FROM emp WHERE name = '王锤');
```

#### 表子查询

表子查询返回一个结果集（多行多列），通常在 `FROM` 子句中使用，将子查询的结果视为临时表。

```sql
SELECT 列名
FROM (SELECT 列名 FROM 表 WHERE 条件) AS 子查询表别名;
```

查询薪资大于2000的员工姓名，薪资和经理ID，然后再查询经理ID为1的员工信息。

```sql
SELECT name,salary,managerid FROM (SELECT name,salary,managerid FROM emp WHERE salary > 2000) AS a WHERE a.managerid = 1;
```

#### 相关子查询

相关子查询依赖于外部查询的值，每次评估子查询时都会引用外部查询中的数据。

```sql
SELECT 列名
FROM 表 别名
WHERE 列名 操作符 (SELECT 列名 FROM 表 WHERE 外部表.列 = 子查询表.列);
```

查询返回高于其部门平均工资的员工。子查询依赖于 e1 表的 managerid，因此每个外部行执行时，子查询也会相应更新

```sql
SELECT name, salary
FROM emp e1
WHERE salary > (SELECT AVG(salary) FROM emp e2 WHERE e1.managerid = e2.managerid);
```

#### EXISTS 子查询

EXISTS 子查询用于检查子查询是否返回至少一行数据。通常用于检查数据是否存在。

```sql
SELECT 列名
FROM 表
WHERE EXISTS (SELECT 1 FROM 表 WHERE 条件);
```

此查询返回至少包含一名员工的部门。子查询 `SELECT 1 FROM emp WHERE dept_id = d.id` 检查是否存在至少一名员工的部门。

```sql
SELECT name
FROM dept d
WHERE EXISTS (SELECT 1 FROM emp e WHERE e.dept_id = d.id);
```

## 事务

### 事务的ACID属性

- **原子性（Atomicity）**：事务中的所有操作要么全部执行成功，要么全部不执行。
- **一致性（Consistency）**：事务前后，数据库的状态应该保持一致，不会破坏数据的完整性。
- **隔离性（Isolation）**：多个事务之间互不干扰，每个事务独立执行，修改数据不会被其他事务看到，直到该事务提交。
- **持久性（Durability）**：一旦事务提交，数据的修改会永久保存，即使系统崩溃也不会丢失。

### 使用事务的步骤

在 MySQL 中，事务通常使用以下语句来控制：

- `START TRANSACTION` 或 `BEGIN`：开始一个新的事务。
- `COMMIT`：提交事务，保存所有修改。
- `ROLLBACK`：回滚事务，撤销自 START TRANSACTION 以来的所有修改。
- `SAVEPOINT`：创建一个保存点，可以部分回滚到此点。
- `RELEASE SAVEPOINT`：删除一个保存点。
- `ROLLBACK TO SAVEPOINT`：回滚到指定的保存点。

```sql
# 开始事务
START TRANSACTION;
# 从ID为1的账户中减去100元
UPDATE `user` SET `Balance` = Balance-100 WHERE id = 1;
# 向ID为2的账户中加上100元
UPDATE `user` SET `Balance` = Balance+100 WHERE id = 2;
# 如果所有语句成功，提交事务
COMMIT;
```

查看当前事务是否为自动提交状态：
> 修改为手动提交时在执行完SQL语句后需要执行commit提交。

```sql
# 1 表示自动提交，0 表示手动提交。
SELECT @@autocommit;
# 设置为手动提交状态。
SET @@autocommit = 0;
```

### 并发事务问题

- **脏读：** 一个事务读取到另一个事务未提交的数据。
- **不可重复读：** 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读。
- **幻读：** 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在。

### 事务隔离级别

> 解决事务并发问题。

| 隔离级别                | 脏读 | 不可重复读 | 幻读 |
| ----------------------- | ---- | ---------- | ---- |
| `Read uncommitted`      | √    | √          | √    |
| `Read committed`        | ×    | √          | √    |
| `Repeatable Read`(默认) | ×    | ×          | √    |
| `Serializable`          | ×    | ×          | ×    |

- 查看事务的隔离级别：`SELECT @@TRANSACTION_ISOLATION`;
- 设置事务的隔离级别：`SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL [READ UNCOMMITTED|READ COMMITTED|REPEATABLE READ|SERIALIZABLE]`;

> [SESSION|GLOBAL] 表示作用范围，SESSION 表示当前会话，GLOBAL 表示全局。

```sql
# 查看当前会话的隔离级别。
SELECT @@TRANSACTION_ISOLATION;
# 设置当前会话的隔离级别`READ UNCOMMITTED`。
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```
