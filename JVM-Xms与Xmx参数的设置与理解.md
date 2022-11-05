---
title: JVM-Xms与Xmx参数的设置与理解
date: 2022-09-29 22:42:12
tags:
- JVM
categories: 
- 博客学习笔记
---

# 参数理解

`Xms `: JVM**初始分配**的堆内存

`Xmx `: JVM**最大允许**分配的堆内存，按需分配



<!--more-->

# 堆内存分配:

JVM初始分配的堆内存由-Xms指定，默认是物理内存的1/64；

JVM最大分配的堆内存由-Xmx指定，默认是物理内存的1/4。

默认空余堆内存	**小于40%**	时，JVM就会增大堆直到-Xmx的最大限制；

空余堆内存	**大于70%**	时，JVM会减少堆直到-Xms的最小限制。

因此服务器一般设置-Xms、-Xmx相等以避免在每次GC 后调整堆的大小。



>  说明：如果-Xmx 不指定或者指定偏小，应用可能会导致java.lang.OutOfMemory错误，此错误来自JVM，不是Throwable的，无法用try…catch捕捉。



