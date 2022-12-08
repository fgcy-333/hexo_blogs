---
title: 第08章_索引的创建与设计原则
date: 2022-09-20 22:02:14
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

# 1. 索引的声明与使用 



## 1.1 索引的分类

MySQL的索引包括普通索引、唯一性索引、全文索引、单列索引、多列索引和空间索引等。 

- 从 功能逻辑 上说，索引主要有 4 种，分别是普通索引、唯一索引、主键索引、全文索引。 

- 按照 物理实现方式 ，索引可以分为 2 种：聚簇索引 和 非聚簇索引。 

- 按照 作用字段个数 进行划分，分成单列索引和联合索引。



<!--more-->



1. 普通索引 

​	在创建普通索引时,不附加任何限制条件,只是用于提高查询效率。这类索引可以创建在任何数据类型中,其值是否唯一和非空,要由字段本身的完整性约束条件决	    定。建立索引以后,可以通过索引进行查询。例如,在表student 的字段name上建立一个普通索引,查询记录时就可以根据该索引进行查询。





2. 唯一性索引 

​	使用`UNIQUE`参数可以设置索引为唯一性索引,在创建唯一性索引时,限制该索引的值必须是唯一的,但允许有空值。

​	在一张数据表里可以有多个唯一索引。例如,在表student 的字段email中创建唯一性索引,那么字段email的值就必须是唯一的。

​	通过唯一性索引,可以更快速地确定某条记录。



3. 主键索引 

​	主键索引就是一种特殊的唯一性索引,在唯一索引的基础上增加了不为空的约束,也就是NOT NULL + UNIQUE,一张表里最多只有一个主键索引。

​	Why? 这是由主键索引的物理实现方式决定的,因为数据存储在文件中只能按照一种顺序进行存储。



4. 单列索引 

​	在表中的单个字段上创建索引。单列索引只根据该字段进行索引。单列索引可以是普通索引,也可以是唯一性索引,还可以是全文索引。

​	只要保证该索引只对应一个字段即可。一个表可以有多个单列索引。



5. 多列(组合、联合)索引 

​	多列索引是在表的多个字段组合上创建一个索引。该索引指向创建时对应的多个字段,可以通过这几个字段进行查询,但是只有查询条件中使用了这些字段中的第一个字段时才会被使用。

​	例如,在表中的字段id,name和gender上建立一个多列索引`idx_id_name_gender`,只有在查询条件中使用了字段id时该索引才会被使用。

​	使用组合索引时遵循最左前缀集合。



6. 全文索引 

全文索引(也称全文检索)是目前搜索引擎使用的一种关键技术。它能够利用【分词技术】等多种算法智能分析出文本文字中关键词的频率和重要性,然后按照一定的算法规则智能地筛选出我们想要的搜索结果。全文索引非常适合大型数据集,对于小的数据集,它的用处比较小。使用参数FULLTEXT 可以设置索引为全文索引。在定义索引的列上支持值的全文查找,允许在这些索引列中插入重复值和空值。全文索引只能创建在CHAR、VARCHAR 或TEXT类型及其系列类型的字段上,查询数据量较大的字符串类型的字段时,使用全文索引可以提高查询速度。例如,表student的字段information是TEXT类型,该字段包含了很多文字信息。在字段information上建立全文索引后,可以提高查询字段information的速度。全文索引典型的有两种类型:自然语言的全文索引和布尔全文索引。

自然语言搜索引擎将计算每一个文档对象和查询的相关度。这里,相关度是基于匹配的关键词的个数,以及关键词在文档中出现的次数。在整个索引中出现次数越少的词语,匹配时的相关度就越高。相反,非常常见的单词将不会被搜索,如果一个词语的在超过50%的记录中都出现了,那么自然语言的搜索将不会搜索这类词语。



MySQL数据库从3.23.23版开始支持全文索引,但MySQL5.6.4以前只有Myisam支持,5.6.4版本以后innodb才支持,但是官方版本不支持中文分词,需要第三方分词插件。在5.7.6版本, MySQL内置了ngram全文解析器,用来支持亚洲语种的分词。测试或使用全文索引时,要先看一下自己的MySQL版本、存储引擎和数据类型是否支持全文索引。

随着大数据时代的到来,关系型数据库应对全文索引的需求已力不从心,逐渐被solr, ElasticSearch等专门的搜索引警所替代。





7. 补充：空间索引 

使用参数SPATIAL 可以设置索引为空间索引。空间索引只能建立在空间数据类型上,这样可以提高系统获取空间数据的效率。MySQL中的空间数据类型包括GEOMETRY, POINT、LINESTRING 和POLYGON 等。目前只有MyISAM存储引擎支持空间检索,而且索引的字段不能为空值。对于初学者来说,这类索引很少会用到。



小结:不同的存储引擎支持的索引类型也不一样

- InnoDB :支持B-tree、 Full-text 等索引,不支持Hash索引【但有自适应hash】;

- MyISAM :支持B-tree, Full-text等索引,不支持Hash索引;

- Memory :支持B-tree, Hash 等索引,不支持 Full-text索引;

- NDB :支持Hash索引,不支持B-tree、Full-text等索引;

- Archive :T支 B-tree, Hash, Full-text 等索引;









## 1.2创建索引

MySQL支持多种方法在单个或多个列上创建索引:在创建表的定义语句`CREATE TABLE`中指定索引列,使用`ALTER TABLE` 语句在存在的表上创建索引,或者使用`CREATE INDEX`语句在已存在的表上添加索引。



### **创建表的时候创建索**

​	引使用CREATE TABLE创建表时,除了可以定义列的数据类型外,还可以定义主键约束、外键约束或者唯一性约束,而不论创建哪种约束,在定义约束的同时相当于在指定列上创建了一个索引。



举例：

隐式的方式创建索引。在声明有主键约束、唯一性约束、外键约束的字段上,会**自动**的添加相关的索引。

~~~mysql
CREATE TABLE emp(
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_name VARCHAR(20) UNIQUE, -- 字段使用unique约束没有指定约束名 字段名就是约束名也是索引名
    dept_id INT,
    CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept_id) -- 外键约束起了名字，所以会用约束名作为索引名
);
~~~

----

![image-20220920233049981](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209202330443.png)

----





~~~mysql
CREATE TABLE dept(
    dept_id INT PRIMARY KEY AUTO_INCREMENT,
    dept_name VARCHAR(20)
);
~~~

----

![image-20220920232921155](C:\Users\fgcy\AppData\Roaming\Typora\typora-user-images\image-20220920232921155.png)

---







显式的方式创建:

显式创建表时创建索引的话，基本语法格式如下：

~~~mysql
CREATE TABLE table_name [col_name data_type]
[UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC | DESC]
~~~

- UNIQUE 、 FULLTEXT 和 SPATIAL 为可选参数，分别表示唯一索引、全文索引和空间索引；

- INDEX 与 KEY 为同义词，两者的作用相同，用来指定创建索引；

- index_name 指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名； 

- col_name 为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择； 

- length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度； 

- ASC 或 DESC 指定升序或者降序的索引值存储

>  特例：主键索引使用主键约束的方式来创建。



**1、创建普通的索引**

在book表中的year_publication字段上建立普通索引，SQL语句如下：

~~~mysql
CREATE TABLE book(
    book_id INT ,
    book_name VARCHAR(100),
    authors VARCHAR(100),
    info VARCHAR(100) ,
    comment VARCHAR(100),
    year_publication YEAR,
    INDEX(year_publication) -- 申明索引
);
~~~

---

![image-20220920234400231](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209202344344.png)

----





查看表结构：

~~~mysql
show create table book\G
*************************** 1. row ***************************
       Table: book
Create Table: CREATE TABLE `book` (
  `book_id` int DEFAULT NULL,
  `book_name` varchar(100) DEFAULT NULL,
  `athors` varchar(100) DEFAULT NULL,
  `info` varchar(100) DEFAULT NULL,
  `comment` varchar(100) DEFAULT NULL,
  `year_publication` year DEFAULT NULL,
  KEY `idx_bname` (`book_name`)-- 索引
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
1 row in set (0.01 sec)
~~~



通过命令查看索引：

~~~java
show index from book;
+-------+------------+-----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name  | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+-----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book  |          1 | idx_bname |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+-----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.17 sec) 
~~~

这里只是介绍基本用法。







性能分析工具：

~~~mysql
explain select * from book where book_name='mysql高级';

+----+-------------+-------+------------+------+---------------+-----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key       | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+-----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | book  | NULL       | ref  | idx_bname     | idx_bname | 403     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+-----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.01 sec)
~~~



**2、创建唯一索引：**

~~~mysql
 create table book1(
    book_id int,
    comment varchar(10),
    unique index uk_idx_cmt(comment)
);
    
Query OK, 0 rows affected (0.03 sec)
~~~



查看索引：	

~~~mysql
show index from book1;
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name   | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book1 |          0 | uk_idx_cmt |            1 | comment     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.01 sec)
~~~





唯一索引起效：

~~~mysql
insert into book1 values(1,'mysql高级');
Query OK, 1 row affected (0.00 sec)

insert into book1 values(2,'mysql高级');
ERROR 1062 (23000): Duplicate entry 'mysql高级' for key 'book1.uk_idx_cmt'
~~~

> 声明有唯一索引的字段，在添加数据时，要保证唯一性，但是可以添加NULL
>
> 可以多次添加NULL值







**3、主键索引**

设定为主键后数据库会自动建立索引，innodb为聚簇索引

通过定义主键约束的方式定义主键索引（隐式创建），语法：

~~~mysql
 CREATE TABLE book2(
    book_id int primary key auto_increment,
    comment varchar(10)
);
Query OK, 0 rows affected (0.02 sec)


show index from book2;
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book2 |          0 | PRIMARY  |            1 | book_id     | A         |           0 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)

~~~



通过删除主键约束的方式删除主键索引

~~~~mysql
ALTER TABLE book2
DROP PRIMARY KEY;
    
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
~~~~

> 原因：申明有AUTO_INCREMENT的字段必须是唯一的
>
> 主键：非空且唯一



再举例：

~~~mysql
DROP TABLE book2;
Query OK, 0 rows affected (0.01 sec)



CREATE TABLE book2(
    book_id int primary key ,
    comment varchar(10)
);
Query OK, 0 rows affected (0.02 sec)


ALTER TABLE book2
DROP PRIMARY KEY;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

> 修改主键索引：必须先删除掉(drop)原索引，再新建(add)索引





**创建单列索引**

~~~mysql
mysql> CREATE TABLE test2(
     id INT NOT NULL,
     name CHAR(50) NULL,
     INDEX single_idx_name(name(20))
 );
Query OK, 0 rows affected (0.02 sec)
~~~

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

~~~mysql
mysql> SHOW INDEX FROM test2 \G
*************************** 1. row ***************************
        Table: test2
   Non_unique: 1
     Key_name: single_idx_name
 Seq_in_index: 1
  Column_name: name
    Collation: A
  Cardinality: 0
     Sub_part: 20
       Packed: NULL
         Null: YES
   Index_type: BTREE
      Comment:
Index_comment:
      Visible: YES
   Expression: NULL
1 row in set (0.01 sec)
~~~





**4、创建联合索引**

~~~mysql
mysql> create table book3(
    id int,
    name varchar(10),
    index mul_bid_bname(id,name)
);
Query OK, 0 rows affected (0.02 sec)
~~~





查看索引：

~~~mysql
show index from book3;
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name      | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book3 |          1 | mul_bid_bname |            1 | id          | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book3 |          1 | mul_bid_bname |            2 | name        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.01 sec)

~~~







性能分析工具：

初略认识一下最左前缀原则：

~~~mysql
-- 满足
explain select * from book3 where id=1001 and name='zhang';
+----+-------------+-------+------------+------+---------------+---------------+---------+-------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key           | key_len | ref         | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------------+------+----------+-------------+
|  1 | SIMPLE      | book3 | NULL       | ref  | mul_bid_bname | mul_bid_bname | 48      | const,const |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+---------------+---------+-------------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)






-- 不满足
explain select * from book3 where name='zhang';
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key           | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | book3 | NULL       | index | mul_bid_bname | mul_bid_bname | 48      | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+---------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
~~~









**5、创建全文索引**

FULLTEXT:全文索引可以用于全文搜索，并且只为CHAR、VARCHAR和TEXT列创建索引。索引总是对整个列进行，不支持局部（前缀）索引。



举例1：创建表test4，在表中的info字段上建立全文索引，SQL语句如下：

~~~mysql
CREATE TABLE test4(
    id INT NOT NULL,
    name CHAR(30) NOT NULL,
    age INT NOT NULL,
    info VARCHAR(255),
    FULLTEXT INDEX futxt_idx_info(info(50))-- 骚操作，避免B+Tree过大
) ENGINE=MyISAM;

Query OK, 0 rows affected (0.01 sec)




show index from test4;
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name       | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| test4 |          1 | futxt_idx_info |            1 | info        | NULL      |        NULL |     NULL |   NULL | YES  | FULLTEXT   |         |               | YES     | NULL       |
+-------+------------+----------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)
~~~

> 在MySQL5.7及之后版本中可以不指定最后的ENGINE了，因为在此版本中InnoDB支持全文索引。





举例:

~~~mysql
CREATE TABLE `papers` (
    `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
    `title` varchar(200) DEFAULT NULL,
    `content` text,
    PRIMARY KEY (`id`),
    FULLTEXT KEY `title` (`title`,`content`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
~~~





不同于like方式的的查询：

~~~mysql
SELECT * FROM papers WHERE content LIKE ‘%查询字符串%’;
~~~



全文索引用match+against方式查询：

~~~mysql
SELECT * FROM papers WHERE MATCH(title,content) AGAINST (‘查询字符串’);
~~~



> 注意点 :
>
> 1. 使用全文索引前，搞清楚版本支持情况；
>
> 2. 全文索引比 like + % 快 N 倍，但是可能存在精度问题； 
> 3. 如果需要全文索引的是大量数据，建议先添加数据，再创建索引。





**6、创建空间索引**

空间索引创建中，要求空间类型的字段必须为 非空 。 举例：创建表test5，在空间类型为GEOMETRY的字段上创建空间索引，SQL语句如下：

~~~mysql
CREATE TABLE test5(
    geo GEOMETRY NOT NULL,
    SPATIAL INDEX spa_idx_geo(geo)
) ENGINE=MyISAM;
~~~



### 在已经存在的表上创建索引

在已经存在的表中创建索引可以使用ALTER TABLE语句或者CREATE INDEX语句。





1、使用ALTER TABLE语句创建索引 

ALTER TABLE语句创建索引的基本语法如下：

~~~mysql
ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY]
[index_name] (col_name[length],...) [ASC | DESC]
~~~



创建表:

~~~mysql
create table book5(
    book_id int,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100),
    COMMENT VARCHAR (100),
    year_publication YEAR
);
Query OK, 0 rows affected (0.02 sec)
~~~



建表后添加索引：【普通索引】

~~~mysql
ALTER TABLE book5
ADD INDEX idx_com(comment);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看：

~~~mysql
 show index from book5;
 
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          1 | idx_com  |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)
~~~





建表后添加唯一索引：

~~~mysql
ALTER TABLE book5
ADD UNIQUE uk_idx_bname(book_name);

Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看索引：

~~~mysql
 show index from book5;
 
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          0 | uk_idx_bname |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | idx_com      |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.00 sec)
~~~





建表后，添加联合索引：

~~~mysql
alter table book5 add index mul_bid_bname_indo(book_id,book_name,info);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

> 注意：联合索引中属性的先后顺序是有讲究的





查看索引：

~~~mysql
  show index from book5;
  
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name           | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          0 | uk_idx_bname       |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | idx_com            |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            1 | book_id     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            2 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            3 | info        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.01 sec)
~~~









### 为某个表创建索引

> CREATE INDEX ......ON.......





创建一个表：

~~~mysql
create table book6(
    book_id int,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100),
    COMMENT VARCHAR (100),
    year_publication YEAR
);

Query OK, 0 rows affected (0.02 sec)
~~~





查看索引：

~~~mysql
show index from book6;

Empty set (0.01 sec)
~~~









创建普通索引：

~~~mysql
CREATE INDEX idx_cmt ON book6(COMMENT);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看索引：

~~~mysql
 show index from book6;
 
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book6 |          1 | idx_cmt  |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.01 sec)

~~~





创建唯一索引：

~~~mysql
CREATE UNIQUE INDEX uk_idx_bname ON book6(book_name);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看索引：

~~~mysql
 show index from book6;
 
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book6 |          0 | uk_idx_bname |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book6 |          1 | idx_cmt      |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.01 sec)
~~~





创建联合索引：

~~~mysql
CREATE INDEX mul_bid_bname_indo ON book6(book_id,book_name,info);

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~





查看索引：

~~~mysql
 show index from book6;
 
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name           | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book6 |          0 | uk_idx_bname       |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book6 |          1 | idx_cmt            |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book6 |          1 | mul_bid_bname_indo |            1 | book_id     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book6 |          1 | mul_bid_bname_indo |            2 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book6 |          1 | mul_bid_bname_indo |            3 | info        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.00 sec)
~~~



## 1.3 删除索引

MySQL中删除索引使用ALTER TABLE或者DROP INDEX语句，两者可实现相同的功能，DROP INDEX语句在内部被映射到一个ALTER TABLE语句中。



查看索引：

~~~mysql
  show index from book5;
  
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name           | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          0 | uk_idx_bname       |            1 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | idx_com            |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            1 | book_id     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            2 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            3 | info        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.00 sec)

~~~







1. 使用ALTER TABLE删除索引 

   ALTER TABLE删除索引的基本语法格式如下：

~~~mysql
ALTER TABLE table_name DROP INDEX index_name;
~~~



举例：

~~~mysql
alter table book5
drop index idx_com;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



> 提示
> 添加AUTO_INCREMENT约束字段的唯一索引不能被删除。







2. DROP INDEX ......ON .....

~~~~mysql
drop index uk_idx_bname On book5;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~~







删除字段对联合索引的影响：





查看索引：

~~~mysql
SHOW INDEX FROM book5;

+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name           | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          1 | mul_bid_bname_indo |            1 | book_id     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            2 | book_name   | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            3 | info        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
3 rows in set (0.01 sec)
~~~





删除 字段`book_name`

~~~~mysql
ALTER TABLE book5
DROP COLUMN book_name;

Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~~



只剩下两个了：

~~~mysql
 show index from book5;
 
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name           | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book5 |          1 | mul_bid_bname_indo |            1 | book_id     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book5 |          1 | mul_bid_bname_indo |            2 | info        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.00 sec)
~~~





删除剩下的字段：

~~~mysql
ALTER TABLE book5
DROP COLUMN book_id;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

ALTER TABLE book5
DROP COLUMN info;

Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~





查看索引：

~~~mysql
SHOW INDEX FROM book5;
 
Empty set (0.00 sec)
~~~





> 提示
> 删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都
> 被删除，则整个索引将被删除。











# 2. MySQL8.0索引新特性

降序索引以降序存储键值。虽然在语法上，从MySQL4版本开始就已经支持降序索引的语法了，但实际上该DESC定义是被忽略的，直到MySQL8.x版本才开始  **真正支持降序索引**（**仅限于noDB存储引擎**）。
MySQL在  **8.0版本之前**  创建的  仍然是  **升序索引**，使用时进行  **反向扫描**，这大大降低了数据库的效率。在某些场景下，降序索引意义重大。

例如，如果一个查询，需要对多个列进行排序，且顺序要求不一致，那么使用降序索引将会避免数据库使用额外的文件排序操作，从而提高性能。



## 2.1 支持降序索引 

举例：分别在MySQL 5.7版本和MySQL 8.0版本中创建数据表ts1，结果如下：





**Mysql8.0**

~~~mysql
CREATE TABLE ts1(
    a INT,
    b INT,
    INDEX idx_a_b(a ASC,b DESC)
);
    
Query OK, 0 rows affected (0.02 sec)
~~~



查看索引：

~~~mysql
show index from ts1;

+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| ts1   |          1 | idx_a_b  |            1 | a           | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| ts1   |          1 | idx_a_b  |            2 | b           | D         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.01 sec)
~~~



查看建表语句：

~~~mysql
SHOW CREATE TABLE ts1;
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                           |
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ts1   | CREATE TABLE `ts1` (
  `a` int DEFAULT NULL,
  `b` int DEFAULT NULL,
  KEY `idx_a_b` (`a`,`b` DESC)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
~~~





在MySQL 5.7版本中查看数据表ts1的结构，结果如下：

---

![image-20220925120337899](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209251203056.png)

---

从结果可以看出，索引仍然是默认的升序。



在MySQL 8.0版本中查看数据表ts1的结构，结果如下：

---

![image-20220925120410605](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209251204392.png)

---

从结果可以看出，索引已经是降序了。下面继续测试降序索引在执行计划中的表现。 







分别在MySQL 5.7版本和MySQL 8.0版本的数据表ts1中插入800条随机数据，执行语句如下：

~~~mysql
DELIMITER //
CREATE PROCEDURE ts_insert()
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i< 800
	DO
		insert into ts1 select rand()*80000,rand()*80000;
		SET i=i+1;
		
	END WHILE
	commit; 
END //
DELIMITER ;
~~~







在MySQL 5.7版本中查看数据表ts1的执行计划，结果如下：

~~~mysql
EXPLAIN SELECT * FROM ts1 ORDER BY a,b DESC LIMIT 5;
~~~

---

![image-20220925131131013](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209251311424.png)

---

从结果可以看出，执行计划中扫描数为799，而且使用了Using filesort。

> 提示 Using filesort是MySQL中一种速度比较慢的外部排序，能避免是最好的。多数情况下，管理员 可以通过优化索引来尽量避免出现Using filesort，从而提高数据库执行速度









在MySQL 8.0版本中查看数据表ts1的执行计划。

---

![image-20220925131410160](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209251314879.png)

---

从结果可以看出，执行计划中扫描数为5，而且没有使用 Using filesort。



注意 降序索引只对查询中特定的排序顺序有效，如果使用不当，反而查询效率更低。

例如，上述 查询排序条件改为order by a desc, b desc，MySQL 5.7的执行计划要明显好于MySQL 8.0。

---

![image-20220925131717442](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209251317713.png)

---







## 2.2 隐藏索引

作用：

**1、为了可以兼容性删除**

在MySQL 5.7版本及之前，只能通过显式的方式删除索引。此时，如果发现删除索引后出现错误，又只能 通过显式创建索引的方式将删除的索引创建回来。

如果数据表中的数据量非常大，或者数据表本身比较 大，这种操作就会消耗系统过多的资源，操作成本非常高。 

从MySQL 8.x开始支持 隐藏索引（invisible indexes） ，只需要将待删除的索引设置为隐藏索引，使 查询优化器不再使用这个索引（即使使用force index（强制使用索引），优化器也不会使用该索引）， 确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。 

​	**这种通过先将索引设置为隐藏索 引，再删除索引的方式就是软删除** 。



**2、为了可以对比有无该索引的性能差别**

同时，如果你想验证某个索引删除之后的查询性能影响，就可以暂时先隐藏该索引。

> 注意：
> 主键不能被设置为隐藏索引。当表中没有显式主键时，表中第一个唯一非空索引会成为隐式主键，也不能设
> 置为隐藏索引。





索引默认是可见的，在使用CREATE TABLE,CREATE INDEX或者ALTER TABLE等语句时可以通过VISIBLE或者
INVISIBLE关键词设置索引的可见性





1. 创建表时直接创建 

在MySQL中创建隐藏索引通过SQL语句INVISIBLE来实现，其语法形式如下：

~~~mysql
CREATE TABLE tablename(
    propname1 type1[CONSTRAINT1],
    propname2 type2[CONSTRAINT2],
    ……
    propnamen typen,
    INDEX [indexname](propname1 [(length)]) INVISIBLE
);
~~~

上述语句比普通索引多了一个关键字`INVISIBLE`，用来标记索引为不可见索引。



举例：

~~~mysql
create table book7(
    book_id int,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100),
    COMMENT VARCHAR (100),
    year_publication YEAR,
    -- 创建不可见索引
    INDEX idx_cmt(comment) invisible
);

Query OK, 0 rows affected (0.03 sec)
~~~





查看索引：

~~~mysql
 show index from book7;
 
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book7 |          1 | idx_cmt  |            1 | COMMENT     | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
+-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)
~~~

> Visible为NO



查看索引是否生效：

~~~mysql
 explain select * from book7 where comment='asa';
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | book7 | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~

> 全表扫描，没有用到索引





2. 在已经存在的表上创建 

可以为已经存在的表设置隐藏索引，其语法形式如下：

~~~~mysql
CREATE INDEX indexname
ON tablename(propname[(length)]) INVISIBLE;
~~~~



举例：

~~~mysql
CREATE INDEX idx_pu_year ON book7(year_publication);

Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~

> 这个没有设置索引是不可见的，因为默认是可见的，所以是可见的。





性能工具验证，索引是生效的：

~~~mysql
EXPLAIN SELECT  * FROM book7 WHERE year_publication='2022';

+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key         | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | book7 | NULL       | ref  | idx_pu_year   | idx_pu_year | 2       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~







3. 通过ALTER TABLE语句创建

语法形式如下：

~~~mysql
ALTER TABLE tablename
ADD INDEX indexname (propname [(length)]) INVISIBLE;
~~~



举例：

~~~mysql
ALTER TABLE book7
ADD UNIQUE INDEX uk_idx_bname(book_name) INVISIBLE;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~





查看索引

两个不可见索引，一个可见索引：

~~~mysql
 show index from book7;
 
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name      | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book7 |          0 | uk_idx_bname |            1 | book_name        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
| book7 |          1 | idx_cmt      |            1 | COMMENT          | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
| book7 |          1 | idx_pu_year  |            1 | year_publication | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
3 rows in set (0.00 sec)
~~~





4. 切换索引可见状态 已存在的索引可通过如下语句切换可见状态：

~~~mysql
ALTER TABLE tablename ALTER INDEX index_name INVISIBLE; #切换成隐藏索引
ALTER TABLE tablename ALTER INDEX index_name VISIBLE; #切换成非隐藏索引
~~~



举例1：【可见---->不可见】

~~~mysql
ALTER TABLE book7 
AlTER INDEX idx_pu_year invisible;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



性能工具验证索引失效：

~~~mysql
EXPLAIN SELECT  * FROM book7 WHERE year_publication='2022';

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | book7 | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~





举例2：【不可见----->可见】

~~~mysql
ALTER TABLE book7
ALTER INDEX idx_cmt visible;

Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看索引：

~~~mysql
SHOW INDEX FROM book7;
    +-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name      | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book7 |          0 | uk_idx_bname |            1 | book_name        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
| book7 |          1 | idx_cmt      |            1 | COMMENT          | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book7 |          1 | idx_pu_year  |            1 | year_publication | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
3 rows in set (0.00 sec)
~~~





性能工具验证：

~~~mysql
EXPLAIN SELECT * FROM book7 WHERE COMMENT='sss';

+----+-------------+-------+------------+------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | book7 | NULL       | ref  | idx_cmt       | idx_cmt | 403     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



如果将index_cname索引切换成可见状态，通过explain查看执行计划，发现优化器选择了index_cname索 引。

了解：使隐藏索引对查询优化器可见【默认情况下，隐藏索引对优化器不可见】

通过设置隐藏索引的可见性可以查看索引对调优的帮助。



> 注意
>
> 当索引被隐藏时，它的内容仍然是和正常索引一样实时更新的。如果一个索引需要长期被隐 藏，那么可以将其删除，因为索引的存在会影响插入、更新和删除的性能。





5. 使隐藏索引对查询优化器可见

在MySQL 8.x版本中，为索引提供了一种新的测试方式，可以通过查询优化器的一个开关 （use_invisible_indexes）来打开某个设置，使隐藏索引对查询优化器可见。如果 use_invisible_indexes 设置为off(默认)，优化器会忽略隐藏索引。如果设置为on，即使隐藏索引不可见，优化器在生成执行计 划时仍会考虑使用隐藏索引。



（1）在MySQL命令行执行如下命令查看查询优化器的开关设置：

~~~mysql
 select @@optimizer_switch \G
 
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on
1 row in set (0.00 sec)
~~~

在输出的结果信息中找到如下属性配置：

~~~mysql
use_invisible_indexes=off
~~~

此属性配置值为off，说明隐藏索引默认对查询优化器不可见。





（2）使隐藏索引对查询优化器可见，需要在MySQL命令行执行如下命令：

~~~mysql
set session optimizer_switch="use_invisible_indexes=on";
~~~





SQL语句执行成功，再次查看查询优化器的开关设置：

~~~mysql
set session optimizer_switch="use_invisible_indexes=on";
Query OK, 0 rows affected (0.00 sec)



select @@optimizer_switch \G
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=on,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on
1 row in set (0.00 sec)
~~~





验证优化器已经对不可见索引生效：

~~~MYSQL
 EXPLAIN SELECT  * FROM book7 WHERE year_publication='2022';
 
+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key         | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | book7 | NULL       | ref  | idx_pu_year   | idx_pu_year | 2       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+-------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



~~~mysql
SHOW INDEX FROM book7;

+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table | Non_unique | Key_name     | Seq_in_index | Column_name      | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| book7 |          0 | uk_idx_bname |            1 | book_name        | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
| book7 |          1 | idx_cmt      |            1 | COMMENT          | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| book7 |          1 | idx_pu_year  |            1 | year_publication | A         |           0 |     NULL |   NULL | YES  | BTREE      |         |               | NO      | NULL       |
+-------+------------+--------------+--------------+------------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
3 rows in set (0.00 sec)
~~~











# 3. 索引的设计原则

为了使索引的使用效率更高，在创建索引时，必须考虑在哪些字段上创建索引和创建什么类型的索引。

索引设计不合理或者缺少索引都会对数据库和应用程序的性能造成障碍。高效的索引对于获得良好的性能非常重要。设计索引时，应该考虑相应准则







## 3.1 数据准备 

第1 步：创建数据库、创建表

~~~~mysql
CREATE DATABASE atguigudb1;


USE atguigudb1;


#1.创建学生表和课程表
CREATE TABLE `student_info` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `student_id` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `course_id` INT NOT NULL ,
    `class_id` INT(11) DEFAULT NULL,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;


CREATE TABLE `course` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `course_id` INT NOT NULL ,
    `course_name` VARCHAR(40) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
~~~~





第2步：创建模拟数据必需的存储函数

~~~mysql
#函数1：创建随机产生字符串函数
DELIMITER //
    CREATE FUNCTION rand_string(n INT)
    RETURNS VARCHAR(255) #该函数会返回一个字符串
    BEGIN
        DECLARE chars_str VARCHAR(100) DEFAULT
        'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
        DECLARE return_str VARCHAR(255) DEFAULT '';
        DECLARE i INT DEFAULT 0;
        WHILE i < n DO
        SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
        SET i = i + 1;
        END WHILE;
        RETURN return_str;
    END //
DELIMITER ;
~~~

创建函数，假如报错：

~~~
错误代码： 1418
This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
~~~



由于开启过慢查询日志bin-log, 我们就必须为我们的function指定一个参数。 主从复制，主机会将写操作记录在bin-log日志中。

从机读取bin-log日志，执行语句来同步数据。如果使 用函数来操作数据，会导致从机和主机操作时间不一致。所以，默认情况下，mysql不开启创建函数设 置。

~~~mysql
 SELECT  @@log_bin_trust_function_creators;
+-----------------------------------+
| @@log_bin_trust_function_creators |
+-----------------------------------+
|                                 0 |
+-----------------------------------+
~~~



命令开启：

允许创建函数设置：

~~~mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前会话有效。
~~~



mysqld重启，上述参数又会消失。

永久方法： 

windows下：my.ini[mysqld]加上：

~~~~mysql
log_bin_trust_function_creators=1
~~~~



linux下：/etc/my.cnf下my.cnf[mysqld]加上：

~~~mysql
log_bin_trust_function_creators=1
~~~





~~~mysql
#函数2：创建随机数函数
DELIMITER //
    CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
        RETURN i;
    END //
DELIMITER ;
~~~



第3步：创建插入模拟数据的存储过程

~~~mysql
# 存储过程1：创建插入课程表存储过程
DELIMITER //
    CREATE PROCEDURE insert_course( max_num INT )
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0; #设置手动提交事务
        REPEAT #循环
        SET i = i + 1; #赋值
        INSERT INTO course (course_id, course_name ) VALUES
        (rand_num(10000,10100),rand_string(6));
        UNTIL i = max_num
        END REPEAT;
        COMMIT; #提交事务
    END //
DELIMITER ;
~~~



~~~mysql
# 存储过程2：创建插入学生信息表存储过程
DELIMITER //
    CREATE PROCEDURE insert_stu( max_num INT )
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0; #设置手动提交事务
        REPEAT #循环
        SET i = i + 1; #赋值
        INSERT INTO student_info (course_id, class_id ,student_id ,NAME ) VALUES
        (rand_num(10000,10100),rand_num(10000,10200),rand_num(1,200000),rand_string(6));
        UNTIL i = max_num
        END REPEAT;
        COMMIT; #提交事务
    END //
DELIMITER ;
~~~



第4步：调用存储过程：

~~~mysql
CALL insert_course(100);
CALL insert_stu(1000000);
~~~







## 3.2 哪些情况适合创建索引 

### 1. 字段的数值有唯一性的限制

索引本身可以起到约束的作用，比如唯一索引、主键索引都是可以起到唯一件约束的，因此在我们的数据表中如果某个字段是唯一性的，就可以直接创建唯一性索引，或者主键索引。这样可以更快速地通过该索引来确定某条记录

例如，学生表中学号是具有唯一性的字段，为该字段建立唯一性索引可以很快确定某个学生的信息，如果使用名的话，可能存在同名现象，从而降低查询速度。

> 业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。（来源：Alibaba） 
>
> 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的。









### 2. 频繁作为 WHERE 查询条件的字段 

某个字段在SELECT语句的 WHERE 条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在 数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。

 比如student_info数据表（含100万条数据），假设我们想要查询 student_id=123110 的用户信息。

如果我们没有对 student_id 字段创建索引，进行如下查询:

~~~mysql
SELECT course_id, class_id, NAME,student_id
FROM student_info 
WHERE student_id= 123110;-- 230ms
~~~



查看索引情况：

~~~mysql
 SHOW INDEX FROM student_info;
 
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table        | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student_info |          0 | PRIMARY  |            1 | id          | A         |      997194 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)
~~~



给student id字段添加索引

~~~mysql
-- 给student id字段添加索引
ALTER TABLE student_info
ADD INDEX idx_sid(student_id)-- 2190ms
~~~



再次查询：

~~~mysql
SELECT course_id, class_id, NAME,student_id
FROM student_info 
WHERE student_id= 123110;-- 1ms
~~~



使用性能分析工具查看：

~~~mysql
EXPLAIN SELECT course_id, class_id, NAME,student_id
FROM student_info 
WHERE student_id= 123110;

+----+-------------+--------------+------------+------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | TABLE        | PARTITIONS | TYPE | possible_keys | KEY     | key_len | ref   | ROWS | filtered | Extra |
+----+-------------+--------------+------------+------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | student_info | NULL       | ref  | idx_sid       | idx_sid | 4       | const |    2 |   100.00 | NULL  |
+----+-------------+--------------+------------+------+---------------+---------+---------+-------+------+----------+-------+
1 ROW IN SET, 1 warning (0.00 sec)
~~~





### 3. 经常 GROUP BY 和 ORDER BY 的列 

索引就是让数据按照某种顺序进行存储或检索，因此当我们使用 GROUP BY 对数据进行分组查询，或者 使用 ORDER BY 对数据进行排序的时候，就需要 对分组或者排序的字段进行索引 。如果待排序的列有多个，那么可以在这些列上建立 组合索引 。

**GROUP BY** 

在有索引的情况下进行查找：

~~~mysql
SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id 
LIMIT 100; -- 1ms

+------------+-----+
| student_id | num |
+------------+-----+
|          1 |   9 |
|          2 |   6 |
|          3 |   3 |
|          4 |   4 |
|          5 |   9 |
|          6 |   5 |
....................
|        101 |   1 |
+------------+-----+
100 rows in set (0.00 sec)
~~~



将索引设置为不可见：

~~~mysql
ALTER TABLE student_info
ALTER INDEX idx_sid invisible;
~~~



再次查询：

~~~mysql
SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id 
LIMIT 100; -- 539ms
~~~







**ORDER BY** 

~~~mysql
 SHOW INDEX FROM student_info;
 
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table        | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student_info |          0 | PRIMARY  |            1 | id          | A         |      997194 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid  |            1 | student_id  | A         |      196684 |     NULL |   NULL |      | BTREE      |         |               | NO      | NULL       |
+--------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.00 sec)
~~~



修改student_id索引为可见的

~~~mysql
ALTER TABLE student_info
ALTER INDEX idx_sid visible;
~~~



添加单列索引

~~~mysql
ALTER TABLE student_info
ADD INDEX idx_cre_time(create_time);
~~~



查询：【既有GROUP BY 又有 ORDER BY】
~~~mysql
SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id
ORDER BY create_time DESC
LIMIT 100; -- 2min 12s
~~~





性能分析工具：

~~~mysql
EXPLAIN SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id
ORDER BY create_time DESC
LIMIT 100;

+----+-------------+--------------+------------+-------+---------------+---------+---------+------+--------+----------+---------------------------------+
| id | select_type | table        | partitions | type  | possible_keys | key     | key_len | ref  | rows   | filtered | Extra                           |
+----+-------------+--------------+------------+-------+---------------+---------+---------+------+--------+----------+---------------------------------+
|  1 | SIMPLE      | student_info | NULL       | index | idx_sid       | idx_sid | 4       | NULL | 997194 |   100.00 | Using temporary; Using filesort |
+----+-------------+--------------+------------+-------+---------------+---------+---------+------+--------+----------+---------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~

> 这里可以看到使用的是，idx_sid索引。
>
> 因为索引只能使用一个，且SQL语句的执行顺序是先GROUP BY 再ORDER BY



创建联合索引

~~~mysql
ALTER TABLE student_info
ADD INDEX idx_sid_cre_time(student_id,create_time DESC);
~~~







查看索引
~~~mysql
SHOW INDEX FROM student_info;

+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table        | Non_unique | Key_name         | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student_info |          0 | PRIMARY          |            1 | id          | A         |      997194 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid          |            1 | student_id  | A         |      196684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_cre_time     |            1 | create_time | A         |          56 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            1 | student_id  | A         |      197684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            2 | create_time | D         |      954014 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.01 sec)
~~~





查询测试：

~~~mysql
SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id
ORDER BY create_time DESC
LIMIT 100; -- 227ms
~~~





性能分析：

~~~mysql
EXPLAIN SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id
ORDER BY create_time DESC
LIMIT 100;

+----+-------------+--------------+------------+-------+--------------------------+------------------+---------+------+--------+----------+----------------------------------------------+
| id | select_type | table        | partitions | type  | possible_keys            | key              | key_len | ref  | rows   | filtered | Extra                                        |
+----+-------------+--------------+------------+-------+--------------------------+------------------+---------+------+--------+----------+----------------------------------------------+
|  1 | SIMPLE      | student_info | NULL       | index | idx_sid,idx_sid_cre_time | idx_sid_cre_time | 10      | NULL | 997194 |   100.00 | Using index; Using temporary; Using filesort |
+----+-------------+--------------+------------+-------+--------------------------+------------------+---------+------+--------+----------+----------------------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~

> 这里有两个可以使用的索引：idx_sid , idx_sid_cre_time 
>
> 这里会使用idx_sid_cre_time  ，更符合



若再创建一个联合索引

~~~mysql
ALTER TABLE student_info
ADD INDEX idx_cre_time_sid(create_time DESC，student_id);
~~~

并且删除之前创建的联合索引：

~~~mysql
ALTER TABLE student_info
DROP INDEX idx_sid_cre_time;
~~~

那么进行查询时，会使用哪个索引：

~~~mysql
SELECT student_id,COUNT(*) AS num FROM student_info
GROUP BY student_id
ORDER BY create_time DESC
LIMIT 100;
~~~

> 答案是使用 idx_sid ，因为是先执行GROUP BY语句，再执行ORDER BY语句
>
> 如果使用 idx_cre_time_sid，不符合使用场景。因为这个索引是以create_time开始，然后才是student_id
>
> 而该查询语句首先使用student_id，然后才是create_time





### 4. UPDATE、DELETE 的 WHERE 条件列 

当我们对某条数据讲行UPDATE 或者DELETE操作的时候、是否也需要对WHERE的条件列创建索引？

我们先看一下对数据进行UPDATE的情况：

我们想要把 name 为 462eed7ac6e791292a79 对应的 student_id 修改头10002，当我们没有对name 进行索引的时候，执行SOL语句：

执行之前先查找一下是否有 关于name字段的索引：

~~~mysql
SHOW INDEX FROM student_info;

+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table        | Non_unique | Key_name         | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student_info |          0 | PRIMARY          |            1 | id          | A         |      997194 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid          |            1 | student_id  | A         |      196684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_cre_time     |            1 | create_time | A         |          56 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            1 | student_id  | A         |      197684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            2 | create_time | D         |      954014 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.00 sec)
~~~



修改：

~~~mysql
UPDATE student_info SET student_id = 10002
WHERE name= '462eed7ac6e791292a79'; -- 609ms
~~~





给`name`字段添加索引：

~~~mysql
ALTER TABLE student_info
ADD INDEX idx_name(name);

<n>查询：ALTER TABLE student_info ADD INDEX idx_name(name)
~~~



查看索引：

~~~mysql
 SHOW INDEX FROM student_info;
 
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table        | Non_unique | Key_name         | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student_info |          0 | PRIMARY          |            1 | id          | A         |      997194 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid          |            1 | student_id  | A         |      196684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_cre_time     |            1 | create_time | A         |          56 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            1 | student_id  | A         |      197684 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_sid_cre_time |            2 | create_time | D         |      954014 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student_info |          1 | idx_name         |            1 | name        | A         |      505905 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+--------------+------------+------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
6 rows in set (0.00 sec)
~~~



再做一次更新：

~~~mysql
UPDATE student_info SET student_id = 10002
WHERE name= '462eed7ac6e791292a79'; -- 0ms
~~~



DELETE同理，这里不赘述。



对数据按照某个条件进行查询后再进行 UPDATE 或 DELETE 的操作，如果对 WHERE 字段创建了索引，就 能大幅提升效率。原理是因为我们需要先根据 WHERE 条件列检索出来这条记录，然后再对它进行更新或 删除。

如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更 新不需要对索引进行维护。





### 5. DISTINCT 字段需要创建索引 

有时候我们需要对某个字段进行去重，使用 DISTINCT，那么对这个字段创建索引，也会提升查询效率。 比如，我们想要查询课程表中不同的 student_id 都有哪些，如果我们没有对 student_id 创建索引，执行 SQL 语句：

~~~mysql
SELECT DISTINCT(student_id) FROM `student_info`;
~~~

运行结果（600637 条记录，运行时间 0.683s ）： 如果我们对 student_id 创建索引，再执行 SQL 语句：

~~~mysql
SELECT DISTINCT(student_id) FROM `student_info`;
~~~

运行结果（600637 条记录，运行时间 0.010s ）： 你能看到 SQL 查询效率有了提升，同时显示出来的 student_id 还是按照 递增的顺序 进行展示的。

这是因 为索引会对数据按照某种顺序进行排序，所以在去重的时候也会快很多。





### 6. 多表 JOIN 连接操作时，创建索引注意事项 

首先， **连接表的数量尽量不要超过 3 张** ，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增 长会非常快，严重影响查询的效率。

 其次， 对 WHERE 条件创建索引 ，因为 WHERE 才是对数据条件的过滤。如果在数据量非常大的情况下， 没有 WHERE 条件过滤是非常可怕的。

最后， 对用于连接的字段创建索引 ，并且该字段在多张表中的 类型必须一致 。



比如 course_id 在 student_info 表和 course 表中都为 int(11) 类型，而不能一个为 int 另一个为 varchar 类型。 举个例子，如果我们只对 student_id 创建索引，执行 SQL 语句：

~~~mysql
EXPLAIN SELECT s.course_id, s.name, s.student_id, c.course_name
FROM student_info s JOIN course c
ON s.course_id = c.course_id
WHERE name = '462eed7ac6e791292a79';

+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra                                      |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------------------------+
|  1 | SIMPLE      | s     | NULL       | ref  | idx_name      | idx_name | 63      | const |    1 |   100.00 | NULL                                       |
|  1 | SIMPLE      | c     | NULL       | ALL  | NULL          | NULL     | NULL    | NULL  |  100 |    10.00 | Using where; Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
~~~



在有索引的情况下：

~~~mysql
SELECT s.course_id, s.name, s.student_id, c.course_name
FROM student_info s JOIN course c
ON s.course_id = c.course_id
WHERE name = '462eed7ac6e791292a79'; -- 0ms
~~~





删除索引：

~~~mysql
DROP INDEX idx_name ON student_info;
~~~



查看索引：

---

![image-20220925223203750](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209252232866.png)

---







再进行查询：

~~~mysql
SELECT s.course_id, s.name, s.student_id, c.course_name
FROM student_info s JOIN course c
ON s.course_id = c.course_id
WHERE NAME = '462eed7ac6e791292a79';-- 230ms
~~~



### 7. 使用列的类型小的创建索引

我们这里所说的类型大小指的就是该类型表示的数据范围的大小我们在定义表结构的时候要显式的指定列的类型，以整数类型为例，有TINYINT、NEDIUNINT、INT、BIGINT等，他们占用地存储空间是一次递增地，能表示整数地范围也是依次递增地。

如果我们想要对某个整数列建立索引的话，在表示的整数范围允许的情况下，尽量让索引列使用较小的类型，比如我们能使用INT就不要使用BIGINT，能使用MEDIUMINT就不要使用INT。这是因为：

- 数据类型越小，在查询时进行的比较操作越快
- 数据类型越小，索引占用的存储空间就越少，在一个数据页内就可以放下更多的记录，从而减少磁盘I/0带来的性能损耗，也就意味着可以把更多的数据页缓存在内存中，从而加快读写效率



这个建议对于表的  **主键来说更加适用**，因为不仅是聚簇索引中会存储主键值，其他所有的二级索引的节点处都会存储一份记录的主键值，如果主键使用更小的数据类型，也就意味着节省更多的存储空间和更高效的IO







### 8. 使用字符串前缀创建索引

假设我们的字符串很长，那存储一个字符串就需要占用很大的存储空间。在我们需要为这个字符串列建立索引时，那就意味着在对应的B+树中有这么两个问题：

- B+树索引中的记录需要把该列的完整字符串存储起来更费时。而且字符串越长，在索引中占用的存储空间越大
- 如果B+树索引中素引列存储的字符串很长，那在**做字符串 比较时会占用更多的时间**

我们可以通过截取字段的前面一部分内容建立索引，这个就叫 **前缀索引。**这样在查找记录时虽然不能精确的定位到记录的位置，但是能定位到相应前缀所在的位置，然后根据前缀相同的记录的主键值回表查询完整的字符串值。既节约空间，又减少了字符串的比较时间，还大体能解决排序的问题



例如，TEXT和BLOG类型的字段，进行全文检索会很浪费时间，如果只检索字段前面的若干字符，这样可以提高检索速度





创建一张商户表，因为地址字段比较长，在地址字段上建立前缀索引

~~~mysql
create table shop(address varchar(120) not null);
alter table shop add index(address(12));
~~~



问题是，截取多少呢？

截取得多了，达不到节省索引存储空间的目的；截取得少了，重复内容太多，字 段的散列度(选择性)会降低。怎么计算不同的长度的选择性呢？ 

先看一下字段在全部数据中的选择度：

~~~mysql
select count(distinct address) / count(*) from shop;
~~~



通过不同长度去计算，与全表的选择性对比：

公式：

~~~mysql
count(distinct left(列名, 索引长度))/count(*)
~~~

例如：

~~~mysql
select count(distinct left(address,10)) / count(*) as sub10, -- 截取前10个字符的选择度
count(distinct left(address,15)) / count(*) as sub11, -- 截取前15个字符的选择度
count(distinct left(address,20)) / count(*) as sub12, -- 截取前20个字符的选择度
count(distinct left(address,25)) / count(*) as sub13 -- 截取前25个字符的选择度
from shop;
~~~





引申另一个问题：索引列前缀对排序的影响 

如果使用了索引列前缀，比方说前边只把address列的前12个字符放到了二级索引中，下边这个查询可能就有点儿尴尬了：

~~~mysql
SELECT * FROM shop
ORDER BY address
LIMIT 12
~~~

因为二级索引中不包含完整的address列信息，所以无法对前12个字符相同，后边的字符不同的记录进行排序，也就是使用索引列前缀的方式**无法支持使用索引排序**，只能使用文件排序



拓展：Alibaba《Java开发手册》 

【 强制 】**在 varchar 字段上建立索引时，必须指定索引长度**，没必要对全字段建立索引，根据实际文本 区分度决定索引长度。

 说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会 高达 90% 以上 ，

可以使用	` count(distinct left(列名, 索引长度))/count(*)	`  的区分度来确定。







### 9. 区分度高(散列性高)的列适合作为索引

列的基数指的是某一列中不重复数据的个数，比方说某个列包含值2，5，8，2，5，8，2，5，8，虽然有9条记录，但该列的基数却是3。【2，5，8 】

也就是说，在记录行数一定的情况下，列的基数越大，该列中的值越分散：列的基数越小，该列中的值越集中。

这个列的基数指标非常重要，直接影响我们是否能有效的利用素引。最好为列的基数大的列建立索引，为基数太小列的建立索引效果可能不好
可以使用公式	` select count（distinct a）/count（*）from t1`	计算区分度，越接近1越好，一般超过33%就算是比较高效的索引了

拓展：联合索引把区分度高微散列性高的列放在前面，





### 10. 使用最频繁的列放到联合索引的左侧 

这样也可以较少的建立一些索引。同时，由于"最左前缀原则"，可以增加联合索引的使用率。



### 11. 在多个字段都要创建索引的情况下，联合索引优于单值索引



## 3.3 限制索引的数目

在实际工作中，我们也需要注意平衡，索引的数目不是越多越好。

我们需要眼制每张表上的索引数量，建议单张表索引数量	**不超过6个**。

原因：

- 每个索引都需要	**占用磁盘空间**，索引越多，需要的磁盘空间就越大。
- 索引会**影响 INSERT、DELETE、UPDATE等语句的性能**，因为表中的数据更改的同时，索引也会进行调整和更新，会造成负担

- 优化器在选择如何优化查询时，会根据统一信息，对每一个可以用到的索引来进行评估，以生成出一个最好的执行计划，如果同时有很多个索引都可以用于查询，会增加MySQL优化器生成执行计划时间，降低查询性能

~~~
使用EXPLAIN 可以查看该条SQL语句的执行计划
它里面有一个字段`possible_keys`：代表可能会用到的索引，优化器会根据成本分析，选择一个合适的索引。
索引要是多的话，优化器经行成本分析时，所需的时间也多。
~~~



## 3.4 哪些情况不适合创建索引



### 1. 在where中使用不到的字段，不要设置索引

​		WHERE条件（包括GROUPBY、ORDERBY）里用不到的字段不需要创建索引，索引的价值是快速定位，如果起不到宁位的字段诵常是不需要创建索引的。

举个例子：

~~~mysql
SELECT course_id, student_id, create_time
FROM student_info
WHERE studentid = 41251
~~~

因为我们是按照 `student_id `来进行检索的，所以不需要对其他字段创建索引，即使这些字段出现在SELECT字段



### 2. 数据量小的表最好不要使用索引

如果表记录太少，比如少于1000个，那么是不需要创建索引的。表记录太少，是否创建索引对查询效率的影响并不大。、

甚至说，查询花费的时间可能比遍历索引的时间还要短，索引可能不会产生优化效果。



举例：创建表1：

~~~mysql
CREATE TABLE t_without_index(
    a INT PRIMARY KEY AUTO_INCREMENT,
    b INT
);
~~~



提供存储过程1：

~~~mysql
#创建存储过程
DELIMITER //
CREATE PROCEDURE t_wout_insert()
    BEGIN
        DECLARE i INT DEFAULT 1;
        WHILE i <= 900
        DO
        INSERT INTO t_without_index(b) SELECT RAND()*10000;
        SET i = i + 1;
        END WHILE;
        COMMIT;
    END //
DELIMITER ;

#调用
CALL t_wout_insert();
~~~





创建表2：

~~~mysql
CREATE TABLE t_with_index(
    a INT PRIMARY KEY AUTO_INCREMENT,
    b INT,
    INDEX idx_b(b)
);
~~~



创建存储过程2：

~~~mysql
#创建存储过程
DELIMITER //
CREATE PROCEDURE t_with_insert()
    BEGIN
        DECLARE i INT DEFAULT 1;
        WHILE i <= 900
        DO
        INSERT INTO t_with_index(b) SELECT RAND()*10000;
        SET i = i + 1;
        END WHILE;
        COMMIT;
    END //
DELIMITER ;

#调用
CALL t_with_insert();
~~~



查询对比：

~~~mysql
select * from t_without_index where b = 9879;
+------+------+
| a    | b    |
+------+------+
| 1242 | 9879 |
+------+------+
1 row in set (0.00 sec)


select * from t_with_index where b = 9879;
+-----+------+
| a | b |
+-----+------+
| 112 | 9879 |
+-----+------+
1 row in set (0.00 sec)
~~~

你能看到运行结果相同，但是在数据量不大的情况下，索引就发挥不出作用了。

> 结论：在数据表中的数据行数比较少的情况下，比如不到 1000 行，是不需要创建索引的。







### 3. 有大量重复数据的列上不要建立索引 

举例1：要在 100 万行数据中查找其中的 50 万行（比如性别为男的数据），一旦创建了索引，你需要先 访问 50 万次索引，然后再访问 50 万次数据表，这样加起来的开销比不使用索引可能还要大。 举例2：假设有一个学生表，学生总数为 100 万人，男性只有 10 个人，也就是占总人口的 10 万分之 1。

 学生表 student_gender 结构如下, 其中数据表中的 student_gender 字段取值为 0 或 1，0 代表女性，1 代 表男性。

~~~mysql
CREATE TABLE student_gender(
    student_id INT(11) NOT NULL,
    student_name VARCHAR(50) NOT NULL,
    student_gender TINYINT(1) NOT NULL,
    PRIMARY KEY(student_id)
)ENGINE = INNODB;
~~~



如果我们要筛选出这个学生表中的男性，可以使用：

~~~mysql
SELECT * FROM student_gender WHERE student_gender = 1;
~~~



运行结果（10 条数据，运行时间 0.696s ）：

----

![image-20220926223816938](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209262238196.png)

---

>  结论：当数据重复度大，比如 高于 10% 的时候，也不需要对这个字段使用索引。





### 4. 避免对经常更新的表创建过多的索引

第一层含义：频繁更新的字段不一定要创建索引。因为更新数据的时候，也需要更新索引，如果索引太多，在更新索引的时候也会造成负担，从而影响效率.
第二层含义：避免对经常更新的表创建过多的索引，并且索引中的列尽可能少。此时，虽然提高了查询速度，同时知会降低更新表的速度





### 5. 不建议用无序的值作为索引

例如身份证、UUID（在索引比较时需要转为ASCII，并且插入时可能造成页分裂）、MD5、HASH、无序长字符串等







### 6. 删除不再使用或者很少使用的索引

表中的数据被大量更新，或者数据的使用方式被改变后，原有的一些索引可能不再需要。数据库管理员应当定期找出这些索引，将它们删除，从而减少索引对更新操作的影响，





### 7. 不要定义冗余或重复的索引

① 冗余索引 

举例：建表语句如下

~~~mysql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number),
    KEY idx_name (name(10))
);
~~~

我们知道，通过 `idx_name_birthday_phone_number `索引就可以对 name 列进行快速搜索，再创建一 个专门针对 name 列的索引就算是一个 冗余索引 ，维护这个索引只会增加维护的成本，并不会对搜索有 什么好处。



② 重复索引 

另一种情况，我们可能会对某个列 重复建立索引 ，比方说这样：

~~~mysql
CREATE TABLE repeat_index_demo (
    col1 INT PRIMARY KEY,
    col2 INT,
    UNIQUE uk_idx_c1 (col1),
    INDEX idx_c1 (col1)
);
~~~

我们看到，col1 既是主键、又给它定义为一个唯一索引，还给它定义了一个普通索引，可是主键本身就 会生成聚簇索引，所以定义的唯一索引和普通索引是重复的，这种情况要避免。







## 3.5小结
索引是一把双刃剑，可提高查询效率，但也会降低插入和更新的速度并占用磁盘空间。
选择索引的最终目的是为了使查询的速度变快，上面给出的原则是最基本的准则，但不能拘泥于上面的准则，大家要在以后的学习和工作中进行不断的实践，根据应用的实际情况行分析和判断，选择最合话的索引方式。





















































































































































































































