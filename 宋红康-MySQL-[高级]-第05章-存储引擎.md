---
title: 第05章_存储引擎
date: 2022-08-28 20:59:49
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---





# 存储引擎（表处理器）

为了管理方便,人们把  `连接管理`、`查询缓存`  、`语法解析`  、`查询优化`    这些并不涉及  **真实数据存储**  的功能划分为  **MySQL server 的功能**

把  **真实存取数据**   的功能划分为  **存储引擎的功能**

所以在    `MySQL server完成了`   **查询优化后**   ,只需按照生成的   `执行计划   `   **调用**     ` 底层存储引擎提供的API  `  ,获取到数据后返回给客户端就好了

MySQL中提到了存储引擎的概念。简而言之,**存储引擎就是指表的类型**

其实存储引擎以前叫做 `表处理器` ,后来改名为存储引擎

它的功能就是接收上层传下来的指令,然后对表中的数据进行提取或写入操作。



## 1、查看存储引擎 查看mysql提供什么存储引擎：

查看mysql提供什么存储引擎：

> SHOW ENGINES;

<!--more-->

---

![image-20220708202505149](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282100848.png)

---

查询结果显示, `MySQL 8.0` 支持  `9种存储引擎`  

**`Engine `**参数表示存储引擎名称

`Support参数` 表示MySQL数据库管理系统是否支持该存储引擎:  YES表示支持    ,NO表示不支持   DEFAULT表示系统  默认支持  的存储引擎

`Comment参数`表示对存储引擎的说明

`Transactions参数` 表示存储引擎是否  支持事务   :YES表示支持,NO表示不支持

`XA参数` 表示代表着该存储引擎是否支持分布式事务:YES表示支持,NO表示不支持。

`Savepoints参数` 表示存储引擎是否支持事务处理的保存点: YES表示支持,NO表示不支持。也就是说,该存储引擎是否支持`  部分事务回滚`





## 2. 设置系统默认的存储引擎

查看默认的存储引擎：

~~~mysql
show variables like '%storage_engine%';

+---------------------------------+-----------+
| Variable_name                   | Value     |
+---------------------------------+-----------+
| default_storage_engine          | InnoDB    |
| default_tmp_storage_engine      | InnoDB    |
| disabled_storage_engines        |           |
| internal_tmp_mem_storage_engine | TempTable |
+---------------------------------+-----------+
4 rows in set (0.14 sec)

#或

SELECT @@default_storage_engine; -- 会话级别系统变量

+--------------------------+
| @@default_storage_engine |
+--------------------------+
| InnoDB                   |
+--------------------------+
1 row in set (0.00 sec)
~~~



tip:

MySQL5.5之前，默认是MyISAM存储引擎

在创建表的时候不指定存储引擎，使用MySQL默认的存储引擎：

~~~mysql
show databases;
+--------------------+
| Database           |
+--------------------+
| atguigudb          |
| dbtest1            |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
6 rows in set (0.03 sec)


use dbtest1;
Database changed


show tables;
+-------------------+
| Tables_in_dbtest1 |
+-------------------+
| emp1              |
| emp2              |
| student_myisam    |
+-------------------+
3 rows in set (0.02 sec)


-- 不指定存储引擎建表
create table tem(id int ,name varchar(15));
Query OK, 0 rows affected (0.04 sec)



-- 查看建表语句   （存储引擎为InnoDB）
show create table tem;
+-------+-----------------------------------------------------------------------------------------------------------------------
| Table | Create Table                                                                                                           
+-------+-----------------------------------------------------------------------------------------------------------------------
| tem   | CREATE TABLE `tem` (
  `id` int DEFAULT NULL,
  `name` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+-----------------------------------------------------------------------------------------------------------------------
1 row in set (0.01 sec)
~~~





修改默认的存储引擎

如果在创建表的语句中没有显式指定表的存储引擎的话，那就会默认使用 InnoDB 作为表的存储引擎

 如果我们想改变表的默认存储引擎的话，可以这样写启动服务器的命令行：

> SET DEFAULT_STORAGE_ENGINE=MyISAM;          -- 会话级别修改







或者修改 my.cnf 文件：

> vim /etc/my.cnf

> default-storage-engine=MyISAM



重启服务:

>  systemctl restart mysqld.service





尝试会话级别修改

~~~mysql
SET DEFAULT_STORAGE_ENGINE=MyISAM;

Query OK, 0 rows affected (0.00 sec)
~~~



查看当前默认的存储引擎

~~~mysql
SELECT @@default_storage_engine;

+--------------------------+
| @@default_storage_engine |
+--------------------------+
| MyISAM                   |
+--------------------------+
1 row in set (0.00 sec)
~~~



尝试建表

~~~mysql
CREATE TABLE temp2(id int,name varchar(15));

Query OK, 0 rows affected (0.01 sec)



-- 查看建表语句  (存储引擎变为 MyISAM)
SHOW CREATE TABLE temp2;

+-------+-----------------------------------------------------------------------------------------------------------------------
| Table | Create Table                                                                                                                                         |
+-------+-----------------------------------------------------------------------------------------------------------------------
| temp2 | CREATE TABLE `temp2` (
  `id` int DEFAULT NULL,
  `name` varchar(15) DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+-----------------------------------------------------------------------------------------------------------------------
1 row in set (0.00 sec)
~~~



查看数据文件

~~~shell
cd /var/lib/mysql

cd dbtest1

-rw-r-----. 1 mysql mysql   2408 7月   8 10:05 temp2_375.sdi
-rw-r-----. 1 mysql mysql      0 7月   8 10:05 temp2.MYD
-rw-r-----. 1 mysql mysql   1024 7月   8 10:05 temp2.MYI
~~~



## 3. 设置表的存储引擎



### 3.1 创建表时指定存储引擎

我们之前创建表的语句都没有指定表的存储引擎，那就会使用默认的存储引擎 InnoDB 

如果我们想显 式的指定一下表的存储引擎，那可以这么写：

~~~
CREATE TABLE 表名(
建表语句;
) ENGINE = 存储引擎名称;
~~~



当前会话默认存储引擎为MyISAM

尝试建表时修改：

~~~mysql
CREATE TABLE temp3(id int ,name varchar(15)) ENGINE=InnoDb;

Query OK, 0 rows affected (0.01 sec)



-- 查看建表语句 (当前表使用的是InnoDb引擎)
SHOW CREATE TABLE temp3;

+-------+-----------------------------------------------------------------------------------------------------------------------
| Table | Create Table                                                                                                           
+-------+-----------------------------------------------------------------------------------------------------------------------
| temp3 | CREATE TABLE `temp3` (
  `id` int DEFAULT NULL,
  `name` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+-----------------------------------------------------------------------------------------------------------------------
1 row in set (0.00 sec)
~~~





## 3.2 修改表的存储引擎

如果表已经建好了，我们也可以使用下边这个语句来修改表的存储引擎：

> ALTER TABLE 表名 ENGINE = 存储引擎名称



~~~mysql
-- 查询建表语句 (使用的是MyISAM引擎)
SHOW CREATE TABLE temp2;

+-------+-----------------------------------------------------------------------------------------------------------------------
| Table | Create Table                                                                                                                                         |
+-------+-----------------------------------------------------------------------------------------------------------------------
| temp2 | CREATE TABLE `temp2` (
  `id` int DEFAULT NULL,
  `name` varchar(15) DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+-----------------------------------------------------------------------------------------------------------------------
1 row in set (0.00 sec)




-- 修改表的存储引擎
ALTER TABLE temp2
ENGINE=InnoDb;

Query OK, 0 rows affected (0.08 sec)
Records: 0  Duplicates: 0  Warnings: 0



-- 查询建表语句 (使用的是InnoDb引擎)
SHOW CREATE TABLE temp2;

+-------+-----------------------------------------------------------------------------------------------------------------------
| Table | Create Table                                                                                                                                         |
+-------+-----------------------------------------------------------------------------------------------------------------------
| temp2 | CREATE TABLE `temp2` (
  `id` int DEFAULT NULL,
  `name` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+-------+-----------------------------------------------------------------------------------------------------------------------
1 row in set (0.00 sec)
~~~



## 4. 引擎介绍

### 4.1 InnoDB 引擎：具备外键支持功能的事务存储引擎

 `MySQL从3.23.34a`开始就  `包含InnoDB存储引擎`，当时默认使用M有ISAM

MySQL版本 `大于 等于 5.5` 之后，`默认采用InnoDB引擎` 

InnoDB是`MySQL的 默认事务型引擎` ，它被设计用来处理大量的短期(short-lived)事务。可以确保`事务 的完整提交(Commit)和回滚(Rollback)`

除了增加和查询外，还需要更新、删除操作，那么，应优先选择InnoDB存储引擎 **(如果仅仅只是INSERT、SELECT那么选择MyISAM的效率要高)**

除非有非常特别的原因需要使用其他的存储引擎，否则应该   `优先考虑InnoDB引擎`



 数据文件结构：（在《第02章_MySQL数据目录》章节已讲） 

- 表名.frm `存储表结构`（MySQL8.0时，合并在表名.ibd中） 

- 表名.ibd `存储数据和索引` 



 InnoDB是` 为处理巨大数据量的最大性能设计` 

- 在以前的版本中，字典数据以元数据文件、非事务表等来存储。现在这些元数据文件被删除 了

- 比如： .frm ， .par ， .trn ， .isl ， .db.opt 等都在MySQL8.0中不存在了



InnoeDB缺点：

- 对比MyISAM的存储引擎， InnoDB写的 `处理效率差一些` 

- 并且会占用更多的磁盘空间以保存 `  数据和 索引`；

  MyISAM只缓存索引，不缓存真实数据；

  InnoDB不仅缓存索引还要缓存真实数据， 对内存要求较 高 ，而且内存大小对性能有决定性的影响



### 4.2 MyISAM 引擎：主要的非事务处理存储引擎

1、MyISAM提供了大量的特性，包括 `全文索引` 、`压缩` 、`空间函数(GIS)` 等

2、MyISAM ` 不支持事务、行级 锁、外键`  ，有一个毫无疑问的缺陷就是 `崩溃后无法安全恢复    `    原因：不支持事务

3、`5.5之前`默认的存储引擎 

4、优势是访问的 速度快 ，对事务完整性没有要求或者以SELECT、INSERT为主的应用 

5、针对数据统计有额外的常数存储。故而 count(*) 的查询效率很高



 数据文件结构：（在《第02章_MySQL数据目录》章节已讲）

- 表名.frm `存储表结构`

- 表名.MYD  `存储数据`  (MYData) 

- 表名.MYI `存储索引` (MYIndex) 



应用场景：只读应用或者以读为主的业务



### 4.3 Archive 引擎：用于数据存档 

`archive` 是  归档(多插少查)  的意思,`仅仅支持` **插入 **  和  **查询**   两种功能   **(行被插入后不能再修改)**

在MySQL5.5以后支持  **索引功能**

拥有很好的  **压缩机制**  ,使用   **zlib压缩库**   ,  **在记录请求的时候实时的进行压缩**   ,经常被用来作为  **仓库**  使用

创建ARCHIVE表时,存储引擎会创建名称以表名开头的文件。**数据文件的扩展名为.ARZ**

根据英文的测试结论来看,同样数据量下, `Archive表比MyISAM表要小大约75%`,`比支持事务处理的InnoDB表小大约83%`

ARCHIVE存储引擎采用了**行级锁**。该ARCHIVE引擎支持`AUTO_INCREMENT`列属性。AUTO_INCREMENT列可以具有唯一索引或非唯一索引。尝试在任何其他列上创建索引会导致错误

Archive表适合日志和数据采集(档案)类应用;适合存储大量的独立的作为历史记录的数据。拥有很高的插入速度,但是**对查询的支持较差**





下表展示了ARCHIVE 存储引擎功能

---

![image-20220708114358812](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282100775.png)

---



### 4.4 Blackhole 引擎：丢弃写操作，读操作会返回空内容





### 4.5 CSV 引擎：存储数据时，以逗号分隔各个数据项

CSV引擎 可以   `将普通的CSV文件作为MySQL的表来处理`   ,但不支持索引

CSV引擎 可以作为一种   `数据交换的机制`  ,非常有用

CSV存储 的数据直接可以在操作系统里,用文本编辑器,或者excel读取

对于数据的快速导入、导出是有明显优势的



创建CSV表时,服务器会创建一个纯文本数据文件,其名称以表名开头并带有.CSV扩展名。当你将数据存储到表中时,存储引擎将其以逗号分隔值格式保存到数据文件中





~~~mysql
use dbtest1;

Database changed



-- 报错 错误原因：不支持空列
CREATE TABLE csv_demo(id int ,name char(20)) ENGINE=CSV;

-- ERROR 1178 (42000): The storage engine for the table doesn't support nullable columns


-- 解决：
CREATE TABLE csv_demo(id int not null,name char(20) not null) ENGINE=CSV;

Query OK, 0 rows affected (0.00 sec)



-- 查看建表语句 (存储引擎是CSV)
SHOW CREATE TABLE csv_demo;

+----------+--------------------------------------------------------------------------------------------------------------------
| Table    | Create Table                                                                                                                              |
+----------+--------------------------------------------------------------------------------------------------------------------
| csv_demo | CREATE TABLE `csv_demo` (
  `id` int NOT NULL,
  `name` char(20) NOT NULL
) ENGINE=CSV DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+----------+--------------------------------------------------------------------------------------------------------------------
1 row in set (0.00 sec)




-- 插入数据
INSERT INTO csv_demo VALUES(1,'atguigudb1'),(2,'atguigudb2');

Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0





-- 查看数据
SELECT * FROM csv_demo;

+----+------------+
| id | name       |
+----+------------+
|  1 | atguigudb1 |
|  2 | atguigudb2 |
+----+------------+
2 rows in set (0.00 sec)
~~~



查看相关数据库文件

~~~shell
cd /var/lib/mysql
cd dbtest1
ll

-rw-r-----. 1 mysql mysql   2525 7月   8 11:58 csv_demo_379.sdi
-rw-r-----. 1 mysql mysql     35 7月   8 20:33 csv_demo.CSM
-rw-r-----. 1 mysql mysql     30 7月   8 20:33 csv_demo.CSV
~~~

用于 存储表的状态 和 表中存在的行数 。此文件的名称与表的名称相同，后缀为 CSM



这种格式可以被 Microsoft Excel 等电子表格应用程序读取，甚至写   入。使用Microsoft Excel打开如图所示

---

![image-20220708205212466](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282100496.png)

---



### 4.6 Memory 引擎：置于内存的表

概述： 

Memory采用的逻辑介质是 `内存` ， 响应速度很快 ，但是当mysqld守护进程崩溃的时候 数据会丢失 

另 外，要求存储的数据是 ` 数据长度  不变 `  的格式，比如，Blob和Text类型的数据不可用(长度不固定的)



主要特征： 

- Memory同时 支持    ` 哈希（HASH）索引` 和  `  B+树索引 `

  哈希     `索引相等的比较快`   ,但是对于     `范围的比较慢很多`

  `默认使用哈希(HASH)索引` ,其速度要比使用B型树(BTREE)索引快

  如果希望使用B树索引,可以在创建索引时选择使用

- Memory表至少比MyISAM表要 快一个数量级 

- MEMORY 表的大小是受到限制的

  表的大小主要取决于两个参数，分别是 `max_rows `和 `max_heap_table_size `

  其中，`max_rows`可以在创建表时指定；`max_heap_table_size`的大小默 认为16MB，可以按需要进行扩大

- **数据文件**  与  **索引文件**  **分开存储** 

  每个基于MEMORY存储引擎的表实际对应一个磁盘文件,该文件的文件名与表名相同,类型为  `frm类型`

  该文件中只存储   **表的结构**   ,而其   **数据文件**   都是存储在  **内存**  中的。这样   **有利于数据的快速处理**   ,提供整个表的处理交

  

  缺点：其数据易丢失，生命周期短。基于这个缺陷，选择MEMORY存储引擎时需要特别小心



**使用Memory存储引擎的场景：**

1、`目标数据比较小` ，而且非常 `频繁的进行访问` ，在内存中存放数据，如果 ` 太大的数据 `  会造成 `内存溢 出 `

​		可以通过参数 `max_heap_table_size `控制Memory表的大小，限制Memory表的最大的大 小 

2、如果 `数据是临时的` ，而且 `必须立即可用` 得到，那么就可以放在内存中

3、存储在Memory表中的数据如果突然间 `丢失的话也没有太大的关系`





### 4.7 Federated 引擎：访问远程表

`Federated`引擎是访问其他MySQL服务器的一个 代理 ，尽管该引擎看起来提供了一种很好的 跨服务 器的灵活性 

但也经常带来问题，因此 **默认是禁用的**







### 4.8 Merge引擎：管理多个MyISAM表构成的表集合





### 4.9 NDB引擎：MySQL集群专用存储引擎

也叫做 NDB Cluster 存储引擎，主要用于 `MySQL Cluster`     `分布式集群 环境`，类似于 Oracle 的 RAC 集 群



### 4.10 引擎对比

MySQL中同一个数据库，不同的表可以选择不同的存储引擎。如下表对常用存储引擎做出了对比

---

![image-20220709092450211](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282100746.png)

![image-20220709092516637](http://img.fgcy.xyz/image-20220709092516637.png)

![image-20220709092545605](http://img.fgcy.xyz/image-20220709092545605.png)

---

其实这些东西大家没必要立即就给记住，列出来的目的就是想让大家明白不同的存储引擎支持不同的功 能

其实我们最常用的就是 `InnoDB `和   `MyISAM ` ，有时会提一下 Memory 

其中 **InnoDB 是 MySQL 默认的存 储引擎**







## 5. MyISAM和InnoDB

很多人对 InnoDB 和 MyISAM 的取舍存在疑问，到底选择哪个比较好呢？

​	 MySQL5.5之前的默认存储引擎是MyISAM，5.5之后改为了InnoDB





---

![image-20220708111221458](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282101521.png)

---





## 6. 阿里巴巴、淘宝用哪个



---

![image-20220708111410677](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282101811.png)

---



Percona 为 MySQL 数据库服务器进行了改进，在功能和性能上较 MySQL 有很显著的提升

 该版本提升了在高负载情况下的 InnoDB 的性能、为 DBA 提供一些非常有用的性能诊断工具；另外 有更多的参数和命令来控制服务器行为

该公司新建了一款`存储引擎叫 Xtradb `完全可以替代 `Innodb `，并且在 ` 性能和并发 `  上做得更好 阿里巴巴大部分mysql数据库其实使用的percona的原型加以修改







# 课外补充：

## 1、InnoDB表的优势

InnoDB存储引擎在实际应用中拥有诸多优势，比如操作便利、提高了数据库的性能、维护成本低等

如 果由于硬件或软件的原因导致服务器崩溃，那么在重启服务器之后不需要进行额外的操作。InnoDB崩溃 恢复功能自动将之前提交的内容定型，然后撤销没有提交的进程，重启之后继续从崩溃点开始执行

 InnoDB存储引擎在主内存中维护缓冲池，高频率使用的数据将在内存中直接被处理。这种缓存方式应用 于多种信息，加速了处理进程

在专用服务器上，物理内存中高达80%的部分被应用于缓冲池。如果需要将数据插入不同的表中，可以 设置外键加强数据的完整性。更新或者删除数据，关联数据将会被自动更新或删除。如果试图将数据插 入从表，但在主表中没有对应的数据，插入的数据将被自动移除。如果磁盘或内存中的数据出现崩溃， 在使用脏数据之前，校验和机制会发出警告。当每个表的主键都设置合理时，与这些列有关的操作会被 自动优化。插入、更新和删除操作通过做改变缓冲自动机制进行优化。 `InnoDB不仅支持当前读写，也会 缓冲改变的数据到数据流磁盘` 



 InnoDB的性能优势不只存在于长时运行查询的大型表。在同一列多次被查询时，自适应哈希索引会提高 查询的速度。使用InnoDB可以压缩表和相关的索引，可以 在不影响性能和可用性的情况下创建或删除索 引 。对于大型文本和BLOB数据，使用动态行形式，这种存储布局更高效。通过查询 INFORMATION_SCHEMA库中的表可以监控存储引擎的内部工作。在同一个语句中，InnoDB表可以与其他 存储引擎表混用。即使有些操作系统限制文件大小为2GB，InnoDB仍然可以处理。 `当处理大数据量时， InnoDB兼顾CPU，以达到最大性能`





## 2、InnoDB和ACID模型

ACID模型是一系列数据库设计规则，这些规则着重强调可靠性，而可靠性对于商业数据和任务关键型应 用非常重要

MySQL包含类似InnoDB存储引擎的组件，与ACID模型紧密相连，这样出现意外时，数据不 会崩溃，结果不会失真

如果依赖ACID模型，可以不使用一致性检查和崩溃恢复机制。如果拥有额外的 软件保护，极可靠的硬件或者应用可以容忍一小部分的数据丢失和不一致，可以将MySQL设置调整为只 依赖部分ACID特性，以达到更高的性能

下面讲解InnoDB存储引擎与ACID模型相同作用的四个方面

1、原子方面

 ACID的原子方面主要涉及InnoDB事务，与MySQL相关的特性主要包括：

 自动提交设置。

 COMMIT语句

 ROLLBACK语句

 操作INFORMATION_SCHEMA库中的表数据。



2、一致性方面 

ACID模型的一致性主要涉及保护数据不崩溃的内部InnoDB处理过程，与MySQL相关的特性 主要包括：

 InnoDB双写缓存

 InnoDB崩溃恢复



 3、隔离方面 

隔离是应用于事务的级别，与MySQL相关的特性主要包括：

 自动提交设置

SET ISOLATION LEVEL语句

 InnoDB锁的低级别信息



4.、耐久性方面

 ACID模型的耐久性主要涉及与硬件配置相互影响的MySQL软件特性。由于硬件复杂多样 化，耐久性方面没有具体的规则可循

与MySQL相关的特性有：

 InnoDB双写缓存，通过innodb_doublewrite配置项配置

 配置项innodb_flush_log_at_trx_commit

 配置项sync_binlog

 配置项innodb_file_per_table

 存储设备的写入缓存

 存储设备的备用电池缓存

 运行MySQL的操作系统

 持续的电力供应

 备份策略

 对分布式或托管的应用，最主要的在于硬件设备的地点以及网络情况







## 3、InnoDB架构 

1、 缓冲池 

缓冲池是主内存中的一部分空间，用来缓存已使用的表和索引数据

缓冲池使得经常被使用的 数据能够直接在内存中获得，从而提高速度



2、更改缓存 

更改缓存是一个特殊的数据结构，当受影响的索引页不在缓存中时，更改缓存会缓存辅助索 引页的更改

索引页被其他读取操作时会加载到缓存池，缓存的更改内容就会被合并。不同于集群索 引，辅助索引并非独一无二的。当系统大部分闲置时，清除操作会定期运行，将更新的索引页刷入磁 盘。更新缓存合并期间，可能会大大降低查询的性能。在内存中，更新缓存占用一部分InnoDB缓冲池。 在磁盘中，更新缓存是系统表空间的一部分。更新缓存的数据类型由innodb_change_buffering配置项管 理。 



3、 自适应哈希索引 

自适应哈希索引将负载和足够的内存结合起来，使得InnoDB像内存数据库一样运行， 不需要降低事务上的性能或可靠性

这个特性通过`innodb_adaptive_hash_index`选项配置，或者通过 `-- skip-innodb_adaptive_hash_index` 命令行在服务启动时关闭。



4、重做日志缓存

重做日志缓存存放要放入重做日志的数据。重做日志缓存大小通过  `innodb_log_buffer_size` 配置项配置

重做日志缓存会定期地将日志文件刷入磁盘。大型的重做日志缓存 使得大型事务能够正常运行而不需要写入磁盘。



 5、系统表空间

 系统表空间包括InnoDB数据字典、双写缓存、更新缓存和撤销日志，同时也包括表和索引 数据

多表共享，系统表空间被视为共享表空间



6、双写缓存

 双写缓存位于系统表空间中，用于写入从缓存池刷新的数据页

只有在刷新并写入双写缓存 后，InnoDB才会将数据页写入合适的位置



7、撤销日志

撤销日志是一系列与事务相关的撤销记录的集合，包含如何撤销事务最近的更改。如果其他 事务要查询原始数据，可以从撤销日志记录中追溯未更改的数据

撤销日志存在于撤销日志片段中，这 些片段包含于回滚片段中



8、每个表一个文件的表空间 

每个表一个文件的表空间是指每个单独的表空间创建在自身的数据文件中， 而不是系统表空间中。这个功能通过 `innodb_file_per_table` 配置项开启。每个表空间由一个单独的.ibd数 据文件代表，该文件默认被创建在数据库目录中



9、通用表空间 

使用 `CREATE TABLESPACE` 语法创建共享的InnoDB表空间。通用表空间可以创建在MySQL数 据目录之外能够管理多个表并支持所有行格式的



10、撤销表空间

 撤销表空间由一个或多个包含撤销日志的文件组成。撤销表空间的数量由 `innodb_undo_tablespaces`配置项配置



11、临时表空间 

用户创建的临时表空间和基于磁盘的内部临时表都创建于临时表空间。 innodb_temp_data_file_path配置项定义了相关的路径、名称、大小和属性

如果该值为空，默认会在 innodb_data_home_dir变量指定的目录下创建一个自动扩展的数据文件



12、重做日志

重做日志是基于磁盘的数据结构，在崩溃恢复期间使用，用来纠正数据。正常操作期间， 重做日志会将请求数据进行编码，这些请求会改变InnoDB表数据

遇到意外崩溃后，未完成的更改会自 动在初始化期间重新进行
