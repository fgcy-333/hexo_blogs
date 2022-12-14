---
title: 第13章_约束
date: 2022-08-21 19:09:29
tags:
- 尚硅谷-宋红康-MySQL
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

# 第13章_约束



## 1. 约束(constraint)概述

### 1.1 为什么需要约束

数据完整性（Data Integrity）是指数据的精确性（Accuracy）和可靠性（Reliability）。它是防止数据库中存在不符合语义规定的数据和防止因错误信息的输入输出造成无效操作或错误信息而提出的。

为了保证数据的完整性，SQL规范以约束的方式对**表数据进行额外的条件限制**。从以下四个方面考虑：

- `实体完整性（Entity Integrity）`：例如，同一个表中，不能存在两条完全相同无法区分的记录
- `域完整性（Domain Integrity）`：例如：年龄范围0-120，性别范围“男/女”
- `引用完整性（Referential Integrity）`：例如：员工所在部门，在部门表中要能找到这个部门
- `用户自定义完整性（User-defined Integrity）`：例如：用户名唯一、密码不能为空等，本部门经理的工资不得高于本部门职工的平均工资的5倍。

<!--more-->

即：

为了保证数据的完整性

主要分为：实体完整性、参照完整性、用户自定义完整性



### 1.2 什么是约束

对表中字段的限制

约束是表级的强制规定

可以在**创建表时规定约束（通过 CREATE TABLE 语句）**，或者在**表创建之后通过 ALTER TABLE 语句规定约束**。





### 1.3 约束的分类

- **根据约束数据列的限制，**约束可分为：

  - **单列约束**：每个约束只约束一列
  - **多列约束**：每个约束可约束多列数据

  

- **根据约束的作用范围**，约束可分为：

  - **列级约束**：只能作用在一个列上，跟在列的定义后面 （约束声明在对应字段后边）
  - **表级约束**：可以作用在多个列上，不与列一起，而是单独定义 （在所有字段的后边声明的约束）



```
			位置			支持的约束类型					是否可以起约束名
列级约束：	列的后面		语法都支持，但外键没有效果		不可以
表级约束：	所有列的下面	   默认和非空不支持，其他支持	   可以（主键没有效果）
```





- **根据约束起的作用**，约束可分为：

  - **NOT NULL** **非空约束，规定某个字段不能为空**
  - **UNIQUE**  **唯一约束**，**规定某个字段在整个表中是唯一的**
  - **PRIMARY KEY  主键(非空且唯一)约束**
  - **FOREIGN KEY**  **外键约束**
  - **CHECK**  **检查约束**
  - **DEFAULT**  **默认值约束**

  

> 注意： MySQL不支持check约束，但可以使用check约束，而没有任何效果



### 1.4如何添加约束



CREATE TABLE 时 添加约束

ALTER TABLE 时 添加约束、删除约束





### 1.5 查看约束



- 查看某个表已有的约束

```mysql
#information_schema数据库名（系统库）
#table_constraints表名称（专门存储各个表的约束）
SELECT * FROM information_schema.table_constraints 
WHERE table_name = '表名称';
```



~~~mysql
SELECT * FROM information_schema.table_constraints 
WHERE table_name = 'employees';

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | atguigudb         | emp_email_uk    | atguigudb    | employees  | UNIQUE          | YES      |
| def                | atguigudb         | emp_emp_id_pk   | atguigudb    | employees  | UNIQUE          | YES      |
| def                | atguigudb         | PRIMARY         | atguigudb    | employees  | PRIMARY KEY     | YES      |
| def                | atguigudb         | emp_dept_fk     | atguigudb    | employees  | FOREIGN KEY     | YES      |
| def                | atguigudb         | emp_job_fk      | atguigudb    | employees  | FOREIGN KEY     | YES      |
| def                | atguigudb         | emp_manager_fk  | atguigudb    | employees  | FOREIGN KEY     | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
6 rows in set (0.01 sec)
~~~









###  2. 非空约束

### 2.1 作用

限定某个字段/某列的值不允许为空

---

![1555426972098](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/1555426972098.png)

---



### 2.2 关键字

NOT NULL



### 2.3 特点

- 默认，所有的类型的值都可以是NULL，包括INT、FLOAT等数据类型
- 非空约束只能出现在表对象的列上，只能某个列单独限定非空，**不能组合非空**
- 一个表可以有很多列都分别限定了非空
- 空字符串''不等于NULL，0也不等于NULL



### 2.4 添加非空约束

（1）建表时

```mysql
CREATE TABLE 表名称(
	字段名  数据类型,
    字段名  数据类型 NOT NULL,  
    字段名  数据类型 NOT NULL
);
```



举例：

```mysql
CREATE TABLE emp(
id INT(10) NOT NULL,
NAME VARCHAR(20) NOT NULL,
sex CHAR NULL
);
```







```mysql
-- 建库
CREATE DATABASE dbtest13;

Query OK, 1 row affected (0.00 sec)


-- 切换数据库
USE dbtest13;

Database changed



-- 建表
CREATE TABLE test1(
	id INT NOT NULL,
    last_name varchar(15) NOT NULL,
    email varchar(25),
    salary decimal(10,2)
);

Query OK, 0 rows affected (0.02 sec)





-- 查看建表语句

DESC test1;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   |     | NULL    |       |
| last_name | varchar(15)   | NO   |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.01 sec)




-- 赋值

INSERT INTO test1(id,last_name,email,salary)
VALUES(1,'Tom','tom@126.com',3400);

Query OK, 1 row affected (0.01 sec)



-- 非空约束1
INSERT INTO test1(id,last_name,email,salary)
VALUES(NULL,'Jerry','tom@126.com',3400);

ERROR 1048 (23000): Column 'id' cannot be null


-- 非空约束2
INSERT INTO test1(id,email,salary)
VALUES(3,'tom@126.com',3400);

ERROR 1364 (HY000): Field 'last_name' doesn't have a default value


-- 非空约束3

UPDATE test1 SET last_name =NULL
WHERE id=1;

ERROR 1048 (23000): Column 'last_name' cannot be null
```



（2）建表后

```mysql
alter table 表名称 modify 字段名 数据类型 not null;
```



举例：

```mysql
-- 查看建表语句

DESC test1;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   |     | NULL    |       |
| last_name | varchar(15)   | NO   |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)



ALTER TABLE test1
MODIFY email VARCHAR(30) NOT NULL;

Query OK, 0 rows affected (0.07 sec)
Records: 0  Duplicates: 0  Warnings: 0



SELECT * FROM test1;

+----+-----------+-------------+---------+
| id | last_name | email       | salary  |
+----+-----------+-------------+---------+
|  1 | Tom       | tom@126.com | 3400.00 |
+----+-----------+-------------+---------+
1 row in set (0.00 sec)
```

注意：

如果之前的记录中，emal有为空的值，添加非空约束会失败





### 2.5 删除非空约束

```mysql
alter table 表名称 modify 字段名 数据类型 NULL;#去掉not null，相当于修改某个非注解字段，该字段允许为空

或 

alter table 表名称 modify 字段名 数据类型;#去掉not null，相当于修改某个非注解字段，该字段允许为空
```

举例：

```mysql
ALTER TABLE test1
MODIFY email VARCHAR(25) NULL;


Query OK, 1 row affected (0.03 sec)
Records: 1  Duplicates: 0  Warnings: 0


-- 查看表结构
DESC test1;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   |     | NULL    |       |
| last_name | varchar(15)   | NO   |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)


```

```mysql
ALTER TABLE test1
MODIFY email VARCHAR(25) DEFAULT '' NULL;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 查看表结构
DESC test1;
+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   |     | NULL    |       |
| last_name | varchar(15)   | NO   |     | NULL    |       |
| email     | varchar(25)   | YES  |     |         |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```







## 3. 唯一性约束

### 3.1 作用

用来限制某个字段/某列的值不能重复。

---

 ![1555427198811](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/1555427198811.png)

----



### 3.2 关键字

UNIQUE



### 3.3 特点

- 同一个表可以有多个唯一约束。
- 唯一约束可以是某一个列的值唯一，也可以多个列组合的值唯一。
- **唯一性约束允许列值为空**
- 在创建唯一约束的时候，如果不给唯一约束命名，就默认和列名相同。
- **MySQL会给唯一约束的列上默认创建一个唯一索引**



### 3.4 添加唯一约束

（1）建表时

```mysql
-- 方式一：列级约束
create table 表名称(
	字段名  数据类型,
    字段名  数据类型  unique,  
    字段名  数据类型  unique key,
    字段名  数据类型
);



-- 方式二：表级约束
create table 表名称(
	字段名  数据类型,
    字段名  数据类型,  
    字段名  数据类型,
    [constraint 约束名] unique key(字段名)
);
```



举例：

```mysql
-- 列级约束
create table test2(
	id INT UNIQUE,
    last_name varchar(15),
    email varchar(25) UNIQUE,
    salary decimal(10,2),
    -- 表级约束
    CONSTRAINT uk_test3_email UNIQUE(email)
);

Query OK, 0 rows affected, 1 warning (0.02 sec)


/*
注意：
NOT NULL没有表级约束
*/



-- 查看表结构
DESC test2;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  | UNI | NULL    |       |
| last_name | varchar(15)   | YES  |     | NULL    |       |
| email     | varchar(25)   | YES  | UNI | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.01 sec)
```



~~~mysql
-- 查看某个表的约束信息
SELECT * FROM information_schema.table_constraints
WHERE table_name ='test2';

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | email           | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | id              | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | uk_test3_email  | dbtest13     | test2      | UNIQUE          | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
3 rows in set (0.00 sec)

-- 注意：列级约束默认使用 字段名作为约束名






-- 插入数据

INSERT INTO test2 (id)
VALUES(1);

Query OK, 1 row affected (0.00 sec)




-- 破环主键完整性
INSERT INTO test2 (id)
VALUES(1);

ERROR 1062 (23000): Duplicate entry '1' for key 'test2.id'


-- 唯一性约束可以插入空值
-- 可以插入多次
INSERT INTO test2 (id)
VALUES(NULL);

Query OK, 1 row affected (0.00 sec)



-- 查看表信息
SELECT * FROM test2;

+------+-----------+-------+--------+
| id   | last_name | email | salary |
+------+-----------+-------+--------+
|    1 | NULL      | NULL  |   NULL |
| NULL | NULL      | NULL  |   NULL |
| NULL | NULL      | NULL  |   NULL |
+------+-----------+-------+--------+
3 rows in set (0.00 sec)
~~~



（2）建表后指定唯一键约束

```mysql
#字段列表中如果是一个字段，表示该列的值唯一。如果是两个或更多个字段，那么复合唯一，即多个字段的组合是唯一的
#方式1：
alter table 表名称
add constraint 约束名 unique(字段列表); 
```





```mysql
#方式2：
alter table 表名称 
modify 字段名 字段类型 unique;
```





举例：

```mysql
-- 表级约束
ALTER TABLE test2 
ADD  CONSTRAINT uk_test2_email UNIQUE(email);

Query OK, 0 rows affected, 1 warning (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 1
```



```mysql
-- 列级约束
ALTER TABLE test2 
MODIFY last_name VARCHAR(20) UNIQUE;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC test2;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  | UNI | NULL    |       |
| last_name | varchar(20)   | YES  | UNI | NULL    |       |
| email     | varchar(25)   | YES  | UNI | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)


```



### 3.5 关于复合唯一约束

格式：

```mysql
create table 表名称(
	字段名  数据类型,
    字段名  数据类型,  
    字段名  数据类型,
    unique key(字段列表) #字段列表中写的是多个字段名，多个字段名用逗号分隔，表示那么是复合唯一，即多个字段的组合是唯一的
);
```



~~~mysql
-- 建表

CREATE TABLE USER(
	 id INT,
    `name` varchar(15),
    `password` varchar(25),
    
    -- 表级约束 复合唯一约束
    CONSTRAINT uk_username_pwd UNIQUE(`name`,`password`)
);

Query OK, 0 rows affected (0.02 sec)





-- 验证复合
INSERT INTO USER 
VALUES(1,'Tom1','abc');

Query OK, 1 row affected (0.00 sec)

INSERT INTO USER 
VALUES(1,'Tom2','abc');

Query OK, 1 row affected (0.00 sec)
~~~



```mysql
#学生表
create table student(
	sid int,	#学号
    sname varchar(20),			#姓名
    tel char(11) unique key,  #电话
    cardid char(18) unique key #身份证号
);

Query OK, 0 rows affected (0.02 sec)





#课程表
create table course(
	cid int,  #课程编号
    cname varchar(20)     #课程名称
);

Query OK, 0 rows affected (0.02 sec)




#选课表
create table student_course(
    id int,
	sid int,
    cid int,
    score int,
    unique key(sid,cid)  #复合唯一
);

Query OK, 0 rows affected (0.02 sec)
```





```mysql
insert into student 
values(1,'张三','13710011002','101223199012015623');#成功


insert into student 
values(2,'李四','13710011003','101223199012015624');#成功



insert into course 
values(1001,'Java'),(1002,'MySQL');#成功

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

```mysql
-- 学生表
select * from student;
+-----+-------+-------------+--------------------+
| sid | sname | tel         | cardid             |
+-----+-------+-------------+--------------------+
|   1 | 张三  | 13710011002 | 101223199012015623 |
|   2 | 李四  | 13710011003 | 101223199012015624 |
+-----+-------+-------------+--------------------+
2 rows in set (0.00 sec)




-- 课程表
select * from course;
+------+-------+
| cid  | cname |
+------+-------+
| 1001 | Java  |
| 1002 | MySQL |
+------+-------+
2 rows in set (0.00 sec)
```

```mysql
-- 选课表

insert into student_course values
(1, 1, 1001, 89),
(2, 1, 1002, 90),
(3, 2, 1001, 88),
(4, 2, 1002, 56);#成功

Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0



-- 复合唯一性约束重复
INSERT INTO student_course
VALUES(5,2,1002,100);


ERROR 1062 (23000): Duplicate entry '2-1002' for key 'student_course.sid'
```

```mysql
-- 选课表
select * from student_course;

+----+------+------+-------+
| id | sid  | cid  | score |
+----+------+------+-------+
|  1 |    1 | 1001 |    89 |
|  2 |    1 | 1002 |    90 |
|  3 |    2 | 1001 |    88 |
|  4 |    2 | 1002 |    56 |
+----+------+------+-------+
4 rows in set (0.00 sec)
```







### 3.5 删除唯一约束

- **添加唯一性约束的列**上也会**自动创建唯一索引**

- **删除唯一约束****只能通过**删除**唯一索引**的方式删除。

- 删除时需要指定唯一索引名，**唯一索引名就和唯一约束名一样**。

- 如果创建唯一约束时未指定名称，如果是**单列，就默认和列名相同**；

  如果是组合列，那么**默认和()中排在第一个的列名相同**。也可以自定义唯一性约束名



```mysql
SELECT * FROM information_schema.table_constraints
WHERE table_name = '表名'; #查看都有哪些约束





SELECT * FROM information_schema.table_constraints
WHERE table_name = 'test2'; #查看都有哪些约束

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | email           | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | id              | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | last_name       | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | uk_test2_email  | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | uk_test3_email  | dbtest13     | test2      | UNIQUE          | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
5 rows in set (0.00 sec)
```



```mysql
-- 删除列级约束
ALTER TABLE test2 
DROP INDEX last_name;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0






-- 删除表记约束
ALTER TABLE test2 
DROP INDEX uk_test3_email;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0






--  查看都有哪些约束
SELECT * FROM information_schema.table_constraints
WHERE table_name = 'test2';

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | email           | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | id              | dbtest13     | test2      | UNIQUE          | YES      |
| def                | dbtest13          | uk_test2_email  | dbtest13     | test2      | UNIQUE          | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
3 rows in set (0.00 sec)




```

> 注意：可以通过 `show index from 表名称; `查看表的索引





~~~mysql
show index from test2;
~~~

---

![image-20220703192014170](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220703192014170.png)

----





## 4. PRIMARY KEY 约束

### 4.1 作用

用来唯一标识表中的一行记录



### 4.2 关键字

primary key



### 4.3 特点

- 主键约束相当于**唯一约束+非空约束的组合**
- 主键约束列不允许重复，也不允许出现空值

---

![1555427492244](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/1555427492244.png)

---



- 一个表**最多只能有**一个主键约束，**建立主键约束可以在列级别创建，也可以在表级别上创建**




- 主键约束对应着表中的**一列或者多列**（复合主键）




- 如果是多列组合的复合主键约束，那么这些列都不允许为空值，并且组合的值不允许重复




- **MySQL的主键名总是PRIMARY**，就算自己命名了主键约束名也没用




- 当创建主键约束时，系统默认会在所在的列或列组合上建立对应的**主键索引**（能够根据主键查询的，就根据主键查询，效率更高）

  如果删除主键约束了，主键约束对应的索引就自动删除了

  


- 需要注意的一点是，不要修改主键字段的值

  因为主键是数据记录的唯一标识，如果修改了主键的值，就有可能会破坏数据的完整性



### 4.4 添加主键约束

（1）建表时指定主键约束

格式：

```mysql
create table 表名称(
	字段名  数据类型  primary key, #列级模式
    字段名  数据类型,  
    字段名  数据类型  
);



create table 表名称(
    字段名  数据类型,
    字段名  数据类型,  
    字段名  数据类型,
    [constraint 约束名] primary key(字段名) #表级模式
);
```





举例：

```mysql
-- 列级约束
create table test3(
	id int primary key,
    last_name varchar(20),
    ssalary decimal(10,2),
    email varchar(25)
);

Query OK, 0 rows affected (0.02 sec)



-- 表级约束
create table test4(
	id int,
    last_name varchar(20),
    ssalary decimal(10,2),
    email varchar(25),
    CONSTRAINT pk_test4_id PRIMARY KEY(id)
);

Query OK, 0 rows affected (0.01 sec)

```

```mysql
-- 查看test3约束
SELECT * FROM information_schema.table_constraints
WHERE table_name='test3';

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | PRIMARY         | dbtest13     | test3      | PRIMARY KEY     | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
1 row in set (0.00 sec)



-- 查看test4约束
SELECT * FROM information_schema.table_constraints
WHERE table_name='test4';

-- 尝试修改主键约束的名字，失败  
-- 约束名 PRIMARY
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | PRIMARY         | dbtest13     | test4      | PRIMARY KEY     | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
1 row in set (0.00 sec)



desc test3;
+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   | PRI | NULL    |       |
| last_name | varchar(20)   | YES  |     | NULL    |       |
| ssalary   | decimal(10,2) | YES  |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```





（2）建表后增加主键约束

```mysql
ALTER TABLE 表名称
ADD PRIMARY KEY(字段列表); #字段列表可以是一个字段，也可以是多个字段，如果是多个字段的话，是复合主键
```



~~~mysql
-- 建表时没有 主键约束 之后添加
create table test5(
	id int,
    last_name varchar(20),
    ssalary decimal(10,2),
    email varchar(25)
);

Query OK, 0 rows affected (0.02 sec)



-- 查看表结构
DESC test5;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  |     | NULL    |       |
| last_name | varchar(20)   | YES  |     | NULL    |       |
| ssalary   | decimal(10,2) | YES  |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)




-- 添加约束
ALTER TABLE test5
ADD PRIMARY KEY(id);

Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0





-- 查看表结构
DESC test5;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   | PRI | NULL    |       |
| last_name | varchar(20)   | YES  |     | NULL    |       |
| ssalary   | decimal(10,2) | YES  |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
~~~



### 4.5 关于复合主键

```mysql
create table 表名称(
	字段名  数据类型,
    字段名  数据类型,  
    字段名  数据类型,
    primary key(字段名1,字段名2)  #表示字段1和字段2的组合是唯一的，也可以有更多个字段
);
```



举例：

~~~mysql
-- 建表 
-- 复合字段作为主键

CREATE TABLE user1(
	id INT,
    name varchar(15),
    password varchar(25),
    primary key(name,password)
);

Query OK, 0 rows affected (0.01 sec)
~~~



~~~mysql
INSERT INTO user1
VALUES(1,'Tom1','abc');

Query OK, 1 row affected (0.00 sec)


INSERT INTO user1
VALUES(1,'Tom2','abc');

Query OK, 1 row affected (0.00 sec)





-- 查看数据
SELECT * FROM user1;

+------+------+----------+
| id   | name | password |
+------+------+----------+
|    1 | Tom1 | abc      |
|    1 | Tom2 | abc      |
+------+------+----------+
2 rows in set (0.00 sec)



-- 非空且唯一
INSERT INTO user1
VALUES(1,NULL,'abc');

ERROR 1048 (23000): Column 'name' cannot be null
-- 复合主键约束 任何一个字段都不能为空
~~~





### 4.6 删除主键约束



固定格式：

```mysql
alter table 表名称 
drop primary key;
```



举例：

```mysql
-- 删除主键约束
ALTER TABLE test5 
DROP PRIMARY KEY;

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 查看表结构

DESC test5;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | NO   |     | NULL    |       |
| last_name | varchar(20)   | YES  |     | NULL    |       |
| ssalary   | decimal(10,2) | YES  |     | NULL    |       |
| email     | varchar(25)   | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```





> 说明：删除主键约束，不需要指定主键名，因为一个表只有一个主键，删除主键约束后，非空还存在



## 5. 自增列：AUTO_INCREMENT

### 5.1 作用

某个字段的值自增



### 5.2 关键字

auto_increment



### 5.3 特点和要求

（1）一个表**最多只能有一个自增长列**

（2）当需要产生**唯一标识符**或顺序值时，可设置自增长

（3）自增长列约束的列必须是键列（主键列，唯一键列）

（4）自增约束的列的数据类型**必须是整数类型**

（5）如果自增列指定了 0 和 null，会在当前最大值的基础上自增；如果自增列手动指定了具体值，直接赋值为具体值

必须满足：

实体完整性、整数



错误演示：

```mysql
-- 没有唯一性约束
create table employee(
	eid int auto_increment,
    ename varchar(20)
);
# ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key   
```

```mysql
-- 不是整数
create table employee(
	eid int primary key,
    ename varchar(20) unique key auto_increment
);
# ERROR 1063 (42000): Incorrect column specifier for column 'ename'  因为ename不是整数类型
```



### 5.4 如何指定自增约束

**（1）建表时**

```mysql
-- 列级约束
create table 表名称(
	字段名  数据类型  primary key auto_increment,
    字段名  数据类型  unique key not null,  
    字段名  数据类型  unique key,
    字段名  数据类型  not null default 默认值, 
);


-- 表级约束
create table 表名称(
	字段名  数据类型 default 默认值 ,
    字段名  数据类型 unique key auto_increment,  
    字段名  数据类型 not null default 默认值,,
    primary key(字段名)
);
```



举例：

```mysql
create table test6(
	id int primary key auto_increment,
    last_name varchar(20)
);
Query OK, 0 rows affected (0.01 sec)
```

```mysql
-- 查看表结构
desc test6;


+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| id        | int         | NO   | PRI | NULL    | auto_increment |
| last_name | varchar(20) | YES  |     | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)




-- 新增数据
INSERT INTO test6(last_name)
VALUES('Tom');

Query OK, 1 row affected (0.00 sec)



INSERT INTO test6(last_name)
VALUES('Tom');

Query OK, 1 row affected (0.00 sec)


-- 查看数据
SELECT * FROM test6;

+----+-----------+
| id | last_name |
+----+-----------+
|  1 | Tom       |
|  2 | Tom       |
+----+-----------+
2 rows in set (0.00 sec)


-- 当我们向主键（含有auto_increament）的字段添加 0、NULL时
-- 会自动地网上递增
INSERT INTO test6(id,last_name)
VALUES(0,'Tom');

Query OK, 1 row affected (0.00 sec)


INSERT INTO test6(id,last_name)
VALUES(NULL,'Tom');

Query OK, 1 row affected (0.00 sec)


-- 查看数据
SELECT * FROM test6;

+----+-----------+
| id | last_name |
+----+-----------+
|  1 | Tom       |
|  2 | Tom       |
|  3 | Tom       |
|  4 | Tom       |
+----+-----------+
4 rows in set (0.00 sec)



-- 当我们向主键（含有auto_increament）的字段添加除了 0、NULL、且目前记录中不存在时
-- 可以添加成功

INSERT INTO test6(id,last_name)
VALUES(-100,'Tom');

Query OK, 1 row affected (0.00 sec)




INSERT INTO test6(id,last_name)
VALUES(6,'Tom');

Query OK, 1 row affected (0.00 sec)



-- 主机重复
INSERT INTO test6(id,last_name)
VALUES(1,'Tom');

ERROR 1062 (23000): Duplicate entry '1' for key 'test6.PRIMARY'




INSERT INTO test6(id,last_name)
VALUES(0,'Tom');

-- 查看数据
SELECT * FROM test6;

+------+-----------+
| id   | last_name |
+------+-----------+
| -100 | Tom       |
|    1 | Tom       |
|    2 | Tom       |
|    3 | Tom       |
|    4 | Tom       |
|    6 | Tom       |
|    7 | Tom       |
+------+-----------+
7 rows in set (0.00 sec)
```







**（2）建表后**

```mysql
alter table 表名称 
modify 字段名 数据类型 auto_increment;
```



例如：

```mysql
-- 建表
create table test7(
	ed int primary key ,
    last_name varchar(20)
);

Query OK, 0 rows affected (0.02 sec)
```



```mysql
-- 查看表结构
desc test7;

+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| ed        | int         | NO   | PRI | NULL    |       |
| last_name | varchar(20) | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)



-- 添加自增
ALTER TABLE test7
MODIFY ed INT AUTO_INCREMENT;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0





-- 查看表结构
desc test7;

+-----------+-------------+------+-----+---------+----------------+
| Field     | Type        | Null | Key | Default | Extra          |
+-----------+-------------+------+-----+---------+----------------+
| ed        | int         | NO   | PRI | NULL    | auto_increment |
| last_name | varchar(20) | YES  |     | NULL    |                |
+-----------+-------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
```



### 5.5 如何删除自增约束

格式：

```mysql
#alter table 表名称 modify 字段名 数据类型 auto_increment;#给这个字段增加自增约束

alter table 表名称 
modify 字段名 数据类型; #去掉auto_increment相当于删除
```





```mysql
-- 删除自增约束

alter table test7
modify ed int;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

```mysql
-- 查看表结构

desc test7;

+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| ed        | int         | NO   | PRI | NULL    |       |
| last_name | varchar(20) | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```





### 5.6 MySQL 8.0新特性—自增变量的持久化

在MySQL 8.0之前，自增主键AUTO_INCREMENT的值如果大于max(primary key)+1，在MySQL重启后，会重置AUTO_INCREMENT=max(primary key)+1，这种现象在某些情况下会导致业务主键冲突或者其他难以发现的问题。
下面通过案例来对比不同的版本中自增变量是否持久化。



在MySQL 5.7版本中，测试步骤如下：

创建的数据表中包含自增主键的id字段，语句如下：

```mysql
CREATE TABLE test1(
id INT PRIMARY KEY AUTO_INCREMENT
);
```



插入4个空值，执行如下：

```mysql
INSERT INTO test1
VALUES(0),(0),(0),(0);
```



查询数据表test1中的数据，结果如下：

```mysql
SELECT * FROM test1;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
+----+
4 rows in set (0.00 sec)

```



删除id为4的记录，语句如下：

```mysql
DELETE FROM test1 WHERE id = 4;
```



再次插入一个空值，语句如下：

```mysql
INSERT INTO test1 VALUES(0);
```



查询此时数据表test1中的数据，结果如下：

```mysql
mysql> SELECT * FROM test1;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  5 | -- 裂缝
+----+
4 rows in set (0.00 sec)
```

从结果可以看出，虽然删除了id为4的记录，但是再次插入空值时，并没有重用被删除的4，而是分配了5。



删除id为5的记录，结果如下：

```mysql
DELETE FROM test1 where id=5;
```





**重启数据库**，重新插入一个空值

```mysql
INSERT INTO test1 values(0);
```





再次查询数据表test1中的数据，结果如下：

```mysql
SELECT * FROM test1;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
+----+
4 rows in set (0.00 sec)
```

从结果可以看出，新插入的0值分配的是4，按照重启前的操作逻辑，此处应该分配6。出现上述结果的主要原因是自增主键没有持久化。



结论：

在MySQL 5.7系统中，对于自增主键的分配规则，是由InnoDB数据字典内部一个`计数器`来决定的，而该计数器只在`内存中维护`，并不会持久化到磁盘中。当数据库重启时，该计数器会被初始化







在MySQL 8.0版本中，上述测试步骤最后一步的结果如下：

```mysql
SELECT * FROM test1;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  6 |
+----+
4 rows in set (0.00 sec)
```

从结果可以看出，自增变量已经持久化了。





结论：

MySQL 8.0将自增主键的计数器持久化到`重做日志`中

每次计数器发生改变，都会将其写入重做日志中

如果数据库重启，InnoDB会根据重做日志中的信息来初始化计数器的内存值







## 6. FOREIGN KEY 约束

### 6.1 作用

限定某个表的某个字段的引用完整性**（参照完整性）**

比如：员工表的员工所在部门的选择，必须在部门表能找到对应的部分。

----

![1555428214706](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/1555428214706.png)

---



### 6.2 关键字

FOREIGN KEY



### 6.3 主表和从表/父表和子表

主表（父表）：被引用的表，被参考的表

从表（子表）：引用别人的表，参考别人的表

例如：员工表的员工所在部门这个字段的值要参考部门表：部门表是主表，员工表是从表。

例如：学生表、课程表、选课表：选课表的学生和课程要分别参考学生表和课程表，学生表和课程表是主表，选课表是从表



### 6.4 特点

（1）从表的外键列，必须参考 **主表** 的 **主键或唯一约束** 的列

​          为什么？因为被依赖/被参考的值必须是唯一的

（2）在创建外键约束时，如果不给外键约束命名

​			**默认名不是列名，而是自动产生一个外键名**（例如 student_ibfk_1;）

​			也可以指定外键约束名。

（3）创建(CREATE)表时就指定外键约束的话，**先创建主表**，再创建从表

（4）删表时，**先删从表**（或先删除外键约束），**再删除主表**

（5）当主表的记录被从表参照时，主表的记录将不允许删除，如果要删除数据，需要先删除从表中依赖该记录的数据，然后才可以删除主表的数据

（6）在“从表”中指定外键约束，并且**一个表可以建立多个外键约束**

（7）从表的外键列与主表被参照的列**名字可以不相同**，但是数据类型必须一样，逻辑意义一致

​			如果类型不一样，创建子表时，就会出现错误“ERROR 1005 (HY000): Can't create table'database.tablename'(errno: 150)”。

​          例如：都是表示部门编号，都是int类型。

（8）**当创建外键约束时，系统默认会在所在的列上建立对应的普通索引**。但是**索引名是外键的约束名**（外键的约束名，如果不是自定义的，就会自己生成一个，			不是列名）。（根据外键查询效率很高）

（9）删除外键约束后，**必须`手动`删除对应的索引**



注意：

外键可以自关联



### 6.5 添加外键约束

（1）建表时

```mysql
-- 先创建主表
create table 主表名称(
	字段1  数据类型  primary key,
    字段2  数据类型
);



-- 后创建从表
create table 从表名称(
	字段1  数据类型  primary key,
    字段2  数据类型,
    [CONSTRAINT <外键约束名称>] FOREIGN KEY（从表的某个字段) references 主表名(被参考字段)
);
#(从表的某个字段)的数据类型必须与主表名(被参考字段)的数据类型一致，逻辑意义也一样
#(从表的某个字段)的字段名可以与主表名(被参考字段)的字段名一样，也可以不一样

-- FOREIGN KEY: 在表级指定子表中的列
-- REFERENCES: 标示在父表中的列
```



举例:

```mysql
-- 创建主表
create table dept( 			#主表
	id int primary key,		#部门编号  主表中的
    name varchar(50)		#部门名称
);

Query OK, 0 rows affected (0.01 sec)



-- 创建子表
create table emp(#从表
	id int primary key,  #员工编号
    name varchar(5),     #员工姓名
    department_id int,	 #员工所在的部门
    -- 表级约束
    CONSTRAINT fk_emp_department_id foreign key (department_id) references dept(id)   #在从表中指定外键约束
    #emp表的deptid和和dept表的did的数据类型一致，意义都是表示部门的编号
);

Query OK, 0 rows affected (0.02 sec)

#这两个表创建时，没有指定外键的话，那么创建顺序是随意

/*
说明：
（1）主表dept必须先创建成功，然后才能创建emp表，指定外键成功。
（2）删除表时，先删除从表emp，再删除主表dept
*/



-- 查看约束

SELECT * FROM information_schema.table_constraints
WHERE table_name ='emp';

+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME      | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | PRIMARY              | dbtest13     | emp        | PRIMARY KEY     | YES      |
| def                | dbtest13          | fk_emp_department_id | dbtest13     | emp        | FOREIGN KEY     | YES      |
+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
2 rows in set (0.00 sec)
```



（2）建表后

一般情况下，表与表的关联都是提前设计好了的，因此，会在创建表的时候就把外键约束定义好。不过，如果需要修改表的设计（比如添加新的字段，增加新的关联关系），但没有预先定义外键约束，那么，就要用修改表的方式来补充定义。

格式：

```mysql
ALTER TABLE 从表名 
ADD [CONSTRAINT 约束名] FOREIGN KEY (从表的字段) REFERENCES 主表名(被引用字段) [on update xx][on delete xx];
```



举例：

```mysql
ALTER TABLE emp1
ADD [CONSTRAINT emp_dept_id_fk] FOREIGN KEY(dept_id) REFERENCES dept(dept_id);
```



举例：

```mysql

```

```mysql
alter table emp add foreign key (deptid) references dept(did);
```



### 6.6 演示问题



**演示外键效果**

~~~MYSQL
-- 主表没有记录 
-- 从表不能添加

INSERT INTO emp
VALUES(1001,'Tom',12);

ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`dbtest13`.`emp`, CONSTRAINT `fk_emp_department_id` FOREIGN KEY (`department_id`) REFERENCES `dept` (`id`))




-- 添加部门记录
INSERT INTO dept
VALUES(12,'IT');

Query OK, 1 row affected (0.01 sec)




-- 添加员工记录

INSERT INTO emp
VALUES(1001,'Tom',12);

Query OK, 1 row affected (0.00 sec)





-- 删除失败
delete from dept 
where id=12;

ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`dbtest13`.`emp`, CONSTRAINT `fk_emp_department_id` FOREIGN KEY (`department_id`) REFERENCES `dept` (`id`))


-- 更新失败
UPDATE dept 
set id=10
where id=12;

ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails (`dbtest13`.`emp`, CONSTRAINT `fk_emp_department_id` FOREIGN KEY (`department_id`) REFERENCES `dept` (`id`))
~~~



总结：约束关系是针对双方的

* 添加了外键约束后，主表的修改和删除数据受约束

* 添加了外键约束后，从表的添加和修改数据受约束
* 在从表上建立外键，要求主表必须存在
* 删除主表时，要求从表从表先删除，或将从表中外键引用该主表的关系先删除



### 6.7 约束等级

* `Cascade方式`：**在父表上update/delete记录时，同步update/delete掉子表的匹配记录** 
* `Set null方式`：在父表上update/delete记录时，将子表上匹配记录的列设为null，但是要注意子表的外键列不能为not null  



* `No action方式`：如果子表中有匹配的记录，则不允许对父表对应候选键进行update/delete操作  
* `Restrict方式`：同no action， 都是立即检查外键约束
* `Set default方式`（在可视化工具SQLyog中可能显示空白）：父表有变更时，子表将外键列设置成一个默认的值，但Innodb不能识别

注意：

如果没有指定等级，就相当于Restrict方式。

对于外键约束，最好是采用: `ON UPDATE CASCADE ON DELETE RESTRICT` 的方式。



（1）演示1：on update cascade on delete set null

```mysql
create table dept1(
	did int primary key,		#部门编号
    dname varchar(50)			#部门名称
);

Query OK, 0 rows affected (0.02 sec)



create table emp1(
	eid int primary key,  #员工编号
    ename varchar(5),     #员工姓名
    deptid int,				#员工所在的部门
    foreign key (deptid) references dept1(did)  on update cascade on delete set null
    -- 把修改操作设置为级联修改等级
    -- 把删除操作设置为set null等级
);

Query OK, 0 rows affected (0.03 sec)
```





```mysql
insert into dept1 values(1001,'教学部');
insert into dept1 values(1002, '财务部');
insert into dept1 values(1003, '咨询部');

Query OK, 1 row affected (0.00 sec)



insert into emp1 values(1,'张三',1001); #在添加这条记录时，要求部门表有1001部门
insert into emp1 values(2,'李四',1001);
insert into emp1 values(3,'王五',1002);

Query OK, 1 row affected (0.00 sec)
```



```mysql
select * from dept1;

+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1002 | 财务部 |
| 1003 | 咨询部 |
+------+--------+
3 rows in set (0.00 sec)



select * from emp1;

+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   1002 |
+-----+-------+--------+
3 rows in set (0.00 sec)
```

```mysql
update dept set did = 1004 where did = 1002;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0




-- 修改主表成功，从表也跟着修改，修改了主表被引用的字段1002为1004，从表的引用字段就跟着修改为1004了
select * from dept1;
+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1003 | 咨询部 |
| 1004 | 财务部 | #原来是1002，修改为1004
+------+--------+
3 rows in set (0.00 sec)



select * from emp1;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   1004 | #原来是1002，跟着修改为1004
+-----+-------+--------+
3 rows in set (0.00 sec)
```





```mysql
delete from dept where did = 1001;
Query OK, 1 row affected (0.01 sec)






-- 删除主表的记录成功，从表对应的字段的值被修改为null

 select * from dept1;
+------+--------+
| did  | dname  | #记录1001部门被删除了
+------+--------+
| 1003 | 咨询部  |
| 1004 | 财务部  |
+------+--------+
2 rows in set (0.00 sec)






select * from emp1;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   NULL | #原来引用1001部门的员工，deptid字段变为null
|   2 | 李四  |   NULL |
|   3 | 王五  |   1004 |
+-----+-------+--------+
3 rows in set (0.00 sec)
```





（2）演示2：on update set null on delete cascade

```mysql
create table dept2(
	did int primary key,		#部门编号
    dname varchar(50)			#部门名称
);

create table emp2(
	eid int primary key,  #员工编号
    ename varchar(5),     #员工姓名
    deptid int,				#员工所在的部门
    foreign key (deptid) references dept2(did)  on update set null on delete cascade
    #把修改操作设置为set null等级，把删除操作设置为级联删除等级
);
```



```mysql
insert into dept2 values(1001,'教学部');
insert into dept2 values(1002, '财务部');
insert into dept2 values(1003, '咨询部');

insert into emp2 values(1,'张三',1001); #在添加这条记录时，要求部门表有1001部门
insert into emp2 values(2,'李四',1001);
insert into emp2 values(3,'王五',1002);
```



```mysql
select * from dept2;
+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1002 | 财务部 |
| 1003 | 咨询部 |
+------+--------+
3 rows in set (0.00 sec)


select * from emp2;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   1002 |
+-----+-------+--------+
3 rows in set (0.00 sec)
```





```mysql
#修改主表，从表对应的字段设置为null
update dept2 set did = 1004 where did = 1002;

Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0





select * from dept2;
+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1003 | 咨询部 |
| 1004 | 财务部 | #原来did是1002
+------+--------+
3 rows in set (0.00 sec)



select * from emp2;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   NULL | #原来deptid是1002，因为部门表1002被修改了，1002没有对应的了，就设置为null
+-----+-------+--------+
3 rows in set (0.00 sec)
```

```mysql
delete from dept2 where did=1001;
Query OK, 1 row affected (0.00 sec)





-- 删除主表的记录成功，主表的1001行被删除了，从表相应的记录也被删除了
select * from dept;
+------+--------+
| did  | dname  | #部门表中1001部门被删除
+------+--------+
| 1003 | 咨询部 |
| 1004 | 财务部 |
+------+--------+
2 rows in set (0.00 sec)





select * from emp;
+-----+-------+--------+
| eid | ename | deptid |#原来1001部门的员工也被删除了
+-----+-------+--------+
|   3 | 王五  |   NULL |
+-----+-------+--------+
1 row in set (0.00 sec)

```









（3）演示：on update cascade on delete cascade

```mysql
create table dept3(
	did int primary key,		#部门编号
    dname varchar(50)			#部门名称
);





create table emp3(
	eid int primary key,  #员工编号
    ename varchar(5),     #员工姓名
    deptid int,				#员工所在的部门
    foreign key (deptid) references dept3(did)  on update cascade on delete cascade
    #把修改操作设置为级联修改等级，把删除操作也设置为级联删除等级
);
```

```mysql
insert into dept3 values(1001,'教学部');
insert into dept3 values(1002, '财务部');
insert into dept3 values(1003, '咨询部');



insert into emp3 values(1,'张三',1001); #在添加这条记录时，要求部门表有1001部门
insert into emp3 values(2,'李四',1001);
insert into emp3 values(3,'王五',1002);
```

```mysql
select * from dept3;

+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1002 | 财务部 |
| 1003 | 咨询部 |
+------+--------+
3 rows in set (0.00 sec)




 select * from emp3;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   1002 |
+-----+-------+--------+
3 rows in set (0.00 sec)
```

```mysql
#修改主表，从表对应的字段自动修改
update dept3 set did = 1004 where did = 1002;

Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0


select * from dept3;
+------+--------+
| did  | dname  |
+------+--------+
| 1001 | 教学部 |
| 1003 | 咨询部 |
| 1004 | 财务部 | #部门1002修改为1004
+------+--------+
3 rows in set (0.00 sec)



select * from emp3;
+-----+-------+--------+
| eid | ename | deptid |
+-----+-------+--------+
|   1 | 张三  |   1001 |
|   2 | 李四  |   1001 |
|   3 | 王五  |   1004 | #级联修改
+-----+-------+--------+
3 rows in set (0.00 sec)
```





```mysql
#删除主表的记录成功，主表的1001行被删除了，从表相应的记录也被删除了
delete from dept3 where did=1001;

Query OK, 1 row affected (0.00 sec)



select * from dept3;
+------+--------+
| did  | dname  | #1001部门被删除了
+------+--------+
| 1003 | 咨询部 |
| 1004 | 财务部 | 
+------+--------+
2 rows in set (0.00 sec)


select * from emp3;
+-----+-------+--------+
| eid | ename | deptid |  #1001部门的员工也被删除了
+-----+-------+--------+
|   3 | 王五  |   1004 |
+-----+-------+--------+
1 row in set (0.00 sec)
```



### 6.8 删除外键约束

流程如下：

```mysql
-- (1)第一步先查看约束名和删除外键约束
SELECT * FROM information_schema.table_constraints 
WHERE table_name = '表名称';#查看某个表的约束名





ALTER TABLE 从表名
DROP FOREIGN KEY 外键约束名;




-- （2）第二步查看索引名和删除索引。（注意，只能手动删除）
SHOW INDEX FROM 表名称; #查看某个表的索引名

ALTER TABLE 从表名 DROP INDEX 索引名;

```

举例：

```mysql
-- 查看约束
SELECT * FROM information_schema.table_constraints
WHERE table_name = 'emp';

+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME      | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
| def                | dbtest13          | PRIMARY              | dbtest13     | emp        | PRIMARY KEY     | YES      |
| def                | dbtest13          | fk_emp_department_id | dbtest13     | emp        | FOREIGN KEY     | YES      |
+--------------------+-------------------+----------------------+--------------+------------+-----------------+----------+
2 rows in set (0.00 sec)




-- 删除外键约束
alter table emp 
drop foreign key fk_emp_department_id;

Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```



查看索引

~~~mysql
show index from emp;
~~~

---

![image-20220703233647255](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220703233647255.png)

---



删除与外键关联的普通索引

~~~mysql
alter table emp
drop index fk_emp_department_id;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 查看索引
show index from emp;
~~~



----

![image-20220703233857408](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220703233857408.png)

---



### 6.9 开发场景

**问题1：如果两个表之间有关系（一对一、一对多），比如：员工表和部门表（一对多），它们之间是否一定要建外键约束？**

答：不是的



**问题2：建和不建外键约束有什么区别？**

答：建外键约束，你的操作（创建表、删除表、添加、修改、删除）会受到限制，从语法层面受到限制。

例如：在员工表中不可能添加一个员工信息，它的部门的值在部门表中找不到。

不建外键约束，你的操作（创建表、删除表、添加、修改、删除）不受限制，要保证数据的`引用完整性`，只能依`靠程序员的自觉`，或者是`在Java程序中进行限定`。例如：在员工表中，可以添加一个员工的信息，它的部门指定为一个完全不存在的部门。

**问题3：那么建和不建外键约束和查询有没有关系？**

答：没有



> 在 MySQL 里，外键约束是有成本的，需要消耗系统资源。对于大并发的 SQL 操作，有可能会不适合。
>
> 比如大型网站的中央数据库，可能会`因为外键约束的系统开销而变得非常慢`。
>
> 所以， MySQL 允许你不使用系统自带的外键约束，在`应用层面`完成检查数据一致性的逻辑。
>
> 也就是说，即使你不用外键约束，也要想办法通过应用层面的附加逻辑，来实现外键约束的功能，确保数据的一致性。





### 6.10 阿里开发规范

【`强制`】不得使用外键与级联，一切外键概念必须在应用层解决。 



说明：（概念解释）学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新，即为级联更新



不使用外键的原因：

1、外键与级联更新适用于`单机低并发`，不适合`分布式`、`高并发集群`；

2、级联更新是强阻塞，存在数据库`更新风暴`的风险；

3、外键影响数据库的`插入速度`





## 7. CHECK 约束

### 7.1 作用

检查某个字段的值是否符号xx要求，一般指的是值的范围



### 2、关键字

CHECK



### 3、说明：MySQL 5.7 不支持（check约束）

MySQL5.7 可以使用check约束，但check约束对数据验证没有任何作用。添加数据时，没有任何错误或警告



但是**MySQL 8.0中可以使用check约束了**。

```mysql
create table test10(
	id INT ,
    last_name varchar(15),
    salary decimal(10,2) check(salary >2000)
);
Query OK, 0 rows affected (0.02 sec)

```

```mysql
insert into test10 values(1,'张三',2500);

Query OK, 1 row affected (0.01 sec)



-- 不符合检查约束
insert into test10 values(1,'张三',1500);

ERROR 3819 (HY000): Check constraint 'test10_chk_1' is violated.
```



## 8. DEFAULT约束

### 8.1 作用

给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默认值



### 8.2 关键字

DEFAULT



### 8.3 如何给字段加默认值



（1）建表时

```mysql
create table 表名称(
	字段名  数据类型  primary key,
    字段名  数据类型  unique key not null,  
    字段名  数据类型  unique key,
    字段名  数据类型  not null default 默认值, 
);

create table 表名称(
	字段名  数据类型 default 默认值 ,
    字段名  数据类型 not null default 默认值,  
    字段名  数据类型 not null default 默认值,
    primary key(字段名),
    unique key(字段名)
);

说明：默认值约束一般不在唯一键和主键列上加
```

```mysql
create table test11(
	id INT,
    last_name varchar(15),
    salary decimal(10,2) default 2000
);

Query OK, 0 rows affected (0.01 sec)


desc test11;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  |     | NULL    |       |
| last_name | varchar(15)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | 2000.00 |       |
+-----------+---------------+------+-----+---------+-------+
3 rows in set (0.01 sec)
```



```mysql
insert into test11 values(1,'汪飞',1000); 

Query OK, 1 row affected (0.00 sec)




INSERT INTO test11 (id,last_name)
VALUES(2,'tom');

Query OK, 1 row affected (0.00 sec)
```

```mysql
-- 查看数据
select * from test11;

+------+-----------+---------+
| id   | last_name | salary  |
+------+-----------+---------+
|    1 | 汪飞      | 1000.00 |
|    2 | tom       | 2000.00 |
+------+-----------+---------+
2 rows in set (0.00 sec)
```







再举例：

```mysql
CREATE TABLE myemp(
id INT AUTO_INCREMENT PRIMARY KEY,
NAME VARCHAR(15),
salary DOUBLE(10,2) DEFAULT 2000
);
```



（2）建表后

```mysql
alter table 表名称 
modify 字段名 数据类型 default 默认值;

#如果这个字段原来有非空约束，你还保留非空约束，那么在加默认值约束时，还得保留非空约束，否则非空约束就被删除了
#同理，在给某个字段加非空约束也一样，如果这个字段原来有默认值约束，你想保留，也要在modify语句中保留默认值约束，否则就删除了


-- 保留非空约束
alter table 表名称
modify 字段名 数据类型 default 默认值 not null;
```





~~~mysql
-- 建表
CREATE TABLE test12(
	id INT,
    last_name varchar(15),
    salary decimal(10,2)
);

Query OK, 0 rows affected (0.01 sec)


-- 查看表结构

DESC test12;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  |     | NULL    |       |
| last_name | varchar(15)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
3 rows in set (0.01 sec)





-- 添加默认约束
alter table test12
modify salary decimal(10,3) default 1000;#给tel字段增加默认值约束，并保留非空约束

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

```mysql
-- 查看表结构
desc test12;

+-----------+---------------+------+-----+----------+-------+
| Field     | Type          | Null | Key | Default  | Extra |
+-----------+---------------+------+-----+----------+-------+
| id        | int           | YES  |     | NULL     |       |
| last_name | varchar(15)   | YES  |     | NULL     |       |
| salary    | decimal(10,3) | YES  |     | 1000.000 |       |
+-----------+---------------+------+-----+----------+-------+
3 rows in set (0.00 sec)
```



### 8.4 如何删除默认值约束

```mysql
alter table 表名称 
modify 字段名 数据类型 ;#删除默认值约束，也不保留非空约束




alter table 表名称
modify 字段名 数据类型  not null; #删除默认值约束，保留非空约束
```





举例：

```mysql
alter table test12
modify salary decimal(10,2); #删除gender字段默认值约束，如果有非空约束，也一并删除

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

```

```mysql
-- 查看表结构
 
desc test12;

+-----------+---------------+------+-----+---------+-------+
| Field     | Type          | Null | Key | Default | Extra |
+-----------+---------------+------+-----+---------+-------+
| id        | int           | YES  |     | NULL    |       |
| last_name | varchar(15)   | YES  |     | NULL    |       |
| salary    | decimal(10,2) | YES  |     | NULL    |       |
+-----------+---------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```



## 9. 面试

**面试1、为什么建表时，加 not null default '' 或 default 0**

答：不想让表中出现null值



**面试2、为什么不想要 null 的值**

答:（1）不好比较。null是一种特殊值，比较时只能用专门的is null 和 is not null来比较。碰到运算符，通常返回null。

​     （2）效率不高。影响提高索引效果。因此，我们往往在建表时 not null default '' 或 default 0



**面试3、带AUTO_INCREMENT约束的字段值是从1开始的吗？**
在MySQL中，默认AUTO_INCREMENT的初始值是1，每新增一条记录，字段值自动加1。

设置自增属性（AUTO_INCREMENT）的时候，还可以指定第一条插入记录的自增字段的值，这样新插入的记录的自增字段值从初始值开始递增，如在表中插入第一条记录，同时指定id值为5，则以后插入的记录的id值就会从6开始往上增加。

添加主键约束时，往往需要设置字段自动增加属性



**面试4、并不是每个表都可以任意选择存储引擎？**
外键约束（FOREIGN KEY）不能跨引擎使用。

MySQL支持多种存储引擎，每一个表都可以指定一个不同的存储引擎，需要注意的是：

外键约束是用来保证数据的参照完整性的，如果表之间需要关联外键，却指定了不同的存储引擎，那么这些表之间是不能创建外键约束的

所以说，存储引擎的选择也不完全是随意的





# 课后练习题



## 基础练习： 


## 练习1 

已经存在数据库test04_emp，两张表emp2和dept2

~~~mysql
-- 建库
CREATE DATABASE test04_emp;

Query OK, 1 row affected (0.01 sec)


-- 切换数据库
use test04_emp;

Database changed


-- 建表
CREATE TABLE emp2(
id INT,
emp_name VARCHAR(15)
);

Query OK, 0 rows affected (0.02 sec)


-- 建表
CREATE TABLE dept2(
id INT,
dept_name VARCHAR(15)
);

Query OK, 0 rows affected (0.02 sec)
~~~



### 1.向表emp2的id列中添加PRIMARY KEY约束

~~~mysql
ALTER TABLE emp2
ADD  PRIMARY KEY(id);

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC emp2;

+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | NO   | PRI | NULL    |       |
| emp_name | varchar(15) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
2 rows in set (0.01 sec)
~~~





### 2. 向表dept2的id列中添加PRIMARY KEY约束

~~~MYSQL
ALTER TABLE dept2
ADD PRIMARY KEY(id);

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 查看表结构
DESC dept2;

+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| id        | int         | NO   | PRI | NULL    |       |
| dept_name | varchar(15) | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
2 rows in set (0.00 sec)
~~~



### 3. 向表emp2中添加列dept_id，并在其中定义FOREIGN KEY约束，与之相关联的列是dept2表中的id列。

~~~mysql
-- 添加字段
ALTER TABLE emp2
ADD dept_id INT;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 查看表结构
DESC emp2;

+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int         | NO   | PRI | NULL    |       |
| emp_name | varchar(15) | YES  |     | NULL    |       |
| dept_id  | int         | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)



-- 添加外键约束
ALTER TABLE emp2
ADD CONSTRAINT fk_emp2_deptid FOREIGN KEY(dept_id) REFERENCES dept2(id);-- 引用主表的id字段，且此字段必须有唯一性约束

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 查看约束
SELECT * FROM information_schema.table_constraints
WHERE table_name='emp2';

+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | test04_emp        | PRIMARY         | test04_emp   | emp2       | PRIMARY KEY     | YES      |
| def                | test04_emp        | fk_emp2_deptid  | test04_emp   | emp2       | FOREIGN KEY     | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
2 rows in set (0.00 sec)

~~~



## 练习2 

承接《第11章_数据处理之增删改》的综合案例。

~~~mysql
-- 切换数据库
use test01_library;

Database changed



-- 查看表结构
DESC books;

+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| id      | int          | YES  |     | NULL    |       |
| name    | varchar(50)  | YES  |     | NULL    |       |
| authors | varchar(100) | YES  |     | NULL    |       |
| price   | float        | YES  |     | NULL    |       |
| pubdate | year         | YES  |     | NULL    |       |
| note    | varchar(100) | YES  |     | NULL    |       |
| num     | int          | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
7 rows in set (0.00 sec)
~~~



---





使用ALTER语句给books按如下要求增加相应的约束

---

![image-20220704092627739](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220704092627739.png)

---



### 针对主键字段

方式一：给id列添加约束

 ~~~mysql
-- 给id增加主键约束
ALTER TABLE books
ADD PRIMARY KEY(id);

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 给id字段增加自增约束
ALTER TABLE books
MODIFY id INT AUTO_INCREMENT;

Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0





-- 查看表结构
DESC books;

+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int          | NO   | PRI | NULL    | auto_increment |
| name    | varchar(50)  | YES  |     | NULL    |                |
| authors | varchar(100) | YES  |     | NULL    |                |
| price   | float        | YES  |     | NULL    |                |
| pubdate | year         | YES  |     | NULL    |                |
| note    | varchar(100) | YES  |     | NULL    |                |
| num     | int          | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
7 rows in set (0.00 sec)
 ~~~





方式二：删除id列的约束，一步到位添加约束

~~~mysql
-- 删除id列的自增约束
ALTER TABLE books
MODIFY id INT;

Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0




-- 删除id列的主键约束
ALTER TABLE books
DROP PRIMARY KEY;

Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC books;

+---------+--------------+------+-----+---------+-------+
| Field   | Type         | Null | Key | Default | Extra |
+---------+--------------+------+-----+---------+-------+
| id      | int          | NO   |     | NULL    |       |
| name    | varchar(50)  | YES  |     | NULL    |       |
| authors | varchar(100) | YES  |     | NULL    |       |
| price   | float        | YES  |     | NULL    |       |
| pubdate | year         | YES  |     | NULL    |       |
| note    | varchar(100) | YES  |     | NULL    |       |
| num     | int          | YES  |     | NULL    |       |
+---------+--------------+------+-----+---------+-------+
7 rows in set (0.00 sec)





-- 一步到位添加约束
ALTER TABLE books
MODIFY id INT PRIMARY KEY AUTO_INCREMENT;

Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC books;

+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int          | NO   | PRI | NULL    | auto_increment |
| name    | varchar(50)  | YES  |     | NULL    |                |
| authors | varchar(100) | YES  |     | NULL    |                |
| price   | float        | YES  |     | NULL    |                |
| pubdate | year         | YES  |     | NULL    |                |
| note    | varchar(100) | YES  |     | NULL    |                |
| num     | int          | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
7 rows in set (0.00 sec)
~~~



### 针对非主键字段

~~~mysql
-- 名称
ALTER TABLE books
MODIFY name varchar(50) NOT NULL;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 作者
ALTER TABLE books
MODIFY authors varchar(100) NOT NULL;


Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 价格
ALTER TABLE books
MODIFY price decimal(10,2) NOT NULL;



Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0



-- 发布年
ALTER TABLE books
MODIFY pubdate YEAR NOT NULL;


Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 数量
ALTER TABLE books
MODIFY num INT NOT NULL;


Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC books;

+---------+---------------+------+-----+---------+----------------+
| Field   | Type          | Null | Key | Default | Extra          |
+---------+---------------+------+-----+---------+----------------+
| id      | int           | NO   | PRI | NULL    | auto_increment |
| name    | varchar(50)   | NO   |     | NULL    |                |
| authors | varchar(100)  | NO   |     | NULL    |                |
| price   | decimal(10,2) | NO   |     | NULL    |                |
| pubdate | year          | NO   |     | NULL    |                |
| note    | varchar(100)  | YES  |     | NULL    |                |
| num     | int           | NO   |     | NULL    |                |
+---------+---------------+------+-----+---------+----------------+
7 rows in set (0.00 sec)
~~~





## 练习3



### 1、创建数据库test04_company

~~~mysql
CREATE DATABASE IF NOT EXISTS test04_company;

Query OK, 1 row affected (0.00 sec)



-- 切换数据库
USE test04_company;

Database changed
~~~



### 2、按照下表给出的表结构在test04_company数据库中创建两个数据表offices和employees

- Office表

---

![image-20220704101650872](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220704101650872.png)

---



~~~mysql
CREATE TABLE office(
	officeCode INT PRIMARY KEY ,
    city VARCHAR(50) NOT NULL,
    address VARCHAR(50),
    country VARCHAR(50) NOT NULL,
    postalCode VARCHAR(15) UNIQUE
    -- ,CONSTRAINT uk_office_postalCode UNIQUE(postalCode)
);

Query OK, 0 rows affected (0.03 sec)






-- 查看表结构
DESC office;

+------------+-------------+------+-----+---------+-------+
| Field      | Type        | Null | Key | Default | Extra |
+------------+-------------+------+-----+---------+-------+
| officeCode | int         | NO   | PRI | NULL    |       |
| city       | varchar(50) | NO   |     | NULL    |       |
| address    | varchar(50) | YES  |     | NULL    |       |
| country    | varchar(50) | NO   |     | NULL    |       |
| postalCode | varchar(15) | YES  | UNI | NULL    |       |
+------------+-------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
~~~





- employees表

----

![image-20220704101719064](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220704101719064.png)

---

~~~mysql
CREATE TABLE employees(
	employeeNumber INT PRIMARY KEY AUTO_INCREMENT,
    lastName VARCHAR(50) NOT NULL,
    firstName VARCHAR(50) NOT NULL,
    mobile VARCHAR(25) UNIQUE,
    officeCode INT NOT NULL,
    jobTitle VARCHAR(50) NOT NULL,
    birth DATETIME NOT NULL,
    note VARCHAR(255),
    sex VARCHAR(5),
    CONSTRAINT fk_employees_officeCode FOREIGN KEY(officeCode) REFERENCES office(officeCode)
);
Query OK, 0 rows affected (0.02 sec)




-- 查看表结构
DESC employees;

+----------------+--------------+------+-----+---------+----------------+
| Field          | Type         | Null | Key | Default | Extra          |
+----------------+--------------+------+-----+---------+----------------+
| employeeNumber | int          | NO   | PRI | NULL    | auto_increment |
| lastName       | varchar(50)  | NO   |     | NULL    |                |
| firstName      | varchar(50)  | NO   |     | NULL    |                |
| mobile         | varchar(25)  | YES  | UNI | NULL    |                |
| officeCode     | int          | NO   | MUL | NULL    |                |
| jobTitle       | varchar(50)  | NO   |      | NULL    |                |
| birth          | datetime     | NO   |     | NULL    |                |
| note           | varchar(255) | YES  |     | NULL    |                |
| sex            | varchar(5)   | YES  |     | NULL    |                |
+----------------+--------------+------+-----+---------+----------------+
9 rows in set (0.01 sec)
~~~



### 3、将表employees的mobile字段修改到officeCode字段后面

~~~MYSQL
ALTER TABLE employees
MODIFY mobile varchar(25) AFTER officeCode;

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC employees;

+----------------+--------------+------+-----+---------+----------------+
| Field          | Type         | Null | Key | Default | Extra          |
+----------------+--------------+------+-----+---------+----------------+
| employeeNumber | int          | NO   | PRI | NULL    | auto_increment |
| lastName       | varchar(50)  | NO   |     | NULL    |                |
| firstName      | varchar(50)  | NO   |     | NULL    |                |
| officeCode     | int          | NO   | MUL | NULL    |                |
| mobile         | varchar(25)  | YES  | UNI | NULL    |                |
| jobTitle       | varchar(50)  | NO   |     | NULL    |                |
| birth          | datetime     | NO   |     | NULL    |                |
| note           | varchar(255) | YES  |     | NULL    |                |
| sex            | varchar(5)   | YES  |     | NULL    |                |
+----------------+--------------+------+-----+---------+----------------+
9 rows in set (0.00 sec)
~~~





### 4、将表employees的birth字段改名为employee_birth

~~~mysql
ALTER TABLE employees
CHANGE birth employee_birth DATETIME;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC employees;

+----------------+--------------+------+-----+---------+----------------+
| Field          | Type         | Null | Key | Default | Extra          |
+----------------+--------------+------+-----+---------+----------------+
| employeeNumber | int          | NO   | PRI | NULL    | auto_increment |
| lastName       | varchar(50)  | NO   |     | NULL    |                |
| firstName      | varchar(50)  | NO   |     | NULL    |                |
| officeCode     | int          | NO   | MUL | NULL    |                |
| mobile         | varchar(25)  | YES  | UNI | NULL    |                |
| jobTitle       | varchar(50)  | NO   |     | NULL    |                |
| employee_birth | datetime     | YES  |     | NULL    |                |
| note           | varchar(255) | YES  |     | NULL    |                |
| sex            | varchar(5)   | YES  |     | NULL    |                |
+----------------+--------------+------+-----+---------+----------------+
9 rows in set (0.00 sec)
~~~



### 5、修改sex字段，数据类型为CHAR(1)，非空约束

~~~mysql
ALTER TABLE employees
MODIFY sex CHAR(1) NOT NULL;

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0


-- 查看表结构
DESC employees;

+----------------+--------------+------+-----+---------+----------------+
| Field          | Type         | Null | Key | Default | Extra          |
+----------------+--------------+------+-----+---------+----------------+
| employeeNumber | int          | NO   | PRI | NULL    | auto_increment |
| lastName       | varchar(50)  | NO   |     | NULL    |                |
| firstName      | varchar(50)  | NO   |     | NULL    |                |
| officeCode     | int          | NO   | MUL | NULL    |                |
| mobile         | varchar(25)  | YES  | UNI | NULL    |                |
| jobTitle       | varchar(50)  | NO   |     | NULL    |                |
| employee_birth | datetime     | YES  |     | NULL    |                |
| note           | varchar(255) | YES  |     | NULL    |                |
| sex            | char(1)      | NO   |     | NULL    |                |
+----------------+--------------+------+-----+---------+----------------+
9 rows in set (0.00 sec)
~~~



### 6、 删除字段note

~~~MYSQL
ALTER TABLE employees
DROP COLUMN note;

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC employees;

+----------------+-------------+------+-----+---------+----------------+
| Field          | Type        | Null | Key | Default | Extra          |
+----------------+-------------+------+-----+---------+----------------+
| employeeNumber | int         | NO   | PRI | NULL    | auto_increment |
| lastName       | varchar(50) | NO   |     | NULL    |                |
| firstName      | varchar(50) | NO   |     | NULL    |                |
| officeCode     | int         | NO   | MUL | NULL    |                |
| mobile         | varchar(25) | YES  | UNI | NULL    |                |
| jobTitle       | varchar(50) | NO   |     | NULL    |                |
| employee_birth | datetime    | YES  |     | NULL    |                |
| sex            | char(1)     | NO   |     | NULL    |                |
+----------------+-------------+------+-----+---------+----------------+
8 rows in set (0.00 sec)
~~~



### 7、 增加字段名favoriate_activity，数据类型为VARCHAR(100)

~~~mysql
ALTER TABLE employees
ADD favoriate_activity 	VARCHAR(100);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0




-- 查看表结构
DESC employees;

+--------------------+--------------+------+-----+---------+----------------+
| Field              | Type         | Null | Key | Default | Extra          |
+--------------------+--------------+------+-----+---------+----------------+
| employeeNumber     | int          | NO   | PRI | NULL    | auto_increment |
| lastName           | varchar(50)  | NO   |     | NULL    |                |
| firstName          | varchar(50)  | NO   |     | NULL    |                |
| officeCode         | int          | NO   | MUL | NULL    |                |
| mobile             | varchar(25)  | YES  | UNI | NULL    |                |
| jobTitle           | varchar(50)  | NO   |     | NULL    |                |
| employee_birth     | datetime     | YES  |     | NULL    |                |
| sex                | char(1)      | NO   |     | NULL    |                |
| favoriate_activity | varchar(100) | YES  |     | NULL    |                |
+--------------------+--------------+------+-----+---------+----------------+
9 rows in set (0.00 sec)
~~~



### 8、将表employees名称修改为employees_info

~~~MYSQL
RENAME TABLE employees TO employees_info;

Query OK, 0 rows affected (0.01 sec)





-- 查看表结构
DESC employees_info;

+--------------------+--------------+------+-----+---------+----------------+
| Field              | Type         | Null | Key | Default | Extra          |
+--------------------+--------------+------+-----+---------+----------------+
| employeeNumber     | int          | NO   | PRI | NULL    | auto_increment |
| lastName           | varchar(50)  | NO   |     | NULL    |                |
| firstName          | varchar(50)  | NO   |     | NULL    |                |
| officeCode         | int          | NO   | MUL | NULL    |                |
| mobile             | varchar(25)  | YES  | UNI | NULL    |                |
| jobTitle           | varchar(50)  | NO   |     | NULL    |                |
| employee_birth     | datetime     | YES  |     | NULL    |                |
| sex                | char(1)      | NO   |     | NULL    |                |
| favoriate_activity | varchar(100) | YES  |     | NULL    |                |
+--------------------+--------------+------+-----+---------+----------------+
9 rows in set (0.00 sec)
~~~

