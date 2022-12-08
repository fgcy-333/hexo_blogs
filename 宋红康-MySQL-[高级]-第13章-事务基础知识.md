---
title: 第13章_事务基础知识
date: 2022-10-07 10:15:13
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

# 1. 数据库事务概述 

事务 是 **数据库** 区别于 **文件系统** 的重要特性之一，当我们有了事务就会让数据库始终保持 **一致性**，同时我们还能通讨事务的机制 **恢复到某个时间点**，这样可以保证已提交到数据库的修改不会因为系统崩溃而丢失



<!--more-->

## 1.1 存储引擎支持情况 



`SHOW ENGINES `命令来查看当前 MySQL 支持的存储引擎都有哪些，以及这些存储引擎是否支持事务。

~~~mysql
show engines;

+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
~~~

能看出在 MySQL 中，只有InnoDB 是支持事务的。 





## 1.2 基本概念 

事务：一组逻辑操作单元，使数据从一种状态变换到另一种状态。 



事务处理的原则：保证所有事务都作为 一个工作单元 来执行，即使出现了故障，都不能改变这种执行方式。

当在一个事务中执行多个操作时，要么所有的事务都被提交( commit )，那么这些修改就 永久 地保 存下来；

要么数据库管理系统将 放弃 所作的所有 修改 ，整个事务回滚( rollback )到最初状态。 



案例：AA用户给BB用户转账100

~~~mysql
update account set money = money-100 where name ='AA'
-- --------------
-- 服务器宕机
-- --------------
update account set money = money + 100 where name ='BB'
~~~





## 1.3 事务的ACID特性 

- 原子性（atomicity）： 

​		原子性是指事务是一个不可分割的工作单位，要么全部提交，要么全部失败回滚。即要么转账成功，要么转账失败，是不存在中间的状态。

​		如果无法保证原子性会怎么样？就会出现数据不一致的情形，A账户减去100元，而B账户增加100元操作失败，系统将无故丢失100元。



-  一致性（consistency）： 

​	（国内很多网站上对一致性的阐述有误，具体你可以参考 Wikipedia 对Consistency的阐述） 

​		根据定义，一致性是指事务执行前后，数据从一个` 合 法性状态 `变换到另外一个` 合法性状态 `。这种状态 是 语义上 的而不是语法上的，跟具体的业务有关。

​		 那什么是合法的数据状态呢？满足 预定的约束 的状态就叫做合法的状态。通俗一点，这状态是由你自己 来定义的（比如满足现实世界中的约束）。

​		满足这个状态，数据就是一致的，不满足这个状态，数据就 是不一致的！如果事务中的某个操作失败了，系统就会自动撤销当前正在执行的事务，返回到事		务操作 之前的状态。



举例1：

~~~
A账户有200元，转账300元出去，此时A账户余额为-100元。你自然就发现了此时数据是不一致的，为什么呢？因为你定义了一个状态，余额这列必须>=0
~~~



举例2：

~~~
A账户200元，转账50元给B账户，A账户的钱扣了，但是B账户因为各种意外，余额并没有增加。
你也知道此时数据是不一致的，为什么呢？因为你定义了一个状态，要求A+B的总余额必须不变
~~~



举例3：

~~~
在数据表中我们将姓名字段设置为唯一性约束，这时当事务进行提交或者事务发生回滚的时候，如果数据表中的姓名不唯一，就破坏了事务的一致性要求
~~~





-  隔离型（isolation）：

​		事务的隔离性是指一个事务的执行 **不能被其他事务干扰** ，即一个事务内部的操作及使用的数据对 **并发** 的 其他事务是隔离的，并发执行的各个事务之间不能		互相干扰【涉及隔离级别】。 

如果无法保证隔离性会怎么样？假设A账户有200元，B账户0元。A账户往B账户转账两次，每次金额为50 元，分别在两个事务中执行。

如果无法保证隔离性，会出现下面的情形：

~~~mysql
UPDATE accounts SET money = money - 50 WHERE NAME = 'AA';

UPDATE accounts SET money = money + 50 WHERE NAME = 'BB';
~~~



----

![image-20221007115647666](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210071156055.png)

----





- 持久性（durability）： 

​		持久性是指一个事务一旦被提交，它对数据库中数据的改变就是 永久性的 ，接下来的其他操作和数据库 故障不应该对其有任何影响。 

​		持久性是通过 事务日志 来保证的。日志包括了 重做日志 和 回滚日志 。当我们通过事务对数据进行修改 的时候，首先会将数据库的变化信息记录到重做日志		中，然后再对数据库中对应的行进行修改。这样做 的好处是，即使数据库系统崩溃，数据库重启后也能找到没有更新到数据库系统中的重做日志，重新执 		行，从而使事务具有持久性。





> 总结
> ACID是事务的四大特性，在这四个特性中，原子性是基础，隔离性是手段，一致性是约束条件，而持久性是我们的目的
> 数据库事务，其实就是数据库设计者为了方便起见，把  需要保证  原子性、隔离性、一致性和持久性的一个**或多个数据库操作称为一个事务**





## 1.4 事务的状态 

我们现在知道 事务 是一个抽象的概念，它其实对应着一个或多个数据库操作，MySQL根据这些操作所执 行的不同阶段把 事务 大致划分成几个状态： 

- 活动的（active） 

  事务对应的数据库操作正在执行过程中时，我们就说该事务处在 活动的 状态。 



- 部分提交的（partially committed） 

  当事务中的最后一个操作执行完成，但由于操作都在内存中执行，所造成的影响并 **没有刷新到磁盘** 时，我们就说该事务处在 部分提交的 状态。 



- 失败的（failed） 

  当事务处在 活动的 或者 部分提交的 状态时，可能遇到了某些错误（数据库自身的错误、操作系统 错误或者直接断电等）而无法继续执行，或者人为的停止当前事务的执行，我们就说该事务处在 失 败的 状态。



- 中止的（aborted） 

  如果事务执行了一部分而变为 失败的 状态，那么就需要把已经修改的事务中的操作  **还原到** 事务执 行前的状态。换句话说，就是要撤销失败事务对当前数据库造成的影响。我们把这个撤销的过程称 之为 回滚 。当 回滚 操作执行完毕时，也就是数据库  **恢复到了执行事务之前的状态**，我们就说该事 务处在了 中止的 状态。

  



 举例：

~~~mysql
UPDATE accounts SET money = money - 50 WHERE NAME = 'AA';

UPDATE accounts SET money = money + 50 WHERE NAME = 'BB';
~~~





- 提交的（committed）

   当一个处在 部分提交的 状态的事务将修改过的数据都 **同步到磁盘 上之后**，我们就可以说该事务处 在了 提交的 状态。





一个基本的状态转换图如下所示：

----

![image-20221007121452978](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210071214344.png)

---

> 图中可见，只有当事务处于提交的或者中止的状态时，一个事务的生命周期才算是结束了。
>
> 对于已经提交的事务来说，该事务对数据库所做的修改将永久生效
>
> 对于处于中止状态的事务，该事务对数据库所做的所有修改都会被回滚到没执行该事务之前的状态，





# 2. 如何使用事务

使用事务有两种方式，分别为 显式事务 和 隐式事务 。



使用事务的过程：

```
步骤1：开启事务

步骤2：一系列的DML操作

步骤3：事务结束的状态：提交的状态（COMMIT）中止的状态（ROLLBACK)
```





## 2.1 显式事务 



步骤1：使用关键字 `START TRANSACTION ` 或者 `BEGIN `，作用是显式开启一个事务。

~~~mysql
BEGIN;

-- 或者

START TRANSACTION;
~~~

START TRANSACTION 语句相较于 BEGIN 特别之处在于，后边能跟随几个 修饰符 ：

① `READ ONLY `：标识当前事务是一个 只读事务 ，也就是属于该事务的数据库操作只能读取数据，而不 能修改数据。

> 补充：只读事务中只是不允许修改那些其他事务也能访问到的表中的数据，对于临时表来说（我们使用CREATETMEPORARYTABLE创建的表），由于它们只能在当前会话中可见，所以以只读事务其实也是可以对临时表讲行增，邢，改操作的

② `READ WRITE `：标识当前事务是一个 读写事务 ，也就是属于该事务的数据库操作既可以读取数据， 也可以修改数据。 【默认】

③` WITH CONSISTENT SNAPSHOT `：启动一致性读。 

比如：

~~~mysql
-- 开启一个只读事务
START TRANSACTION READ ONLY;
~~~



~~~mysql
-- 开启只读事务和一致性读
START TRANSACTION READ ONLY，WITH CONSISTENT SNAPSHOT; 
~~~





步骤2：一系列事务中的操作（主要是DML，不含DDL） 



步骤3：提交事务 或 中止事务（即回滚事务）

~~~mysql
-- 提交事务。当提交事务后，对数据库的修改是永久性的
COMMIT;
~~~



~~~mysql
-- 回滚事务。即撤销正在进行的所有没有提交的修改
ROLLBACK;
~~~



~~~mysql
-- 将事务回滚到某个保存点
ROLLBACK TO [SAVEPOINT];
~~~



其中关于`SAVEPOINT`相关操作有:

~~~mysql
-- 在事务中创建保存点，方便后续针对保存点进行回滚。一个事务中可以存在多个保存点。
SAVEPOINT 保存点名称
~~~



~~~mysql
-- 删除某个保存点、
RELEASE SAVEPOINT 保存点名称
~~~







## 2.2 隐式事务

MySQL中有一个系统变量autocommit

关键字  `autocommit` 默认是开启的

~~~mysql
SHOW VARIABLES LIKE 'autocommit';

+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    |
+---------------+-------+
1 row in set, 1 warning (0.01 sec)
~~~



默认情况下，如果我们不显式的使用`START TRANSACTION`或者BEGIN语句开启一个事务，那么每一条语句都算是一个独立的事务，这种特性称之为事务的自动提交。也就是说，不以START TRANSACTION或者BEGIN语句显式的开启一个事务，那么下边这两条语句就相当于放到两个独立的事务中去执行

~~~mysql
UPDATE account SET balance = balanle - 10 WHERE 1d = 1;-- 比时这条DML操作是一个独立的事务

UPDATE account SET balance = balance + 10 WHERE id = 2;-- 比时这条DML操作是一个独立的事务
~~~





关闭这种 自动提交 的功能可以使用下边两种方法之一：

方式一：把系统变量 autocommit 的值设置为 OFF

~~~mysql
SET AUTOCOMMIT = FALSE; -- 针对DML的操作是有效的 对DDL操作是无效的

Query OK, 0 rows affected (0.00 sec)
~~~

~~~mysql
UPDATE account SET balance = balance-10 WHERE id-1;

UPDATE account SET balance = balance + 10 WHERE id =2;

COMMIT; -- 或rol1back；
~~~

这样的话，我们写入的多条语句就算是属于同一个事务了，直到我们显式的写出COMMIT语句来把这个事务提交掉，或者显式的写出ROLLBACK语句来把这个事务回滚掉



> 补充：Oracle 默认不自动提交，需要手写COMMIT命令，而MySQL默认自动提交.



方式2：我们在 `autocommit `为 `true `的情况下，使用 `start transaction `或 `begin`开启事务，那么DML 操作就不会自动提交数据

~~~MYSQL
START TRANSACTION;

UPDATE account sET balance = balance-10 WHERE id = 1;

UPDATE account sET balance = balance + 10 WHERE id = 2;

COMMIT; -- 或rollback;
~~~







## 2.3 隐式提交数据的情况 

- 数据定义语言（Data definition language，缩写为：DDL）

​		数据库对象，指的就是 **数据库**、**表**、**视图**、**存储过程**等结构。

​		当我们使用 `CREATE、ALTER、DROP` 等语句去修改数据库对象时，就会隐式的提交前边语句所属于的事务。即


~~~mysql
BEGIN;

SELECT ... -- 事务中的一条语句
UPDATE...  -- 事务中的一条语句
.... -- 事务中的其它语句

CREATE TABLE..,, -- 此语句会隐式的提交前边语句所属于的事务
~~~





- 隐式使用或修改mysql数据库中的表

​		当我们使用ALTER USER、CREATE USER、DROP USER、GRANT、RENAME USER、REVOKE、SET PASSWORD等语句时也会隐式的提交前边语句所属于的		事务





- 事务控制或关于锁定的语句

① 当我们在一个事务还没提交或者回滚时就又使用 START TRANSACTION 或者 BEGIN 语句开启了 另一个事务时，会 隐式的提交 上一个事务。即：

~~~mysql
BEGIN ;

SELECT..... -- 事务中的一条语句
UPDATE.... -- 事务中的一条语句
...... -- 事务中的其它语句
BEGIN; -- 此语句会隐式的提交前边语句所属于的事务
~~~

② 当前的 `autocommit `系统变量的值为 OFF ，我们手动把它调为 ON 时，也会 隐式的提交 前边语 句所属的事务。

③ 使用 LOCK TABLES 、 UNLOCK TABLES 等关于锁定的语句也会 隐式的提交 前边语句所属的事务。



- 加载数据的语句

​		使用 LOAD DATA语句来批量往数据库中导入数据时，也会隐式的提交前边语句所属的事务



- 关于MySQL复制的一些语句

​		使用START SLAVE、STOP SLAVE、RESET SLAVE、CHANGE MASTER TO等语句时会隐式的提交前边语句所属的事务。



- 其它的一些语句

​		使用ANALYZE TABLE、CACHE INDEX、CHECK TABLE、FLUSH、LOAD INDEX INTO CACHE OPTIMIZE TABLE、REPAIR TABLE、RESET等语句也会 隐式的		提交 前边语句所属的事务





案例分析：

举例一：体会commit 与 rollback的使用

~~~mysql
-- 情况一：

 -- 保证是自动提交事务
 SET AUTOCOMMIT =TRUE; 
 -- 切换数据库
 USE atguigudb1;
 
 -- 新建表
 CREATE TABLE use3(NAME VARCHAR(15) PRIMARY KEY );
 
 
 SELECT * FROM use3;
 
 
 -- 开启事务
 BEGIN ;
 
 INSERT INTO use3 VALUES('fgcy'); -- 此时不会自动提交事务 但有记录 ‘fgcy’
 
 -- 结束事务
 COMMIT;
 
 
 
BEGIN ;

INSERT INTO use3 VALUES('张三'); -- 此时不会自动提交事务 但有记录 ‘张三’
  
INSERT INTO use3 VALUES('张三'); -- 受主键影响 不会插入 ‘张三’
 
ROLLBACK; -- 回滚后 只有记录 ‘fgcy’
~~~



~~~mysql
 -- 情况二：
 
 TRUNCATE TABLE use3; -- DDL操作 会自动提交数据 不受系统变量autocommit的影响
 
 
 SELECT * FROM use3; -- 表为空
 
 
 BEGIN ;-- 开启事务
 
 INSERT INTO use3 VALUES('fgcy'); -- 此时不会自动提交事务 但有记录 ‘fgcy’
 
 COMMIT;-- 结束事务
 
 
 -- 默认情况下 DML 会自动提交事务 直接插入数据
 INSERT INTO use3 VALUES('张三'); 
 
 -- 受主键影响 不会插入 ‘张三’
 INSERT INTO use3 VALUES('张三'); -- 事务是失败的状态
 
 
 ROLLBACK; -- 回滚后还是 fgcy 张三 事务是中止的状态
~~~







举例二：体会 Innodb 与 Myisam的区别

~~~mysql
CREATE TABLE test1(i INT )ENGINE=INNODB;
 
 
CREATE TABLE test2(i INT )ENGINE=MYISAM;



-- 针对 innodb
BEGIN;
INSERT INTO test1 VALUES (1); -- 有记录 1
ROLLBACK; -- 没有记录

SELECT * FROM test1;



-- 针对 Myisam MyISAM不支持事务
BEGIN;
INSERT INTO test2 VALUES (1); -- 有记录 1
ROLLBACK; -- 有记录 1
~~~





举例三：体会savePoint

~~~mysql
-- 创表
CREATE TABLE use1(
	NAME VARCHAR(15),
	balance DECIMAL(10,2)
);


BEGIN;-- 开启事务
INSERT INTO use1 VALUES('fgcy',1000);
COMMIT;-- 提交事务


BEGIN;-- 开启事务 【只有开启了事务才能有保存点】
UPDATE use1 SET balance =balance-100 WHERE NAME='fgcy'; -- 900

UPDATE use1 SET balance =balance-100 WHERE NAME='fgcy'; -- 800

SAVEPOINT s1;-- 设置保存点

UPDATE use1 SET balance =balance+1 WHERE NAME='fgcy'; -- 801

ROLLBACK TO s1; -- 800

ROLLBACK; -- 1000

SELECT * FROM use1;
~~~





## 2.4 使用举例1：

提交与回滚 我们看下在 MySQL 的默认状态下，下面这个事务最后的处理结果是什么。 



情况1：







当我们设置 autocommit=0 时，不论是否采用 START TRANSACTION 或者 BEGIN 的方式来开启事 务，都需要用 COMMIT 进行提交，让事务生效，使用 ROLLBACK 对事务进行回滚。 当我们设置 autocommit=1 时，每条 SQL 语句都会自动进行提交。 不过这时，如果你采用 START TRANSACTION 或者 BEGIN 的方式来显式地开启事务，那么这个事务只有在 COMMIT 时才会生效， 在 ROLLBACK 时才会回滚。













你能看到相同的SQL代码，只是在事务开始之前设置了SET@@completion_type= 1；，结果就和我们第一次处理的一样，只有一个"张三”。这是为什么呢？
这里讲解下 MySQL 中completion_type 参数的作用，实际上这个参数有3 种可能：

- completion=0，这是默认情况。当我们执行 COMMIT的时候会提交事务，在执行下一个事务时，还需要使用START TRANSACTION或者 BEGIN来开启。
- completion=1，这种情况下，当我们提交事务后，相当于执行了 COMMIT AND CHAIN，也就是开启一个链式事务，即当我们提交事务之后会开启一个相同隔离级别的事务
- completion=2，这种情况下 COMMIT=COMMIT AND RELEASE，也就是当我们提交后，会自动与服务器断开连接。









# 3. 事务隔离级别 

MySQL是一个 客户端／服务器 架构的软件，对于同一个服务器来说，可以有若干个客户端与之连接，每 个客户端与服务器连接上之后，就可以称为一个会话（ Session ）。

每个客户端都可以在自己的会话中 向服务器发出请求语句，一个请求语句可能是某个事务的一部分，也就是对于服务器来说可能同时处理 多个事务。

事务有 隔离性 的特性，理论上在某个事务 对某个数据进行访问 时，其他事务应该进行 排 队 ，当该事务提交之后，其他事务才可以继续访问这个数据。

但是这样对 性能影响太大 ，我们既想保持 事务的隔离性，又想让服务器在处理访问同一数据的多个事务时 性能尽量高些【并发】 ，那就看二者如何权衡取舍了。



## 3.1 数据准备 我们需要创建一个表：

~~~mysql
CREATE TABLE student (
    studentno INT,
    name VARCHAR(20),
    class varchar(20),
    PRIMARY KEY (studentno)
) Engine=InnoDB CHARSET=utf8;
~~~



然后向这个表里插入一条数据：

~~~mysql
INSERT INTO student VALUES(1, '小谷', '1班');
~~~



现在表里的数据就是这样的：

~~~mysql
select * from student;
 
+-----------+------+-------+
| studentno | name | class |
+-----------+------+-------+
|         1 | 小谷 | 1班   |
+-----------+------+-------+
1 row in set (0.00 sec)
~~~







## 3.2 数据并发问题 

针对事务的隔离性和并发性，我们怎么做取舍呢？先看一下访问相同数据的事务在 **不保证串行执行** （也 就是执行完一个再执行另一个）的情况下可能会出现哪些问题：



1. 脏写（ Dirty Write ） 

对于两个事务 Session A、Session B，如果事务Session A 修改了 另一个 未提交 事务Session B 修改过 的数 据，那就意味着发生了 脏写

---

![image-20221008221704928](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210082217025.png)

---

Session A 和 Session B 各开启了一个事务，Session B中的事务先将studentno列为1的记录的name列更新为'李四'然后Session A中的事务接着又把这条studentno列为1的记录的name列更新为'张三"。如果之后Session B中的事务进行了回滚，那么SessionA中的更新也将不复存在，这种现象就称之为脏写。

这时 Session A中的事务就没有效果，明明把数据更新了，最后也提交了事务，最后看到的数据什么变化也没有。这里大家对事务的隔离级比较了解的话，会发现默认隔离级别下，上面SessionA中的更新语句会处于等待状态，这里只是跟大家说明一下会出现这样现象，











2. 脏读（ Dirty Read ） 

对于两个事务 Session A、Session B，Session A 读取 了已经被 Session B 更新 但还 没有被提交 的字段。 之后若 Session B 回滚 ，Session A 读取 的内容就是 临时且无效 的。

---

![image-20221008222542882](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210082225920.png)

----

Session A和Session B各开启了一个事务，Session B中的事务先将studentno列为1的记录的name列更新 为'张三'，然后Session A中的事务再去查询这条studentno为1的记录，如果读到列name的值为'张三'，而 Session B中的事务稍后进行了回滚，那么Session A中的事务相当于读到了一个不存在的数据，这种现象 就称之为 脏读 。











3. 不可重复读（ Non-Repeatable Read ） 

对于两个事务Session A、Session B，Session A 读取 了一个字段，然后 Session B 更新 了该字段。 之后 Session A 再次读取 同一个字段， 值就不同 了。

那就意味着发生了不可重复读。-

-----

![image-20221008223032119](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210082230495.png)

----

 我们在Session B中提交了几个 隐式事务 （注意是隐式事务，意味着语句结束事务就提交了），这些事务 都修改了studentno列为1的记录的列name的值，每次事务提交之后，如果Session A中的事务都可以查看 到最新的值，这种现象也被称之为 不可重复读 。



4. 幻读（ Phantom ） 

对于两个事务Session A、Session B, Session A 从一个表中 读取 了一个字段, 然后 Session B 在该表中 插 入 了一些新的行。 之后, 如果 Session A 再次读取 同一个表, 就会多出几行。那就意味着发生了幻读。

---

![image-20221008223437018](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210082234294.png)

----

Session A中的事务先根据条件 `studentno > 0` 这个条件查询表student，得到了name列值为'张三'的记录； 之后Session B中提交了一个 隐式事务 ，该事务向表student中插入了一条新记录；

之后Session A中的事务 再根据相同的条件 studentno > 0查询表student，得到的结果集中包含Session B中的事务新插入的那条记 录，这种现象也被称之为 幻读 

我们把新插入的那些记录称之为 幻影记录 。



**注意1：**

如果Session B 中删除了一些符合 `studentno >0 ` 的记录而不是插入新记录，那SessionA之后再根据studentno >0的条件读取的记录变少了，这种现象算不算幻读呢？这种现象不属于幻读，幻读强调的是一个事务按照其个相同条件多次读取记录时，后读取时读到了之前没有读到的记录



**注意2：**

那对于先前已经读到的记录，之后又读取不到这种情况，算啥呢？这相当于对 **每一条记录** 都发生了 **不可重复读的现象**。

幻读只是重点强调了**读取到了之前读取没有获取到的记录**



## 3.3 SQL中的四种隔离级别 

面介绍了几种并发事务执行过程中可能遇到的一些问题，这些问题有轻重缓急之分，我们给这些问题 按照严重性来排一下序：

> 脏写 > 脏读 > 不可重复读 > 幻读



我们愿意舍弃一部分隔离性来换取一部分性能在这里就体现在：设立一些隔离级别，隔离级别越低，并 发问题发生的就越多。 **SQL标准** 中设立了 **4个 隔离级别** ： 

- `READ UNCOMMITTED` ：读未提交，在该隔离级别，所有事务都可以看到其他未提交事务的执行结 果。不能避免脏读、不可重复读、幻读。
- `READ COMMITTED` ：读已提交，它满足了隔离的简单定义：一个事务只能看见已经提交事务所做 的改变。这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。可以避免脏读，但不可 重复读、幻读问题仍然存在。
- `REPEATABLE READ` ：可重复读，事务A在读到一条数据之后，此时事务B对该数据进行了修改并提 交，那么事务A再读该数据，读到的还是原来的内容。可以避免脏读、不可重复读，但幻读问题仍 然存在。这是MySQL的默认隔离级别。 
- `SERIALIZABLE `：可串行化，确保事务可以从一个表中读取相同的行。在这个事务持续期间，禁止 其他事务对该表执行插入、更新和删除操作。所有的并发问题都可以避免，但性能十分低下。能避 免脏读、不可重复读和幻读。 SQL标准 中规定，针对不同的隔离级别，并发事务可以发生不同严重程度的问题，具体情况如下：

----

![image-20221009202359625](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210092024759.png)

----

脏写 怎么没涉及到？因为脏写这个问题太严重了，不论是哪种隔离级别，都不允许脏写的情况发生。 不同的隔离级别有不同的现象，并有不同的锁和并发机制，隔离级别越高，数据库的并发性能就越差



4 种事务隔离级别与并发性能的关系如下：

---

![image-20221009203200767](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210092032095.png)

----



不同的数据库厂商对SOL标准中规定的四种隔离级别支持不一样。比如，Oracle 就只支持 `READ COMNITTED` (默认隔离级别）和 `SERIALIZABLE` 隔离级别。MySQL虽然支持4种隔离级别，但与SQL标准中所规定的各级隔离级别允许发的问题却有些出入，`MySOL ` 在 ` REPEATABL EREAD` 隔离级别下，是可以禁止幻读问题的发生的，禁止幻读的原因我们在第16章讲解

MySOL的默认隔离级别为 `REPEATABLE READ`，我们可以手动修改一下事务的隔离级别:

~~~mysql
-- 查看隔离级别，MySQL 5.7.20的版本之前：
SHOW VARIABLES LIKE 'tx_isolation';
~~~



~~~mysql
-- MySQL 5.7.20版本之后，引入transaction_isolation来替换tx_isolation
-- 查看隔离级别，MySQL 5.7.20的版本及之后：
SHOW VARIABLES LIKE 'transaction_isolation';

+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set, 1 warning (0.00 sec)
~~~



~~~mysql
-- 或者不同MySQL版本中都可以使用的：
SELECT @@transaction_isolation;

+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)
~~~

如果在服务器启动时想改变事务的默认隔离级别，可以修改启动参数transaction_isolation 的值。比如，在启动服务器时指定`transaction_iso1ation=SERIALIZABLE`，那么事务的默认隔离级别就从原来的 `REPEATABLE-READ `变成了 `SERIALIZABLE`



> 小结：数据库规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别械高，数据一致性就越好但并发性越弱;

## 3.5 如何设置事务的隔离级别 

通过下面的语句修改事务的隔离级别：

~~~mysql
SET [GLOBAL|SESSION] TRANSACTION ISOLATION LEVEL 隔离级别;
#其中，隔离级别格式：
> READ UNCOMMITTED
> READ COMMITTED
> REPEATABLE READ
> SERIALIZABLE
~~~

或是：

~~~mysql
SET [GLOBAL|SESSION] TRANSACTION_ISOLATION = '隔离级别'
#其中，隔离级别格式：
> READ-UNCOMMITTED
> READ-COMMITTED
> REPEATABLE-READ
> SERIALIZABLE
~~~

关于设置时使用 `GLOBAL` 或 `SESSION` 的影响： 使用 GLOBAL 关键字（在全局范围影响）：

~~~mysql
SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
#或
SET GLOBAL TRANSACTION_ISOLATION = 'SERIALIZABLE';
~~~

则： 

- 当前已经存在的会话无效 

- 只对执行完该语句之后产生的会话起作用



使用 SESSION 关键字（在会话范围影响）：

~~~mysql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
#或
SET SESSION TRANSACTION_ISOLATION = 'SERIALIZABLE';
~~~

则： 

- 对当前会话的所有后续的事务有效 

- 如果在事务之间执行，则对后续的事务有效 

- 该语句可以在已经开启的事务中间执行，但不会影响当前正在执行的事务

> 小结： 数据库规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性 就越好，但并发性越弱。







## 3.6 不同隔离级别举例 

演示1. 读未提交之脏读 设置隔离级别为 **未提交读** ：

~~~mysql
-- 切换数据库
USE dbtest2;

-- 建表
CREATE TABLE account(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(15),
	balance DECIMAL(10,2)
);

-- 插入数据
INSERT INTO account VALUES(1,'张三',100),(2,'李四',0);

-- 查看数据
SELECT * FROM account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)


-- 查看隔离级别
select @@transaction_isolation;
+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)

-- 修改隔离级别
SET SESSION transaction_isolation = 'read-uncommitted';
Query OK, 0 rows affected (0.00 sec)

SELECT @@transaction_isolation; -- READ-UNCOMMITTED
~~~

另一个会话也设置为 `读未提交`

在一个事务中进行修改：

~~~mysql
-- 查看数据
select * from account;

+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)

-- 开启事务
begin;
Query OK, 0 rows affected (0.00 sec)

-- 修改数据
update account set balance = balance + 100 where id =1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
-- 此时还没有提交
~~~



在另一个（会话）事务中进行查看:

~~~mysql
select * from account;
 
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  200.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



此时之前的哪个事务进行回滚，那么读取到的 `200 ` 就是脏数据，脏读：

~~~mysql
-- 事务回滚
rollback;
Query OK, 0 rows affected (0.00 sec)

select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~

----

![image-20221009225455210](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210092255248.png)

---



演示二：脏读问题演示：

会话一： 张三向李四转账，事务未提交

~~~mysql
select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)


begin;
Query OK, 0 rows affected (0.00 sec)


update account set balance = balance+100 where id =2;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

update account set balance = balance-100 where id =1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |    0.00 |
|  2 | 李四 |  100.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



会话二：李四向张三转账

~~~mysql
mysql>  update account set balance = balance-100 where id =2;
 -- 这里发现是 “卡住”的状态
~~~





会话一：回滚事务

~~~mysql
rollback;

Query OK, 0 rows affected (0.00 sec)
~~~



会话二：完成转账

~~~mysql
update account set balance = balance-100 where id =2;

Query OK, 1 row affected (5.83 sec)
Rows matched: 1  Changed: 1  Warnings: 0


update account set balance = balance+100 where id =1;

Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0


select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  200.00 |
|  2 | 李四 | -100.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~

---

![image-20221009225513323](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210092255589.png)

---





演示3：读已提交

会话一：准备数据，修改隔离级别

~~~mysql
-- 清空数据
truncate table account;
Query OK, 0 rows affected (0.03 sec)

-- 插入数据
INSERT INTO account VALUES(1,'张三',100),(2,'李四',0);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

-- 查看数据
select * from account;

+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)


-- 设置事务隔离级别 读已提交
SET SESSION transaction_isolation = 'read-committed';
Query OK, 0 rows affected (0.00 sec)
~~~



会话二：修改隔离级别

~~~mysql
-- 设置事务隔离级别 读已提交
SET SESSION transaction_isolation = 'read-committed';
Query OK, 0 rows affected (0.00 sec)
~~~



会话二：开启事务，查看数据

~~~mysql
begin;
Query OK, 0 rows affected (0.00 sec)

select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



会话一：开启事务，修改数据

~~~mysql
begin;
Query OK, 0 rows affected (0.00 sec)


update account set balance =balance-50 where id =1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0


-- 查看数据
 select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~





会话二：查看数据

~~~mysql
-- 查看数据
select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |  100.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
-- 发现没有读取到脏数据
~~~



会话一：提交事务

~~~mysql
commit;
Query OK, 0 rows affected (0.00 sec)
~~~



会话二：查看数据

~~~mysql
-- 查看数据
select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
-- 在同一个事务中，读取相同的数据发现 数据不相同 -----》不可重复度
~~~





两个事务设置重复读：

会话一、二：

~~~mysql
SET SESSION transaction_isolation = 'repeatable-read';
Query OK, 0 rows affected (0.00 sec)
~~~





会话一：

~~~mysql
select @@transaction_isolation;

+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)

-- 开启事务
begin;
Query OK, 0 rows affected (0.00 sec)


select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~





会话二：

~~~mysql
select @@transaction_isolation;

+-------------------------+
| @@transaction_isolation |
+-------------------------+
| REPEATABLE-READ         |
+-------------------------+
1 row in set (0.00 sec)


-- 开启事务
begin;
Query OK, 0 rows affected (0.00 sec)

select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)

update account set balance = balance -10 where id=1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   40.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~





会话一：

~~~mysql
select * from account;

+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 | -- 没有脏读的问题【没有读取到提交的数据】
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



会话二：

~~~mysql
commit;
Query OK, 0 rows affected (0.00 sec)

select * from account;-- 与上面的数据虽然是一样的，但是一个是内存级别，这个是磁盘级别
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   40.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



会话一：

~~~mysql
-- 在事务没有结束前 读取数据 没有不可重复读的问题
select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   50.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)


-- 提交数据
commit;
Query OK, 0 rows affected (0.00 sec)

-- 退出上一个事务后，即可读取到另一个事务修改后的数据
select * from account;
+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   40.00 |
|  2 | 李四 |    0.00 |
+----+------+---------+
2 rows in set (0.00 sec)
~~~



在隔离级别为 `repeatable-read` 的情况下，演示不可解决幻读问题：

会话二：

~~~mysql
begin;
Query OK, 0 rows affected (0.00 sec)

select * from account where id=3;
Empty set (0.00 sec)
~~~





会话一：

~~~mysql
begin;
Query OK, 0 rows affected (0.00 sec)


insert into account values(3,'王五',100);
Query OK, 1 row affected (0.00 sec)



select * from account;

+----+------+---------+
| id | name | balance |
+----+------+---------+
|  1 | 张三 |   40.00 |
|  2 | 李四 |    0.00 |
|  3 | 王五 |  100.00 |
+----+------+---------+
3 rows in set (0.00 sec)

commit;
Query OK, 0 rows affected (0.00 sec)
~~~



会话二：

~~~mysql
select count(*) from account where id=3;

+----------+
| count(*) |
+----------+
|        0 | -- 不是说可重复读级别不能解决幻读问题嘛 这是因为mysql使用了间隙锁，优化了可重复读的情况下发生幻读的问题幻读
+----------+
1 row in set (0.00 sec)


insert into account values(3,'wangwu',1000); -- 证明有幻读
ERROR 1062 (23000): Duplicate entry '3' for key 'account.PRIMARY'
~~~



---

![image-20221010223233263](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210102232558.png)

----

这里要灵活的理解读取的意思，第一次select是读取，第二次的 inser 其实也属于隐式的读取，只不过是在mysq的机制中读取的，插入数据也是要先读取一下有没有主键冲突才能决定是否执行插入



幻读，并不是说两次读取获取的结果集不同，幻读侧重的方面是某一次的 selet 操作得到的结果所表征的数据状态无法支撑后续的业务操作。

更为具体一些：select 某记录是否存在，不存在，准备插入此记录，但执行inser时发现此记录已存在，无法插入，此时就发生了幻读



在RR隔离级别下，step1、step2 是会正常执行的，step3 则会报错主键冲突，对于事务1的业务来说是执行失败的，这里事务1就是发生了幻读，因为事务1在step1中读取的数据状态并不能支撑后续的业务操作，事务1：“见鬼了，我刚才读到的结果应该可以支持我这样操作才对啊，为什么现在不可以"。事务1不敢相信的又执行了step4，发现和setp1 读取的结果是一样的（RR下的MVCC机制）。此时，幻读无疑已经发生，事务1无论读取多少次，都查不到id=3的记录，但它的确无法插入这条他通过读取来认定不存在的记录（此数据已被事务2插入），对于事务1来说，它幻读了



其实 RR 也是可以避免幻读的，通过对select 操作动加行X锁（独占锁）（SELECTFOR UPDATE 这也正是SERIALIZABLE隔离级别下会隐式为你做的事情）。同时，即便当前记录不存在，比如id=3是不存在的，当前事务也会获得一把记录锁（因为InnoDB的行锁锁定的是索引，故记录实体存在与否没关系，存在就加行X锁，不存在就加间隙锁》，其他事务则无法插入此素引的记录，故杜绝了幻读



在 `SERIALIZABLE 隔离级别` 下，step1 执行时是会隐式的添加` 行（X）锁` / `gap（X）锁` 的，从而step2会被阻塞step3会正常执行，待事务1提交后，事务2才能继续执行（主键冲突执行失败），对于事务1来说业务是正确的，成功的阳塞扼杀了扰乱业务的事务2，对于事务1来说他前期读取的结果是可以支撑其后续业务的

所以MySOL的幻读并非读取两次返回的结果集不同，而是 **事务在插入事先检测不到记录时** ，然后放心大胆地进行插入，惊奇的发现这些数据已经存在了，之前的检测做据如同鬼影一般。

# 4. 事务的常见分类 

从事务理论的角度来看，可以把事务分为以下几种类型：

-  扁平事务（Flat Transactions） 

​		扁平事务是事务类型中最简单的一种，但是在实际生产环境中，这可能是使用最频繁的事务，在扁平事务中，所有操作都处于同一层次，其由BEGIN WORK开		始，由COMMIT WORK或ROLLBACK WORK结束，其间的操作是原子的，要么都执行，要么都回滚，因此，扁平事务是应用程序成为原子操作的基本组成模		块。扁平事务虽然简单，但是在实际环境中使用最为频繁，也正因为其简单，使用频繁，故每个数据库系统都实现了对扁平事务的支持、扁平事务的丰要限制		是不能提衣或者回滚事务的某一部分，或分几个步骤提交。



- 带有保存点的扁平事务（Flat Transactions with Savepoints） 

​		带有保存点的扁平事务除了支持扁平事务支持的操作外，还允许在事务执行过程中回滚到同一事务中较早的一个状态。这是因为某些事务可能在执行过程中出		现的错误并不会导致所有的操作都无效，放弃整个事务不合乎要求，开销太大



​		保存点 `Savepoint `用来通知事务系统应该记住事务当前的状态，以便当之后发生错误时，事务能回到保存点当时的状态。对于扁平的事务来说，隐式的设置		了一个保存点，然而在整个事务中，只有这一个保存点，因此回滚只能会滚到事务开始时的状态

- 链事务（Chained Transactions） 

​		链事务是指一个事务由多个子事务链式组成，它可以被视为保存点模式的一个变种。带有保存点的扁平事务，当发生系统崩溃时，所有的保存点都将消失，这		意味着当进行恢复时，事务需要从开始处重新执行，而不能从最近的一个保存点继续执行。

​		链事务的思想是：在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务，前一个子事务的提交操作和下一个子事		务的开始操作合并成一个原子操作，这意味着下一个事务将看到上一个事务的结果，就好像在一个事务中进行一样。这样，在提交子事务时就可以释放不需要		的数据对象，而不必等到整个事务完成后才释放。

其工作方式如下：

---

![image-20221007232833618](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210072328047.png)

---

1、链事务与带有保存点的扁平事务的不同之处体现在：带有保存点的扁平事务能回滚到任意正确的保存点，而链事务中的回滚仅限于当前事务，即只能恢复到最近的一个保存点。
2、对于锁的处理，两者也不相同，链事务在执行COMMIT后即程放了当前所持有的锁，而带有保存点的扁平事务不影响迄今为止所持有的锁，



- 嵌套事务（Nested Transactions） 

​		嵌套事务是一个层次结构框架，由一个顶层事务（Top-LevelTransaction）控制着各个层次的事务，顶层事务之下嵌套的事务被称为子事务							       （Subtransaction），其控制着每一个局部的变换，子事务本身也可以是嵌套事务因此，嵌套事务的层次结构可以看成是一棵树

- 分布式事务（Distributed Transactions）

​		分布式事务通常是在一个分布式环境下运行的扁平事务，因此，需要根据数据所在位置访问网络中不同节点的数据库资源。

​		例如，一个银行用户从招商银行的账户向工商银行的账户转账1000元，这里需要用到分布式事务，因为不能仅调用某一家银行的数据库就完成任务

