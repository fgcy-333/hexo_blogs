---
title: 深入理解+MySQL+事务
date: 2022-09-29 22:42:42
tags:
- MySQL
- 事务
categories: 
- 博客学习笔记
index_img: /images/mysql.png
---

## 什么是事务？

事务（Transaction）在计算机术语中是指访问并可能更新数据库中各种数据项的一个   **程序执行单元**。

事务通常由高级  **数据库操作语言**  或  **编程语言**（如 SQL，C++ 或 Java）书写的用户程序的执行所引起，并用形如begin transaction和end transaction语句（或函数调用）来界定。事务由事务开始（begin transaction）和事务结束（end transaction）之间执行的全部操作组成。

对于 MySQL 数据库来说，事务是指以执行start transaction命令开始，到执行commit或者rollback命令结束之间的全部 SQL 操作，如果这些 SQL 操作全部执行成功，则执行commit命令提交事务，表示事务执行成功；如果这些 SQL 操作中任一操作执行失败，则执行rollback命令回滚事务，表示事务执行失败，并将数据库回滚到执行start transaction命令之前的状态。

>  特别地，在现阶段的 MySQL 数据库中，仅 InnoDB 和 NDB 两个存储引擎是支持事务的。

<!--more-->

以 MySQL 的 InnoDB 存储引擎为例，其默认是开启`autocommit`配置的，即自动提交事务。在自动提交模式下，如果没有以start transaction显式地开始一个事务，那么每条 SQL 语句都会被当做一个事务执行提交操作。

通过`set autocommit = 0`命令可以关闭自动提交模式，如果关闭了autocommit，则所有的 SQL 语句都在一个事务中，直到执行commit或rollback，该事务结束，并同时开始另外一个新的事务。

在此，需要我们注意的是，autocommit参数是针对连接的，在一个连接中修改了参数，不会对其他连接产生影响。【是会话级别，而不是系统级别】



除此之外，在 MySQL 中，还存在一些特殊的命令，如果在事务中执行了这些命令，则会强制执行commit命令提交事务，如 **DDL 语句**（create table/drop table/alter table）、lock tables语句等。

不过，我们常用的select【DQL语句】、insert、update和delete【DML语句】命令，都不会强制提交事务。









##   事务的四个特性：ACID

通过上面的内容，我们已经知道了什么是事务，但实际上，事务还具有以下四个特性，即：

- 原子性（Atomicity）
- 一致性（Consistency）
- 隔离性（Isolation）
- 持久性（Durability）

按照严格的标准，只有同时满足 ACID 特性才是事务，但是在各大数据库厂商的实现中，**真正满足 ACID 的事务少之又少**。

例如，MySQL 的 NDB 事务不满足持久性和隔离性；

InnoDB 默认的事务隔离级别是  **“可重复读”**，不满足隔离性；

Oracle 默认的事务隔离级别为  **“读已提交”**，不满足隔离性等等

> 因此与其说 ACID 是事务 必须满足的条件，不如说它们是衡量事务的四个维度。



我们刚刚提到的“隔离级别”在后文中会进行详细的讲解，下面我们先详细介绍 ACID 特性及其实现原理。

### 原子性（Atomicity）

定义
原子性，是指一个  **事务**  是一个  **不可分割的工作单位**，其中的操作要么都做，要么都不做；

如果事务中一个 SQL 语句执行失败，则已执行的语句也必须回滚，数据库回退到事务开始前的状态。

实现原理：`undo log`

在说明原子性的实现原理之前，我们先来了解一下 MySQL 的事务日志。

MySQL 的日志有很多种，如二进制日志（bin log）、错误日志、查询日志、慢查询日志等

此外 InnoDB 存储引擎还提供了两种事务日志：redo log（重做日志）和undo log（回滚日志）。

其中，`redo log`用于保证事务  **持久性**；`undo log`则是事务  **原子性**  和  **隔离性**  实现的基础。



实现原子性的关键，是当事务回滚时能够撤销所有已经成功执行的 SQL 语句。

InnoDB 实现回滚，靠的是undo log：当事务对数据库进行修改时，InnoDB 会生成对应的undo log；如果事务执行失败或调用了rollback，导致事务需要回滚，便可以利用undo log中的信息将数据回滚到修改之前的样子。

undo log属于逻辑日志，它记录的是 SQL 执行的相关信息。当发生回滚时，InnoDB 会根据undo log的内容做与之前相反的工作：

对于每个insert，回滚时会执行delete

对于每个delete，回滚时会执行insert；

对于每个update，回滚时会执行一个相反的update，把数据改回去。

以update操作为例：当事务执行update时，其生成的undo log中会包含被修改行的主键（以便知道修改了哪些行）、修改了哪些列、这些列在修改前后的值等信息，回滚时便可以使用这些信息将数据还原到update之前的状态。





### 持久性（Durability）

定义

持久性，是指事务一旦提交，它对数据库的改变就应该是永久性的，接下来的  **其他操作  或  故障  不应该对其有任何影响**。

> 实现原理：redo log



redo log和undo log都属于 InnoDB 的事务日志。下面先聊一下redo log存在的背景。

InnoDB 作为 MySQL 的存储引擎，数据是存放在磁盘中的，但如果每次读写数据都需要磁盘 IO，效率会很低。为此，InnoDB 提供了缓存（Buffer Pool），Buffer Pool中包含了磁盘中部分数据页的映射，作为访问数据库的缓冲：

当从数据库读取数据时，会首先从Buffer Pool中读取，如果Buffer Pool中没有，则从磁盘读取后放入Buffer Pool；

当向数据库写入数据时，会首先写入Buffer Pool，Buffer Pool中修改的数据会定期刷新到磁盘中，这一过程称为 **“刷脏”**。【MySQL8.0中已经不再使用缓冲池】

Buffer Pool的使用大大提高了读写数据的效率，但是也带了新的问题：如果 MySQL 宕机，而此时Buffer Pool中修改的数据还没有刷新到磁盘，就会导致 **数据的丢失**，事务的持久性无法保证。

于是，redo log被引入来解决这个问题：当数据修改时，除了修改Buffer Pool中的数据，还会在redo log记录这次操作；

当事务提交时，会调用fsync接口对redo log进行刷盘。

如果 MySQL 宕机，重启时可以读取redo log中的数据，对数据库进行恢复。



redo log采用的是 WAL（Write-ahead logging，预写式日志），所有修改先写入日志，再更新到Buffer Pool，保证了数据不会因 MySQL 宕机而丢失，从而满足了持久性要求。



既然`redo log`也需要  **在事务提交时**  将日志写入磁盘，为什么它比直接将Buffer Pool中修改的数据写入磁盘（即刷脏）要快呢？

主要有以下两方面的原因：

1. 刷脏是随机 IO，因为每次修改的数据位置随机，但写redo log是追加操作，属于顺序 IO。

2. 刷脏是以数据页（Page）为单位的，MySQL 默认页大小是 16 KB，一个Page上一个小修改都要整页写入；而redo log中只包含真正需要写入的部分，无效 IO 大大减少。

   

我们知道，在 MySQL 中还存在binlog（二进制日志）也可以记录写操作并用于数据的恢复，但二者是有着根本的不同的.

- 作用不同：

  redo log是用于crash recovery的，保证 **MySQL 宕机**也  **不会影响持久性**；

  binlog是用于point-in-time recovery的，保证服务器可以  **基于时间点恢复数据**，此外``binlog`还用于 **主从复制**。

- 层次不同：

  redo log是 InnoDB **存储引擎**实现的，而binlog是 **MySQL 的服务器层**实现的，同时支持 InnoDB 和其他存储引擎。



- 内容不同：

  redo log是物理日志，内容基于磁盘的Page；

  binlog的内容是二进制的，根据binlog_format参数的不同，可能基于 SQL 语句、基于数据本身或者二者的混合。



- 写入时机不同：

  binlog在事务提交时写入；

  redo log的写入时机相对多元：

  - 前面曾提到当事务提交时会调用fsync对redo log进行刷盘，这是默认情况下的策略，修改innodb_flush_log_at_trx_commit参数可以改变该策略，但事务的持久性将无法保证。
  - 除了事务提交时，还有其他刷盘时机，如master thread每秒刷盘一次redo log等，这样的好处是不一定要等到commit时刷盘，commit速度大大加快。





### 隔离性（Isolation）

定义
与原子性、持久性侧重于研究事务本身不同，隔离性研究的是  **不同事务之间的相互影响**  。

隔离性，是指事务内部的操作与其他事务是隔离的，并发执行的各个事务之间不能互相干扰。

严格的隔离性，对应了事务  **隔离级别**  中的 `Serializable`（可串行化），但实际应用中出于性能方面的考虑很少会使用可串行化。

隔离性追求的是  **并发情形**  下  **事务之间互不干扰**  。

简单起见，我们仅考虑最简单的读操作和写操作（暂时不考虑带锁读等特殊操作），那么隔离性的探讨，主要可以分为两个方面：

> （一个事务）写操作对（另一个事务）写操作的影响：锁机制保证隔离性
> （一个事务）写操作对（另一个事务）读操作的影响：MVCC 保证隔离性



锁机制
首先来看两个事务的写操作之间的相互影响。

**隔离性** 要求  **同一时刻**  只能有一个事务对数据进行**写操作**，InnoDB 通过锁机制来保证这一点。



锁机制的基本原理可以概括为：

事务在修改数据之前，需要先获得相应的锁；

获得锁之后，事务便可以修改数据；

该事务操作期间，这部分数据是锁定的，其他事务如果需要修改数据，需要等待当前 **事务提交** 或 **回滚后 ** **释放锁**。



按照粒度，锁可以分为表锁、行锁以及其他位于二者之间的锁。

表锁在操作数据时会锁定整张表，并发性能较差；

行锁则只锁定需要操作的数据，并发性能好。但是由于加锁本身需要消耗资源（获得锁、检查锁、释放锁等都需要消耗资源），因此在锁定数据较多情况下使用表锁可以节省大量资源。

MySQL 中不同的存储引擎支持的锁是不一样的，例如 **MyIsam 只支持表锁**，而 I**nnoDB 同时支持表锁和行锁**，且出于性能考虑，绝大多数情况下使用的都是行锁。


> 如何查看锁信息：有多种方法可以查看 InnoDB 中锁的情况，例如

~~~mysql
select * from information_schema.innodb_locks; # 查询锁的概况
show engine innodb status; # 查询 InnoDB 整体状态，其中包括锁的情况
~~~





举例：

~~~mysql
# 在事务 A 中执行
start transaction;
update account SET balance = 1000 where id = 1;

# 在事务 B 中执行
start transaction;
update account SET balance = 2000 where id = 1;
~~~



查看锁的情况：

---

![image-20220928150102472](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209281501438.png)

---

通过上述命令可以查看事务24052和24053占用锁的情况，其中lock_type为RECORD，代表锁为行锁（记录锁）；lock_mode为X，代表排它锁（写锁）。

除了排它锁（写锁）之外，MySQL 中还有共享锁（读锁）的概念。

介绍完写操作之间的相互影响，下面讨论写操作对读操作的影响。


## 脏读、不可重复读和幻读

首先来看并发情况下，读操作可能存在的三类问题。

- **脏读**：当前事务（`A`）中可以读到其他事务（`B`）未提交的数据（脏数据），这种现象是脏读。

举例如下（以账户余额表为例）：

---

![image-20220928150701285](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209281507120.png)

----



**不可重复读**：在事务`A`中先后两次读取同一个数据，两次读取的结果不一样，这种现象称为不可重复读。

脏读与不可重复读的区别在于：前者读到的是其他事务未提交的数据，后者读到的是其他事务已提交的数据。

举例如下：

---

![image-20220928150756843](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209281508573.png)

---





**幻读**：在事务`A`中按照某个条件先后两次查询数据库，两次查询结果的  **条数不同**，这种现象称为幻读。

**不可重复读**  与  **幻读** 的区别可以通俗的理解为：前者是数据变了，后者是数据的行数变了。

举例如下：

---

![image-20220928163132339](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209281631857.png)



## 事务隔离级别

SQL 标准中定义了四种隔离级别，并规定了每种隔离级别下上述几个问题是否存在。一般来说，隔离级别越低，系统开销越低，可支持的并发越高，但隔离性也越差。

隔离级别与读问题的关系如下：

---

![image-20220928163232913](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209281632984.png)

---

在实际应用中，读未提交在并发时会导致很多问题，而性能相对于其他隔离级别提高  **却很有限**，因此使用较少。

可串行化强制事务串行，并发效率很低，只有当对数据一致性要求极高且可以接受没有并发时使用，因此使用也较少。

因此在大多数数据库系统中，默认的隔离级别是**读已提交**（如 Oracle）或  **可** **重复读**（后文简称RR）。

可以通过如下两个命令分别查看  **全局隔离级别** 和 **本次会话的隔离级别**：

~~~mysql
select @@global.tx_isolation # 查询全局隔离级别
select @@tx_isolation # 查询本次会话隔离级别
~~~

InnoDB 默认的隔离级别是RR，后文会重点介绍RR。

需要注意的是，在 SQL 标准中，RR是无法避免幻读问题的，但是 InnoDB 实现的RR避免了幻读问题。




MVCC
RR解决脏读、不可重复读、幻读等问题，使用的是 MVCC：

MVCC 全称`Multi-Version Concurrency Control`，即多版本的并发控制协议。

下面的例子很好的体现了 MVCC 的特点：在同一时刻，不同的事务读取到的数据可能是不同的（即多版本）—— 在 T5 时刻，事务A和事务C可以读取到不同版本的数据。

----

![image-20220928205556788](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209282055570.png)

----

`MVCC ` 最大的优点是读不加锁，因此读写不冲突，并发性能好。

InnoDB 实现 MVCC，多个版本的数据可以共存，主要是依靠数据的隐藏列（也可以称之为标记位）和 undo log。

其中，数据的隐藏列包括了该行数据的版本号、删除时间、指向 `undo log`的指针等等；

当读取数据时，MySQL 可以通过隐藏列判断是否需要回滚并找到回滚需要的undo log，从而实现 MVCC；隐藏列的详细格式不再展开。

下面结合前文提到的几个问题分别说明。




- **脏读**

----

![image-20220928205842844](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209282059845.png)

----

当事务`A`在 T3 时间节点读取`zhangsan`的余额时，会发现数据已被其他事务修改，且状态为未提交。此时事务`A`读取最新数据后，根据数据的`undo log`执行回滚操作，得到事务`B`修改前的数据，从而避免了脏读。



- **不可重复读**

---

![image-20220928205954500](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209282059680.png)

---

当事务A在 T2 节点第一次读取数据时，会记录该数据的版本号（数据的版本号是以row为单位记录的），假设版本号为1；

当事务B提交时，该行记录的版本号增加，假设版本号为2；当事务A在 T5 再一次读取数据时，发现数据的版本号2大于第一次读取时记录的版本号1，因此会根据undo log执行回滚操作，得到版本号为1时的数据，从而实现了 **可重复读**。






幻读
InnoDB 实现的RR通过next-key lock机制避免了幻读现象。next-key lock是行锁的一种，实现相当于record lock(记录锁) + gap lock(间隙锁)；其特点是不仅会锁住记录本身（record lock的功能），还会锁定一个范围（gap lock的功能）。当然，这里我们讨论的是不加锁读：此时的next-key lock并不是真的加锁，只是为读取的数据增加了标记（标记内容包括数据的版本号等），准确起见姑且称之为类机制`next-key lock`。

还是以前面的例子来说明：

---

![image-20220929091503272](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209290915695.png)

---

当事务A在 T2 节点第一次读取0<id<5数据时，标记的不只是id=1的数据，而是将范围(0, 5)进行了标记，这样当 T5 时刻再次读取0<id<5数据时，便可以发现id=2的数据比之前标记的版本号更高，此时再结合undo log执行回滚操作，避免了幻读。



小结
概括来说，InnoDB 实现的RR，通过锁机制、数据的隐藏列、undo log和类next-key lock，实现了一定程度的隔离性，可以满足大多数场景的需要。不过需要说明的是，RR虽然避免了幻读问题，但是毕竟不是Serializable，不能保证完全的隔离.

下面是一个例子，大家可以自己验证一下:

----

![image-20220929091549364](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209290915670.png)

---

## 一致性（Consistency）

定义
一致性，是指事务执行结束后，数据库的  **完整性约束**  没有被破坏，**事务执行的前后都是合法的数据状态**。

数据库的完整性约束包括但不限于：实体完整性（如行的主键存在且唯一）、列完整性（如字段的类型、大小、长度要符合要求）、外键约束、用户自定义完整性（如转账前后，两个账户余额的和应该不变）。

实现方式
可以说，**一致性**  是事务追求的最终目标：**前面提到的原子性、持久性和隔离性，都是为了保证数据库状态的一致性**。

此外，除了数据库层面的保障，一致性的实现也需要应用层面进行保障。实现一致性的措施包括：

保证原子性、持久性和隔离性，如果这些特性无法保证，事务的一致性也无法保证；
数据库本身提供保障，例如不允许向整形列插入字符串值、字符串长度不能超过列的限制等；
应用层面进行保障，例如如果转账操作只扣除转账者的余额，而没有增加接收者的余额，无论数据库实现的多么完美，也无法保证状态的一致。



## 总结

下面总结一下 ACID 特性及其实现原理：

原子性：保证事务要么全执行，要么全不执行，是事务最核心的特性，事务本身就是以原子性来定义的，实现主要基于` undo log` 日志；
持久性：保证事务提交后不会因为宕机等原因导致数据丢失，实现主要基于  ` redo log` 日志；
隔离性：保证事务执行尽可能不受其他事务影响，InnoDB 默认的隔离级别是RR，RR的实现主要基于锁机制、数据的隐藏列、undo log日志和类next-key lock机制；
一致性：**事务追求的最终目标**，一致性的实现既需要数据库层面的保障，也需要应用层面的保障。





## 进一步了解事务的隔离级别

在上文中，我们已经大致介绍了事务的四种隔离级别，下面我们在来看看这 **四种隔离级别的实现原理**。



### 事务隔离的原理是什么？

我们都知道事务的四种性质，数据库为了维护这些性质，尤其是一致性和隔离性，一般使用加锁这种方式。

同时数据库又是个高并发的应用，同一时间会有大量的并发访问，如果加锁过度，会极大的降低并发处理能力。

所以，**对于加锁的处理**，可以说就是  **数据库对于事务处理的精髓所在**  。



### 一次封锁 or 两段锁？

因为有大量的并发访问，为了预防死锁，一般应用中推荐使用一次封锁法，就是在方法的开始阶段，已经预先知道会用到哪些数据，然后全部锁住，在方法运行之后，再全部解锁。这种方式可以有效的避免循环死锁，但在数据库中却不适用，因为在事务开始阶段，数据库并不知道会用到哪些数据。





**数据库遵循的是两段锁协议**，**将事务分成两个阶段**，分别为：

**加锁阶段：**在该阶段可以进行加锁操作。

在对任何数据进行读操作之前要申请并获得S锁（共享锁，其它事务可以继续加共享锁，但不能加排它锁），在进行写操作之前要申请并获得X锁（排它锁，其它事务不能再获得任何锁）。

加锁不成功，则事务进入等待状态，直到加锁成功才继续执行。



**解锁阶段：**当事务释放了一个封锁以后，事务进入解锁阶段

在该阶段只能进行解锁操作不能再进行加锁操作。
这种方式虽然  **无法避免死锁**，但是  **两段锁协议**  可以  保证事务的  **并发调度是串行化**（串行化很重要，尤其是在数据恢复和备份的时候）的。



事务中的加锁方式

在数据库操作中，为了有效保证并发读取数据的正确性，提出的 **事务隔离级别** ，而数据库锁，就是为了构建这些隔离级别存在的。



**读未提交**
Read Uncommitted，虽然其也是隔离级别中的一种，但因为其可能引发的问题比较多，所以数据库一般都不会使用这种隔离级别，而且其任何操作都不会加锁，这里就不讨论了。



**读已提交**
Read Committed，在这种隔离级别中，数据的 **读取都是不加锁的** ，但是 **数据的写入、修改和删除是需要加锁** 的。



**可重复读**
Repeatable Read，这是 MySQL 中 **InnoDB 存储引擎 ** **默认的隔离级别**。我们姑且分“读”和“写”两个模块来讲解。

读（快照读）
读就是可重复读，其解决了  **脏读**  和  **不可重复读**  的问题，但却可能引发幻读的问题。

讲到这里，我们先来好好地说明下不可重复读和幻读的区别：

很多人容易搞混不可重复读和幻读，两者确实有些相似，但不可重复读的重点在于**update**  和  **delete**操作，而幻读的重点则在于insert操作。

如果使用锁机制来实现这两种隔离级别，在可重复读中，该 SQL 第一次读取到数据后，就将这些数据加锁，其它事务无法修改这些数据，就可以实现可重复读了。

但这种方法却无法锁住insert的数据，所以当事务A先前读取了数据，或者修改了全部数据，事务B还是可以insert数据提交，这时事务A就会发现莫名其妙多了一条之前没有的数据，这就是  **幻读**，**不能通过行锁来避免**。

需要Serializable隔离级别 ，读用读锁，写用写锁，读锁和写锁互斥，这么做可以有效的避免幻读、不可重复读、脏读等问题，但却会极大的降低数据库的并发能力。
所以说，不可重复读和幻读最大的区别，就在于如何 **通过锁机制来解决他们产生的问题**。

MySQL、Oracle、PostgreSQL 等成熟的数据库，出于性能考虑，都是使用了以  **乐观锁为理论基础**  的 **MVCC**（多版本并发控制）来避免这两种问题。



这里继续扩展下悲观锁和乐观锁的知识。

悲观锁：正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。
悲观锁的实现，往往依靠数据库提供的锁机制（也只有  **数据库层**  提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。
在悲观锁的情况下，为了保证事务的隔离性，就需要一致性锁定读。

读取数据时要加锁，其它事务无法修改这些数据；

修改删除数据时也要加锁，其它事务无法读取这些数据。



乐观锁：相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。而乐观锁机制在一定程度上解决了这个问题。
乐观锁大多是基于数据版本（version）记录机制实现。何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个  **version字段**  来实现。
读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号  **大于数据库表  当前版本号**，则予以更新，否则认为是过期数据。
需要说明的是，MVCC 的实现没有固定的规范，每个数据库都会有不同的实现方式，这里讨论的是 InnoDB 的 MVCC。



接下来，讲解 MVCC 在 MySQL 的 InnoDB 中的实现：

在 InnoDB 中，会在每行数据后添加两个额外的隐藏的值来实现 MVCC，这两个值一个记录这行数据何时被创建，另外一个记录这行数据何时过期（或者被删除）。 在实际操作中，存储的并不是时间，而是事务的版本号，每开启一个新事务，事务的版本号就会递增。 



在 **RR事务** 隔离级别下：
insert时，保存当前事务版本号为行的  **创建版本号**  ；
delete时，保存当前事务版本号为行的    **删除版本号**     ；
select时，读取创建版本号<=当前事务版本号，删除版本号为空或>当前事务版本号；
update时，插入一条新纪录，保存当前事务版本号为行创建版本号，同时保存当前事务版本号到原来删除的行。
通过 MVCC，虽然每行记录都需要额外的存储空间、更多的行检查工作以及一些额外的维护工作，但可以减少锁的使用，大多数读操作都不用加锁，读数据操作很简单，性能很好，并且也能保证只会读取到符合标准的行，也只锁住必要行。

事务的隔离级别其实都是对于读数据的定义，但到了这里，就被拆成了读和写两个模块来讲解。这主要是因为 MySQL 中的读，和事务隔离级别中的读，是不一样的。

我们且看，在RR级别中，通过 MVCC 机制，虽然让数据变得可重复读，但我们读到的数据可能是历史数据，是不及时的数据，不是数据库当前的数据。这在一些对于数据的时效特别敏感的业务中，就很可能出问题。

对于这种读取历史数据的方式，我们叫它快照读（snapshot read），而读取数据库当前版本数据的方式，叫当前读（current read）。很显然，在 MVCC 中：

快照读：就是select操作；
当前读：特殊的读操作，insert、update和delete操作，属于当前读，处理的都是当前的数据，需要加锁。
事务的隔离级别实际上都是定义了当前读的级别，MySQL 为了减少锁处理（包括等待其它锁）的时间，提升并发能力，引入了快照读的概念，使得select不用加锁。而update、insert这些“当前读”，就需要另外的模块来解决了。因为更新数据、插入数据是针对当前数据的，所以不能以快照的历史数据为参考，此处就是这个意思。

写（当前读）
事务的隔离级别中虽然只定义了读数据的要求，实际上这也可以说是写数据的要求。上文的“读”，实际是讲的快照读，而这里说的“写”就是当前读了。

为了解决当前读中的幻读问题，MySQL 事务使用了next-key lock锁，我们在上文中已经介绍过了，它是行锁和间隙锁的组合。

行锁可以防止不同事务版本的数据修改提交时造成数据冲突的情况。但如何避免别的事务插入数据就成了问题。行锁防止别的事务修改或删除，间隙锁防止别的事务新增，行锁和间隙锁结合形成的的next-key lock锁就共同解决了RR级别在写数据时的幻读问题。





可串行化

Serializable，这个级别很简单，读加共享锁，写加排他锁，读写互斥。使用悲观锁的理论，实现简单，数据更加安全，但是并发能力非常差。如果你的业务并发的特别少或者没有并发，同时又要求数据及时可靠的话，可以使用这种模式。



在这里需要注意改变一个观念，不要看到select就说不会加锁了，在Serializable这个级别中，select还是会加锁的。

