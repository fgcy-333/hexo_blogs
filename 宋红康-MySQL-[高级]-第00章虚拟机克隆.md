---
title: 第零章虚拟机克隆
date: 2022-08-28 20:44:10
tags:
- 尚硅谷-宋红康-MySQL(高级)
categories: 
- 视频学习笔记
index_img: /images/mysql.png
---

> 本文以centos为例





vmware中的完整克隆是基于指定的虚拟机克隆出相同的一份出来，不必再安装

但是我们要保证几个地方不能一样

1、主机名称（hostname）

2、虚拟网卡设备mac地址

3、ip地址

4、UUID

所以我们在克隆后要对这四个地方进行修改





<!--more-->

# 步骤

## 1.首先进行完全克隆，注意（要克隆的虚拟机在克隆前是需要处于关闭状态）



---

![image-20220704132255622](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044147.png)

---



---

![image-20220704132529106](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044012.png)

---



---

![image-20220704132555698](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044150.png)

---



这一步选择完整克隆

注意：链接克隆是指在一些资源上两个虚拟机会共用，这里不做详细介绍，完整克隆是完全独立出来的一个新虚拟机





---

![image-20220704132631922](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044113.png)

---



注意，克隆完之后所有信息与原虚拟机一样，所以下面我们进行一些信息的修改



## 2.开机前修改mac地址

注意：

如果是动态生成IP地址，请在启动前先启动被克隆的虚拟机，以保证原来虚拟机的ip不会变

否则原来虚拟机ip会变，克隆后的虚拟机ip是原来的虚拟机ip







点击编辑虚拟机设置,选择网络适配器，点击右下角高级然后下边就是mac地址，这个mac地址和被克隆的是一样的，我们点击生成，重新生成一个新的：---

---

![image-20220704132939738](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044357.png)

---



---

![image-20220704133057357](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208282044269.png)

---









## 3.开机后修改主机名称(这步不用改也可以)

修改主机名可能不同linux版本不同，修改方法也不同

centos7就是vim /etc/hostname 命令来编辑主机名

需要重启（reboot）







## 4.修改IP地址 

此处需要注意的是：如果虚拟机使用的是动态ip分配，那么不需要更改ip

如果想改为静态ip，请修改：

> vim /etc/sysconfig/network-scripts/ifcfg-ens33 



## 5.修改UUID

同样在修改IP地址的路径中修改即可



~~~shell
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER ONLY=" no
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="34e4484b-b5b0-5b97-lf2c-3a9e7f51e079"     # 修改UUID
DEVICE="ens33"
ONBOOT="yes"
DNS1=192.168.1.2
~~~



>  保存退出    Esc  +  :   + wq



>  重启网络：systemctl restart network

