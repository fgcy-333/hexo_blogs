---
title: SpringMVC
date: 2022-08-26 17:33:13
tags:
- 动力节点-张啊荣-SpringMVC
categories: 
- 视频学习笔记
index_img: /images/springmvc.png
---









# SpringMVC概述

## 什么是SpringMVC

  它是**基于MVC开发模式**的**框架**,用来优化控制器.它是Spring家族的一员.它也具备IOC和AOP.



##  什么是MVC?

  它是一种**开发模式**,它是模型 视图 控制器的简称.所有的**web应用都是基于MVC开发**.

- M:模型层,包含实体类,业务逻辑层,数据访问层
- V:视图层,html,javaScript,vue等都是视图层,用来显现数据
- C:控制器,它是用来接收客户端的请求,并返回响应到客户端的组件,Servlet就是组件



**图解MVC**

---

![image-20220509122947011](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261738023.png)

---

<!--more-->

## SpringMVC框架的优点

- 轻量级,核心功能的jar包很小，非入侵的
- 基于MVC的框架
- 它具备IOC和AOP
- 完全基于注解开发



## SpringMVC执行的流程



---

![image-20220509124318425](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261738821.png)

---

1.  向服务器发送HTTP请求，请求被前端控制器 DispatcherServlet 捕获。
2.  DispatcherServlet 根据\<servlet-name>中的配置对请求的URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用 HandlerMapping 获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以 HandlerExecutionChain 对象的形式返回。
3.  DispatcherServlet 根据获得的Handler，选择一个合适的 HandlerAdapter。
4.  提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：
5.  HttpMessageConveter：将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息。
6.  数据转换：对请求消息进行数据转换。如String转换成Integer、Double等。
7.  数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
8.  数据验证：验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中。
9.  Handler(Controller)执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象。
10.  根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet。
11.  ViewResolver 结合Model和View，来渲染视图。
12.  视图负责将渲染结果返回给客户端



## SpringMVC的优化方向

---

![image-20220512101645217](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261738551.png)

---

![image-20220510000807342](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261738918.png)

---



# 基于注解的SpringMVC程序

所谓 SpringMVC 的注解式开发是指，在代码中通过对类与方法的注解，便可完成处理器在 springmvc 容器的注册。注解式开发是重点。

项目案例功能：用户提交一个请求，服务端处理器在接收到这个请求后，给出一条欢迎信息，在响应页面中显示该信息。



1. 新建项目,选择webapp模板.

~~~
maven-webapp
~~~

2. 修改目录,添加缺失的test, java, resources (两套),并修改目录属性

~~~
--src
	--main
		--java
		--resources
		--webapp
			--WEB-INF
				--web.xml
	--test
		--java
		--resources
--pom.xml
~~~

3. 修改pom.xml文件,添加SpringMVC的依赖,添加Servlet的依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bjpowernode</groupId>
  <artifactId>springmvc_001_demo</artifactId>
  <version>1.0</version>
  <packaging>war</packaging>



  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
      
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
      
    <!--添加springmvc的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <!--添加servlet的依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>
  </dependencies>

  <build>
    <finalName>springmvc</finalName>
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
    </resources>
  </build>
</project>

~~~

4. 添加springmvc.xml配置文件,指定包扫描,添加视图解析器,

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd 				    	        
                           http://www.springframework.org/schema/context 
                           https://www.springframework.org/schema/context/spring-context.xsd">

    <!--添加包扫描 添加context命名空间-->
    <context:component-scan base-package="com.bjpowernode.controller"></context:component-scan>
    
    <!--添加视图解析器 【内部资源视图解析器】-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        
        <!--配置前缀 指定访问某个目录下的页面文件-->
        <property name="prefix" value="/admin/"></property>
        
        <!--配置后缀 指定访问什么类型的文件-->
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
~~~



5. 在web.xml文件中注册springMVC框架(所有的web请求都是基于servlet的)

​			在web.xml文件中注册SpringMvc框架。因为web的请求都是由Servlet来进行处理的，而SpringMVC的核心处理器就是一个DispatcherServlet

​			它负责接收客户端的请求，并根据请求的路径分派给对应的action（控制器）进行处理，处理结束后依然由核心处理器DispatcherServlet进行响应返回。

​			中央调度器的全限定性类名在导入的 Jar 文件 spring-webmvc-5.2.5.RELEASE.jar 的第一个包org.springframework.web.servlet下可找到。

~~~xml
 <?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--注册SpringMVC框架-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--加载spring核心配置文件到 dispatchServlet中-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
          指定拦截什么样的请求
          http://localhost:8080/one
          http://localhost:8080/index.jsp
          http://localhost:8080/demo.action
          <a href="${pageContext.request.contextPath}/demo.action">访问服务器</a>
        -->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
    
</web-app>
~~~

6. 删除index.jsp页面,并新建,发送请求给服务器

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<br><br><br>
<a href="${pageContext.request.contextPath}/zar/demo.action">访问服务器zar</a>
<a href="${pageContext.request.contextPath}/user/demo.action">访问服务器user</a>
</body>
</html>

~~~

7. 开发控制器(Servlet),它是一个普通的类.

   * 以前的Servlet的规范
   * protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {}
   * action中所有的功能实现都是由方法来完成的
   * action方法的规范【SpringMVC处理器中的方法】
     * 1)访问权限是public
     * 2)方法的返回值任意
     * 3)方法名称任意
     * 4)方法可以没有参数,如果有可是任意类型
     * 5)要使用@RequestMapping注解来声明一个访问的路径(名称)

~~~java
@Controller  //交给Spring去创建对象
@RequestMapping("/zar")
public class DemoAction {

    @RequestMapping("/demo")
    public String demo(){
        System.out.println("zar服务器被访问到了.......");
        return "main";  //可以直接跳到/admin/main.jsp页面上
    }
}

~~~



8. 添加tomcat进行测试功能 

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<br><br><br>
<a href="${pageContext.request.contextPath}/zar/demo.action">访问服务器zar</a>
<a href="${pageContext.request.contextPath}/user/demo.action">访问服务器user</a>
</body>
</html>
```

## 分析web请求

  web请求执行的流程

  					   核心处理器【请求分发，响应返回】

  index.jsp<----------------------->DispatcherServlet<------------------------------>**SpringMVC的处理器**是一个【普通的方法】
  one.jsp  <------------------------>DispatcherServlet<------------------------------>SpringMVC的处理器是一个普通的方法

  DispatcherServlet要在web.xml文件中注册才可用.因为这B也是一个Servlet



# @RequestMapping定义请求规则

**此注解就是来映射服务器访问的路径**

## 指定模块名称

- 通过@RequestMapping 注解可以定义处理器对于请求的映射规则。该注解可以注解在**方法上**，也可以注解在**类上**，但**意义是不同**的。
- value 属性值常以“/”开始。@RequestMapping 的 value 属性用于定义所匹配请求的 URI。

- 一个@Controller 所注解的类中，可以定义多个处理器方法。当然，不同的处理器方法所匹配的 URI 是不同的。
- 这些不同的 URI 被指定在注解于方法之上的@RequestMapping 的value 属性中。但若这些请求具有相同的 URI 部分，则这些相同的 URI部分可以被抽取到注解在类之上的@RequestMapping 的 value 属性中。
- 既是要访问处理器的指定方法，必须要在方法指定 URI 之前加上处理器类前定义的模块名称。



## 对请求提交方式的定义

- 对于@RequestMapping，其有一个属性 method，用于对被注解方法所处理请求的提交
- 方式进行限制，即只有满足该 method 属性指定的提交方式的请求，才会执行该被注解方法。
- Method  属性的取值为 RequestMethod  枚举常量。常用的为 RequestMethod.GET  与RequestMethod.POST
- 分别表示提交方式的匹配规则为 GET 与 POST 提交。

~~~java
@RequestMapping(value = "/hello",method = RequestMethod.POST)
~~~



## **客户端浏览器常用的请求方式，及其提交方式有以下几种**

---

![image-20220510001109182](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737796.png)

---

​	也就是说，只要指定了处理器方法匹配的请求提交方式为 POST，则相当于指定了请求发送的方式：要么使用表单请求，要么使用 AJAX 请求。

​	其它请求方式被禁用。

​	当然，若不指定 method 属性，则无论是 GET 还是 POST 提交方式，均可匹配。即对于请求的提交方式无要求。



# 五种数据提交的方式

**前四种数据注入的方式，会自动进行类型转换。但无法自动转换日期类型。**

日期类型的注入需要使用\<mvc:annotation-driven/>

## (1)单个数据注入

在方法中声明一个和表单提交的参数名称相同的入参，由框架按照名称直接注入

---

![image-20220510002657742](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737761.png)

---

~~~html
  <form action="${pageContext.request.contextPath}/one.action">
      姓名:<input name="myname"><br>
      年龄:<input name="age"><br>
      <input type="submit" value="提交">
  </form>
~~~

~~~java
  @RequestMapping("/one")
    public String one(String myname,int age){  ===>自动注入,并且类型转换
        System.out.println("myname="+myname+",age="+(age+100));
        return "main";
    }
~~~



## (2)对象封装注入

在方法中声明一个自定义的实体类参数，框架调用实体类中相应的setter方法注入属性值

只要保证实体类中成员变量的名称与提交请求的name属性值一致即可。

---

![image-20220510002820647](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737971.png)

---

~~~java
    public class Users {
    private String name;
    private int age;
    }

    页面:
	<form action="${pageContext.request.contextPath}/two.action" method="post">
	    姓名:<input name="name"><br>
	    年龄:<input name="age"><br>
	    <input type="submit" value="提交">
	</form>
            
    action:
    @RequestMapping("/two")
    public String two(Users u){
        System.out.println(u);
        return "main";
    }
~~~



## (3) 动态占位符提交（用于GET或超链接）

使用框架提供的一个注解**@PathVariable**，将请求url中的值作为参数进行提取，只能是超链接。

restful风格下的数据提取方式。restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。

它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。



---

![image-20220510142225769](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737033.png)

---

```java
仅限于超链接或地址拦提交数据.它是一杠一值,一杠一大括号,使用注解@PathVariable来解析.      
<a href="${pageContext.request.contextPath}/three/张三/22.action">动态提交</a>  
    
@RequestMapping("/three/{uname}/{uage}")
public String three(
        @PathVariable("uname")  ===>用来解析路径中的请求参数
        String name,
        @PathVariable("uage")
        int age){
    System.out.println("name="+name+",age="+(age+100));
    return "main";
}
```

## (4)请求参数名称与形参名称不一致

请求与形参中的名字不对应，可以使用

**@RequestParam**(value="name1"） String name  来进行参数名称绑定。



---

![image-20220510142526776](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737492.png)

---

~~~java
 提交请求参数与action方法的形参的名称不一致,使用注解@RequestParam来解析
    /**
     *  姓名:<input name="name"><br>
     *  年龄:<input name="age"><br>
     */
    @RequestMapping("/four")
    public String four(
            @RequestParam("name")  ===>专门用来解决名称不一致的问题
            String uname,
            @RequestParam("age")
            int uage){
        System.out.println("uname="+uname+",uage="+(uage+100));
        return "main";
    }
~~~



## (5)使用HttpServletRequest对象提取

在方法参数中声明一个request对象，使用request的getParameter()获取表单提交的数据

这样得到的数据,但还要手工进行数据类型的转换。



---

![image-20220510142704240](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737523.png)

---

~~~java
  /**
     *  姓名:<input name="name"><br>
     *  年龄:<input name="age"><br>
     */
  @RequestMapping("/five")
    public String five(HttpServletRequest request){
        String name = request.getParameter("name");
        int age = Integer.parseInt(request.getParameter("age"));
        System.out.println("name="+name+",age="+(age+100));
        return "main";
    }   
~~~



# 请求参数中文乱码解决

## 前提

对于前面所接收的请求参数，若含有中文，则会出现中文乱码问题。

Spring 对于请求参数中的中文乱码问题

给出了专门的字符集过滤器：

 **spring-web-5.2.5.RELEASE.jar** 的org.springframework.web.filter 包下的 CharacterEncodingFilter 类。

  ## 配置过滤器

~~~xml
	<filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--
          配置参数
            private String encoding;
            private boolean forceRequestEncoding;
            private boolean forceResponseEncoding;
        -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping> 
~~~

​		在 web.xml 中注册字符集过滤器，即可解决 Spring 的请求参数的中文乱码问题。

​		不过，最好将**该过滤器注册在其它过滤器之前**。因为过滤器的执行是按照其注册顺序【filter-mapping】进行的





## 源码分析

---

![image-20220510144542391](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737898.png)

---







# 处理器方法的返回值

## 概要

使用@Controller 注解的处理器的方法，其返回值常用的有四种类型：

Ø 第一种：ModelAndView

Ø 第二种：String

Ø 第三种：无返回值void

Ø 第四种：返回对象类型



## 1.0 返回对象Object

处理器方法也可以返回 Object 对象。这个 Object 可以是 Integer，自定义对象，Map，List 等。

但返回的对象不是作为逻辑视图出现的，而是作为直接在页面显示的数据出现的。

返回对象，需要使用@ResponseBody 注解，将转换后的 JSON 数据放入到响应体中。

Ajax请求多用于Object返回值类型。由于转换器底层使用了Jackson 转换方式将对象转换为JSON 数据，所以**需要添加Jackson的相关依赖**。



- 在pom.xml文件中添加依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.fgcy</groupId>
    <artifactId>ajax</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>


    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.10.1</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
</project>

~~~



- 添加jQuery的函数库,在webapp目录下,新建js目录,拷贝jquery-3.3.1.js到目录下



- 在页面添加jQuery的函数库的引用

~~~html
   <script src="js/jquery-3.3.1.js"></script>
~~~



- 发送ajax请求

~~~js

<script type="text/javascript">
    function showStu() {
        $.ajax({
            url: "${pageContext.request.contextPath}/list.do",
            type: "get",
            dataType: "json",
            success: function (stuList) {
                var s = "";
                $.each(stuList, function (i, stu) {
                    s += stu.name + "---" + stu.age + "<br>";
                });
                $("#mydiv").html(s);
            }
        });
    }
</script>
~~~



- 开发action

~~~java
    @ResponseBody
    @RequestMapping("/list")
    public List<Student> getStudentList() {
        ArrayList<Student> list = new ArrayList<>();

        Student aaa = new Student(12, "aaa");
        Student bbb = new Student(12, "ccc");
        Student vvv = new Student(12, "sss");
        Student ccc = new Student(12, "www");
        Collections.addAll(list, aaa, bbb, ccc, vvv);
        return list;
    }
~~~



- 在springmvc.xml文件中添加注解驱动

~~~xml
  <mvc:annotation-driven></mvc:annotation-driven>
~~~



- index.jsp页面

~~~html
<a href="javascript:showStu()">获取数据</a>
<div id="mydiv"></div>
~~~



## 2.0 返回 ModelAndView

若处理器方法处理完后，需要跳转到其它资源，且又要在跳转的资源间传递数据，此时处理器方法返回 ModelAndView 比较好。

当然，若要返回 ModelAndView，则处理器方法中需要定义 ModelAndView 对象。

在使用时，若该处理器方法只是进行跳转而不传递数据，或只是传递数据而并不向任何资源跳转（如对页面的 Ajax  异步响应），此时若返回 ModelAndView

则将总是有一部分多余：要么 Model 多余，要么 View 多余。即此时返回 ModelAndView 将不合适。较少使用。



## 3.0 返回 String

处理器方法返回的字符串**可以指定逻辑视图名**

通过视图解析器解析可以将其转换为物理视图地址。



---

![image-20220512104210352](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737602.png)



---





## 4.0 无返回值void

对于处理器方法返回 void 的应用场景，应用在AJAX 响应处理。若处理器对请求处理后

无需跳转到其它任何资源，此时可以让处理器方法返回 void。

一般不会使用，因为前端需要一些信息来判断操作是否成功







#  SpringMVC的四种跳转方式

## 概述

默认的跳转是请求转发，直接跳转到jsp页面展示，还可以使用框架提供的关键字redirect:，进行一个重定向操作包括重定向**页面**和**重定向action**

使用框架提供的关键字forward:，进行服务器内部转发操作，包括转发页面和转发action。

当使用redirect:和forward:关键字时，视图解析器中前缀后缀的拼接就无效了。

~~~
  本质还是两种跳转:请求转发和重定向,衍生出四种是请求转发页面,转发action,重定向页面,重定向action
~~~



## 页面部分：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<br><br><br>
<br>
<a href="${pageContext.request.contextPath}/one.action">1.请求转发页面(默认)</a><br><br>
<a href="${pageContext.request.contextPath}/two.action">2.请求转发action</a><br><br>
<a href="${pageContext.request.contextPath}/three.action">3.重定向页面</a><br><br>
<a href="${pageContext.request.contextPath}/four.action">4.重定向action</a><br><br>
<a href="${pageContext.request.contextPath}/five.action">5.随便跳页面</a><br><br>

</body>
</html>
```



## Controller部分

```java
/**
 * 请求转发+重定向 cross join 页面+action
 */
@Controller
public class JumpAction {

    @RequestMapping("/one")
    public String one(){
        System.out.println("这是请求转发页面跳转.........");
        return "main";  //默认是请求转发,使用视图解析器【InternalResourceViewResolver】拼接前缀后缀进行页面跳转
    }
    @RequestMapping("/two")
    public String two(){
        System.out.println("这是请求转发action跳转.........");
        //  /admin/  /other.action  .jsp
        //forward: 这组字符串可以屏蔽前缀和后缀的拼接.实现请求转发跳转
        return "forward:/other.action";  //默认是请求转发,使用视图解析器拼接前缀后缀进行页面跳转
    }
    @RequestMapping("/three")
    public String three(){
        System.out.println("这是重定向页面.......");
        //redirect:  这组字符串可以屏蔽前缀和后缀的拼接.实现重定向跳转
        return "redirect:/admin/main.jsp";
    }
    @RequestMapping("/four")
    public String four(){
        System.out.println("这是重定向action.......");
        //redirect:  这组字符串可以屏蔽前缀和后缀的拼接.实现重定向跳转
        return "redirect:/other.action";
    }
    @RequestMapping("/five")
    public String five(){
        System.out.println("这是随便跳.......");
        return "forward:/fore/login.jsp";
    }
}
```

  



# SpringMVC支持的默认参数类型

~~~
这些类型只要写在方法参数中就可以使用了。不需要去创建,直接拿来使用即可.
~~~

**1）HttpServletRequest 对象**

**2）HttpServletResponse 对象**

**3）HttpSession 对象**

**4）Model**

**6) ModelMap 对象**　

**7) Map<String,Object>对象**

~~~
  注意:Map,Model,ModelMap和request一样,都使用请求作用域进行数据传递.所以服务器端的跳转必须是请求转发.
~~~

 

## 页面

~~~jsp
<a href="${pageContext.request.contextPath}/data.action?name=zar">访问服务器,进行数据携带跳转</a><br><br>
~~~



~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h2>main...........显示数据</h2>
<%--
        request.setAttribute("requestUsers",u);
        session.setAttribute("sessionUsers",u);
        model.addAttribute("modelUsers",u);
        map.put("mapUsers",u);
        modelMap.addAttribute("modelMapUsers",u);
--%>
requestUsers:${requestUsers}<br>
sessionUsers:${sessionUsers}<br>
modelUsers:${modelUsers}<br>
mapUsers:${mapUsers}<br>
modelMapUsers:${modelMapUsers}<br>
    <%--EL表达式的内置对象 param--%>
从index.jsp页来来的数据${param.name}
</body>
</html>

~~~



## Action

```java
/**
 *SpringMVC支持的默认参数类型
 */
@Controller
public class DataAction {

    @RequestMapping("/data")
    public String data(HttpServletRequest request,
                       HttpServletResponse response,
                       HttpSession session,
                       Model model,
                       Map map,
                       ModelMap modelMap){

        //做一个数据,传到main.jsp页面上
        Users u = new Users("张三",22);

        //传递数据
        request.setAttribute("requestUsers",u);
        session.setAttribute("sessionUsers",u);
        model.addAttribute("modelUsers",u);
        map.put("mapUsers",u);
        modelMap.addAttribute("modelMapUsers",u);

        return "main";  //请求转发方式跳转
        //使用重定向，除了HttpSession域对象还可以携带参数外，其他都失效
       // return "redirect:/admin/main.jsp";
        
       /*
       *Map,Model,ModelMap和request一样,都使用请求作用域进行数据传递.所以服务器端的跳转必须是请求转发.
       */
    }
}
```



# 日期注入 

日期类型**不能自动注入**到方法的参数中。需要单独做转换处理。

使用**@DateTimeFormat**注解，需要**在springmvc.xml文件中添加   \<mvc:annotation-driven/>**   标签。



## (1)在方法的参数上【入参】使用@DateTimeFormat注解【单个日期处理】

```java
@RequestMapping("/myDate")
public String submitdateone(
@DateTimeFormat(pattern="yyyy-MM-dd")
        Date mydate){
    System.out.println(mydate);
    return "dateShow";
```



## (2)在类的成员setXXX()方法上使用@DateTimeFormat注解【单个日期处理】

~~~java
@DateTimeFormat(pattern="yyyy-MM-dd")//给入参赋值
public void setDate(Date date) {
this.date = date;
}
~~~



但这种解决方案要在每个使用日期类型的地方都去添加使用@DateTimeFormat注解

比较麻烦

我们可以使用@InitBinder注解来进行类中统一日期类型的处理。



## (3)@InitBinder注解解决类中日期问题 【类中全局日期处理】

~~~java
//注册一个注解,用来解析本类中所有的日期类型,自动转换.

@InitBinder
public void initBinder(WebDataBinder dataBinder) {
    SimpleDateFormat sf = new SimpleDateFormat("yyyy-MM-dd");
	dataBinder.registerCustomEditor(Date.class, new CustomDateEditor(sf, true));
}
~~~



这样在类中出现的所有日期都可以进行转换了。

使用该注解后就**不需要**在springmvc.xml文件中添加   **\<mvc:annotation-driven/>**   标签



# 日期显示



## JSON中的日期显示

需要在类中的成员变量的getXXX方法上加注解.

~~~java
@JsonFormat(pattern="yyyy-MM-dd HH:mm:ss")
public Date getDate() {
return date;
}
~~~



## （2）JSP页面的日期显示

​			

添加依赖jstl

~~~xml
      <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
~~~



如果是list中的实体类对象的成员变量是日期类型,则必须使用jstl进行显示.


    <%--导入jstl核心标签库--%>
    <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%--导入jstl格式化标签库--%>
    <%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>



使用JSTL将Date对象的数据转为方便看的

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--导入jstl核心标签库--%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%--导入jstl格式化标签库--%>
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h2>show.........</h2>
单个日期显示:${mydate}
<br><br><br>
<h2>学生集合</h2>
<table width="800px" border="1">
    <tr>
        <th>姓名</th>
        <th>生日</th>
    </tr>
    <c:forEach items="${list}" var="stu">
        <tr>
            <td>${stu.name}</td>
            <td>${stu.birthday}------ <fmt:formatDate value="${stu.birthday}" pattern="yyyy-MM-dd"></fmt:formatDate> </td>
        </tr>
    </c:forEach>
</table>
</body>
</html>
```





# \<mvc:annotation-driven/>标签的使用

\<mvc:annotation-driven/>会自动注册两个bean

分别为DefaultAnnotationHandlerMapping和AnnotationMethodHandlerAdapter **是 springmvc 为 @controller 分发请求所必须的**。

除了注册了这两个bean，还提供了很多支持。

1）支持使用ConversionService 实例对表单参数进行类型转换，【可以进行全局的类型转换，但老有bug，不用】

 2）支持使用 @NumberFormat 、@**DateTimeFormat**；

 3）注解完成数据类型的格式化；

 4）支持使用 @**RequestBody** 和 @**ResponseBody** 注解；

 5）**静态资源的分流**也使用这个标签;







# 资源在WEB-INF目录下

 很多企业会将动态资源放在WEB-INF目录下，这样可以保证资源的安全性。

~~~
在WEB-INF目录下的动态资源不可以直接访问，必须要通过请求转发的方式进行访问。
~~~



## 重新配置SpringMVC的拦截路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--中文编码过滤器配置-->
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--
          配置参数
            private String encoding;
            private boolean forceRequestEncoding;
            private boolean forceResponseEncoding;
        -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!--注册springmvc框架-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        
        <url-pattern>/</url-pattern>
        
    </servlet-mapping>
</web-app>
```



## 通过请求转发的方式访问静态资源

~~~java
@Controller
public class ShowAction {
    @RequestMapping("/showIndex")
    public String showIndex(){
        System.out.println("index.............");
        return "index";
    }
    @RequestMapping("/showMain")
    public String showMain(){
        System.out.println("main.............");
        return "main";
    }
    @RequestMapping("/showLogin")
    public String showLogin(){
        System.out.println("login.............");
        return "login";
    }
~~~







# SpringMVC拦截器

## 概述

SpringMVC 中的 Interceptor 拦截器，它的主要作用是拦截指定的用户请求，并进行相应的预处理与后处理。

其拦截的时间点在“处理器映射器根据用户提交的请求映射出了所要执行的处理器类，并且也找到了要执行该处理器类的处理器适配器

在处理器适配器执行处理器之前”。在处理器映射器映射出所要执行的处理器类时，已经将拦截器与处理器组合为了一个处理器执行链，并返回给了中央调度器。

~~~
  针对请求和响应进行的额外的处理.在请求和响应的过程中添加预处理,后处理和最终处理.
~~~



## 拦截器的执行原理

---

![image-20220510211958217](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737633.png)

---



## 拦截器执行的时机

- 1)preHandle():在请求被处理之前进行操作

- 2)postHandle():在请求被处理之后,但结果还没有渲染前进行操作,可以改变响应结果

- 3)afterCompletion:所有的请求响应结束后执行善后工作,清理对象,关闭资源





## 拦截器实现的两种方式

- 1)继承HandlerInterceptorAdapter的父类
- 2)实现HandlerInterceptor接口,实现的接口,**【推荐使用实现接口的方式】**
- 单继承的情况下，肯定希望找一个功能强大的父类



## 拦截器的应用场景

1、日志记录：记录请求信息的日志

2、权限检查，如登录检查

3、性能检测：检测方法的执行时间

拦截器 约等于 过滤器+监听器



# HandlerInterceptor接口分析

**自定义拦截器，需要实现 HandlerInterceptor 接口。而该接口中含有三个方法**：



## (1) preHandle【前置处理】

该方法在**处理器方法执行之前**执行。其返回值为 boolean，若为 true，则紧接着会执行处理器方法

且会将 afterCompletion()方法放入到一个专门的方法栈中等待执行。

~~~
通过验证，返回true 才能执行目标方法【处理器】
~~~

## **(2)** postHandle【后置处理】

该方法在**处理器方法执行之后**执行。处理器方法若最终未被执行，则该方法不会执行。

由于该方法是在处理器方法执行完后执行，且该方法参数中包含 ModelAndView

所以该方法可以修改处理器方法的处理结果数据，且可以修改跳转方向。

~~~
处理器执行后，可以修改返回的值；
若前置处理被不通过，这个也不会执行
~~~



## (3)afterCompletion【最终处理】

当preHandle()方法返回 true 时，会将该方法放到专门的方法栈中，等到对请求进行响应的所有工作完成之后才执行该方法。

即该方法是在**中央调度器渲染（数据填充）了响应页面之后**执行的，此时对 ModelAndView 再操作也对响应无济于事。

afterCompletion 最后执行的方法，清除资源，例如在 Controller 方法中加入数据等。



# 自定义拦截器实现权限验证



## web.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--中文编码过滤器配置-->
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!--注册springmvc框架-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
~~~



## SpringMVC.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.controller"></context:component-scan>
    
    <!--添加视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    
    <!--添加注解驱动-->
    <!--<mvc:annotation-driven></mvc:annotation-driven>-->

    <!--注册拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--映射要拦截的请求-->
            <mvc:mapping path="/**"/>
            <!--设置放行的请求-->
            <mvc:exclude-mapping path="/showLogin"></mvc:exclude-mapping>
            <mvc:exclude-mapping path="/login"></mvc:exclude-mapping>
            <!--配置具体的拦截器实现功能的类-->
            <bean class="com.bjpowernode.interceptor.LoginInterceptor"></bean>
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
~~~



## 拦截器

~~~java
/**
 *
 */
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //是否登录过的判断
            /**
             * HttpSession session = request.getSession(false);
             * if(session!=null && session.getAttribute("users")!=null)
            */
        if(request.getSession().getAttribute("users") == null){
            //此时就是没有登录,打回到登录页面,并给出提示
            request.setAttribute("msg","您还没有登录,请先去登录!");
            request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
            return false;
        }
        return true;//放行请求
    }
}

~~~



## 控制器

~~~java
/**
 *
 */
@Controller
public class WebInfAction {

    @RequestMapping("/showIndex")
    public String showIndex(){
        System.out.println("访问index.jsp");
        return "index";
    }
    @RequestMapping("/showMain")
    public String showMain(){
        System.out.println("访问main.jsp");
        return "main";
    }

    @RequestMapping("/showLogin")
    public String showLogin(){
        System.out.println("访问login.jsp");
        return "login";
    }

    //登录的业务判断
    @RequestMapping("/login")
    public String login(String name, String pwd, HttpServletRequest request){
        if("zar".equalsIgnoreCase(name) && "123".equalsIgnoreCase(pwd)){
            //在session中存储用户信息,用于进行权限验证
            request.getSession().setAttribute("users",name);
            return "main";
        }else{
            request.setAttribute("msg","用户名或密码不正确!");
            return "login";
        }
    }
}

~~~



## 静态资源目录

---

![image-20220510214352721](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737392.png)

---





# SSM整合

~~~
SSM整合的步骤
  0)建库,建表
  1)新建Maven项目,选择webapp模板
  2)修改目录
  3)修改pom.xml文件(使用老师提供)
  4)添加jdbc.properties属性文件
  5)添加SqlMapConfig.xml文件(使用模板)
  6)添加applicationContext_mapper.xml文件(数据访问层的核心配置文件)
  7)添加applicationContext_service.xml文件(业务逻辑层的核心配置文件)
  8)添加spirngmvc.xml文件
  9)删除web.xml文件,新建,改名,设置中文编码,并注册spirngmvc框架,并注册Spring框架
  10)新建实体类user
  11)新建UserMapper.java接口
  12)新建UserMapper.xml实现增删查所有功能,没有更新
  13)新建service接口和实现类
  14)新建测试类,完成所有功能的测试
  15)新建控制器,完成所有功能
  16)浏览器测试功能
~~~



## (1)新建Maven项目，添加pom依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bjpowernode</groupId>
  <artifactId>springmvc_006_ssm</artifactId>
  <version>1.0</version>
  <packaging>war</packaging>




  <!-- 集中定义依赖版本号 -->
  <properties>
    <!--单元测试的依赖-->
    <junit.version>4.12</junit.version>
    <!--spring的相关依赖-->
    <spring.version>5.2.5.RELEASE</spring.version>
    <!--mybatis的相关依赖-->
    <mybatis.version>3.5.1</mybatis.version>
    <!--mybaits与spring整合的依赖-->
    <mybatis.spring.version>1.3.1</mybatis.spring.version>
    <!--mybatis支持的分页插件的依赖-->
    <mybatis.paginator.version>1.2.15</mybatis.paginator.version>
    <!--mysql的依赖-->
    <mysql.version>5.1.32</mysql.version>
    <!--slf4j日志依赖-->
    <slf4j.version>1.6.4</slf4j.version>
    <!--阿里的数据库连接池-->
    <druid.version>1.1.12</druid.version>
    <!--分页插件的依赖-->
    <pagehelper.version>5.1.2</pagehelper.version>
    <!--JSTL的依赖(jsp的标准标签库)-->
    <jstl.version>1.2</jstl.version>
    <!--servlet的依赖-->
    <servlet-api.version>3.0.1</servlet-api.version>
    <!--jsp的依赖-->
    <jsp-api.version>2.0</jsp-api.version>
    <!--jackson的依赖,springmvc框架默认进行JSON转换的依赖工具-->
    <jackson.version>2.9.6</jackson.version>
  </properties>


  <dependencies>
    <!-- spring -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
       <!--springmvc的核心依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
      <!--spring推荐的支持aop的框架-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jms</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>
      
           <!--spring整合junit 可以加载spring容器-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <!-- Mybatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>
      
           <!--spring整合mybatis-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>${mybatis.spring.version}</version>
    </dependency>
      
         <!--mybatis分页插件-->
    <dependency>
      <groupId>com.github.miemiedev</groupId>
      <artifactId>mybatis-paginator</artifactId>
      <version>${mybatis.paginator.version}</version>
    </dependency>
      
      <!--分页插件-->
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>${pagehelper.version}</version>
    </dependency>
      
    <!-- MySql -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
    </dependency>
      
    <!--德鲁伊连接池，配置数据源 管理连接 【缓存】 -->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>${druid.version}</version>
    </dependency>

    <!-- junit -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>


    <!-- java标准标签库 -->
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>${jstl.version}</version>
    </dependency>
         <!--javaEE的servlet规范 web服务器或应用服务器中有-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.0.1</version>
      <scope>provided</scope>
    </dependency>
      
      <!--javaEE的jsp规范 web服务器或应用服务器中有-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jsp-api</artifactId>
      <scope>provided</scope>
      <version>${jsp-api.version}</version>
    </dependency>
      
    <!-- Jackson Json处理工具包 @ResponseBody-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>${jackson.version}</version>
    </dependency>

  </dependencies>

  <!-- 插件配置 -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
        </configuration>
      </plugin>
    </plugins>
      
    <!--识别所有的配置文件-->
    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>
  </build>
</project>

~~~

## (2)开发配置文件applicationContext_mapper.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--读取属性文件-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <!--配置数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!--配置SqlSessionFactoryBean-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--配置数据源-->
        <property name="dataSource" ref="dataSource"></property>
        <!--配置SqlMapConfig.xml核心配置-->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"></property>
        <!--注册实体类-->
        <property name="typeAliasesPackage" value="com.bjpowernode.pojo"></property>
    </bean>
    <!--注册mapper.xml文件-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.bjpowernode.mapper"></property>
    </bean>
</beans>
~~~



## (3)开发配置文件applicationContext_service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.service.impl"></context:component-scan>
    <!--添加事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--切记切记:配置数据源-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!--配置事务切面-->
    <tx:advice id="myadvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*select*" read-only="true"/>
            <tx:method name="*find*" read-only="true"/>
            <tx:method name="*serach*" read-only="true"/>
            <tx:method name="*get*" read-only="true"/>
            <tx:method name="*insert*" propagation="REQUIRED"/>
            <tx:method name="*add*" propagation="REQUIRED"/>
            <tx:method name="*save*" propagation="REQUIRED"/>
            <tx:method name="*set*" propagation="REQUIRED"/>
            <tx:method name="*update*" propagation="REQUIRED"/>
            <tx:method name="*change*" propagation="REQUIRED"/>
            <tx:method name="*modify*" propagation="REQUIRED"/>
            <tx:method name="*delete*" propagation="REQUIRED"/>
            <tx:method name="*drop*" propagation="REQUIRED"/>
            <tx:method name="*remove*" propagation="REQUIRED"/>
            <tx:method name="*clear*" propagation="REQUIRED"/>
            <tx:method name="*" propagation="SUPPORTS"/>
        </tx:attributes>
    </tx:advice>
    <!--配置切入点+绑定-->
    <aop:config>
        <aop:pointcut id="mycut" expression="execution(* com.bjpowernode.service.impl.*.*(..))"></aop:pointcut>
        <aop:advisor advice-ref="myadvice" pointcut-ref="mycut"></aop:advisor>
    </aop:config>
</beans>
```

## (4)开发配置文件springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.controller"></context:component-scan>
    <!--添加注解驱动-->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!--因为本项目全部是ajax请求,不需要配置视图解析器-->
</beans>
```

## (5)开发配置文件SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>


    <!--设置日志输出语句,显示相应操作的sql语名-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

</configuration>
```

## (6)在web.xml文件中完成springmvc,spring两个框架的注册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--
		添加中文编码过滤器
        private String encoding;
        private boolean forceRequestEncoding;
        private boolean forceResponseEncoding;
    -->
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--注册SpringMVC框架-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <!--注册Spring框架,目的就是启动spring容器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext_*.xml</param-value>
    </context-param>
</web-app>
```

## (7)创建实体类对象User

```java
/**
 *
 */
public class User {
    private String userId;
    private String cardType;
    private String cardNo;
    private String userName;
    private String userSex;
    private String userAge;
    private String userRole;

//有参 无参 set get toString
}
```

## (8)创建UserMapper接口和实现功能的UserMapper.xml

```java
package com.bjpowernode.mapper;

import com.bjpowernode.pojo.User;
import org.apache.ibatis.annotations.Param;

import java.util.List;

/**
 *
 */
public interface UserMapper {
    /**
     * url /user/selectUserPage?userName=z&userSex=男&page=null
     * 参数
         * userName:表单中用户名称
         * userSex:表单中用户性别
         * page:提交的页码(第一次访问为null)
     * 结果  有数据时：
     * [{
     * "userId":"15968954638794962",
     * "cardType":"身份证","
     * cardNo":"343343554654",
     * "userName":"撒撒旦",
     * "userSex":"女",
     * "userAge":"29",
     * "userRole":"生产、运输设备操作人员及有关人员"},
     * {….}
     * ]
     * 无数据时：
     * []
     *
     * select * from user
     * #limit (当前页码-1)*每页条数,每页条数
     * limit 10,5;
     */
    List<User> selectUserPage(
            @Param("userName")
            String userName,
            @Param("userSex")
            String userSex,
            @Param("startRow")  //算好的起始行的值
            int startRow);

    /**
     * url /user/createUser(参数见下面)
     * 参数
         * cardType: this.ruleForm.cardType,//证件类型
         * cardNo: this.ruleForm.cardNo,//证件号码
         * userName: this.ruleForm.userName,//用户姓名
         * userSex: this.ruleForm.userSex,//用户性别
         * userAge: this.ruleForm.userAge,//用户年龄
         * userRole: this.ruleForm.userRole,//用户角色
     * 结果  增加成功时：
     * 1
     * 增加失败时:
     * 0
     */
    int createUser(User user);

    /**
     * url /user/ deleteUserById?userId= 15968162087363060
     * 参数
     *      userId:删除用户的id
     * 结果
     * 删除成功时：
     * 1
     * 删除失败时:
     * 0
     */
    int deleteUserById(String userId);

    /**
     * url /user/getRowCount?userName=z&userSex=男
     * 参数
     *      userName:表单中用户名称
     *      userSex:表单中用户性别
     * 结果  有数据时:
     * 12
     * 无数据时:
     * 0
     */
    int getRowCount(
            @Param("userName")
            String userName,
            @Param("userSex")
            String userSex);
}
```

---

```xml
<?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.bjpowernode.mapper.UserMapper">
    <!--完成实体类与表中列名的映射
        private String userId;
        private String cardType;
        private String cardNo;
        private String userName;
        private String userSex;
        private String userAge;
        private String userRole;
    -->
    <resultMap id="usermap" type="user">
        <id property="userId" column="user_id"></id>
        <result property="cardType" column="card_type"></result>
        <result property="cardNo" column="card_no"></result>
        <result property="userName" column="user_name"></result>
        <result property="userSex" column="user_sex"></result>
        <result property="userAge" column="user_age"></result>
        <result property="userRole" column="user_role"></result>
    </resultMap>

    <!--定义全部列名-->
    <sql id="allColumns">
        user_id,card_type,card_no,user_name,user_sex,user_age,user_role
    </sql>

    <!--
      List<User> selectUserPage(
            @Param("userName")
            String userName,
            @Param("userSex")
            String userSex,
            @Param("startRow")  //算好的起始行的值
            int startRow);
    -->
    <select id="selectUserPage" resultMap="usermap">
        select <include refid="allColumns"></include>
        from user
        <where>
            <if test="userName != null and userName != ''">
                and user_name like concat('%',#{userName},'%')
            </if>
            <if test="userSex != null and userSex != ''">
                and user_sex = #{userSex}
            </if>
        </where>
        limit #{startRow},5
    </select>

    <!--
      int createUser(User user);
    -->
    <insert id="createUser" parameterType="user">
        insert into user values(#{userId},#{cardType},#{cardNo},#{userName},#{userSex},#{userAge},#{userRole})
    </insert>

    <!--
      int deleteUserById(String userId);
    -->
    <delete id="deleteUserById" parameterType="string">
        delete from user where user_id = #{userId}
    </delete>

    <!--
      int getRowCount(
            @Param("userName")
            String userName,
            @Param("userSex")
            String userSex);
    -->
    <select id="getRowCount" resultType="int">
        select count(*)
        from user
        <where>
            <if test="userName != null and userName != ''">
                and user_name like concat('%',#{userName},'%')
            </if>
            <if test="userSex != null and userSex != ''">
                and user_sex = #{userSex}
            </if>
        </where>
    </select>

</mapper>
```

## (9) 创建业务逻辑层UserService接口和实现类

```java
/**
 *
 */
public interface UserService {
    /**
     * url /user/selectUserPage?userName=z&userSex=男&page=null
     */
    List<User> selectUserPage(String userName,String userSex,int startRow);

    /**
     * /user/createUser(参数见下面)
     */
    int createUser(User user);

    /**
     * user/ deleteUserById?userId= 15968162087363060
     */
    int deleteUserById(String userId);
    /**
     * /user/getRowCount?userName=z&userSex=男
     */
    int getRowCount(String userName,String userSex);
}
```

---

```java
/**
 *
 */
@Service
public class UserServiceImpl implements UserService {

    //切记切记:一定会有数据访问层的对象
    @Autowired
    UserMapper userMapper;

    @Override
    public List<User> selectUserPage(String userName, String userSex, int startRow) {
        return userMapper.selectUserPage(userName,userSex,startRow);
    }

    @Override
    public int createUser(User user) {
        return userMapper.createUser(user);
    }

    @Override
    public int deleteUserById(String userId) {
        return userMapper.deleteUserById(userId);
    }

    @Override
    public int getRowCount(String userName, String userSex) {
        return userMapper.getRowCount(userName,userSex);
    }
}
```



## (10) 创建测试类进行功能测试

```java
/**
 *
 */
//启动spring容器
@RunWith(SpringJUnit4ClassRunner.class)
//获取配置文件
@ContextConfiguration(locations = {"classpath:applicationContext_mapper.xml","classpath:applicationContext_service.xml"})
public class MyTest {

    @Autowired
    UserService userService;

    @Test
    public void testSelectUserPage(){
        List<User> list = userService.selectUserPage("三","男",0);
        list.forEach(user -> System.out.println(user));
    }

    @Test
    public void testDeleteUserById(){
       int num = userService.deleteUserById("15968162087363060");
        System.out.println(num);
    }
    @Test
    public void testGetRowCount(){
        int num = userService.getRowCount(null,"男");
        System.out.println(num);
    }

    @Test
    public void testCreateUser(){
        User u = new User("125412145214547846","身份证","121451245784","哈哈","男","23","工人");
        int num = userService.createUser(u);
        System.out.println("-----"+num);
    }
}
```

## (11)jdbc.properties

```properties
jdbc.driverClassName=com.mysql,cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmuser?useUnicode=true&characterEncoding=utf8&serverTimezone=Asia/Shanghai
jdbc.username=root
jdbc.password=root123
```

# 跨域问题

## 什么是跨域？

**浏览器**从一个域名的网页去**请求另一个域名的资源**时，域名、端口、协议任一不同，都是跨域.

域名：

　主域名不同 http://www.baidu.com/index.html -->http://www.sina.com/test.js

　子域名不同 http://www.666.baidu.com/index.html -->http://www.555.baidu.com/test.js

　域名和域名ip http://www.baidu.com/index.html -->http://180.149.132.47/test.js

端口：

　http://www.baidu.com:8080/index.html–> http://www.baidu.com:8081/test.js

协议：

　http://www.baidu.com:8080/index.html–> https://www.baidu.com:8080/test.js



## 注意：

　1、端口和协议的不同，只能通过后台来解决

　2、localhost和127.0.0.1虽然都指向本机，但也属于跨域

​	3、另外一种解决方案是在控制器上添加@CrossOrigin注解.

​	4、或者自定义过滤器,进行跨域处理.





# Vue实现前台功能



Element UI官网地址:

https://element.eleme.cn/#/zh-CN/component/installation

Element UI是Vue使用的前端的框架,通过官网可以自行学习.



安装node.js

查看版本编号

node -v

npm –v

node.js的安装是为了使当前的计算机使用vue的框架,预安装的工具.有点类似于运行java程序时必须安装JDK一样的道理.



(2)构建项目

使用命令行进入到当前要运行的vue的项目的目录下,运行以下命令进行项目搭建.

npm i element -ui -S 下载elementUI的框架

npm install //打包项目

npm install --save vue-axios //下载跨域访问组件axios



(3)为idea添加vue插件

---

![image-20220512100918137](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737738.png)

---



(4)配置启动项

---

![image-20220512100959671](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737627.png)

---



(5)项目结构

---

![image-20220512101022896](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261737892.png)

---





build 	项目构建(webpack)相关代码

config 	配置目录，包括端口号等。我们初学可以使用默认的。

node_modules 	npm 加载的项目依赖模块

src 	

这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：

  assets: 放置一些图片，如logo等。

  components: 目录里面放了一个组件文件，可以不用。

  App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。

  main.js: 项目的核心文件。

 

static 	静态资源目录，如图片、字体等。

test 	初始测试目录，可删除

.xxxx文件 	这些是一些配置文件，包括语法配置，git配置等。

index.html 	首页入口文件，你可以添加一些 meta 信息或统计代码啥的。

package.json 	项目配置文件。

README.md 	项目的说明文档，markdown 格式





## UserHome.vue解读

UserHome.vue是所有功能实现的组件.与后台跨域访问,分页显示数据,并实现增加,按主键删除,批量删除,更新,多条件查询等功能.

A.首先确定钩子函数中的启动访问函数

​	 created() {

​	  this.handlePageChange();      ===>分页

​	  this.getRowCount();         ===>计算总行数

​	 }

B.分页函数解析

~~~js
	 handlePageChange() {

		 //定义变量,封装将要提交的数据

	   let postData=this.qs.stringify({


	    page:this.currentPage,


	    userName:this.formInline.search1,


	    userSex:this.formInline.search2



	   });


	   this.$axios({           ==>发出跨域访问的请求,参考$.ajax();


	    method:'post',         ==>请求提交的方式


	    url:'/api/user/selectUserPage',==>服务器的地址


	    data:postData  //this.qs.stringify==>{"page":1,"userName":"","userSex":""}                ==>提交的数据


	   }).then(response=>{        ==>成功后进入到这里


	    this.tableData=response.data;  ==>数据绑定,返回的5个用户的json数据,一下子绑定给表格


	   }).catch(error=>{         ==>出错了进入到这里


	    console.log(error);


	   })


	  }
~~~





 C.计算总行数函数分析

~~~js
   getRowCount() {

	   //创建变量,提取文本框和下拉列表框中的数据

	   let postData=this.qs.stringify({

    userName:this.formInline.search1,

	    userSex:this.formInline.search2

	   });

	   this.$axios({          ==>发出跨域访问的请求,参考$.ajax();

	    method:'post',        ==>请求提交的方式

	    url:'/api/user/getRowCount', ==>服务器的地址

	    data:postData         ===>提交的数据

	   }).then(response=>{

	    this.total=response.data;   ==>返回的总行数赋值给变量total

	   }).catch(error=>{

	    console.log(error);

	   })

	  },
~~~



D.按主键删除分析

~~~js
 //弹出提示框,让用户确定是否删除 

   this.$confirm('删除操作, 是否继续?', '提示', {

   confirmButtonText: '确定',

    cancelButtonText: '取消',

    type: 'warning'            ==>黄色的警告图标

   }).then(() => {             ==>用户点击确定后进入到这里

    let postData = this.qs.stringify({  ==>封装成JSON数据格式

     userId: row.userId,         ==>将要提交的主键值

    });

    this.$axios({            ==>发出跨域访问的请求,参考$.ajax();

     method: 'post',           ==>请求提交的方式

     url: '/api/user/deleteUserById',

    data: postData  //{"userId":15968162893439470}

    }).then(response => {         ==>跨域请求成功后进入这里

     this.getRowCount();   ==>计算删除后的总行数,进行分页插件的页码变化

     ...

     this.handlePageChange();       ==>删除后重新分页

     this.$message({           ==>删除成功后弹框

      type: 'success',

     message: '删除成功!'

    });      

    }).catch(error => {

     console.log(error);

    });

 

   }).catch(() => {             ===>用户点击取消后进入到这里

   this.$message({

     type: 'info',

    message: '已取消删除'

    });

   });

  }
~~~





# 总结

---

![SpringMVC](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261733493.png)

---

