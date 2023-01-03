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
CREATE INDEX idx_age ON student(age);

CREATE INDEX idx_age_classid ON student(age, classId);

CREATE INDEX idx_age_classid_name ON student(age,classId,name);
~~~















































































































































































































