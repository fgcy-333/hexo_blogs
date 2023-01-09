---
title: 第10章_索引优化与查询优化
date: 2023-01-03 22:27:02
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

都有哪些维度可以进行数据库调优？简言之：

- 索引失效、没有充分利用到索引--索引建立
- 关联查询太多JOIN(设计缺陷或不得已的需求)——SQL优化、反范式设计
- 服务器调优及各个参数设置(缓冲、线程数等)--调整my.cnf
- 数据过多——分库分表



关于数据库调优的知识点非常分散。不同的DBMS,不同的公司,不同的职位,不同的项目遇到的问题都不尽相同。这里我们分为三个章节进行细致讲解。

虽然SQL查询优化的技术有很多，但是大方向上完全可以分成	`物理查询优化`	和	`逻辑查询优化`	两大块。

- 物理查询优化是通过索引和表连接方式等技术来进行优化,这里重点需要掌握索引的使用。
- 逻辑查询优化就是通过SQL等价变换提升查询效率，直白一点就是说，换一种查询写法执行效率可能更高。





# 1. 数据准备 

学员表 插 50万 条， 班级表 插 1万 条。

步骤1：建表

~~~sql
CREATE DATABASE atguigudb2;
USE atguigudb2;

CREATE TABLE IF NOT EXISTS`class` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`className` VARCHAR(30) DEFAULT NULL,
`address` VARCHAR(40) DEFAULT NULL,
`monitor` INT NULL ,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;


CREATE TABLE IF NOT EXISTS `student` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`stuno` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL,
`age` INT(3) DEFAULT NULL,
`classId` INT(11) DEFAULT NULL,
PRIMARY KEY (`id`)
#CONSTRAINT `fk_class_id` FOREIGN KEY (`classId`) REFERENCES `t_class` (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
~~~

步骤2：设置参数 命令开启：允许创建函数设置：

~~~sql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
~~~



步骤3：创建函数 保证每条数据都不同。

~~~sql
#随机产生字符串
DELIMITER //
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
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

#假如要删除
#drop function rand_string;
~~~

随机产生班级编号

~~~SQL
#用于随机产生多少到多少的编号
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
    BEGIN
    DECLARE i INT DEFAULT 0;
    SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
    RETURN i;
    END //
DELIMITER ;

#假如要删除
#drop function rand_num;
~~~





步骤4：创建存储过程

~~~SQL
-- 创建往stu表中插入数据的存储过程
DELIMITER //
CREATE PROCEDURE insert_stu( START INT , max_num INT )
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0; #设置手动提交事务
        REPEAT #循环
        SET i = i + 1; #赋值
        INSERT INTO student (stuno, name ,age ,classId ) VALUES
        ((START+i),rand_string(6),rand_num(1,50),rand_num(1,1000));
        UNTIL i = max_num
        END REPEAT;
        COMMIT; #提交事务
    END //
DELIMITER ;

#假如要删除
#drop PROCEDURE insert_stu;
~~~



~~~SQL
-- 执行存储过程，往class表添加随机数据
DELIMITER //
CREATE PROCEDURE `insert_class`( max_num INT )
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0;
        REPEAT
        SET i = i + 1;
        INSERT INTO class ( classname,address,monitor ) VALUES
        (rand_string(8),rand_string(10),rand_num(1,100000));
        UNTIL i = max_num
        END REPEAT;
        COMMIT;
    END //
DELIMITER ;


#假如要删除
#drop PROCEDURE insert_class;
~~~



步骤5：调用存储过程 class

~~~SQL
#执行存储过程，往class表添加1万条数据
CALL insert_class(10000);
~~~

stu

~~~SQL
#执行存储过程，往stu表添加50万条数据
CALL insert_stu(100000,500000);
~~~



步骤6：删除某表上的索引 创建存储过程 (之后使用)

~~~SQL
DELIMITER //
CREATE PROCEDURE `proc_drop_index`(dbname VARCHAR(200),tablename VARCHAR(200))
    BEGIN
            DECLARE done INT DEFAULT 0;
            DECLARE ct INT DEFAULT 0;
            DECLARE _index VARCHAR(200) DEFAULT '';
            DECLARE _cur CURSOR FOR SELECT index_name FROM
            information_schema.STATISTICS WHERE table_schema=dbname AND table_name=tablename AND
            seq_in_index=1 AND index_name <>'PRIMARY' ;
            #每个游标必须使用不同的declare continue handler for not found set done=1来控制游标的结束
            DECLARE CONTINUE HANDLER FOR NOT FOUND set done=2 ;
            #若没有数据返回,程序继续,并将变量done设为2
            OPEN _cur;
            FETCH _cur INTO _index;
            WHILE _index<>'' DO
                SET @str = CONCAT("drop index " , _index , " on " , tablename );
                PREPARE sql_str FROM @str ;
                EXECUTE sql_str;
                DEALLOCATE PREPARE sql_str;
                SET _index='';
                FETCH _cur INTO _index;
            END WHILE;
        CLOSE _cur;
    END //
DELIMITER ;
~~~





执行存储过程

~~~SQL
CALL proc_drop_index("dbname","tablename");
~~~









# 2. 索引失效案例

MySQL中 提高性能 的一个最有效的方式是对数据表设计合理的索引。索引提供了高效访问数据的方法,并且加快查询的速度，因此索引对查询的速度有着至关重要的影响。

- 使用索引可以快速地定位表中的某条记录，从而提高数据库查询的速度，提高数据库的性能。

- 如果查询时没有使用索引,查询语句就会扫描表中的所有记录。在数据量大的情况下，这样查询的速度会很慢。



大多数情况下都(默认)采用B+树 来构建索引。只是空间列类型的索引使用R-树,并且MEMORY表还支持hash索引。



其实,用不用索引,最终都是优化器说了算。优化器是基于什么的优化器?基于cost开销(CostBaseOptimizer),它不是基于 规则(Rule-BasedOptimizer),也不是基于 语义。怎么样开销小就怎么来。 另外，SQL语句是否使用索引，跟数据库版本、数据量、数据选择度都有关系。









## 2.1 全值匹配我最爱

系统中经常出现的sq语句如下:

~~~sql
 EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30;
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |    10.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)



 EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 and classId=4;
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |     1.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)


 EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 and classId=4 AND name='abcd';
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |     0.10 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~





建立索引前执行: (关注执行时间)

~~~sql
SELECT SQL_NO_CACHE * FROM student WHERE age=30 and classId=4 AND name='abcd';
Empty set, 1 warning (0.12 sec)
~~~





建立索引

~~~sql
-- 情况一：
CREATE INDEX idx_age ON student(age);

EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME='abcd';
+----+-------------+---------+------------+------+---------------+---------+---------+-------+-------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key     | key_len | ref   | rows  | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+---------+---------+-------+-------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age       | idx_age | 5       | const | 10097 |     1.00 | Using where |
+----+-------------+---------+------------+------+---------------+---------+---------+-------+-------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)

SELECT SQL_NO_CACHE * FROM student WHERE age=30 and classId=4 AND name='abcd';
Empty set, 1 warning (0.04 sec)




-- 情况二：
CREATE INDEX idx_age_classid ON student(age, classId);


EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME='abcd';
+----+-------------+---------+------------+------+-------------------------+-----------------+---------+-------------+------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys           | key             | key_len | ref         | rows | filtered | Extra       |
+----+-------------+---------+------------+------+-------------------------+-----------------+---------+-------------+------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age,idx_age_classid | idx_age_classid | 10      | const,const |    6 |    10.00 | Using where |
+----+-------------+---------+------------+------+-------------------------+-----------------+---------+-------------+------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)

SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME='abcd';
Empty set, 1 warning (0.00 sec)




-- 情况三：
CREATE INDEX idx_age_classid_name ON student(age,classId,name);


EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME='abcd';
+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
| id | select_type | table   | partitions | type | possible_keys                                | key                  | key_len | ref               | rows | filtered | Extra |
+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age,idx_age_classid,idx_age_classid_name | idx_age_classid_name | 73      | const,const,const |    1 |   100.00 | NULL  |
+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
1 row in set, 2 warnings (0.00 sec)


SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND classId=4 AND NAME='abcd';
Empty set, 1 warning (0.00 sec)
~~~







## 2.2 最佳左前缀法则

在MySQL建立联合索引时会遵守最佳左前缀匹配原则,即最左优先,在检索数据时从联合索引的最左边开始匹配。



---

![image-20230109221746108](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202301092217773.png)

----





举例1:符合最左前缀原则

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 AND name='abcd';

+----+-------------+---------+------------+------+----------------------------------------------+---------+---------+-------+-------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys                                | key     | key_len | ref   | rows  | filtered | Extra       |
+----+-------------+---------+------------+------+----------------------------------------------+---------+---------+-------+-------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age,idx_age_classid,idx_age_classid_name | idx_age | 5       | const | 10097 |    10.00 | Using where |
+----+-------------+---------+------------+------+----------------------------------------------+---------+---------+-------+-------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~



举例2:不符合最左前缀原则

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE classid=1 AND name='abcd';

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |     1.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~



举例3:

索引idx_age_classid_name能正常使用 符合最左前缀原则

联合索引的第一个索引，有被过滤条件中的字段使用到

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE classid=4 AND student.age=30 AND student.name = 'abcd';

+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
| id | select_type | table   | partitions | type | possible_keys                                | key                  | key_len | ref               | rows | filtered | Extra |
+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age,idx_age_classid,idx_age_classid_name | idx_age_classid_name | 73      | const,const,const |    1 |   100.00 | NULL  |
+----+-------------+---------+------------+------+----------------------------------------------+----------------------+---------+-------------------+------+----------+-------+
1 row in set, 2 warnings (0.00 sec)
~~~





查看`student`表所有索引：

~~~sql
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table   | Non_unique | Key_name             | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student |          0 | PRIMARY              |            1 | id          | A         |      499086 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age              |            1 | age         | A         |          48 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid      |            1 | age         | A         |          48 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid      |            2 | classId     | A         |       50217 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            1 | age         | A         |          48 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            2 | classId     | A         |       49690 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            3 | name        | A         |      499086 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
7 rows in set (0.00 sec)
~~~



删除索引`idx_age` 、`idx_age_classid`:

~~~sql
DROP INDEX idx_age ON student;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0


DROP INDEX idx_age_classid ON student;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



查看`student`表所有索引：

~~~sql
show index from student;
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table   | Non_unique | Key_name             | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student |          0 | PRIMARY              |            1 | id          | A         |      499086 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            1 | age         | A         |          48 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            2 | classId     | A         |       49690 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            3 | name        | A         |      499086 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
4 rows in set (0.00 sec)
~~~



例子：部分使用

虽然可以正常使用,但是只有部分被使用到了。

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=4 AND student.name = 'abcd';
+----+-------------+---------+------------+------+----------------------+----------------------+---------+-------+-------+----------+-----------------------+
| id | select_type | table   | partitions | type | possible_keys        | key                  | key_len | ref   | rows  | filtered | Extra                 |
+----+-------------+---------+------------+------+----------------------+----------------------+---------+-------+-------+----------+-----------------------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age_classid_name | idx_age_classid_name | 5       | const | 19022 |    10.00 | Using index condition |
+----+-------------+---------+------------+------+----------------------+----------------------+---------+-------+-------+----------+-----------------------+
1 row in set, 2 warnings (0.00 sec)
key_len=5 int=4、null=1
~~~







例子：完全没有使用

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE classid=4 AND student.name = 'abcd';

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |     1.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~

> 结论: MySQL可以为多个字段创建索引,一个索引可以包括16个字段。对于多列索引,过滤条件要使用索引必须按照索引建立时的顺序,依次满足,一旦跳过某个字段,索引后面的字段都无法被使用。如果查询条件中没有使用这些字段中第1个字段时,多列(或联合)索引不会被使用。



拓展：Alibaba《Java开发手册》 索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索引。







## 2.3 主键插入顺序

对于一个使用 `InnoDB` 存储引擎的表来说,在我们没有显式的创建索引时,表中的数据实际上都是存储在聚簇索引的叶子节点的。而记录又是存储在数据页中的,数据页和记录又是按照记录 `主键值从小到大` 的顺序进行排序,所以如果我们插入的记录的主键值是依次增大的话,那我们每插满一个数据页就换到下一个数据页继续插,而如果我们插入的主键值忽大忽小的话,就比较麻烦了

假设某个数据页存储的记录已经满了,它存储的主键值在1~100 之间:

---

![image-20230109223723514](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202301092237893.png)

---



如果此时再插入一条主键值为 9 的记录，那它插入的位置就如下图：

---

![image-20230109223742970](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202301092237602.png)

----

可这个数据页已经满了，再插进来咋办呢？我们需要把当前 页面分裂 成两个页面，把本页中的一些记录 移动到新创建的这个页中。页面分裂和记录移位意味着什么？意味着： 性能损耗 ！所以如果我们想尽量 避免这样无谓的性能损耗，最好让插入的记录的 主键值依次递增 ，这样就不会发生这样的性能损耗了。 所以我们建议：让主键具有 AUTO_INCREMENT ，让存储引擎自己为表生成主键，而不是我们手动插入 



比如： person_info 表：

~~~sql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number)
);
~~~

我们自定义的主键列 id 拥有 AUTO_INCREMENT 属性，在插入记录时存储引擎会自动为我们填入自增的 主键值。这样的主键占用空间小，顺序写入，减少页分裂。







## 2.4 计算、函数、类型转换(自动或手动)导致索引失效



创建索引：

~~~sql
CREATE INDEX idx_name ON student(name);

Query OK, 0 rows affected (0.84 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~





查看student表中的索引：

~~~sql
show index from student;

+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table   | Non_unique | Key_name             | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student |          0 | PRIMARY              |            1 | id          | A         |      499086 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            1 | age         | A         |          48 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            2 | classId     | A         |       49690 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_age_classid_name |            3 | name        | A         |      499086 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| student |          1 | idx_name             |            1 | name        | A         |      343423 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
5 rows in set (0.00 sec)
~~~



例子1：

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%';

+----+-------------+---------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table   | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+---------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | student | NULL       | range | idx_name      | idx_name | 63      | NULL |   15 |   100.00 | Using index condition |
+----+-------------+---------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 2 warnings (0.00 sec)


key_len=63 name 20个字符 null=1 一个字符3个字节 可变两个字节 20*3+1+2
~~~

> 索引下推





~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc';
 
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |   100.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~







~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name like '%abc';

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |    11.11 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~





例子2：

创建索引：

~~~sql
CREATE INDEX idx_sno ON student(stuno);
Query OK, 0 rows affected (0.52 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



~~~sql
EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001;

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |   100.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~



~~~sql
EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno = 90000;

+----+-------------+---------+------------+------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table   | partitions | type | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+---------+------------+------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_sno       | idx_sno | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+---------+------------+------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 2 warnings (0.00 sec)
~~~

你能看到如果对索引进行了表达式计算,索引就失效了。这是因为我们需要把索引字段的取值都取出来,然后依次进行表达式的计算来进行条件判断,因此采用的就是全表扫描的方式,运行时间也会慢很多







举例三：

~~~sql
EXPLAIN SELECT id, stuno, name FROM student WHERE SUBSTRING(name, 1,3)= 'abc';
 
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |   100.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~





## 2.5 类型转换导致索引失效

下列哪个sql语句可以用到索引。（假设name字段上设置有索引）





~~~sql
--  未使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name=123; -- 隐式转换 将字段 字符串转为数字

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | idx_name      | NULL | NULL    | NULL | 499086 |    10.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 5 warnings (0.00 sec)
~~~





~~~sql
# 使用到索引
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name='123';

+----+-------------+---------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table   | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+---------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_name      | idx_name | 63      | const |    1 |   100.00 | NULL  |
+----+-------------+---------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 2 warnings (0.00 sec)
~~~

> 结论:设计实体类属性时,一定要与数据库字段类型相对应。否则,就会出现类型转换的情况。







## 2.6 范围条件右边的列索引失效

1、 如果系统经常出现的sql如下:

~~~sql
-- ALTER TABLE student DROP INDEX idx_name;
-- ALTER TABLE student DROP INDEX idx_age;
-- ALTER TABLE student DROP INDEX idx_age_classid;

CALL proc_drop_index ('atguigudb2', 'student');
Query OK, 0 rows affected (0.03 sec)
~~~



~~~sql
show index from student;

+---------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table   | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+---------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| student |          0 | PRIMARY  |            1 | id          | A         |      499086 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
+---------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
1 row in set (0.00 sec)
~~~







创建索引：

~~~sql
CREATE INDEX idx_age_classid_name oN student (age, classId, NAME);

Query OK, 0 rows affected (0.83 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student
WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ;

+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows  | filtered | Extra                            |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
|  1 | SIMPLE      | student | NULL       | range | idx_age_classid_name | idx_age_classid_name | 10      | NULL | 18254 |    10.00 | Using index condition; Using MRR |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
1 row in set, 2 warnings (0.00 sec)

key_len =10 age=4+null classid=4+null
~~~

索引 `idx_age_classid_name` 这个索引不能正常使用，范围右边的列不能使用。比如:(<) (<=) (>) (>=)和between等是范围



将范围查询条件放置语句最后:

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.name = 'abc' AND student.classId>20 ;

+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows  | filtered | Extra                            |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
|  1 | SIMPLE      | student | NULL       | range | idx_age_classid_name | idx_age_classid_name | 10      | NULL | 18254 |    10.00 | Using index condition; Using MRR |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+-------+----------+----------------------------------+
1 row in set, 2 warnings (0.00 sec)
-- 结果没有区别 与这个有关的是所以字段的顺序
~~~



解决：

~~~sql
CREATE INDEX idx_age_name_classid ON student(age,name,classid);

Query OK, 0 rows affected (0.92 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ;

+----+-------------+---------+------------+-------+-------------------------------------------+----------------------+---------+------+------+----------+-----------------------+
| id | select_type | table   | partitions | type  | possible_keys                             | key                  | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+---------+------------+-------+-------------------------------------------+----------------------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | student | NULL       | range | idx_age_classid_name,idx_age_name_classid | idx_age_name_classid | 73      | NULL |    1 |   100.00 | Using index condition |
+----+-------------+---------+------------+-------+-------------------------------------------+----------------------+---------+------+------+----------+-----------------------+
1 row in set, 2 warnings (0.00 sec)
~~~



> 应用开发中范围查询,例如:金额查询,日期查询往往都是范围查询。应将查询条件放置where语句最后。(创建的联合索引中,务必把范围涉及到的字段写在最后)







## 2.7 不等于(!= 或者<>)索引失效 

~~~sql
create index idx_name on student(name);

Query OK, 0 rows affected (0.86 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~





~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name<>'aaa';

+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 499086 |    90.00 | Using where |
+----+-------------+---------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~





## 2.8 is null可以使用索引，is not null无法使用索引



例子1：

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NULL;
 
+----+-------------+---------+------------+------+-------------------------------------------+----------------------+---------+-------+------+----------+-----------------------+
| id | select_type | table   | partitions | type | possible_keys                             | key                  | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+---------+------------+------+-------------------------------------------+----------------------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | student | NULL       | ref  | idx_age_classid_name,idx_age_name_classid | idx_age_classid_name | 5       | const |    1 |   100.00 | Using index condition |
+----+-------------+---------+------------+------+-------------------------------------------+----------------------+---------+-------+------+----------+-----------------------+
1 row in set, 2 warnings (0.00 sec)
~~~



例子2：

~~~sql
EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age IS NOT NULL;
 
+----+-------------+---------+------------+------+-------------------------------------------+------+---------+------+--------+----------+-------------+
| id | select_type | table   | partitions | type | possible_keys                             | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+---------+------------+------+-------------------------------------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | student | NULL       | ALL  | idx_age_classid_name,idx_age_name_classid | NULL | NULL    | NULL | 499086 |    50.00 | Using where |
+----+-------------+---------+------------+------+-------------------------------------------+------+---------+------+--------+----------+-------------+
1 row in set, 2 warnings (0.00 sec)
~~~



> 结论:最好在设计数据表的时候就将字段设置为NOT NULL 约束,比如你可以将INT类型的字段,默认值设置为0。将字符类型的默认值设置为空字符串(")。
>
> 拓展:同理,在查询中使用not like 也无法使用索引,导致全表扫描。









































































