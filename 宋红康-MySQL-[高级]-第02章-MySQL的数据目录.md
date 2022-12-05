---
title: 第02章_MySQL的数据目录
date: 2022-08-28 20:48:57
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

# 1.目录结构



## 1、MySQL8的主要目录结构

安装好MySQL 之后，我们查看与之相关的目录结构：

~~~shell
find / -name mysql

/etc/logrotate.d/mysql
/var/lib/mysql
/var/lib/mysql/mysql
/usr/bin/mysql
/usr/lib64/mysql
/usr/share/mysql
~~~



<!--more-->

## 1.1 数据库文件的存放路径

~~~mysql
-- 查看数据库文件存放路径
show variables like 'datadir';

+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.03 sec)
~~~

从结果中可以看出，在我的计算机上MySQL的数据目录就是   ` /var/lib/mysql/`



查看该目录

~~~shell
# 进入存放数据库文件目录
cd /var/lib/mysql/


# 查看该目录下的文件
ll

总用量 122952
-rw-r-----. 1 mysql mysql       56 7月   4 17:05 auto.cnf
-rw-------. 1 mysql mysql     1672 7月   4 17:05 ca-key.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 ca.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 client-cert.pem
-rw-------. 1 mysql mysql     1676 7月   4 17:05 client-key.pem
drwxr-x---. 2 mysql mysql       88 7月   5 14:26 dbtest1						    # 数据库
-rw-r-----. 1 mysql mysql      382 7月   5 00:09 ib_buffer_pool
-rw-r-----. 1 mysql mysql 12582912 7月   5 14:34 ibdata1
-rw-r-----. 1 mysql mysql 50331648 7月   5 14:34 ib_logfile0
-rw-r-----. 1 mysql mysql 50331648 7月   4 17:05 ib_logfile1
-rw-r-----. 1 mysql mysql 12582912 7月   5 10:14 ibtmp1
drwxr-x---. 2 mysql mysql     4096 7月   4 17:05 mysql                       # 数据库
srwxrwxrwx. 1 mysql mysql        0 7月   5 10:14 mysql.sock
-rw-------. 1 mysql mysql        5 7月   5 10:14 mysql.sock.lock
drwxr-x---. 2 mysql mysql     8192 7月   4 17:05 performance_schema					    # 数据库
-rw-------. 1 mysql mysql     1676 7月   4 17:05 private_key.pem
-rw-r--r--. 1 mysql mysql      452 7月   4 17:05 public_key.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 server-cert.pem
-rw-------. 1 mysql mysql     1676 7月   4 17:05 server-key.pem
drwxr-x---. 2 mysql mysql     8192 7月   4 17:05 sys							    # 数据库
~~~





## 1.2 相关命令目录

相关命令目录： 存放许多可执行文件的目录

mysqladmin、mysqlbinlog、mysqldump等命令

> /usr/bin

> /usr/sbin





## 1.3 配置文件目录

配置文件目录：

> /usr/share/mysql-8.0（命令及配置文件）
>
> /etc/mysql

> 如 : /etc/my.cnf





# 2. 数据库和文件系统的关系

像InnoDB、MyISAM这样的存储引擎都是**把表存储在磁盘上**的,**操作系统**用来管理**磁盘的结构**被称为**文件系统**

所以用专业一点的话来表述就是:像I**nnoDB、MyISAM这样的存储引擎都是把表存储在文件系统上的**；

当我们想读取数据的时候,这些存储引擎会从文件系统中把数据读出来返回给我们；

当我们想写入数据的时候,这些存储引擎会把这些数据又写回文件系统

本章学习一下：InnoDB 和MyISAM这两个存储引擎的数据如何在文件系统中存储



## 2.1 查看默认数据库 

查看一下在我的计算机上当前有哪些数据库：

~~~mysql
SHOW DATABASES;

+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbtest1            | -- 这个是自己创建的，其他四个是系统自带的
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.02 sec)
~~~



可以看到有4个数据库是属于MySQL自带的系统数据库

-  mysql

 MySQL 系统自带的核心数据库，它存储了MySQL的用户账户和权限信息，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，一些帮助信息以及时区信息等。 





- information_schema 

MySQL 系统自带的数据库，这个数据库保存着MySQL服务器 维护的所有其他数据库的信息 ，比如有 哪些表、哪些视图、哪些触发器、哪些列、哪些索引。这些信息并不是真实的用户数据，而是一些 描述性信息，有时候也称之为 元数据 。在系统数据库 information_schema 中提供了一些以 innodb_sys 开头的表，用于表示内部系统表。



~~~mysql
USE information_schema;
Database changed



SHOW TABLES LIKE 'innodb_sys%';
+--------------------------------------------+
| Tables_in_information_schema (innodb_sys%) |
+--------------------------------------------+
| INNODB_SYS_DATAFILES 					 	 |
| INNODB_SYS_VIRTUAL 					  	 |
| INNODB_SYS_INDEXES 						 |
| INNODB_SYS_TABLES							 |
| INNODB_SYS_FIELDS 						 |
| INNODB_SYS_TABLESPACES 					 |
| INNODB_SYS_FOREIGN_COLS 					 |
| INNODB_SYS_COLUMNS 						 |
| INNODB_SYS_FOREIGN						 |
| INNODB_SYS_TABLESTATS						 |
+--------------------------------------------+
10 rows in set (0.00 sec)

~~~



- performance_schema

 MySQL 系统自带的数据库，这个数据库里主要保存MySQL服务器运行过程中的一些状态信息，可以 用来 监控 MySQL 服务的各类性能指标  包括统计最近执行了哪些语句，在执行过程的每个阶段都 花费了多长时间，内存的使用情况等信息





- sys 

MySQL 系统自带的数据库，这个数据库主要是通过 视图 的形式把 information_schema 和 performance_schema 结合起来，帮助系统管理员和开发人员监控 MySQL 的技术性能





## 2.2 数据库在文件系统中的表示

MySQL5.7.28

~~~shell
cd /var/lib/mysql

ll

总用量 122952
-rw-r-----. 1 mysql mysql       56 7月   4 17:05 auto.cnf
-rw-------. 1 mysql mysql     1672 7月   4 17:05 ca-key.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 ca.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 client-cert.pem
-rw-------. 1 mysql mysql     1676 7月   4 17:05 client-key.pem
drwxr-x---. 2 mysql mysql       88 7月   5 14:26 dbtest1
-rw-r-----. 1 mysql mysql      382 7月   5 00:09 ib_buffer_pool
-rw-r-----. 1 mysql mysql 12582912 7月   5 14:34 ibdata1   	 #系统表空间 
-rw-r-----. 1 mysql mysql 50331648 7月   5 14:34 ib_logfile0
-rw-r-----. 1 mysql mysql 50331648 7月   4 17:05 ib_logfile1
-rw-r-----. 1 mysql mysql 12582912 7月   5 10:14 ibtmp1
drwxr-x---. 2 mysql mysql     4096 7月   4 17:05 mysql
srwxrwxrwx. 1 mysql mysql        0 7月   5 10:14 mysql.sock
-rw-------. 1 mysql mysql        5 7月   5 10:14 mysql.sock.lock
drwxr-x---. 2 mysql mysql     8192 7月   4 17:05 performance_schema
-rw-------. 1 mysql mysql     1676 7月   4 17:05 private_key.pem
-rw-r--r--. 1 mysql mysql      452 7月   4 17:05 public_key.pem
-rw-r--r--. 1 mysql mysql     1112 7月   4 17:05 server-cert.pem
-rw-------. 1 mysql mysql     1676 7月   4 17:05 server-key.pem
drwxr-x---. 2 mysql mysql     8192 7月   4 17:05 sys
~~~



这个数据目录下的文件和子目录比较多，除了 information_schema 这个系统数据库外，其他的数据库 在 数据目录 下都有对应的子目录

 以我的 dbtest1数据库为例，在MySQL5.7 中打开：

~~~mysql
use dbtest1;


show tables;

+-------------------+
| Tables_in_dbtest1 |
+-------------------+
| emp1              |
| mytb12            |
+-------------------+
2 rows in set (0.00 sec)

~~~



~~~shell
cd dbtest1/
ll

总用量 220
-rw-r-----. 1 mysql mysql    61 7月   5 10:18 db.opt   #存储该数据库的字符集、比较规则......
-rw-r-----. 1 mysql mysql  8588 7月   5 00:26 emp1.frm   #存储表结构
-rw-r-----. 1 mysql mysql 98304 7月   5 00:17 emp1.ibd	#默认放在此处（独立表空间）
-rw-r-----. 1 mysql mysql  8644 7月   5 14:26 mytb12.frm
-rw-r-----. 1 mysql mysql 98304 7月   5 14:34 mytb12.ibd
~~~







## 2.3 表在文件系统中的表示



### 2.3.1 InnoDB存储引擎模式 

1. 表结构 

为了保存表结构， InnoDB 在 数据目录 下对应的数据库子目录下创建了一个专门用于 描述表结构的文 件 

文件名是这样：

> 表名.frm



我们在dbtest1数据库中创建了一个表  `emp1`

那在数据库 dbtest1对应的子目录下就会创建一个名为 emp1.frm 的用于描述表结构的文件

> 基于MySQL5.7

~~~
-rw-r-----. 1 mysql mysql    61 7月   5 10:18 db.opt
-rw-r-----. 1 mysql mysql  8588 7月   5 00:26 emp1.frm
-rw-r-----. 1 mysql mysql 98304 7月   5 00:17 emp1.ibd
~~~



注意：

.frm文件 的格式在不同的平台上都是相同的

这个后缀名为.frm是以 二进制格式 存储的，我们直接打开是乱码的







2. 表中数据和索引

① 系统表空间（system tablespace） 默认情况下，InnoDB会在数据目录下创建一个名为 ibdata1 、大小为 12M 的文件，这个文件就是对应 的 系统表空间 在文件系统上的表示

怎么才12M？

注意这个文件是 自扩展文件 ，当不够用的时候它会自 己增加文件大小

当然，如果你想让系统表空间对应文件系统上多个实际文件，或者仅仅觉得原来的 ibdata1 这个文件名 难听

那可以在MySQL启动时配置对应的文件路径以及它们的大小

比如我们这样修改一下my.cnf 配置 文件：

~~~shell
[server]
innodb_data_file_path=data1:512M;data2:512M:autoextend
~~~

记得重启服务器



② 独立表空间(file-per-table tablespace)

在MySQL5.6.6以及之后的版本中，InnoDB并不会默认的把各个表的数据存储到系统表空间中，而是为 每 一个表建立一个独立表空间 

也就是说我们创建了多少个表，就有多少个独立表空间。使用 独立表空间 来 存储表数据的话，会在该表所属数据库对应的子目录下创建一个表示该独立表空间的文件，文件名和表 名相同，只不过添加了一个 .ibd 的扩展名而已

所以完整的文件名称长这样：

> 表名.ibd



**在8.0.16中打开**

~~~mysql
use dbtest1;


show tables;

+-------------------+
| Tables_in_dbtest1 |
+-------------------+
| emp1              |
| emp2              |
+-------------------+
2 rows in set (0.00 sec)

~~~



~~~mysql
cd dbtest1/
ll

总用量 160
-rw-r-----. 1 mysql mysql 114688 7月   5 15:31 emp1.ibd
-rw-r-----. 1 mysql mysql 114688 7月   5 15:31 emp2.ibd
~~~





疑问.frm在MySQL8中不存在了。那去哪里了呢?这就需要解析ibd文件

Oracle 官方将frm文件的信息及更多信息移动到叫做序列化字典信息(Serialized DictionaryInformation, SDI), SDI被写在ibd文件内部

MySQL 8.0属于Oracle旗下,同理。为了从IBD文件中提取SDI信息, Oracle提供了一个应用程序ibd2sdi

==ibd2sdi官方文档==这个工具不需要下载, **MySQL8自带**的有,只要你配好环境变量就能到处用





(1)查看表结构到存储ibd文件的目录下,执行下面的命令:

~~~shell
# 进入具体数据库文件
cd dbtest1/

# 查看所有文件
ll

总用量 160
-rw-r-----. 1 mysql mysql 114688 7月   5 15:31 emp1.ibd
-rw-r-----. 1 mysql mysql 114688 7月   5 15:31 emp2.ibd


# mysql8.0以上 直接使用该命令 生成可查看的.txt文件
ibd2sdi --dump-file=emp1.txt emp1.ibd

~~~







③ 系统表空间与独立表空间的设置 

我们可以自己指定使用 系统表空间 还是 独立表空间 来存储数据，这个功能由启动参数 innodb_file_per_table 控制

比如说我们想刻意将表数据都存储到 系统表空间 时，可以在启动 MySQL服务器的时候这样配置：

~~~shell
[server]
innodb_file_per_table=0 # 0：代表使用系统表空间； 1：代表使用独立表空间
~~~

记得重启服务器



默认情况：

~~~mysql
show variables like 'innodb_file_per_table';

+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set (0.05 sec)
~~~





④ 其他类型的表空间 随着MySQL的发展，除了上述两种老牌表空间之外，现在还新提出了一些不同类型的表空间

比如通用 表空间（general tablespace）、临时表空间（temporary tablespace）等。











### 2.3.2 MyISAM存储引擎模式

1. 表结构 

在存储表结构方面， MyISAM 和 InnoDB 一样，也是在 数据目录 下对应的数据库子目录下创建了一个专 门用于描述表结构的文件：

> 表名.frm



2. 表中数据和索引

在MyISAM中的索引全部都是 二级索引 ，该存储引擎的 **数据和索引是分开存放** 的

所以在文件系统中也是 使用不同的文件来存储数据文件和索引文件，同时表数据都存放在对应的数据库子目录下



假如 test 表使用MyISAM存储引擎的话，那么在它所在数据库对应的 atguigu 目录下会为 test 表创建这三个文 件：

~~~
test.frm 存储表结构
test.MYD 存储数据 (MYData)
test.MYI 存储索引 (MYIndex)
~~~





举例：

创建一个 MyISAM 表，使用 ENGINE 选项显式指定引擎。因为 InnoDB 是默认引擎

~~~mysql
-- 切换数据库
use dbtest1;

Database changed




-- 建表
CREATE TABLE `student_myisam` (
	`id` bigint NOT NULL AUTO_INCREMENT,
	`name` varchar(64) DEFAULT NULL,
	`age` int DEFAULT NULL,
	`sex` varchar(2) DEFAULT NULL,
	PRIMARY KEY (`id`)
)ENGINE=MYISAM AUTO_INCREMENT=0 DEFAULT CHARSET=utf8mb3;

Query OK, 0 rows affected, 1 warning (0.05 sec)




-- 查看数据库
show tables;
+-------------------+
| Tables_in_dbtest1 |
+-------------------+
| emp1              |
| mytb12            |
| student_myisam    |
+-------------------+
3 rows in set (0.00 sec)
~~~



当前数据库文件中有三个关于student_myisam的文件

> 基于MySQL05.7

~~~shell
-rw-r-----. 1 mysql mysql  8642 7月   5 16:44 student_myisam.frm
-rw-r-----. 1 mysql mysql     0 7月   5 16:44 student_myisam.MYD
-rw-r-----. 1 mysql mysql  1024 7月   5 16:44 student_myisam.MYI
~~~



> 基于MySQL8.0

~~~shell
-rw-r-----. 1 mysql mysql   4330 7月   5 16:43 student_myisam_362.sdi #相当于MySQL5.7中的 .frm
-rw-r-----. 1 mysql mysql      0 7月   5 16:43 student_myisam.MYD
-rw-r-----. 1 mysql mysql   1024 7月   5 16:43 student_myisam.MYI
~~~





## 2.4 小结

举例： 数据库a ， 表b 



1、如果表b采用 InnoDB ，data\a中会产生1个或者2个文件：

> 基于MySQL5.7

b.frm ：描述表结构文件，字段长度等 

如果采用 系统表空间 模式的，数据信息和索引信息都存储在 ibdata1 中 （MySQL5.6.6还在使用系统表空间）

如果采用 独立表空间 存储模式，data\a中还会产生 b.ibd 文件（存储数据信息和索引信息） （MySQL5.7默认使用）



此外： 

① MySQL5.7 中会在data/a的目录下生成 db.opt 文件用于保存数据库的相关配置。比如：字符集、比较 规则；

**而MySQL8.0不再提供db.opt文件**

② MySQL8.0中不再单独提供b.frm，而是合并在b.ibd文件中。 



2、如果表b采用 MyISAM ，data\a中会产生3个文件：

**MySQL5.7中**： b.frm ：描述表结构文件，字段长度等

**MySQL8.0中** b.xxx.sdi ：描述表结构文件，字段长度等



无论是MySQL5.7还是MySQL8.0中都有这部分：

b.MYD (MYData)：数据信息文件，存储数据信息(如果采用独立表存储模式) 

b.MYI (MYIndex)：存放索引信息文件





## 2.5视图在文件系统中的表示

我们知道MySQL中的视图其实是虚拟的表,也就是某个查询语句的一个别名而已

所以在存储视图的时候是不需要存储真实的数据的,只需要把它的结构存储起来就行了和表一样

描述视图结构的文件也会被存储到所属数据库对应的子目录下边,只会存储一个视图名.frm的文件，而没有对应的 .ibd文件







## 2.6其他的文件

除了我们上边说的这些用户自己存储的数据以外,数据目录下还包括为了更好运行程序的一些额外文件

主要包括这几种类型的文件:

- **服务器进程文件**

我们知道每运行一个MySQL服务器程序,都意味着启动一个进程。MySQL服务器会把自己的进程ID写入到一个文件中



- 服务器日志文件

在服务器运行过程中,会产生各种各样的日志,比如常规的查询日志、错误日志、二进制日志、redo日志等。这些日志各有各的用途,后面讲解



- 默认/自动生成的SSL和RSA证书和密钥文件

主要是为了客户端和服务器安全通信而创建的一些文件
