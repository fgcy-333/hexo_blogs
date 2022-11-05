---
title: 第01章_Linux下MySQL的安装与使用
date: 2022-08-28 20:46:46
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
---

# 1、安装前说明

## 1.1 Linux系统及工具的准备

安装并启动好两台虚拟机： CentOS 7
掌握克隆虚拟机的操作

- mac地址
- 主机名
- ip地址
- UUID



1、安装有 Xshell 和 Xftp 等访问CentOS系统的工具

2、CentOS6和CentOS7在MySQL的使用中的区别

- 防火墙：6是iptables，7是firewalld 
- 启动服务的命令：6是service，7是systemctl



<!--more-->

## 1.2 查看是否安装过MySQL

查看M有SQL版本

~~~shell
mysql --version


mysql  Ver 8.0.25 for Linux on x86_64 (MySQL Community Server - GPL)
~~~





如果你是用rpm安装, 检查一下RPM PACKAGE：

~~~shell
rpm -qa | grep -i mysql # -i 忽略大小写



mysql-community-client-8.0.25-1.el7.x86_64
mysql-community-common-8.0.25-1.el7.x86_64
mysql-community-server-8.0.25-1.el7.x86_64
mysql-community-client-plugins-8.0.25-1.el7.x86_64
mysql-community-libs-8.0.25-1.el7.x86_64

~~~





检查mysql service状态：

~~~shell
systemctl status mysqld.service



● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 一 2022-07-04 14:49:20 CST; 10min ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 1098 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 1489 (mysqld)
   Status: "Server is operational"
    Tasks: 37
   CGroup: /system.slice/mysqld.service
           └─1489 /usr/sbin/mysqld
~~~

当前服务处于运行状态





## 1.3 MySQL的卸载 

1、关闭 mysql 服务

~~~shell
systemctl stop mysqld.service 




# 查看状态
systemctl status mysqld.service
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: inactive (dead) since 一 2022-07-04 15:03:18 CST; 50s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 1489 ExecStart=/usr/sbin/mysqld $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 1098 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 1489 (code=exited, status=0/SUCCESS)
   Status: "Server shutdown complete"
~~~



2、查看当前 mysql 安装状况

~~~shell
rpm -qa | grep -i mysql 

# 或 yum list installed | grep mysql



mysql-community-client-8.0.25-1.el7.x86_64
mysql-community-common-8.0.25-1.el7.x86_64
mysql-community-server-8.0.25-1.el7.x86_64
mysql-community-client-plugins-8.0.25-1.el7.x86_64
mysql-community-libs-8.0.25-1.el7.x86_64
~~~



3、卸载上述命令查询出的已安装程序

> 格式： yum remove mysql-xxx mysql-xxx mysql-xxx mysqk-xxxx



~~~shell
# 删除文件
yum remove mysql-community-libs.x86_64 

已加载插件：fastestmirror, langpacks
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-libs.x86_64.0.8.0.25-1.el7 将被 删除
--> 正在处理依赖关系 mysql-community-libs(x86-64) >= 8.0.11，它被软件包 mysql-community-client-8.0.25-1.el7.x86_64 需要
--> 正在检查事务
---> 软件包 mysql-community-client.x86_64.0.8.0.25-1.el7 将被 删除
--> 正在处理依赖关系 mysql-community-client(x86-64) >= 8.0.11，它被软件包 mysql-community-server-8.0.25-1.el7.x86_64 需要
--> 正在检查事务
---> 软件包 mysql-community-server.x86_64.0.8.0.25-1.el7 将被 删除
--> 解决依赖关系完成

依赖关系解决

================================================================================================================================
 Package                                                     架构                                        版本                                                 源                                              大小
================================================================================================================================
正在删除:
 mysql-community-libs                                        x86_64                                      8.0.25-1.el7                                         installed                                       20 M
为依赖而移除:
 mysql-community-client                                      x86_64                                      8.0.25-1.el7                                         installed                                      220 M
 mysql-community-server                                      x86_64                                      8.0.25-1.el7                                         installed                                      2.0 G

事务概要
================================================================================================================================
移除  1 软件包 (+2 依赖软件包)

安装大小：2.2 G
是否继续？[y/N]：y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
警告：RPM 数据库已被非 yum 程序修改。
  正在删除    : mysql-community-server-8.0.25-1.el7.x86_64                                                                                                                                                     1/3 
  正在删除    : mysql-community-client-8.0.25-1.el7.x86_64                                                                                                                                                     2/3 
  正在删除    : mysql-community-libs-8.0.25-1.el7.x86_64                                                                                                                                                       3/3 
  验证中      : mysql-community-client-8.0.25-1.el7.x86_64                                                                                                                                                     1/3 
  验证中      : mysql-community-libs-8.0.25-1.el7.x86_64                                                                                                                                                       2/3 
  验证中      : mysql-community-server-8.0.25-1.el7.x86_64                                                                                                                                                     3/3 

删除:
  mysql-community-libs.x86_64 0:8.0.25-1.el7                                                                                                                                                                       

作为依赖被删除:
  mysql-community-client.x86_64 0:8.0.25-1.el7                                                             mysql-community-server.x86_64 0:8.0.25-1.el7                                                            

完毕！



# 查看剩余文件
 rpm -qa | grep -i mysql
 
mysql-community-common-8.0.25-1.el7.x86_64
mysql-community-client-plugins-8.0.25-1.el7.x86_64








#继续删除文件
yum remove mysql-community-client-plugins.x86_64 




#继续删除文件
yum remove mysql-community-common.x86_64






# 查看剩余文件
 rpm -qa | grep -i mysql
~~~

务必卸载干净，反复执行 rpm -qa | grep -i mysql 确认是否有卸载残留



4、删除 mysql 相关文件 



查找相关文件

~~~shell
find / -name mysql

/etc/selinux/targeted/active/modules/100/mysql
/etc/selinux/targeted/tmp/modules/100/mysql
/var/lib/mysql
/var/lib/mysql/mysql
/usr/lib64/mysql

~~~





递归删除上述命令查找出的相关文件

>  格式：rm -rf xxx



~~~shell
rm -rf /etc/selinux/targeted/active/modules/100/mysql
rm -rf /etc/selinux/targeted/active/modules/100/mysql
rm -rf /etc/selinux/targeted/tmp/modules/100/mysql
rm -rf /var/lib/mysql
rm -rf /var/lib/mysql/mysql
rm -rf /usr/lib64/mysql



#验证是否删除干净
find / -name mysql
~~~



5.删除 my.cnf

~~~shell
rm -rf /etc/my.cnf
~~~





tip:

关闭锁屏

---

![image-20220704152707664](http://img.fgcy.xyz/image-20220704152707664.png)

---





---

![image-20220704152737451](http://img.fgcy.xyz/image-20220704152737451.png)

---





# 2. MySQL的Linux版安装



## 2.1 MySQL的4大版本 

1、MySQL Community Server 社区版本，开源免费，自由下载，但不提供官方技术支持，适用于 大多数普通用户

2、MySQL Enterprise Edition 企业版本，需付费，不能在线下载，可以试用30天。提供了更多的 功能和更完备的技术支持，更适合于对数据库的功能和可靠性要	  求较高的企业客户

3、 MySQL Cluster 集群版，开源免费。用于架设集群服务器，可将几个MySQL Server封装成一个 Server。需要在社区版或企业版的基础上使用

 4、MySQL Cluster CGE 高级集群版，需付费





截止目前，官方最新版本为 MySQL Community Server 8.0.29

此前，8.0.0 在 2016.9.12日就发布了

本课程中**主要使用 8.0.25版本** 。同时为了更好的说明MySQL8.0新特性，还会安装 MySQL5.7 版 本，作为对比



此外，官方还提供了 MySQL Workbench （GUITOOL）一款专为MySQL设计的 ER/数据库建模工具 。

它是 著名的数据库设计工具DBDesigner4的继任者。MySQLWorkbench又分为两个版本，分别是 社区版 （MySQL Workbench OSS）、 商用版 （MySQL WorkbenchSE）







## 2.2 下载MySQL指定版本



>  下载地址 官网：https://www.mysql.com





打开官网，点击DOWNLOADS 然后，点击 MySQL Community(GPL) Downloads

---

![image-20220704153812291](http://img.fgcy.xyz/image-20220704153812291.png)

---





点击 MySQL Community Server

---

![image-20220704153851859](http://img.fgcy.xyz/image-20220704153851859.png)

---





选择历史版本：

----

![image-20220704153935653](http://img.fgcy.xyz/image-20220704153935653.png)

---







---

![image-20220704154138950](http://img.fgcy.xyz/image-20220704154138950-16569208557851.png)

---







下载的tar包，用压缩工具打开

解压后rpm安装包 



---

![image-20220704162629115](http://img.fgcy.xyz/image-20220704162629115.png)

---



---

![image-20220704162206879](http://img.fgcy.xyz/image-20220704162206879.png)

---



### 2.3 CentOS7下检查MySQL依赖

  1. 检查/tmp临时目录权限（必不可少） 由于mysql安装过程中，会通过mysql用户在/tmp目录下新建tmp_db文件，所以请给/tmp较大的权限。执 行 ：

~~~shell
chmod -R 777 /tmp
~~~



2. 安装前，检查依赖

~~~shell
# 1检
rpm -qa|grep libaio

libaio-0.3.109-13.el7.x86_64



# 2检
rpm -qa|grep net-tools
net-tools-2.0-0.25.20131004git.el7.x86_64
~~~



注意：

如果不存在需要到centos安装盘里进行rpm安装。安装linux如果带图形化界面，这些都是安装好 的





### 2.4 CentOS7下MySQL安装过程



1、将安装程序拷贝到/opt目录下

在mysql的安装文件目录下执行：（必须按照顺序执行）

~~~shell
rpm -ivh mysql-community-common-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-plugins-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-8.0.25-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-8.0.25-1.el7.x86_64.rpm
~~~



注意: 

如在检查工作时，没有检查mysql依赖环境在安装mysql-community-server会报错





参数解释：

~~~
rpm 是Redhat Package Manage缩写，通过RPM的管理，用户可以把源代码包装成以rpm为扩展名的 文件形式，易于安装。

-i , --install 安装软件包 
-v , --verbose 提供更多的详细信息输出 
-h , --hash 软件包安装的时候列出哈希标记 (和 -v 一起使用效果更好)，展示进度条
~~~





安装过程中可能的报错信息：

---

![image-20220704163300022](http://img.fgcy.xyz/image-20220704163300022.png)

---

解决：

> 一个命令：yum remove mysql-libs 解决，清除之前安装过的依赖即可





2、查看MySQL版本



查看MySQL版本 执行如下命令，如果成功表示安装mysql成功。类似java -version如果打出版本等信息

~~~shell
mysql --version
#或
mysqladmin --version


mysql  Ver 8.0.25 for Linux on x86_64 (MySQL Community Server - GPL)
~~~





执行如下命令，查看是否安装成功。需要增加 -i 不用去区分大小写，否则搜索不到

~~~shell
rpm -qa|grep -i mysql

mysql-community-client-8.0.25-1.el7.x86_64
mysql-community-common-8.0.25-1.el7.x86_64
mysql-community-server-8.0.25-1.el7.x86_64
mysql-community-client-plugins-8.0.25-1.el7.x86_64
mysql-community-libs-8.0.25-1.el7.x86_64

~~~







3、 服务的初始化 

为了保证数据库目录与文件的所有者为 mysql 登录用户，如果你是以 root 身份运行 mysql 服务，需要执 行下面的命令初始化：

~~~shell
mysqld --initialize --user=mysql
~~~



说明：

 --initialize 选项默认以“安全”模式来初始化，则会为 root 用户生成一个密码并将 该密码标记为过 期 ，登录后你需要设置一个新的密码

生成的 临时密码 会往日志中记录一份



查看密码：

~~~shell
cat /var/log/mysqld.log
~~~



---

![image-20220704164119274](http://img.fgcy.xyz/image-20220704164119274.png)

---



root@localhost: 后面就是初始化的密码







4、启动MySQL，查看状态

~~~shell
#加不加.service后缀都可以
#启动：
systemctl start mysqld.service





#关闭：
systemctl stop mysqld.service


#重启：


systemctl restart mysqld.service



#查看状态：
systemctl status mysqld.service

mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 一 2022-07-04 16:40:32 CST; 3s ago   # 启动状态
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 2942 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 2970 (mysqld)
   Status: "Server is operational"
    Tasks: 38
   CGroup: /system.slice/mysqld.service
           └─2970 /usr/sbin/mysqld

~~~





注意：

mysqld 这个可执行文件就代表着 MySQL 服务器程序，运行这个可执行文件就可以直接启动一个 服务器进程











5、查看MySQL服务是否自启动

~~~mysql
systemctl list-unit-files|grep mysqld.service




mysqld.service                                enabled 
~~~

默认是enabled



如不是enabled可以运行如下命令设置自启动

~~~shell
systemctl enable mysqld.service
~~~



如果希望不进行自启动，运行如下命令设置

~~~shell
systemctl disable mysqld.service
~~~

----

![image-20220704164656502](http://img.fgcy.xyz/image-20220704164656502.png)

---











## Linux系统下安装MySQL的几种方式 

 **Linux系统下安装软件的常用三种方式：** 



### 方式1：rpm命令

使用rpm命令安装扩展名为".rpm"的软件包



.rpm包的一般格式：

---

![image-20220704154411875](http://img.fgcy.xyz/image-20220704154411875.png)

---







### 方式2：yum命令

需联网，从 互联网获取 的yum源，直接使用yum命令安装





### 方式3：编译安装源码包

针对 tar.gz 这样的压缩格式，要用tar命令来解压；

如果是其它压缩格式，就使用其它命令







Linux系统下安装MySQL，官方给出多种安装方式

----

![image-20220704154630530](http://img.fgcy.xyz/image-20220704154630530.png)

---





这里不能直接选择CentOS 7系统的版本，所以选择与之对应的 Red Hat Enterprise Linux

https://downloads.mysql.com/archives/community/ 直接点Download下载RPM Bundle全量 包

包括了所有下面的组件。不需要一个一个下载了。







# 3. MySQL登录



## 3.1 首次登录 

通过 mysql -hlocalhost -P3306 -uroot -p 进行登录，在Enter password：录入初始化密码



~~~shell
mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.28

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> 

~~~





## 3.2 修改密码

因为初始化密码默认是过期的，所以查看数据库会报错 



修改密码：

~~~shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
~~~



5.7版本之后（不含5.7），mysql加入了全新的密码安全机制。设置新密码太简单会报错

---

![image-20220704171016461](http://img.fgcy.xyz/image-20220704171016461.png)

---



改为更复杂的密码规则之后，设置成功，可以正常使用数据库了

---

![image-20220704171053988](http://img.fgcy.xyz/image-20220704171053988.png)

---





## 3.3 设置远程登录

1、 当前问题 在用SQLyog或Navicat中配置远程连接Mysql数据库时遇到如下报错信息，这是由于Mysql配置了不支持远 程连接引起的。



---

![image-20220704171529501](http://img.fgcy.xyz/image-20220704171529501.png)

---







2、确认网络

 1.在远程机器上使用ping ip地址 保证网络畅通

~~~shell
ping 192.168.175.134

正在 Ping 192.168.175.134 具有 32 字节的数据:
来自 192.168.175.134 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.175.134 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.175.134 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.175.134 的回复: 字节=32 时间<1ms TTL=64

192.168.175.134 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 0ms，最长 = 0ms，平均 = 0ms
~~~



 2.在远程机器上使用telnet命令 保证端口号开放 访问

~~~shell
telnet 192.168.175.134 3306
~~~



拓展： telnet命令开启 :



---

![image-20220704171746126](http://img.fgcy.xyz/image-20220704171746126.png)

---





----

![image-20220704171801356](http://img.fgcy.xyz/image-20220704171801356.png)

---





---

![image-20220704171902604](http://img.fgcy.xyz/image-20220704171902604.png)

---



3、.关闭防火墙或开放端口



方式一：关闭防火墙



CentOS6 ：

~~~shell
service iptables stop
~~~



CentOS7：

~~~shell
# 开启防火墙
systemctl start firewalld.service

#查看防火墙状态
systemctl status firewalld.service


# 停用
systemctl stop firewalld.service

#设置开机启用防火墙
systemctl enable firewalld.service

#设置开机禁用防火墙
systemctl disable firewalld.service

~~~



方式二：开放端口



查看开放的端口号

~~~shell
firewall-cmd --list-all
~~~





设置开放的端口号

~~~shell
firewall-cmd --add-service=http --permanent


firewall-cmd --add-port=3306/tcp --permanent
~~~





重启防火墙

~~~shell
firewall-cmd --reload
~~~







4、Linux下修改配置 在Linux系统MySQL下测试：

~~~mysql
use mysql;

select Host,User from user;
~~~

----

![image-20220704221609794](http://img.fgcy.xyz/image-20220704221609794.png)

---



可以看到root用户的当前主机配置信息为localhost



这里修改Host为通配符%

1、Host列指定了允许用户登录所使用的IP，比如user=root Host=192.168.1.1

2、这里的意思就是说root用户只 能通过192.168.1.1的客户端去访问

3、user=root Host=localhost，表示只能通过本机客户端去访问

4、而 % 是个 通配符 ，如果Host=192.168.1.%，那么就表示只要是IP地址前缀为“192.168.1.”的客户端都可以连 接

5、如果 Host=% ，表示所有IP都有连接权限

~~~shell
#修改权限
update user
set host = '%'
where user ='root';



# Host修改完成后记得执行flush privileges使配置立即生效：
flush privileges;





#查看权限
select Host,User from user;

+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | root             |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
+-----------+------------------+
4 rows in set (0.00 sec)
~~~





注意：

在生产环境下不能为了省事将host设置为%，这样做会存在安全问题，具体的设置可以根据生产环境的IP进行设置





5、 测试



如果是 MySQL5.7 版本，接下来就可以使用SQLyog或者Navicat成功连接至MySQL了。

如果是 MySQL8 版本，连接时还会出现如下问题：

---

![image-20220704222031972](http://img.fgcy.xyz/image-20220704222031972.png)

---

问题：配置新连接报错：错误号码 2058，分析是 mysql 密码加密方法变了



解决方法：



1、Linux下 mysql -u root -p 登录你的 mysql 数据库

2、然后 执行这条SQL：

~~~shell
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'your_password';
~~~



3、然后在重新配置SQLyog的连接，则可连接成功了，OK





# 4. MySQL8的密码强度评估（了解）



## 4.1 MySQL不同版本设置密码(可能出现)

 MySQL5.7中：成功

~~~mysql
alter user 'root' identified by 'root123';

Query OK, 0 rows affected (0.00 sec)
~~~



MySQL8.0中：失败

~~~mysql
alter user 'root' identified by 'root123';

ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
~~~







## 4.2 MySQL8之前的安全策略 

在MySQL 8.0之前，MySQL使用的是validate_password插件检测、验证账号密码强度，保障账号的安全 性。



安装/启用插件方式1：在参数文件my.cnf中添加参数

> vim /etc/my.cnf



~~~shell
[mysqld]

# 添加如下内容
plugin-load-add=validate_password.so

#ON/OFF/FORCE/FORCE_PLUS_PERMANENT: 是否使用该插件(及强制/永久强制使用)

validate-password=FORCE_PLUS_PERMANENT
~~~



说明1： plugin library中的validate_password文件名的后缀名根据平台不同有所差异

​				对于Unix和 Unix-like系统而言，它的文件后缀名是.so，对于Windows系统而言，它的文件后缀名是.dll

说明2： 修改参数后**必须重启MySQL服务才能生效**

说明3： 参数FORCE_PLUS_PERMANENT是为了防止插件在MySQL运行时的时候被卸载。当你卸载插 件时就会报错。如下所示。





~~~mysql
-- 查看插件信息
SELECT PLUGIN_NAME, PLUGIN_LIBRARY, PLUGIN_STATUS, LOAD_OPTION
FROM INFORMATION_SCHEMA.PLUGINS
WHERE PLUGIN_NAME = 'validate_password';
 
+-------------------+----------------------+---------------+----------------------+
| PLUGIN_NAME       | PLUGIN_LIBRARY       | PLUGIN_STATUS | LOAD_OPTION          |
+-------------------+----------------------+---------------+----------------------+
| validate_password | validate_password.so | ACTIVE        | FORCE_PLUS_PERMANENT |
+-------------------+----------------------+---------------+----------------------+
1 row in set (0.00 sec)



-- 当卸载插件时，会报错
UNINSTALL PLUGIN validate_password;
ERROR 1702 (HY000): Plugin 'validate_password' is force_plus_permanent and can not be
unloaded
~~~





安装/启用插件方式2：运行时命令安装（推荐）

~~~mysql
INSTALL PLUGIN validate_password SONAME 'validate_password.so';


Query OK, 0 rows affected, 1 warning (0.11 sec)
~~~

此方法也会注册到元数据，也就是mysql.plugin表中，所以不用担心MySQL重启后插件会失效





## 4.3 MySQL8的安全策略

MySQL 8.0，引入了服务器组件（Components）这个特性，validate_password插件已用服务器组件重新实现。

8.0.25版本的数据库中，默认自动安装validate_password组件。



 未安装插件前，执行如下两个指令 ，执行效果：

~~~mysql
show variables like 'validate_password%';
Empty set (0.04 sec)


SELECT * FROM mysql.component;
ERROR 1146 (42S02): Table 'mysql.component' doesn't exist
~~~





安装插件后，执行如下两个指令 ，执行效果：

~~~mysql
SELECT * FROM mysql.component;

+--------------+--------------------+------------------------------------+
| component_id | component_group_id | component_urn                      |
+--------------+--------------------+------------------------------------+
| 1            | 1                  | file://component_validate_password |
+--------------+--------------------+------------------------------------+
1 row in set (0.00 sec)




show variables like 'validate_password%';

+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec
~~~





关于 validate_password 组件对应的系统变量说明：

----

![image-20220704224157306](http://img.fgcy.xyz/image-20220704224157306.png)

---

提示： 

组件和插件的默认值可能有所不同。例如，MySQL 5.7. validate_password_check_user_name的默认 值为OFF



**修改安全策略**

修改密码验证安全强度

~~~mysql
SET GLOBAL validate_password_policy=LOW;

SET GLOBAL validate_password_policy=MEDIUM;

SET GLOBAL validate_password_policy=STRONG;

SET GLOBAL validate_password_policy=0; # For LOW

SET GLOBAL validate_password_policy=1; # For MEDIUM

SET GLOBAL validate_password_policy=2; # For HIGH

#注意，如果是插件的话,SQL为set global validate_password_policy=LOW
~~~





此外，还可以修改密码中字符的长度

~~~mysql
set global validate_password_length=1;
~~~





**密码强度测试**

如果你创建密码是遇到“Your password does not satisfy the current policy requirements”，可以通过函数组 件去检测密码是否满足条件： 0-100

当评估在100时就是说明使用上了最基本的规则：大写+小写+特殊 字符+数字组成的8位以上密码



~~~mysql
SELECT VALIDATE_PASSWORD_STRENGTH('medium');

+--------------------------------------+
| VALIDATE_PASSWORD_STRENGTH('medium') |
+--------------------------------------+
| 25                                   |
+--------------------------------------+
1 row in set (0.00 sec)





SELECT VALIDATE_PASSWORD_STRENGTH('K354*45jKd5');
+-------------------------------------------+
| VALIDATE_PASSWORD_STRENGTH('K354*45jKd5') |
+-------------------------------------------+
|                   100                     |
+-------------------------------------------+
1 row in set (0.00 sec)

~~~

注意：

如果没有安装validate_password组件或插件的话，那么这个函数永远都返回0



 关于密码复杂度对 应的密码复杂度策略。如下表格所示：

---

![image-20220704224730865](http://img.fgcy.xyz/image-20220704224730865.png)

---



## 4.4 卸载插件、组件(了解)



卸载插件

~~~mysql
UNINSTALL PLUGIN validate_password;

Query OK, 0 rows affected, 1 warning (0.01 sec)
~~~



卸载组件

~~~mysql
UNINSTALL COMPONENT 'file://component_validate_password';

Query OK, 0 rows affected (0.02 sec)
~~~





# 5. 字符集的相关操作





## 5.1 修改MySQL5.7字符集



**1、修改步骤**

在MySQL 8.0版本之前，默认字符集为 latin1 ，utf8字符集指向的是 utf8mb3 

网站开发人员在数据库 设计的时候往往会将编码修改为utf8字符集

如果遗忘修改默认的编码，就会出现乱码的问题

从MySQL 8.0开始，数据库的默认编码将改为 utf8mb4 ，从而避免上述乱码的问题



操作1：查看默认使用的字符集

~~~mysql
show variables like 'character%';

+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)


# 或者
show variables like '%char%';

+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     | -- 默认使用该字符集
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     | -- 服务器使用该字符集
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
~~~

注意：

MySQL 5.7 默认的客户端和服务器都用了 latin1 ，不支持中文，保存中文会报错





~~~mysql
-- 使用默认字符集建库
CREATE DATABASE dbtest1;

Query OK, 1 row affected (0.01 sec)



-- 切换数据库
use dbtest1;

Database changed

-- 使用当前数据库的字符集 作为建表的字符集
CREATE TABLE emp1(
	id INT,
    lname varchar(15)
);

Query OK, 0 rows affected (0.01 sec)
~~~



~~~mysql
-- 在MySQL5.7中添加中文数据时，报错：
INSERT INTO emp1 (id,lname)
VALUES(1,'张三');

ERROR 1366 (HY000): Incorrect string value: '\xD5\xC5\xC8\xFD' for column 'lname' at row 1




-- 因为默认情况下，创建表使用的是 latin1 。如下：

-- 查看建库语句
SHOW CREATE DATABASE dbtest1;

+----------+--------------------------------------------------------------------+
| Database | Create Database                                                    |
+----------+--------------------------------------------------------------------+
| dbtest1  | CREATE DATABASE `dbtest1` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+--------------------------------------------------------------------+
1 row in set (0.01 sec)



-- 查看建表语句
SHOW CREATE TABLE emp1;

+-------+----------------------------------------------+
| Table |         Create Table                         |                                                                                 |                                              |   
+-------+-----------------------------------------------
| emp1  | CREATE TABLE `emp1` (                        |
  `id` int(11) DEFAULT NULL,
  `lname` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1                 |
+-------+----------------------------------------------+
1 row in set (0.00 sec)
~~~





**操作2：修改字符集**



~~~shell
vim /etc/my.cnf
~~~





在文件最后 [mysqld]之后 加上中文字符集配置：

~~~shell
character_set_server=utf8
~~~



**操作3：重新启动MySQL服务**

~~~shell
systemctl restart mysqld
~~~



注意：

原库、原表的设定不会发生变化，参数修改只对新建的数据库生效





字符集已经变更

~~~mysql
show variables like '%character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | gbk                        |
| character_set_connection | gbk                        |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | gbk                        |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
~~~







**已有库&表字符集的变更**



MySQL5.7版本中，以前创建的库，创建的表字符集还是latin1

~~~mysql
-- 查看建库语句
SHOW CREATE DATABASE dbtest1;

+----------+--------------------------------------------------------------------+
| Database | Create Database                                                    |
+----------+--------------------------------------------------------------------+
| dbtest1  | CREATE DATABASE `dbtest1` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+--------------------------------------------------------------------+
1 row in set (0.01 sec)



-- 查看建表语句
SHOW CREATE TABLE emp1;

+-------+----------------------------------------------+
| Table |         Create Table                         |                                                                                 |                                              |   
+-------+-----------------------------------------------
| emp1  | CREATE TABLE `emp1` (                        |
  `id` int(11) DEFAULT NULL,
  `lname` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1                 |
+-------+----------------------------------------------+
1 row in set (0.00 sec)
~~~





修改已创建数据库的字符集

~~~mysql
alter database dbtest1 character set 'utf8';

Query OK, 1 row affected (0.00 sec)
~~~







修改已创建数据表的字符集

~~~mysql
-- alter table emp1 convert to character set 'utf8';

alter table emp1  character set 'utf8';


Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
~~~



~~~mysql
-- 查看建库语句
SHOW CREATE DATABASE dbtest1;

+----------+--------------------------------------------------------------------+
| Database | Create Database                                                    |
+----------+--------------------------------------------------------------------+
| dbtest1  |  CREATE DATABASE `dbtest1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+--------------------------------------------------------------------+
1 row in set (0.01 sec)



-- 查看建表语句
SHOW CREATE TABLE emp1;

+-------+----------------------------------------------+
| Table |         Create Table                         |                                                                                 |                                              |   
+-------+-----------------------------------------------
| emp1  | CREATE TABLE `emp1` (                        |
  `id` int(11) DEFAULT NULL,
  `lname` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8                   |
+-------+----------------------------------------------+
1 row in set (0.00 sec)
~~~





~~~mysql
-- 添加数据
INSERT INTO emp1 (id,lname)
VALUES(1,'张三');

Query OK, 1 row affected (0.00 sec)





-- 查看数据
SELECT * FROM emp1;

+------+-------+
| id   | lname |
+------+-------+
|    1 | 张三  |
+------+-------+
1 row in set (0.00 sec)
~~~

注意：

但是原有的数据如果是用非'utf8'编码的话，数据本身编码不会发生改变。已有数据需要导 出或删除，然后重新插入







## 5.2 各级别的字符集



MySQL有4个级别的字符集和比较规则，

分别是：

1、服务器级别

2、数据库级别

3、表级别

4、列级别



执行如下SQL语句：

~~~mysql
show variables like 'character%';

+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | gbk                        |
| character_set_connection | gbk                        |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | gbk                        |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
~~~



character_set_server：服务器级别的字符集 

character_set_database：当前数据库的字符集

 character_set_client：服务器解码请求时使用的字符集

 character_set_connection：服务器处理请求时会把请求字符串从

character_set_client转为 character_set_connection 

character_set_results：服务器向客户端返回数据时使用的字符集



1. 服务器级别

character_set_server ：服务器级别的字符集

我们可以在启动服务器程序时通过启动选项或者在服务器程序运行过程中使用 SET 语句修改这两个变量（字符集与比较规则）的值

比如我们可以在配置文件中这样写：

~~~shell
server]
character_set_server=gbk # 默认字符集
collation_server=gbk_chinese_ci #对应的默认的比较规则
~~~

重启服务

当服务器启动的时候读取这个配置文件后这两个系统变量的值便修改了



2. 数据库级别

 character_set_database ：当前数据库的字符集

我们在创建和修改数据库的时候可以指定该数据库的字符集和比较规则，具体语法如下：

~~~mysql
-- 建库时指定字符集
CREATE DATABASE 数据库名
[[DEFAULT] CHARACTER SET 字符集名称]
[[DEFAULT] COLLATE 比较规则名称];


-- 建库后指定修改字符集
ALTER DATABASE 数据库名
[[DEFAULT] CHARACTER SET 字符集名称]
[[DEFAULT] COLLATE 比较规则名称];

~~~





3. 表级别

我们也可以在创建和修改表的时候指定表的字符集和比较规则，语法如下：

~~~mysql
-- 建表时指定字符集
CREATE TABLE 表名 (列的信息)
[[DEFAULT] CHARACTER SET 字符集名称]
[COLLATE 比较规则名称]]




-- 建表后指定修改字符集
ALTER TABLE 表名
[[DEFAULT] CHARACTER SET 字符集名称]
[COLLATE 比较规则名称]
~~~



注意：

如果创建和修改表的语句中没有指明字符集和比较规则，将使用该表所在数据库的字符集和比较规则作 为该表的字符集和比较规则



4. 列级别 

对于存储字符串的列，同一个表中的不同的列也可以有不同的字符集和比较规则

我们在创建和修改列 定义的时候可以指定该列的字符集和比较规则语法如下：

~~~mysql
-- 建表时指定字段的字符集
CREATE TABLE 表名(
列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称],
其他列...
);




-- 建表后 指定字段的字符集
ALTER TABLE 表名 
MODIFY 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称];

~~~

对于某个列来说，如果在创建和修改的语句中没有指明字符集和比较规则，将使用该列所在表的字符集 和比较规则作为该列的字符集和比较规则



注意：

1、server决定database ，修改配置文件改的时server

2、如果 创建或修改列 时没有显式的指定字符集和比较规则，则该列 默认用表的 字符集和比较规则

3、如果 创建表时 没有显式的指定字符集和比较规则，则该表 默认用数据库的 字符集和比较规则 

4、如果 创建数据库时 没有显式的指定字符集和比较规则，则该数据库 默认用服务器的 字符集和比较规则







提示 

在转换列的字符集时需要注意，如果转换前列中存储的数据不能用转换后的字符集进行表示会发生 错误

比方说原先列使用的字符集是utf8，列中存储了一些汉字，现在把列的字符集转换为ascii的 话就会出错，因为ascii字符集并不能表示汉字字符





知道了这些规则之后，对于给定的表，我们应该知道它的各个列的字符集和比较规则是什么，从而根据 这个列的类型来确定存储数据时每个列的实际数据占用的存储空间大小了。

比方说我们向表 t 中插入一 条记录：

~~~mysql
INSERT INTO t(col) VALUES('我们');

Query OK, 1 row affected (0.00 sec)


SELECT * FROM t;

+--------+
| s      |
+--------+
| 我们    |
+--------+
1 row in set (0.00 sec)

~~~



首先列 col 使用的字符集是 gbk 

一个字符 '我' 在 gbk 中的编码为 0xCED2 ，占用两个字节，两个字 符的实际数据就占用4个字节

如果把该列的字符集修改为 utf8 的话，这两个字符就实际占用6个字节







## 5.3 字符集与比较规则(了解)

1、utf8 与 utf8mb4

utf8 字符集表示一个字符需要使用1～4个字节，但是我们常用的一些字符使用1～3个字节就可以表示 了

而字符集表示一个字符所用的最大字节长度，在某些方面会影响系统的存储和性能

所以设计 MySQL的设计者偷偷的定义了两个概念：

- utf8mb3 ：阉割过的 utf8 字符集，只使用1～3个字节表示字符

- utf8mb4 ：正宗的 utf8 字符集，使用1～4个字节表示字符



2、比较规则

上表中，MySQL版本一共支持41种字符集，其中的 Default collation 列表示这种字符集中一种默认 的比较规则，里面包含着该比较规则主要作用于哪种语言

比如 utf8_polish_ci 表示以波兰语的规则 比较， utf8_spanish_ci 是以西班牙语的规则比较， utf8_general_ci 是一种通用的比较规则

后缀表示该比较规则是否区分语言中的重音、大小写。具体如下：

---

![image-20220705093632533](http://img.fgcy.xyz/image-20220705093632533.png)

---





查看字符集：

~~~mysql
 SHOW CHARSET;
 
+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| armscii8 | ARMSCII-8 Armenian              | armscii8_general_ci |      1 |
| ascii    | US ASCII                        | ascii_general_ci    |      1 |
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| binary   | Binary pseudo charset           | binary              |      1 |
| cp1250   | Windows Central European        | cp1250_general_ci   |      1 |
| cp1251   | Windows Cyrillic                | cp1251_general_ci   |      1 |
| cp1256   | Windows Arabic                  | cp1256_general_ci   |      1 |
| cp1257   | Windows Baltic                  | cp1257_general_ci   |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
| cp852    | DOS Central European            | cp852_general_ci    |      1 |
| cp866    | DOS Russian                     | cp866_general_ci    |      1 |
| cp932    | SJIS for Windows Japanese       | cp932_japanese_ci   |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
| euckr    | EUC-KR Korean                   | euckr_korean_ci     |      2 |
| gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 |
| gb2312   | GB2312 Simplified Chinese       | gb2312_chinese_ci   |      2 |
| gbk      | GBK Simplified Chinese          | gbk_chinese_ci      |      2 |
| geostd8  | GEOSTD8 Georgian                | geostd8_general_ci  |      1 |
| greek    | ISO 8859-7 Greek                | greek_general_ci    |      1 |
| hebrew   | ISO 8859-8 Hebrew               | hebrew_general_ci   |      1 |
| hp8      | HP West European                | hp8_english_ci      |      1 |
| keybcs2  | DOS Kamenicky Czech-Slovak      | keybcs2_general_ci  |      1 |
| koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
| koi8u    | KOI8-U Ukrainian                | koi8u_general_ci    |      1 |
| latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
| latin2   | ISO 8859-2 Central European     | latin2_general_ci   |      1 |
| latin5   | ISO 8859-9 Turkish              | latin5_turkish_ci   |      1 |
| latin7   | ISO 8859-13 Baltic              | latin7_general_ci   |      1 |
| macce    | Mac Central European            | macce_general_ci    |      1 |
| macroman | Mac West European               | macroman_general_ci |      1 |
| sjis     | Shift-JIS Japanese              | sjis_japanese_ci    |      2 |
| swe7     | 7bit Swedish                    | swe7_swedish_ci     |      1 |
| tis620   | TIS620 Thai                     | tis620_thai_ci      |      1 |
| ucs2     | UCS-2 Unicode                   | ucs2_general_ci     |      2 |
| ujis     | EUC-JP Japanese                 | ujis_japanese_ci    |      3 |
| utf16    | UTF-16 Unicode                  | utf16_general_ci    |      4 |
| utf16le  | UTF-16LE Unicode                | utf16le_general_ci  |      4 |
| utf32    | UTF-32 Unicode                  | utf32_general_ci    |      4 |
| utf8     | UTF-8 Unicode                   | utf8_general_ci     |      3 |
| utf8mb4  | UTF-8 Unicode                   | utf8mb4_0900_ai_ci  |      4 |
+----------+---------------------------------+---------------------+--------+
41 rows in set (0.01 sec)

-- 或者
-- SHOW CHARACTER SET;
~~~



Default collation 对应的是该字符集的比较规则 

最后一列 Maxlen ，它代表该种字符集表示一个字符最多需要几个字节



常见的字符集和Maxlen对应关系如下：

---

![image-20220705100940341](http://img.fgcy.xyz/image-20220705100940341.png)

---





常用操作1：

~~~mysql
#查看GBK字符集的比较规则
SHOW COLLATION LIKE 'gbk%';

+----------------+---------+----+---------+----------+---------+---------------+
| Collation      | Charset | Id | Default | Compiled | Sortlen | Pad_attribute |
+----------------+---------+----+---------+----------+---------+---------------+
| gbk_bin        | gbk     | 87 |         | Yes      |       1 | PAD SPACE     |
| gbk_chinese_ci | gbk     | 28 | Yes     | Yes      |       1 | PAD SPACE     |
+----------------+---------+----+---------+----------+---------+---------------+
2 rows in set (0.00 sec)





#查看UTF-8字符集的比较规则
SHOW COLLATION LIKE 'utf8%';

+----------------------------+---------+-----+---------+----------+---------+---------------+
| Collation                  | Charset | Id  | Default | Compiled | Sortlen | Pad_attribute |
+----------------------------+---------+-----+---------+----------+---------+---------------+
| utf8mb4_0900_ai_ci         | utf8mb4 | 255 | Yes     | Yes      |       0 | NO PAD        |
| utf8mb4_0900_as_ci         | utf8mb4 | 305 |         | Yes      |       0 | NO PAD        |
| utf8mb4_0900_as_cs         | utf8mb4 | 278 |         | Yes      |       0 | NO PAD        |
| utf8mb4_0900_bin           | utf8mb4 | 309 |         | Yes      |       1 | NO PAD        |
| utf8mb4_bin                | utf8mb4 |  46 |         | Yes      |       1 | PAD SPACE     |
| utf8mb4_croatian_ci        | utf8mb4 | 245 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_cs_0900_ai_ci      | utf8mb4 | 266 |         | Yes      |       0 | NO PAD        |
| utf8mb4_cs_0900_as_cs      | utf8mb4 | 289 |         | Yes      |       0 | NO PAD        |
| utf8mb4_czech_ci           | utf8mb4 | 234 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_danish_ci          | utf8mb4 | 235 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_da_0900_ai_ci      | utf8mb4 | 267 |         | Yes      |       0 | NO PAD        |
| utf8mb4_da_0900_as_cs      | utf8mb4 | 290 |         | Yes      |       0 | NO PAD        |
| utf8mb4_de_pb_0900_ai_ci   | utf8mb4 | 256 |         | Yes      |       0 | NO PAD        |
| utf8mb4_de_pb_0900_as_cs   | utf8mb4 | 279 |         | Yes      |       0 | NO PAD        |
| utf8mb4_eo_0900_ai_ci      | utf8mb4 | 273 |         | Yes      |       0 | NO PAD        |
| utf8mb4_eo_0900_as_cs      | utf8mb4 | 296 |         | Yes      |       0 | NO PAD        |
| utf8mb4_esperanto_ci       | utf8mb4 | 241 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_estonian_ci        | utf8mb4 | 230 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_es_0900_ai_ci      | utf8mb4 | 263 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_0900_as_cs      | utf8mb4 | 286 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_trad_0900_ai_ci | utf8mb4 | 270 |         | Yes      |       0 | NO PAD        |
| utf8mb4_es_trad_0900_as_cs | utf8mb4 | 293 |         | Yes      |       0 | NO PAD        |
| utf8mb4_et_0900_ai_ci      | utf8mb4 | 262 |         | Yes      |       0 | NO PAD        |
| utf8mb4_et_0900_as_cs      | utf8mb4 | 285 |         | Yes      |       0 | NO PAD        |
| utf8mb4_general_ci         | utf8mb4 |  45 |         | Yes      |       1 | PAD SPACE     | --  一般用这个
| utf8mb4_german2_ci         | utf8mb4 | 244 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_hr_0900_ai_ci      | utf8mb4 | 275 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hr_0900_as_cs      | utf8mb4 | 298 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hungarian_ci       | utf8mb4 | 242 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_hu_0900_ai_ci      | utf8mb4 | 274 |         | Yes      |       0 | NO PAD        |
| utf8mb4_hu_0900_as_cs      | utf8mb4 | 297 |         | Yes      |       0 | NO PAD        |
| utf8mb4_icelandic_ci       | utf8mb4 | 225 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_is_0900_ai_ci      | utf8mb4 | 257 |         | Yes      |       0 | NO PAD        |
| utf8mb4_is_0900_as_cs      | utf8mb4 | 280 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ja_0900_as_cs      | utf8mb4 | 303 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ja_0900_as_cs_ks   | utf8mb4 | 304 |         | Yes      |      24 | NO PAD        |
| utf8mb4_latvian_ci         | utf8mb4 | 226 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_la_0900_ai_ci      | utf8mb4 | 271 |         | Yes      |       0 | NO PAD        |
| utf8mb4_la_0900_as_cs      | utf8mb4 | 294 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lithuanian_ci      | utf8mb4 | 236 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_lt_0900_ai_ci      | utf8mb4 | 268 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lt_0900_as_cs      | utf8mb4 | 291 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lv_0900_ai_ci      | utf8mb4 | 258 |         | Yes      |       0 | NO PAD        |
| utf8mb4_lv_0900_as_cs      | utf8mb4 | 281 |         | Yes      |       0 | NO PAD        |
| utf8mb4_persian_ci         | utf8mb4 | 240 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_pl_0900_ai_ci      | utf8mb4 | 261 |         | Yes      |       0 | NO PAD        |
| utf8mb4_pl_0900_as_cs      | utf8mb4 | 284 |         | Yes      |       0 | NO PAD        |
| utf8mb4_polish_ci          | utf8mb4 | 229 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_romanian_ci        | utf8mb4 | 227 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_roman_ci           | utf8mb4 | 239 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_ro_0900_ai_ci      | utf8mb4 | 259 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ro_0900_as_cs      | utf8mb4 | 282 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ru_0900_ai_ci      | utf8mb4 | 306 |         | Yes      |       0 | NO PAD        |
| utf8mb4_ru_0900_as_cs      | utf8mb4 | 307 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sinhala_ci         | utf8mb4 | 243 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sk_0900_ai_ci      | utf8mb4 | 269 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sk_0900_as_cs      | utf8mb4 | 292 |         | Yes      |       0 | NO PAD        |
| utf8mb4_slovak_ci          | utf8mb4 | 237 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_slovenian_ci       | utf8mb4 | 228 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sl_0900_ai_ci      | utf8mb4 | 260 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sl_0900_as_cs      | utf8mb4 | 283 |         | Yes      |       0 | NO PAD        |
| utf8mb4_spanish2_ci        | utf8mb4 | 238 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_spanish_ci         | utf8mb4 | 231 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_sv_0900_ai_ci      | utf8mb4 | 264 |         | Yes      |       0 | NO PAD        |
| utf8mb4_sv_0900_as_cs      | utf8mb4 | 287 |         | Yes      |       0 | NO PAD        |
| utf8mb4_swedish_ci         | utf8mb4 | 232 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_tr_0900_ai_ci      | utf8mb4 | 265 |         | Yes      |       0 | NO PAD        |
| utf8mb4_tr_0900_as_cs      | utf8mb4 | 288 |         | Yes      |       0 | NO PAD        |
| utf8mb4_turkish_ci         | utf8mb4 | 233 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_unicode_520_ci     | utf8mb4 | 246 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_unicode_ci         | utf8mb4 | 224 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_vietnamese_ci      | utf8mb4 | 247 |         | Yes      |       8 | PAD SPACE     |
| utf8mb4_vi_0900_ai_ci      | utf8mb4 | 277 |         | Yes      |       0 | NO PAD        |
| utf8mb4_vi_0900_as_cs      | utf8mb4 | 300 |         | Yes      |       0 | NO PAD        |
| utf8mb4_zh_0900_as_cs      | utf8mb4 | 308 |         | Yes      |       0 | NO PAD        |
| utf8_bin                   | utf8    |  83 |         | Yes      |       1 | PAD SPACE     |
| utf8_croatian_ci           | utf8    | 213 |         | Yes      |       8 | PAD SPACE     |
| utf8_czech_ci              | utf8    | 202 |         | Yes      |       8 | PAD SPACE     |
| utf8_danish_ci             | utf8    | 203 |         | Yes      |       8 | PAD SPACE     |
| utf8_esperanto_ci          | utf8    | 209 |         | Yes      |       8 | PAD SPACE     |
| utf8_estonian_ci           | utf8    | 198 |         | Yes      |       8 | PAD SPACE     |
| utf8_general_ci            | utf8    |  33 | Yes     | Yes      |       1 | PAD SPACE     |
| utf8_general_mysql500_ci   | utf8    | 223 |         | Yes      |       1 | PAD SPACE     |
| utf8_german2_ci            | utf8    | 212 |         | Yes      |       8 | PAD SPACE     |
| utf8_hungarian_ci          | utf8    | 210 |         | Yes      |       8 | PAD SPACE     |
| utf8_icelandic_ci          | utf8    | 193 |         | Yes      |       8 | PAD SPACE     |
| utf8_latvian_ci            | utf8    | 194 |         | Yes      |       8 | PAD SPACE     |
| utf8_lithuanian_ci         | utf8    | 204 |         | Yes      |       8 | PAD SPACE     |
| utf8_persian_ci            | utf8    | 208 |         | Yes      |       8 | PAD SPACE     |
| utf8_polish_ci             | utf8    | 197 |         | Yes      |       8 | PAD SPACE     |
| utf8_romanian_ci           | utf8    | 195 |         | Yes      |       8 | PAD SPACE     |
| utf8_roman_ci              | utf8    | 207 |         | Yes      |       8 | PAD SPACE     |
| utf8_sinhala_ci            | utf8    | 211 |         | Yes      |       8 | PAD SPACE     |
| utf8_slovak_ci             | utf8    | 205 |         | Yes      |       8 | PAD SPACE     |
| utf8_slovenian_ci          | utf8    | 196 |         | Yes      |       8 | PAD SPACE     |
| utf8_spanish2_ci           | utf8    | 206 |         | Yes      |       8 | PAD SPACE     |
| utf8_spanish_ci            | utf8    | 199 |         | Yes      |       8 | PAD SPACE     |
| utf8_swedish_ci            | utf8    | 200 |         | Yes      |       8 | PAD SPACE     |
| utf8_tolower_ci            | utf8    |  76 |         | Yes      |       1 | PAD SPACE     |
| utf8_turkish_ci            | utf8    | 201 |         | Yes      |       8 | PAD SPACE     |
| utf8_unicode_520_ci        | utf8    | 214 |         | Yes      |       8 | PAD SPACE     |
| utf8_unicode_ci            | utf8    | 192 |         | Yes      |       8 | PAD SPACE     |
| utf8_vietnamese_ci         | utf8    | 215 |         | Yes      |       8 | PAD SPACE     |
+----------------------------+---------+-----+---------+----------+---------+---------------+
103 rows in set (0.00 sec)
~~~



常用操作2：

~~~mysql
#查看服务器的字符集和比较规则
SHOW VARIABLES LIKE '%_server';

+-------------------------+--------------------+
| Variable_name           | Value              |
+-------------------------+--------------------+
| character_set_server    | utf8mb4            |
| collation_server        | utf8mb4_0900_ai_ci |
| innodb_dedicated_server | OFF                |
+-------------------------+--------------------+
3 rows in set, 1 warning (0.01 sec)




#查看数据库的字符集和比较规则
SHOW VARIABLES LIKE '%_database';

+------------------------+--------------------+
| Variable_name          | Value              |
+------------------------+--------------------+
| character_set_database | utf8mb4            |
| collation_database     | utf8mb4_0900_ai_ci |
| skip_show_database     | OFF                |
+------------------------+--------------------+
3 rows in set, 1 warning (0.00 sec)







#查看具体数据库的字符集
SHOW CREATE DATABASE dbtest1;

+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| dbtest1  | CREATE DATABASE `dbtest1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.01 sec)





#修改具体数据库的字符集、比较规则
ALTER DATABASE dbtest1 CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';

Query OK, 1 row affected (0.00 sec)


#查看具体数据库的字符集
SHOW CREATE DATABASE dbtest1;

+----------+------------------------------------------------------------------+
| Database | Create Database                                                  |
+----------+------------------------------------------------------------------+
| dbtest1  | CREATE DATABASE `dbtest1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+------------------------------------------------------------------+
1 row in set (0.00 sec)
~~~



说明一：

utf8_unicode_ci和utf8_general_ci对中、英文来说没有实质的差别

utf8_general_ci 校对速度快,但准确度稍差

utf8_unicode_ci准确度高,但校对速度稍慢

一般情况,用utf8_general_ci就够了,但如果你的应用有德语、法语或者俄语,请一定使用utf8_unicode_ci



说明二：

修改了数据库的默认字符集和比较规则后,原来已经创建的表格的字符集和比较规则并不会改变,如果需要,那么需单独修改





常用操作3：

~~~mysql
#查看表的字符集
show create table emp1;

+-------+-----------------------------------------+
| Table | Create Table                            |                                                                             +-------+-----------------------------------------+
| emp1  | CREATE TABLE `emp1` (                   |
  `id` int(11) DEFAULT NULL,
  `lname` varchar(15) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8              |
+-------+-----------------------------------------+
1 row in set (0.03 sec)






#查看表的比较规则
show table status from dbtest1 like 'emp1';



#修改表的字符集和比较规则
ALTER TABLE emp1  CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
~~~





## 5.4 请求到响应过程中字符集的变化



---

![image-20220705094245485](http://img.fgcy.xyz/image-20220705094245485.png)

---





---

![image-20220705102828512](http://img.fgcy.xyz/image-20220705102828512.png)

---





为了体现出字符集在请求处理过程中的变化，我们这里特意修改一个系统变量的值：

~~~mysql
set character_set_connection = gbk;

Query OK, 0 rows affected (0.00 sec)
~~~





现在假设我们客户端发送的请求是下边这个字符串：

~~~mysql
SELECT * FROM t 
	WHERE s = '我';
~~~



为了方便理解这个过程，我们只分析字符 '我' 在这个过程中字符集的转换



现在看一下在请求从发送到结果返回过程中字符集的变化：



1、 客户端发送请求所使用的字符集 一般情况下客户端所使用的字符集和当前操作系统一致，不同操作系统使用的字符集可能不一 样



如下：

类 Unix 系统使用的是 utf8

 Windows 使用的是 gbk 

当客户端使用的是 utf8 字符集，字符 '我' 在发送给服务器的请求中的字节形式就是： `0xE68891`



提示 :

如果你使用的是可视化工具，比如navicat之类的，这些工具可能会使用自定义的字符集来编 码发送到服务器的字符串

而不采用操作系统默认的字符集（所以在学习的时候还是尽量用 命令行窗口）



2、服务器接收到客户端发送来的请求其实是一串二进制的字节，它会认为这串字节采用的字符集是 character_set_client ，然后把这串字节转换为 character_set_connection 字符集编码的 字符

由于我的计算机上 character_set_client 的值是 utf8 ，首先会按照 utf8 字符集对字节串 0xE68891 进行解码，得到的字符串就是 '我' ，然后按照 character_set_connection 代表的 字符集，也就是 gbk 进行编码，得到的结果就是字节串   ` 0xCED2 `



3、因为表 t 的列 col 采用的是 gbk 字符集，与 character_set_connection 一致，所以直接到列 中找字节值为 0xCED2 的记录，最后找到了一条记录



提示 ：

如果某个列使用的字符集和character_set_connection代表的字符集不一致的话，还需要进行 一次字符集转换





4、上一步骤找到的记录中的 col 列其实是一个字节串 0xCED2 ， col 列是采用 gbk 进行编码的，所 以首先会将这个字节串使用 gbk 进行解码，得到字符串 '我' ，然后再把这个字符串使用 character_set_results 代表的字符集，也就是 utf8 进行编码，得到了新的字节串： 0xE68891 ，然后发送给客户端



5、由于客户端是用的字符集是 utf8 ，所以可以顺利的将 0xE68891 解释成字符 我 ，从而显示到我 们的显示器上，所以我们人类也读懂了返回的结果



总结图示如下：

---

![image-20220705110941243](http://img.fgcy.xyz/image-20220705110941243.png)

---



从这个分析中我们可以得出这么几点需要注意的地方:

- 服务器认为客户端发送过来的请求是用character_set_client编码的。假设你的客户端采用的字符集和character_set_client不一样的话,这就会出现识别不准确的情况

  比如我的客户端使用的是utf8字符集,如果把系统变量character_set_client的值设置为ascii的话,服务器可能无法理解我们发送的请求,更别谈处理这个请求了

  

- 服务器将把得到的结果集使用character_set_results编码后发送给客户端。假设你的客户端采用的字符集和 aharacter_set_results不一样的话,这就可能会出现客户端无法解码结果集的情况,结果就是在你的屏幕上出现乱码

  比如我的客户端使用的是utf8字符集,如果把系统变量character_set_results 的值设置为ascii的话,可能会产生乱码

  

- character_set_connection 只是服务器在将请求的字节串从character_set_client转换为character_set_connection 时使用

  一定要注意,该字符集包含的字符范围一定涵盖请求中的字符,要不然会导致有的字符无法使用character_set_connection代表的字符集进行编码





经验:

开发中通常把`character_set_client`、`character_set_connection`,` character_set_results`这三个系统变量设置成和客户端使用的字符集一致的情况,这样减少了很多无谓的字符集转换。

为了方便我们设置, MySQL提供了一条非常简便的语句:

~~~mysql
SET NAMES 字符集名;
~~~



这一条语句产生的效果和我们执行这3条的效果是一样的:

~~~mysql
SET character_set_client = 字符集名;
SET character_set_connection = 字符集名;
SET character_set_results = 字符集名;
~~~



比方说我的客户端使用的是utf8字符集,所以需要把这几个系统变量的值都设置为utf8:

~~~mysql
SET NAMES utf8;
~~~



另外,如果你想在启动客户端的时候就把character_set_client、character_set_connection、character_set_results这三个系统变量的值设置成一样的

那我们可以在启动客户端的时候指定一个叫default-character-set的启动选项,比如在配置文件里可以这么写:

~~~shell
[client]
default-character-set=utf8
~~~



它起到的效果和执行一遍SET NAMES utf8是一样一样的,都会将那三个系统变量的值设置成utf8





# 6. SQL大小写规范



## 6.1 Windows和Linux平台区别

 在 SQL 中，关键字和函数名是不用区分字母大小写的

比如 SELECT、WHERE、ORDER、GROUP BY 等关 键字，以及 ABS、MOD、ROUND、MAX 等函数名



不过在 SQL 中，你还是要确定大小写的规范，因为在 Linux 和 Windows 环境下，你可能会遇到不同的大 小写问题。

windows系统默认大小写不敏感 

但是 linux系统是大小写敏感的 



 通过如下命令查看：

~~~mysql
SHOW VARIABLES LIKE '%lower_case_table_names%';


-- Linux
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 0     |
+------------------------+-------+
1 row in set (0.03 sec)


-- window
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 1     |
+------------------------+-------+
1 row in set, 1 warning (0.00 sec)
~~~





lower_case_table_names参数值的设置： 

默认为0，大小写 `敏感` 。

设置1，大小写**不敏感**。创建的表，数据库都是以**小写形式存放在磁盘上**，对于sql语句都是转 **换为小写对表和数据库进行查找**。

设置2，创建的表和数据库依据语句上格式存放（怎么创的，怎么存），**凡是查找都是转换为小写进行**。



 两个平台上SQL大小写的区别具体来说：

MySQL在Linux下数据库名、表名、列名、别名大小写**规则**是这样的： 

1、**数据库名**、**表名**、**表的别名**、变量名是**严格区分大小写的**；

 2、`关键字`、 `函数名称` 在 SQL 中**不区分大小写**；

 3、**列名**（或字段名）与**列的别名**（或字段别名）**在所有的情况下均是忽略大小写**的；

MySQL在Windows的环境下全部不区分大小写







## 6.2 Linux下大小写规则设置 

当想设置为大小写不敏感时，要在 my.cnf 这个配置文件 [mysqld] 中加入 lower_case_table_names=1 ，然后重启服务器。



但是要在重启数据库实例之前就需要将原来的数据库和表转换为小写，否则将找不到数据库名

>   SHOW VARIABLES LIKE '%lower_case_table_names%'        此参数适用于MySQL5.7



在MySQL 8下  禁止  在重新启动 MySQL 服务时将 lower_case_table_names 设置成不同于初始化 MySQL 服务时设置的 lower_case_table_names 值。如果非要将MySQL8设置为大小写不敏感，具体步骤为：



1、停止MySQL服务 

2、删除数据目录，即删除 /var/lib/mysql 目录 （删除数据库文件）

3、在MySQL配置文件（ /etc/my.cnf ）中添加 lower_case_table_names=1 

4、启动MySQL服务





## 6.3 SQL编写建议

如果你的变量名命名规范没有统一，就可能产生错误。



这里有一个有关命名规范的建议：

1、**关键字**和**函数名称**全部大写； **（关键字、函数名不区分大小写）**

2、 数据库名、表名、表别名、字段名、字段别名等全部小写； **（字段名、字段别名在任何情况下都不区分大小写）**

3.、SQL 语句必须以分号结尾。



数据库名、表名和字段名在 Linux MySQL 环境下是区分大小写的，因此建议你统一这些字段的命名规 则，比如全部采用小写的方式

虽然关键字和函数名称在 SQL 中不区分大小写，也就是如果小写的话同样可以执行

但是同时将关键词 和函数名称全部大写，以便于区分数据库名、表名、字段名





# 7. sql_mode的合理设置



## 7.1 宽松模式 vs 严格模式

宽松模式： 

如果设置的是宽松模式，那么我们在插入数据的时候，即便是给了一个错误的数据，也可能会被接受， 并且不报错



举例 ：

我在创建一个表时，该表中有一个字段为name，给name设置的字段类型时 char(10) ，如果我 在插入数据的时候，其中name这个字段对应的有一条数据的 长度超过了10 ，例如'1234567890abc'，超 过了设定的字段长度10，那么不会报错，并且取前10个字符存上，也就是说你这个数据被存为 了'1234567890'，而'abc'就没有了。但是，我们给的这条数据是错误的，因为超过了字段长度，但是并没 有报错，并且mysql自行处理并接受了，这就是宽松模式的效果。 



应用场景 ：

通过设置sql mode为宽松模式，来保证大多数sql符合标准的sql语法，这样应用在不同数据 库之间进行 迁移 时，则不需要对业务sql 进行较大的修改



---



 严格模式：

出现上面宽松模式的错误，应该报错才对，所以MySQL5.7版本就将sql_mode默认值改为了严格模式

所 以在 生产等环境 中，我们必须采用的是严格模式，进而 开发、测试环境 的数据库也必须要设置，这样在 开发测试阶段就可以发现问题。并且我们即便是用的MySQL5.6，也应该自行将其改为严格模式。

开发经验 ：

MySQL等数据库总想把关于数据的所有操作都自己包揽下来，包括数据的校验，其实开发 中，我们应该在自己 开发的项目程序级别将这些校验给做了 ，虽然写项目的时候麻烦了一些步骤，但是这 样做之后，我们在进行数据库迁移或者在项目的迁移时，就会方便很多。

 

改为严格模式后可能会存在的问题：

若设置模式中包含了 NO_ZERO_DATE ，那么MySQL数据库不允许插入零日期，插入零日期会抛出错误而 不是警告。

例如，表中含字段TIMESTAMP列（如果未声明为NULL或显示DEFAULT子句）将自动分配 DEFAULT '0000-00-00 00:00:00'（零时间戳），这显然是不满足sql_mode中的NO_ZERO_DATE而报错。





## 7.2 模式查看



**查看当前的sql_mode**

~~~mysql
-- 当前会话范围
select @@session.sql_mode;

-- 全局范围 （重启服务器后可能失效）
select @@global.sql_mode;
 
#或者
show variables like 'sql_mode';
~~~



---

![image-20220705142444026](http://img.fgcy.xyz/image-20220705142444026.png)

---





## 7.3 宽松模式的举例

~~~mysql
-- 切换数据库
USE dbtest1;

Database changed



-- 建表
CREATE TABLE mytb12(
	id INT ,
    name VARCHAR(16),
    age INT,
    dept INT
);

Query OK, 0 rows affected (0.02 sec)




-- 添加数据
INSERT INTO mytb12 VALUES
(1,'ZHANG3',33,101),
(2,'LI4',34,101),
(3,'WANG5',34,102),
(4,'ZHAO6',34,102),
(5,'TIAN7',36,102);

Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0





-- 查看每个部门年龄最大的人（错误写法）
SELECT max(age),dept,name
FROM mytb12
GROUP BY dept;

ERROR 1055 (42000): Expression #3 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'dbtest1.mytb12.name' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by





-- 设置sql_mode 宽松模式
SET SESSION sql_mode='';

Query OK, 0 rows affected, 1 warning (0.00 sec)







-- 查看每个部门年龄最大的人错误写法）
SELECT max(age),dept,name
FROM mytb12
GROUP BY dept;

+----------+------+--------+
| max(age) | dept | name   |
+----------+------+--------+
|       34 |  101 | ZHANG3 |
|       36 |  102 | WANG5  |
+----------+------+--------+
2 rows in set (0.00 sec)







-- 添加类型不正确的数据
INSERT INTO mytb12 (id,name,age)
VALUES(6,'Tom','aaa');

Query OK, 1 row affected, 1 warning (0.00 sec)





-- 查看数据
SELECT * FROM mytb12;

+------+--------+------+------+
| id   | name   | age  | dept |
+------+--------+------+------+
|    1 | ZHANG3 |   33 |  101 |
|    2 | LI4    |   34 |  101 |
|    3 | WANG5  |   34 |  102 |
|    4 | ZHAO6  |   34 |  102 |
|    5 | TIAN7  |   36 |  102 |
|    6 | Tom    |    0 | NULL |   -- int类型的age 插入字符串
+------+--------+------+------+
6 rows in set (0.00 sec)
~~~



## 7.4 模式的设置



1、**临时设置方式：设置当前窗口中设置sql_mode**

~~~mysql
SET GLOBAL sql_mode = 'modes...'; #全局
SET SESSION sql_mode = 'modes...'; #当前会话
~~~





举例：

~~~mysql
#改为严格模式。此方法只在当前会话中生效，关闭当前会话就不生效了。
set SESSION sql_mode='STRICT_TRANS_TABLES';


#改为严格模式。此方法在当前服务中生效，重启MySQL服务后失效。
set GLOBAL sql_mode='STRICT_TRANS_TABLES';
~~~





2、**永久设置方式：在/etc/my.cnf中配置sql_mode**

在my.cnf文件(windows系统是my.ini文件)，新增：

~~~shell
[mysqld]
sql_mode=ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR
_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
~~~

然后 重启MySQL 



当然生产环境上是禁止重启MySQL服务的，所以采用 临时设置方式 + 永久设置方式 来解决线上的问题， 那么即便是有一天真的重启了MySQL服务，也会永久生效了。

