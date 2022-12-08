---
title: 第09章_性能分析工具的使用
date: 2022-09-26 22:50:59
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---



在数据库调优中，我们的目标就是响应时间更快，吞吐量更大、利用  **宏观的监控工且 ** 和 **微观的日志分析**  可以帮我们快速找到调优的思路和方式

# 1. 数据库服务器的优化步骤

当我们遇到数据库调优问题的时候，该如何思考呢？这里把思考的流程整理成下面这张图。

 整个流程划分成了 观察（Show status） 和 行动（Action） 两个部分。字母 S 的部分代表观察（会使 用相应的分析工具），字母 A 代表的部分是行动（对应分析可以采取的行动）。



<!--more-->

---

![image-20220926225919083](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209262304432.png)

![image-20220926225823285](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209262304921.png)

![image-20220926230451341](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209262304836.png)

----

我们可以通过观察了解数据库整体的运行状态，通过性能分析工具可以让我们了解执行慢的SOL都有哪些，查看具体的SOL执行计划，甚至是SOL执行中的每一步的成本代价，这样才能定位问题所在，找到了问题，再采取相应的行动.





详细解释一下这张图
首先在S1部分，我们需要观察服务器的状态是否存在周期性的波动。如果存在周期性波动，有可能是周期性节点的原因，比如双十一、促销活动等。这样的话，我们可以通过A1这一步骤解决，也就是加缓存，或者更改缓存失效策略



如果缓存策略没有解决，或者不是周期性波动的原因，我们就需要进一步分析查询延迟和卡顿的原因。接下来进入S2这一步，我们需要开启慢查询。

慢查询可以帮我们定位执行慢的SOL语句。我们可以通讨设置`Long_query_tIme `参数定义"慢”的阈值，如果SQL执行时间超过了 `long_query_time`，则会认为是慢查询。当收集上来这些慢查询之后，我们就可以通过分析工具对慢查询日志进行分析



在S3这一步骤中，我们就知道了执行慢的SOL，这样就可以针对性地用`EXPLAIN `查看对应SOL语句的执行计划，或者使用 **show profile**查看 SQL中每一个步骤的时间成本。这样我们就可以了解SQL查询慢是因为执行时间长，还是等待时间长。



如果是SOL等待时间长，我们进入A2步骤。在这一步骤中，我们可以调优服务器的参数，比如活当增加数据库缓冲池等。如果是SOL执行时间长，就进入A3 步骤，这一步中我们需要考虑是索引设计的问题？还是查询关联的数据表过多？还是因为数据表的字段设计问题导致了这一现象。然后在这些维度上讲行对应的调整



如果A2和A3都不能解决问题，我们需要考虑数据库自身的 SOL查询性能是否已经达到了瓶颈，如果确认没有达到性能瓶颈，就需要重新检查，重复以上的步骤。如果已经达到了性能瓶颈，进入A4阶段，需要考虑增加服务器，采用读写分离的架构，或者考虑对数据库进行分库分表，比如垂直分库、垂直分表和水平分表等。

6

以上就是数据库调优的流程思路。如果我们发现执行SOL时存在不规则延迟或卡顿的时候，就可以采用分析工具帮我们定位有问题的SQL，这三种分析工具你可以理解是SOL调优的三个步骤：**慢查询**、**EXPLAIN** 和 **SHOW PROFILING**



小结：

---

![image-20220926232655526](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209262326721.png)

---





# 2. 查看系统性能参数

在MySQL中，可以使用 SHOW STATUS 语句查询一些MySQL数据库服务器的 性能参数 、 执行频率 。

SHOW STATUS语句语法如下：

~~~mysql
SHOW [GLOBAL|SESSION] STATUS LIKE '参数';
~~~

一些常用的性能参数如下：

- Connections：连接MySQL服务器的次数。

---

![image-20220927225231660](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272252693.png)

---



- Uptime：MySQL服务器的上 线时间。

---

![image-20220927225338548](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272253183.png)

---

~~~shell
# 重启mysql服务
systemctl restart mysqld; 
~~~

---

![image-20220927225702219](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272257574.png)

---



- Slow_queries：慢查询的次数。 

---

![image-20220927225733586](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272257484.png)

---

> 查询超过10s以上的就是慢查询 
>
> 默认是是10s

慢查询次数参数可以结合慢查询日志找出慢查询语句，然后针对慢查询语句进行表结构优化或者查询语句优化再比如，如下的指令可以查看相关的指令情况:

---

![image-20220927230232098](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272302256.png)

---



进行两次查询

---

![image-20220927230330273](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272303182.png)

----



---

![image-20220927230404993](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272304314.png)

---





- Innodb_rows_read：Select查询返回的行数 

- Innodb_rows_inserted：执行INSERT操作插入的行数 

- Innodb_rows_updated：执行UPDATE操作更新的 行数 

- Innodb_rows_deleted：执行DELETE操作删除的行数 





- Com_select：查询操作的次数。

- Com_insert：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。 

- Com_update：更新操作 的次数。

- Com_delete：删除操作的次数。





# 3. 统计SQL的查询成本：last_query_cost

一条SOL查询语句在执行前需要确定查询执行计划，如果存在多种执行计划的话，MySOL会计算每个执行计划所需要的成本，从中选择成本最小的一个作为最终执行的执行计划。
如果我们想要查看某条 SOL语句的 **查询成本** ，可以在执行完这条SOL语句之后，通过查看当前会话中的 `last-query-cost `变量值来得到当前查询的成本。

它通常也是我们评价一个查询的执行效率的一个常用指标。

这个查询成本对应的是   **SOL语句**  所需要  **读取的页的数量**



我们依然使用第8章的 student_info 表为例：

~~~mysql
 show create table student_info;
+--------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table        | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| student_info | CREATE TABLE `student_info` (
  `id` int NOT NULL AUTO_INCREMENT,
  `student_id` int NOT NULL,
  `name` varchar(20) DEFAULT NULL,
  `course_id` int NOT NULL,
  `class_id` int DEFAULT NULL,
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_sid` (`student_id`),
  KEY `idx_cre_time` (`create_time`),
  KEY `idx_sid_cre_time` (`student_id`,`create_time` DESC)
) ENGINE=InnoDB AUTO_INCREMENT=1000001 DEFAULT CHARSET=utf8mb3 |
+--------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
~~~



如果我们想要查询 id=900001 的记录，然后看下查询成本，我们可以直接在聚簇索引上进行查找：

~~~mysql
SELECT student_id, class_id, NAME, create_time FROM student_info
WHERE id = 900001;

+------------+----------+--------+---------------------+
| student_id | class_id | NAME   | create_time         |
+------------+----------+--------+---------------------+
|      68890 |    10148 | AyTXiX | 2022-09-25 18:16:39 |
+------------+----------+--------+---------------------+
1 row in set (0.01 sec)
~~~



然后再看下查询优化器的查询成本，实际上我们只需要检索一个页即可：

~~~mysql
 show status like 'last_query_cost';
 
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| Last_query_cost | 1.000000 | -- 使用一个数据页
+-----------------+----------+
1 row in set (0.01 sec)
~~~





如果我们想要查询 id 在 900001 到 9000100 之间的学生记录呢？

~~~mysql
SELECT student_id, class_id, NAME, create_time FROM student_info
WHERE id BETWEEN 900001 AND 900100;

100 rows in set (0.00 sec)
~~~





然后再看下查询优化器的查询成本，实际上我们只需要检索21个页即可：

~~~mysql
show status like 'last_query_cost';
 
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| Last_query_cost | 21.135463 |
+-----------------+-----------+
1 row in set (0.00 sec)
~~~





>  select * from student_info where student_id > 199900;

~~~mysql
66 rows in set (0.01 sec)

mysql> show status like 'last_query_cost';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| Last_query_cost | 76.158136 |
+-----------------+-----------+
~~~





>  select * from student_info where student_id>199000;

~~~mysql
4922 rows in set (0.12 sec)

mysql> show status like 'last_query_cost';
+-----------------+-------------+
| Variable_name   | Value       |
+-----------------+-------------+
| Last_query_cost | 4756.799866 |
+-----------------+-------------+
~~~



你能看到页的数量是刚才的62倍，但是查询的效率相差12倍，实际上这两个SOL查询的时间基本上一样，就是因为采用了顺序读取的方式将页面一次性加载到缓冲池中，然后再进行查找。虽然页数量（astquery_cost）增加了不少，但是通过缓冲池的机制，并没有增加多少查询时间

使用场量：它对千比较开销是非常有用的，特别是我们有好几种查询方式可选的时候



> SOL查询是一个动态的过程，从页加载的角度来看，我们可以得到以下两点结论：
>
> 1、位置决定效率。如果页就在数据库缓神池中，那么效率是最高的，否则还需要从内存或者磁盘中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多
>
> 2、批量决定效率。如果我们从磁盘中对单一页进行随机读，那么效率是很低的（差不多10ms），而采用顺序读取的方式，批量对页进行读取，平均一页的读取效率就会提升很多，甚至要快于单个页面在内存中的随机读取
>
> 所以说，遇到I/0并不用担心，方法找对了，效率还是很高的。我们首先要考虑数据存放的位置，如果是经常使用的数据就要尽量放到 缓冲池 中，其次我们可以充分利用磁盘的吞吐能力，一次性批量读取数据，这样单个页的读取效率也就得到了提升







# 4. 定位执行慢的 SQL：慢查询日志

MySOL的慢查询日志，用来记录在MySQL中响应时间超过阀值的语句，具体指运行时间超过` long_query-tame` 值的SQL，则会被记录到慢查询日志中。long_queryLtime的默认值为10，意思是运行10秒以上（不含10秒）的语句、认为是超出了我们的最大忍耐时间值。

它的主要作用是，帮助我们发现那些执行时间特别长的SOL查询，并且有针对性地进行优化，从而提高系统的整体效率、当我们的数据库服务器发生阻塞、运行变慢的时候，检查一下慢查询日志，找到那些慢查询，对解决问题很有帮助。

比如一条sql执行超过5秒钟，我们就算慢SQL，希望能收集超过5秒的sql，结合explain进行全面分析



默认情况下，MySQL数据库没有开启慢查询日志，需要我们手动来设置这个参数。

**如果不是调优需要的话，一般不建议启动该参数**，因为开启慢查询日志会或多或少带来一定的性能影响



慢查询日志支持将日志记录写入文件



## 4.1 开启慢查询日志参数 

### 1. 开启slow_query_log

~~~mysql
set global slow_query_log='ON';
~~~



查询慢查询阈值：

-----

![image-20220927233545830](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272335285.png)

---



修改阈值：

---

![image-20220927233636610](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272336115.png)

---



这里介绍另一个参数，慢查询日志文件

-----

![image-20220927233723625](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272337290.png)

----

你能看到这时慢查询分析已经开启，同时文件保存在` /var/lib/mysql/atguigu05-slow.1og`  文件中





### 2. 修改long_query_time阈值 

接下来我们来看下慢查询的时间阈值设置，使用如下命令：

>  show variables like '%long_query_time%';

---

![image-20220927234035391](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272340327.png)

---





这里如果我们想把时间缩短，比如设置为 1 秒，可以这样设置：

~~~mysql
set global long_query_time = 1;

show global variables like '%long_query_time%';


set long_query_time=1;

show variables like '%long_query_time%';
~~~

>  测试发现：设置global的方式对当前session的long_query_time失效。对新连接的客户端有效。所以一并设置系统会话变量



---

![image-20220927234334885](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272343702.png)

---



---

![image-20220927234359581](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272344875.png)

---

> 补充：配置文件中一并设置参数
> 如下的方式相较于前面的命令行方式，可以看作是永久设置的方式
> 修改my.cnf 文件，[mysqld]下增加或修改参数long_query_time、slow_query_log 和slow.query_1og-file 后，然后重启MySQL服务器。T





~~~ini
[mysq1d]
s1ow_query_1og=ON#开启慢查询日志的开关
s1ow-query_1og_f1le= /var/1ib/mysql/atguigu-slow.log #慢查询日志的日录和文件名信息
1ong-query-tame=3 #设置慢查询的闭值为3秒，超出此设定值的SOL即被记录到慢杏询日志
1og_output=FILE
~~~



如果不指定存储路径，慢查询日志将默认存储到MvSOL数据库的数据文件夹下。如果不指定文件名，默认文件名为hostname-slow.Iog。





## 4.2 查看慢查询数目

 查询当前系统中有多少条慢查询记录

~~~mysql
SHOW GLOBAL STATUS LIKE '%Slow_queries%';
~~~

---

![image-20220927234752516](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209272347546.png)

---







## 4.3 案例演示

步骤1. 建表

~~~mysql
CREATE TABLE `student` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `stuno` INT NOT NULL ,
    `name` VARCHAR(20) DEFAULT NULL,
    `age` INT(3) DEFAULT NULL,
    `classId` INT(11) DEFAULT NULL,
    PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
~~~





步骤2：设置参数 log_bin_trust_function_creators

创建函数，假如报错：

~~~mysql
This function has none of DETERMINISTIC......
~~~



命令开启：允许创建函数设置：

~~~mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
~~~





步骤3：创建函数 

随机产生字符串：（同上一章）

~~~mysql
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


# 调用函数
SELECT rand_string(10);
~~~



产生随机数值：（同上一章）

~~~mysql
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11)
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
        RETURN i;
    END //
DELIMITER ;

#调用函数：
SELECT rand_num(10,100);
~~~



步骤4：创建存储过程

~~~mysql
DELIMITER //
CREATE PROCEDURE insert_stu1( START INT , max_num INT )
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0; #设置手动提交事务
        REPEAT #循环
        SET i = i + 1; #赋值
        INSERT INTO student (stuno, NAME ,age ,classId ) VALUES
        ((START+i),rand_string(6),rand_num(10,100),rand_num(10,1000));
        UNTIL i = max_num
        END REPEAT;
        COMMIT; #提交事务
    END //
DELIMITER ;
~~~



步骤5：调用存储过程

~~~mysql
#调用刚刚写好的函数, 4000000条记录,从100001号开始
CALL insert_stu1(100001,4000000);
~~~







## 4.4 **测试及分析**

查看条数：

~~~mysql
 SELECT COUNT(*) FROM student;
 
+----------+
| COUNT(*) |
+----------+
|  4000000 |
+----------+
1 row in set (0.76 sec)
~~~





查看某条记录：

~~~mysql
 SELECT * FROM student WHERE stuno=3453451;
 
+---------+---------+--------+------+---------+
| id      | stuno   | name   | age  | classId |
+---------+---------+--------+------+---------+
| 3353450 | 3453451 | YDSHFe |   70 |      37 |
+---------+---------+--------+------+---------+
1 row in set (0.93 sec)
~~~



~~~mysql
 SELECT * FROM student WHERE name='YDSHFe';
+---------+---------+--------+------+---------+
| id      | stuno   | name   | age  | classId |
+---------+---------+--------+------+---------+
|  297768 |  397769 | YdsHfE |   66 |     356 |
| 1445574 | 1545575 | YDSHFe |   69 |     973 |
| 2102837 | 2202838 | YdsHfE |   68 |     439 |
| 3353450 | 3453451 | YDSHFe |   70 |      37 |
+---------+---------+--------+------+---------+
4 rows in set (1.02 sec)
~~~





查看慢查询:

~~~mysql
 show status like 'slow_queries';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Slow_queries  | 1     |
+---------------+-------+
1 row in set (0.00 sec)
~~~



补充说明:

除了上述变量，控制慢查询日志的还有一个系统变量：`min_examined_row_limit`。

这个变量的意思是，查询扫描过的最少记录数。这个变量和查询执行时间，共同组成了判别一个查询是否是慢查询的条件。

如果查询扫描过的记录数大于等于这个变量的值，**并且**  查询执行时间超过long_query_time 的值，那么，这个查询就被记录到慢查询日志中；

反之，则不被记录到慢查询日志中，



`show variables like 'min%'`

~~~mysql
show variables like 'min%';

+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| min_examined_row_limit | 0     | -- 默认是零，我们可以使用默认值即可
+------------------------+-------+
1 row in set, 1 warning (0.00 sec)
~~~

这个值默认是0。与 `long_query_time` = 10 合在一起，表示只要查询的执行时间超过10秒钟，哪怕一个记录也没有扫描过，都要被记录到慢查询日志中。

你也可以根据需要，通过修改"my.ini"文件，来修改查询时长，或者通过SET指令，用SQL语句修改 `minLexamined_row_limit` 的值









## 4.5 慢查询日志分析工具：mysqldumpslow

在生产环境中，如果要手工分析日志，查找、分析SQL，显然是个体力活，MySQL提供了日志分析工具 mysqldumpslow 。



查看mysqldumpslow的帮助信息:

~~~mysql
mysqldumpslow --help;
~~~

----

![image-20220930220038868](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209302201356.png)

---

mysqldumpslow 命令的具体参数如下：

-a: 不将数字抽象成N，字符串抽象成S 

-s: 是表示按照何种方式排序： 

​	c: 访问次数 

​	l: 锁定时间 

​	r: 返回记录

​	 t: 查询时间 

​	al:平均锁定时间 

​	ar:平均返回记录数 

​	at:平均查询时间 （默认方式） 

​	ac:平均查询次数

 -t: 即为返回前面多少条的数据；



在Linux系统上，查询慢查询日志：

使用时间降序排列

---

![image-20220930222143252](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209302222968.png)

----





---

![image-20220930222353615](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209302223305.png)

----





## 4.6 关闭慢查询日志

 MySQL服务器停止慢查询日志功能有两种方法：



## 方式1：永久性方式

~~~mysql
[mysqld]
slow_query_log=OFF
~~~

或者，把slow_query_log一项注释掉 或 删除

~~~mysql
[mysqld]
#slow_query_log =OFF
~~~



重启MySQL服务，执行如下语句查询慢日志功能。

~~~~mysql
SHOW VARIABLES LIKE '%slow%'; #查询慢查询日志所在目录
SHOW VARIABLES LIKE '%long_query_time%'; #查询超时时长
~~~~



## 方式2：临时性方式 

使用SET语句来设置。

（1）停止MySQL慢查询日志功能，具体SQL语句如下。

~~~mysql
SET GLOBAL slow_query_log=off;
~~~



（2）重启MySQL服务，使用SHOW语句查询慢查询日志功能信息，具体SQL语句如下

~~~~mysql
SHOW VARIABLES LIKE '%slow%';
#以及
SHOW VARIABLES LIKE '%long_query_time%';
~~~~





使用SHOW语句显示慢查询日志信息，具体SOL语句如下





## 4.7 删除慢查询日志

~~~mysql
SHOW VARIABLES LIKE 'slow_query_1og%';
~~~

---

![image-20221001152211638](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210011522824.png)

---

从执行结果可以看出，慢查询日志的日录默认为MySQL的数据日录，在该日录下 **手动删除慢查询日志文件即可**  

使用命令mysqladmin flush_logs 来重新生成查询日志文件，具体命令如下，执行完毕会在数据目录下重新生成慢查询日志文件。



~~~~mysql
mysqladmin -uroot -p flush-1ogs s1ow
~~~~

>  提示
> 慢查询日志都是使用 `mysqladmin flush-logs `命令来删除重建的。
>
> 使用时一定要注意，一旦执行了这个命令，慢查询日志都只存在新的日志文件中，如果需要旧的查询日志，就必须事先备份







# 5. 查看 SQL 执行成本：SHOW PROFILE

Show Profile是MySOL提供的可以用来分析当前会话中SOL都做了什么、执行的资源消耗情况的工具，可用于sql调优的测量。

默认情况下处于关闭状态，并保存最近15次的运行结果



~~~mysql
show variables like 'profiling';
~~~

---

![image-20221001153112503](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210011531169.png)

---





我们可以在会话级别开启这个功能:

通过设置` profiling='ON’` 来开启 show profile：

~~~mysql
SET profiling ='on';

Query OK, 0 rows affected, 1 warning (0.00 sec)
~~~



然后执行相关的查询语句。接着看下当前会话都有哪些 profiles，使用下面这条命令：

~~~mysql
show profiles;

+---------------+----------+
| Status        | Duration |
+---------------+----------+
| starting      | 0.000039 |
| freeing items | 0.000029 |
| cleaning up   | 0.000003 |
+---------------+----------+
3 rows in set, 1 warning (0.00 sec)
~~~



你能看到当前会话一共有 2 个查询。如果我们想要查看最近一次查询的开销，可以使用：

~~~mysql
 show profile;
 
+--------------------------------+----------+
| Status                         | Duration |
+--------------------------------+----------+
| starting                       | 0.000058 |
| Executing hook on transaction  | 0.000003 |
| starting                       | 0.000005 |
| checking permissions           | 0.000004 |
| Opening tables                 | 0.000035 |
| init                           | 0.000003 |
| System lock                    | 0.000006 |
| optimizing                     | 0.000008 |
| statistics                     | 0.000014 |
| preparing                      | 0.000014 |
| executing                      | 0.988680 |
| end                            | 0.000018 |
| query end                      | 0.000003 |
| waiting for handler commit     | 0.000007 |
| closing tables                 | 0.000008 |
| freeing items                  | 0.000061 |
| cleaning up                    | 0.000009 |
+--------------------------------+----------+
17 rows in set, 1 warning (0.00 sec)
~~~





我们也可以查看指定的 Query ID 的开销，比如` show profile for query 1` 查询结果是一样的。

在SHOW PROFILE 中我们可以查看不同部分的开销，比如 **cpu、block、 io** 等



~~~mysql
 show profile cpu,block io for query 1;
 
+--------------------------------+----------+----------+------------+--------------+---------------+
| Status                         | Duration | CPU_user | CPU_system | Block_ops_in | Block_ops_out |
+--------------------------------+----------+----------+------------+--------------+---------------+
| starting                       | 0.000058 | 0.000000 |   0.000000 |         NULL |          NULL |
| Executing hook on transaction  | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| starting                       | 0.000005 | 0.000000 |   0.000000 |         NULL |          NULL |
| checking permissions           | 0.000004 | 0.000000 |   0.000000 |         NULL |          NULL |
| Opening tables                 | 0.000039 | 0.000000 |   0.000000 |         NULL |          NULL |
| init                           | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| System lock                    | 0.000006 | 0.000000 |   0.000000 |         NULL |          NULL |
| optimizing                     | 0.000008 | 0.000000 |   0.000000 |         NULL |          NULL |
| statistics                     | 0.000015 | 0.000000 |   0.000000 |         NULL |          NULL |
| preparing                      | 0.000014 | 0.000000 |   0.000000 |         NULL |          NULL |
| executing                      | 0.992499 | 0.875000 |   0.140625 |         NULL |          NULL |
| end                            | 0.000008 | 0.000000 |   0.000000 |         NULL |          NULL |
| query end                      | 0.000003 | 0.000000 |   0.000000 |         NULL |          NULL |
| waiting for handler commit     | 0.000014 | 0.000000 |   0.000000 |         NULL |          NULL |
| closing tables                 | 0.000008 | 0.000000 |   0.000000 |         NULL |          NULL |
| freeing items                  | 0.000049 | 0.000000 |   0.000000 |         NULL |          NULL |
| cleaning up                    | 0.000009 | 0.000000 |   0.000000 |         NULL |          NULL |
+--------------------------------+----------+----------+------------+--------------+---------------+
17 rows in set, 1 warning (0.00 sec)
~~~



show profile的常用查询参数： 

① ALL：显示所有的开销信息。

② BLOCK IO：显示块IO开销。 

③ CONTEXT SWITCHES：上下文切换开 销。 

④ CPU：显示CPU开销信息。 

⑤ IPC：显示发送和接收开销信息。 

⑥ MEMORY：显示内存开销信 息。 

⑦ PAGE FAULTS：显示页面错误开销信息。 

⑧ SOURCE：显示和Source_function，Source_file， Source_line相关的开销信息。 

⑨ SWAPS：显示交换次数开销信息。





日常开发需注意的结论：

- convertang HEAP to NyISAN：查询结果太大，内存不够，数据往磁盘上搬了。
- Creating tmp table：创建临时表。先拷贝数据到临时表，用完后再删除临时表
- Copying to tmp table on disk：把内存中临时表复制到磁盘上，警惕！
- locked。

如果在show profile诊断结果中出现了以上4条结果中的任何一条，则sql语句需要优化



>  注意：不过 SHOW PROFILE命令将被弃用，我们可以从 information_schema 中的profiling 数据表进行查看，







# 6. 分析查询语句：EXPLAIN

定位了查询慢的SQL之后，我们就可以使用`EXPLAIN `或 `DESCRIBE` 工具做针对性的分析查询语句。

**DESCRIBE语句**    **EXPLAIN语句**是一样的，并且分析结果也是一样的。



MvSOL中有专门负责优化SELECT语句的优化器模块，主要功能：通过计算分析系统中收集到的统计信息，为客户端请求的Query提供  **它认为**  最优的执行计划（ **他认为** 最优的数据检索方式，**但不见得是 DBA认为是最优的**，这部分最费时间
这个执行计划展示了接下来具体执行查询的方式，比如多表连接的顺序是什么，对于每个表采用什么访问方法来具体执行查询等等。

MySOL为我们提供了EXPLAIN语句来帮助我们查看某个查询语句的具体执行计划，大家看懂EXPLAIN语包的各个输出项，可以有针对件的提高我们查询语句的性能。





## 1. 能做什么？

- 表的读取顺序
- 数据读取操作的操作类型
- 哪些索引可以使用
- 哪些素引被实际使用
- 表与表之间的引用
- 每张表有多少行被优化器查询





## 2. 官网介绍 ：

https://dev.mysql.com/doc/refman/5.7/en/explain-output.html 

https://dev.mysql.com/doc/refman/8.0/en/explain-output.html

> 一个MySQL5.7 一个MySQL8.0







版本情况 

- MySQL 5.6.3以前只能 EXPLAIN SELECT ；
- MYSQL 5.6.3以后就可以 EXPLAIN SELECT，UPDATE， DELETE 
- 在5.7以前的版本中，想要显示 partitions 需要使用 explain partitions 命令；想要显示 filtered 需要使用 explain extended 命令。
- 在5.7版本后，默认explain  **直接显示**  partitions和 filtered中的信息。



~~~mysql
EXPLAIN SELECT * FROM student_info;

+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
| id | select_type | table        | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra |
+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
|  1 | SIMPLE      | student_info | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 997194 |   100.00 | NULL  |
+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
1 row in set, 1 warning (0.01 sec)
~~~





> 注意，EXPLAIN只是查看执行计划，并不会执行真实的SQL语句

~~~~mysql
 EXPLAIN DELETE FROM student_info WHERE id=1;
+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
| id | select_type | table        | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra       |
+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
|  1 | DELETE      | student_info | NULL       | range | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using where |
+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~~



使用`DESCRIBE`达到同样的效果：

~~~mysql
 DESCRIBE DELETE FROM student_info WHERE id=1;

+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
| id | select_type | table        | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra       |
+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
|  1 | DELETE      | student_info | NULL       | range | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | Using where |
+----+-------------+--------------+------------+-------+---------------+---------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~







## 6.2 基本语法 

EXPLAIN 或 DESCRIBE语句的语法形式如下：

~~~~mysql
EXPLAIN SELECT select_options
或者
DESCRIBE SELECT select_options
~~~~





如果我们想看看某个查询的执行计划的话，可以在具体的查询语句前边加一个 EXPLAIN ，就像这样：

~~~mysql
EXPLAIN SELECT 1;
~~~



EXPLAIN 语句输出的各个列的作用如下：

----

![image-20221001163129689](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210011631541.png)

----

> 在这里把它们都列出来只是为了描袜一个轮廊，让大家有一个大致的印象







## 6.3 数据准备 

#### 1、建表

~~~mysql
CREATE TABLE s1 (
        id INT AUTO_INCREMENT,
        key1 VARCHAR(100),
        key2 INT,
        key3 VARCHAR(100),
        key_part1 VARCHAR(100),
        key_part2 VARCHAR(100),
        key_part3 VARCHAR(100),
        common_field VARCHAR(100),
        PRIMARY KEY (id),
        INDEX idx_key1 (key1),
        UNIQUE INDEX idx_key2 (key2),
        INDEX idx_key3 (key3),
        INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;

Query OK, 0 rows affected, 1 warning (0.03 sec)
~~~



~~~mysql
CREATE TABLE s2 (
        id INT AUTO_INCREMENT,
        key1 VARCHAR(100),
        key2 INT,
        key3 VARCHAR(100),
        key_part1 VARCHAR(100),
        key_part2 VARCHAR(100),
        key_part3 VARCHAR(100),
        common_field VARCHAR(100),
        PRIMARY KEY (id),
        INDEX idx_key1 (key1),
        UNIQUE INDEX idx_key2 (key2),
        INDEX idx_key3 (key3),
        INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;

Query OK, 0 rows affected, 1 warning (0.03 sec)
~~~





#### 2.创建函数

~~~mysql
DELIMITER //
CREATE FUNCTION rand_string1(n INT)
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

-- This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)

~~~



创建函数，假如报错，需开启如下命令：

允许创建函数设置：

~~~mysql
set global log_bin_trust_function_creators=1; # 不加global只是当前窗口有效。
~~~







#### 3. . 创建存储过程 
创建往s1表中插入数据的存储过程：

~~~mysql
DELIMITER //
CREATE PROCEDURE insert_s1 (IN min_num INT (10),IN max_num INT (10))
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0;
        REPEAT
        SET i = i + 1;
        INSERT INTO s1 VALUES(
        (min_num + i),
        rand_string1(6),
        (min_num + 30 * i + 5),
        rand_string1(6),
        rand_string1(10),
        rand_string1(5),
        rand_string1(10),
        rand_string1(10));
        UNTIL i = max_num
        END REPEAT;
        COMMIT;
    END //
DELIMITER ;
~~~



创建往s2表中插入数据的存储过程：

~~~mysql
DELIMITER //
CREATE PROCEDURE insert_s2 (IN min_num INT (10),IN max_num INT (10))
    BEGIN
        DECLARE i INT DEFAULT 0;
        SET autocommit = 0;
        REPEAT
        SET i = i + 1;
        INSERT INTO s2 VALUES(
        (min_num + i),
        rand_string1(6),
        (min_num + 30 * i + 5),
        rand_string1(6),
        rand_string1(10),
        rand_string1(5),
        rand_string1(10),
        rand_string1(10));
        UNTIL i = max_num
        END REPEAT;
        COMMIT;
    END //
DELIMITER ;
~~~



#### 4. 调用存储过程

s1表数据的添加：加入1万条记录：

~~~mysql
CALL insert_s1(10001,10000);

-- 总耗时      : 1.627 sec
~~~

数据验证：

~~~mysql
 select count(*) from s1;
+----------+
| count(*) |
+----------+
|    10000 |
+----------+
1 row in set (0.01 sec)
~~~





s2表数据的添加：加入1万条记录：

~~~mysql
CALL insert_s2(10001,10000);

-- 总耗时      : 1.604 sec
~~~



数据验证：

~~~~mysql
 select count(*) from s2;
+----------+
| count(*) |
+----------+
|    10000 |
+----------+
1 row in set (0.01 sec)
~~~~





## 6.4 EXPLAIN各列作用 

为了让大家有比较好的体验，我们调整了下 EXPLAIN 输出列的顺序。 

### 1. table 

不论我们的查询语句有多复杂，里边儿 包含了多少个表 ，到最后也是需要对每个表进行 单表访问 的，所 以MySQL规定EXPLAIN语句输出的每条记录都对应着某个单表的访问方法，该条记录的table列代表着该 表的表名（有时不是真实的表名字，可能是简称）。



~~~mysql
#查询的每一行记录都对应着一个单表
EXPLAIN SELECT * FROM s1;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                         |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL                          |
|  1 | SIMPLE      | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
2 rows in set, 1 warning (0.00 sec)
~~~

> s1:驱动表  s2:被驱动表



### 2. id 

我们写的查询语句一般都以 SELECT 关键字开头，比较简单的查询语句里只有一个 SELECT 关键字，比 如下边这个查询语句：

在一个大的查询语句中  **每个SELECT关键字**  都对应一个 **唯一的id**



~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';

+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)

-- 一个SELECT 关键字 id仅有一个
-- 只涉及一个表 一行记录
~~~





~~~mysql
 EXPLAIN SELECT * FROM s1 INNER JOIN s2;
 
 +----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                         |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL                          |
|  1 | SIMPLE      | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
2 rows in set, 1 warning (0.00 sec)

-- 一个SELECT 关键字 id仅有一个
-- 只涉及两个表 两行记录
~~~





~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
|  1 | PRIMARY     | s1    | NULL       | ALL   | idx_key3      | NULL     | NULL    | NULL | 9895 |   100.00 | Using where |
|  2 | SUBQUERY    | s2    | NULL       | index | idx_key1      | idx_key1 | 303     | NULL | 9895 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)

-- 两个SELECT 关键字 id有两个
-- 只涉及两个表 两行记录
~~~







**查询优化器可能对涉及子查询的查询语句进行重写,转变为多表查询的操作**

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key2 FROM s2 WHERE common_field = 'a');
 
 +----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+
| id | select_type | table | partitions | type   | possible_keys | key      | key_len | ref                | rows | filtered | Extra                              |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL    | idx_key1      | NULL     | NULL    | NULL               | 9895 |   100.00 | Using where                        |
|  1 | SIMPLE      | s2    | NULL       | eq_ref | idx_key2      | idx_key2 | 5       | atguigudb1.s1.key1 |    1 |    10.00 | Using index condition; Using where |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+

2 rows in set, 2 warnings (0.00 sec)
~~~







Union去重

~~~mysql
EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;
 
+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type  | table      | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | PRIMARY      | s1         | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL            |
|  2 | UNION        | s2         | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL            |
| NULL | UNION RESULT | <union1,2> | NULL       | ALL  | NULL          | NULL | NULL    | NULL | NULL |     NULL | Using temporary |
+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
3 rows in set, 1 warning (0.00 sec)

-- 通过临时表实现去重 一共涉及三张表
-- 只有两个SELECT 关键字 id为1,2
~~~







Union All 不去重：

~~~mysql
EXPLAIN SELECT * FROM s1  UNION ALL SELECT * FROM s2;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | PRIMARY     | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
|  2 | UNION       | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
2 rows in set, 1 warning (0.00 sec)

-- 不需要实现去重 一共涉及两张表
-- 只有两个SELECT 关键字 id为1,2
~~~



>  小结:
>
> id如果相同，可以认为是一组，从上往下顺序执行 
>
> 在所有组中，id值越大，优先级越高，越先执行 
>
> 关注点：id号每个号码，表示一趟独立的查询, 一个sql的查询趟数越少越好	





### 3. select_type

一条大的查询语句里边可以包含若干个SELECT关键字，每个SELECT关键字代表着一个小的查询语句，而每个SELECT关键字的FROM子句中都可以包含若干张表（这些表用来做连接查询），每一张表都对应着执行计划输出中的一条记录，对于在同一个SELECT关键字中的表来说，它们的id值是相同的.



MySQL为每一个SELECT关键字代表的小查询都定义了一个称之为 select_type 的属性，意思是我们只要知道了某个小查询的select_type属性，就知道了这个小查

询在整个大查询中扮演了一个什么角色，我们看一下selecttype都能取哪些值，请看官方文档：

----

![image-20221001200024988](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210012000546.png)

----

`select_type` ：SELECT关键字对应的那个查询的类型,确定小查询在整个大查询中扮演了一个什么角色



查询语句中不包含`UNION`或者子查询的查询都算作是`SIMPLE`类型

~~~~mysql
EXPLAIN SELECT * FROM s1;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~~



多表连接查询也算是`SIMPLE`类型

~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2;
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                         |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL                          |
|  1 | SIMPLE      | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------------+
2 rows in set, 1 warning (0.00 sec)
~~~





对于包含`UNION`或者`UNION ALL`或 者 `子查询 `的大查询来说，它是由几个小查询组成的，其中最左边的那个查询的 `select_type` 值就是`PRIMARY`

~~~~mysql
EXPLAIN SELECT * FROM s1 UNION SELECT * FROM s2;

+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type  | table      | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | PRIMARY      | s1         | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL            |
|  2 | UNION        | s2         | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL            |
| NULL | UNION RESULT | <union1,2> | NULL       | ALL  | NULL          | NULL | NULL    | NULL | NULL |     NULL | Using temporary |
+----+--------------+------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
3 rows in set, 1 warning (0.00 sec)
~~~~



`MySQL`选择使用临时表来完成`UNION`查询的去重工作，针对该临时表的查询的 `select_type` 就是 `UNION RESULT`

~~~mysql
EXPLAIN SELECT * FROM s1 UNION ALL SELECT * FROM s2;
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | PRIMARY     | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
|  2 | UNION       | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
2 rows in set, 1 warning (0.00 sec)
~~~



​		

子查询：

如果包含子查询的查询语句  **不能  够转为对应**的`semi-join`的形式【多表连接的方式】，并且该子查询是  **不是**  **相关子查询**。该子查询的第一个`SELECT`关键字代表的那个查询的 `select_type` 就是 `SUBQUERY`

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
 
 +----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
|  1 | PRIMARY     | s1    | NULL       | ALL   | idx_key3      | NULL     | NULL    | NULL | 9895 |   100.00 | Using where |
|  2 | SUBQUERY    | s2    | NULL       | index | idx_key1      | idx_key1 | 303     | NULL | 9895 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
~~~



如果包含子查询的查询语句  **不能够转为对应**  的`semi-join`的形式，并且该子查询 是 **相关子查询**，则该子查询的第一个`SELECT`关键字代表的那个查询 `select_type`就是 `DEPENDENT SUBQUERY`

~~~mysql
EXPLAIN SELECT * FROM s1 
WHERE key1 IN (SELECT key1 FROM s2 WHERE s1.key2 = s2.key2) OR key3 = 'a';

+----+--------------------+-------+------------+--------+-------------------+----------+---------+--------------------+------+----------+-------------+
| id | select_type        | table | partitions | type   | possible_keys     | key      | key_len | ref                | rows | filtered | Extra       |
+----+--------------------+-------+------------+--------+-------------------+----------+---------+--------------------+------+----------+-------------+
|  1 | PRIMARY            | s1    | NULL       | ALL    | idx_key3          | NULL     | NULL    | NULL               | 9895 |   100.00 | Using where |
|  2 | DEPENDENT SUBQUERY | s2    | NULL       | eq_ref | idx_key2,idx_key1 | idx_key2 | 5       | atguigudb1.s1.key2 |    1 |    10.00 | Using where |
+----+--------------------+-------+------------+--------+-------------------+----------+---------+--------------------+------+----------+-------------+
2 rows in set, 2 warnings (0.00 sec)
~~~

> 注意的是，select_type为`DEPENDENT SUBQUERY`的查询 可能 会被执行多次。





在包含`UNION` 或者`UNION ALL`的大查询中，如果各个小查询都依  **赖于外层查询的话**，那除了最左边的那个小查询之外，其余的小查询的`select_type`的值就是`DEPENDENT UNION`。
~~~mysql
EXPLAIN SELECT * FROM s1 
WHERE key1 IN (SELECT key1 FROM s2 WHERE key1 = 'a' UNION SELECT key1 FROM s1 WHERE key1 = 'b');

+----+--------------------+------------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------+
| id | select_type        | table      | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra                    |
+----+--------------------+------------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------+
|  1 | PRIMARY            | s1         | NULL       | ALL  | NULL          | NULL     | NULL    | NULL  | 9895 |   100.00 | Using where              |
|  2 | DEPENDENT SUBQUERY | s2         | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | Using where; Using index |
|  3 | DEPENDENT UNION    | s1         | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | Using where; Using index |
| NULL | UNION RESULT       | <union2,3> | NULL       | ALL  | NULL          | NULL     | NULL    | NULL  | NULL |     NULL | Using temporary          |
+----+--------------------+------------+------------+------+---------------+----------+---------+-------+------+----------+--------------------------+
4 rows in set, 1 warning (0.00 sec)
~~~

> 这里使用了 DEPENDENT SUNQUERY 和 DEPENDENT UNION 说明内部的查询依赖于外部的查询。但这里没有看到相关子查询。
>
> 原因：
>
> 这里使用 IN 关键字 一般情况下优化器会将 这条SQL语句改为 使用EXISTS 关键字的相关子查询



对于包含`派生表`的查询，该 **派生表对应的子查询**的`select_type`就是`DERIVED`

~~~mysql
 EXPLAIN SELECT * 
 FROM (SELECT key1, COUNT(*) AS c FROM s1 GROUP BY key1) AS derived_s1 WHERE c > 1;
 
 +----+-------------+------------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
| id | select_type | table      | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra       |
+----+-------------+------------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
|  1 | PRIMARY     | <derived2> | NULL       | ALL   | NULL          | NULL     | NULL    | NULL | 9895 |   100.00 | NULL        |
|  2 | DERIVED     | s1         | NULL       | index | idx_key1      | idx_key1 | 303     | NULL | 9895 |   100.00 | Using index |
+----+-------------+------------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
~~~

>  <derived2> ：外层查询所依赖的表是一个派生表
>
> 根据一些表查询到的数据作为一张临时表 它的查询类型是DERIVED 





 当查询优化器在执行包含子查询的语句时，选择将  **子查询物化之后**  与外层查询进行连接查询时，该子查询对应的`select_type`属性就是 `MATERIALIZED`

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2); -- 子查询被转为了物化表 

+----+--------------+-------------+------------+--------+---------------------+---------------------+---------+--------------------+------+----------+-------------+
| id | select_type  | table       | partitions | type   | possible_keys       | key                 | key_len | ref                | rows | filtered | Extra       |
+----+--------------+-------------+------------+--------+---------------------+---------------------+---------+--------------------+------+----------+-------------+
|  1 | SIMPLE       | s1          | NULL       | ALL    | idx_key1            | NULL                | NULL    | NULL               | 9895 |   100.00 | Using where |
|  1 | SIMPLE       | <subquery2> | NULL       | eq_ref | <auto_distinct_key> | <auto_distinct_key> | 303     | atguigudb1.s1.key1 |    1 |   100.00 | NULL        |
|  2 | MATERIALIZED | s2          | NULL       | index  | idx_key1            | idx_key1            | 303     | NULL               | 9895 |   100.00 | Using index |
+----+--------------+-------------+------------+--------+---------------------+---------------------+---------+--------------------+------+----------+-------------+
3 rows in set, 1 warning (0.00 sec)
~~~

### 4. partitions (可略) 

代表分区表中的命中情况，非分区表，该项为NULL。

一般情况下我们的查询语句的执行计划的partations列的信都是NULL。



如果想详细了解，可以如下方式测试。创建分区表：

~~~mysql
-- 创建分区表，
-- 按照id分区，id<100 p0分区，其他p1分区
CREATE TABLE user_partitions (id INT auto_increment,
    NAME VARCHAR(12),PRIMARY KEY(id))
    PARTITION BY RANGE(id)(
    PARTITION p0 VALUES less than(100),
    PARTITION p1 VALUES less than MAXVALUE
);
~~~

---

![image-20221001222910052](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210012229080.png)

---





~~~mysql
DESC SELECT * FROM user_partitions WHERE id>200;
~~~

查询id大于200（200>100，p1分区）的记录，查看执行计划，partitions是p1，符合我们的分区规则

---

![image-20221001223015378](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210012230997.png)

---





### 5. type ☆ 

执行计划的一条记录就代表着MySQL对某个表的执行查询时的访问方法，又称“访问类型”，其中的type列就表明了这个访问方法是啥，是较为重要的一个指标。

比如，看到type 列的值是ref，表明MySQL即将使用ref访问方法来执行对s1表的查询

完整的访问方法如下：

`system` ， `const` ， `eq_ref` ， `ref `， `fulltext `， `ref_or_null` ， `index_merge` ， `unique_subquery` ， `index_subquery` ， `range` ， `index` ， `ALL` 

 我们详细解释一下：

 type：针对单表的访问方法



当表中`只有一条记录`并且该表使用的存储引擎的统计 **数据是精确** 的，比如MyISAM、Memory，那么对该表的访问方法就是`system`。

~~~mysql
-- 建表
CREATE TABLE t(i INT) ENGINE=MYISAM;
 INSERT INTO t VALUES(1);
~~~



~~~MYSQL
-- 查看执行计划
EXPLAIN SELECT * FROM t;
 
 +----+-------------+-------+------------+--------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type   | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+--------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | t     | NULL       | system | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+--------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~

> 如果有两条数据，那么type就是ALL



~~~mysql
 -- 换成InnoDB
 CREATE TABLE tt(i INT) ENGINE=INNODB;
 INSERT INTO tt VALUES(1);
 
 
 -- 查看执行计划
 EXPLAIN SELECT * FROM tt;
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | tt    | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~







 当我们根据 **主键** 或者 **唯一二级索引** 列与常数进行等值匹配时，对单表的访问方法就是`const`

主键：

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE id = 10005;
  
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



唯一：

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key2 = 10066;
  
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | const | idx_key2      | idx_key2 | 5       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



在进行多表连接查询时，如果  **被驱动表**  是通过  **主键 或者 唯一二级索引 列**     **等值匹配**  的方式进行访问的（如果该主键或者唯一二级索引是  **联合索引**的话，所有的索引列都必须进行等值比较），则对该被驱动表的访问方法就是`eq_ref`

~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;

+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref              | rows | filtered | Extra |
+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ALL    | PRIMARY       | NULL    | NULL    | NULL             | 9895 |   100.00 | NULL  |
|  1 | SIMPLE      | s2    | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | atguigudb1.s1.id |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
2 rows in set, 1 warning (0.00 sec)
~~~





 当通过 **普通的二级索引列 **  与 **常量** 进行  **等值匹配**时来查询某个表，那么对该表的访问方法就可能是`ref`
~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
 
 +----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



索引失效场景之一：

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key3 = 10066;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | idx_key3      | NULL | NULL    | NULL | 9895 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 3 warnings (0.00 sec)
~~~

> key3 有普通索引 为什么这里是全表扫描
>
> 因为key3是字符串类型的，而这里使用数字类型与之比较，所以需要进行隐式转换【将字符串转为数字】
>
> 在这个过程中会修改到索引，所以会发生索引失效

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key3 = '10066';

+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key3      | idx_key3 | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



 当对  **普通二级索引**  进行 **等值匹配查询**，该索引列的值  也可以是`NULL`值   时，那么对该表的访问方法就可能是`ref_or_null`
~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key1 IS NULL;
 
 +----+-------------+-------+------------+-------------+---------------+----------+---------+-------+------+----------+-----------------------+
| id | select_type | table | partitions | type        | possible_keys | key      | key_len | ref   | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------------+---------------+----------+---------+-------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | ref_or_null | idx_key1      | idx_key1 | 303     | const |    2 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------------+---------------+----------+---------+-------+------+----------+-----------------------+
1 row in set, 1 warning (0.00 sec)
~~~





 单表访问方法时在某些场景下可以使用`Intersection`、`Union`、`Sort-Union`这三种索引合并的方式来执行查询
~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
 
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
| id | select_type | table | partitions | type        | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                       |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | index_merge | idx_key1,idx_key3 | idx_key1,idx_key3 | 303,303 | NULL |    2 |   100.00 | Using union(idx_key1,idx_key3); Using where |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~



如果使用AND 就是 普通索引的等值查询：

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' AND key3 = 'a';
+----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys     | key      | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1,idx_key3 | idx_key1 | 303     | const |    1 |     5.00 | Using where |
+----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~





 `unique_subquery`是针对在一些包含`IN`子查询的查询语句中，如果查询优化器决定将`IN`子查询转换为`EXISTS`子查询，而且子查询可以使用到  **主键进行等值匹配**  的话，那么该子查询执行计划的 `type` 列的值就是`unique_subquery`
~~~mysql
EXPLAIN SELECT * FROM s1 
WHERE key2 IN (SELECT id FROM s2 WHERE s1.key1 = s2.key1) OR key3 = 'a';

+----+--------------------+-------+------------+-----------------+------------------+---------+---------+------+------+----------+-------------+
| id | select_type        | table | partitions | type            | possible_keys    | key     | key_len | ref  | rows | filtered | Extra       |
+----+--------------------+-------+------------+-----------------+------------------+---------+---------+------+------+----------+-------------+
|  1 | PRIMARY            | s1    | NULL       | ALL             | idx_key3         | NULL    | NULL    | NULL | 9895 |   100.00 | Using where |
|  2 | DEPENDENT SUBQUERY | s2    | NULL       | unique_subquery | PRIMARY,idx_key1 | PRIMARY | 4       | func |    1 |    10.00 | Using where |
+----+--------------------+-------+------------+-----------------+------------------+---------+---------+------+------+----------+-------------+
2 rows in set, 2 warnings (0.00 sec)
~~~

> 转为这种：
>
> SELECT * FROM s1 
>
> WHERE EXISTS (SELECT 1 FROM s2 WHERE s1.key2=s2.id)





如果使用索引获取某些`范围区间`的记录，那么就可能使用到`range`访问方法

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 IN ('a', 'b', 'c');
 
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |    3 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.01 sec)


 -- 同上
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'a' AND key1 < 'b';
 
 +----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |  392 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.00 sec)

~~~





 当我们可以使用  **索引覆盖** 【可以不需要回表就可以查看到所有的想要select的数据】，但需要扫描全部的索引记录时，该表的访问方法就是`index`
~~~mysql
EXPLAIN SELECT key_part2 FROM s1 WHERE key_part3 = 'a';

+----+-------------+-------+------------+-------+---------------+--------------+---------+------+------+----------+--------------------------+
| id | select_type | table | partitions | type  | possible_keys | key          | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+-------+------------+-------+---------------+--------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | s1    | NULL       | index | idx_key_part  | idx_key_part | 909     | NULL | 9895 |    10.00 | Using where; Using index |
+----+-------------+-------+------------+-------+---------------+--------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
~~~



如果不是索引覆盖的话就会发生 **全表扫描**：

~~~mysql
EXPLAIN SELECT key1,key_part2 FROM s1 WHERE key_part3 = 'a';

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~



小结： 结果值从最好到最坏依次是： 

**system** > **const** > **eq_ref** > **ref** > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > **range** > **index** > **ALL** 其中比较重要的几个提取出来

**SQL 性能优化的目标**：**至少要达到 range 级别，要求是 ref 级别，最好是 consts级别**。（阿里巴巴 开发手册要求）





### 6. possible_keys和key

在EXPLAIN语句输出的执行计划中，possible_keys 列表示在某个查询语句中，对某个表执行单表查询时可能用到的索引有哪些。

一般查询涉及到的字段上若存在索引，则该索引将被列出，但 **不一定被查询使用** 。

key列表示 **实际用到的 索引** 有哪些，如果为 **NULL**，则**没有使用索引**。'

比方说下边这个查询：

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key3 = 'a';
 
 +----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys     | key      | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1,idx_key3 | idx_key3 | 303     | const |    1 |     5.00 | Using where |
+----+-------------+-------+------------+------+-------------------+----------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~



索引合并：

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' OR key3 = 'a';
 
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+--------------------------------------------------+
| id | select_type | table | partitions | type        | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                            |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+--------------------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | index_merge | idx_key1,idx_key3 | idx_key1,idx_key3 | 303,303 | NULL |  417 |   100.00 | Using sort_union(idx_key1,idx_key3); Using where |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+--------------------------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~





### 7.  key_len：

实际使用到的索引长度(即：字节数)

帮你检查`是否充分的利用上了索引`，`值越大越好`, **主要针对于联合索引**，有一定的参考意义。

 ~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE id = 10005;
 
 +----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
 | id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref   | rows | filtered | Extra |
 +----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
 |  1 | SIMPLE      | s1    | NULL       | const | PRIMARY       | PRIMARY | 4       | const |    1 |   100.00 | NULL  |
 +----+-------------+-------+------------+-------+---------------+---------+---------+-------+------+----------+-------+
 1 row in set, 1 warning (0.00 sec)
 ~~~

> INT 类型四个字节





~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key2 = 10126;
 
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | const | idx_key2      | idx_key2 | 5       | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~

> INT 类型四个字节 有可能为空，用一个字节表示



~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';

+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~

> 该表使用utf-8编码，一个字符三个字节 共三百字节
>
>  有可能为空，用一个字节表示 
>
> 这个字段是变长类型 用两位记录当前长度





联合索引的情况

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a';
 
+----+-------------+-------+------------+------+---------------+--------------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key          | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key_part  | idx_key_part | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a' AND key_part2 = 'b';
 
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key          | key_len | ref         | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key_part  | idx_key_part | 606     | const,const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



使用联合索引 key_len最大的情况：

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key_part1 = 'a' AND key_part2 = 'b' AND key_part3 = 'c';
 
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key          | key_len | ref               | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key_part  | idx_key_part | 909     | const,const,const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+--------------+---------+-------------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



不符合最左前缀原则，不走索引

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key_part3 = 'a';
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~



练习：
~~~mysql
1.varchar(10)变长字段且允许NULL  
= 10 * ( character set：utf8=3,gbk=2,latin1=1) + 1(NULL)+2(变长字段)

2.varchar(10)变长字段且不允许NULL 
= 10 * ( character set：utf8=3,gbk=2,latin1=1) + 2(变长字段)

3.char(10)固定字段且允许NULL    
= 10 * ( character set：utf8=3,gbk=2,latin1=1) + 1(NULL)

4.char(10)固定字段且不允许NULL  
= 10 * ( character set：utf8=3,gbk=2,latin1=1)
~~~



 ### 8. ref：

当使用索引列等值查询时，与索引列进行等值匹配的对象信息。

比如只是一个常数或者是某个列。

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';

+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec) 
~~~

> 'a' 是一个常量 所以是const





~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.id = s2.id;
 
+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
| id | select_type | table | partitions | type   | possible_keys | key     | key_len | ref              | rows | filtered | Extra |
+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
|  1 | SIMPLE      | s2    | NULL       | ALL    | PRIMARY       | NULL    | NULL    | NULL             | 9895 |   100.00 | NULL  |
|  1 | SIMPLE      | s1    | NULL       | eq_ref | PRIMARY       | PRIMARY | 4       | atguigudb1.s2.id |    1 |   100.00 | NULL  |
+----+-------------+-------+------------+--------+---------------+---------+---------+------------------+------+----------+-------+
2 rows in set, 1 warning (0.00 sec)
~~~

> 全表扫描s2 然后拿s2的id值与s1的id值进行等值匹配



~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s2.key1 = UPPER(s1.key1);

+----+-------------+-------+------------+------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL     | NULL    | NULL | 9895 |   100.00 | NULL                  |
|  1 | SIMPLE      | s2    | NULL       | ref  | idx_key1      | idx_key1 | 303     | func |    1 |   100.00 | Using index condition |
+----+-------------+-------+------------+------+---------------+----------+---------+------+------+----------+-----------------------+
2 rows in set, 1 warning (0.00 sec)
~~~

> 全表扫描s2 遍历s1中全部的key1 并拿他作为函数的入参获取一个返回值，与s2中的key1做等值匹配





### 9. rows：

**预估的需要读取的记录条数**   `值越小越好`

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z';
 
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |  416 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.00 sec)
~~~

> 值越小，需查找的数据在同一页的概率就会大，那么IO的次数就会少一点



~~~mysql
-- 上面预估要找416条
SELECT COUNT(*) FROM s1 WHERE key1 > 'z';

+----------+
| COUNT(*) |
+----------+
|      416 |
+----------+
1 row in set (0.00 sec)
~~~



### 10. filtered: 

某个表 经过  **搜索条件过滤后**  **剩余记录条数的百分比**

如果使用的是索引执行的单表扫描，那么计算时需要估计出满足除使用到对应索引的搜索条件外的其他搜索条件的记录有多少条。

 ~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND common_field = 'a';
 
 +----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+------------------------------------+
 | id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                              |
 +----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+------------------------------------+
 |  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |  416 |    10.00 | Using index condition; Using where |
 +----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+------------------------------------+
 1 row in set, 1 warning (0.00 sec)
 ~~~

> 这里通过有索引的字段进行过滤预估可能有416条记录
>
> 此时多加一个条件进行判断 对应filtered 为10.0 即 10%
>
> 那么最终只有41条记录左右



对于单表查询来说，这个filtered列的值没什么意义，我们`更关注在  连接查询中驱动表  对应的执行计划记录的filtered值`

它决定了被驱动表要执行的次数 ( 即：rows * filtered)
 ~~~mysql
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key1 WHERE s1.common_field = 'a';
 
 +----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
 | id | select_type | table | partitions | type | possible_keys | key      | key_len | ref                | rows | filtered | Extra       |
 +----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
 |  1 | SIMPLE      | s1    | NULL       | ALL  | idx_key1      | NULL     | NULL    | NULL               | 9895 |    10.00 | Using where |
 |  1 | SIMPLE      | s2    | NULL       | ref  | idx_key1      | idx_key1 | 303     | atguigudb1.s1.key1 |    1 |   100.00 | NULL        |
 +----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
 2 rows in set, 1 warning (0.00 sec)
 ~~~



 

### 11. Extra ☆

顾名思义，Extra列是用来说明一些额外信息的，包含不适合在其他列中显示但  **十分重要 的 额外信息**。

我们可以通讨这些额外信息来更准确的  **理解MySOL到底将如何执行给定的查询语句**。

MySOL提供的额外信息有好几十个，我们就不一个一个介绍了，所以我们只挑比较重要的额外信息介绍：





当查询语句的没有`FROM`子句时将会提示该额外信息 `No tables used`

~~~mysql
EXPLAIN SELECT 1;
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra          |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | No tables used |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
1 row in set, 1 warning (0.00 sec)
~~~



查询语句的`WHERE`子句永远为`FALSE`时将会提示该额外信息 `Impossible WHERE`

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE 1 != 1;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra            |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Impossible WHERE |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------+
1 row in set, 1 warning (0.00 sec)
~~~





当我们使用全表扫描来执行对某个表的查询，并且该语句的`WHERE` 子句中有针对该表的搜索条件且该条件所处的字段中没有索引时，在`Extra`列中会提示上述额外信息`Using where `

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE common_field = 'a';

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~



 当使用索引访问来执行对某个表的查询，并且该语句的`WHERE`子句中 有除了该索引包含的列之外的其他搜索条件时，在`Extra`列中也会提示上述额外信息。`Using where`

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' AND common_field = 'a';
 
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |    10.00 | Using where |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~



当查询列表处有`MIN`或者`MAX`聚合函数，但是并没有符合`WHERE`子句中的搜索条件的记录时，将会提示该额外信息
~~~mysql
 EXPLAIN SELECT MIN(key1) FROM s1 WHERE key1 = 'abcdefg';
 
 +----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                   |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | No matching min/max row |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------------------------+
1 row in set, 1 warning (0.00 sec)
~~~



~~~mysql
EXPLAIN SELECT MIN(key1) FROM s1 WHERE key1 = 'QwHZZa'; #QwHZZa 是 s1表中key1字段真实存在的数据
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                        |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Select tables optimized away |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~





 当我们的查询列表以及搜索条件中只包含属于某个索引的列，也就是在  **可以使用覆盖索引**  的情况下，在`Extra`列将会提示该额外信息。

比方说下边这个查询中只需要用到`idx_key1`而不需要回表操作【覆盖索引】：

~~~mysql
EXPLAIN SELECT key1,id FROM s1 WHERE key1 = 'a';
 
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref   | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | const |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+----------+---------+-------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~

> 在字段key1中建立了一个普通的二级索引，而且查找时也只是查找了关于该索引的字段与主键字段，不需要进行回表操作
>
> 每个二级索引都会存储该条记录的主键id 方便以后进行回表操作





 有些搜索条件中虽然出现了索引列，但却不能使用到索引 ,  理解索引条件下推`Using index condition`
~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%a';

+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |  416 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.00 sec)
~~~

在以前版本的MySQL中是按照下边步骤来执行这个查询的：

- 先根据 ` key1 >'z'  `这个条件，从二级索引 `idx_key1` 中获取到对应的二级索引记录
- 根据上一步骤得到的二级索引记录中的 **主键值** 进行**回表**，找到完整的记录 用key1 LIKE'%a'这个条件，将符合条件的记录加入到最后的结果集



改进

这个条件只涉及到了key1 列，所以MySOL把上边的步骤改进了一下：

- 先根据 `key1 >z` 这个条件，定位到二级索引 `idx_key1`  中对应的二级索引记录，对于指定的二级索引记录，先不着急回表，而是先检测一下该记录是否满足    `key1  LIKE  '%a'`这个条件  如果这个条件不满足，则该二级索引记录压根儿就没必要回表
- 对于满足 `key1  LIKE '%a'` 这个条件的二级索引记录执行回表操作我们说回表操作其实是一个随机I0，比较耗时，所以上述修改虽然只改讲了一点点，但是可以省去好多回表





MySQL把他们的这个改进称之为索引条件下推（英文名：Index Condition Pushdown)

如果在查询语句的执行过程中将要使用索引条件下推这个特性，在Extra列中将会显示Using andex condition

比如这祥:

~~~mysql
 EXPLAIN SELECT * FROM s1 WHERE key1 > 'z' AND key1 LIKE '%b';
 
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra                 |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | range | idx_key1      | idx_key1 | 303     | NULL |  416 |   100.00 | Using index condition |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-----------------------+
1 row in set, 1 warning (0.00 sec)

~~~



在连接查询执行过程中，当被驱动表不能有效的利用索引加快访问速度，MySQL一般会为其分配一块名叫`join buffer`的内存块来加快查询速度，也就是我们所讲的`基于块的嵌套循环算法`
~~~mysql
 EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.common_field = s2.common_field;
 
 +----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra                                      |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | NULL                                       |
|  1 | SIMPLE      | s2    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |    10.00 | Using where; Using join buffer (hash join) |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+--------------------------------------------+
2 rows in set, 1 warning (0.00 sec)
~~~







 当我们使用左（外）连接时，如果`WHERE`子句中包含要求被驱动表的某个列等于`NULL`值的搜索条件，而且那个列又是不允许存储`NULL`值的，那么在该表的执行计划的Extra列就会提示`Not exists`额外信息
~~~mysql
 EXPLAIN SELECT * FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE s2.id IS NULL;
 
 +----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref                | rows | filtered | Extra                   |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL     | NULL    | NULL               | 9895 |   100.00 | NULL                    |
|  1 | SIMPLE      | s2    | NULL       | ref  | idx_key1      | idx_key1 | 303     | atguigudb1.s1.key1 |    1 |    10.00 | Using where; Not exists |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------------------+
2 rows in set, 1 warning (0.00 sec)
~~~

> 主键值是非空且唯一，让主键值为NULL 且是被驱动表的字段

 





如果执行计划的`Extra`列出现了`Using intersect(...)`提示，说明准备使用`Intersect`索引合并的方式执行查询，括号中的`...`表示需要进行索引合并的索引名称；

如果出现了`Using union(...)`提示，说明准备使用`Union`索引合并的方式执行查询；

出现了`Using sort_union(...)`提示，说明准备使用`Sort-Union`索引合并的方式执行查询。

~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
 
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
| id | select_type | table | partitions | type        | possible_keys     | key               | key_len | ref  | rows | filtered | Extra                                       |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | index_merge | idx_key1,idx_key3 | idx_key1,idx_key3 | 303,303 | NULL |    2 |   100.00 | Using union(idx_key1,idx_key3); Using where |
+----+-------------+-------+------------+-------------+-------------------+-------------------+---------+------+------+----------+---------------------------------------------+
1 row in set, 1 warning (0.00 sec)
~~~





 当我们的`LIMIT`子句的参数为`0`时，表示压根儿不打算从表中读出任何记录，将会提示该额外信息`Zero limit`
~~~mysql
 EXPLAIN SELECT * FROM s1 LIMIT 0;	
 
 +----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra      |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------+
|  1 | SIMPLE      | NULL  | NULL       | NULL | NULL          | NULL | NULL    | NULL | NULL |     NULL | Zero limit |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+------------+
1 row in set, 1 warning (0.00 sec)
~~~





 有一些情况下对结果集中的 **记录进行排序** 是可以 **使用到索引的**。
 比如：

~~~mysql
 EXPLAIN SELECT * FROM s1 ORDER BY key1 LIMIT 10;
 
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------+
|  1 | SIMPLE      | s1    | NULL       | index | NULL          | idx_key1 | 303     | NULL |   10 |   100.00 | NULL  |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
~~~



 



 如果某个查询需要使用文件排序的方式执行查询，就会在执行计划的`Extra`列中显示`Using filesort`提示
```mysql
EXPLAIN SELECT * FROM s1 ORDER BY common_field LIMIT 10;
 
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra          |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using filesort |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+----------------+
1 row in set, 1 warning (0.00 sec)
```

>  **很多情况下排序操作**  **无法使用到索引**，只能在  内存中（记录较少的时候)或者  磁盘中（记录较多的时候)进行排序
>
> MySQL把这种在内存中或者磁盘上进行排序的方式统称为 **文件排序**（英文名：`filesort`）。





在许多查询的执行过程中，MySQL可能会  **借 助临时表**   来完成一些功能，比如去重、排序之类的，比如我们在执行许多包含`DISTINCT`、`GROUP BY`、`UNION`等子句的查询过程中，如果不能有效利用索引来完成查询，MySQL很有可能寻求通过建立内部的临时表来执行查询。

如果查询中使用到了内部的临时表，在执行计划的`Extra`列将会显示`Using temporary`提示

~~~mysql
EXPLAIN SELECT DISTINCT common_field FROM s1;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using temporary |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
1 row in set, 1 warning (0.01 sec)
~~~



有索引去重：

~~~mysql
EXPLAIN SELECT DISTINCT key1 FROM s1;

+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | index | idx_key1      | idx_key1 | 303     | NULL | 9895 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~

> key1上有索引，本身是有序的状态，去重起来很简单





~~~mysql
EXPLAIN SELECT common_field, COUNT(*) AS amount FROM s1 GROUP BY common_field;

+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | SIMPLE      | s1    | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 9895 |   100.00 | Using temporary |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-----------------+
1 row in set, 1 warning (0.00 sec)
~~~

>  执行计划中出现`Using temporary`并不是一个好的征兆，因为建立与维护临时表要付出很大成本的，所以我们`最好能使用索引来替代掉使用临时表`。比如：扫描指定的索引idx_key1即可



使用有索引的字段进行分组，会走索引的全表扫描 Extra是 `Using index` 表名该次查询有使用 **索引覆盖**

~~~mysql
EXPLAIN SELECT key1, COUNT(*) AS amount FROM s1 GROUP BY key1;

+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key      | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | s1    | NULL       | index | idx_key1      | idx_key1 | 303     | NULL | 9895 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+----------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
~~~





### 12.小结
- EXPLAIN不考虑各种Cache
- EXPLAIN不能显示MySOL在执行查询时所作的优化工作
- EXPLAIN不会告诉你关干触发器、存储讨程的信息或用户自定义函数 **对** **查询的影响情况**
- 部分统计信息是估算的，**并非精确值**



# 7. EXPLAIN的进一步使用 

## 7.1 EXPLAIN四种输出格式 

这里谈谈EXPLAIN的输出格式。

EXPLAIN可以输出四种格式： **传统格式** ， **JSON格式** ， **TREE格式** 以及 **可 视化输出** 。

用户可以根据需要选择适用于自己的格式。 



### 1. 传统格式 

传统格式简单明了，输出是一个表格形式，概要说明查询计划。



~~~mysql
EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 WHERE
s2.common_field IS NOT NULL;

+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref                | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
|  1 | SIMPLE      | s2    | NULL       | ALL  | idx_key1      | NULL     | NULL    | NULL               | 9895 |    90.00 | Using where |
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | atguigudb1.s2.key1 |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
~~~





### 2. JSON格式 JSON格式：

第1种格式中介绍的EXPLAIN语句输出中缺少了一个衡量执行计划好坏的重要属性--  **成本**。

而JSON格式是四种格式里面输出信息  **最详尽的格式**  ，里面包含了执行的成本信息

- 在EXPLAIN单词和真正的查询语句中间加上 `FORMAT=JSON `。

~~~mysql
EXPLAIN FORMAT=JSON SELECT ....
~~~

- EXPLAIN的Column与JSON的对应关系：（来源于MySQL5.7文档)

----

![image-20221004110719119](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210041107728.png)

---





JSON 格式：

~~~mysql
EXPLAIN FORMAT=JSON SELECT * FROM s1 
INNER JOIN s2 ON s1.key1 = s2. key2 
WHERE s1.common_field='a'\G
 
*************************** 1. row ***************************
EXPLAIN: {
  "query_block": {
    "select_id": 1,   -- 整个查询语句只有1个SELECT关键字，该关键字对应的1d号为1
    "cost_info": {
      "query_cost": "2088.46"   -- 整个查询的执行成本预计为2088.46
    },
    "nested_loop": [   -- 几个表之间采用流套循环连接靠法技行
     
        -- 以下是参与嵌套循环连接算法的各个表的信息
        

        
      {
        "table": {
          "table_name": "s1", -- s1表是取动表
          "access_type": "ALL", -- 访问方法为ALL，意味着便用全表扫描访问
          "possible_keys": [
            "idx_key1"   -- 可能使用的索引
          ],

        
          "rows_examined_per_scan": 9895, -- 查询一次s1表大致需要扫描9895条记录
          "rows_produced_per_join": 989, -- 驱动表s1的扇出是989
          "filtered": "10.00",    -- condition filtering代表的百分比

          "cost_info": {
            "read_cost": "914.80", -- 稍后解释
            "eval_cost": "98.95", -- 稍后解释
            "prefix_cost": "1013.75", -- 单次查询s1表总共的成本
            "data_read_per_join": "1M" -- 读取的数据量
          },
        
          "used_columns": [ -- 执行查询中涉及到的列
            "id",
            "key1",
            "key2",
            "key3",
            "key_part1",
            "key_part2",
            "key_part3",
            "common_field"
          ],
        
        -- 对s1表访问时针对单表查询的条件
          "attached_condition": "((`atguigudb1`.`s1`.`common_field` = 'a') and (`atguigudb1`.`s1`.`key1` is not null))"
        }
      },
        
        
     
      {
        "table": {
          "table_name": "s2", --  s2表是被驱动表
          "access_type": "eq_ref", -- 访问方法为eq_ref，意味着使用 唯一索引 等值匹配 的方式访问
          "possible_keys": [  -- 可能使用的索引
            "idx_key2"
          ],
        
       

          "key": "idx_key2", --  实际使用的索引
          "used_key_parts": [ -- 使用到的索引列
            "key2"
          ],
        
          "key_length": "5", -- key_length
        
          "ref": [
            "atguigudb1.s1.key1" -- 与key2列进行等值匹配的对象
          ],
        
          "rows_examined_per_scan": 1, -- 查询一次s2表大致需要扫描1条记录
          "rows_produced_per_join": 989, -- 被聚动表s2的扇出是989 （由于后边没有多余的表进行连接，所以这个值也没啥用)
          "filtered": "100.00", -- condition filtering代表的自分区
          "index_condition": "(cast(`atguigudb1`.`s1`.`key1` as double) = cast(`atguigudb1`.`s2`.`key2` as double))",
        
        -- s2表使用素引进行查询的索引条件
          "cost_info": {
            "read_cost": "975.76", -- 稍后解释
            "eval_cost": "98.95", -- 稍后解释
            "prefix_cost": "2088.46", -- 单次查询s1、多次查询S2表总共的成本
            "data_read_per_join": "1M" -- 读取的数据量
          },
        
        -- 执行查询中涉及到的列
          "used_columns": [
            "id",
            "key1",
            "key2",
            "key3",
            "key_part1",
            "key_part2",
            "key_part3",
            "common_field"
          ]
        }
      }
    ]
  }
}
1 row in set, 2 warnings (0.00 sec)
~~~



传统格式：

~~~mysql
EXPLAIN SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2. key2 WHERE s1.common_field='a';

+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+-----------------------+
| id | select_type | table | partitions | type   | possible_keys | key      | key_len | ref                | rows | filtered | Extra                 |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+-----------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL    | idx_key1      | NULL     | NULL    | NULL               | 9895 |    10.00 | Using where           |
|  1 | SIMPLE      | s2    | NULL       | eq_ref | idx_key2      | idx_key2 | 5       | atguigudb1.s1.key1 |    1 |   100.00 | Using index condition |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+-----------------------+
2 rows in set, 2 warnings (0.00 sec)
~~~

我们使用 # 后边跟随注释的形式为大家解释了` EXPLAIN FORMAT=JSON `语句的输出内容，但是大家可能 有疑问 **"cost_info"** 里边的成本看着怪怪的，它们是怎么计算出来的？

先看 s1 表的` "cost_info" `部 分：

~~~mysql
"cost_info": {
    "read_cost": "1840.84",
    "eval_cost": "193.76",
    "prefix_cost": "2034.60",
    "data_read_per_join": "1M"
}
~~~

read_cost 是由下边这两部分组成的： 

- IO 成本 
- 检测 `rows × (1 - filter)` 条 记录的 CPU 成本

> 小贴士： 
>
> rows和filter都是我们前边介绍执行计划的输出列，在JSON格式的执行计划中，rows 相当于rows_examined_per_scan，filtered名称不变。

eval_cost 是这样计算的： 

- 检测 `rows × filter` 条记录的成本。 
- `prefix_cost` 就是单独查询 s1 表的成本，也就是：`read_cost + eval_cost `
- `data_read_per_join` 表示在此次查询中需要读取的数据量。





对于 s2 表的 "cost_info" 部分是这样的：

~~~mysql
"cost_info": {
    "read_cost": "968.80",
    "eval_cost": "193.76",
    "prefix_cost": "3197.16",
    "data_read_per_join": "1M"
}
~~~

由于 s2 表是被驱动表，所以可能被读取多次，这里的 read_cost 和 eval_cost 是访问多次 s2 表后累 加起来的值，大家主要关注里边儿的 prefix_cost 的值代表的是整个连接查询预计的成本，也就是单 次查询 s1 表和多次查询 s2 表后的成本的和，也就是：

> 968.80 + 193.76 + 2034.60 = 3197.16





### 3. TREE格式

TREE格式是8.0.16版本之后引入的新格式，主要根据查询的 各个部分之间的关系 和 各部分的执行顺序 来描 述如何查询。



~~~mysql
EXPLAIN FORMAT=tree SELECT * FROM s1 INNER JOIN s2 ON s1.key1 = s2.key2 
WHERE s1.common_field = 'a'\G

*************************** 1. row ***************************
EXPLAIN: -> Nested loop inner join  (cost=2174.95 rows=990)
    -> Filter: ((s1.common_field = 'a') and (s1.key1 is not null))  (cost=1086.50 rows=990)
        -> Table scan on s1  (cost=1086.50 rows=9895)
    -> Single-row index lookup on s2 using idx_key2 (key2=s1.key1), with index condition: (cast(s1.key1 as double) = cast(s2.key2 as double))  (cost=1.00 rows=1)

1 row in set, 1 warning (0.01 sec)
~~~





### 4. 可视化输出 

可视化输出，可以通过MySQL Workbench可视化查看MySQL的执行计划。通过点击Workbench的放大镜图 标，即可生成可视化的查询计划。 上图按从左到右的连接顺序显示表。红色框表示 全表扫描 ，而绿色框表示使用 索引查找 。对于每个表， 显示使用的索引。还要注意的是，每个表格的框上方是每个表访问所发现的行数的估计值以及访问该表 的成本。





---

![image-20221006210642895](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210062106479.png)

---



---

![image-20221006211001440](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210062110319.png)

----





## 7.2 SHOW WARNINGS的使用

在我们使用`EXPLAIN`语句查看了某个查询的执行计划后，紧接着还可以使用`SHOW WARNINGS`语句查看与这个查询的执行计划有关的一些扩展信息，比如这样

~~~mysql
EXPLAIN SELECT s1.key1, s2.key1 FROM s1 LEFT JOIN s2 ON s1.key1 = s2.key1 
WHERE s2.common_field IS NOT NULL;

+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
| id | select_type | table | partitions | type | possible_keys | key      | key_len | ref                | rows | filtered | Extra       |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
|  1 | SIMPLE      | s2    | NULL       | ALL  | idx_key1      | NULL     | NULL    | NULL               | 9895 |    90.00 | Using where |
|  1 | SIMPLE      | s1    | NULL       | ref  | idx_key1      | idx_key1 | 303     | atguigudb1.s2.key1 |    1 |   100.00 | Using index |
+----+-------------+-------+------------+------+---------------+----------+---------+--------------------+------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
~~~



~~~mysql
SHOW WARNINGS\G
*************************** 1. row ***************************
  Level: Note
   Code: 1003
Message: /* select#1 */ select `atguigudb1`.`s1`.`key1` AS `key1`,`atguigudb1`.`s2`.`key1` AS `key1` from `atguigudb1`.`s1` join `atguigudb1`.`s2` where ((`atguigudb1`.`s1`.`key1` = `atguigudb1`.`s2`.`key1`) and (`atguigudb1`.`s2`.`common_field` is not null))
1 row in set (0.00 sec)
~~~

> 将外连接转为内连接







~~~mysql
EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key2 FROM s2 WHERE common_field = 'a');

+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+
| id | select_type | table | partitions | type   | possible_keys | key      | key_len | ref                | rows | filtered | Extra                              |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+
|  1 | SIMPLE      | s1    | NULL       | ALL    | idx_key1      | NULL     | NULL    | NULL               | 9895 |   100.00 | Using where                        |
|  1 | SIMPLE      | s2    | NULL       | eq_ref | idx_key2      | idx_key2 | 5       | atguigudb1.s1.key1 |    1 |    10.00 | Using index condition; Using where |
+----+-------------+-------+------------+--------+---------------+----------+---------+--------------------+------+----------+------------------------------------+
2 rows in set, 2 warnings (0.00 sec)
~~~



~~~mysql
SHOW WARNINGS\G
*************************** 1. row ***************************
  Level: Warning
   Code: 1739
Message: Cannot use ref access on index 'idx_key1' due to type or collation conversion on field 'key1'
*************************** 2. row ***************************
  Level: Note
   Code: 1003
Message: /* select#1 */ select `atguigudb1`.`s1`.`id` AS `id`,`atguigudb1`.`s1`.`key1` AS `key1`,`atguigudb1`.`s1`.`key2` AS `key2`,`atguigudb1`.`s1`.`key3` AS `key3`,`atguigudb1`.`s1`.`key_part1` AS `key_part1`,`atguigudb1`.`s1`.`key_part2` AS `key_part2`,`atguigudb1`.`s1`.`key_part3` AS `key_part3`,`atguigudb1`.`s1`.`common_field` AS `common_field` from `atguigudb1`.`s2` join `atguigudb1`.`s1` where ((`atguigudb1`.`s2`.`common_field` = 'a') and (cast(`atguigudb1`.`s1`.`key1` as double) = cast(`atguigudb1`.`s2`.`key2` as double)))
2 rows in set (0.00 sec)
~~~

> 将内连接转为多表连接





# 8. 分析优化器执行计划：trace

`OPTINIZER_TRACE` 是MySQL5.6引入的一项跟踪功能，它可以跟踪优化器做出的各种决策（比如访问表的方法各种开销计算、各种转换等），并将跟踪结果记录到` INFORMATION_SCHEMA.OPTIMIZER_TRACE `表中。



此功能默认关闭。开启trace，并设置格式为JSON，同时设置trace最大能够使用的内存大小，避免解析过程中  **因为默认内存过小而不能够完整展示** 。

~~~mysql
SET optimizer_trace="enabled=on",end_markers_in_json=on;
Query OK, 0 rows affected (0.00 sec)

set optimizer_trace_max_mem_size=1000000;
Query OK, 0 rows affected (0.00 sec)
~~~









开启后，可分析如下语句： SELECT  INSERT  REPLACE  UPDATE  DELETE EXPLAIN  SET  DECLARE  CASE I F  RETURN  CALL 



测试：执行如下SQL语句

~~~MYSQL
select * from student where id < 10;
 
+----+--------+--------+------+---------+
| id | stuno  | name   | age  | classId |
+----+--------+--------+------+---------+
|  1 | 100002 | CjoQoX |   96 |     855 |
|  2 | 100003 | wDDfSY |   32 |     366 |
|  3 | 100004 | ciHLJl |   15 |     678 |
|  4 | 100005 | jTPtAu |   48 |     953 |
|  5 | 100006 | zFCVYd |   51 |      63 |
|  6 | 100007 | VtjOsw |   20 |     294 |
|  7 | 100008 | dArjSN |   33 |     989 |
|  8 | 100009 | iSPuFP |   30 |     727 |
|  9 | 100010 | WANlRC |   27 |     359 |
+----+--------+--------+------+---------+
9 rows in set (0.01 sec)
~~~



最后， 查询 information_schema.optimizer_trace 就可以知道MySQL是如何执行SQL的 ：

~~~mysql
 select * from information_schema.optimizer_trace\G

*************************** 1. row ***************************
 							-- 第1部分：查询语句
                            QUERY: select * from student where id < 10
                            -- 第2部分：QUERY字段对应语句的跟踪信息
                            TRACE: {
  "steps": [
    {
      "join_preparation": { -- 准备工作
        "select#": 1,
        "steps": [
          {
            "expanded_query": "/* select#1 */ select `student`.`id` AS `id`,`student`.`stuno` AS `stuno`,`student`.`name` AS `name`,`student`.`age` AS `age`,`student`.`classId` AS `classId` from `student` where (`student`.`id` < 10)"
          }
        ] /* steps */
      } /* join_preparation */
    },
    {
      "join_optimization": { -- 进行优化
        "select#": 1,
        "steps": [
          {
            "condition_processing": { -- 条件处理
              "condition": "WHERE",
              "original_condition": "(`student`.`id` < 10)",
              "steps": [
                {
                  "transformation": "equality_propagation",
                  "resulting_condition": "(`student`.`id` < 10)"
                },
                {
                  "transformation": "constant_propagation",
                  "resulting_condition": "(`student`.`id` < 10)"
                },
                {
                  "transformation": "trivial_condition_removal",
                  "resulting_condition": "(`student`.`id` < 10)"
                }
              ] /* steps */
            } /* condition_processing */
          },
          {
            "substitute_generated_columns": { -- 替换生成的列
            } /* substitute_generated_columns */
          },
          {
            "table_dependencies": [ -- 表的依赖关系
              {
                "table": "`student`",
                "row_may_be_null": false,
                "map_bit": 0,
                "depends_on_map_bits": [
                ] /* depends_on_map_bits */
              }
            ] /* table_dependencies */
          },
          {
            "ref_optimizer_key_uses": [ -- 使用键
            ] /* ref_optimizer_key_uses */
          },
          {
            "rows_estimation": [  -- 行判断
              {
                "table": "`student`",
                "range_analysis": {
                  "table_scan": { -- 扫描表
                    "rows": 3859527,
                    "cost": 396814
                  } /* table_scan */,
                  "potential_range_indexes": [ -- 潜在的范围索引
                    {
                      "index": "PRIMARY",
                      "usable": true,
                      "key_parts": [
                        "id"
                      ] /* key_parts */
                    }
                  ] /* potential_range_indexes */,
                  "setup_range_conditions": [
                  ] /* setup_range_conditions */,  -- 设置范围条件
                  "group_index_range": {
                    "chosen": false,
                    "cause": "not_group_by_or_distinct"
                  } /* group_index_range */,
                  "skip_scan_range": {
                    "potential_skip_scan_indexes": [
                      {
                        "index": "PRIMARY",
                        "usable": false,
                        "cause": "query_references_nonkey_column"
                      }
                    ] /* potential_skip_scan_indexes */
                  } /* skip_scan_range */,
                  "analyzing_range_alternatives": { -- 分析范围选项
                    "range_scan_alternatives": [
                      {
                        "index": "PRIMARY",
                        "ranges": [
                          "id < 10"
                        ] /* ranges */,
                        "index_dives_for_eq_ranges": true,
                        "rowid_ordered": true,
                        "using_mrr": false,
                        "index_only": false,
                        "rows": 9,
                        "cost": 1.91986,
                        "chosen": true
                      }
                    ] /* range_scan_alternatives */,
                    "analyzing_roworder_intersect": {
                      "usable": false,
                      "cause": "too_few_roworder_scans"
                    } /* analyzing_roworder_intersect */
                  } /* analyzing_range_alternatives */,
                  "chosen_range_access_summary": { -- 选择范围访问摘要
                    "range_access_plan": {
                      "type": "range_scan",
                      "index": "PRIMARY",
                      "rows": 9,
                      "ranges": [
                        "id < 10"
                      ] /* ranges */
                    } /* range_access_plan */,
                    "rows_for_plan": 9,
                    "cost_for_plan": 1.91986,
                    "chosen": true
                  } /* chosen_range_access_summary */
                } /* range_analysis */
              }
            ] /* rows_estimation */
          },
          {
            "considered_execution_plans": [ -- 考虑执行计划
              {
                "plan_prefix": [
                ] /* plan_prefix */,
                "table": "`student`",
                "best_access_path": { -- 最佳访问路径
                  "considered_access_paths": [
                    {
                      "rows_to_scan": 9,
                      "access_type": "range",
                      "range_details": {
                        "used_index": "PRIMARY"
                      } /* range_details */,
                      "resulting_rows": 9,
                      "cost": 2.81986,
                      "chosen": true
                    }
                  ] /* considered_access_paths */
                } /* best_access_path */,
                "condition_filtering_pct": 100, -- 行过滤百分比
                "rows_for_plan": 9,
                "cost_for_plan": 2.81986,
                "chosen": true
              }
            ] /* considered_execution_plans */
          },
          {
            "attaching_conditions_to_tables": { -- 将条件附加到表上
              "original_condition": "(`student`.`id` < 10)",
              "attached_conditions_computation": [
              ] /* attached_conditions_computation */,
              "attached_conditions_summary": [
                {
                  "table": "`student`",
                  "attached": "(`student`.`id` < 10)"
                }
              ] /* attached_conditions_summary */ -- 附加条件概要
            } /* attaching_conditions_to_tables */
          },
          {
            "finalizing_table_conditions": [
              {
                "table": "`student`",
                "original_table_condition": "(`student`.`id` < 10)",
                "final_table_condition   ": "(`student`.`id` < 10)"
              }
            ] /* finalizing_table_conditions */
          },
          {
            "refine_plan": [ -- 精简计划
              {
                "table": "`student`"
              }
            ] /* refine_plan */
          }
        ] /* steps */
      } /* join_optimization */
    },
    {
      "join_execution": { -- 执行
        "select#": 1,
        "steps": [
        ] /* steps */
      } /* join_execution */
    }
  ] /* steps */
}
 -- 第3部分：跟踪信息过长时，被截断的跟踪信息的字节数
MISSING_BYTES_BEYOND_MAX_MEM_SIZE: 0 -- 丢失的超出最大容量的字节
-- 第4部分：执行跟踪语句的用户是否有查看对象的权限。当不具有权限时，该列信息为1且TRACE字段为空，一般在调用带有 SQL SECURITY DEFINER的视图或者是存储过程的情况下，会出现此问题
          INSUFFICIENT_PRIVILEGES: 0 -- 缺失权限
1 row in set (0.00 sec)
~~~



# 9. MySQL监控分析视图-sys schema

关于MySQL的性能监控和问题诊断，我们一般都从performance_schema中去获取想要的数据，在MySQL5.7.7版本中新增sys schema，它将`performance_schema`和 `informationLschema` 中的数据以更容易理解的方式总结旧纳为"视图”

其目的就是为了降低查询`performance_schema`的复杂度 让DBA能够快速的定位问题。下面看看这些库中都有哪些监控表和视图，掌握了这些，在我们开发和运维的讨程中就起到了事半功倍的效果





## 9.1 Sys schema视图摘要 

1. 主机相关：以host_summary开头，主要汇总了IO延迟的信息。 
2. Innodb相关：以innodb开头，汇总了innodb buffer信息和事务等待innodb锁的信息。
3. I/o相关：以io开头，汇总了等待I/O、I/O使用量情况。
4.  内存使用情况：以memory开头，从主机、线程、事件等角度展示内存的使用情况
5. 连接与会话信息：processlist和session相关视图，总结了会话相关信息。
6. 表相关：以schema_table开头的视图，展示了表的统计信息。
7. 索引信息：统计了索引的使用情况，包含冗余索引和未使用的索引情况。
8. 语句相关：以statement开头，包含执行全表扫描、使用临时表、排序等的语句信息。
9.  用户相关：以user开头的视图，统计了用户使用的文件I/O、执行语句统计信息。
10. 等待事件相关信息：以wait开头，展示等待事件的延迟情况。







## 9.2 Sys schema视图使用场景 

**索引情况**

~~~mysql
-- 1. 查询冗余索引
select * from sys.schema_redundant_indexes;

+--------------+--------------+-------------------------+-------------------------+----------------------------+---------------------+------------------------+---------------------------+----------------+----------------------------------------------------------------------------+
| table_schema | table_name   | redundant_index_name    | redundant_index_columns | redundant_index_non_unique | dominant_index_name | dominant_index_columns | dominant_index_non_unique | subpart_exists | sql_drop_index                                                             |
+--------------+--------------+-------------------------+-------------------------+----------------------------+---------------------+------------------------+---------------------------+----------------+----------------------------------------------------------------------------+
| atguigudb    | departments  | dept_id_pk              | department_id           |                          0 | PRIMARY             | department_id          |                         0 |              0 | ALTER TABLE `atguigudb`.`departments` DROP INDEX `dept_id_pk`              |
| atguigudb    | employees    | emp_emp_id_pk           | employee_id             |                          0 | PRIMARY             | employee_id            |                         0 |              0 | ALTER TABLE `atguigudb`.`employees` DROP INDEX `emp_emp_id_pk`             |
| atguigudb    | job_history  | jhist_emp_id_st_date_pk | employee_id,start_date  |                          0 | PRIMARY             | employee_id,start_date |                         0 |              0 | ALTER TABLE `atguigudb`.`job_history` DROP INDEX `jhist_emp_id_st_date_pk` |
| atguigudb    | jobs         | job_id_pk               | job_id                  |                          0 | PRIMARY             | job_id                 |                         0 |              0 | ALTER TABLE `atguigudb`.`jobs` DROP INDEX `job_id_pk`                      |
| atguigudb    | locations    | loc_id_pk               | location_id             |                          0 | PRIMARY             | location_id            |                         0 |              0 | ALTER TABLE `atguigudb`.`locations` DROP INDEX `loc_id_pk`                 |
| atguigudb    | regions      | reg_id_pk               | region_id               |                          0 | PRIMARY             | region_id              |                         0 |              0 | ALTER TABLE `atguigudb`.`regions` DROP INDEX `reg_id_pk`                   |
| atguigudb1   | student_info | idx_sid                 | student_id              |                          1 | idx_sid_cre_time    | student_id,create_time |                         1 |              0 | ALTER TABLE `atguigudb1`.`student_info` DROP INDEX `idx_sid`               |
+--------------+--------------+-------------------------+-------------------------+----------------------------+---------------------+------------------------+---------------------------+----------------+----------------------------------------------------------------------------+
7 rows in set (0.01 sec)
~~~



什么是冗余索引

~~~mysql
 show index from student_info;
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

> 这里联合索引 idx_sid_cre_time 既与student_id字段有关联 又与create_time字段有关联
>
> 所以单列索引 idx_cre_time 就显得多余









~~~mysql
-- 2. 查询未使用过的索引
 select * from sys.schema_unused_indexes;
 
+--------------------+----------------------------------------------+----------------------------------+
| object_schema      | object_name                                  | index_name                       |
+--------------------+----------------------------------------------+----------------------------------+
| atguigudb          | countries                                    | countr_reg_fk                    |
| atguigudb          | departments                                  | dept_loc_fk                      |
| atguigudb          | departments                                  | dept_mgr_fk                      |
| atguigudb          | employees                                    | emp_dept_fk                      |
| atguigudb          | employees                                    | emp_job_fk                       |
| atguigudb          | employees                                    | emp_manager_fk                   |
| atguigudb          | job_history                                  | jhist_job_fk                     |
| atguigudb          | job_history                                  | jhist_dept_fk                    |
| atguigudb          | locations                                    | loc_c_id_fk                      |
| atguigudb1         | s1                                           | idx_key1                         |
| atguigudb1         | s1                                           | idx_key3                         |
| atguigudb1         | s1                                           | idx_key_part                     |
| atguigudb1         | s2                                           | idx_key1                         |
| atguigudb1         | s2                                           | idx_key3                         |
| atguigudb1         | s2                                           | idx_key_part                     |
| atguigudb1         | student_info                                 | idx_sid                          |
| atguigudb1         | student_info                                 | idx_cre_time                     |
| atguigudb1         | student_info                                 | idx_sid_cre_time                 |
| dbtest2            | book                                         | idx_bname                        |
| dbtest2            | book3                                        | mul_bid_bname                    |
| dbtest2            | book6                                        | idx_cmt                          |
| dbtest2            | book6                                        | mul_bid_bname_indo               |
| dbtest2            | book7                                        | idx_cmt                          |
| dbtest2            | book7                                        | idx_pu_year                      |
| dbtest2            | test2                                        | single_idx_name                  |
| dbtest2            | test4                                        | futxt_idx_info                   |
| dbtest2            | ts1                                          | idx_a_b                          |
| performance_schema | cond_instances                               | NAME                             |
| performance_schema | data_lock_waits                              | REQUESTING_ENGINE_LOCK_ID        |
| performance_schema | data_lock_waits                              | BLOCKING_ENGINE_LOCK_ID          |
| performance_schema | data_lock_waits                              | REQUESTING_ENGINE_TRANSACTION_ID |
| performance_schema | data_lock_waits                              | BLOCKING_ENGINE_TRANSACTION_ID   |
| performance_schema | data_lock_waits                              | REQUESTING_THREAD_ID             |
| performance_schema | data_lock_waits                              | BLOCKING_THREAD_ID               |
| performance_schema | data_locks                                   | ENGINE_TRANSACTION_ID            |
| performance_schema | data_locks                                   | THREAD_ID                        |
| performance_schema | data_locks                                   | OBJECT_SCHEMA                    |
| performance_schema | error_log                                    | THREAD_ID                        |
| performance_schema | error_log                                    | PRIO                             |
| performance_schema | error_log                                    | ERROR_CODE                       |
| performance_schema | error_log                                    | SUBSYSTEM                        |
| performance_schema | events_waits_summary_by_instance             | EVENT_NAME                       |
| performance_schema | file_instances                               | EVENT_NAME                       |
| performance_schema | file_summary_by_instance                     | FILE_NAME                        |
| performance_schema | file_summary_by_instance                     | EVENT_NAME                       |
| performance_schema | host_cache                                   | HOST                             |
| performance_schema | metadata_locks                               | OBJECT_TYPE                      |
| performance_schema | metadata_locks                               | OWNER_THREAD_ID                  |
| performance_schema | mutex_instances                              | NAME                             |
| performance_schema | mutex_instances                              | LOCKED_BY_THREAD_ID              |
| performance_schema | prepared_statements_instances                | STATEMENT_ID                     |
| performance_schema | prepared_statements_instances                | STATEMENT_NAME                   |
| performance_schema | prepared_statements_instances                | OWNER_OBJECT_TYPE                |
| performance_schema | replication_applier_status_by_coordinator    | THREAD_ID                        |
| performance_schema | replication_applier_status_by_worker         | THREAD_ID                        |
| performance_schema | replication_asynchronous_connection_failover | CHANNEL_NAME                     |
| performance_schema | replication_connection_status                | THREAD_ID                        |
| performance_schema | rwlock_instances                             | NAME                             |
| performance_schema | rwlock_instances                             | WRITE_LOCKED_BY_THREAD_ID        |
| performance_schema | socket_instances                             | THREAD_ID                        |
| performance_schema | socket_instances                             | SOCKET_ID                        |
| performance_schema | socket_instances                             | IP                               |
| performance_schema | socket_summary_by_instance                   | EVENT_NAME                       |
| performance_schema | table_handles                                | OBJECT_TYPE                      |
| performance_schema | table_handles                                | OWNER_THREAD_ID                  |
| performance_schema | threads                                      | PROCESSLIST_ID                   |
| performance_schema | threads                                      | THREAD_OS_ID                     |
| performance_schema | threads                                      | NAME                             |
| performance_schema | threads                                      | PROCESSLIST_ACCOUNT              |
| performance_schema | threads                                      | PROCESSLIST_HOST                 |
| performance_schema | threads                                      | RESOURCE_GROUP                   |
+--------------------+----------------------------------------------+----------------------------------+
71 rows in set (0.01 sec)
~~~



~~~mysql
#3. 查询索引的使用情况
select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
from sys.schema_index_statistics where table_schema='dbname' ;
Empty set (0.00 sec)
~~~







**表相关**

~~~mysql
-- 1. 查询表的访问量
select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from
 sys.schema_table_statistics group by table_schema,table_name order by io desc;
+--------------------+------------------------------------------------------+------+
| table_schema       | table_name                                           | io   |
+--------------------+------------------------------------------------------+------+
| atguigudb1         | student                                              |    5 |
| atguigudb1         | t                                                    |    2 |
| dbtest2            | test4                                                |    2 |
| mysql              | general_log                                          |    2 |
| mysql              | slow_log                                             |    2 |
| atguigudb          | countries                                            |    0 |
| atguigudb          | departments                                          |    0 |
| atguigudb          | employees                                            |    0 |
| atguigudb          | job_grades                                           |    0 |
| atguigudb          | job_history                                          |    0 |
| atguigudb          | jobs                                                 |    0 |
| atguigudb          | locations                                            |    0 |
| atguigudb          | order                                                |    0 |
| atguigudb          | regions                                              |    0 |
| atguigudb1         | course                                               |    0 |
| atguigudb1         | s1                                                   |    0 |
| atguigudb1         | s2                                                   |    0 |
| atguigudb1         | student_info                                         |    0 |
| atguigudb1         | tt                                                   |    0 |
| cloud_disk         | cloud_disk                                           |    0 |
| cloud_disk         | cloud_file                                           |    0 |
| cloud_disk         | cloud_file_history                                   |    0 |
| cloud_disk         | cloud_file_share                                     |    0 |
| cloud_disk         | cloud_folder                                         |    0 |
| cloud_disk         | cloud_group                                          |    0 |
| cloud_disk         | cloud_group_user                                     |    0 |
| cloud_disk         | cloud_log                                            |    0 |
| cloud_disk         | portal_seq_gen                                       |    0 |
| dbtest15           | departments                                          |    0 |
| dbtest15           | employees                                            |    0 |
| dbtest2            | book                                                 |    0 |
| dbtest2            | book1                                                |    0 |
| dbtest2            | book2                                                |    0 |
| dbtest2            | book3                                                |    0 |
| dbtest2            | book5                                                |    0 |
| dbtest2            | book6                                                |    0 |
| dbtest2            | book7                                                |    0 |
| dbtest2            | test2                                                |    0 |
| dbtest2            | ts1                                                  |    0 |
| sys                | sys_config                                           |    0 |
| mysql              | dd_properties                                        | NULL |
| mysql              | schemata                                             | NULL |
| mysql              | tablespace_files                                     | NULL |
| mysql              | tablespaces                                          | NULL |
| mysql              | check_constraints                                    | NULL |
| mysql              | column_type_elements                                 | NULL |
| mysql              | columns                                              | NULL |
| mysql              | foreign_key_column_usage                             | NULL |
| mysql              | foreign_keys                                         | NULL |
| mysql              | index_column_usage                                   | NULL |
| mysql              | index_partitions                                     | NULL |
| mysql              | indexes                                              | NULL |
| mysql              | table_partition_values                               | NULL |
| mysql              | table_partitions                                     | NULL |
| mysql              | tables                                               | NULL |
| mysql              | triggers                                             | NULL |
| mysql              | view_routine_usage                                   | NULL |
| mysql              | view_table_usage                                     | NULL |
| mysql              | collations                                           | NULL |
| mysql              | character_sets                                       | NULL |
| mysql              | plugin                                               | NULL |
| mysql              | server_cost                                          | NULL |
| mysql              | engine_cost                                          | NULL |
| mysql              | resource_groups                                      | NULL |
| mysql              | gtid_executed                                        | NULL |
| mysql              | component                                            | NULL |
| mysql              | user                                                 | NULL |
| mysql              | db                                                   | NULL |
| mysql              | tables_priv                                          | NULL |
| mysql              | columns_priv                                         | NULL |
| mysql              | procs_priv                                           | NULL |
| mysql              | proxies_priv                                         | NULL |
| mysql              | role_edges                                           | NULL |
| mysql              | default_roles                                        | NULL |
| mysql              | global_grants                                        | NULL |
| mysql              | password_history                                     | NULL |
| mysql              | time_zone_leap_second                                | NULL |
| mysql              | time_zone_name                                       | NULL |
| mysql              | time_zone                                            | NULL |
| mysql              | time_zone_transition_type                            | NULL |
| mysql              | time_zone_transition                                 | NULL |
| mysql              | servers                                              | NULL |
| mysql              | func                                                 | NULL |
| mysql              | slave_master_info                                    | NULL |
| mysql              | slave_relay_log_info                                 | NULL |
| mysql              | slave_worker_info                                    | NULL |
| mysql              | events                                               | NULL |
| mysql              | catalogs                                             | NULL |
| mysql              | table_stats                                          | NULL |
| mysql              | column_statistics                                    | NULL |
| mysql              | parameters                                           | NULL |
| mysql              | routines                                             | NULL |
| mysql              | st_spatial_reference_systems                         | NULL |
| mysql              | index_stats                                          | NULL |
| mysql              | help_category                                        | NULL |
| mysql              | help_keyword                                         | NULL |
| mysql              | help_relation                                        | NULL |
| mysql              | help_topic                                           | NULL |
| mysql              | innodb_index_stats                                   | NULL |
| mysql              | innodb_table_stats                                   | NULL |
| mysql              | replication_asynchronous_connection_failover         | NULL |
| mysql              | replication_asynchronous_connection_failover_managed | NULL |
| mysql              | replication_group_configuration_version              | NULL |
| mysql              | replication_group_member_actions                     | NULL |
| performance_schema | accounts                                             | NULL |
| performance_schema | binary_log_transaction_compression_stats             | NULL |
| performance_schema | cond_instances                                       | NULL |
| performance_schema | data_lock_waits                                      | NULL |
| performance_schema | data_locks                                           | NULL |
| performance_schema | error_log                                            | NULL |
| performance_schema | events_errors_summary_by_account_by_error            | NULL |
| performance_schema | events_errors_summary_by_host_by_error               | NULL |
| performance_schema | events_errors_summary_by_thread_by_error             | NULL |
| performance_schema | events_errors_summary_by_user_by_error               | NULL |
| performance_schema | events_errors_summary_global_by_error                | NULL |
| performance_schema | events_stages_current                                | NULL |
| performance_schema | events_stages_history                                | NULL |
| performance_schema | events_stages_history_long                           | NULL |
| performance_schema | events_stages_summary_by_account_by_event_name       | NULL |
| performance_schema | events_stages_summary_by_host_by_event_name          | NULL |
| performance_schema | events_stages_summary_by_thread_by_event_name        | NULL |
| performance_schema | events_stages_summary_by_user_by_event_name          | NULL |
| performance_schema | events_stages_summary_global_by_event_name           | NULL |
| performance_schema | events_statements_current                            | NULL |
| performance_schema | events_statements_histogram_by_digest                | NULL |
| performance_schema | events_statements_histogram_global                   | NULL |
| performance_schema | events_statements_history                            | NULL |
| performance_schema | events_statements_history_long                       | NULL |
| performance_schema | events_statements_summary_by_account_by_event_name   | NULL |
| performance_schema | events_statements_summary_by_digest                  | NULL |
| performance_schema | events_statements_summary_by_host_by_event_name      | NULL |
| performance_schema | events_statements_summary_by_program                 | NULL |
| performance_schema | events_statements_summary_by_thread_by_event_name    | NULL |
| performance_schema | events_statements_summary_by_user_by_event_name      | NULL |
| performance_schema | events_statements_summary_global_by_event_name       | NULL |
| performance_schema | events_transactions_current                          | NULL |
| performance_schema | events_transactions_history                          | NULL |
| performance_schema | events_transactions_history_long                     | NULL |
| performance_schema | events_transactions_summary_by_account_by_event_name | NULL |
| performance_schema | events_transactions_summary_by_host_by_event_name    | NULL |
| performance_schema | events_transactions_summary_by_thread_by_event_name  | NULL |
| performance_schema | events_transactions_summary_by_user_by_event_name    | NULL |
| performance_schema | events_transactions_summary_global_by_event_name     | NULL |
| performance_schema | events_waits_current                                 | NULL |
| performance_schema | events_waits_history                                 | NULL |
| performance_schema | events_waits_history_long                            | NULL |
| performance_schema | events_waits_summary_by_account_by_event_name        | NULL |
| performance_schema | events_waits_summary_by_host_by_event_name           | NULL |
| performance_schema | events_waits_summary_by_instance                     | NULL |
| performance_schema | events_waits_summary_by_thread_by_event_name         | NULL |
| performance_schema | events_waits_summary_by_user_by_event_name           | NULL |
| performance_schema | events_waits_summary_global_by_event_name            | NULL |
| performance_schema | file_instances                                       | NULL |
| performance_schema | file_summary_by_event_name                           | NULL |
| performance_schema | file_summary_by_instance                             | NULL |
| performance_schema | global_status                                        | NULL |
| performance_schema | global_variables                                     | NULL |
| performance_schema | host_cache                                           | NULL |
| performance_schema | hosts                                                | NULL |
| performance_schema | keyring_component_status                             | NULL |
| performance_schema | keyring_keys                                         | NULL |
| performance_schema | log_status                                           | NULL |
| performance_schema | memory_summary_by_account_by_event_name              | NULL |
| performance_schema | memory_summary_by_host_by_event_name                 | NULL |
| performance_schema | memory_summary_by_thread_by_event_name               | NULL |
| performance_schema | memory_summary_by_user_by_event_name                 | NULL |
| performance_schema | memory_summary_global_by_event_name                  | NULL |
| performance_schema | metadata_locks                                       | NULL |
| performance_schema | mutex_instances                                      | NULL |
| performance_schema | objects_summary_global_by_type                       | NULL |
| performance_schema | performance_timers                                   | NULL |
| performance_schema | persisted_variables                                  | NULL |
| performance_schema | prepared_statements_instances                        | NULL |
| performance_schema | processlist                                          | NULL |
| performance_schema | replication_applier_configuration                    | NULL |
| performance_schema | replication_applier_filters                          | NULL |
| performance_schema | replication_applier_global_filters                   | NULL |
| performance_schema | replication_applier_status                           | NULL |
| performance_schema | replication_applier_status_by_coordinator            | NULL |
| performance_schema | replication_applier_status_by_worker                 | NULL |
| performance_schema | replication_asynchronous_connection_failover         | NULL |
| performance_schema | replication_asynchronous_connection_failover_managed | NULL |
| performance_schema | replication_connection_configuration                 | NULL |
| performance_schema | replication_connection_status                        | NULL |
| performance_schema | replication_group_member_stats                       | NULL |
| performance_schema | replication_group_members                            | NULL |
| performance_schema | rwlock_instances                                     | NULL |
| performance_schema | session_account_connect_attrs                        | NULL |
| performance_schema | session_connect_attrs                                | NULL |
| performance_schema | session_status                                       | NULL |
| performance_schema | session_variables                                    | NULL |
| performance_schema | setup_actors                                         | NULL |
| performance_schema | setup_consumers                                      | NULL |
| performance_schema | setup_instruments                                    | NULL |
| performance_schema | setup_objects                                        | NULL |
| performance_schema | setup_threads                                        | NULL |
| performance_schema | socket_instances                                     | NULL |
| performance_schema | socket_summary_by_event_name                         | NULL |
| performance_schema | socket_summary_by_instance                           | NULL |
| performance_schema | status_by_account                                    | NULL |
| performance_schema | status_by_host                                       | NULL |
| performance_schema | status_by_thread                                     | NULL |
| performance_schema | status_by_user                                       | NULL |
| performance_schema | table_handles                                        | NULL |
| performance_schema | table_io_waits_summary_by_index_usage                | NULL |
| performance_schema | table_io_waits_summary_by_table                      | NULL |
| performance_schema | table_lock_waits_summary_by_table                    | NULL |
| performance_schema | threads                                              | NULL |
| performance_schema | tls_channel_status                                   | NULL |
| performance_schema | user_defined_functions                               | NULL |
| performance_schema | user_variables_by_thread                             | NULL |
| performance_schema | users                                                | NULL |
| performance_schema | variables_by_thread                                  | NULL |
| performance_schema | variables_info                                       | NULL |
| mysql              | parameter_type_elements                              | NULL |
+--------------------+------------------------------------------------------+------+
215 rows in set (0.01 sec)
~~~





~~~mysql
# 2. 查询占用bufferpool较多的表
select object_schema,object_name,allocated,data
from sys.innodb_buffer_stats_by_table order by allocated limit 10;

+---------------+--------------------+------------+------------+
| object_schema | object_name        | allocated  | data       |
+---------------+--------------------+------------+------------+
| mysql         | catalogs           | 16.00 KiB  |   17 bytes |
| mysql         | character_sets     | 16.00 KiB  | 2.58 KiB   |
| mysql         | check_constraints  | 16.00 KiB  |    0 bytes |
| mysql         | index_stats        | 16.00 KiB  | 3.16 KiB   |
| mysql         | innodb_table_stats | 16.00 KiB  | 2.34 KiB   |
| mysql         | table_partitions   | 16.00 KiB  |    0 bytes |
| mysql         | triggers           | 16.00 KiB  |   54 bytes |
| mysql         | view_routine_usage | 16.00 KiB  |  930 bytes |
| mysql         | view_table_usage   | 16.00 KiB  | 13.16 KiB  |
| mysql         | tables             | 208.00 KiB | 127.37 KiB |
+---------------+--------------------+------------+------------+
10 rows in set (0.00 sec)
~~~





~~~mysql
# 3. 查看表的全表扫描情况
select * from sys.statements_with_full_table_scans where db='dbname';

Empty set (0.01 sec)
~~~





**语句相关**

~~~mysql
#1. 监控SQL执行的频率
select db,exec_count,query from sys.statement_analysis
order by exec_count desc;

+------------+------------+-------------------------------------------------------------------+
| db         | exec_count | query                                                             |
+------------+------------+-------------------------------------------------------------------+
| NULL       |         19 | SELECT ( `cat` . `name` COLLAT ... te_data` , COALESCE ( `stat` . |
| NULL       |         10 | SELECT ( `cat` . `name` COLLAT ... ` AS `DATABASE_COLLATION` FROM |
| atguigudb1 |          9 | SELECT ( `cat` . `name` COLLAT ... den` ) ) , COALESCE ( `stat` . |
| NULL       |          6 | SELECT ( `cat` . `name` COLLAT ... den` ) ) , COALESCE ( `stat` . |
| atguigudb1 |          6 | SET `PROFILING` = ?                                               |
| atguigudb1 |          6 | SET `profiling_history_size` = ?                                  |
| atguigudb1 |          6 | SHOW STATUS                                                       |
| NULL       |          5 | SELECT ( `cat` . `name` COLLAT ... ) AS `NUMERIC_SCALE` , `col` . |
| NULL       |          5 | WITH RECURSIVE `role_graph` (  ... AS `INTERNAL_GET_USERNAME()` , |
| NULL       |          5 | SELECT `t` . `THREAD_ID` AS `t ... T_NUMBER_OF_BYTES_USED` ) DESC |
| NULL       |          4 | SELECT ( `cat` . `name` COLLAT ... database` ( `sch` . `name` ) ) |
| NULL       |          4 | SELECT COUNT (?) AS `cnt` , `r ... y_by_digest` GROUP BY `avg_us` |
| NULL       |          3 | SET @@SESSION . `wait_timeout` = ?                                |
| NULL       |          3 | SELECT ( `cat` . `name` COLLAT ... ERED` , `convert_tz` ( `evt` . |
| NULL       |          3 | SELECT IF ( ( `performance_sch ... by_event_name` GROUP BY IF ( ( |
| NULL       |          3 | SELECT `extract_schema_from_fi ... , SUM ( `performance_schema` . |
| NULL       |          3 | SELECT IF ( ( `locate` ( ? , ` ... ` . `COMPRESSED_SIZE` ) ) DESC |
| NULL       |          3 | SELECT IF ( ( `performance_sch ... by_event_name` GROUP BY IF ( ( |
| NULL       |          3 | SHOW STATUS                                                       |
| atguigudb1 |          3 | SHOW PROFILES                                                     |
| atguigudb1 |          3 | SELECT `state` , `round` ( SUM ... duration (summed) in sec` DESC |
| NULL       |          2 | SELECT `mysql` . `st_spatial_r ... `st_spatial_reference_systems` |
| NULL       |          2 | SELECT `r` . `trx_wait_started ... ` , `r` . `trx_rows_locked` AS |
| NULL       |          2 | SELECT `substring_index` ( `pe ... ? ) AND ( `performance_schema` |
| NULL       |          2 | SELECT SCHEMA ( )                                                 |
| atguigudb1 |          2 | SHOW WARNINGS                                                     |
| atguigudb1 |          2 | SELECT * FROM `student` WHERE `id` < ?                            |
| NULL       |          2 | SET `PROFILING` = ?                                               |
| NULL       |          2 | SET `profiling_history_size` = ?                                  |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `countries`                       |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `departments`                     |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `employees`                       |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `job_grades`                      |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `job_history`                     |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `jobs`                            |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `locations`                       |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `order`                           |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb` . `regions`                         |
| NULL       |          1 | SELECT `e` . `employee_id` AS  ... ` = `c` . `country_id` ) AND ( |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `course`                         |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `s1`                             |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `s2`                             |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `student`                        |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `student_info`                   |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `t`                              |
| NULL       |          1 | SHOW CREATE TABLE `atguigudb1` . `tt`                             |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_disk`                     |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_file`                     |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_file_history`             |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_file_share`               |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_folder`                   |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_group`                    |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_group_user`               |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `cloud_log`                      |
| NULL       |          1 | SHOW CREATE TABLE `cloud_disk` . `portal_seq_gen`                 |
| NULL       |          1 | SHOW CREATE TABLE `dbtest15` . `departments`                      |
| NULL       |          1 | SHOW CREATE TABLE `dbtest15` . `employees`                        |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book`                              |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book1`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book2`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book3`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book5`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book6`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `book7`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `test2`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `test4`                             |
| NULL       |          1 | SHOW CREATE TABLE `dbtest2` . `ts1`                               |
| NULL       |          1 | SELECT `cs` . `name` AS `CHARA ... ation_id` = `col` . `id` ) ) ) |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... ject` ( `tbl` . `hidden` ) ) ) |
| NULL       |          1 | SELECT `col` . `name` AS `COLL ... ol` . `character_set_id` ) ) ) |
| NULL       |          1 | SELECT `col` . `name` AS `COLL ... er_set_id` = `cs` . `id` ) ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `COLUMN_PRIVILEGES`      |
| NULL       |          1 | SELECT `mysql` . `column_stati ... statistics` . `table_name` ) ) |
| NULL       |          1 | SELECT `cat` . `name` AS `TABL ...  `hidden` NOT IN (...) ) ) ) ) |
| NULL       |          1 | SELECT `current_user_enabled_r ... ) `current_user_enabled_roles` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `ENGINES`                |
| NULL       |          1 | SELECT `internal_tablespace_id ... _ROWS` , ? AS `UPDATE_COUNT` , |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_BUFFER_PAGE`     |
| NULL       |          1 | SHOW CREATE TABLE `information ... ma` . `INNODB_BUFFER_PAGE_LRU` |
| NULL       |          1 | SHOW CREATE TABLE `information ... ` . `INNODB_BUFFER_POOL_STATS` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CACHED_INDEXES`  |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CMP`             |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CMP_PER_INDEX`   |
| NULL       |          1 | SHOW CREATE TABLE `information ... . `INNODB_CMP_PER_INDEX_RESET` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CMP_RESET`       |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CMPMEM`          |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_CMPMEM_RESET`    |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_COLUMNS`         |
| NULL       |          1 | SELECT `get_dd_tablespace_priv ... ) AND ( `ts` . `name` != ? ) ) |
| NULL       |          1 | SELECT `get_dd_index_private_d ... AND ( `tbl` . `engine` = ? ) ) |
| NULL       |          1 | SELECT ( `concat` ( `sch` . `n ... ` = ? ) ) GROUP BY `fk` . `id` |
| NULL       |          1 | SELECT ( `concat` ( `sch` . `n ... AND ( `tbl` . `engine` = ? ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information ... a` . `INNODB_FT_BEING_DELETED` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_FT_CONFIG`       |
| NULL       |          1 | SHOW CREATE TABLE `information ... . `INNODB_FT_DEFAULT_STOPWORD` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_FT_DELETED`      |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_FT_INDEX_CACHE`  |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_FT_INDEX_TABLE`  |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_INDEXES`         |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_METRICS`         |
| NULL       |          1 | SHOW CREATE TABLE `information ... NODB_SESSION_TEMP_TABLESPACES` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_TABLES`          |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_TABLESPACES`     |
| NULL       |          1 | SELECT `get_dd_tablespace_priv ... ) AND ( `ts` . `name` != ? ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_TABLESTATS`      |
| NULL       |          1 | SHOW CREATE TABLE `information ... ma` . `INNODB_TEMP_TABLE_INFO` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_TRX`             |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `INNODB_VIRTUAL`         |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ...  ) ) ) JOIN ( LATERAL ( SELECT |
| NULL       |          1 | SELECT `j` . `word` AS `WORD`  ... served` INTEGER PATH ? ) ) `j` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `OPTIMIZER_TRACE`        |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... ULL ) , ? , `ifnull` ( `prm` . |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... `tbl` . `engine` , ? ) , `tbl` |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `PLUGINS`                |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `PROCESSLIST`            |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `PROFILING`              |
| NULL       |          1 | SELECT `cat` . `name` AS `CONS ... ject` ( `tbl` . `hidden` ) ) ) |
| NULL       |          1 | SELECT `res` . `resource_group ... s` . `resource_group_name` ) ) |
| NULL       |          1 | SELECT `rtn` . `name` AS `SPEC ... ( `cs_result` . `name` = ? ) , |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `SCHEMA_PRIVILEGES`      |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... database` ( `sch` . `name` ) ) |
| NULL       |          1 | SELECT `st_units_of_measure` . ... TH ? ) ) `st_units_of_measure` |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ...  ) AS `CONSTRAINT_NAME` , ? AS |
| NULL       |          1 | SELECT `cat` . `name` AS `CONS ... ject` ( `tbl` . `hidden` ) ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `TABLE_PRIVILEGES`       |
| NULL       |          1 | SELECT `cat` . `name` AS `TABL ... ject` ( `tbl` . `hidden` ) ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `TABLESPACES`            |
| NULL       |          1 | SELECT `tsps` . `name` AS `TAB ... `mysql` . `tablespaces` `tsps` |
| NULL       |          1 | SELECT `mysql` . `user` . `Use ...  `mysql` . `user` . `Host` ) ) |
| NULL       |          1 | SHOW CREATE TABLE `information_schema` . `USER_PRIVILEGES`        |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... me` , `rtn` . `type` , `rtn` . |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... iner` , `vw` . `options` ) ) ) |
| NULL       |          1 | SELECT ( `cat` . `name` COLLAT ... ction_collation_id` ) ) ) JOIN |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `columns_priv`                        |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `component`                           |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `db`                                  |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `default_roles`                       |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `e ... arset) ? THEN ? ELSE ? END ) ) |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `func`                                |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `general_log`                         |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `global_grants`                       |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `gtid_executed`                       |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `help_category`                       |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `help_keyword`                        |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `help_relation`                       |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `help_topic`                          |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `innodb_index_stats`                  |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `innodb_table_stats`                  |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `password_history`                    |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `plugin`                              |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `procs_priv`                          |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `proxies_priv`                        |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `r ... nchronous_connection_failover` |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `r ... s_connection_failover_managed` |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `r ... n_group_configuration_version` |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `replication_group_member_actions`    |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `role_edges`                          |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `s ... arset) ? THEN ? ELSE ? END ) ) |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `servers`                             |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `slave_master_info`                   |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `slave_relay_log_info`                |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `slave_worker_info`                   |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `slow_log`                            |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `tables_priv`                         |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `time_zone`                           |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `time_zone_leap_second`               |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `time_zone_name`                      |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `time_zone_transition`                |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `time_zone_transition_type`           |
| NULL       |          1 | SHOW CREATE TABLE `mysql` . `user`                                |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `accounts`               |
| NULL       |          1 | SHOW CREATE TABLE `performance ... transaction_compression_stats` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `cond_instances`         |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `data_lock_waits`        |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `data_locks`             |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `error_log`              |
| NULL       |          1 | SHOW CREATE TABLE `performance ... s_summary_by_account_by_error` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... rors_summary_by_host_by_error` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... rs_summary_by_thread_by_error` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... rors_summary_by_user_by_error` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... rrors_summary_global_by_error` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `events_stages_current`  |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `events_stages_history`  |
| NULL       |          1 | SHOW CREATE TABLE `performance ... . `events_stages_history_long` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mary_by_account_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_host_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mmary_by_thread_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_user_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _summary_global_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  . `events_statements_current` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... tatements_histogram_by_digest` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... s_statements_histogram_global` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  . `events_statements_history` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... vents_statements_history_long` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mary_by_account_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _statements_summary_by_digest` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_host_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... statements_summary_by_program` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mmary_by_thread_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_user_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _summary_global_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  `events_transactions_current` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  `events_transactions_history` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... nts_transactions_history_long` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mary_by_account_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_host_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mmary_by_thread_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_user_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _summary_global_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `events_waits_current`   |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `events_waits_history`   |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  . `events_waits_history_long` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mary_by_account_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_host_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... nts_waits_summary_by_instance` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mmary_by_thread_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_user_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _summary_global_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `file_instances`         |
| NULL       |          1 | SHOW CREATE TABLE `performance ... . `file_summary_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ` . `file_summary_by_instance` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `global_status`          |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `global_variables`       |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `host_cache`             |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `hosts`                  |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ` . `keyring_component_status` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `keyring_keys`           |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `log_status`             |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mary_by_account_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_host_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... mmary_by_thread_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... summary_by_user_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _summary_global_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `metadata_locks`         |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `mutex_instances`        |
| NULL       |          1 | SHOW CREATE TABLE `performance ... bjects_summary_global_by_type` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `performance_timers`     |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `persisted_variables`    |
| NULL       |          1 | SHOW CREATE TABLE `performance ... prepared_statements_instances` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `processlist`            |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ication_applier_configuration` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  `replication_applier_filters` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... cation_applier_global_filters` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... . `replication_applier_status` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... applier_status_by_coordinator` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... tion_applier_status_by_worker` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... nchronous_connection_failover` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... s_connection_failover_managed` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... tion_connection_configuration` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... replication_connection_status` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... eplication_group_member_stats` |
| NULL       |          1 | SHOW CREATE TABLE `performance ...  . `replication_group_members` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `rwlock_instances`       |
| NULL       |          1 | SHOW CREATE TABLE `performance ... session_account_connect_attrs` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `session_connect_attrs`  |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `session_status`         |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `session_variables`      |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `setup_actors`           |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `setup_consumers`        |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `setup_instruments`      |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `setup_objects`          |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `setup_threads`          |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `socket_instances`       |
| NULL       |          1 | SHOW CREATE TABLE `performance ... `socket_summary_by_event_name` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... . `socket_summary_by_instance` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `status_by_account`      |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `status_by_host`         |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `status_by_thread`       |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `status_by_user`         |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `table_handles`          |
| NULL       |          1 | SHOW CREATE TABLE `performance ... _waits_summary_by_index_usage` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ble_io_waits_summary_by_table` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... e_lock_waits_summary_by_table` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `threads`                |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `tls_channel_status`     |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ma` . `user_defined_functions` |
| NULL       |          1 | SHOW CREATE TABLE `performance ... ` . `user_variables_by_thread` |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `users`                  |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `variables_by_thread`    |
| NULL       |          1 | SHOW CREATE TABLE `performance_schema` . `variables_info`         |
| NULL       |          1 | SHOW CREATE TABLE `sys` . `sys_config`                            |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ame` . `SUM_TIMER_WAIT` ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ost_by_event_name` . `HOST` IS |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... mmary_by_host_by_event_name` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... , SUM ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... sent` , `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `locate` ( ? , ` ... ` . `COMPRESSED_SIZE` ) ) DESC |
| NULL       |          1 | SELECT IF ( ( `locate` ( ? , ` ... ` . `COMPRESSED_SIZE` ) ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ency` , `performance_schema` . |
| NULL       |          1 | SELECT `sys` . `format_path` ( ... vg_write` , `format_bytes` ( ( |
| NULL       |          1 | SELECT `sys` . `format_path` ( ... RDER BY `performance_schema` . |
| NULL       |          1 | SELECT `substring_index` ( `pe ... ER_OF_BYTES_READ` / `nullif` ( |
| NULL       |          1 | SELECT `substring_index` ( `pe ... count_read` , `format_bytes` ( |
| NULL       |          1 | SELECT IF ( ( `information_sch ... _history_long` . `THREAD_ID` = |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... _NUMBER_OF_BYTES_ALLOC` ) ) AS |
| NULL       |          1 | SELECT `mt` . `THREAD_ID` AS ` ... ST_HOST` USING `utf8mb4` ) ) , |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... _NUMBER_OF_BYTES_ALLOC` ) ) AS |
| NULL       |          1 | SELECT `performance_schema` .  ... llif` ( `performance_schema` . |
| NULL       |          1 | SELECT `format_bytes` ( SUM (  ... _summary_global_by_event_name` |
| NULL       |          1 | SELECT `lower` ( `performance_ ... ) AND ( `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... us` . `VARIABLE_VALUE` > ? ) ) |
| NULL       |          1 | SELECT `performance_schema` .  ... PDATE` ) AS `update_latency` , |
| NULL       |          1 | SELECT `g` . `OBJECT_SCHEMA` A ... , `gt` . `PROCESSLIST_ID` ) AS |
| NULL       |          1 | SELECT `performance_schema` .  ... dex_usage` . `COUNT_READ` DESC |
| NULL       |          1 | SELECT `sslver` . `THREAD_ID`  ... slver` . `VARIABLE_NAME` = ? ) |
| NULL       |          1 | SELECT `sys` . `format_stateme ... atency` , `format_pico_time` ( |
| NULL       |          1 | SELECT `sys` . `format_stateme ... tatements_summary_by_digest` . |
| NULL       |          1 | SELECT `sys` . `format_stateme ... y_digest` . `SUM_ROWS_SENT` AS |
| NULL       |          1 | SELECT `sys` . `format_stateme ... RANGE` AS `sort_using_range` , |
| NULL       |          1 | SELECT `sys` . `format_stateme ... ll` ( ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ame` . `SUM_TIMER_WAIT` ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ent_name` . `USER` IS NULL ) , |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... mmary_by_user_by_event_name` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... , SUM ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... sent` , `performance_schema` . |
| NULL       |          1 | SELECT ? AS `sys_version` , `version` ( ) AS `mysql_version`      |
| NULL       |          1 | SELECT `substring_index` ( `pe ... lobal_by_event_name` WHERE ( ( |
| NULL       |          1 | SELECT `substring_index` ( `pe ... ERE ( ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ) AND ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ) AND ( `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... _name` . `SUM_TIMER_WAIT` DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ame` . `SUM_TIMER_WAIT` ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ) , ? , `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ST` ) , `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... me` . `SUM_ROWS_AFFECTED` ) AS |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... EXAMINED` AS `rows_examined` , |
| NULL       |          1 | SELECT IF ( ( `locate` ( ? , ` ... ` . `COMPRESSED_SIZE` ) ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... d_id` , `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... . `file_summary_by_instance` . |
| NULL       |          1 | SELECT `performance_schema` .  ... tance` . `SUM_TIMER_WAIT` DESC |
| NULL       |          1 | SELECT `substring_index` ( `pe ... read` , `performance_schema` . |
| NULL       |          1 | SELECT `substring_index` ( `pe ... ll` ( ( `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `information_sch ...  . `THREAD_ID` ) ) ) LEFT JOIN |
| NULL       |          1 | SELECT `performance_schema` .  ... H_COUNT_USED` , ? ) ) , ? ) AS |
| NULL       |          1 | SELECT SUM ( `performance_sche ... _summary_global_by_event_name` |
| NULL       |          1 | SELECT `performance_schema` .  ... _index_usage` . `COUNT_DELETE` |
| NULL       |          1 | SELECT `g` . `OBJECT_SCHEMA` A ... cking_connection` FROM ( ( ( ( |
| NULL       |          1 | SELECT `performance_schema` .  ... dex_usage` . `COUNT_READ` DESC |
| NULL       |          1 | SELECT `performance_schema` .  ... TIMER_WAIT` AS `avg_latency` , |
| NULL       |          1 | SELECT `performance_schema` .  ... `FIRST_SEEN` AS `first_seen` , |
| NULL       |          1 | SELECT `performance_schema` .  ... sent` , `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... ange` , `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... _TMP_DISK_TABLES` / `nullif` ( |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ame` . `SUM_TIMER_WAIT` ) DESC |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ) , ? , `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ER` ) , `performance_schema` . |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... me` . `SUM_ROWS_AFFECTED` ) AS |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... EXAMINED` AS `rows_examined` , |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... BY IF ( ( `performance_schema` |
| NULL       |          1 | SELECT IF ( ( `performance_sch ... ) AND ( `performance_schema` . |
| NULL       |          1 | SELECT `performance_schema` .  ... _name` . `SUM_TIMER_WAIT` DESC |
| NULL       |          1 | SELECT `db` , NAME , TYPE , `param_list` FROM `mysql` . `proc`    |
| NULL       |          1 | SELECT @@`version_comment` LIMIT ?                                |
| atguigudb1 |          1 | EXPLAIN FORMAT = `tree` SELECT ... HERE `s1` . `common_field` = ? |
| atguigudb1 |          1 | EXPLAIN SELECT `s1` . `key1` , ... ` . `common_field` IS NOT NULL |
| atguigudb1 |          1 | EXPLAIN SELECT * FROM `s1` WHE ... s2` WHERE `common_field` = ? ) |
| atguigudb1 |          1 | SET `optimizer_trace` = ? , `end_markers_in_json` = ON            |
| atguigudb1 |          1 | SET `optimizer_trace_max_mem_size` = ?                            |
| atguigudb1 |          1 | SELECT * FROM `information_schema` . `optimizer_trace`            |
| atguigudb1 |          1 | SELECT `performance_schema` .  ... PDATE` ) AS `update_latency` , |
| NULL       |          1 | SHOW VARIABLES LIKE ? (unknown ... n_variables` . `VARIABLE_NAME` |
| NULL       |          1 | SHOW PROFILES                                                     |
| NULL       |          1 | SELECT `state` , `round` ( SUM ... duration (summed) in sec` DESC |
| NULL       |          1 | USE `atguigudb1`                                                  |
| atguigudb1 |          1 | USE `atguigudb1`                                                  |
| atguigudb1 |          1 | SELECT SCHEMA ( )                                                 |
| atguigudb1 |          1 | SELECT `extract_schema_from_fi ... , SUM ( `performance_schema` . |
| atguigudb1 |          1 | SELECT IF ( ( `locate` ( ? , ` ... ` . `COMPRESSED_SIZE` ) ) DESC |
| atguigudb1 |          1 | SELECT `sys` . `format_stateme ... y_digest` . `SUM_ROWS_SENT` AS |
+------------+------------+-------------------------------------------------------------------+
369 rows in set (0.02 sec)
~~~



~~~mysql
#2. 监控使用了排序的SQL
select db,exec_count,first_seen,last_seen,query
from sys.statements_with_sorting limit 1;

+------------+------------+----------------------------+----------------------------+-------------------------------------------------------------------+
| db         | exec_count | first_seen                 | last_seen                  | query                                                             |
+------------+------------+----------------------------+----------------------------+-------------------------------------------------------------------+
| atguigudb1 |          9 | 2022-10-06 22:52:27.615292 | 2022-10-06 22:56:39.188806 | SELECT ( `cat` . `name` COLLAT ... den` ) ) , COALESCE ( `stat` . |
+------------+------------+----------------------------+----------------------------+-------------------------------------------------------------------+
1 row in set (0.00 sec)
~~~



**IO相关**

~~~mysql
#1. 查看消耗磁盘IO的文件
select file,avg_read,avg_write,avg_read+avg_write as avg_io
from sys.io_global_by_file_by_bytes order by avg_read limit 10;

+-------------------------------------+------------+------------+--------+
| file                                | avg_read   | avg_write  | avg_io |
+-------------------------------------+------------+------------+--------+
| @@datadir\#innodb_temp\temp_9.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\#innodb_temp\temp_5.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\mysql\slow_log.CSV        |    0 bytes |    0 bytes |      0 |
| @@datadir\#innodb_temp\temp_4.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\#innodb_temp\temp_3.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\#innodb_temp\temp_1.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\cloud_disk\cloud_disk.ibd |    0 bytes |    0 bytes |      0 |
| @@datadir\#innodb_temp\temp_2.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\#innodb_temp\temp_6.ibt   |    0 bytes | 48.00 KiB  |     48 |
| @@datadir\#innodb_temp\temp_7.ibt   |    0 bytes | 48.00 KiB  |     48 |
+-------------------------------------+------------+------------+--------+
10 rows in set, 20 warnings (0.01 sec)
~~~





**Innodb 相关**

~~~mysql
-- 1. 行锁阻塞情况
select * from sys.innodb_lock_waits;

Empty set (0.00 sec)
~~~





> 风险提示：
>
> 通过sys库去查询时，MySOL会消耗大量资源 去收集相关信息，严重的可能会导致业务请求被阻塞，从而引起故障、建议生产上不要频繁的去查询sys或者performance_schema.iniormation schema来完成监控、巡检等工作



























































