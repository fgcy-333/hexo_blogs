---
title: IDEA性能优化设置
date: 2022-09-29 22:42:32
tags:
---



IDEA软件在安装的时候就设置了默认的内存使用上限【默认给我分配的使用上限是1GB】，当我运行大量代码时自然而然的就会产生卡顿。

<!--more-->

我们可以通过显示内存使用情况来查看当前项目占用的内存大小。

---

![image-20220927143628474](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209271436541.png)

----

可以看到当前我的程序占用了814MB的内存，而上限是4096。我们可以通过进入IDEA的设置来更改这些配置.





## 具体步骤：（三步）

第一步：

![image-20220927144131492](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209271446424.png)

---





第二步:

我们具体根据个人电脑配置对其进行适当的修改，并保存文件

~~~java
-Xmx4096m
-Xms2048m
-XX:ReservedCodeCacheSize=256m
-XX:+UseG1GC
~~~









第三步

重启IDEA

---

![image-20220927144439947](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209271447824.png)

![image-20220927144506041](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202209271447021.png)

---

