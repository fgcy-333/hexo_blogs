---
title: javaweb-servlet-jsp-tomcat
date: 2022-08-26 16:56:24
tags:
- 动力节点-老杜-javaWeb
categories: 
- 视频学习笔记
---





# 一、系统架构

## 1.0 C/S**架构**

1.0 Client / Server（客户端 / 服务器）



2.0 C/S架构的特点

 - 需要   **安装**   特定的   **客户端软件**   。



3.0    **C/S架构   的系统**   优点和缺点分别是什么？

- 优点：
  - 速度快（软件中的   **数据大部分**    都是   集成到**客户端软件**当中    的，    **很少量的数据**     ` 从服务器端传送过来`，所以C/S结构的系统速度快）
  - 界面酷炫（专门的语言去实现界面的，不仅是HTML / CSS / JS。）
  - 体验好（速度又快，界面又酷炫，当然体验好了。）
  - 服务器压力小（因为大量的数据都是集成在客户端软件当中，所以     `  服务器`      只需要传送      **很少的数据量**，当然服务器压力小。）
  - 安全（因为大量的数据是集成在客户端软件当中的，并且客户端有很多个，服务器虽然只有一个，就算服务器那边地震了，火灾了，服务器受损了，问题也不大，因为大量的数据在多个    **客户端上有缓  存**   ，有存储，所以从这个方面来说，**C/S结构的系统比较安全 数据安全**）
  - .....
- 缺点：
  -    **升级维护**    比较差劲。（升级维护比较麻烦。成本比较高。每一个客户端软件都需要升级。有一些软件不是那么容易安装的）
  
  <!--more-->

## 2.0 B/S架构

1.0 B/S（Browser / Server，浏览器 / 服务器）



2.0  **B/S结构的系统**     还是一个   **C/S**，只不过这个C比较特殊，这个Client是一个固定不变浏览器软件【`B/S结构的系统是一个特殊的C/S系统`】



3.0 B/S结构的系统优点和缺点是：

- 优点：
  -  ` 升级维护 `  方便，成本比较低。（只需要升级服务器端即可。）
  -  **不需要安装`特定的`  客户端软件**   ，用户操作极其方便。只需要打开浏览器，输入网址即可。
- 缺点：
  - 速度慢（不是因为带宽低的问题，是因为    `所有的数据都是在服务器上 `   ，用户发送的每一个请求都是需要服务器全身心的响应数据，所以B/S结构的系统在网络中   `传送的数据量` 比较大。）
  - 体验差（界面不是那么酷炫，因为浏览器只支持三个语言HTML CSS JavaScript。在加上速度慢。）
  - 不安全（所有的数据都在服务器上，只要服务器发生火灾，地震等不可抗力，最终数据全部丢失。）



4.0 C/S和B/S结构的系统，哪个好，哪个不好？

- 这个问题问的没有水平。并不是哪个好，哪个不好。    **不同结构的系统**     在    **不同的业务场景**     下有    **不同的适用场景**。
- 娱乐性软件建议使用？
  - C/S 结构
- 公司内部使用的一些业务软件建议使用？
  - 公司内部使用的系统，需要维护成本低。
  - 公司内部使用的系统，不需要很酷炫。
  - 公司内部使用的企业级系统主要是能够进行数据的维护即可。
  - B/S 结构。



5.0 开发一个WEB系统你需要会哪些技术？

- WEB前端（运行在浏览器上的程序）
  - HTML
  - CSS
  - JavaScript
- WEB后端（WEB服务器端的程序）
  - Java可以（Java做WEB开发我们称为JavaWeb开发。  JavaWeb开发    `最核心`的**规范**    ：Servlet）
  - C语言也可以
  - C++也可以
  - Python也行
  - PHP也可以

注意：

> Server Applet服务器端的Java小程序







6.0 **开发B/S结构的系统**，其实就是   **开发网站**  ，其实就是开发一个WEB系统



## 3.0 java三大块

- JavaSE
  - J**ava标准版**（**一套类库**：别人写好的一套类库，只不过这个类库是标准类库，学习EE，或者ME，这个SE一定是基础，先学。）
- JavaEE（WEB方向，WEB系统）
  - **Java企业版**（也是一**套类库**：也是别人写好的一套类库，只不过**这套类库可以帮助我们完成企业级项目的开发**，专门为企业内部提供解决方案的一套（多套）类库）
  - 别人写好的，你用就行了，用它可以开发企业级项目。
  - 可以开发web系统。
  - Java比较火爆的就是这个JavaEE方向。
- **JavaME**
  - Java微型版（**还是一套类库**，只不过这套类库帮助我们进行电子微型设备内核程序的开发）
  - 机顶盒内核程序，吸尘器内核程序，电冰箱内核程序，电饭煲内核程序.......



## 4.0 B/S结构的系统通信原理

1.0 在浏览器url输入栏中输入一个域名，敲回车的过程发生了什么？(一个WEB系统的通信原理)

​	通信步骤：

- 第一步：用户输入网址（URL）
- 第二步：先在本机host文件中查找有无该域名对应的ip地址，没有就访问  **域名解析器**    进行域名解析，获取ip地址   ：http://110.242.68.3:80/index.html
- 第三步：浏览器软件在网络中搜索110.242.68.3这一台主机，直到找到这台主机。
- 第四步：定位110.242.68.3这台主机上的服务器软件，因为是80端口，可以很轻松的     **定位到80端口对应的服务器软件**。
- 第五步：80端口对应的服务器软件得知浏览器想要的资源名是：index.html
- 第六步：服务器软件找到index.html文件，并且将index.html文件中的内容直接输出响应到浏览器上。
- 第七步：浏览器接收到来自服务器的代码（HTML CSS JS）
- 第八步：浏览器渲染，执行HTML CSS JS代码，展示效果。



2. 0 关于域名：

- 网站：https://www.baidu.com/ 
- 域名：www.baidu.com 
- 在浏览器地址栏上输入域名，回车之后，域名解析器会将域名解析出来一个具体的       **IP地址     和      端口号**等。
- 解析结果也许是：http://110.242.68.3:80/index.html【index.html是默认的欢迎页】



3.0 IP地址

- 唯一标识网络上的计算机。在   **同一个网络当中   ，IP地址是唯一的**。
- A计算机要想和B计算机通信，首先你需要知道B计算机的IP地址，有了IP地址才能建立连接



4.0 概要图

---

![image-20220501101534005](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27image-20220501101534005.png)

---

注意：

网络通信三要数：ip地址、端口、通信协议



## 5.0 WEB服务器软件

1.0 WEB服务器软件都有哪些呢？

- Tomcat（WEB服务器）
- jetty（WEB服务器）
- JBOSS（应用服务器）
- WebLogic（应用服务器）
- WebSphere（应用服务器）



2.0 应用服务器和WEB服务器的关系？

- **应用服务器**    实现了     **JavaEE的所有规范**    。(JavaEE有**13**个不同的规范)
- **WEB服务器**    只实现    了    `JavaEE`中的**Servlet + JSP两个核心 **  的规范。
- 通过这个讲解说明了：应用服务器是包含WEB服务器的。
- 用过JBOSS服务器的同学应该很清楚，JBOSS中内嵌了一个Tomcat服务器



## 6.0 Tomcat

1.0 关于tomcat

- tomcat还有另外一个名字：catalina（catalina是美国的一个岛屿，风景秀丽，据说作者是在这个风景秀丽的小岛上开发了一个    `轻量级的WEB服务器`   ，体积小，运行速度快，因此tomcat又被称为catalina）
- tomcat的logo是一只 `  公猫`（寓意表示Tomcat服务器是轻巧的;  体积小，运行速度快，**只实现了Servlet+JSP规范**）
- tomcat是   `java语言` 写的。
- tomcat服务器要想运行，必须先有  **jre**（Java的运行时环境）



2.0 tomcat的启动

- bin目录下有一个文件：startup.bat,通过它可以启动Tomcat服务器。

  - xxx.bat文件是个什么文件？`   bat文件  ` 是  ` windows操作系统专用的`  ，bat文件是 ` 批处理文件`，这种文件中   可以    `编写大量的windows的dos命令`   

    然后执行bat文件就相当于**批量的执行dos命令**。

  - startup.sh，这个文件在windows当中无法执行，在Linux环境当中可以使用。**在Linux环境下能够执行的是shell命令**， ` 大量的shell命令 `   编写在shell文件当中，然后**执行这个shell文件可以批量的执行shell命令**。

  - tomcat服务器    提`供    了bat和sh文件 `  ，说明了这个  `tomcat服务器的通用性`。

  

  - **分析startup.bat文件**得出，执行这个命令，**实际上最后是执行：catalina.bat文件**。
  - catalina.bat文件中有这样一行配置：MAINCLASS=**org.apache.catalina.startup.Bootstrap** （    `Bootstrap` `这个类就是main方法所在的类`）
  - tomcat服务器就是Java语言写的，既然是java语言写的，那么**启动Tomcat服务器就是执行main方法**。

- 我们尝试打开dos命令窗口，在dos命令窗口中输入startup.bat来启动tomcat服务器。

- 启动Tomcat服务器只配置path对应的bin目录是不行的。有两个环境变量需要配置：

  - **JAVA_HOME=JDK的根**
  - **CATALINA_HOME=Tomcat服务器的根**



3.0 Tomcat服务器的目录

- bin ： 这个目录是Tomcat服务器的**命令文件存放的目录**，比如：启动Tomcat，关闭Tomcat等。
- conf： 这个目录是Tomcat服务器的**配置文件存放目录**。（server.xml文件中可以配置端口号，默认Tomcat端口是8080）
- lib ：这个目录是Tomcat服务器的**核心程序**目录，因为Tomcat服务器是Java语言编写的，这里的jar包里面都是class文件。
- logs: Tomcat服务器的**日志目录**，Tomcat服务器启动等信息都会在这个目录下生成日志文件。
- temp：Tomcat服务器的临时目录。存储临时文件。
- webapps：这个目录当中就是用来存放大量的**webapp**（web application：**web应用**）
- work：这个目录是用来存放    **JSP文件翻译之后的            java文件**      以及      **编译之后的class文件**



4.0 配置Tomcat服务器需要哪些环境变量？

- JAVA_HOME=JDK的根
- CATALINA_HOME=Tomcat服务器的根
- PATH=%JAVA_HOME%\bin;%CATALINA_HOME%\bin



5.0 启动Tomcat：命令行窗口 + startup



6.0 关闭Tomcat：stop （shutdown.bat文件重命名为stop.bat，为什么？

原因是shutdown命令和windows中的关机命令冲突。所以修改一下（重命名文件名）



## 7.0 实现一个最基本的web应用

- 第一步：找到CATALINA_HOME\webapps目录

  - 因为所有的webapp要放到webapps目录下。（没有为什么，这是Tomcat服务器的要求。如果不放到这里，Tomcat服务器找不到你的应用）

- 第二步：在CATALINA_HOME\webapps目录下新建一个子目录，起名：oa

  - 这个目录名oa就是你这个webapp的名字。

- 第三步：在oa目录下新建资源文件，例如：index.html

  - 编写index.html文件的内容。

- 第四步：启动Tomcat服务器

- 第五步：打开浏览器，在浏览器地址栏上输入这样的URL：

- http://127.0.0.1:8080/oa/index.html

- 思考一个问题：

  - 我们在浏览器上直接输入一个URL，然后回车。这个动作和超链接一样吗？

    既然是一样的，我们完全可以使用超链接。

    ```html
    <!--注意以下的路径，以/开始，带项目名，是一个绝对路径。不需要添加：http://127.0.0.1:8080-->
    <a href="/oa/login.html">user login2</a>
    
    <!--多个层级也没有关系，正常访问即可。-->
    <!--注意：我们目前前端上的路径都以“/”开始的，都是加项目名的-->
    <a href="/oa/test/debug/d.html">d page</a>
    ```


- http://127.0.0.1:8080/oa/userList.html 
  - 访问这个地址，可以展示一个用户列表页面。但是这个用户列表页面是写死在HTML文件当中的。这种资源我们称为静态资源。怎么能变成动态资源。显然需要连接数据库。
  - 连接数据库需要JDBC程序，也就是说需要编写Java程序连接数据库，数据库中有多少条记录，页面上就显示多少条记录，这种技术被称为动态网页技术。（动态网页技术并不是说页面中有flash动画。动态网页技术是说     页面中的    **数据是动态**的     ，**根据数据库中数据的变化而变化**。）





## 8.0 角色参与

**对于一个动态的web应用来说，一个请求和响应的过程有多少个角色参与，角色和角色之间有多少个协议**

---

![BS结构系统的通信原理2](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209052153350.png)

---



1.0 有哪些角色（在整个BS结构的系统当中，有哪些人参与进去了）

- 浏览器软件的开发团队（浏览器软件太多了：谷歌浏览器 、火狐浏览器、IE浏览器....）
- WEB Server的开发团队（WEB Server这个软件也是太多了：Tomcat、Jetty、WebLogic、JBOSS、WebSphere....）
- DB Server的开发团队（DB Server这个软件也是太多了：Oracle、MySQL.....）
- webapp的开发团队（WEB应用是我们作为JavaWEB程序员开发的）



---

![BS结构系统的角色和协议](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202209052153844.png)

----



2.0 角色和角色之间需要遵守哪些规范，哪些协议？

- `webapp的开发团队`   和    `WEB Server的开发团队`  之间有一套规范: **WEB Server的开发团队**   **Servlet规范**。
  - Servlet规范的作用是什么？
    - WEB Server   和   webapp解耦合。

- Browser  和   WebServer之间有一套传输协议：HTTP协议。（超文本传输协议）

- webapp开发团队  和  DB Server的开发团队之间有一套规范：**JDBC规范**。





# 二、Servlet

## 1.0 模拟Servlet

- 充当Tomcat服务器开发者

~~~java
package org.apache;

import java.util.Scanner;
import java.util.Properties;
import java.io.FileReader;
import javax.servlet.Servlet;

// 充当Tomcat服务器的开发者
public class Tomcat{
	public static void main(String[] args) throws Exception{
		System.out.println("Tomcat服务器启动成功，开始接收用户的访问。");

		// 简单的使用Scanner来模拟一下用户的请求
		// 用户访问服务器是通过浏览器上的“请求路径”
		// 也就是说用户请求路径不同，后台执行的Servlet不同。
		/*
			/userList    UserListServlet
			/login		 UserLoginServlet
			/bank		 BankServlet
			......
		*/
		System.out.print("请输入您的访问路径：");
		Scanner s = new Scanner(System.in);

		// 用户的请求路径  /bbbb
		String key = s.nextLine(); // Tomcat服务器已经获取到了用户的请求路径了。

		// Tomcat服务器应该通过用户的请求路径找对应的XXXServlet
		// 请求路径和XXXServlet之间的关系应该由谁指定呢？
		// 对于Tomcat服务器来说需要解析配置文件
		FileReader reader = new FileReader("web.properties");
		Properties pro = new Properties();
		pro.load(reader);
		reader.close();

		// 通过key获取value
		String className = pro.getProperty(key);
		// 通过反射机制创建对象
		Class clazz = Class.forName(className);
		Object obj = clazz.newInstance(); // obj的类型对于Tomcat服务器开发人员来说不知道。
		
		// 但是Tomcat服务器的开发者知道，你写的XXXXServlet一定实现了Servlet接口
		Servlet servlet = (Servlet)obj;
		servlet.service();

	}
}
~~~



- 充当webapp开发者
  - 对于我们javaweb程序员来说,我们只需要做两件事:
  - 编写一个类实现Servlet接口。
  - 将编写的类配置到配置文件中,在配置文件中:     指定 **请求路径   和 类名的关系**。

~~~java
package com.bjpowernode.servlet;

import javax.servlet.Servlet;

// 只要是我们webapp开发者写的XXXServlet都要实现Servlet接口
public class BankServlet implements Servlet{
	public void service(){
		System.out.println("BankServlet's service...");
	}
}
~~~



- web.xml配置文件

~~~properties
/aaaa=com.bjpowernode.servlet.UserListServlet
/bbbb=com.bjpowernode.servlet.UserLoginServlet
/cccc=com.bjpowernode.servlet.BankServlet
~~~



- 制定servlet规范

~~~java
package javax.servlet;

/*
	我们现在充当的角色是SUN公司。
	SUN公司把Servlet接口/规范制定出来了。
*/
public interface Servlet{

	// 一个专门提供服务的方法
	void service();
}
~~~



## 2.0 Servlet规范

- **遵循Servlet规范的webapp**，这个webapp就**可以      放在     不同的WEB服务器     中运行**。（因为这个webapp是遵循Servlet规范的）
- Servlet规范包括什么呢？
  - 规范了    哪些**接口**
  - 规范了   哪些**类**
  - 规范了一个web应用中应该    有哪些**配置文件**
  - 规范了一个web应用中   **配置文件的名字**
  - 规范了一个web应中   **配置文件存放的路径**
  - 规范了一个web应用中   **配置文件的内容**
  - 规范了一个   合法有效的     **web应用它的目录结构**    应该是怎样的。
  - .....



## 3.0 开发带有Servlet(Java小程序)的webapp

开发步骤是怎样的？

- 第一步：在**webapps目录下新建一个目录**，起名crm（这个crm就是webapp的名字）。当然，也可以是其它项目，比如银行项目，可以创建一个目录bank，办公系统可以创建一个oa。

  -  注意：crm就是这个webapp的根

- 第二步：在**webapp的根下新建一个目录：WEB-INF**

  - 注意：这个目录的名字是    ` Servlet规范中 `   规定的，必须全部大写，必须**一模一样**。必须的必须。

- 第三步：在**WEB-INF目录下新建一个目录：classes**

  - 注意：这个目录的名字必须是全部小写的classes。这也是    **Servlet规范中   规定的**。另外这个目录下一定存放的是J**ava程序编译之后的class文件**（这里存放的是字节码文件）。

- 第四步：在**WEB-INF目录下新建一个目录：lib**

  - 注意：这个目录不是必须的。但如果一个webapp需要第三方的jar包的话，这个jar包要放到这个lib目录下，这个目录的名字也不能随意编写，必须是全部小写的lib。例如java语言连接数据库需要数据库的驱动jar包。那么这个jar包就一定要放到lib目录下。这Servlet规范中规定的。

- 第五步：在**WEB-INF目录下新建一个文件：web.xml**

  - 注意：

    这个文件是必须的，这个文件名必须叫做web.xml。这个文件必须放在这里。

    一个合法的webapp，web.xml文件是必须的，这个web.xml文件就是一个配置文件，在这个配置文件中描述了请求路径和Servlet类之间的对照关系。

  - 这个文件最好从其他的webapp中拷贝，最好别手写。没必要。复制粘贴

  

  - web.xml

  ~~~xml
  <?xml version="1.0" encoding="UTF-8"?>
  
  <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                        https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
    version="5.0"
    metadata-complete="true">
  
  
  </web-app>
  ~~~

  

- 第六步：编写一个Java程序，这个小Java程序也不能随意开发，这个小java程序必须**实现Servlet接口**。

  - **这个Servlet接口不在JDK当中。（因为Servlet不是JavaSE了。Servlet属于JavaEE，是另外的一套类库**。）

  - Servlet接口（Servlet.class文件）是Oracle提供的。（最原始的是sun公司提供的）09年oracle收购了sun公司。

  - **Servlet接口是JavaEE的规范中的一员**。

  - `   Tomcat服务器`   **实现了**  ` Servlet规范` ，所以   **Tomcat服务器   也   需要使用   Servlet接口**   。

    Tomcat服务器中应该有这个接口，**Tomcat服务器的CATALINA_HOME\lib目录下有一个servlet-api.jar**

    **解压这个servlet-api.jar之后，你会看到里面有一个Servlet.class文件**。

    

  - 重点：

    从**JakartaEE9开始，Servlet接口的全名变了：jakarta.servlet.Servlet**

  

  - 注意：

    编写这个Java小程序的时候，java源代码你愿意在哪里就在哪里，位置无所谓，你只需要将   j**ava源代码编译之后的class文件   放到   classes目录下即可**。

  

- 第七步：编译我们编写的HelloServlet

  - 重点：你怎么能让你的HelloServlet编译通过呢？**配置环境变量CLASSPATH**

    CLASSPATH=.;C:\dev\apache-tomcat-10.0.12\lib\servlet-api.jar     **【当前还没有使用集成开发工具，所以需要配置加载jar包的类路径】**

    **这里其实配置了两个路径，一个是从当前路劲，一个是从配置有servlet接口的jar包的绝对路径**

    **加载servlet-api的jar包，以达到编译通过的目的** 

    

  - 思考问题：以上配置的CLASSPATH和Tomcat服务器运行有没有关系？

    - 没有任何关系，以上配置这个环境变量只是为了让你的HelloServlet能够正常编译生成class文件
    - tomcat中的lib目录下本来就有servlet接口相关的jar包

  

- 第八步：将以上编译之后的HelloServlet.class文件拷贝到WEB-INF\classes目录下。

- 第九步：在web.xml文件中编写配置信息，让“请求路径”和“Servlet类名”关联在一起。

  

  - 这一步用专业术语描述：在web.xml文件中注册Servlet类。

  - ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    
    <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                          https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
      version="5.0"
      metadata-complete="true">
    
    	<!--servlet描述信息-->
    	<!--任何一个servlet都对应一个servlet-mapping -->
    	<servlet>
    		<servlet-name>fdsafdsagfdsafdsa</servlet-name>
    		<!--这个位置必须是带有包名的全限定类名-->
    		<servlet-class>com.bjpowernode.servlet.HelloServlet</servlet-class>
    	</servlet>
    
    	<!--servlet映射信息-->
    	<servlet-mapping>
    		<!--这个也是随便的，不过这里写的内容要和上面的一样。-->
    		<servlet-name>fdsafdsagfdsafdsa</servlet-name>
    		<!--这里需要一个路径-->
    		<!--这个路径唯一的要求是必须以 / 开始-->
    		<!--当前这个路径可以随便写-->
    		<url-pattern>/fdsa/fd/saf/d/sa/fd/sa/fd</url-pattern>
    	</servlet-mapping>
    	
    </web-app>
    
    ```

    

- 第十步：启动Tomcat服务器

- 第十一步：打开浏览器，在浏览器地址栏上输入一个url，这个URL必须是：

  - http://127.0.0.1:8080/crm/fdsa/fd/saf/d/sa/fd/sa/fd   
  - 非常重要的一件事：浏览器上的请求路径不能随便写，这个请求路径必须和web.xml文件中的`url-pattern`一致。
  - 注意：浏览器上的请求路径和web.xml文件中的url-pattern的唯一区别就是：**浏览器上的请求路径带项目名：/crm**【虚拟路径】



注意：

- 浏览器上编写的路径太复杂，可以使用超链接。
- **非常重要：html页面只能放到WEB-INF目录外面。**
- 以后不需要我们编写main方法了。**tomcat服务器负责调用main方法**，**Tomcat服务器启动的时候执行的就是main方法**。我们javaweb程序员只需要编写Servlet接口的实现类，然后将其注册到web.xml文件中，即可



一个**合法的webapp目录结构**：

~~~
webapproot
     |------WEB-INF
     		  |------classes(存放字节码)
     		  |------lib(第三方jar包)
     		  |------web.xml(注册Servlet)
     |------html
     |------css
     |------javascript
     |------image
     ....
~~~



## 4.0 解决Tomcat服务器在DOS命令窗口中的乱码问题（控制台乱码）

将`CATALINA_HOME/conf/logging.properties`文件中的内容修改如下：

`java.util.logging.ConsoleHandler.encoding` = GBK



## 5.0 向浏览器响应一段HTML代码

~~~java
public void service(ServletRequest request, ServletResponse response){
    //设置响应的内容类型是text/html
    //要在getWriter之前设置 
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    out.print("<h1>hello servlet!</h1>");
}
~~~



## 6.0 在Servlet中连接数据库，怎么做？

- Servlet是Java程序，所以在Servlet中完全可以编写JDBC代码连接数据库。

- 在一个webapp中去连接数据库，需要将驱动jar包放到WEB-INF/lib目录下。

  **com.mysql.cj.jdbc.Driver** 这个类就在驱动jar包当中。8.0版本

~~~java
	public void service(ServletRequest request,ServletResponse response)
		throws ServletException , IOException{

		response.setContentType("text/html");
		PrintWriter out = response.getWriter();

		// 编写JDBC代码，连接数据库，查询所有学生信息。
		Connection conn = null;
		PreparedStatement ps = null;
		ResultSet rs = null;
		try{
			// 注册驱动 （com.mysql.jdbc.Driver，这个已过时）
			// 新版本中建议使用：com.mysql.cj.jdbc.Driver驱动。
			Class.forName("com.mysql.cj.jdbc.Driver");
			// 获取连接
			String url = "jdbc:mysql://localhost:3306/bjpowernode";
			String user = "root";
			String password = "root";
			conn = DriverManager.getConnection(url,user,password);
			// 获取预编译的数据库操作对象
			String sql = "select no,name from t_student";
			ps = conn.prepareStatement(sql);
			// 执行SQL
			rs = ps.executeQuery();
			// 处理查询结果集
			while(rs.next()){
				String no = rs.getString("no");
				String name = rs.getString("name");
				//System.out.println(no + "," + name);
				out.print(no + "," + name + "<br>");
			}
		}catch(Exception e){
			e.printStackTrace();
		}finally{
			// 释放资源
			if(rs != null){
				try{
					rs.close();
				}catch(Exception e){
					e.printStackTrace();
				}
			}
			if(ps != null){
				try{
					ps.close();
				}catch(Exception e){
					e.printStackTrace();
				}
			}
			if(conn != null){
				try{
					conn.close();
				}catch(Exception e){
					e.printStackTrace();
				}
			}
		}
	}
~~~



## 7.0 在集成开发环境当中开发Servlet程序

1.0 集成工具介绍

​       集成开发工具很多，其中目前使用比较多的是：

​			IntelliJ IDEA（这个居多，IDEA在提示功能方面要强于Eclipse，也就是说IDEA使用起来比Eclipse更加智能，更好用。JetBrain公司开发的。收费的）

​			Eclipse（这个少一些），Eclipse目前还是有团队使用，只不过处于减少的趋势，自己从事工作之后，可能会遇到。Eclipse是IBM团队开发的。Eclipse寓意			是“日食”。“**日食**”表示将太阳吃掉。太阳是SUN。IBM团队开发Eclipse的寓意是吞并SUN公司，但是**2009年的时候SUN公司被Oracle公司并购**了。IBM并			没有成功并购SUN公司



2.0 使用IDEA集成开发工具开发Servlet

- 第一步：New Project（一**般情况下新建的Project的名字最好和目录的名字一致**。）【建议：先创建一个Empty Project【空工程】，然后在空工程下新建Module【模块】】

- 第二步：新建模块（File --> new --> Module...）

  - 这里**新建的是一个普通的JavaSE模块**（**这里先不要新建Java Enterprise模块**）
  - 注意project与module的层级关系。
  - 这个Module起名：servlet01

- 第三步：让Module变成JavaEE的模块。（让**Module变成webapp的模块**。符合webapp规范。符合Servlet规范的Module）

  - 在Module上点击右键：Add Framework Support...（添加框架支持）
  - 在弹出的窗口中，选择**Web Application**（选择的是webapp的支持）
  - 选择了这个webapp的支持之后，IDEA会自动给你生成一个符合Servlet规范的webpp目录结构。
  - **重点，需要注意的：在IDEA工具中根据Web Application模板生成的目录中有一个web目录，这个目录就代表webapp的根**

- 第四步（非必须）：根据Web Application生成的资源中有index.jsp文件，默认访问。

- 第五步：编写Servlet（StudentServlet）

  - class StudentServlet **implements Servlet**

  

  - 这个时候发现Servlet.class文件没有。怎么办？

  将   `CATALINA_HOME/lib/servlet-api.jar`  和  `jsp-api.jar`  添加到`classpath`当中（这里的classpath说的是IDEA的classpath 也就是所依赖的外部库）

     Tomcat本身    也依赖    **jre环境**    下  的     **rt.jar包下的核心类库**，所以核心类库不需要添加到WEB-INF下的lib目录下；

  同理Tomcat中的lib目录中也存在servlet-api.jar包；所以WEB-INF下的lib中不用添加；

  只需要在classPath中配置即可；

  ---

  ![image-20220501162820309](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261719546.png)

  ---

  

​	实现jakarta.servlet.Servlet接口中的5个方法。



- 第六步：在Servlet当中的service方法中编写业务代码（我们这里连接数据库了）

- 第七步：在WEB-INF目录下新建了一个子目录：lib（这个目录名可   **不能随意**，必须是全部小写的lib），并且将连接数据库的驱动jar包放到lib目录下。

- 第八步：在web.xml文件中完成StudentServlet类的注册。（请求路径和Servlet之间对应起来）

- 第九步：给一个html页面，在HTML页面中编写一个超链接，用户点击这个超链接，发送请求，Tomcat执行后台的StudentServlet。
  - **student.html**
  - 这个文件   **不能放到WEB-INF目录里面**   ，**只能放到WEB-INF目录外面**

- 第十步：让   **IDEA工具**   去   **关联Tomcat服务器**。关联的过程当中    **将webapp**   **部署到**   **Tomcat服务器**当中。

  - IDEA工具右上角，绿色小锤子右边有一个：Add Configuration
  - 左上角加号，点击Tomcat Server --> local
  - 在弹出的界面中设置服务器Server的参数（基本上不用动）
  - 在当前窗口中有一个Deployment（点击这个用来部署webapp），继续点击加号，部署即可。
  - 修改 Application context为：/xmm【**虚拟目录/虚拟路径**】

- 第十一步：启动Tomcat服务器

  - 在右上角有绿色的箭头，或者绿色的小虫子，点击这个绿色的小虫子，可以采用debug的模式启动Tomcat服务器。
  - 我们**开发中建议适用debug模式启动Tomcat**

- 第十二步：打开浏览器，在浏览器地址栏上输入：http://localhost:8080/xmm/student.html



## 8.0 Servlet对象的生命周期

1.0 什么是Servlet对象生命周期？

- Servlet对象的生命周期表示：一个    **Servlet对象     从出生在最后的死亡   ，整个过程是怎样的**。



2.0 Servlet对象是由谁来维护的？

- **Servlet对象的创建**，**对象上方法的调用**，**对象最终的销毁**，Javaweb程序员是无权干预的。
- Servlet对象的生命周期是由WEB Server【这里是**Tomcat服务器**】**全权负责**的。
- **Tomcat服务器**    通常我们又称为：**WEB容器**。（这个叫法你要知道【WEB Container】）
- **WEB容器 **   来管理     **Servlet对象的死活(生命周期)**



3.0 思考：我们自己new的Servlet对象受WEB容器的管理吗？

- 我们    **自己new的Servlet对象**    是   **不受**   **WEB容器管理**   的。
- **WEB容器创建的Servlet对象**，这些Servlet对象 `   都   会`   被放到    **一个集合当中（HashMap）**，只有   **放到这个HashMap集合  中的Servlet**   才能够被**WEB容器管理**，自己new的Servlet对象不会被WEB容器管理。（自己new的Servlet对象不在容器当中）



web容器底层应该有一个   **HashMap这样的集合**，在这个集合当中存储了   **Servlet对象**   和   **请求路径之间**     的关系

---

![WEB容器中的Map集合](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261719296.png)

---

4.0 研究：服务器在启动的时候，Servlet对象有没有被创建出来（默认情况下）？

验证方法：

在Servlet中提供一个无参数的构造方法，启动服务器的时候看看构造方法是否执行。



经过测试得出结论：**默认情况下**，`服务器在启动的时候`      ` Servlet对象并不会被实例化`。



这个设计是合理的:

 用户没有发送请求之前，如果提前创建出来所有的Servlet对象，必然是耗费内存的

 并且创建出来的Servlet如果一直没有用户访问，显然这个Servlet对象是一个废物，没必要先创建。



5.0 怎么让服务器启动的时候创建Servlet对象呢？

- 在**servlet标签     中**      添加 `<load-on-startup>`子标签，在该子标签中填写    整数    ，**越小的整数优先级越高**。
- 0到正整数越小,启动服务器时实例servlet的优先级越高；**负数**    代表服务器启动时，不会实例servlet，访问的时候才创建(默认情况)

~~~xml
<servlet>
    <servlet-name>bservlet</servlet-name>
    <servlet-class>com.bjpowernode.javaweb.servlet.BServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>bservlet</servlet-name>
    <url-pattern>/b</url-pattern>
</servlet-mapping>
~~~



6.0 Servlet对象生命周期

- 默认情况下服务器启动的时候AServlet对象并没有被实例化

- 用户发送第一次请求的时候，控制台输出了以下内容：

  ```
  AServlet无参数构造方法执行了
  AServlet's init method execute!
  AServlet's service method execute!
  ```

- 根据以上输出内容得出结论：

  - 用户在发送第一次请求的时候Servlet对象被实例化（AServlet的构造方法被执行了。并且执行的是无参数构造方法）
  - AServlet对象被创建出来之后，Tomcat服务器马上调用了AServlet对象的init方法。（init方法在执行的时候，AServlet对象已经存在了。已经被创建出来了）
  - 用户发送第一次请求的时候，init方法执行之后，Tomcat服务器马上调用AServlet对象的service方法。

  

- 用户继续发送第二次请求，控制台输出了以下内容：

  ```
  AServlet's service method execute!
  ```

  根据以上输出结果得知，用户在发送第二次，或者第三次，或者第四次请求的时候，Servlet对象并没有新建，还是使用之前创建好的Servlet对象，直接调用该Servlet对象的service方法

  

  这说明：

  - 第一：Servlet对象是`单例的`（单实例的。但是要注意：Servlet对象是单实例的，但是Servlet类并   **不符合单例模式**我们称之为**假单例**

    之所以单例是因为Servlet对象的创建我们javaweb程序员管不着，这个对象的创建只能是Tomcat来说了算，Tomcat只创建了一个，所以导致了单例，但是属于假单例。真单例模式，构造方法是私有化的）

  - 第二：`无参数构造方法 `  、` init方法 `  只在    `第一次用户   发送请求的时候执行`。也就是说**无参数构造方法只执行一次**。**init方法也只被Tomcat服务器调用一次**。

  - 第三：只要用户发送一次请求：service方法必然会被Tomcat服务器调用一次。发送100次请求，service方法会被调用100次。

  

- 关闭服务器的时候，控制台输出了以下内容：

  ```
  AServlet's destroy method execute!
  ```

  

- 综上，可得出：

  - 构造方法：只执行一次。
  - init方法：只执行一次。
  - service方法：用户发送一次请求则执 行一次，发送N次请求则执行N次。
  - destroy方法：只被Tomcat服务器调用一次。

  

  7.0 destroy方法是在什么时候被调用的？

  - 在   **服务器关闭的时候**。
  - 因为服务器关闭的时候要销毁AServlet对象的内存。
  - **服务器在销毁AServlet对象内存 **  之前    ，**Tomcat服务器会自动调用 **    `AServlet对象    的destroy方法`。



8.0 当我们Servlet类中编写一个有参数的构造方法，如果没有手动编写无参数构造方法会出现什么问题？

- 报错了：500错误。
- 注意：500是一个    **HTTP协议**   的    **错误状态码**。
- **500一般情况下是因为服务器端的Java程序出现了异常**。（服务器端的错误都是500错误：服务器内部错误）
- 如果没有无参数的构造方法，会导致出现500错误，无法实例化Servlet对象。
- 所以，一定要注意：在Servlet开发当中，不建议程序员来定义构造方法，因为定义不当，一不小心就会导致无法实例化Servlet对象。



9.0 思考：Servlet的无参数构造方法是在对象第一次创建的时候执行，并且只执行一次。init方法也是在对象第一次创建的时候执行，并且只执行一次。那么这个无参数构造方法可以代替掉init方法吗？

- 不能。

- Servlet规范中有要求，作为javaweb程序员，编写Servlet类的时候

  **不建议手动编写构造方法，因为     编写构造方法    ，很容易让     无参数构造方法     消失**，这个操作可能会导致Servlet对象无法实例化

  所以  **init方法   是有存在的必要的**。



10. init、service、destroy方法中使用最多的是哪个方法？
    - 使用最多就是service方法，**service方法是    一定    要实现的**，因为service方法是处理用户请求的核心方法。
    - 什么时候使用init方法呢？
      - init方法很少用。
      - 通常在init方法当中做初始化操作，并且这个初始化操作只需要执行一次。例如：初始化数据库连接池，初始化线程池....
    - 什么时候使用destroy方法呢？
      - destroy方法也很少用。
      - 通常在destroy方法当中，进行资源的关闭。马上对象要被销毁了，还有什么没有关闭的，抓紧时间关闭资源。还有什么资源没保存的，抓紧时间保存一下。





## 9.0 GenericServlet

1.0 我们编写一个Servlet类直接实现Servlet接口有什么   **缺点**   ？

- 我们**只需要service方法，其他方法大部分情况下是不需要使用**的。代码很丑陋。

- **适配器设计模式    **Adapter

- 编写一个GenericServlet类，这个类是一个抽象类，其中有一个抽象方法service。

  - GenericServlet实现Servlet接口。
  - **GenericServlet是一个适配器**。

  **GenericServlet抽象类    是Servlet规范中的一员**

  - 以后编写的所有Servlet类继承GenericServlet，重写service方法即可。



2.0 思考第一个问题：我提供了一个GenericServlet之后，init方法还会执行吗？

- 还会执行

  会执行GenericServlet类中的init方法。因为抽象类GenericServlet实现了Servlet的init方法，根据多态，调用的就是GenericServlet重写的init方法



3.0 思考第二个问题：**init方法**是谁调用的？

- **Tomcat服务器调用**的。



4.0 思考第三个问题：`init方法`  中的  ` ServletConfig对象`是谁创建的？是谁传过来的？

- 都是Tomcat干的。
- **Tomcat服务器**先创建了 `  ServletConfig对象`，然后调用init方法，**将ServletConfig对象传给了init方法**。

~~~java
ServletConfig servletConfig = new org.apache.catalina.core.StandardWrapperFacade();
~~~

**注意：ServletConfig是Servlet的规范之一，该接口由Tomcat实现**



5.0 tomcat调用servlet  伪代码

~~~java
public class Tomcat {
    public static void main(String[] args){

        // 创建LoginServlet对象（通过反射机制，调用无参数构造方法来实例化LoginServlet对象）
        // 应该是从web.xml获取servlet的注册信息
        Class clazz = Class.forName("com.bjpowernode.javaweb.servlet.LoginServlet");
        Object obj = clazz.newInstance();
        
        // 向下转型(为了使用servlet独有的方法)
        Servlet servlet = (Servlet)obj;
        
        // 创建ServletConfig对象
        // Tomcat服务器负责将ServletConfig对象实例化出来。
        // 多态（Tomcat服务器   完全实现了Servlet规范  ）
        ServletConfig servletConfig = new org.apache.catalina.core.StandardWrapperFacade();
        
        // 调用Servlet的init方法
        servlet.init(servletConfig);
        
        //tomcat创建ServletRequest、ServletResponse对象作为service方法的入参
        // 调用Servlet的service方法
        servlet.service(request,response)
        
    }
}  
~~~



6.0 GenericServlet中    **模板方法**   设计体现**【不是源码，只是模拟】**

~~~java
package com.bjpowernode.javaweb.servlet;

import jakarta.servlet.*;

import java.io.IOException;

/**
 * 编写一个标准通用（适配器模式）的Servlet，起名：GenericServlet
 * 以后所有的Servlet类都不要直接实现Servlet接口了。
 * 以后所有的Servlet类都要继承GenericServlet类。
 * GenericServlet 就是一个适配器。
 */
public abstract class GenericServlet implements Servlet {

    // 成员变量
    private ServletConfig config;

    /**
     * init方法中的ServletConfig对象是小猫咪创建好的。
     * 这个ServletConfig对象目前在init方法的参数上，属于局部变量。
     * 那么ServletConfig对象肯定以后要在service方法中使用，怎么才能保证ServletConfig对象在service方法中能够使用呢？
     * 提升 变量作用范围（局部变量------》成员变量）
     */
    @Override
    public final void init(ServletConfig config) throws ServletException {
        //System.out.println("servletConfig对象，小猫咪创建好的：" + config);
        this.config = config;
        // 调用本类的init()方法，重载
        this.init();
    }

    /**
     * 这个init方法是供子类重写的。【体现模板方法】
     */
    public void init(){

    }

    @Override
    public ServletConfig getServletConfig() {
        return config;
    }

    /**
     * 抽象方法，这个方法最常用。所以要求子类必须实现service方法。
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse)
            throws ServletException, IOException;

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
~~~

## 10.  ServletConfig

1.0  ServletConfig是什么？

- javax.servlet.ServletConfig (jakarta.servlet.ServletConfig)

 *      显然`ServletConfig`是**Servlet规范中的一员**。
 *      **ServletConfig是一个接口**。（jakarta.servlet.Servlet是一个接口）



2.0  谁去实现了这个接口呢？

-  **WEB服务器实现**



结论：Tomcat服务器实现了ServletConfig接口。

 *      思考：如果把Tomcat服务器换成jetty服务器，输出ServletConfig对象的时候，还是这个结果吗？
 *      不一定一样，包名类名可能和Tomcat不一样。但是他们都实现了ServletConfig这个规范。



3.0  一个   **Servlet对象**   中    有一个    **ServletConfig对象**【Servlet和ServletConfig对象是一对一】

- 100个Servlet，就应该有100个ServletConfig对象。



4.0 ServletConfig对象是谁创建的？在什么时候创建的？

- **WEB服务器**【Tomcat服务器】创建了ServletConfig对象。

- **在创建Servlet对象的时候，同时创建ServletConfig对象**



5.0 ServletConfig接口到底是干啥的？有什么用呢？

- ServletConfig对象被翻译为：**Servlet对象的配置信息对象**。
- 一个Servlet对象就有一个配置信息对象。
- 两个Servlet对象就有两个配置信息对象。



6.0 ServletConfig对象中到底包装了什么信息呢？

- ServletConfig对象中包装web.xml文件中\<servlet>\</servlet>标签的配置信息。

- Tomcat会解析web.xml文件，将web.xml文件中\<servlet>\</servlet>标签中的配置信息自动包装到ServletConfig对象中

~~~xml
      <servlet>
         <servlet-name>configTest</servlet-name>
         <servlet-class>com.bjpowernode.javaweb.servlet.ConfigTestServlet</servlet-class>
      </servlet>
~~~



7.0 ServletConfig接口中有哪些方法？

~~~xml
      <servlet>
           <!--配置servlet名字，可以获取-->
          <servlet-name>configTest</servlet-name>
          <!--配置servlet的全限定类名-->
          <servlet-class>com.bjpowernode.javaweb.servlet.ConfigTestServlet</servlet-class>
          <!--
				这里是可以配置一个Servlet对象的初始化信息的。
				可以获取配置的某个servlet的一些初始参数
				getInitParameter(String name)
				getInitParameterNames();
		   -->
          <init-param>
             <param-name>driver</param-name>
             <param-value>com.mysql.cj.jdbc.Driver</param-value>
          </init-param>
          <init-param>
              <param-name>url</param-name>
              <param-value>jdbc:mysql://localhost:3306/bjpowernode</param-value>
          </init-param>
          <init-param>
              <param-name>user</param-name>
              <param-value>root</param-value>
          </init-param>
          <init-param>
              <param-name>password</param-name>
              <param-value>root1234</param-value>
          </init-param>
      </servlet>
~~~

- 以上\<servlet><\/servlet>标签中的\<init-param>\</init-param>是初始化参数。这个初始化参数信息会被comcat封装到ServletConfig对象当中。



8.0 ServletConfig接口中有4个方法

- 第1个方法：

~~~java
public String getInitParameter(String name);
~~~

 *          第2个方法：

~~~java
public Enumeration<String> getInitParameterNames();
~~~

 *          第3个方法：

~~~java
public ServletContext getServletContext();          
~~~

- 第4个方法：

~~~java
public String getServletName();
~~~

**注意：**

**在GenericServlet中的init(ServletConfig servlerConfig),将GernericServlet中的成员变量ServletConifg赋值**

**该对象由Tomcat根据web.xml中servlet标签中的信息创建；**

**且GenervicServlet实现了ServletConfig接口所以说，在 `GenericServlet`    中可以   `直接使用`      `this` 调用 `  ServletConfig中的方法`，而不需要用成员变量servletConfig调用**



9.0 小结

什么是ServletConfig？

  - Servlet对象的配置信息对象
  - ServletConfig对象中封装了`<servlet></servlet>`标签中的配置信息。【web.xml文件中servlet的配置信息】

- `一个Servlet`对应  `一个ServletConfig对象`。

- **Servlet对象**   是    **Tomcat服务器创建**，并且 `ServletConfig对象` 也是 `  Tomcat服务器 创建`。并且默认情况下，他们都是在用户发送第一次请求的时候创建【创建servlet的同时创建servletconfig，因为调用servlet中的init方法时需要传入入参ServletConfig】

- `ServletConfig接口`的实现类是`Tomcat服务器给实现`的。（`Tomcat服务器`说的就是WEB服务器）
- 不同的应用服务器对ServletConfig接口的实现有不同，至少包名就不同



ServletConfig接口有哪些常用的方法？

~~~java
// 通过初始化参数的name获取value
 public String getInitParameter(String name); 

 // 获取所有的初始化参数的name
 public Enumeration<String> getInitParameterNames(); 

 // 获取ServletContext对象
 public ServletContext getServletContext(); 

 // 获取Servlet的name
 public String getServletName(); 
~~~



- 以上方法在Servlet类当中，都可以使用this去调用。因为**GenericServlet实现了ServletConfig接口**，实际上调用的是本类中的servletConfig对象 



## 11. ServletContext

1.0 ServletContext是什么？

  - ServletContext是`接口`，是   `Servlet规范`   中的一员  。



2.0 ServletContext是谁实现的？

 - Tomcat服务器（**WEB服务器**）实现了ServletContext接口。

~~~java
public class org.apache.catalina.core.ApplicationContextFacade implements ServletContext {}
~~~



3.0 `ServletContext对象`是谁创建的？在什么时候创建的？

 - **ServletContext对象**   是   **WEB服务器创建**   的。
 - `ServletContext对象`   在   ` WEB服务器启动的时候创建`。
 - 对于  **一个webapp**   来说，**ServletContext对象只有一个**。
 - **ServletContext对象**  在   **服务器关闭**   的时候**销毁**。



4.0 `ServletContext`怎么理解？

- Servlet对象的环境对象。（`Servlet对象的上下文对象`）
- `  ServletContext对象`   其实对应的就是    **整个web.xml文件**    。
- 放在    S**ervletContext对象**    `当中的数据`   ，**所有Servlet一定是共享的**。
- Tomcat是一个容器，一个容器当中可以放多个webapp，  **一个webapp**   对应一个   **ServletContext对象**。
- `ServletContext ` 是    `一个接口`，Tomcat服务器对ServletContext接口进行了实现。
- `ServletContext对象的创建`   也是`Tomcat服务器`来完成的。启动webapp的时候创建的。
- **`组件的部署过程 `  就是      ServletContext的创建过程**



5.0 `ServletContext对象    的   生命周期`

- **ServletContext对象在服务器启动阶段创建，在服务器关闭的时候销毁**。



6.0 `ServletContext接口`中有哪些   `常用的方法`？

~~~java
  // 通过初始化参数的name获取value
  public String getInitParameter(String name); 

  // 获取所有的初始化参数的name
  public Enumeration<String> getInitParameterNames(); 

  //获取上下文路径【虚拟目录】【应用名字】
  //这个方法可以动态获取应用的上下文路径
  public String getContextPath();

  // 获取文件的绝对路径
  public String getRealPath(String path);

  //通过ServletContext对象也是可以记录日志的
  public void log(String message);
  public void log(String message, Throwable t);
~~~



> getInitParameter(String name)
>
> getInitParameterNames();

~~~xml
<!--注意：以上的配置信息属于应用级的配置信息，一般一个项目中共享的配置信息会放到以上的标签当中。-->
<!--如果你的配置信息只是想给某一个servlet作为参考，那么你配置到servlet标签当中即可，使用ServletConfig对象来获取。-->
<context-param>
    <param-name>pageSize</param-name>
    <param-value>10</param-value>
</context-param>
<context-param>
    <param-name>startIndex</param-name>
    <param-value>0</param-value>
</context-param>
~~~



> getServletContext()

~~~java
// 获取应用的根路径【虚拟目录名】（非常重要），因为在java源代码当中有一些地方可能会需要应用的根路径，这个方法可以动态获取应用的根路径
// 在java源码当中，不要将应用的根路径写死，因为你永远都不知道这个应用在最终部署的时候，起一个什么名字。

//在GenericServlet的子类中
//servlet中，init方法的入参ServletConfig有一个方法：getServletContext();用于获取servlet上下文
//ServletContext application = servletConfig.getServletContext();

//GenericServlet实现了ServletContext接口
ServletContext application = this.getServletContext();
String contextPath = application.getContextPath();
~~~



>  getRealPath(String path)

~~~java
    // 后面的这个路径，加了一个“/”，这个“/”代表的是web的根
    //String realPath = application.getRealPath("/index.html"); // 可以
    // 你不加“/”，默认也是从根下开始找。
    //String realPath = application.getRealPath("index.html"); // 不加“/”也可以
  
  	// C:\Users\Administrator\IdeaProjects\javaweb\out\artifacts\servlet04_war_exploded\common\common.html
    String realPath = application.getRealPath("/common/common.html");
~~~



> log(String message)

~~~java
  /*
      Tomcat服务器的【logs目录】下都有哪些日志文件？
        服务器端的java程序运行的控制台信息。
  	  	catalina.2021-11-05.log 
  	  	
  	  	ServletContext对象的log方法记录的日志信息存储到这个文件中。
  	  	localhost.2021-11-05.log 
  	  	
  	  	访问日志
  	  	localhost_access_log.2021-11-05.txt 
  */
~~~



7.0 `ServletContext`**【应用域】** 是一个**域对象**

- 域对象，还有请求域、会话域

- 如果所有的用户   **共享一份数据**，并且这个数据   **很少被修改**，**并且这个数据量很少**，可以将这些数据放到`ServletContext`这个应用域中

  ~~~
  不是共享的没有意义。因为ServletContext这个对象只有一个。只有共享的数据放进去才有意义【不然，放在请求域或者是会话域就可以  自己独享】
  
  因为数据量比较大的话，太占用堆内存，并且这个对象的生命周期比较长，服务器关闭的时候，这个对象才会被销毁。大数据量会影响服务器的性能。占用内存较小的数据量可以考虑放进去
  
  所有用户共享的数据，如果涉及到修改操作，必然会存在线程并发所带来的安全问题。所以放在ServletContext对象中的数据一般都是只读的
  ~~~

  

- 数据量小、所有用户共享、又不修改，这样的数据放到ServletContext这个应用域当中，会大大提升效率。因为应用域相当于一个缓存，放到缓存中的数据，下次在用的时候，不需要从数据库中再次获取，大大提升执行效率



**操作域对象**的**三个方法**`通用`

~~~java
// 存（怎么向ServletContext应用域中存数据）
// map.put(k, v) 键是字符串 值是对象
public void setAttribute(String name, Object value); 

// 取（怎么从ServletContext应用域中取数据）
// Object v = map.get(k)
public Object getAttribute(String name);

// 删（怎么删除ServletContext应用域中的数据）
// map.remove(k)
public void removeAttribute(String name); 
~~~



## 12. HttpServlet

1.0 引入

- 以后我们编写Servlet类的时候，实际上是不会去直接继承GenericServlet类的
- 因为我们是     **B/S结构    的系统，这种系统是基于    HTTP超文本传输协议**    的
- ​      **在Servlet规范    当中，提供了一个类叫做HttpServlet**，它是专门为HTTP协议准备的一个Servlet类
- 我们编写的   **Servlet类    要   继承      HttpServlet**。HttpServlet是HTTP协议专用的。使用HttpServlet处理HTTP协议更便捷。



2.0 但是你需要直到它的继承结构：

~~~java
javax.servlet.Servlet// （接口）【爷爷】  servlet规范
    
javax.servlet.GenericServlet implements Servlet//（抽象类）【儿子】 servlet规范
    
javax.servlet.http.HttpServlet extends GenericServlet//（抽象类）【孙子】 servlet规范

//我们以后编写的Servlet直接继承HttpServlet类。【曾孙】根据规范实现
~~~





# 关于JavaEE的版本

- **JavaEE目前最高版本是         JavaEE8**
- `JavaEE`   被  ` Oracle`    捐献了，Oracle将JavaEE规范捐献给Apache了。
- `Apache`把J   avaEE    换名了，以后不叫JavaEE了，以后叫做 **jakarta EE**。
- 以后没有JavaEE了。以后都叫做`Jakarta EE`。
- `JavaEE8`版本升级之后的"JavaEE 9"，不再是"JavaEE9"这个名字了，叫做    **JakartaEE9**
- JavaEE8的时候对应的Servlet类名是：`javax.servlet.Servlet`
- JakartaEE9的时候对应的Servlet类名是：`jakarta.servlet.Servlet （`**包名都换了**）
- 如果你之前的项目还是在使用javax.servlet.Servlet，那么你的项目   **无法直接部署**  到    **Tomcat10+  版本**  上。你只能部署到Tomcat9-版本上
- 在Tomcat9以及Tomcat9之前的版本中还是能够识别javax.servlet这个包。





# 大家到目前为止都接触过哪些**缓存机制**

- ` 堆内存 ` 当中的  **字符串常量池**。

  - **"abc"** 先在字符串常量池中查找，如果有，直接拿来用。如果没有则新建，然后再放入字符串常量池。

- `堆内存 `   当中的   **整数型常量池**。

  - **[-128 ~ 127]** 一共  **256个Integer类型的    引用**，放在`整数型常量池`中。没有超出这个范围的话，直接从常量池中取。

- 连接池(**Connection Cache**) 【`数据库连接池`】

  - 这里所说的中的连接`数据库连接池`Java语言连接数据库的连接对象：**java.sql.Connection对象**。
  - **JVM  是   一个进程**。**MySQL数据库   是一个  进程**。    **进程    和    进程之间    建立连接，  打开通道    是很费劲的。是很耗费资源的**。
  - 此时，可以 `  提前先创建好  ` **N个Connection连接对象**，将`   连接对象 `  放到  一个集合当中，我们把这个放有   **Connection对象  的  集合**   称为  `连接池`。
  - 每一次用户连接的时候   不`需要   再   新建连接对象`，**省去了新建的环节，直接从连接池中获取连接对象，大大提升访问效率**。
  - **连接池**
    - **最小**  连接数
    - **最大**  连接数
    - 连接池可以提高用户的    访问**效率 ** 。当然也可以保证  ` 数据库的安全性`  。

- **线程池**

  -    **Tomcat服务器   本身就是支持   多线程   的**

  

  Tomcat服务器是在用户发送一次请求，就新建一个Thread线程对象吗？

  - 当然不是，实际上是**在   Tomcat服务器启动的时候**   ，会   ` 先创建好`   **N多个线程Thread对象**   ，然后   **将线程对象**    放到   **集合**   当中，称为**线程池**。
  - 用户发送    **请求过来之后**   ，需要有  ` 一个对应的线程   来处理这个请求`   ，这个时候线程对象就会直接从线程池中拿，效率比较高。
  - `所有的`   **WEB服务器**，或者  **应用服务器**，`都是支持多线程的`，都有**线程池机制**。

- **redis**

  - NoSQL数据库。非关系型数据库。缓存数据库。

- 向ServletContext应用域中存储数据，也等于是将数据存放到**缓存cache**当中了**【往域名对象中放数据】**





# HTTP协议

## 1.0 什么是协议？

- `协议            `       实际上     是制定好的    **一套规范**，大家都按照这个规范来，这样可以做到沟通无障碍。

- 我说的话你能听懂，你说的话，我也能听懂，这说明我们之间是有一套规范的，一套协议的

  这套协议就是：中国普通话协议。**我们都遵守这套协议，我们之间就可以沟通无障碍**。



## 2.0 什么是HTTP协议？

- `HTTP协议`：是`W3C`制定的一种`超文本传输协议`。

​		`通信协议`：提前制定好的 ` 发送消息的模板`



## 3.0 W3C：

- **万维网联盟**组织
- **负责制定标准的：`HTTP`    `HTML4.0`     `HTML5 `      `XML `    `DOM`   等规范都是W3C制定的**。
- 万维网之父：蒂姆·伯纳斯·李



## 4.0 什么是超文本？

- **超文本说的就是：不是   `普通文本`，比如  `流媒体  `：声音、视频、图片等**。
- HTTP协议支持：  **不但   可以传送    普通字符串，同样支持传递   ` 声音、视频、图片等流媒体`   信息**。

- **这种协议游走在B和S之间**。`B向S`发数据要遵循HTTP协议。`S向B`发数据同样需要遵循HTTP协议。
- 这样B和S才能  ` 解耦合`。B不会依赖特定的A，同样A也一样不会依赖特定的B



## 5.0 什么是解耦合？

- `随便一个Browser  `   不 依赖   **某个特定的Server**。
- `**随便一个**Server`也   不  依赖  **某个Browser**。

- B/S表示：B/S结构的系统（**浏览器访问WEB服务器的系统**）
- 浏览器   向   WEB服务器发送数据，叫做：请求（request)
- WEB服务器   向   浏览器发送数据，叫做：响应（response）



## 6.0 HTTP协议包括：

- **请求协议**
  - **浏览器  向   WEB服务器发送数据**的时候，这个发送的数据需要遵循一套标准，这套标准中规定了发送的数据具体格式。
- **响应协议**
  - **WEB服务器  向  浏览器发送数据**的时候，这个发送的数据需要遵循一套标准，这套标准中规定了发送的数据具体格式。



## 7.0  HTTP协议   就是   **提前制定好  的   一种消息模板**。

- 不管你是哪个品牌的浏览器，都是这么发。 
- 不管你是哪个品牌的WEB服务器，都是这么发。
- xx浏览器  可以向 Tomcat发送请求，也可以向Jetty服务器发送请求。**浏览器不依赖具体的服务器品牌**。
- **WEB服务器也不依赖具体的浏览器品牌**。可以是edg浏览器，也可以是Chrome浏览器，可以是IE，都行。



## 8.0 HTTP的请求协议（B --> S）

- **HTTP的请求协议**   包括：4部分

  - 请求行

  ~~~
  由三部分组成：第一部分：请求方式（7种）	第二部分：URI	第三部分：HTTP协议版本号
  
  1. 请求方式
      get（常用的）、post（常用的）、delete、 put 、head、 options、trace
  
  2. URI
  	什么是URI？ 
  		统一资源标识符。代表网络中某个资源的名字。但是通过URI是无法定位资源的。
  	
  	什么是URL？ 
  		统一资源定位符。代表网络中某个资源，同时，通过URL是可以定位到该资源的。
  		
  	URI和URL什么关系，有什么区别？
    		URL包括URI
   		http://localhost:8080/servlet05/index.html  这是URL。
    		/servlet05/index.html 						这是URI。
    		
    	发送GET请求，会通过URI传递参数
    	 以？开头，用&做分隔符
  	 key1=value1&key2=value2&key3=value3
    
  3. 第三部分：HTTP协议版本号
  	HTTP1.0是短链接 访问服务器中的一个资源，但该资源关联其他资源，如图片；此时会建立一个新的TCP连接去发送HTTP请求，请求资源
  	HTTP1.1是长连接 不会断开TCP连接，再发送一个新的HTTP请求，请求资源
  ~~~

  - 请求头

  ~~~
  1.0 请求的主机
  2.0 主机的端口
  3.0 浏览器信息息
  4.0 cookie等信息
  ~~~

  - 空白行

  ~~~
  用来区分“请求头”和“请求体”
  ~~~

  - 请求体

  ~~~
  向服务器发送的具体数据。
  
  用&做分隔符
  key1=value1&key2=value2&key3=value3
  ~~~

  

HTTP GET请求协议的具体报文：

~~~text
GET /servlet05/getServlet?username=lucy&userpwd=1111 HTTP/1.1                           请求行
Host: localhost:8080                                                                    请求头
Connection: keep-alive
sec-ch-ua: "Google Chrome";v="95", "Chromium";v="95", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/servlet05/index.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
                                                                                        空白行
                                                                                        请求体
~~~



HTTP POST请求协议的具体报文：

~~~text
POST /servlet05/postServlet HTTP/1.1                                                  请求行
Host: localhost:8080                                                                  请求头
Connection: keep-alive
Content-Length: 25
Cache-Control: max-age=0
sec-ch-ua: "Google Chrome";v="95", "Chromium";v="95", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: http://localhost:8080
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/servlet05/index.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
                                                                                      空白行
username=lisi&userpwd=123                                                             请求体
~~~



## 9.0 HTTP的响应协议（S --> B）

- HTTP的响应协议包括：4部分

  - 状态行

  ~~~
  由三部分组成：第一部分：HTTP协议版本号 第二部分：状态码   第三部分：状态的描述信息
  	1.0 协议版本号（HTTP/1.1）
  	
  	2.0 状态码（HTTP协议中规定的响应状态号。不同的响应结果对应不同的号码。）
  	   - 200 表示请求响应成功，正常结束。
  		- 404表示访问的资源不存在，通常是因为要么是你路径写错了，要么是路径写对了，但是服务器中对应的资源并没有启动成功。总之404错误是前端错误。
  		- 405表示前端发送的请求方式与后端请求的处理方式不一致时发生;没有重写HttpServlet中的doXxx方法
    		  - 比如：前端是POST请求，后端的处理方式按照get方式进行处理时，发生405
    		  - 比如：前端是GET请求，后端的处理方式按照post方式进行处理时，发生405
  		- 500表示服务器端的程序出现了异常。一般会认为是服务器端的错误导致的。
  			- 以4开始的，一般是浏览器端的错误导致的。
  			- 以5开始的，一般是服务器端的错误导致的。
  			
  	3.0 状态的描述信息
  		- ok 表示正常成功结束。
  		- not found 表示资源找不到。
  ~~~

  - 响应头

  ~~~
  - 响应的内容类型
  - 响应的内容长度
  - 响应的时间
  - .......
  ~~~

  - 空白行

  ~~~
  用来分隔“响应头”和“响应体”的。
  ~~~

  - 响应体

  ~~~
  应体就是响应的正文，这些内容是一个长的字符串，这个字符串被浏览器渲染，解释并执行，最终展示出效果。
  ~~~

  

- HTTP响应协议的具体报文：

  - ```
    HTTP/1.1 200 ok                                     状态行
    Content-Type: text/html;charset=UTF-8               响应头
    Content-Length: 160
    Date: Mon, 08 Nov 2021 13:19:32 GMT
    Keep-Alive: timeout=20
    Connection: keep-alive
                                                        空白行
    <!doctype html>                                     响应体
    <html>
        <head>
            <title>from get servlet</title>
        </head>
        <body>
            <h1>from get servlet</h1>
        </body>
    </html>
    ```

10. 怎么向服务器发送GET请求，怎么向服务器发送POST请求？

- 到目前为止，只有一种情况可以发送POST请求：**使用form表单，并且form标签中的method属性值为：method="post"**。
- 其他所有情况一律都是get请求：
  - 在**浏览器地址栏上直接输入URL，敲回车，属于get请求**。
  - 在浏览器上直接点击**超链接**，属于get请求。
  - 使用**form表单提交数据时，form标签中没有写method属性，默认就是get**
  - 或者使用form的时候，form标签中method属性值为：method="get"
  - ....



## 11. GET请求和POST请求有什么区别？

- **get请求发送数据**的时候，**数据会挂在URI的后面**，并且在URI后面添加一个**“?”**，"?"后面是数据。这样会导致发送的数据回显在浏览器的地址栏上。（get请求在“请求行”上发送数据）

  ~~~
  http://localhost:8080/servlet05/getServlet?username=zhangsan&userpwd=1111
  ~~~

- **post请求发送数据的时候，在请求体当中发送**。不会回显到浏览器的地址栏上。也就是说post发送的数据，在浏览器地址栏上看不到。（**post在“请求体”当中发送数据**）

- **post请求可以发送任何类型的数据，包括普通字符串，流媒体等信息：视频、声音、图片**。 **get请求只能发送普通的字符串。并且发送的字符串长度有限制，不同的浏览器限制不同。这个没有明确的规范**。

- post请求可以发送**大数据量**，理论上没有长度限制。get请求**无法**发送大数据量。

- get请求在**W3C中是这样说的：get请求比较适合从服务器端获取数据**。

- post请求在**W3C中是这样说的：post请求比较适合向服务器端传送数据**。

- get请求是安全的。get请求是绝对安全的。为什么？因为get请求只是为了从服务器上获取数据。不会对服务器造成威胁。（get本身是安全的，你不要用错了。用错了之后又冤枉人家get不安全，你这样不好（太坏了），那是你自己的问题，不是get请求的问题。）

- post请求是危险的

  为什么？因为post请求是向服务器提交数据，如果这些数据通过后门的方式进入到服务器当中，服务器是很危险的

  另外post是为了提交数据，所以一般情况下拦截请求的时候，大部分会选择拦截（监听）post请求。

- get**请求    支持缓存**。

~~~
https://n.sinaimg.cn/finance/590/w240h350/20211101/b40c-b425eb67cabc342ff5b9dc018b4b00cc.jpg
~~~

​	**任何一个** `  get请求`   **最终的“响应结果”**都会被浏览器   **缓存起来**  

​	在浏览器缓存当中：

​				一个get请求的路径a  对应  一个资源。

​				一个get请求的路径b  对应  一个资源。

​				一个get请求的路径c  对应  一个资源。

​	实际上，**你   只要   发送    get请求，   浏览器   做的   第一件事   都是先从   本地浏览器缓存    中找，找不到的时候才会去    服务器上获取**。

​	这种缓存机制目的是为了提高用户的体验

​	有没有这样一个需求：我们**不希望get请求走缓存**，怎么办？怎么避免走缓存？

​	我希望每一次这个get请求都去服务器上找资源，我不想从本地浏览器的缓存中取。

​		**只要每一次get请求的请求路径不同即可**。

> https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=789789787897898
>
> https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=789789787897899
>
> https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=系统毫秒数
>
> 在路径的后面添加一个每时每刻都在变化的“时间戳”，这样，每一次的请求路径都不一样，浏览器就不走缓存了。

- post请求不支持缓存。（`POST`是用来   修改服务器端   的  资源  的）
  - post请求之后，服务器“响应的结果”不会被浏览器缓存起来。因为这个缓存没有意义。



## 12. GET请求和POST请求如何选择，什么时候使用GET请求，什么时候使用POST请求？

- 怎么选择GET请求和POST请求呢？衡量标准是什么呢？

  - 你这个请求是想获取服务器端的数据，还是想向服务器发送数据。如果你是想从服务器上获取资源，建议使用GET请求
  - 如果你这个请求是为了向服务器提交数据，建议使用POST请求。
  - 大部分的form表单提交，都是post方式，因为**form表单中要填写大量的数据**，这些数据是收集用户的信息，一般是需要传给服务器，服务器将这些数据保存/修改等。
  - 如果**表单中有敏感信息**，还是建议适用post请求，因为get请求会回显敏感信息到浏览器地址栏上。（例如：密码信息）
  - 做**文件上传**，一定是post请求。**要传的数据不是普通文本**。
  - **其他情况都可以使用get请求**。

  

  **不管你是get请求还是post请求，发送的请求数据格式是完全相同的，只不过位置不同，格式都是统一的**：

  - name=value&name=value&name=value&name=value
  - name是什么？
    - 以form表单为例：form表单中input标签的name属性的值。
  - value是什么？
    - 以form表单为例：form表单中input标签的value的值。





# 模板方法设计模式

## 1.0 什么是设计模式？

-  **某个问题   的   固定    的解决方案**。(可以被重复使用)



## 2.0 你知道哪些设计模式？

![image-20220505233101335](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261720433.png)

---



## 3.0 GoF设计模式：

- 通常我们所说的  **23种** `  设计模式`。（Gang of Four：4人组提出的设计模式）

1. 单例（Singleton）模式：某个类  **只能**   **生成一个实例**，该类提供了一个全局访问点供外部获取该实例，其拓展是有限多例模式。
2. 原型（Prototype）模式：将一个对象作为原型，通过对其进行复制而克隆出多个和原型类似的新实例。
3. 工厂方法（Factory Method）模式：`定义一 个   用于创建产品的 ` **接口**，由  **子类**  **决定生产什么产品**。
4. 抽象工厂（AbstractFactory）模式：提供一个创建产品族的接口，其每个子类可以生产一系列相关的产品。
5. 建造者（Builder）模式：将一个复杂对象分解成多个相对简单的部分，然后根据不同需要分别创建它们，最后构建成该复杂对象。
6. 代理（Proxy）模式：为某对象提供一种代理以控制对该对象的访问。即客户端通过代理间接地访问该对象，从而限制、增强或修改该对象的一些特性
7. 适配器（Adapter）模式：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。
8. 桥接（Bridge）模式：将抽象与实现分离，使它们可以独立变化。它是用组合关系代替继承关系来实现，从而降低了抽象和实现这两个可变维度的耦合度。
9. 装饰（Decorator）模式：动态的给对象增加一些职责，即增加其额外的功能。
10. 外观（Facade）模式：为多个复杂的子系统提供一个一致的接口，使这些子系统更加容易被访问。
11. 享元（Flyweight）模式：运用共享技术来有效地支持大量细粒度对象的复用。
12. 组合（Composite）模式：将对象组合成树状层次结构，使用户对单个对象和组合对象具有一致的访问性。
13. 模板方法（TemplateMethod）模式：定义一个操作中的算法骨架，而将算法的一些步骤延迟到子类中，使得子类可以不改变该算法结构的情况下重定义该算法的某些特定步骤。
14. 策略（Strategy）模式：定义了一系列算法，并将每个算法封装起来，使它们可以相互替换，且算法的改变不会影响使用算法的客户。
15. 命令（Command）模式：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。
16. 职责链（Chain of Responsibility）模式：把请求从链中的一个对象传到下一个对象，直到请求被响应为止。通过这种方式去除对象之间的耦合。
17. 状态（State）模式：允许一个对象在其内部状态发生改变时改变其行为能力。
18. 观察者（Observer）模式：多个对象间存在一对多关系，当一个对象发生改变时，把这种改变通知给其他多个对象，从而影响其他对象的行为。
19. 中介者（Mediator）模式：定义一个中介对象来简化原有对象之间的交互关系，降低系统中对象间的耦合度，使原有对象之间不必相互了解。
20. 迭代器（Iterator）模式：提供一种方法来顺序访问聚合对象中的一系列数据，而不暴露聚合对象的内部表示。
21. 访问者（Visitor）模式：在不改变集合元素的前提下，为一个集合中的每个元素提供多种访问方式，即每个元素有多个访问者对象访问。
22. 备忘录（Memento）模式：在不破坏封装性的前提下，获取并保存一个对象的内部状态，以便以后恢复它。
23. 解释器（Interpreter）模式：提供如何定义语言的文法，以及对语言句子的解释方法，即解释器。





## 4.0 JavaEE设计模式：

**1、开闭原则（Open Close Principle）**

​		对扩展开放，对修改关闭。

**2、里氏代换原则（Liskov Substitution Principle）**

​		只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。

**3、依赖倒转原则（Dependence Inversion Principle）**

​		这个是开闭原则的基础，对接口编程，依赖于抽象而不依赖于具体。

**4、接口隔离原则（Interface Segregation Principle**）

​		使用多个隔离的接口来降低耦合度。

**5、迪米特法则（最少知道原则）（Demeter Principle）**

​		一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

**6、合成复用原则（Composite Reuse Principle）**

​		原则是尽量使用合成/聚合的方式，而不是使用继承。继承实际上破坏了类的封装性，超类的方法可能会被子类修改。

- DAO
- DTO
- VO
- PO
- pojo：POJO是DO、DTO、BO、VO的统称
- ....



## 7.0 什么是模板方法设计模式？

- **在   模板类   的    模板方法   当中定义  核心算法骨架  ，具体的实现步骤   可 以   延迟到   子类当中完成**。

- **模板类通常是一个抽象类**，模板类当中的模板方法定义核心算法，这个方法**通常是final**的（但也可以不是final的）
- 模板类当中的抽象方法就是   **不确定实现的方法**   ，这个不确定怎么实现的事儿交给子类去做    **(模板方法中确定的是算法)**。





# HttpServlet源码分析

## 1.0 前提引入

- ​    **HttpServlet类    是专门为     HTTP协议准备**   的。比GenericServlet更加适合HTTP协议下的开发。
- HttpServlet在哪个包下？

​		`javax.servlet.http.HttpServlet`



## 2.0 到目前为止我们**接触了servlet规范中哪些接口**？

- `javax.servlet.Servlet ` **核心接口**（接口）

​		五个方法：init、destroy、service、getServletConfig、getServletInfo

- `javax.servlet.ServletConfig ` **Servlet配置信息接口**（接口）

​		四个方法：	getInitParameterNames、getInitParameter、getServletConfig、getServletName

- `javax.servlet.ServletContext` **Servlet上下文接口**（接口）
- `javax.servlet.ServletRequest `   **Servlet请求接口**（接口）
- `javax.servlet.ServletResponse`   **Servlet响应接口**（接口）
- `javax.servlet.ServletException` **Servlet异常（类）**
- `javax.servlet.GenericServlet `**标准通用的Servlet类（抽象类）【适配器】**



## 3.0 http包下都有哪些类和接口呢？javax.servlet.http.*;

- `javax.servlet.http.HttpServlet `（**HTTP协议专用的Servlet类，抽象类**）
- `javax.servlet.http.HttpServletRequest `（**HTTP协议专用的请求对象**）
- `javax.servlet.http.HttpServletResponse` （**HTTP协议专用的响应对象**）



## 4.0 HttpServletRequest对象中封装了什么信息？

- `HttpServletRequest`，简称`request`对象。
- **HttpServletRequest中封装了    HTTP   请求协议    的全部内容**。
- **Tomcat服务器（`WEB服务器`）将`“请求协议`”中的数据全部解析出来，然后将这些数据   ` 全部封装到request对象`当中了**。
- 也就是说，我们只要面向HttpServletRequest，就可以获取请求协议中的数据。

- **HttpServletResponse对象是专门用来响应HTTP协议到浏览器**的。



## 5.0 回忆Servlet生命周期？

- 用户第一次请求
  - **Tomcat服务器**  通过   **反射机制**，调用   **无参数构造**方法  。**创建Servlet对象**。(web.xml文件中配置的j请求地址对应的 Servlet类)
  - **Tomcat服务器**   调用 `  Servlet对象`的 `  init方法 `  完成初始化。
  - Tomcat服务器调用Servlet对象的    **service方法处理请求**。
- 用户第二次请求
  - Tomcat服务器调用Servlet对象的   **service方法处理请求**。
- 用户第三次请求
  - Tomcat服务器调用Servlet对象的   **service方法处理请求**。
- ....
  - Tomcat服务器调用Servlet对象的service方法处理请求。
- 服务器关闭
  - Tomcat服务器调用Servlet对象的destroy方法，做   **销毁之前  **的  准备工作。
  - Tomcat服务器销毁Servlet对象。



## 6.0 HttpServlet源码分析：

- 继承HttpServlet的普通servlet类

~~~java
public class HelloServlet extends HttpServlet {
	// 用户第一次请求，创建HelloServlet对象的时候，会执行这个无参数构造方法。
	public HelloServlet() {
    }
    
    //override 重写 doGet方法
    //override 重写 doPost方法
}
~~~

- GenericServlet

```java
public abstract class GenericServlet implements Servlet, ServletConfig,
        java.io.Serializable {
           
	// 用户第一次请求的时候，HelloServlet对象第一次被创建之后，这个init方法会执行。
    //这个是模板方法
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }
            
	// 用户第一次请求的时候，带有参数的init(ServletConfig config)执行之后，会执行这个没有参数的init()
    //这个方法主要是用来给子类重写
	public void init() throws ServletException {
        // NOOP by default
    }
}
```

- HttpServlet

~~~java
// HttpServlet模板类。
public abstract class HttpServlet extends GenericServlet {
    // 用户发送第一次请求的时候这个service会执行
    // 用户发送第N次请求的时候，这个service方法还是会执行。
    // 用户只要发送一次请求，这个service方法就会执行一次。
    @Override
    public void service(ServletRequest req, ServletResponse res)
        throws ServletException, IOException {

        HttpServletRequest  request;
        HttpServletResponse response;

        try {
            // 将ServletRequest和ServletResponse向下转型为带有Http的HttpServletRequest和HttpServletResponse
            request = (HttpServletRequest) req;
            response = (HttpServletResponse) res;
        } catch (ClassCastException e) {
            throw new ServletException(lStrings.getString("http.non_http"));
        }
        // 调用重载的service方法。
        service(request, response);
    }
    
    // 这个service方法的两个参数都是带有Http的。
    // 这个service是一个模板方法。
    // 在该方法中定义核心算法骨架，具体的实现步骤延迟到子类中去完成。
    protected void service(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
        // 获取请求方式
        // 这个请求方式最终可能是：""
        // 注意：request.getMethod()方法获取的是请求方式，可能是七种之一：
        // GET POST PUT DELETE HEAD OPTIONS TRACE
        String method = req.getMethod();

        // 如果请求方式是GET请求，则执行doGet方法。
        if (method.equals(METHOD_GET)) {
            long lastModified = getLastModified(req);
            if (lastModified == -1) {
                // servlet doesn't support if-modified-since, no reason
                // to go through further expensive logic
                doGet(req, resp);
            } else {
                long ifModifiedSince;
                try {
                    ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
                } catch (IllegalArgumentException iae) {
                    // Invalid date header - proceed as if none was set
                    ifModifiedSince = -1;
                }
                if (ifModifiedSince < (lastModified / 1000 * 1000)) {
                    // If the servlet mod time is later, call doGet()
                    // Round down to the nearest second for a proper compare
                    // A ifModifiedSince of -1 will always be less
                    maybeSetLastModified(resp, lastModified);
                    doGet(req, resp);
                } else {
                    resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
                }
            }

        } else if (method.equals(METHOD_HEAD)) {
            long lastModified = getLastModified(req);
            maybeSetLastModified(resp, lastModified);
            doHead(req, resp);

        } else if (method.equals(METHOD_POST)) {
            // 如果请求方式是POST请求，则执行doPost方法。
            doPost(req, resp);

        } else if (method.equals(METHOD_PUT)) {
            doPut(req, resp);

        } else if (method.equals(METHOD_DELETE)) {
            doDelete(req, resp);

        } else if (method.equals(METHOD_OPTIONS)) {
            doOptions(req,resp);

        } else if (method.equals(METHOD_TRACE)) {
            doTrace(req,resp);

        } else {
            //
            // Note that this means NO servlet supports whatever
            // method was requested, anywhere on this server.
            //

            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[1];
            errArgs[0] = method;
            errMsg = MessageFormat.format(errMsg, errArgs);

            resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
        }
    }
    
    //HttpServlet中的doGet方法
    //子类不重写方法但调用了这个方法，走到了父类就405
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException{
        // 报405错误
        String msg = lStrings.getString("http.method_get_not_supported");
        sendMethodNotAllowed(req, resp, msg);
    }
    
    //HttpServlet中的doPost方法
        //子类不重写方法但调用了这个方法，走到了父类就405
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
        // 报405错误
        String msg = lStrings.getString("http.method_post_not_supported");
        sendMethodNotAllowed(req, resp, msg);
    }
}
~~~

- 源码分析
  	假设前端发送的请求是get请求，后端程序员重写的方法是doPost
  	假设前端发送的请求是post请求，后端程序员重写的方法是doGet
  	会发生什么呢？
  		发生405这样的一个错误。
  		4**05表示前端的错误**，**发送的请求方式不对**。和服务器不一致。不是服务器需要的请求方式。
  	通过以上源代码可以知道：只要`HttpServlet类`中的    **doGet方法或doPost方法**  执行了，必然405.

怎么避免405的错误呢？
	后端重写了doGet方法，前端一定要发get请求。
	后端重写了doPost方法，前端一定要发post请求。
	这样可以避免405错误。
	

这种前端到底需要发什么样的请求，其实应该后端说了算。后端让发什么方式，前端就得发什么方式。

有的人，你会看到为了避免405错误，在Servlet类当中，将doGet和doPost方法都进行了重写。这样，确实可以避免405的发生，但是不建议，405错误还是有用的。该报错的时候就应该让他报错。如果你要是同时重写了doGet和doPost，那还不如你直接重写service方法好了。这样代码还能少写一点。


- 问题

​		我们编写的HelloServlet直接继承HttpServlet，直接重写HttpServlet类中的service()方法行吗？

​			可以，只不过你享受不到HTTP协议专属的东西。如;405错误



## 7.0 到今天我们终于得到了最终的一个Servlet类的开发步骤：

- 第一步：编写一个Servlet类，直接   **继承HttpServlet**
- 第二步：重写doGet方法或者重写doPost方法，到底重写谁，javaweb程序员说了算。
- 第三步：将Servlet类配置到web.xml文件当中。
- 第四步：准备前端的页面（form表单），form表单中指定请求路径即可。





# 关于一个web站点的欢迎页面

## 1.0 什么是一个web站点的欢迎页面？

- 对于一个webapp来说，我们是可以设置它的欢迎页面的。

- 设置了欢迎页面之后，当你访问这个webapp的时候，或者访问这个web站点的时候，**没有指定任何“资源路径”**，这个时候会  **默认访问你的欢迎页面**。

- 我们一般的访问方式是：

  - http://localhost:8080/servlet06/login.html 这种方式是指定了要访问的就是login.html资源。

- 如果我们访问的方式是：

  - http://localhost:8080/servlet06 如果我们访问的就是这个站点，**没有指定具体的资源路径**。它默认会访问谁呢？

    默认会访问你设置的欢迎页面。



## 2.0 怎么设置欢迎页面呢？

- 第一步：我在IDEA工具的web目录下【根目录下】新建了一个文件login.html

- 第二步：**在    web.xml文件    中进行了以下的配置**

  - ```xml
    <welcome-file-list>
            <welcome-file>login.html</welcome-file>
        </welcome-file-list>
    ```

  - 注意：**设置欢迎页面的时候，这个路径不需要以“/”开始。并且这个路径  默认   是从   webapp的根下    开始查找**。

- 第三步：启动服务器，浏览器地址栏输入地址

  - http://localhost:8080/servlet07



## 3.0 如果在webapp的根下新建一个目录，目录中再给一个文件，那么这个欢迎页该如何设置呢？

​	1.0 在webapp根下新建page1

​	2.0 在page1下新建page2目录

​	3.0 在page2目录下新建page.html页面

​	4.0 在web.xml文件中应该这样配置

~~~xml
<welcome-file-list>
    <welcome-file>page1/page2/page.html</welcome-file>
</welcome-file-list>
~~~

注意：**路径不需要以“/”开始，并且路径默认从webapp的根下开始找**。



## 4.0 **一个webapp是可以设置多个欢迎页面**的

~~~xml
<welcome-file-list>
    <welcome-file>page1/page2/page.html</welcome-file>
    <welcome-file>login.html</welcome-file>
</welcome-file-list>
~~~

​	注意：   **越靠上    的     优先级越高。找不到的继续向下找**。



有没有注意一件事：当我的文件名设置为index.html的时候，不需要在web.xml文件中进行配置欢迎页面。这是为什么？

- 这是因为小猫咪Tomcat服务器已经提前配置好了。



## 5.0  实际上**配置欢迎页面有两个地方可以配置**：

- 一个是在webapp内部的web.xml文件中。（在这个地方配置的属于   **局部配置**   ）

- 一个是在      **CATALINA_HOME/conf/web.xml**文件    中进行配置。（在这个地方配置的属于   **全局配置**   ）

  - ```xml
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
    ```

  - Tomcat服务器的全局欢迎页面是：index.html index.htm index.jsp

  - 如果你一个web站点没有设置局部的欢迎页面，Tomcat服务器就会以index.html index.htm index.jsp作为一个web站点的欢迎页面。

- 注意原则：**局部优先原则。（就近原则）**





## 6.0 **欢迎页可以是一个Servlet**吗？

- 当然可以。
- 你不要多想，欢迎页就是一个资源，既然是一个资源，那么可以是静态资源，也可以是动态资源。
- 静态资源：HTML 图片 音乐....
- 动态资源：Servlet类....



- servlet作为欢迎页步骤：

  - 第一步：写一个Servlet

    - ```java
      public class WelcomeServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
              response.setContentType("text/html");
              PrintWriter out = response.getWriter();
              out.print("<h1>welcome to bjpowernode!</h1>");
          }
      }
      ```

      

  - 第二步：在web.xml文件中配置servlet

    - ```xml
          <servlet>
              <servlet-name>welcomeServlet</servlet-name>
              <servlet-class>com.bjpowernode.javaweb.servlet.WelcomeServlet</servlet-class>
          </servlet>
          <servlet-mapping>
              <servlet-name>welcomeServlet</servlet-name>
              <url-pattern>/fdsa/fds/a/fds/af/ds/af/dsafdsafdsa</url-pattern>
          </servlet-mapping>
      ```

    

  - 第三步：在web.xml文件中配置欢迎页

    - ```xml
          <welcome-file-list>
              <welcome-file>fdsa/fds/a/fds/af/ds/af/dsafdsafdsa</welcome-file>
          </welcome-file-list>
      ```

      



# 关于WEB-INF目录

- 在WEB-INF目录下新建了一个文件：welcome.html
- 打开浏览器访问：http://localhost:8080/servlet07/WEB-INF/welcome.html 出现了404错误。
- 注意：**放在    WEB-INF目录下   的   资源   是受保护的。在   浏览器上   不能够  通过路径   直接访问**。
- 所以像HTML、CSS、JS、image等静态资源**一定要放到WEB-INF目录  之外**。  





## HttpServletRequest接口详解

## 1.0 前提引入

- HttpServletRequest是一个接口，全限定名称：`jakarta.servlet.http.HttpServletRequest`
- **HttpServletRequest接口是     Servlet规范中的一员**。
- `HttpServletRequest接口  `的   父接口：`ServletRequest `

~~~java
public interface HttpServletRequest extends ServletRequest {}
~~~



## 2.0 HttpServletRequest接口的实现类谁写的? HttpServletRequest对象是谁给创建的？

1.0 实验结论

- 通过测试：**`org.apache.catalina.connector.RequestFacade `实现了 `HttpServletRequest`接口**

  ~~~java
  public class RequestFacade implements HttpServletRequest {}
  ~~~

- 测试结果说明：Tomcat服务器（WEB服务器、WEB容器）实现了HttpServletRequest接口，`HttpServletRequest接口`属于`Servlet规范`。

- 对于我们javaweb程序员来说，实际上不需要关心这个，我们只需要面向接口编程即可。

- 我们关心的是HttpServletRequest接口中有哪些方法，这些方法可以完成什么功能！！！！



2.0 HttpServletRequest对象中都有什么信息？都包装了什么信息？

- HttpServletRequest对象是Tomcat服务器负责创建的。这个对象中  ` 封装HTTP的请求协议`。

- 实际上是用户发送请求的时候，遵循了HTTP协议，发送的是HTTP的请求协议

- **Tomcat服务器   将    HTTP协议中的信息以及数据全部解析出来  ，然后Tomcat服务器把这些信息   封装到   HttpServletRequest对象当中**

  传给了我们javaweb程序员。

- javaweb程序员面向HttpServletRequest接口编程，调用方法就可以获取到请求的信息了。



3.0 request和response对象的生命周期？

- request对象和response对象，一个是	`请求对象`，一个是`响应对象`。这两个对象只在**当前请求**中有效。
- `一次请求`对应  **一个request**。
- 两次请求则对应两个request。
- .....



4.0 HttpServletRequest接口中有哪些常用的方法？

​	以下的4个方法，和获取用户提交的数据有关系。

> Map<String,String[]> getParameterMap()                   这个是获取Map

> Enumeration<String> getParameterNames()          这个是获取Map集合中所有的key

> String[] getParameterValues(String name)                 根据key获取Map集合的value

> String getParameter(String name)                               获取value这个**一维数组当中的第一个元素**。这个方法**最常用**



思考：如果是你，前端的form表单提交了数据之后，你准备怎么存储这些数据，你准备采用什么样的数据结构去存储这些数据呢？

- **前端提交的数据格式**：username=abc&userpwd=111&aihao=s&aihao=d&aihao=tt
- 我会采用Map集合来存储：

~~~java
首当其冲 是 Map<String,String>
    key存储String
    value存储String
 这种想法对吗？不对。
    如果采用以上的数据结构存储会发现key重复的时候value覆盖。
    key         value
    ---------------------
    username    abc
    userpwd     111
    aihao       s
    aihao       d
    aihao       tt
  这样是不行的，因为map的key不能重复。
    
 改进：Map<String, String[]>
    key存储String
    value存储String[]
    key				value
    -------------------------------
    username		{"abc"}
    userpwd			{"111"}
    aihao			{"s","d","tt"}
~~~

注意：前端表单提交数据的时候，假设提交了120这样的“数字”，其实是以字符串"120"的方式提交的，所以服务器端获取到的一定是一个字符串的"120"，而不是一个数字。（     **前端     永远    提交的是      字符串    ，   后端    获取的也   永远是字符串**。）



获取请求参数的四个方法

~~~java
	Map<String,String[]> parameterMap = request.getParameterMap();
	Enumeration<String> names = request.getParameterNames();
	String[] values = request.getParameterValues("name");
	String value = request.getParameter("name");
~~~



# 域对象

**1.0    应用域对象**   是什么？

- `ServletContext `（`Servlet上下文对象`）
- 什么情况下会考虑向ServletContext这个应用域当中绑定数据呢？

  - 第一：`所有用户`   **共享的数据**。
  - 第二：这个共享的 ` 数据量很小`。
  - 第三：这个共享的数据很少的修改操作。
  - 在以上三个条件都满足的情况下，使用这个应用域对象，可以大大提高我们程序执行效率。
  - **实际上向应用域当中绑定数据，就相当于把数据放到了缓存（Cache）当中，然后用户访问的时候直接从缓存中取，减少IO的操作**，大大提升系统的性能，所以缓存技术是提高系统性能的重要手段



2.0 请求域”对象

- `ServletRequest`

- “请求域”对象要比“应用域”对象范围小很多。生命周期短很多。**请求域    只在     一次请求内    有效**。

- **`一个请求对象request`对应一个请求域对象**。**一次请求结束之后，这个请求域就销毁了**。

- 请求  **域对象**  也有这三个方法：

  - ```java
    void setAttribute(String name, Object obj); // 向域当中绑定数据。
    Object getAttribute(String name); // 从域当中根据name获取数据。
    void removeAttribute(String name); // 将域当中绑定的数据移除
    ```

  3.0 请求域和应用域的  **选用原则**？

  - **尽量使用小的域对象**，因为小的域对象占用的资源较少。





# 在一个web应用中应该如何完成资源的跳转

## 1.0 在一个web应用中通过两种方式，可以完成资源的跳转：

- 第一种方式：**转发**
- 第二种方式：**重定向**



## 2.0 两个Servlet怎么共享数据？

- 将数据   **放到ServletContext   应用域   **当中，当然是可以的，但是应用域范围太大，占用资源太多。**不建议使用**。
- 可以将数据放到  `ServletRequest  `**请求域**当中，然后AServlet   **转发**   到BServlet，保证AServlet和BServlet在**同一次请求当中**
- 这样就可以做到两个Servlet，或者`多个Servlet   `   共享   同一份数据。



## 3.0 转发的下一个资源必须是一个Servlet吗？

- `不一定`，**只要是Tomcat服务器当中的合法资源，都是可以转发的**。例如：html....
- 注意：**转发的时候，路径的写法要注意，转发的路径  ` 以“/”开始`   ，`不加项目名`**
- **服务器中进行转发，或     ` 服务器中请求服务器中的URI不用加项目名`**



## 4.0 关于request对象中两个非常容易混淆的方法：

~~~java
// uri?username=zhangsan&userpwd=123&sex=1
//获取的是用户在浏览器上提交的数据。
String username = request.getParameter("username");

// 之前一定是执行过：request.setAttribute("name", new Object())
//获取的是请求域当中绑定的数据。
Object obj = request.getAttribute("name");
~~~

以上两个方法的区别是什么？
	第一个方法：获取的是用户在`浏览器上提交的数据`。(**HttpservletRequest**)
	第二个方法：获取的是`请求域当中绑定的数据`。(**ServletRequest**)



## 5.0 转发和重定向有什么区别？

- 转发(以`/`开始，不用带项目名)

​		注意：设置欢迎页时，不以`/`开始，默认从webapp开始找

~~~java
// 获取请求转发器对象
//【在后台中转发，不经过浏览器，所以不用带项目名】
RequestDispatcher dispatcher = request.getRequestDispatcher("/dept/list");
// 调用请求转发器对象的forward方法完成转发
dispatcher.forward(request, response);


// 转发的时候是一次请求，不管你转发了多少次。都是一次请求。【浏览器中地址不变】
// AServlet转发到BServlet，再转发到CServlet，再转发到DServlet，不管转发了多少次，都在同一个request当中。【可以使用域对象共享数据】
// 这是因为调用forward方法的时候，会将当前的request和response对象传递给下一个Servlet。
~~~

- 重定向

~~~java
// 注意：路径上要加一个项目名。为什么？
//【浏览器发送请求，请求路径上是需要添加项目名的】。
    
// 以下这一行代码会将请求路径“/oa/dept/list”发送给浏览器
// 浏览器会自发的向服务器发送一次全新的请求：/oa/dept/list
response.sendRedirect("/oa/dept/list");
~~~



1.0 形式上有什么区别？

- **请求转发**（一次请求）【**会保持上一次请求的请求方式**】

  - 在浏览器地址栏上发送的请求是：http://localhost:8080/servlet10/a ，最终请求结束之后，浏览器地址栏上的地址还是这个。没变。

  ---

  

![image-20220502225351430](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261720650.png)

---



- **重定向**（两次请求**）【get方式】**
  - 在浏览器地址栏上发送的请求是：http://localhost:8080/servlet10/a ，最终在浏览器地址栏上显示的地址是：http://localhost:8080/servlet10/b

---

![image-20220502225300504](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261720006.png)

---



2.0 转发和重定向的本质区别？

- 转发：是由**`WEB服务器`来控制**的。A资源跳转到B资源，这个跳转动作是Tomcat   **服务器内部**  完成的。
- 重定向：是浏览器完成的。  **具体跳  转  到哪个资源，是   浏览器说了算**。



3.0 转发和重定向应该如何选择？什么时候使用转发，什么时候使用重定向？

- 如果在`上一个Servlet当中向request域当中绑定了数据`，希望从 ` 下一个Servlet当中把request域里面的数据取出来`，使用转发机制。
- 剩下所有的请求均使用重定向。（**重定向使用较多**）
- 转发会存在**浏览器的刷新问题。**刷新的不是当前URL，而是请求链的URL



4.0 **跳转**的下一个资源有没有要求呢？必须是一个Servlet吗？

- 不一定，跳转的资源只要是服务器内部合法的资源即可。包括：Servlet、JSP、HTML、图片



# HttpServletRequest接口的其他常用方法：

## 1.0 获取客户端的IP地址

> String remoteAddr = request.getRemoteAddr();



## 2.0 字符集问题

- get请求在  ` 请求行`  上提交数据。
- post请求在 `请求体` 中提交数据。
- `设置请求体的字符集`。（显然这个方法是处理POST请求的乱码问题。这种方式并   **不能解决 **   `get请求的乱码问题`）
- `Tomcat10之后`，**request请求体**  当中的   **字符集默认就是UTF-8**，不需要设置字符集，不会出现乱码问题。(`对应jakatarEE`)
- Tomcat9前（包括9在内），如果前端请求体提交的是中文，后端获取之后出现乱码



**针对请求体中的字符**

怎么解决这个乱码？执行以下代码。

~~~java
request.setCharacterEncoding("UTF-8");
~~~



- 在Tomcat9之前（包括9），响应中文也是有乱码的，怎么解决这个响应的乱码？

~~~java
response.setContentType("text/html;charset=UTF-8");
~~~

- 在Tomcat10之后，包括10在内，响应中文的时候就不在出现乱码问题了。以上代码就不需要设置UTF-8了。

  

get请求乱码问题怎么解决？

注意：get请求发送的时候，数据是在`请求行上提交`的，不是在请求体当中提交的(GET    /context/img/123.jpeg    Http1.1)

 方案：修改`CATALINA_HOME/conf/server.xml`配置文件

~~~java
<Connector URIEncoding="UTF-8" />
~~~

注意：`从Tomcat8之后`，**URIEncoding的默认值就是UTF-8**，所以GET请求也没有乱码问题了。



## 3.0 获取应用的根路径

HttpServletRequest

**【ServletContext中也有获取虚拟目录的方法】**

~~~java
String contextPath = request.getContextPath();
~~~



## 4.0 获取请求方式【HttpRequest】

~~~java
String method = request.getMethod();
~~~



## 5.0 获取请求的URI【带项目名】

~~~java
String uri = request.getRequestURI(); 
~~~



## 6.0 获取servlet path【不带项目名】

~~~java
String servletPath = request.getServletPath(); 
~~~







# Servlet注解，简化配置

1.0 分析oa项目中的web.xml文件

- 现在只是一个单表的CRUD，没有复杂的业务逻辑，很简单的一丢丢功能。web.xml文件中就有如此多的配置信息。
- 如果采用这种方式，对于一个大的项目来说，这样的话web.xml文件会非常庞大，有可能最终会达到几十兆。
- 在web.xml文件中进行servlet信息的配置，显然开发效率比较低，每一个都需要配置一下。
- 而且在web.xml文件中的配置是很少被修改的，所以这种配置信息能不能直接写到java类当中呢？可以的。 



2.0 Servlet3.0版本之后，推出了各种**Servlet**`基于注解式开发`。优点是什么？

- **开发效率高**，不需要编写**大量的配置信息**。直接在java类上使用注解进行标注。
- `web.xml文件体积变小`了。



3.0 并不是说注解有了之后，web.xml文件就不需要了：

- 有一些需要变化的信息，还是要配置到web.xml文件中。。
- **一些不会经常变化修改的配置建议使用注解。一些可能会被修改的建议写到配置文件**中。
- 如**过滤器**，可以通过调整`filter-mapping`的在`web.xml`中的先后顺序，**动态**   调整过滤器链中的过滤器顺序



4.0 我们的第一个注解：

~~~java
jakarta.servlet.annotation.WebServlet
~~~



5.0 在Servlet类上使用：@WebServlet，WebServlet注解中有哪些   **属性**  呢？

- `name属性`：用来指定Servlet的名字。等同于：\<servlet-name >
- `urlPatterns属性`：用来指定Servlet的映射路径。可以指定多个字符串。\<url-pattern>
- `loadOnStartUp属性`：用来指定在  ` 服务器启动阶段  `**是否加载该Servlet**。等同于：\< load-on-startup>
- `value属性`：当注解的属性名是value的时候，使用注解的时候，value属性名是可以省略的。

注意：不是必须将所有属性都写上，只需要提供需要的。（**需要什么用什么**）

注意：urlPatterns属性是一个数组，如果**数组中只有一个元素**，使用该注解的时候，**属性值的大括号可以省略**。

​	**当注解中有键名为：value时，且该注解的其他属性有默认值时，可以只写值**

- 注解对象的使用格式：

  - @注解名称(属性名=属性值, 属性名=属性值, 属性名=属性值....)



# 反射获取注解

- WelcomeServlet 

~~~java
//@WebServlet(urlPatterns = {"/welcome1", "/welcome2"})

//大括号可以省略。
//@WebServlet(urlPatterns = "/welcome")

// 这个value属性和urlPatterns属性一致，都是用来指定Servlet的映射路径的。
//@WebServlet(value = {"/welcome1", "/welcome2"})
//大括号可以省略。
//@WebServlet(value = "/welcome1")





// 如果注解的属性名是value的话，属性名也是可以省略的。
//@WebServlet({"/wel", "/abc", "/def"})
@WebServlet("/wel")
public class WelcomeServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.print("欢迎学习Servlet。");
    }
}

~~~

- **ReflectAnnotation**

~~~java
public class ReflectAnnotation {
    public static void main(String[] args) throws Exception{

        // 使用反射机制将类上面的注解进行解析。
        // 获取类Class对象
        Class<?> welcomeServletClass = Class.forName("com.bjpowernode.javaweb.servlet.WelcomeServlet");

        // 获取这个类上面的注解对象
        // 先判断这个类上面有没有这个注解对象，如果有这个注解对象，就获取该注解对象。
        //boolean annotationPresent = welcomeServletClass.isAnnotationPresent(WebServlet.class);
        //System.out.println(annotationPresent);
        if (welcomeServletClass.isAnnotationPresent(WebServlet.class)) {
            // 获取这个类上面的注解对象
            WebServlet webServletAnnotation = welcomeServletClass.getAnnotation(WebServlet.class);
            // 获取注解的value属性值。
            String[] value = webServletAnnotation.value();
            for (int i = 0; i < value.length; i++) {
                System.out.println(value[i]);
            }
        }
    }
}
~~~



# 使用模板方法设计模式优化oa项目

上面的    **注解解决了配置文件的问题     **。但是现在的oa项目仍然存在一个   比较臃肿   的问题。

- 一个单标的CRUD，就写了6个Servlet。如果一个复杂的业务系统，这种开发方式，显然会导致类爆炸。（`类的数量太大`）
- 怎么解决这个类爆炸问题？可以   `使用模板方法设计模式 ` 。

~~~java
// 模板类
@WebServlet({"/dept/list", "/dept/save", "/dept/edit", "/dept/detail", "/dept/delete", "/dept/modify"})
// 模糊匹配
// 只要请求路径是以"/dept"开始的，都走这个Servlet。
//@WebServlet("/dept/*")   尝试 httpservletRequest.getURI()
public class DeptServlet extends HttpServlet {

    // 模板方法
    // 重写service方法（并没有重写doGet或者doPost）
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取servlet path
        String servletPath = request.getServletPath();
        if("/dept/list".equals(servletPath)){
            doList(request, response);
        } else if("/dept/save".equals(servletPath)){
            doSave(request, response);
        } else if("/dept/edit".equals(servletPath)){
            doEdit(request, response);
        } else if("/dept/detail".equals(servletPath)){
            doDetail(request, response);
        } else if("/dept/delete".equals(servletPath)){
            doDel(request, response);
        } else if("/dept/modify".equals(servletPath)){
            doModify(request, response);
        }
    }

    private void doList(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取应用的根路径
        String contextPath = request.getContextPath();

        // 设置响应的内容类型以及字符集。防止中文乱码问题。(响应体)
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();

        out.print("<!DOCTYPE html>");
        out.print("<html>");
        out.print("	<head>");
        out.print("		<meta charset='utf-8'>");
        out.print("		<title>部门列表页面</title>");

        out.print("<script type='text/javascript'>");
        out.print("    function del(dno){");
        out.print("        if(window.confirm('亲，删了不可恢复哦！')){");
        out.print("            document.location.href = '"+contextPath+"/dept/delete?deptno=' + dno");
        out.print("        }");
        out.print("    }");
        out.print("</script>");

        out.print("	</head>");
        out.print("	<body>");
        out.print("		<h1 align='center'>部门列表</h1>");
        out.print("		<hr >");
        out.print("		<table border='1px' align='center' width='50%'>");
        out.print("			<tr>");
        out.print("				<th>序号</th>");
        out.print("				<th>部门编号</th>");
        out.print("				<th>部门名称</th>");
        out.print("				<th>操作</th>");
        out.print("			</tr>");
        /*上面一部分是死的*/

        // 连接数据库，查询所有的部门
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            // 获取连接
            conn = DBUtil.getConnection();
            // 获取预编译的数据库操作对象
            String sql = "select deptno as a,dname,loc from dept";
            ps = conn.prepareStatement(sql);
            // 执行SQL语句
            rs = ps.executeQuery();
            // 处理结果集
            int i = 0;
            while(rs.next()){
                String deptno = rs.getString("a");
                String dname = rs.getString("dname");
                String loc = rs.getString("loc");

                out.print("			<tr>");
                out.print("				<td>"+(++i)+"</td>");
                out.print("				<td>"+deptno+"</td>");
                out.print("				<td>"+dname+"</td>");
                out.print("				<td>");
                out.print("					<a href='javascript:void(0)' onclick='del("+deptno+")'>删除</a>");
                out.print("					<a href='"+contextPath+"/dept/edit?deptno="+deptno+"'>修改</a>");
                out.print("					<a href='"+contextPath+"/dept/detail?fdsafdsas="+deptno+"'>详情</a>");
                out.print("				</td>");
                out.print("			</tr>");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 释放资源
            DBUtil.close(conn, ps, rs);
        }

        /*下面一部分是死的*/
        out.print("		</table>");
        out.print("		<hr >");
        out.print("		<a href='"+contextPath+"/add.html'>新增部门</a>");
        out.print("	</body>");
        out.print("</html>");
    }

    private void doSave(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取部门的信息
        // 注意乱码问题（Tomcat10不会出现这个问题）
        request.setCharacterEncoding("UTF-8");
        String deptno = request.getParameter("deptno");
        String dname = request.getParameter("dname");
        String loc = request.getParameter("loc");

        // 连接数据库执行insert语句
        Connection conn = null;
        PreparedStatement ps = null;
        int count = 0;
        try {
            conn = DBUtil.getConnection();
            String sql = "insert into dept(deptno, dname, loc) values(?,?,?)";
            ps = conn.prepareStatement(sql);
            ps.setString(1, deptno);
            ps.setString(2, dname);
            ps.setString(3, loc);
            count = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps, null);
        }

        if (count == 1) {
            // 保存成功跳转到列表页面
            // 转发是一次请求。
            //request.getRequestDispatcher("/dept/list").forward(request, response);

            // 这里最好使用重定向（浏览器会发一次全新的请求。）
            // 浏览器在地址栏上发送请求，这个请求是get请求。
            response.sendRedirect(request.getContextPath() + "/dept/list");

        }else{
            // 保存失败跳转到错误页面
            //request.getRequestDispatcher("/error.html").forward(request, response);

            // 这里也建议使用重定向。
            response.sendRedirect(request.getContextPath() + "/error.html");
        }
    }

    private void doEdit(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取应用的根路径。
        String contextPath = request.getContextPath();

        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        out.print("<!DOCTYPE html>");
        out.print("<html>");
        out.print("	<head>");
        out.print("		<meta charset='utf-8'>");
        out.print("		<title>修改部门</title>");
        out.print("	</head>");
        out.print("	<body>");
        out.print("		<h1>修改部门</h1>");
        out.print("		<hr >");
        out.print("		<form action='"+contextPath+"/dept/modify' method='post'>");

        // 获取部门编号
        String deptno = request.getParameter("deptno");
        // 连接数据库，根据部门编号查询部门的信息。
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            conn = DBUtil.getConnection();
            String sql = "select dname, loc as location from dept where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, deptno);
            rs = ps.executeQuery();
            // 这个结果集中只有一条记录。
            if(rs.next()){
                String dname = rs.getString("dname");
                String location = rs.getString("location"); // 参数"location"是sql语句查询结果列的列名。
                // 输出动态网页。
                out.print("                部门编号<input type='text' name='deptno' value='"+deptno+"' readonly /><br>");
                out.print("                部门名称<input type='text' name='dname' value='"+dname+"'/><br>");
                out.print("                部门位置<input type='text' name='loc' value='"+location+"'/><br>");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps, rs);
        }

        out.print("			<input type='submit' value='修改'/><br>");
        out.print("		</form>");
        out.print("	</body>");
        out.print("</html>");
    }

    private void doDetail(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();

        out.print("<!DOCTYPE html>");
        out.print("<html>");
        out.print("	<head>");
        out.print("		<meta charset='utf-8'>");
        out.print("		<title>部门详情</title>");
        out.print("	</head>");
        out.print("	<body>");
        out.print("		<h1>部门详情</h1>");
        out.print("		<hr >");

        // 获取部门编号
        // /oa/dept/detail?fdsafdsas=30
        // 虽然是提交的30，但是服务器获取的是"30"这个字符串。
        String deptno = request.getParameter("fdsafdsas");

        // 连接数据库，根据部门编号查询部门信息。
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            conn = DBUtil.getConnection();
            String sql = "select dname,loc from dept where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, deptno);
            rs = ps.executeQuery();
            // 这个结果集一定只有一条记录。
            if(rs.next()){
                String dname = rs.getString("dname");
                String loc = rs.getString("loc");

                out.print("部门编号："+deptno+" <br>");
                out.print("部门名称："+dname+"<br>");
                out.print("部门位置："+loc+"<br>");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps, rs);
        }

        out.print("		<input type='button' value='后退' onclick='window.history.back()'/>");
        out.print("	</body>");
        out.print("</html>");
    }

    private void doDel(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 根据部门编号，删除部门。
        // 获取部门编号
        String deptno = request.getParameter("deptno");
        // 连接数据库删除数据
        Connection conn = null;
        PreparedStatement ps = null;
        int count = 0;
        try {
            conn = DBUtil.getConnection();
            // 开启事务（自动提交机制关闭）
            conn.setAutoCommit(false);
            String sql = "delete from dept where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, deptno);
            // 返回值是：影响了数据库表当中多少条记录。
            count = ps.executeUpdate();
            // 事务提交
            conn.commit();
        } catch (SQLException e) {
            // 遇到异常要回滚
            if (conn != null) {
                try {
                    conn.rollback();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps, null);
        }

        // 判断删除成功了还是失败了。
        if (count == 1) {
            //删除成功
            //仍然跳转到部门列表页面
            //部门列表页面的显示需要执行另一个Servlet。怎么办？转发。
            //request.getRequestDispatcher("/dept/list").forward(request, response);
            response.sendRedirect(request.getContextPath() + "/dept/list");
        }else{
            // 删除失败
            //request.getRequestDispatcher("/error.html").forward(request, response);
            response.sendRedirect(request.getContextPath() + "/error.html");
        }
    }

    private void doModify(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 解决请求体的中文乱码问题。
        request.setCharacterEncoding("UTF-8");

        // 获取表单中的数据
        String deptno = request.getParameter("deptno");
        String dname = request.getParameter("dname");
        String loc = request.getParameter("loc");
        // 连接数据库执行更新语句
        Connection conn = null;
        PreparedStatement ps = null;
        int count = 0;
        try {
            conn = DBUtil.getConnection();
            String sql = "update dept set dname = ?, loc = ? where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, dname);
            ps.setString(2, loc);
            ps.setString(3, deptno);
            count = ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps, null);
        }

        if (count == 1) {
            // 更新成功
            // 跳转到部门列表页面（部门列表页面是通过Java程序动态生成的，所以还需要再次执行另一个Servlet）
            //request.getRequestDispatcher("/dept/list").forward(request, response);

            response.sendRedirect(request.getContextPath() + "/dept/list");
        }else{
            // 更新失败
            //request.getRequestDispatcher("/error.html").forward(request, response);
            response.sendRedirect(request.getContextPath() + "/error.html");
        }
    }
}
~~~





# 分析使用纯粹Servlet开发web应用的缺陷

1.0 在Servlet当中编写HTML/CSS/JavaScript等前端代码。存在什么问题？

- java程序中编写前端代码，**编写难度大。麻烦**。
- 显然程序的   **耦合度非常高**。
- **代码非常不美观**。
- **维护成本太高**。（非常难于维护）
  - 修改小小的一个前端代码，只要有改动，就需要重新编译java代码，生成新的class文件，打一个新的war包，重新发布。



2.0 思考一下，如果是你的话，你准备怎么解决这个问题？

- 上面的那个Servlet（Java程序）能不能不写了，让机器自动生成。我们程序员只需要写这个Servlet程序中的“前端的那段代码”
  - 然后**让机器将我们写的“`前端代码`”   自动翻译生成    “`Servlet这种java程序`**”。

- 然后机器再自动将 `  “java”程序   编译生成   "class"文件`   。然后再   `使用JVM调用这个class中的方法`。



# 关于B/S结构系统的会话机制（session机制）

## 1.0 什么是会话？

- 服务器端用于记录用户状态的对象；从会话的创建到会话的失效 整个过程叫做：一次会话。
- **会话在服务器端也有一个对应的java对象，这个java对象叫做：session**。
- 什么是一次请求：用户在浏览器上点击了一下，然后到页面停下来，可以粗略认为是一次请求。请求对应的服务器端的java对象是：request。
- **一个会话当中包含多次请求。（一次会话对应N次请求。）**

- 在java的servlet规范当中，session对应的类名：HttpSession（jarkata.servlet.http.HttpSession）
- session机制属于B/S结构的一部分。session机制实际上是一个规范。然后不同的语言对这种会话机制都有实现。
- session对象最主要的作用是：保存会话状态。
- 如用户登录成功了，这是一种登录成功的状态，你怎么把登录成功的状态一直保存下来呢？使用session对象可以保留会话状态。



## 2.0 为什么需要session对象来保存会话状态呢？

- 因为   **HTTP协议    是一种     无状态协议**。

- **什么是无状态**：

  **请求与响应的时候，B和S是连接的**

  **但是响应结束之后**，连接就断了

  `重新发送请求后`这样服务器就  `无法判别浏览器的状态`



## 3.0 为什么要这么做？HTTP协议为什么要设计成这样？

- 因为这样的**无状态协议，可以  `  降低服务器的压力`  **。**请求的瞬间是连接的，请求结束之后，连接断开，这样`服务器压力小`**。

- 只要B和S断开了，那么关闭浏览器这个动作，服务器知道吗？
  - 不知道。`服务器是不知道浏览器关闭的`。

- 张三打开一个浏览器A，李四打开一个浏览器B，访问服务器之后，在服务器端会生成：
  - 张三  `专属 ` 的  session对象
  - 李四  `专属`  的  session对象



## 4.0 设置Session超时时间

web.xml

~~~xml
    <!--session的超时时长是30分钟。-->
    <!--如果30分钟过去了，session对象仍然没有被访问，session对象会被销毁。-->
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>
~~~



## 5.0 为什么不使用request对象保存会话状态？为什么不使用ServletContext对象保存会话状态？

- request是一次请求一个对象。请求结束后，该对象也很快会消失，**无法保存状态**

- ServletContext对象是服务器启动的时候创建，服务器关闭的时候销毁，这个ServletContext对象只有一个。

- 但是ServletContext对象的域太大，用来存状态信息**比较浪费空间**；

  

注意：

request.setAttribute()存，request.getAttribute()取，ServletContext也有这个方法。Servletrequest是请求域。ServletContext是应用域。

request请求域（ServletRequest）、session会话域（**HttpSession**）、application域（ServletContext）

request < session < application



# 6.0 思考一下：session对象的实现原理

- HttpSession session = request.getSession();
- 这行代码很神奇。`张三访问的时候获取的session对象就是张三`的。`李四访问的时候获取的session对象就是李四的。`



## 7.0 **session的实现原理**：

- `JSESSIONID=xxxxxx` 这个是以   **`Cookie`**  的形式保存在    **`浏览器的内存`**   中的。**浏览器只要关闭**。这个**cookie就没有**了。
- **`session列表`**  是一个Map，map的    **key  是  sessionid**，map的   **value  是  session对象**。
- 用户第一次请求，**服务器**生成session对象，同时生成id，**将id发送给浏览器**。
- 用户第二次请求，**自动 **  将   `浏览器内存中的id ` 发送给服务器，`服务器根据id查找session对象`。
- `关闭浏览器`，**内存消失**，**cookie消失**，`sessionid消失`，会话  **等同于**  结束。

---

![session对象](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261720825.png)

---



## 8.0 Cookie禁用了，session还能找到吗？

- cookie禁用是什么意思？

  服务器正常发送cookie给浏览器，但是浏览器不要了。拒收了。并不是服务器不发了。

- 找不到了。因为浏览器中没有关联该路径的cookies，**每次请求 **  都不会   `携带jsessionid`；所以每一次请求都会获取到新的session对象。



## 9.0 **cookie禁用了，session机制还能实现**吗？

- **可以。需要    使用   URL重写机制  **。

  ~~~
  http://localhost:8080/servlet12/test/session;jsessionid=19D1C99560DCBF84839FA43D58F56E16
  ~~~

- URL重写机制会提高开发者的成本。开发人员在编写任何请求路径的时候，后面都要添加一个sessionid，给开发带来了很大的难度，很大的成本。

- 所以`大部分的网站`都是这样设计的：**你要是禁用cookie，你就别用了**。



## 10  总结一下到目前位置我们所了解的域对象：

- request（对应的类名：`ServletRequest`）

  - 请求域（**请求级别的**）

- session（对应的类名：`HttpSession`）

  - 会话域（**用户级别的**）

- application（对应的类名：`ServletContext`）

  - 应用域（**项目级别的**，所有servlet共享的）

- 这三个域对象的大小关系

  > request < session < application

- 他们三个  **域对象**  都有以下三个公共的方法：

  - setAttribute（向域当中绑定数据）
  - getAttribute（从域当中获取数据）
  - removeAttribute（删除域当中的数据）

- **使用原则：尽量使用小的域**



- session掌握之后，我们怎么解决oa项目中的登录问题，怎么能让登录起作用。

  - **登录成功之后，可以将`用户的登录信息存储到session`当中。**
  - **也就是说`session中如果有用户的信息 `   就代表  `  用户登录成功`了。session中没有用户信息，表示用户没有登录过。则跳转到登录页面**。



**Servlet九大内置对象：**

---

![image-20220721232035162](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261720188.png)

---



## 11.0 销毁session对象：

> session.invalidate();





# Cookie

## 1.0 session的实现原理中，每一个session对象都会关联一个sessionid，例如：

- `JSESSIONID=41C481F0224664BDB28E95081D23D5B8`

- 以上的这个键值对数据其实就是cookie对象。

- **对于session关联的cookie来说，这个cookie是被保存在浏览器的“运行内存”当中**。

- 只要浏览器不关闭，用户再次   **发送请求的时候**   ，会自动将运行内存中的cookie发送给服务器。【该请求包含cookies的关联路径，才会带此cookie】

  例如，这个Cookie: JSESSIONID=41C481F0224664BDB28E95081D23D5B8就会再次发送给服务器。

  服务器就是根据41C481F0224664BDB28E95081D23D5B8这个值来找到对应的session对象的。

  

## 2.0 cookie怎么生成？

- `cookies`是由  ` 服务器生成`  的，**返回给浏览器的**



## 3.0 cookie保存在什么地方？

- cookie最终是保存在浏览器客户端上的。

  - 可以保存在    **运行内存**中。【浏览器只要关闭cookie就消失了` setMaxAge=-1`】如：jsessionid  
  - 也可以保存在   **硬盘文件**中。（永久保存）`setMaxAge=1` ; ` setMaxAge=0`（销毁该cookies对象）



## 4.0 cookie有啥用呢？

- **cookie    和    session  机制   其实都是为了保存会话的状态·**。

- **`cookie ` 是将   会话的状态   保存在浏览器客户端上**。（cookie数据存储在浏览器客户端上的）

- **`session`   是   将会话的状态   保存在服务器端上**。（session对象是存储在服务器上）



## 5.0 为什么要有cookie和session机制呢？

- 因为**HTTP协议是无状态** 
- 浏览器与服务器只在请求与响应中保持连接，等待服务器**响应后**就断开连接；
- 服务器是不清楚浏览器的状态的



## 6.0 cookie的经典案例

1.0 京东商城，在未登录的情况下，向购物车中放几件商品。然后关闭商城，再次打开浏览器，访问京东商城的时候，购物车中的商品还在，这是怎么做的？我没有登录，为什么购物车中还有商品呢？

- 将购物车中的商品编号放到cookie当中，cookie保存在硬盘文件当中。这样即使关闭浏览器。硬盘上的cookie还在。
- 下一次再打开京东商城的时候，查看购物车的时候，会自动读取本地硬盘中存储的cookie，拿到商品编号，动态展示购物车中的商品。
  - 京东存储购物车中商品的cookie可能是这样的：productIds=xxxxx,yyyy,zzz,kkkk
  - 注意：**cookie如果清除掉，购物车中的商品就消失**了。



2.0     126邮箱中有一个功能：十天内免登录

- 这个功能也是需要cookie来实现的。
- 怎么实现的呢？
  - 用户输入正确的用户名和密码，并且同时选择十天内免登录。
  - 登录成功后。浏览器客户端会保存一个cookie，这个**cookie中保存了用户名和密码等信息**，这个cookie是保存在硬盘文件当中的，十天有效。
  - `在十天内 ` 用户再次访问126的时候，**浏览器自动提交126的关联的cookie给服务器**
  - 服务器接收到cookie之后，获取用户名和密码，验证，通过之后，自动登录成功。



## 7.0 怎么让cookie失效？

- 十天过后自动失效。
- 或者改密码。
- 或者在客户端浏览器上清除cookie。

注意：

​	**cookie机制    和session机制   其实都不属于java中的机制**  

​	实际上**cookie机制和session机制都是   HTTP协议   的一部分**。

​	只要是你是做**web开发**，不管是什么编程语言，`cookie和session机制都是需要的`。

​	`HTTP协议中规定`：**任何一个   cookie   都是由   name  和   value组成的**

​	 name和value都是**字符串类型**的。



## 8.0 在java的servlet中，对cookie提供了哪些支持呢？

- 提供了一个Cookie类来专门表示cookie数据。

> jakarta.servlet.http.Cookie; 
>
> javax.servlet.servlet



- java程序怎么把cookie数据发送给浏览器呢？

> Cookie cookie = new Cookie("text","zhangsi");
>
> response.addCookie(cookie);





- **在HTTP协议中是这样规定的：当浏览器发送请求的时候，会自动携带该path下的cookie数据给服务器。（URL）**



## 9.0 关于cookie的有效时间

- 怎么用java设置cookie的有效时间

  ~~~java
  cookie.setMaxAge(60 * 60); //设置cookie在一小时之后失效。【默认秒】
  ~~~

- **没有设置有效时间：`默认`保存在浏览器的运行内存中，浏览器关闭则cookie消失**。

- **只要设置cookie的有效时间 > 0，这个cookie一定会存储到硬盘文件当中。**

- 设置cookie的有效时间 **= 0** 呢？

  - cookie被删除，**同名cookie被删除**。

- 设置cookie的有效时间 **< 0** 呢？

  - **保存在运行内存中**。和不设置一样。



## 10 关于cookie的path，**cookie关联的路径**：

- 假设现在发送的请求路径是“http://localhost:8080/servlet13/cookie/generate”生成的cookie，如果cookie没有设置path，默认的path是什么？

  > 默认的path是：http://localhost:8080/servlet13/cookie 以及它的子路径。

  

  - 也就是说，以后只要浏览器的请求路径是http://localhost:8080/servlet13/cookie 这个路径以及这个路径下的子路径，cookie都会被发送到服务器。

- 手动设置cookie的path

  ~~~java
  // 表示只要是这个servlet13项目的请求路径，都会提交这个cookie给服务器。
  cookie.setPath(“/servlet13”);
  ~~~

- 浏览器发送cookie给服务器了，服务器中的java程序怎么接收？

~~~java
//如果浏览器没有向服务器发送cookies 就nulll 
Cookie[] cookies = request.getCookies(); // 这个方法可能返回null
if(cookies != null){
    for(Cookie cookie : cookies){
        // 获取cookie的name
        String name = cookie.getName();
        // 获取cookie的value
        String value = cookie.getValue();
    }
}
~~~



## 11 使用cookie实现一下十天内免登录功能

- 先实现登录功能 
  - 登录成功
    - 跳转到部门列表页面
  - 登录失败
    - 跳转到登录失败页面
- 修改前端页面
  - 在登录页面给一个复选框，复选框后面给一句话：十天内免登录。
  - 用户选择了复选框：表示要支持十天内免登录。
  - 用户没有选择复选框：表示用户不想使用十天内免登录功能。
- 修改Servlet中的login方法
  - 如果用户登录成功了，并且用户登录时选择了十天内免登录功能，这个时候应该在Servlet的login方法中创建cookie
  - 用来**存储用户名和密码**，并且设置关联路径，设置有效期，将cookie响应给浏览器。（浏览器将其自动保存在硬盘文件当中10天）
- 用户再次访问该网站的时候，访问这个网站的首页的时候，有两个走向:
  - 要么跳转到部门列表页面
  - 要么跳转到登录页面
  - 以上分别有两个走向，这显然是需要编写java程序进行控制的。

~~~java
@WebServlet("/welcome")
public class WelcomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取cookie
        // 这个Cookie[]数组可能是null，如果不是null，数组的长度一定是大于0的。
        Cookie[] cookies = request.getCookies();
        String username = null;
        String password = null;
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                String name = cookie.getName();
                if("username".equals(name)){
                    username = cookie.getValue();
                }else if("password".equals(name)){
                    password = cookie.getValue();
                }
            }
        }

        // 要在这里使用username和password变量
        if(username != null && password != null){
            // 验证用户名和密码是否正确
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;
            boolean success = false;
            try {
                conn = DBUtil.getConnection();
                String sql = "select * from t_user where username = ? and password = ?";
                ps = conn.prepareStatement(sql);
                ps.setString(1,username);
                ps.setString(2,password);
                rs = ps.executeQuery();
                if (rs.next()) {
                    // 登录成功
                    success = true;
                }
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {
                DBUtil.close(conn, ps, rs);
            }

            if (success) {
                // 获取session
                HttpSession session = request.getSession();
                //session.setAttribute("username", username);

                User user = new User(username, password);
                session.setAttribute("user", user);

                // 正确，表示登录成功
                response.sendRedirect(request.getContextPath() + "/dept/list");
            }else{
                // 错误，表示登录失败
                response.sendRedirect(request.getContextPath() + "/index.jsp");
            }
        }else{
            // 跳转到登录页面
            response.sendRedirect(request.getContextPath() + "/index.jsp");
        }
    }
}
~~~



# JSP

## 1.0 前提引入

- 在   `WEB-INF目录之外 `  创建一个index.jsp文件，然后这个文件中没有任何内容。

- 实际上    **访问以上的这个：index.jsp，  底层执行   的是：`index_jsp.class `这个java程序**。
- 这个`index.jsp`会被`tomcat`    **翻译 生成   **`index_jsp.java`文件，然后tomcat服务器又会将 ` index_jsp.java `   **编译 生成**   `index_jsp.class`文件
- 访问`index.jsp`，实际上执行的是`index_jsp.class`中的方法。



## 2.0 JSP实际上就是一个Servlet。

- **index.jsp访问的时候，会自动翻译生成index_jsp.java**，会自动编译生成index_jsp.class，那么index_jsp 这就是一个类。
- **index_jsp 类继承 HttpJspBase，而HttpJspBase类继承的是HttpServlet。所以index_jsp类就是一个Servlet类**。
- jsp的生命周期和Servlet的生命周期完全相同。完全就是一个东西。没有任何区别。
- jsp和servlet一样，**都是单例的。（假单例**。）





## 3.0 jsp文件第一次访问的时候是比较慢的，为什么？

为什么大部分的运维人员在给客户演示项目的时候，为什么提前先把所有的jsp文件先访问一遍。

- 第一次比较麻烦：
  - 要把jsp文件**翻译生成java源文件**
  - java源文件要**编译生成class字节码文件**
  - 然后通过class去**创建servlet对象**
  - 然后调用servlet对象的**init方法**
  - 最后调用servlet对象的service方法。
- 第二次就比较快了，为什么？
  - 因为**第二次直接调用单例servlet对象的service方法**即可。



## 4.0 JSP是什么？

- JSP是java程序。（JSP本质还是一个Servlet）

- JSP是：JavaServer Pages的缩写。（**基于Java语言实现的服务器端的页面**。）

- **Servlet是JavaEE的13个子规范之一，那么JSP也是JavaEE的13个子规范之一**。

- JSP是一套规范。所有的web容器【web服务器】都是遵循这套规范的，都是按照这套规范进行的“翻译”

- 每一个web容器【web**服务器】都会内置一个JSP翻译引擎**。

  - 对JSP进行错误调试的时候，还是要直接打开JSP文件对应的java文件，检查java代码。

    开发JSP的最高境界：

    ​		眼前是JSP代码，但是脑袋中呈现的是java代码。



## 5.0 JSP既然本质上是一个Servlet，那么JSP和Servlet到底有什么区别呢？

- 职责不同：
  - S**ervlet的职责是什么：收集数据。（Servlet的强项是逻辑处理，业务处理，然后链接数据库，获取/收集数据。）**
  - **JSP的职责是什么：展示数据。（JSP的强项是做数据的展示）**



## 6.0 JSP的基础语法

- 在jsp文件中**直接编写文字**，都会自动被翻译到哪里？
  - 翻译到servlet类的service方法的out.write("翻译到这里")，直接翻译到双引号里，**被java程序当做普通字符串打印输出到浏览器**。
  - 在**JSP中编写的HTML CSS JS代码**，这些代码**对于JSP来说只是一个普通的字符串**。
  - 但是JSP把这个普通的字符串一旦输出到浏览器，**浏览器就会对HTML CSS JS进行解释执行。展现一个效果**。



- **JSP的page指令**（这个指令后面再详细说，这里先解决一下中文乱码问题），解决**响应时的中文乱码**问题：

  - 通过page指令来设置响应的内容类型，在内容类型的最后面添加：charset=UTF-8

    ~~~xml
    <!--表示响应的内容类型是text/html，采用的字符集UTF-8-->
    <%@page contentType="text/html;charset=UTF-8"%>
        
     <!--导包-->
    <%@page import="java.util.List,java.util.ArrayList"%>
    ~~~

    

- 怎么在JSP中编写Java程序：

  1. <**% java语句; %>**     代码块

  - **在这个符号当中编写的被视为java程序，被翻译到Servlet类的service方法内部。**
  - **这里你要细心点，你要思考，在<% %>这个符号里面写java代码的时候，你要时时刻刻的记住你正在“方法体”当中写代码，方法体中可以写什么，不可以写什么，你心里是否明白呢？**
  - **在service方法当中编写的代码是有顺序的，方法体当中的代码要遵循自上而下的顺序依次逐行执行。**
  - **service方法当中不能写静态代码块，不能写方法，不能定义成员变量。。。。。。**
  - **在同一个JSP当中 <%%> 这个符号可以出现多个。**

  

  2. **<%! %>**    声明

  - 在这个符号当中编写的java程序会自动**翻译到service方法之外**。
  - **可以是方法【静态成员方法、实例成员方法】，成员变量、静态代码块**
  - 这个语法很少用，为什么？不建议使用，因为在service方法外面写静态变量和实例变量，都会存在线程安全问题
  - 因为JSP就是servlet，servlet是单例的，多线程并发的环境下，这个静态变量和实例变量一旦有修改操作，必然会**存在线程安全问题**。

  

  3. **<%= %>** JSP的输出语句

  - 怎么向浏览器上输出一个java变量。

  ~~~jsp
  <% 
  String name = “jack”; 
  out.write("name = " + name); 
  %>
  ~~~

  - 注意：上述中的**out是JSP的九大内置对象之一**。可直接使用。但必须**只能在service方法内部**使用。【因为这九个货都是service方法的局部变量】

  - 如果向浏览器上输出的内容中没有“java代码”，例如输出的字符串是一个固定的字符串，可以直接在jsp中编写，不需要写到<%%> 这里。

  - 如果输出的内容中含有“java代码”，这个时候可以使用以下语法格式：

    - <**%= %> 注意：在=的后面编写要输出的内容**。

    

    

    <%= %> 这个符号会被翻译到哪里？最终翻译成什么？ 

    - **翻译成了这个java代码：   out.print();**  里面时表达式，不带分号
    - **翻译到service方法当中了。**

    - 什么时候使用<%=%> 输出呢？输出的内容中含有java的变量，输出的内容是一个动态的内容，不是一个死的字符串
    - 如果输出的是一个固定的字符串，直接在JSP文件中编写即可。



4. 在JSP中如何编写JSP的专业注释

~~~jsp
<%--JSP的专业注释，不会被翻译到java源代码当中。--%>

<!--这种注释属于HTML的注释，这个注释信息仍然会被翻译到java源代码当中，不建议。-->
~~~



5. JSP基础语法总结：

- JSP中直接编写普通字符串
  - 翻译到service方法的out.write("这里")
- <%%> jsp代码块
  - 翻译到service方法体内部，里面是一条一条的java语句。
- <%! %> jsp声明
  - 翻译到service方法之外。
- <%= %> jsp输出语句
  - 翻译到service方法体内部，翻译为：out.print();
- <%@page  contentType="text/html;charset=UTF-8"%>
  - page指令，通过contentType属性用来设置响应的内容类型。





## 7.0 使用Servlet + JSP完成oa项目的改造。

- 使用Servlet处理业务，收集数据。 使用JSP展示数据。
- 将之前原型中的html文件，全部修改为jsp，然后在jsp文件头部添加page指令（指定contentType防止中文乱码），将所有的JSP直接拷贝到web目录下。
- 完成所有页面的正常流转。（页面仍然能够正常的跳转。修改超链接的请求路径。）

  - **<%=request.getContextPath() %>  在JSP中动态的获取应用的根路径。**
- Servlet中连接数据库，查询所有的部门，遍历结果集。
  - 遍历结果集的过程中，取出部门编号、部门名、位置等信息，封装成java对象。
  - 将java对象存放到List集合中。
  - 将List集合存储到request域当中。
  - 转发forward到jsp。
- 在JSP中：
  - 从request域当中取出List集合。
  - 遍历List集合，取出每个部门对象。动态生成tr。【表格数据】



思考一个问题：如果我只用JSP这一个技术，能不能开发web应用？

- 当然**可以使用JSP来完成所有的功能**。因为JSP就是Servlet，在JSP的<%%>里面写的代码就是在service方法当中的
- 所以在<%%>当中完全可以编写JDBC代码，连接数据库，查询数据，也可以在这个方法当中编写业务逻辑代码，处理业务，都是可以的
- 所以使用单独的JSP开发web应用完全没问题。
- 虽然JSP一个技术就可以完成web应用，但是不建议，还是建议采用servlet + jsp的方式进行开发。这样都能将各自的优点发挥出来。
- JSP就是做数据展示。Servlet就是做数据的收集。（JSP中编写的Java代码越少越好。）一定要职责分明。



## 8.0 JSP文件的扩展名必须是xxx.jsp吗？

- jsp文件的扩展名是可以配置的。不是固定的。

- 在**CATALINA_HOME/conf/web.xml，在这个文件当中配置jsp文件的扩展名**。

- ```xml
  <servlet-mapping>
      <servlet-name>jsp</servlet-name>
      <url-pattern>*.jsp</url-pattern>
      <url-pattern>*.jspx</url-pattern>
  </servlet-mapping>
  ```

- **xxx.jsp文件对于小猫咪来说，只是一个普通的文本文件**

- **web容器会将xxx.jsp文件最终生成java程序，最终调用的是java对象相关的方法，真正执行的时候，和jsp文件就没有关系了**。



小窍门：JSP如果看不懂，建议把jsp翻译成java代码，就能看懂了。



- 同学问：包名bean是什么意思？

  - javabean（java的logo是一杯冒着热气的咖啡。javabean被翻译为：咖啡豆）
  - java是一杯咖啡，咖啡又是由一粒一粒的咖啡豆研磨而成。
  - 整个java程序中有很多bean的存在。由很多bean组成。

  

  - 什么是javabean？实际上javabean你可以理解为符合某种规范的java类，比如：
    - 有无参数构造方法
    - 属性私有化
    - 对外提供公开的set和get方法
    - 实现java.io.Serializable接口
    - 重写toString
    - 重写hashCode+equals
    - ....
  - **javabean其实就是java中的实体类。负责数据的封装**。
  - **由于javabean符合javabean规范，具有更强的通用性**。



## 9.0 JSP的指令

1.0 指令的作用：指导JSP的翻译引擎如何工作（**指导当前的JSP翻译引擎如何翻译JSP文件**。）



2.0 指令包括哪些呢**？【三大指令】**

- include指令：包含指令，在JSP中完成静态包含，很少用了。（这里不讲）
- taglib指令：引入标签库的指令。这个到JJSTL标签库的时候再学习。现在先不管。
- page指令：目前重点学习一个page指令。



3.0 指令的使用语法是什么？

~~~jsp
<%@指令名  属性名=属性值  属性名=属性值  属性名=属性值....%>
~~~



4.0 关于page指令当中都有哪些常用的属性呢？

- session

~~~jsp
<%@page session="true|false" %>

<%--
    1. true表示启用JSP的内置对象session，表示一定启动session对象。没有session对象会创建。
    2. 如果没有设置，默认值就是session="true"
    3. session="false" 表示不启动内置对象session。当前JSP页面中无法使用内置对象session。
--%>
~~~



- contentType

~~~jsp
<%@page contentType="text/json" %>
contentType属性用来设置响应的内容类型

但同时也可以设置字符集。
<%@page contentType="text/json;charset=UTF-8" %>
~~~



- pageEncoding

~~~jsp
<%@page pageEncoding="UTF-8" %>
pageEncoding="UTF-8" 表示设置响应时采用的字符集。
~~~



- import

~~~jsp
<%@page import="java.util.List, java.util.Date, java.util.ArrayList" %>
<%@page import="java.util.*" %>
import语句，导包。
~~~



- errorPage

~~~jsp
<%@page errorPage="/error.jsp" %>
当前页面出现异常之后，跳转到error.jsp页面。
errorPage属性用来指定出错之后的跳转位置。
~~~



- isErrorPage

~~~jsp
<%@page isErrorPage="true" %>

表示启用JSP九大内置对象之一：exception
默认值是false。表示不能使用jsp九大内置对象中的exception对象
~~~



## 10 JSP的九大内置对象

- jakarta.servlet.jsp.**PageContext** pageContext       页面作用域
- jakarta.servlet.http.**HttpServletRequest** request 请求作用域
- jakarta.servlet.http.**HttpSession** session  会话作用域
- jakarta.servlet.**ServletContext** application 应用作用域
  - pageContext < request < session < application 【四大域对象】
  - 以上四个作用域都有：setAttribute、getAttribute、removeAttribute方法。
  - 以上作用域的使用原则：尽可能使用小的域。

- java.lang.Throwable **exception**   

- jakarta.servlet.**ServletConfig** config

- java.lang.Object **page**  （其实是this，当前的servlet对象）

- jakarta.servlet.jsp.**JspWriter** out  （负责输出）
- jakarta.servlet.http.**HttpServletResponse** response （负责响应）

~~~java
pageContext   pageContext
request		  HttpServletRequest
session		  HttpSession
application	  ServletContext
    
out			  JspWriter
response	  HttpServletResponse
   
exception    
   
config        ServletConfig

page          this
~~~







# EL表达式

## 1.0 EL表达式是干什么用的？

- **Expression Language（表达式语言）**
- **EL表达式可以代替JSP中的java代码，让JSP文件中的程序看起来更加整洁，美观**。
- JSP中夹杂着各种java代码，例如<% java代码 %>、<%=%>等，导致JSP文件很混乱，不好看，不好维护。所以才有了后期的EL表达式。
- E**L表达式可以算是JSP语法的一部分**。EL表达式归属于JSP。



## 2.0 **EL表达式出现在JSP中主要目的**：

- **从某个作用域中取数据，然后将其转换成字符串，然后将其输出到浏览器**。这就是EL表达式的功效。三大功效：

  - 第一功效：从某个域中取数据。

    - 四个域：

      ~~~
      pageContext  request session application
      ~~~

      

  - 第二功效：将取出的数据转成字符串。

    - 如果是一个java对象，也会自动调用java对象的toString方法将其转换成字符串。【为null则不输出】

    

  - 第三功效：将字符串输出到浏览器。

    - 和这个一样：<%= %>，将其输出到浏览器。

    

## 3. EL表达式很好用，基本的语法格式：

- **${表达式}**



## 4.0 EL表达式的使用：

- ```jsp
  <%
  	// 创建User对象
  	User user = new User();
  	user.setUsername("jackson");
  	user.setPassword("1234");
  	user.setAge(50);
  
  	// 将User对象存储到某个域当中。一定要存，因为EL表达式只能从域对象中取数据。
  	// 数据是必须存储到四大范围之一的。
  	request.setAttribute("userObj", user);
  %>
  
  <%--使用EL表达式取--%>
  ${这里写的一定是存储到域对象当中时的name}
  要这样写：${userObj}
  等同于java代码：<%=request.getAttribute("userObj")%>
  你不要这样写：${"userObj"} 这样会输出“userObj
  ```

- ${userObj} 底层是怎么做的？
  	从域中取数据，取出user对象，然后调用user对象的toString方法，转换成字符串，输出到浏览器。



- 如果想输出对象的属性值，怎么办？

~~~java
	${userObj.username} 使用这个语法的前提是：User对象有getUsername()方法。
	${userObj.password} 使用这个语法的前提是：User对象有getPassword()方法。
	${userObj.age} 使用这个语法的前提是：User对象有getAge()方法。
	${userObj.email} 使用这个语法的前提是：User对象有getEmail()方法。
~~~

EL表达式中的. 这个语法，实际上调用了底层的getXxx()方法。

注意：如果没有对应的get方法，则出现异常。报500错误。

~~~java
${userObj.addr222.zipcode}
以上EL表达式对应的java代码：
user.getAddr222().getZipcode()
~~~



- 面试题：\${abc} 和 \${"abc"}的区别是什么？
   \${abc}表示从某个域中取出数据，并且被取的这个数据的name是"abc"，之前一定有这样的代码: 域.setAttribute("abc", 对象);
    		 \${"abc"} 表示直接将"abc"当做普通字符串输出到浏览器。不会从某个域中取数据了。



## 5.0 **EL表达式优先从小范围中读取数据**。

- **当小范围中没有的才从稍大的域对象中取**

- pageContext < request < session < application



## 6.0 EL表达式中有四个隐含的隐式的范围：

**【可以指定域对象 当变量名在多个域中相同时】**

**【隐式范围：自己没有声明，该程序中自己已经定义了】**

- pageScope 对应的是 pageContext范围。
- requestScope 对应的是 request范围。
- sessionScope 对应的是 session范围。
- applicationScope 对应的是 application范围。

~~~jsp
<%@page contentType="text/html;charset=UTF-8" %>

<%
    // 四个域都存储了数据，并且name相同。
    session.setAttribute("data", "session");
    request.setAttribute("data", "request");
    pageContext.setAttribute("data", "pageContext");
    application.setAttribute("data", "application");
%>

<%--在没有指定范围的前提下，EL表达式优先从小范围中取数据--%>
<%--pageContext < request < session < application --%>
${data}

<hr>
<%-- 在EL表达式中可以指定范围来读取数据--%>
<%--EL表达式有4个隐含的隐式的范围对象--%>
<%--pageScope requestScope sessionScope applicationScope--%>
<%--以下是指定范围取数据。--%>
${pageScope.data}<br>
${requestScope.data}<br>
${sessionScope.data}<br>
${applicationScope.data}<br>

<%--在实际开发中，因为向某个域中存储数据的时候，name都是不同的。所以 xxxScope  都是可以省略的。--%>
~~~



## 7.0 EL表达式对null进行了预处理。

【如果是null，则向浏览器输出一个**空字符串**。】

~~~jsp
<%@page contentType="text/html;charset=UTF-8" %>

<%
    request.setAttribute("username", "zhangsan");
%>

<%--取出数据并且输出到浏览器--%>
<%=request.getAttribute("username")%>
采用EL表达式：${username}

<br>

<%=request.getAttribute("usernam")%>

<%--EL表达式主要任务是做页面展示，要求最终页面展示上是友好的。--%>
<%--所以EL表达式对null进行了处理。如果是null，则在浏览器上显示空白。--%>
采用EL表达式：${usernam}

<hr>
<%--EL表达式表面上是这种写法，实际上运行的时候，还是要翻译生成java代码的。--%>
${usernam} 这个EL表达式等同于这一段java代码：<%=request.getAttribute("usernam") == null ? "" : request.getAttribute("usernam")%>
~~~



## 8.0 EL表达式取数据时的两种形式：

- 第一种：**.**  （大部分使用这种方式）
- 第二种：[ ] （如果存储到域的时候，这个name中含有特殊字符，可以使用 [ ]）
  - request.setAttribute("abc.def", "zhangsan");
  - ${requestScope.abc.def} 这样是无法取值的。
  - 应该这样：${requestScope["abc.def"]} 
  - **注意：使用[]里面的值要使用双引号括住**

~~~jsp
<%@ page import="com.bjpowernode.javaweb.jsp.bean.User" %>
<%@page contentType="text/html;charset=UTF-8" %>

<%
    // 创建user对象
    User user = new User();
    user.setUsername("zhangsan");

    // 存储到request域当中。
    request.setAttribute("fdsafdsafdsa", user);

    // 向request域当中存储数据。
    request.setAttribute("abc.def", "hello jsp el!!!");
%>

<%--使用EL表达式取出，并且输出到浏览器--%>
<%--从域当中取user--%>
${fdsafdsafdsa}<br>

<%--取user的username--%>
<%----%>
${fdsafdsafdsa.username}<br>

<%--取user的username，注意[]当中的需要添加 双引号--%>
<%--[] 里面的没有加双引号的话，会将其看做变量。如果是带双引号 "username"，则去找user对象的username属性。--%>
${fdsafdsafdsa["username"]}<br>

<%--将数据取出并输出到浏览器--%>
${requestScope.abc.def}
之前是空白滴：
${requestScope["abc.def"]}
~~~



## 9.0 掌握使用EL表达式从Map集合中取数据：

- **${map.key}**

~~~jsp
<%@ page import="java.util.HashMap" %>
<%@ page import="java.util.Map" %>
<%@ page import="com.bjpowernode.javaweb.jsp.bean.User" %>
<%@page contentType="text/html; charset=UTF-8" %>

<%
    // 一个Map集合
    Map<String,String> map = new HashMap<>();
    map.put("username", "zhangsan");
    map.put("password", "123");

    // 将Map集合存储到request域当中。
	//字符串变量
    request.setAttribute("userMap", map);

	
    Map<String,User> userMap2 = new HashMap<>();
    User user = new User();
    user.setUsername("lisi");
    userMap2.put("user", user);
	//存实体类对象
    request.setAttribute("fdsafdsa", userMap2);
%>




<%--使用EL表达式将Map集合中的user对象中的username取出--%>
${fdsafdsa.user.username}

<hr>

<%--使用EL表达式，将map中的数据取出，并输出到浏览器--%>
${userMap.username}
<br>
${userMap.password}
<br>
${userMap["username"]}
<br>
${userMap["password"]}
~~~



## 10.掌握使用EL表达式，怎么从数组和List集合中取数据：

- ${数组[0]}
- ${数组[1]}
- ${list[0]}

~~~jsp
<%@ page import="com.bjpowernode.javaweb.jsp.bean.User" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.HashSet" %>
<%@ page import="java.util.Set" %>
<%@page contentType="text/html; charset=UTF-8" %>

<%
    // 数组对象
    String[] usernames = {"zhangsan", "lisi", "wangwu"};

    // 向域中存储数组
    request.setAttribute("nameArray", usernames);

    User u1 = new User();
    u1.setUsername("zhangsan");

    User u2 = new User();
    u2.setUsername("lisi");

    User[] users = {u1, u2};

    request.setAttribute("userArray", users);

	//集合 List
    List<String> list = new ArrayList<>();
    list.add("abc");
    list.add("def");

    request.setAttribute("myList", list);

	//无索引集合  Set
    Set<String> set = new HashSet<>();
    set.add("a");
    set.add("b");

    request.setAttribute("set", set);
%>

<%--取出set集合--%>
set集合：${set}
<%--无法获取：PropertyNotFoundException: 类型[java.util.HashSet]上找不到属性[0]--%>
<%--${set[0]}--%>
<hr>

<%--取出List集合--%>
<%--list集合也是通过下标的方式取数据的。--%>
${myList}
${myList[0]}
${myList[1]}
<hr>

<%--取出数组中第二个用户的用户名--%>
${userArray[1].username}
<hr>

<%--使用EL表达式取出数组中的元素--%>
${nameArray} <%--将数组对象直接输出--%>
${nameArray[0]} <%--取出数组中的第一个元素--%>
${nameArray[1]}
${nameArray[2]}

<%--取不出数据，在浏览器上直接显示的就是空白。不会出现数组下表越界。--%>
${nameArray[100]}
~~~



## 11 page指令当中，有一个属性，可以忽略EL表达式

- ```jsp
  <%@page contentType="text/html;charset=UTF-8" isELIgnored="true" %>
  isELIgnored="true" 表示忽略EL表达式
  isELIgnored="false" 表示不忽略EL表达式。（这是默认值）
  
  isELIgnored="true" 这个是全局的控制。
  <!--转义-->
  可以使用反斜杠进行局部控制：\${username} 这样也可以忽略EL表达式。
  ```



## 12. **EL表达式中只有requestScope作用范围，没有request这个九大内置对象之一**

~~~jsp
<%@page contentType="text/html;charset=UTF-8" %>
<%@page import="jakarta.servlet.http.HttpServletRequest"%>

<%--
// JSP有九大内置对象
// pageContext,request,session,application,response,out,config,page,exception
    
// 其中四个域对象，其中最小的域是pageContext
// pageContext翻译为：页面上下文。通过pageContext可以获取？？？？
--%>

	<%--
    在jsp中，pageContext.getRequest() 方法是没用的。
    这是获取request对象。
    而JSP中自带了内置对象request，直接用request内置对象就行了。
    --%>
<%=pageContext.getRequest() %>
<br>



<%=request %>  什么都不输出

<hr>

<%--
    在EL表达式当中没有request这个隐式对象。
    requestScope 这个只代表“请求范围”。不等同于request对象。
    在EL表达式当中有一个隐式的对象：pageContext
    EL表达式中的pageContext和JSP中的九大内置对象pageContext是同一个对象。
--%>
<%--<%=pageContext.getRequest() %>--%>
<%=((HttpServletRequest)pageContext.getRequest()).getContextPath() %>

这段java代码对应的EL表达式：
使用EL表达式来获取应用的根路径：
${pageContext.request.contextPath}
~~~



## 13.通过EL表达式获取应用的根：

- ${pageContext.request.contextPath}



## 14. EL表达式中其他的隐式对象：

- pageContext
- param
- paramValues
- initParam

~~~jsp
<%@page contentType="text/html;charset=UTF-8" %>

<%--JSP中EL表达式的隐含对象：
    1. pageContext
    2. param
    3. paramValues
    4. initParam
    5. 其他（不是重点）
--%>
应用的根路径：${pageContext.request.contextPath}<br>

<%--request是JSP九大内置对象之一。--%>
<%--request.getParameter("username") 获取请求的参数。--%>
<%--用户在浏览器上提交数据：http://localhost:8080/jsp/15.jsp?username=lisi --%>
用户名：<%=request.getParameter("username")%><br>
用户名：${param.username}<br>

<%--假设用户提交的数据：http://localhost:8080/jsp/15.jsp?aihao=smoke&aihao=drink&aihao=tangtou--%>
<%--以上提交的数据显然是采用checkbox进行提交的。同一组的checkbox的name是一样的。--%>
<%--param 获取的是请求参数一维数组当中的第一个元素。--%>
爱好：${param.aihao} <br>
爱好：<%=request.getParameter("aihao")%> <br>

一维数组：${paramValues.aihao}<br>
一维数组：<%=request.getParameterValues("aihao")%><br>

<%--获取数组当中的元素：[下标]--%>
爱好：${paramValues.aihao[0]}、${paramValues.aihao[1]}、${paramValues.aihao[2]} <br>

<%--EL表达式中的隐含对象：initParam--%>
<%--ServletContext是Servlet上下文对象，对应的JSP九大内置对象之一是：application --%>
<%
    String a = application.getInitParameter("pageSize");
    String b = application.getInitParameter("pageNum");
%>

每页显示的记录条数：<%=a%> <br>
页码：<%=b%> <br>

每页显示的记录条数：<%=application.getInitParameter("pageSize")%> <br>
页码：<%=application.getInitParameter("pageNum")%> <br>


每页显示的记录条数：${initParam.pageSize} <br>
页码：${initParam.pageNum} <br>
~~~



## 15. EL表达式的运算符

- 算术运算符

  +、-、*、/、%

​		**“+“不做拼接符**

- 关系运算符
  - [ ] ==   eq    !=   >   >=   <   <= 

不等号、双等号、eq调用的是equal方法

- 逻辑运算符
  - [ ] !  && ||  not and or
- 条件运算符
  - [ ] ? : 
- 取值运算符
  - [ ]和.
- empty运算符
  - [ ] empty运算符的结果是boolean类型
  - [ ] ${empty param.username}
  - [ ] ${not empty param.username}
  - [ ] ${!empty param.password}

~~~jsp
<%@ page import="com.bjpowernode.javaweb.jsp.bean.Student" %>
<%@page contentType="text/html;charset=UTF-8" %>

<%--
    关于EL表达式中的运算符
        1. 算术运算符
            + - * / %
        2. 关系运算符
            == != > >= < <= eq
        3. 逻辑运算符
            ! && || not and or （注意：!和not都是取反）
        4. 条件运算符
            ? :
        5. 取值运算符：
            [] 和 .
        6. empty 运算符
--%>
${10 + 20} <br>

<%-- 在EL表达式当中“+”运算符只能做求和运算。不会进行字符串拼接操作。 --%>
<%--"20"会被自动转换成数字20--%>
${10 + "20"} <br>

<%-- java.lang.NumberFormatException: For input string: "abc" --%>
<%-- + 两边不是数字的时候，一定会转成数字。转不成数字就报错：NumberFormatException--%>
\${10 + "abc"} <br>
\${"king" + "soft"} <br>

<%-- 关系运算符 --%>
${"abc" == "abc"} <br> 真
${"abc"} == ${"abc"} <br> abc = abc

<%
    Object obj = new Object();
    request.setAttribute("k1", obj);
    request.setAttribute("k2", obj);
%>
${k1 == k2} <br> <%--是调用equals方法，因为equals方法没有被重写，调用的还是== 因为是同一个对象 true--%>

<%
    String s1 = new String("hehe");
    String s2 = new String("hehe");
    request.setAttribute("a", s1);
    request.setAttribute("b", s2);
%>
${a == b} <%-- String类重写了字符串，所以内容相等 true--%> <br>

<%
    Object o1 = new Object();
    Object o2 = new Object();
    request.setAttribute("o1", o1);
    request.setAttribute("o2", o2);
%>
${o1 == o2} <br> <%--false--%>

<%
    Student stu1 = new Student("110", "警察");
    Student stu2 = new Student("110", "警察");

    request.setAttribute("stu1", stu1);
    request.setAttribute("stu2", stu2);
%>

<%--EL表达式当中的 "==" 调用了equals方法。--%>
\${stu1 == stu2} <br>
\${stu1 eq stu2} <br>

<hr>
<%-- != 也会调用对象的equals方法。--%>
${stu1 != stu2} <br>
${200 != 200} <br>

<%--以下语法错误，没有加小括号--%>
\${!stu1 eq stu2} <br>
<%--正确的--%>
${!(stu1 eq stu2)}<br>
${not(stu1 eq stu2)}<br>

<%--条件运算符--%>
${empty param.username ? "对不起，用户名不能为空！" : "欢迎访问！！！！" }


<hr>
<%--empty运算符：运算结果是boolean类型。--%>
<%--判断是否为空，如果为空，结果是true。如果不为空，结果是false--%>
${empty param.username} <br>
${!empty param.username} <br>
${not empty param.username} <br>

<hr>
<%--结果false--%>
<%--前半部分是boolean类型。后面是null--%>
${empty param.password == null} <br>
${(empty param.password) == null} <br>

${param.password == null} <br>
~~~



# JSTL标签库

## 1.0 什么是JSTL标签库？

- Java Standard Tag Lib**（Java标准的标签库）**
- **JSTL标签库通常结合EL表达式一起使用**。**目的**是让JSP中的java代码消失。
- 标签是写在JSP当中的，但实际上最终还是要执行对应的java程序。（java程序在jar包当中。）



## 2.0 使用JSTL标签库的步骤：

- 第一步：引入JSTL标签库对应的jar包。

  - tomcat10之后引入的jar包是：

    - jakarta.servlet.jsp.jstl-2.0.0.jar
    - jakarta.servlet.jsp.jstl-api-2.0.0.jar

  - 在IDEA当中怎么引入？

    - 在WEB-INF下新建lib目录，然后将jar包拷贝到lib当中。然后将其“Add Lib...”
    - 一定是要和mysql的数据库驱动一样，都是放在WEB-INF/lib目录下的。
    - 什么时候需要将jar包放到WEB-INF/lib目录下？如果这个jar是tomcat服务器没有的。

    

- 第二步：在JSP中引入要使用标签库。（使用taglib指令引入标签库。）

  - JSTL提供了很多种标签，你要引入哪个标签？？？？

  - **重点掌握核心标签库**。

  - ```
    <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    这个就是核心标签库。
    prefix="这里随便起一个名字就行了，核心标签库，大家默认叫做c，你随意。"
    ```

- 第三步：在需要使用标签的位置使用即可。表面使用的是标签，底层实际上还是java程序。



## 3.0 JSTL标签的原理

~~~java
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

以上uri后面的路径实际上指向了一个xxx.tld文件。
	tld文件实际上是一个xml配置文件。
	在tld文件中描述了“标签”和“java类”之间的关系。
	以上核心标签库对应的tld文件是：c.tld文件。
	
它在哪里
在jakarta.servlet.jsp.jstl-2.0.0.jar里面META-INF目录下，有一个c.tld文件。
~~~



## 4.0 源码解析：配置文件tld解析

- ```
  <tag>
      <description>对该标签的描述</description>
      <name>catch</name> 标签的名字
      <tag-class>org.apache.taglibs.standard.tag.common.core.CatchTag</tag-class> 标签对应的java类。
      <body-content>JSP</body-content> 标签体当中可以出现的内容，如果是JSP，就表示标签体中可以出现符合JSP所有语法的代码。例如EL表达式。
      <attribute>
          <description>
          	对这个属性的描述
          </description>
          <name>var</name> 属性名
          <required>false</required> false表示该属性不是必须的。true表示该属性是必须的。
          <rtexprvalue>false</rtexprvalue> 这个描述说明了该属性是否支持EL表达式。false表示不支持。true表示支持EL表达式。
      </attribute>
    </tag>
  
  <c:catch var="">
  	JSP....
  </c:catch>
  ```



## 5.0 jstl中的核心标签库core当中有哪些常用的标签呢？

- c:if

  - <c:if test="boolean类型，支持EL表达式"></c: if>

~~~jsp
<%@page contentType="text/html;charset=UTF-8" %>

<%--引入核心标签库--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<%--核心标签库中的if标签--%>
<%--test属性是必须的。test属性支持EL表达式。test属性值只能是boolean类型。--%>
<c:if test="${empty param.username}">
<h1>用户名不能为空。</h1>
</c:if>

<%--没有else标签的话，可以搞两个if出来。--%>
<%--if标签还有var属性，不是必须的。--%>
<%--if标签还有scope属性，用来指定var的存储域。也不是必须的。--%>
<%--scope有四个值可选：page(pageContext域)、request(request域)、session(session域)、application(application域)--%>
<%--将var中的v存储到request域。--%>
<c:if test="${not empty param.username}" var="v" scope="request">
    <h1>欢迎你${param.username}。</h1>
</c:if>

<hr>
<%--通过EL表达式将request域当中的v取出--%>
<%--v变量中存储的是test属性的值。--%>
${v}
~~~





## 6.0 c:forEach

- <c:forEach items="集合，支持EL表达式" var="集合中的元素" varStatus="元素状态对象"> ${元素状态对象.count} </c: forEach>
- <c:forEach var="i" begin="1" end="10" step="2"> ${i} </c: forEach>

~~~jsp
<%@ page import="com.bjpowernode.javaweb.jsp.bean.Student" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@page contentType="text/html;charset=UTF-8" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<%--var用来指定循环中的变量--%>
<%--begin开始--%>
<%--end结束--%>
<%--step步长--%>
<%--底层实际上：会将i存储到pageContext域当中。--%>
<c:forEach var="i" begin="1" end="10" step="1">
    <%--所以这里才会使用EL表达式将其取出，一定是从某个域当中取出的。--%>
    ${i}<br>
</c:forEach>

<%
    // 创建List集合
    List<Student> stuList = new ArrayList<>();

    // 创建Student对象
    Student s1 = new Student("110", "经常");
    Student s2 = new Student("120", "救护车");
    Student s3 = new Student("119", "消防车");

    // 添加到List集合中
    stuList.add(s1);
    stuList.add(s2);
    stuList.add(s3);

    // 将list集合存储到request域当中
    request.setAttribute("stuList", stuList);
%>

<hr>
<%--var="s"这个s代表的是集合中的每个Student对象--%>
<%--varStatus="这个属性表示var的状态对象，这里是一个java对象，这个java对象代表了var的状态"--%>
<%--varStatus="这个名字是随意的"--%>
<%--varStatus这个状态对象有count属性。可以直接使用。--%>
<c:forEach items="${stuList}" var="s" varStatus="stuStatus">
    <%--varStatus的count值从1开始，以1递增，主要是用于编号/序号。--%>
    编号：${stuStatus.count},id:${s.id},name:${s.name} <br>
</c:forEach>


~~~



## 7.0 c:choose c:when c:otherwise

- ```jsp
  <%@page contentType="text/html;charset=UTF-8" %>
  <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  
  <%--
      if(){
  
      }else if(){
  
      }else if(){
  
      }else if(){
  
      }else{
  
      }
  --%>
  <c:choose>
      <c:when test="${param.age < 18}">
          青少年
      </c:when>
      <c:when test="${param.age < 35}">
          青年
      </c:when>
      <c:when test="${param.age < 55}">
          中年
      </c:when>
      <c:otherwise>
          老年
      </c:otherwise>
  </c:choose>
  ```

  

# 改造OA

## 1.0 使用什么技术改造呢？

- Servlet + JSP + EL表达式 + JSTL标签。进行改造。



在前端**HTML代码中，有一个标签，叫做base标签，这个标签可以设置整个网页的基础路径**。

- 这是Java的语法，也不是JSP的语法。是**HTML中的一个语法。HTML中的一个标签。通常出现在head标签中**。

- **< base href="http://localhost:8080/oa/">**

- **在当前页面中，凡是路径没有以“/”开始的，都会自动将base中的路径添加到这些路径之前**。

  - < a href="ab/def"></ a>
  - 等同于：< a href="http://localhost:8080/oa/ab/def"></ a>

- 需要注意：**在JS代码中的路径，保险起见，最好不要依赖base标签。JS代码中的路径最好写上全路径**。

- ```
  <base href="${pageContext.request.scheme}://${pageContext.request.serverName}:${pageContext.request.serverPort}${pageContext.request.contextPath}/">
  ```

## 2.0 删除cookies时注意cookies关联路径问题

~~~java
            Cookie[] cookies = request.getCookies();
            if (cookies != null) {
                for (Cookie cookie : cookies) {
                    // 设置cookie的有效期为0，表示删除该cookie
                    cookie.setMaxAge(0);
                    // 设置一个下cookie的路径
                    cookie.setPath(request.getContextPath()); // 删除cookie的时候注意路径问题。
                    // 响应cookie给浏览器，浏览器端会将之前的cookie覆盖。
                    response.addCookie(cookie);
                }
            }
~~~



# Filter过滤器

## 1.0 当前的OA项目存在什么缺陷？

- DeptServlet、EmpServlet、OrderServlet。每一个Servlet都是处理自己相关的业务。在这些Servlet执行之前都是需要判断用户是否登录了。
- 如果用户登录了，可以继续操作，如果没有登录，需要用户登录。这段判断用户是否登录的代码是固定的，并且在每一个Servlet类当中都需要编写
- 显然代码没有得到重复利用。
- 包括每一个Servlet都要解决中文乱码问题，也有公共的代码。这些代码目前都是重复编写，并没有达到复用。怎么解决这个问题?
  - 可以使用**Servlet规范中的Filter过滤器**来解决这个问题。



## 2.0 Filter是什么，有什么用，执行原理是什么？

- Filter是过滤器。
- Filter可以在Servlet这个目标程序**执行之前添加代码**。也可以在目标**Servlet执行之后添加代码**。之前之后都可以添加过滤规则。
- 一般情况下，都是在过滤器当中编写公共代码。



## 3.0 一个过滤器怎么写呢？

- 第一步：编写一个Java类实现一个接口：jarkata.servlet.Filter。并且实现这个接口当中所有的方法。

  - init方法：在Filter对象第一次被创建之后调用，并且只调用一次。
  - doFilter方法：只要用户发送一次请求，则执行一次。发送N次请求，则执行N次。在这个方法中编写过滤规则。
  - destroy方法：在Filter对象被释放/销毁之前调用，并且只调用一次。

  

- 第二步：在**web.xml文件中对Filter进行配置**。这个配置和Servlet很像。

  - ```xml
        <!--Filter1-->
       <filter>
            <filter-name>filter1</filter-name>
            <filter-class>com.bjpowernode.javaweb.servlet.Filter1</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>filter1</filter-name>
            <url-pattern>/a.do</url-pattern>
            <url-pattern>/b.do</url-pattern>
           <url-pattern>*.do</url-pattern>
           <url-pattern>/*</url-pattern>
        </filter-mapping>
    
        <!--<Filter2-->
    	<filter>
            <filter-name>filter2</filter-name>
            <filter-class>com.bjpowernode.javaweb.servlet.Filter2</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>filter2</filter-name>
            <url-pattern>*.do</url-pattern>
        </filter-mapping>
    ```

  - 或者使用**注解**：@WebFilter({"*.do"}) **【不建议】**



- 注意：

  - **Servlet对象**默认情况下，在**服务器启动的时候是不会**新建对象的。
  - **Filter对象**默认情况下，在**服务器启动的时候**会新建对象。
  - Servlet是单例的。Filter也是单例的。（单实例。）



- 目标Servlet是否执行，取决于两个条件：

  - 第一：在过滤器当中是否编写了：chain.doFilter(request, response); 代码**。【是否放行】**
  - 第二：用户发送的请求路径是否和Servlet的请求路径一致**。【是否请求该资源】**



- chain.doFilter(request, response); 这行代码的作用：

  - **执行下一个过滤器**，如果下面**没有过滤器**了，**执行最终的Servlet**。



- 注意：**Filter的优先级，天生的就比Servlet优先级高**。

  - /a.do 对应一个Filter，也对应一个Servlet。那么一定是先执行Filter，然后再执行Servlet。

~~~java
package com.bjpowernode.javaweb.servlet;

import jakarta.servlet.*;
import jakarta.servlet.annotation.WebFilter;

import java.io.IOException;

//@WebFilter("/abc")
//@WebFilter("/a.do")
//@WebFilter({"/a.do", "/b.do"})

/*
以下这个路径属于模糊匹配中的
扩展匹配。以星号开始，注意这种路径不要以/开始。
要么是 /* 要么是*.do
*/
@WebFilter("*.do")

/*属于前缀匹配。要以/开始。*/
//@WebFilter("/dept/*")

// 匹配所有的路径。
//@WebFilter("/*")
public class Filter1 implements Filter {

    /*public Filter1(){
        System.out.println("无参数构造方法执行");
    }*/

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init方法执行。");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
				
        // 在请求的时候添加过滤规则。
		
        System.out.println("Filter1 doFilter方法开始执行。");
        // 执行下一个过滤器，如果下一个不是过滤器了，则执行目标程序Servlet。
        // 向下走。没有它是不行滴。
        chain.doFilter(request, response);

        // 在响应的时候添加过滤规则。
        System.out.println("Filter1 doFilter方法执行结束。");
    }

    @Override
    public void destroy() {
        System.out.println("destroy方法执行");
    }
}
~~~



## 4.0 关于Filter的配置路径：

- /a.do、/b.do、/dept/save。这些配置方式都是**精确匹配**。
- /* **匹配所有**路径。
- *.do **后缀匹配**。**不要以 / 开始 ** **【只有后缀匹配是不以/开头】**
- /dept/*  **前缀匹配**。





## 5.0 在**web.xml文件中进行配置**的时候，**Filter的执行顺序**是什么？

- **依靠filter-mapping标签的配置位置，越靠上优先级越高**。

  

- 过滤器的调用顺序，遵循栈**数据结构**。

- 使用**@WebFilter**的时候，**Filter的执行顺序是怎样**的呢？

  - 执行顺序是：比较Filter这个类名**。【字典顺序】**
  - 比如：FilterA和FilterB，则先执行FilterA。
  - 比如：Filter1和Filter2，则先执行Filter1。

  

  ## 6.0 Filter的生命周期？

  - 和Servlet对象生命周期一致。
  - 唯一的区别：Filter默认情况下，在**服务器启动阶段就实例化**。Servlet不会。



## 7.0 Filter过滤器这里有一个设计模式：

- 责任链设计模式。

**以下只是类比过滤器，他不能动态组合程序执行顺序，** **在编译阶段就确定了程序执行顺序 **

~~~java
package com.bjpowernode.javaweb.servlet;

/**
 * 这个程序的问题：
 *  在编译阶段已经完全确定了调用关系。
 *  如果你想改变他们的调用顺序，必须修改以下java源代码。
 *  java代码修改，需要重新编译，项目需要重新测试，项目需要重新发布。这是一个繁琐的过程。
 *  显然，这种设计违背了：OCP原则。（开闭原则）
 *
 *  开闭原则：对扩展开放，对修改关闭。
 *         对项目扩展我没有意见，但是你扩展的过程中如果修改了我之前写好的代码，这就是你的不对了。
 *         最好要达到的效果是，可以扩展，但是最好别改我之前写好的代码。
 */
public class Test {
    public static void main(String[] args) {
        System.out.println("main begin");
        m1();
        System.out.println("main over");
    }

    private static void m1() {
        System.out.println("m1 begin");
        m2();
        System.out.println("m1 over");
    }

    private static void m2() {
        System.out.println("m2 begin");
        m3();
        System.out.println("m2 over");
    }

    private static void m3() {
        System.out.println("目标正在执行中。。。。");
    }
}

~~~



- 过滤器最大的**优点**：
  - 在程序编译阶段不会确定调用顺序。因为Filter的调用顺序是配置到web.xml文件中的，只要修改web.xml配置文件中filter-mapping的顺序就可以调整Filter的执行顺序。**显然Filter的执行顺序是在程序运行阶段动态组合的**。**那么这种设计模式被称为责任链设计模式**。



- 责任链设计模式最大的核心思想：
  - 在程序运行阶段，动态的组合程序的调用顺序。



# Listener监听器

## 1.0 什么是监听器？

- **监听器是Servlet规范中的一员**。就像Filter一样。**Filter也是Servlet规范中的一员**。
- 在Servlet中，所有的监听器接口都是以“Listener”结尾。



## 2.0 监听器有什么用？

- 监听器实际上是Servlet规范留给我们javaweb程序员的**特殊时机**。
- **特殊的时刻如果想执行这段代码，你需要想到使用对应的监听器**。



~~~java
package com.bjpowernode.javaweb.servlet;

public class Test2 {

    // 静态代码块，在类加载时执行，并且只执行一次。
    // 这个语法很简单，静态代码块什么时候用？这是困扰大家的一个问题。
    // 静态代码块用的实际上不多。但偶尔要用一下。什么时候用呢？
    // 那要看具体的需求，你是否需要在类加载时机去执行一段代码，如果有需要，就可以把这个代码写入静态代码块，到时候自然就执行了。
    // 静态代码块就是java语言为java程序员预留的一个特殊的时机，这个时机叫做：类加载时机。
    static {
        System.out.println("类加载了。");
    }

    // 入口
    public static void main(String[] args) {

    }
}

~~~



## 3.0 Servlet规范中提供了哪些监听器？

- jakarta.servlet包下：

  - ==ServletContextListener==
  - ServletContextAttributeListener
  - ==ServletRequestListener==
    - 该监听器需要使用**@WebListener**注解进行标注。
    - 该监听器监听的是什么？是**request域中数据的变化**。只要数据变化，则执行相应的方法。主要监测点在request域对象上。
  - ServletRequestAttributeListener

- jakarta.servlet.http包下：

  - ==HttpSessionListener==
  - **HttpSessionAttributeListener**
    - 该监听器需要使用**@WebListener**注解进行标注。
    - 该监听器监听的是什么？是**session域中数据的变化**。只要数据变化，则执行相应的方法。主要监测点在session域对象上。
  - **HttpSessionBindingListener**
    - 该监听器**不需要使用**@WebListener进行标注。
    - 假设**User类实现了该监听器**，那么**User对象在被放入session的时候触发bind事件**，**User对象从session中删除的时候，触发unbind事件**。
    - 假设Customer类没有实现该监听器，那么Customer对象放入session或者从session删除的时候，不会触发bind和unbind事件。
  - HttpSessionIdListener
    - session的id发生改变的时候，监听器中的唯一一个方法就会被调用。
  - HttpSessionActivationListener
    - 监听session对象的钝化和活化的。
    - 钝化：session对象从内存存储到硬盘文件。
    - 活化：从硬盘文件把session恢复到内存。

  

## 4.0 实现一个监听器的步骤：以ServletContextListener为例。

- 第一步：编写一个类实现ServletContextListener接口。并且实现里面的方法。

  - ```java
    void contextInitialized(ServletContextEvent event)
    void contextDestroyed(ServletContextEvent event)
    ```

- 第二步：在web.xml文件中对ServletContextListener进行配置，如下：

  - ```xml
    <listener>
        <listener-class>com.bjpowernode.javaweb.listener.MyServletContextListener</listener-class>
    </listener>
    ```

  - 当然，第二步也可以不使用配置文件，也可以用注解，例如：@WebListener



**ServletContextListener**

~~~java
package com.bjpowernode.javaweb.listener;

import jakarta.servlet.ServletContextEvent;
import jakarta.servlet.ServletContextListener;
import jakarta.servlet.annotation.WebListener;

// ServletContextListener监听器主要监听的是：ServletContext对象的状态。
@WebListener
public class MyServletContextListener implements ServletContextListener {

    /**
     * 监听器中的方法不需要程序员手动调用。是发生某个特殊事件之后被服务器调用。
     */
    @Override
    public void contextInitialized(ServletContextEvent sce) { // 服务器启动时间点，想在这个时候执行一段代码，写就行了。
        // 现在这个特殊的时刻写代码，你写就是了。它会被服务器自动调用。
        // 这个方法是在ServletContext对象被创建的时候调用。
        System.out.println("ServletContext对象创建了。");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) { // 服务器关闭时间点。
        // 现在这个特殊的时刻写代码，你写就是了。它会被服务器自动调用。
        // 这个方法是在ServletContext对象被销毁的时候调用。
        System.out.println("ServletContext对象被销毁了。");
    }
}
~~~



**ServletRequestListener**

~~~java
package com.bjpowernode.javaweb.listener;

import jakarta.servlet.ServletRequestEvent;
import jakarta.servlet.ServletRequestListener;
import jakarta.servlet.annotation.WebListener;
//每发送一次请求就会触发该监听器一次
@WebListener
public class MyServletRequestListener implements ServletRequestListener {
    // request对象销毁时间点
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("request对象销毁了");
    }

    // request对象创建时间点
    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        System.out.println("request对象初始化了");
    }
}
~~~



**HttpServletSession**

~~~java
package com.bjpowernode.javaweb.listener;

import jakarta.servlet.annotation.WebListener;
import jakarta.servlet.http.HttpSessionEvent;
import jakarta.servlet.http.HttpSessionListener;

@WebListener
public class MyHttpSessionListner implements HttpSessionListener {
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("session被创建了");
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("session被销毁了");
    }
}

~~~



**HttpSessionAttributeServlet**

~~~java
package com.bjpowernode.javaweb.listener;

import jakarta.servlet.annotation.WebListener;
import jakarta.servlet.http.HttpSessionAttributeListener;
import jakarta.servlet.http.HttpSessionBindingEvent;

@WebListener
public class MyHttpSessionAttributeListener implements HttpSessionAttributeListener {

    // 向session域当中存储数据的时候，以下方法被WEB服务器调用。
    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {
        System.out.println("session data add");
    }

    // 将session域当中存储的数据删除的时候，以下方法被WEB服务器调用。
    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) {
        System.out.println("session data remove");
    }

    // session域当中的某个数据被替换的时候，以下方法被WEB服务器调用。
    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) {
        System.out.println("session data replace");
    }
}

~~~



**HttpSessionBindingListener**

~~~java
package com.bjpowernode.javaweb.bean;

import jakarta.servlet.http.HttpSessionBindingEvent;
import jakarta.servlet.http.HttpSessionBindingListener;

/**
 * 普通的java类。但是它实现了：HttpSessionBindingListener
 */
public class User1 implements HttpSessionBindingListener {

    @Override
    public void valueBound(HttpSessionBindingEvent event) {
        System.out.println("绑定数据");
    }

    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {
        System.out.println("解绑数据");
    }

    private String usercode;
    private String username;
    private String password;

    public User1(String usercode, String username, String password) {
        this.usercode = usercode;
        this.username = username;
        this.password = password;
    }

    public User1() {
    }

    public String getUsercode() {
        return usercode;
    }

    public void setUsercode(String usercode) {
        this.usercode = usercode;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

~~~



```java
package com.bjpowernode.javaweb.servlet;

import com.bjpowernode.javaweb.bean.User1;
import com.bjpowernode.javaweb.bean.User2;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

@WebServlet("/session/bind")
public class HttpSessionBindingListenerServlet extends HttpServlet {
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 获取session对象
    HttpSession session = request.getSession();

    // 准备两个对象：User1 User2
    User1 user1 = new User1("111", "zhangsan", "123");
    User2 user2 = new User2("111", "zhangsan", "123");

    // 将user1存储到session域
    //触发监听器
    //触发点是实现了HttpSessionBingListener
    session.setAttribute("user1", user1);

    // 将user2存储到session域
    session.setAttribute("user2", user2);
 }
}
```


- 注意：所有监听器中的方法都是不需要javaweb程序员调用的，由服务器来负责调用？什么时候被调用呢？

  - 当某个特殊的事件发生（特殊的事件发生其实就是某个时机到了。）之后，被web服务器自动调用。



- 思考一个业务场景：

  - 请编写一个功能，记录该网站实时的在线用户的个数。
  - 我们可以通过服务器端有没有分配session对象，因为一个session代表了一个用户。
  - 有一个session就代表有一个用户。如果你采用这种逻辑去实现的话，session有多少个，在线用户就有多少个。
  - 这种方式的话：HttpSessionListener够用了。session对象只要新建，则count++
  - 然后将count存储到ServletContext域当中，在页面展示在线人数即可。

  

  - 业务发生改变了，只统计登录的用户的在线数量，这个该怎么办？
    - session.setAttribute("user", userObj); 
    - 用户登录的标志是什么？session中曾经存储过User类型的对象。那么这个时候可以让User类型的对象实现HttpSessionBindingListener监听器，只要User类型对象存储到session域中，则count++，然后将count++存储到ServletContext对象中。页面展示在线人数即可。



- 实现oa项目中当前登录在线的人数。
  - 什么代表着用户登录了？
    - session.setAttribute("user", userObj); User类型的对象只要往session中存储过，表示有新用户登录。
  - 什么代表着用户退出了？
    - session.removeAttribute("user"); User类型的对象从session域中移除了。
    - 或者有可能是session销毁了。（session超时）

~~~java
package com.bjpowernode.oa.bean;

import jakarta.servlet.ServletContext;
import jakarta.servlet.http.HttpSessionBindingEvent;
import jakarta.servlet.http.HttpSessionBindingListener;

public class User implements HttpSessionBindingListener {

    @Override
    public void valueBound(HttpSessionBindingEvent event) {
        // 用户登录了
        // User类型的对象向session中存放了。
        // 获取ServletContext对象
        ServletContext application = event.getSession().getServletContext();
        // 获取在线人数。
        Object onlinecount = application.getAttribute("onlinecount");
        if (onlinecount == null) {
            application.setAttribute("onlinecount", 1);
        } else {
            int count = (Integer)onlinecount;
            count++;
            application.setAttribute("onlinecount", count);
        }

    }

    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {
        // 用户退出了
        // User类型的对象从session域中删除了。
        ServletContext application = event.getSession().getServletContext();
        Integer onlinecount = (Integer)application.getAttribute("onlinecount");
        onlinecount--;
        application.setAttribute("onlinecount", onlinecount);
    }

    private String username;
    private String password;

    public User() {
    }

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
~~~
