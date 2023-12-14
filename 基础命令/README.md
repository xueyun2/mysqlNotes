# SQL基础命令

## SQL分类

- `DDL`：数据定义语言，用来定义数据库对象（数据库，表，字段）
- `DML`：数据操作语言，用来对数据库表中的数据进行增删改查。
- `DQL`：数据查询语言，用来查询数据库中表的记录。
- `DCL`：数据控制语言，用来创建数据库用户，控制数据库的访问权限。

## DDL数据库操作

> 在命令后面要加上`;`

**查询：**

```shell
SHOW DATABASES; #查询所有数据库
SELECT DATABASE(); #查询当前数据库
```

**创建：**

`CREATE DATABASE`创建数据库 `IF NOT EXISTS`：创建不存在的数据库，`DEFAULT CHARSET`：设置字符集一般设置：`utf8mb4`，`COLLATE`：排序规则

```shell
CREATE DATABASE myDataBase; #创建myDataBase数据库
```

**删除：**

`DROP DATABASE`删除指定数据库`IF EXISTS`：判断是否存在

```shell
DROP DATABASE IF EXISTS mydatabase; #myDataBase
```

**使用：**

`USE`：数据库名

```shell
USE mydatabase; #使用mydatabase数据库
```

## DDL操作表-查询&创建

- `SHOW TABLES`：查询使用的数据库所有表
- `DESC 表名;` ：查询表结构
- `SHOW CREATE TABLE 表名;` ：查询指定表的建表语句
- `CREATE TABLE 表名（字段1 字段1类型[COMMENT 字段注释]）[COMMENT 表注释];`：创建表，**注意：最后一个字段没有逗号**

```shell
DESC tb_user; #查询表结构
SHOW TABLES; #查询使用的数据库所有表
SHOW CREATE TABLE tb_user #查询指定表的建表语句
```

**创建表：**

```shell
mysql> CREATE TABLE TB_USER(
    -> id int COMMENT '编号',
    -> name varchar(50) COMMENT '姓名',
    -> age int COMMENT '年龄',
    -> gender varchar(50) COMMENT '性别'
    -> ) COMMENT '用户表';
```

## DDL操作表-删除&修改&添加

- 新增： `ALTER TABLE 表名 ADD 字段名 类型 COMMENT '描述';`
- 修改数据类型：`ALTER TABLE 表名 MODIFY 字段名 新类型;`
- 修改字段名和字段类型：`ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) COMMENT '描述' [约束];`

```shell
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

**新增：**

> **注意：** 新增数据时，指定的字段顺序需要与值的顺序时一一对应的。字符串和日期类型数据应该包含再引号中，新增的数据大小，应该再字段的规定范围内。

```shell
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

**修改：**

> **注意：** 修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据

```shell
# 修改staff表中nickname和age,WHERE id=1:表示修改ID=1的这一条数据。
UPDATE staff SET nickname='张三',age=24 WHERE id=1;
```

**删除：**

> **注意：** 如果没有添加条件则删除整张表的数据，DELETE无法删除单个字段值如果要删除单个字段值可以使用UPDATE把这个字段值设置为null。

```shell
# 删除staff表中ID=1的这条数据
DELETE FROM staff WHERE id=1;
```

## DQL基础查询
