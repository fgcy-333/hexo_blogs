---
title: 动力Spring
date: 2022-08-25 23:00:50
tags:
- 动力节点-张啊荣-Spring
categories: 
- 视频学习笔记
---





# Spring框架是什么

- Spring是于2003年兴起的一个**轻量级**的Java开发框架,它是为了**解决企业应用开发的复杂性而创建的**。
- Spring的**核心是控制反转(loc)和面向切面编程(AOP)** 
- Spring是可以在Java SE/EE中使用的轻量级开源框架。 

- Spring 的主要作用就是为代码“解耦”，降低代码间的耦合度。
- 就是让**对象和对象（模块和模块）之间关系不是使用代码关联**，而是**通过配置来说明**。即在 Spring 中说明对象（模块）的关系。

- Spring 根据代码的功能特点，使用 **Ioc 降低业务对象之间耦合度**。
- Ioc使得主业务在相互调用过程中，不用再自己维护关系了，即**不用再自己创建要使用的对象**了。
- 而是由 **Spring 容器统一管理**，自动“注入”,注入即赋值。
- 而 AOP 使得系统级服务得到了最大复用，且不用再由程序员手工**将系统级服务“混杂”到主业务逻辑中**了，而是**由 Spring 容器统一完成“织入”**。



- 它是整合其它框架的框架.
- Spring 是存储对象的容器.
- 它的核心是IOC和AOP.
- 它由20多个模块构成.它在很多领域都提供优秀的解决方案

<!--more-->

# Spring的优点



## 1.0 轻量

**Spring 核心功能的所需的 jar 总共在 3M 左右**。

Spring 框架运行占用的资源少，运行效率高。不依赖其他 jar。

**非入侵的**，maven那种约定大于配置就是入侵的，非要规定目录结构，文件存放位置



## 2.0 面向接口编程，解耦合

Spring 提供了 Ioc 控制反转，由容器管理对象，对象的依赖关系。

原来在程序代码中的对象创建方式，现在由容器完成。对象之间的依赖解耦合。

 使用接口,就是面向灵活,项目的可扩展性,可维护性都极高.接口不关心实现类的类型.使用时接口指向实现类,切换实现类即可切换整个功能.



## 3.0 AOP编程的支持

通过 Spring 提供的 AOP 功能，方便进行面向切面的编程，许多不容易用传统 OOP 实现的功能可以通过 AOP 轻松应付

在 Spring 中，开发人员可以从繁杂的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量。



就是将公共的,通用的,重复的代码单独开发,在需要的时候反织回去.底层的原理是动态代理



## 4.0 方便集成各种优秀框架

​		Spring 不排斥各种优秀的开源框架，相反 Spring 可以降低各种框架的使用难度

​		Spring 提供了对各种优秀框架（如 Struts2,Hibernate、MyBatis）等的直接支持。简化框架的使用。

​		Spring 像插线板一样，其他框架是插头，可以容易的组合到一起。需要使用哪个框架，就把这个插头放入插线板。不需要可以轻易的移除。



  它整合后使其它框架更易用.



# Spring体系结构



----

![image-20220825230311461](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230311461.png)

---



Spring 由 20 多个模块组成它们可以分为

数据访问/集成（Data Access/Integration）

Web

面向切面编程（AOP, Aspects）

提供JVM 的代理（Instrumentation）

消息发送（Messaging）

核心容器（Core Container）

测试（Test）





# 控制反转

## 1.0 特点

- 控制反转IOC(Inversion of Control） 是一种**思想**。
- **对象调用权交给容器**，通过**容器来实现对象的装配和管理**。
- 控制反转就是对**对象控制权的转移**，从程序代码本身反转到了外部容器。
- 通过容器实现对象的创建，属性赋值， 依赖的管理
- IOC实现方式多种多样。当前比较流行的实现方式是依赖注入。应用广泛。



由Spring容器进行对象的创建和依赖注入.程序员在使用时直接取出使用



## 2.0 **反转**



- 实体类Student依赖School

~~~java
public class Student {
    private String name;
    private int age;

    //引用类型的成员变量,学生所在的学校
    private School school;

    public Student() {
        System.out.println("学生的无参的构造方法..........");
    }
    
  //交给Spring容器注入值,必须提供setXXX()方法，以及无参构造 【设值注入】
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setSchool(School school) {
        this.school = school;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", school=" + school +
                '}';
    }
}

~~~



- 实体类School被Student依赖

~~~java
package com.bjpowernode.pojo2;

/**
 *
 */
public class School {
    private String name;
    private String address;

    //为了给成员变量注入值,必须提供无参构造方法和setXXX()方法 【设值注入】
    public void setAddress(String address) {
        this.address = address;
    }

    public void setName(String name) {
        this.name = name;
    }

    public School() {
        System.out.println("学校的无参的构造方法被调用..........");
    }

    @Override
    public String toString() {
        return "School{" +
                "name='" + name + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

~~~



- spring的核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">



    <!--创建学生对象-->
    <bean id="stu" class="com.bjpowernode.pojo2.Student" autowire="byName">
        <property name="name" value="李四"></property>
        <property name="age" value="22"></property>
        <!--真正的依赖注入，依赖的是beanFactory中的bean-->
        <property name="school" ref="school"></property>
    </bean>
    
    <!--创建学校对象-->
    <bean id="school" class="com.bjpowernode.pojo2.School">
        <property name="name" value="清华大学"></property>
        <property name="address" value="海淀区"></property>
    </bean>
    
</beans>
~~~



- 测试类

~~~java
public class MyTest2 {
    
    
    @Test
    public void testStudent(){
        //正转
        //程序员创建对象
        Student stu = new Student();
        System.out.println(stu);
    }

    @Test
    public void testStudent(){
       //反转
       //由spring容器进行对象的创建
       //如果想从spring容器中取出对象,则要先创建容器对象,并启动才可以取对象.
        //通过加载spring核心配置文件，创建spring容器【容器启动完就会创建好全部由spring管理的对象】
        ApplicationContext ac = new ClassPathXmlApplicationContext("s02/applicationContext.xml");
        //取出学校对象
        School school = (School) ac.getBean("school");
        System.out.println(school);
    }
    @Test
    public void testStudent2(){
        //创建容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("s02/applicationContext.xml");
        //取出学校对象
        Student stu = (Student) ac.getBean("stu");
        System.out.println(stu);
    }
}

~~~

**依赖**：classA 类中含有 classB 的实例，在 classA 中调用 classB 的方法完成功能，即 classA对 classB 有依赖。

**切记:Spring容器在启动时,就创建所有的对象**



## 3.0 IOC的实现：

- 什么是依赖注入

依赖注入 DI 是**指程序运行过程中，若需要调用另一个对象协助时，无须在代码中创建被调用者**，而是依赖于外部容器，由外部容器创建后传递给程序



Spring 的依赖注入对调用者与被调用者几乎没有任何要求，完全支持对象之间依赖关系的管理。

**Spring 框架使用依赖注入（DI）实现 IOC**

Spring 容器是一个超级大工厂，负责创建、管理所有的 Java 对象，这些 Java 对象被称为 Bean。

**Spring 使用“依赖注入”的方式来管理 Bean 之间的依赖关系**。使用 IOC 实现对象之间的解耦和。



# 项目案例【一】普通的三层架构

## 1.0 使用三层架构进行用户的插入操作

- 界面层:接收客户端的数据，调用业务层处理数据，返回结果给客户端
- 业务逻辑层 ：处理整个项目的业务逻辑，向下操作数据访问层并获取信息，向上接收客户端的信息并返回结果
- 数据访问层：专门用于对数据库进行增删查改，向上为业务逻辑层提供结果(模拟)



## 2.0 视图层

~~~java
public class UsersController {

    //如何去访问业务逻辑层,就是创建对象
    //切记切记:所有的界面层都会有业务逻辑层的对象
    public UsersService usersService = new UsersServiceImpl();

    //界成层的功能实现,对外提供访问的功能
    public int insert(Users users){
        return usersService.insert(users);
    }
}
~~~



## 3.0 业务逻辑层

- 接口

~~~java
public interface UsersService {

    //增加用户
    int insert(Users users);
}
~~~

- 实现类

~~~java
/**
 *  业务逻辑层的实现类
 */
public class UsersServiceImpl implements UsersService {

    //切记切记:在所有的业务逻辑层中都必定有数据访问层的对象
    private UsersMappr usersMappr = new UsersMapperImpl();

    @Override
    public int insert(Users users) {
        //添加更复杂的业务,但是我们现在没有复杂业务
        return usersMappr.insert(users);
    }
}
~~~



## 4.0 数据访问层

- 接口

~~~java
public interface UsersMappr {
    //增加用户
    int insert(Users u );
}
~~~



- 实现类

~~~Java
/**
 *  数据访问层的实现类
 */
public class UsersMapperImpl implements UsersMappr {
    @Override
    public int insert(Users u) {
        System.out.println(u.getUname()+"用户增加成功!");
        return 1;
    }
}
~~~



## 5.0 实体类

~~~java
public class Users {
    private int uid;
    private String uname;
    private int uage;
	//getter、setter、有参、无参、toString
}
~~~



## 6.0 依赖文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bjpowernode</groupId>
  <artifactId>spring_003_sancengxml</artifactId>
  <version>1.0</version>
  

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
      
    <!--添加spring的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
  </dependencies>


</project>

~~~



## 7.0 测试类

~~~java
public class MyTest {
    @Test
    public void testInsertUsers(){
        //?创建谁的对象  创建UsersController对象
        UsersController usersController = new UsersController();
        int  num = usersController.insert(new Users(100,"张三",22));
        System.out.println(num);
    }
}
~~~





# 使用Spring创建非自定义的类

spring 配置文件加入 java.util.Date 定义：

~~~xml
<bean id="myDate" class="java.util.Date" />
~~~



MyTest 测试类中：

调用 `ac.getBean(“myDate”);  `获取日期类对象



# ApplicationContext 接口（容器）

ApplicationContext 用于加载 Spring 的配置文件，在程序中充当“容器”的角色

其实现类有两个：

---

![image-20220825230423186](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230423186.png)

---



若 Spring 配置文件存放在项目的类路径下，则使用 ClassPathXmlApplicationContext 实现类进行加载。

~~~java
Application ac = new ClassPathXmlApplication("application.xml");
~~~



## ApplicationContext 容器中对象的装配时机

ApplicationContext 容器，会**在容器对象初始化时，将其中的所有对象一次性全部装配好**。

以后代码中若要使用到这些对象，只需从内存中直接获取即可。执行效率较高。但占用内存

Spring初始化对象时要使用无参的构造方法，切记保证类中有无参构造方法。



## 使用 spring 容器创建的 java 对象



---

![image-20220825230438076](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230438076.png)

---



# Spring创建对象的作用域

 Spring容器创建的对象默认的作用域是单例模式的

单例模式的目的就是无论访问多少次,得到的都是同一个对象

我们可以通过创建系统时间来验证Spring创建对象的默认单例模式.



验证单例

~~~xml
<bean id="mydate" class="java.util.Date" scope="singleton"> ===>单例模式
<!--多次获取mydate对象，如果值相同则说明是单例-->
</bean>
~~~

~~~java
@Test
public void testSpringStudent()throws Exception{
    //创建容器对象并启动.自动完成容器中所有对象的创建,默认调用无参的构造方法.
    //如果没有提供无参的构造方法,则容器炸掉
   ApplicationContext ac = new ClassPathXmlApplicationContext("s04/applicationContext.xml");
    Date date1 = (Date) ac.getBean("mydate");
    System.out.println("第一次取出的对象:"+date1);
    System.out.println("********************");
    Thread.sleep(3000);
    Date date2 = (Date) ac.getBean("mydate");
    System.out.println("第二次取出的对象:"+date2);
    System.out.println(date1==date2);
}
~~~

---

![image-20220825230454889](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230454889.png)

---





可以设置为非单例的方式

~~~xml
<bean id="mydate" class="java.util.Date" scope="prototype">   ===>非单例模式
    <!--<property name="time" value="1234567891011"></property>-->
</bean>
~~~





# 什么是注入

- **bean 实例在调用无参构造器创建对象后，就要对 bean 对象的属性进行初始化**。
- **由容器对bean的属性进行初始化，称为注入。**



Spring 使用“依赖注入”的方式来管理 Bean 之间的依赖关系



# 注入分类

根据注入方式的不同，常用的有两类：set 注入、构造注入



# set注入【设值注入】

概念

- set 注入也叫**设值注入**是指，通过 setter 方法传入被**调用者的实例**。
- 这种注入方式简单、直观，因而在 Spring 的依赖注入中大量使用。



## 两种类型

- 普通类型

​	  所有不在beanFactory中的bean

​	  常见的有八种基本数据类型及其包装类、String、Date........

- 引用类型

​		当指定 bean 的某**属性值**为另一 **bean 的实例**时，通过 ref 指定它们间的引用关系。

​		**ref 的值必须为某 bean 的 id 值**



## 1 简单类型

---

![image-20220825230553582](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230553582.png)

---



**还可以创建系统类的对象并赋值**

---

![image-20220825230613314](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230613314.png)

---



## 2 引用类型

当指定 bean 的某属性值为另一 bean 的实例时，通过 ref 指定它们间的引用关系。ref 的值必须为某 bean 的 id 值

---

![image-20220825230640271](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230640271.png)

---

~~~xml
<bean id="student" class="com.bjpowernode.ba02.Student">
    <!--简单类型注入注入-->
	<properties name="name" value="学校"/>
    <properties name="age" value="18"/>
       <!--引用类型注入 【DI】-->
    <properties name="name" ref="mySchool"/>
</bean>
~~~



## **引用类型属性自动注入【基于xml】**

​		对于引用类型属性的注入，也**可不在配置文件中显式**的注入。

​		可以通过在\<bean/>标签设置 autowire 属性值，为引用类型属性进行隐式自动注入（默认是不自动注入引用类型属性）。

​		根据自动注入判断标准的不同

可以分为两种：

### byName：根据名称自动注入

---

![image-20220825230705363](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230705363.png)

---



### byType方式自动注入

---

![image-20220825230723678](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230723678.png)

---

使用 byType 方式自动注入，要求：配置文件中被调用者 bean 的 class 属性指定的类， 要与代码中调用者 bean 类的某引用类型属性类型**同源**。

要么相同，要么有 is-a 关系（子类，或是实现类）。**【本类及其子孙类】**

这样的**同源的被调用 bean 只能有一个**。**多于一个**，容器就不知该匹配哪一个了。此时**需要使用byName方式自动注入**



- 实体类1

~~~java
public class Student {

    private String name;
    private int age;

    //引用类型的成员变量
    private School school;

	//必须要有set方法，和无参构造
    //让容器可以调用无参构造进行初始化对象，调用set方法进行属性的初始化【DI】
}
~~~

- 实体类2

~~~java
package com.bjpowernode.pojo3;

/**
 *
 */
public class School {
    private String name;
    private String address;
    
	//必须要有set方法，和无参构造
    //让容器可以调用无参构造进行初始化对象，调用set方法进行属性的初始化【DI】

}

~~~

- 配置类

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


  
	<!--创建学生对象 启用了引用类型的自动注入【byName】-->
    <bean id="stu" class="com.bjpowernode.pojo2.Student" autowire="byName">
        <property name="name" value="李四"></property>
        <property name="age" value="22"></property>
        
        <!--这个可有可无-->
        <property name="school" ref="school"></property>
    </bean>
    
    <!--创建学校对象 【与顺序无关，只要容器中有】-->
    <bean id="school" class="com.bjpowernode.pojo2.School">
        <property name="name" value="清华大学"></property>
        <property name="address" value="海淀区"></property>
    </bean>
    
    </beans>
~~~



- 测试类

~~~java
    @Test
    public void testStudent2(){
        //创建容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("s02/applicationContext.xml");
        //取出学校对象
        Student stu = (Student) ac.getBean("stu");
        System.out.println(stu);
    }
~~~

# 构造器注入

### 概念

- 构造注入是指，在**构造调用者实例的同时，完成被调用者的实例化。**

- 即 使用构造器设置依赖关系。在实体类中必须提供相应参数的构造方法。



\<constructor-arg />标签中用于指定参数的属性有：

- name：指定参数名称。

- index：指明该参数对应着构造器的第几个参数

  从 0 开始。不过，该属性不要也行，

  但要注意，若参数类型相同，或之间有包含关系，则需**要保证赋值顺序要与构造器中的参数顺序一致**。



## 基于xml配置文件的IOC

### 1.0  使用构造方法的参数名称注入值

- 实体类

~~~java
package com.bjpowernode.pojo3;

@Component//可以创建任意对象【将该类交给spring容器管理】.创建的对象的默认名称是类名的小驼峰命名法.也可以指定对象的名称@Component("指定名称").
public class School {
    private String name;
    private String address;

    //使用带参构造方法注入值
    //构造器中的入参名字，决定了xml配置的构造器注入的名字
    public School(String name3, String address1) {
        this.name = name3;
        this.address = address1;
    }
    
}
~~~



- 配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--创建学校的对象,使用构造方法参数名称注入值-->
    <bean id="school" class="com.bjpowernode.pojo3.School">
        
        <!--构造器注入，名字与实体类有参构造的入参名字相同-->
        
        <!--与设值注入不同，构造器注入不需要set方法与无参构造，因为这种方法在创建对象的时候，进行初始化操作-->
        
        <constructor-arg name="address1" value="海淀区"></constructor-arg>
        
        <constructor-arg name="name3" value="清华大学"></constructor-arg>
        
    </bean>
</beans>
~~~



- 测试类

~~~java
   @Test
    public void testSchool(){
       ApplicationContext ac = new ClassPathXmlApplicationContext("s03/applicationContext.xml");
       School school = (School) ac.getBean("school");
       System.out.println(school);
   }
~~~



### 2.0  使用构造方法的参数索引下标注入值

- 实体类

~~~java
public class Student {

    private String name;
    private int age;

    //引用类型的成员变量
    private School school;

    public Student(String name, int age, School school) {
        this.name = name;
        this.age = age;
        this.school = school;
    }
}
~~~



- Spring核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
<!--创建学生对象,使用构造方法的参数的下标注入值-->
    <bean id="stu" class="com.bjpowernode.pojo3.Student">
        <constructor-arg index="0" value="钱七"></constructor-arg>
        <constructor-arg index="2" ref="school"></constructor-arg>
        <constructor-arg index="1" value="22"></constructor-arg>
    </bean>
</beans>
~~~



- 测试类

~~~java
    @Test
    public void testStudent(){
        ApplicationContext ac = new ClassPathXmlApplicationContext("s03/applicationContext.xml");
       Student stu = (Student) ac.getBean("stu");
        System.out.println(stu);
    }
~~~



### 3.0 不指定名称和下标索引的注入

- 实体类

~~~java
public class Student {

    private String name;
    private int age;

    //引用类型的成员变量
    private School school;

    public Student(String name, int age, School school) {
        this.name = name;
        this.age = age;
        this.school = school;
    }
}
~~~



- Spring核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
    <!--创建学生对象,使用默认的构造方法的参数顺序-->
    <!--使用该方法一定要严格按照顺序配置参数，上面两个可以不按顺序-->
    <bean id="stuSequence" class="com.bjpowernode.pojo3.Student">
        <constructor-arg value="陈十"></constructor-arg>
        <constructor-arg value="22"></constructor-arg>
        <constructor-arg ref="school"></constructor-arg>
    </bean>

</beans>
~~~



- 测试类

~~~java
    @Test
    public void testStudent(){
        ApplicationContext ac = new ClassPathXmlApplicationContext("s03/applicationContext.xml");
       Student stu = (Student) ac.getBean("stu");
        System.out.println(stu);
    }
~~~



## 4.0 注入系统的类



---

![image-20220825230808795](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230808795.png)

---



# 项目案例【二】使用spring改造三层架构

使用三层架构完成用户数据的增加操作.由Spring容器负责对象的创建与依赖注入.

在分层开发中，Spring管理controller,service,dao各层的实现类对象的创建及依赖管理。



## 2.0 视图层

~~~java
public class UsersController {

    //如何去访问业务逻辑层,就是创建对象
    //切记切记:所有的界面层都会有业务逻辑层的对象
    public UsersService usersService ; //= new UsersServiceImpl();

    //交给Spring去注入值,必须提供setXXX()方法，无参构造默认就会只有
    public void setUsersService(UsersService usersService) {
        this.usersService = usersService;
    }

    //界成层的功能实现,对外提供访问的功能
    public int insert(Users users){
        return usersService.insert(users);
    }
}
~~~



## 3.0 业务逻辑层



- 接口

~~~java
public interface UsersService {

    //增加用户
    int insert(Users users);
}
~~~



- 实现类

~~~java
public class UsersServiceImpl implements UsersService {

    //切记切记:在所有的业务逻辑层中都必定有数据访问层的对象
    private UsersMappr usersMappr ;//= new UsersMapperImpl();

    //交给Spring去依赖注入值,必须提供setXXX()方法

    public void setUsersMappr(UsersMappr usersMappr) {
        this.usersMappr = usersMappr;
    }

    @Override
    public int insert(Users users) {
        //添加更复杂的业务,但是我们现在没有复杂业务
        return usersMappr.insert(users);
    }
}
~~~



## 4.0 数据访问层

- 接口

~~~java
public interface UsersMappr {
    //增加用户
    int insert(Users u );
}
~~~



- 实现类

~~~Java
/**
 *  数据访问层的实现类
 */
public class UsersMapperImpl implements UsersMappr {
    @Override
    public int insert(Users u) {
        System.out.println(u.getUname()+"用户增加成功!");
        return 1;
    }
}
~~~



## 5.0 实体类

~~~java
public class Users {
    private int uid;
    private String uname;
    private int uage;
	//getter、setter、有参、无参、toString
}
~~~



## 6.0 Spring核心配置文件

- 在 `src/main/resources/`目录现创建一个xml 文件，文件名可以随意

- 但 Spring 建议的名称为` applicationContext.xml`。

- spring 配置中需要加入约束文件才能正常使用，约束文件是 xsd 扩展名。

- \<bean />：用于定义一个实例对象。一个实例对应一个 bean 元素。

- id：该属性是 Bean 实例的唯一标识，程序通过 id 属性访问 Bean，Bean 与 Bean 间的依赖关系也是通过 id 属性关联的。

- class:指定该Bean所属的类，注意这里只能是类，不能是接口。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--    使用xml的方法，通过核心配置文件，创建各种对象-->

    <!--创建数据访问层对象-->
    <bean id="uMapper" class="com.bjpowernode.dao.UsersMapperImpl"></bean>

    <!--创建业务逻辑层对象-->
    <bean id="uService" class="com.bjpowernode.service.impl.UsersServiceImpl">
        
        <!--依赖注入：将容器中管理的对象赋值给某个对象的属性-->
        <property name="usersMappr" ref="uMapper"/>
    </bean>

    <!--创建视图层的对象-->
    <bean id="uController" class="com.bjpowernode.controller.UsersController">
            <!--依赖注入：将容器中管理的对象赋值给某个对象的属性-->
        <property name="usersService" ref="uService"></property>
    </bean>
</beans>
~~~



## 7.0 测试类

~~~java
public class MyTest {
    @Test
    public void testInsertUsers(){
        //通过spring的配置文件创建容器并启动
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicatoinContext_controller.xml");
        //按名字取出对象取出对象【byName】
        UsersController usersController = (UsersController) ac.getBean("uController");
        //测试功能
        int num = usersController.insert(new Users(200,"王五",24));
        System.out.println(num);
    }
}
~~~



## 8.0 pom

~~~xml
properties>
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
<!--    添加spring的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
  </dependencies>
~~~



# 基于注解的DI

## 概念

​		依赖注入：DI(Dependency Injection)，对于 DI 使用注解，将不再需要在 Spring 配置文件中声明bean 实例。

​		Spring 中使用注解， 需要在原有 Spring 运行环境基础上再做一些改变。

​		需要在 Spring 配置文件中配置组件扫描器，用于在指定的基本**包中扫描**注解。



## 指定多个包扫描的三种方式

​		1)使用多个 context:component-scan 指定不同的包路径

---

![image-20220825230826498](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230826498.png)

---



​		2)指定base-package的值使用分隔符   分隔符可以使用逗号（，）或分号（;），还可以使用空格，不建议使用空格。

---

![image-20220825230845913](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/image-20220825230845913.png)

---



​		3)base-package 是指定到父包名

​				base-package 的值表是基本包，容器启动会扫描包及其子包中的注解，当然也会扫描到子包下级的子包

​				所以 base-package 可以指定一个父包就可以。

​				但不建议使用顶级的父包，扫描的路径比较多，导致容器启动时间变慢。

​				指定到目标包和合适的。也就是注解所在包全路径。例如注解的类在 com.bjpowernode.beans 包中



---

![image-20220825224535006](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220825224535006.png)

---



## 常用注解

### 	创建对象注解

​	 	    @Component :创建所有对象都可以使用此注解,除了控制器,业务逻辑层,数据访问层的对象

​			@Controller:创建控制器层的对象,此对象可以接收用户请求,返回处理结果

​			@Service:创建业务逻辑层的对象,此对象可施事务控制,向上给控制器返回数据,向下调用数据访问层

​			@Repository:创建数据访问层的对象 ,对数据库中的数据进行增删改查操作

​	

### 	给对象赋值注解

​	     @Value:给简单类型赋值

​		 @Autowired:给引用类型按类型注入

​		 @Qualifier:给引用类型按名称注入



### 	定义Bean的注解@Component掌握）

​			在类上使用注解@Component，该注解的 value 属性用于指定该 bean 的 id 值。

​			@Component:可以**创建任意对象**.创建的对象的默认名称是类名的小驼峰命名法.也可以指定对象的名称@Component("指定名称").
​			@Controller:专门用来**创建控制器的对象**(Servlet),这种对象可以接收用户的请求,可以返回处理结果给客户端.
​			@Service:专门用来**创建业务逻辑层**的对象,负责向下访问数据访问层,处理完毕后的结果返回给界面层.
​			@Repository:专门用来创建**数据访问层**的对象,负责数据库中的增删改查所有操作.



@Repository，@Service，@Controller 是对@Component 注解的细化，标注不同层的对象。即持久层对象，业务层对象，控制层对象。





### 	简单类型属性注入@Value(掌握)

​			需要在属性上使用注解@Value，该注解的 value 属性用于指定要注入的值。

​			**使用该注解完成属性注入时，类中无需 setter**。

​			当然，若**属性有 setter，则也可将其加到 setter 上**。



- 实体类

~~~java
@Component("stu")  //交给Spring去创建对象,就是在容器启动时创建
public class Student {
    @Value("张三")//简单类型值得注入@Value()
    private String name;
    @Value("22")
    private int age;

    public Student() {
        System.out.println("学生对象的无参构造方法.........");
    }
}

~~~



- spring核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.s01"></context:component-scan>
</beans>
~~~



- 测试类

~~~java
    @Test
    public void testStudent(){
        //创建容器对象并启动
        ApplicationContext ac = new ClassPathXmlApplicationContext("s01/applicationContext.xml");
        //取出对象
        Student stu = (Student) ac.getBean("stu");
        System.out.println(stu);
    }

学生对象的无参构造方法.........
Student{name='张三', age=22}
~~~



## **引用类型的自动注入**

对于引用类型属性的注入，也可不在配置文件中显示的注入。

可以通过为\<bean/>标签设置 autowire 属性值，为引用类型属性进行隐式自动注入（默认是不自动注入引用类型属性）。

根据自动注入判断标准的不同，可以分为两种：

byName：根据名称自动注入

byType： 根据类型自动注入





### byType 自动注入@Autowired(掌握)

​			需要在引用属性上使用注解@Autowired，该注解**默认**使用**按类型自动装配 Bean** 的方式。

​			从整个Bean工厂中搜索**同源类型**【**本类及其子孙类**】的对象进行注入.

​			**使用该注解完成属性注入时，类中无需 setter**。@Value()也不用

​			当然，若属性有 setter，则也可将其加到 setter 上。

---

![image-20220512153040034](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512153040034.png)	

---



​	注意：

​		**@Autowired 还有一个属性 required，默认值为 true，表示当匹配失败后，会终止程序运行**。

​		若将其值设置为 false，则匹配失败，将被忽略，未匹配的属性值为 null。

​		**如果可注入的类型多于一个【有父子类型，或接口与实现类类型】,则 按名称 进行二次匹配.如果有匹配到则注入,如果没有匹配到,则报错**。

---

![image-20220512153104017](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512153104017.png)

---



### byName自动注入@Qualifier(了解)

​			需要在引用属性上**联合使用**注解**@Autowired 与@Qualifier**。

​			@Qualifier 的 value 属性用于**指定要匹配的 Bean 的 id 值**，**如果不指定，那么就用 变量名 去匹配bean的id值**。

​			**类中无需 set 方法，如果有也可加到 set 方法上。**

​			当有同源类型时，使用@qualifier就可以确定是哪个实现类了。

​		**如果可注入的类型多于一个,则按名称进行匹配.如果有匹配到则注入,如果没有匹配到,则报错**

---

![image-20220512153134009](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512153134009.png)

---



# 注解@Resource 自动注入(了解)

Spring 提供了对jdk 中@Resource 注解的支持。

@Resource 注解既可以按名称匹配Bean， 也可以按类型匹配 Bean。

默认是按名称注入。使用该注解，要求 JDK 必须是 6 及以上版本。

@Resource 可在属性上，也可在 set 方法上。



## byType 注入引用类型属性

@Resource 注解若不带任何参数，采用默认按名称的方式注入，按名称不能注入 bean， 则会按照类型进行 Bean 的匹配注入。

**注意：上述与@AutoWired相反【互斥】**

---

![image-20220512153602560](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512153602560.png)

---



## byName 注入引用类型属性

@Resource 注解指定其 name 属性，则 name 的值即为按照名称进行匹配的 Bean 的 id。

---

![image-20220512153710561](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512153710561.png)

---





# 注解与 XML 的对比

## 注解优点是：

- 方便
- 直观
- 高效（代码少，没有配置文件的书写那么复杂）。

其弊端也显而易见：以硬编码的方式写入到 Java 代码中，修改是需要重新编译代码的。



## XML 方式优点是：

- 配置和代码是分离的
- 在 xml 中做修改，无需编译代码，只需重启服务器即可将新的配置加载。

xml 的缺点是：编写麻烦，效率低，大型项目过于复杂。



# 为应用指定多个Spring配置文件



- 在实际应用里，随着应用规模的增加，系统中 Bean 数量也大量增加，导致配置文件变得非常庞大、臃肿。

- 为了避免这种情况的产生，提高配置文件的可读性与可维护性，可以将Spring 配置文件分解成多个配置文件。



## 拆分策略

### 按层拆

applicationContext_controller.xml      【视图层】

~~~xml
		<bean id="uController" class="com.bjpowernode.controller.UsersController">
        <bean id="bController" class="com.bjpowernode.controller.BookController">
~~~

​      applicationContext_service.xml         【业务逻辑层】

 ~~~xml
       <bean id="uService" class="com.bjpowernode.controller.UsersService">
       <bean id="bService" class="com.bjpowernode.controller.BookService">
 ~~~

​      applicationContext_mapper.xml        【数据访问层】

```xml
    <bean id="uMapper" class="com.bjpowernode.controller.UsersMapper">
    <bean id="bMapper" class="com.bjpowernode.controller.BookMapper">

```

### 按功能【模块】拆

​	applicationContext_users.xml  

~~~xml
    <bean id="uController" class="com.bjpowernode.controller.UsersController">
    <bean id="uService" class="com.bjpowernode.controller.UsersService">
    <bean id="uMapper" class="com.bjpowernode.controller.UsersMapper">
~~~

  applicationContext_book.xml   

~~~xml
    <bean id="bController" class="com.bjpowernode.controller.BookController">
    <bean id="bService" class="com.bjpowernode.controller.BookService">
    <bean id="bMapper" class="com.bjpowernode.controller.BookMapper">
~~~



## spring核心配置文件的整合

### 1)单个文件导入 

~~~xml
    <import resource="applicatoinContext_mapper.xml"></import>
    <import resource="applicatoinContext_service.xml"></import>
    <import resource="applicatoinContext_controller.xml"></import>
~~~

### 2)批量导入

~~~xml
    <import resource="applicatoinContext_*.xml"></import>
~~~

注意：

​	多个配置文件中有一个总文件，总配置文件将各其它子文件通过\<import>引入。

​	在 Java代码中**只需要使用总配置文件对容器进行初始化**即可。

​	可以使用通配符进行整合。但此时要求父配置文件名**不能满足所能匹配的格式**，否则将出现循环递归包含。

​	就本例而言，父配置文件不能匹配 applicationContext-*.xml 的格式，即不能起名为applicationContext-total.xml。



# 面向切面编程

## 概念

-  AOP（Aspect Orient Programming），面向切面编程。
-  切面:公共的,**通用的,重复的功能称为切面**,**面向切面编程就是将切面提取出来,单独开发,在需要调用的方法中通过动态代理的方式进行织入**.
-  **AOP 底层，就是采用动态代理模式实现的**。采用了两种代理：JDK 的动态代理，与 CGLIB的动态代理。
-  可通过运行期动态代理实现**程序功能的统一维护**的一种技术。
-  AOP 是 Spring 框架中的一个重要内容。利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



面向切面编程，就是将交叉业务逻辑封装成切面，利用 AOP 容器的功能将切面织入到主业务逻辑中。
所谓交叉业务逻辑是指，通用的、与主业务逻辑无关的代码，如安全检查、事务、日志、缓存等。若不使用 AOP，则会出现代码纠缠
即交叉业务逻辑与主业务逻辑混合在一起。这样， 会使主业务逻辑变的混杂不清。
例如，转账，在真正转账业务逻辑前后，需要权限控制、日志记录、加载事务、结束事务等交叉业务逻辑，而这些业务逻辑与主业务逻辑间并无直接关系。
但它们的代码量所占比重能达到总代码量的一半甚至还多。它们的存在，不仅产生了大量的“冗余”代码，还大大干扰了主业务逻辑



## 面向切面编程对有什么好处

1.减少重复；

2.专注业务；

注意：面向切面编程只是**面向对象编程的一种补充**。用 AOP 减少重复代码，专注业务实现。

---

![image-20220512155600613](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512155600613.png)

---



# 模拟AOP框架实现

- 业务:图书购买业务
- 切面:事务



  ## 1)第一个版本:业务和切面紧耦合在一起,没有拆分.

~~~java
/**
 *  图书购买业务和事务切面耦合在一起
 */
public class BookServiceImpl {

    public void buy(){
        try {
            System.out.println("事务开启.......");
            System.out.println("图书购买业务功能实现...........");
            System.out.println("事务提交.......");
        } catch (Exception e) {
            System.out.println("事务回滚.......");
        }
    }
}
~~~

  

## 2)第二个版本:使用子类代理的方式拆分业务和切面.

~~~java
/**
 *  使用子类代理的方式进行图书业务和事务切面的拆分
 */
public class BookServiceImpl {
    //在父类中只有干干净净的业务
    public void buy(){
        System.out.println("图书购买功能实现........");
    }
}
~~~

~~~java
/**
 *  子类就是代理类,将父类的图书购买功能添加事务切面
 */
public class SubBookServiceImpl extends BookServiceImpl {

    @Override
    public void buy() {
        try {
            //事务切面
            System.out.println("事务开启.........");
            //主业务实现
            super.buy();
            //事务切面
            System.out.println("事务提交.........");
        } catch (Exception e) {
            System.out.println("事务回滚.........");
        }
    }
}
~~~

  

## 3)第三个版本:使用静态代理拆分业务和切面.业务和业务接口已拆分.此时切面紧耦合在业务中.

~~~java
/**
 *  业务接口
 */
public interface Service {
    //规定业务功能
     void buy();
}
~~~

~~~java
/**
 *  目标对象:产品业务功能的具体实现
 */
public class ProductServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("商品购买业务实现.........");
    }
}
~~~

~~~java
/**
 *  目标对象:书籍业务功能的具体实现
 */
public class BookServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("图书购买业务功能实现............");
    }
}
~~~

~~~java
/**
 *  静态代理 实现了目标对象的灵活切换
 *  图书购买业务,商品购买业务
 */
public class Agent implements Service {

    //设计成员变量的类型为接口,为了灵活切换目标对象【面向接口编程】
    public Service target;

    //使用构造方法传入目标对象
    public Agent(Service target){
        this.target = target;
    }
    
    @Override
    public void buy() {
        try {
            //切面功能
            System.out.println("事务开启......");  //日志 权限验证
            //业务功能
            target.buy();
            //切面功能
            System.out.println("事务提交.......");
        } catch (Exception e) {
            System.out.println("事务回滚.......");
        }
    }
}
~~~

~~~java
    @Test
    public void test02(){
        Service agent = new Agent(new ProductServiceImpl());
        agent.buy();
    }
~~~

 

## 4)第四个版本:使用静态代理拆分业务和业务接口,切面和切面接口.

---

![image-20220507211056504](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220507211056504.png)

---



~~~java
/**
 *切面接口
 */
public interface AOP {
    default void before(){}
    default void after(){}
    default void exception(){}
}
~~~



~~~java
/**
 *日志的切面实现
 */
public class LogAop implements AOP {
    @Override
    public void before() {
        System.out.println("前置日志输出.......");
    }
}
~~~



~~~java
/**
 *业务接口
 */
public interface Service {
    //规定业务功能
     void buy();
}
~~~



~~~java
/**
 *  图书业务功能的具体实现
 */
public class BookServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("图书购买业务功能实现............");
    }
}
~~~



~~~java
/**
 *产品业务功能的具体实现
 */
public class ProductServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("商品购买业务实现.........");
    }
}
~~~



~~~java
/**
 *代理对象
 */
public class Agent implements Service {

    //传入目标(业务)对象,切面对象
    Service target;
    AOP aop;

    //使用构造方法初始化业务对象和切面对象
    public Agent(Service target,AOP aop){
        this.target = target;
        this.aop = aop;
    }

    @Override
    public void buy() {
        try {
            //切面
            aop.before();  //事务  日志
            //业务
            target.buy();  //图书  商品
            //切面
            aop.after();   //事务

        } catch (Exception e) {
            //切面
            aop.exception();
        }

    }
}
~~~



~~~java
    /**
     * 实现一个业务包含多个切面
     */
    @Test
    public void test02(){
        Service agent = new Agent(new ProductServiceImpl(),new TransAop());
        Service agent1 = new Agent(agent,new LogAop());
        agent1.buy();
    }
~~~



  ## 5)第五个版本:使用动态代理完成第四个版本的优化

~~~java
/**
 *业务接口
 */
public interface Service {
    //规定业务功能
     void buy();

     //增加有参有返回值的方法测试代理功能
    default String show(int age){return null;}
}
~~~



~~~java
/**
 *  图书业务功能的具体实现
 */
public class BookServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("图书购买业务功能实现............");
    }

    @Override
    public String show(int age) {
        System.out.println("这是show()方法被调用...."+age);
        return "abcd";
    }
}
~~~



~~~java
/**
 *产品业务功能的具体实现
 */
public class ProductServiceImpl implements Service {
    @Override
    public void buy() {
        System.out.println("商品购买业务实现.........");
    }
}
~~~



~~~java
/**
 *切面接口
 */
public interface AOP {
    default void before(){}
    default void after(){}
    default void exception(){}
}
~~~



~~~java
/**
 *日志切面实现
 */
public class LogAop implements AOP {
    @Override
    public void before() {
        System.out.println("前置日志输出.......");
    }
}
~~~



~~~java
/**
 *事务切面实现
 */
public class TransAop implements AOP {
    @Override
    public void before() {
        System.out.println("事务开启........");
    }

    @Override
    public void after() {
        System.out.println("事务提交........");
    }

    @Override
    public void exception() {
        System.out.println("事务回滚........");
    }
}

~~~



~~~java
/**
 *生成动态代理工具类
 */
public class ProxyFactory {

    public static Object getAgent(Service target,AOP aop){
        //返回生成的动态代理对象
        return Proxy.newProxyInstance(
                //类加载器
                target.getClass().getClassLoader(),
                //目标对象实现的所有的接口
                target.getClass().getInterfaces(),
                //代理功能实现
                new InvocationHandler() {
                    @Override
                    public Object invoke(
                            //生成的代理对象
                            Object proxy,
                            //正在被调用的目标方法buy(),show()
                            Method method,
                            //目标方法的参数
                            Object[] args) throws Throwable {
                        Object obj = null;
                        try {
                            //切面
                            aop.before();  //事务  日志
                            //业务
                            obj = method.invoke(target,args);
                            //切面
                            aop.after();

                        } catch (Exception e) {
                            //切面
                           aop.exception();
                        }

                        return obj; //目标方法的返回值
                    }
                }
        );
    }
}
~~~

# Spring的AOP通知类型(了解)

Spring支持AOP的编程，常用的有以下几种：



1）Before通知：

在目标方法被调用前调用，涉及接口org.springframework.aop.**MethodBeforeAdvice**;



2）After通知：

在目标方法被调用后调用，涉及接口为org.springframework.aop.**AfterReturningAdvice**; 



 3）Throws通知：

目标方法抛出异常时调用，涉及接口org.springframework.aop.**ThrowsAdvice**; 



4）Around通知：

拦截对目标对象方法调用，涉及接口为org.aopalliance.intercept.**MethodInterceptor**。





# SpringAop案例

~~~java
/**
 * 实现了前置增强接口的切面类【日志】
 */
public class LogAdvice  implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        SimpleDateFormat sf = new SimpleDateFormat("yyyy-MM-dd");
        System.out.println("\n[系统日志]"+sf.format(new Date())+"---"+method.getName()+ Arrays.toString(objects));
    }
}
~~~



~~~java
/**
 * 业务接口
 */
public interface BookService {
    public boolean buy(String userName,String bookName,double price);
    public void comment(String userName,String comments);
}
~~~



~~~java
/**
 * 图书业务功能实现【目标对象】
 */
public class BookServiceImpl implements BookService {
    @Override
    public boolean buy(String userName, String bookName, double price) {
        System.out.println("购买图书功能实现...........");
        System.out.println(userName+"购买了"+bookName+",花费了"+price+"元.");
        System.out.println(userName+"增加了"+(price/10));
        System.out.println("图书购买业务结束");
        return false;
    }

    @Override
    public void comment(String userName, String comments) {
        System.out.println("评论功能的实现 ...........");
        System.out.println(userName+"发表了"+comments+".");
        System.out.println("评论功能结束 ...........");
    }
}
~~~



~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

            <!--Spring的核心配置文件-->
    
    <!--创建业务对象-->
    <bean id ="bookServiceTarget" class="com.bjpowernode.service.impl.BookServiceImpl"></bean>
    
    <!--创建切面的对象-->
    <bean id="logAdvice" class="com.bjpowernode.advice.LogAdvice"></bean>

    <!--创建代理对象  绑定业务和切面-->
    <bean id="bookService" class="org.springframework.aop.framework.ProxyFactoryBean">
        <!--配置业务接口-->
        <property name="interfaces" value="com.bjpowernode.service.BookService"></property>

        <!--配置切面-->
        <property name="interceptorNames">
            <list>
                <value>logAdvice</value>
            </list>
        </property>
        
        <!--织入-->
        <!--具体业务实现类 -->
        <property name="target" ref="bookServiceTarget"></property>
    </bean>
    
</beans>
~~~



```java
/**
 * 测试代码
 */
@Test
public  void test01(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    //该对象是jdk动态代理对象
    BookService proxy = (BookService) ac.getBean("bookService");
    System.out.println(proxy.getClass());
    proxy.buy("张三","平凡的世界",55);
    proxy.comment("张三","还是很好看,可以看一看.....");
}
```

---

![image-20220508102530745](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508102530745.png)

---



Spring的AOP利用一种称为“横切”的技术，剖解开封装的对象内部，并将那些影响了多个类的**公共行为封装到一个可重用模块**，并将其名为“Aspect”【方面】

便于**减少系统的重复代码**，**降低模块间的耦合度**，并有利于未来的可操作性和可维护性。

总结：**AOP的核心思想就是“将应用程序中的商业逻辑同对其提供支持的通用服务进行分离。**



# AOP常用的术语

## 切面:

就是那些重复的,公共的,**通用的功能称为切面**   例如:日志,事务,权限.

## 连接点:

就是**目标方法**.因为在目标方法中要实现目标方法的功能和切面功能.

## 切入点(Pointcut):

指定切入的位置,**多个连接点构成切入点**.切入点可以是一个目标方法,可以是一个类中的所有方法,可以是某个包下的所有类中的方法

**符合表达式的某些方法**

## 目标对象:

操作谁,谁就是目标对象.

## 通知(Advice):

来指定**切入的时机**.是在目标方法执行前还是执行后还是出错时,还是环绕目标方法切入切面功能.



# AspectJ 对 AOP 的实现(掌握)

-  概念

​			**AspectJ 是一个优秀面向切面的框架**，它扩展了 Java 语言，提供了强大的切面实现。

​			在 Spring 中使用 AOP 开发时，一般使用 AspectJ 的实现方式。【Spring中有原生的AOP的实现，但Spring觉得AspectJ框架更好】



## AspectJ 的通知类型(理解)

​			AspectJ 中常用的通知有四种类型：

​					（1）前置通知@Before

​					（2）后置通知@AfterReturning

​					（3）环绕通知@Around

​					（4）最终通知@After

​					（5）定义切入点@Pointcut(了解)



### AspectJ 的切入点表达式(掌握)

- 切入点表达式

​		**execution(访问权限 方法返回值 方法声明(参数) 异常类型)**

注意，**表达式中 访问权限、异常类型 可省略**，**各部分间用空格分开**。在其中可以使用以下符号：

---

![image-20220508104555025](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508104555025.png)

---

- 示例

~~~
  execution(public * *(..))							任意的公共方法
  execution(* set*(..))								任何一个以“set”开始的方法
  execution(* com.xyz.service.impl.*.*(..))		    任意的返回值类型,在com.xyz.service.impl包下的任意类的任意方法的任意参数
  execution(* com.xyz.service..*.*(..))				任意的返回值类型 ,在com.xyz.service及其子包下的任意类的任意方法的任意参数  
   										com.xyz.service.a.b.*.*(..)  com.xyz.service.*.*(..)
  execution(* *..service.*.*(..))					service之前可以有任意的子包
  execution(* *.service.*.*(..))					service之前只有一个包
~~~



### 前置通知

@Before

​		**前置通知的切面方法的规范**

​			1)访问权限是public

​			2)方法的返回值是void

​			3)方法名称自定义

​			4)方法没有参数,如果有也只能是**JoinPoint**类型

​			5)必须使用**@Before注解来声明切入的时机是前切功能和切入点**

​					**参数:value  指定切入点表达式**

---

![image-20220508141953931](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508141953931.png)

---



~~~java
/**
 *  业务接口
 */
public interface SomeService {
    String doSome(String name,int age);
    void show();
}
~~~



```java
/**
 *  业务实现类
 */
@Service
public class SomeServiceImpl implements SomeService {
    @Override
    public String doSome(String name, int age) {
        System.out.println("doSome的业务功能实现................");
        return "abcd";
    }

    @Override
    public void show() {
        System.out.println("show()的业务方法实现执行.............");
    }
}
```



```java
/**
 *  此类为切面类,包含各种切面方法
 */
@Aspect  //交给AspectJ的框架去识别切面类
@Component  //交给spirng容器管理 记得扫包
public class MyAspect {
    /**
     * 所有切面的功能都是由切面方法来实现的
     * 可以将各种切面都在此类中进行开发
     * 业务方法
     * public String doSome(String name, int age)
     */
    
    //切入点的精准匹配
/*
    @Before(value = "execution(public String com.bjpowernode.s01.SomeServiceImpl.*(String,int))")
    public void myBefore(){
        System.out.println("切面方法中的前置通知功能实现............");
    }
*/
  
    //方法名与形参列表不固定
  /*  
    @Before(value = "execution(public * com.bjpowernode.s01.SomeServiceImpl.*(..))")
    public void myBefore(){
        System.out.println("切面方法中的前置通知功能实现............");
    }
*/
    
    //修饰符 部分包名与类名以及方法名和形参列表不固定
    //指定通知类型和切入点
    @Before(value = "execution( * com.bjpowernode.s01.*.*(..))")
    //入参类型可以是也只能是JoinPoint 或没有【不写也是可以的】
    //通过该参数，可获取切入点表达式、方法签名、目标对象等。
    //不光前置通知的方法，可以包含一个 JoinPoint 类型参数，所有的通知方法均可包含该参数。
    public void myBefore(JoinPoint jp){
        System.out.println("切面方法中的前置通知功能实现............");
        System.out.println("目标方法的签名:"+jp.getSignature());
        System.out.println("目标方法的参数:"+ Arrays.toString(jp.getArgs()));
    }
    
    
    /*
    @Before(value = "execution( * com.bjpowernode.s01..*(..))")
    public void myBefore(){
        System.out.println("切面方法中的前置通知功能实现............");
    }
*/
    
    //全部方法
    /*
    @Before(value = "execution( * *(..))")
    public void myBefore(){
        System.out.println("切面方法中的前置通知功能实现............");
    }
    */
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


    <!--基于注解的访问要添加包扫描-->
<context:component-scan base-package="com.bjpowernode.s01"></context:component-scan>

<!--    
	这种注册bean的方法也可以
    <bean id="service" class="com.bjpowernode.s01.SomeServiceImpl"></bean>

    <bean id="myaspect" class="com.bjpowernode.s01.MyAspect"></bean>
-->
    
    <!--绑定-->
    <!--
		<aop:aspectj-autoproxy/>通过扫描找到@Aspect 定义的切面类，再由切面类根据切入点找到目标类的目标方法
		再由通知类型找到切入的时间点 按通知类型与切入点，将其织入，并生成代理。
	-->
    <aop:aspectj-autoproxy ></aop:aspectj-autoproxy>
</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bjpowernode</groupId>
  <artifactId>spring_007_aspectj</artifactId>
  <version>1.0</version>
  <dependencies>

    <!--添加spring的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <!--添加aspectj的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.2.5.RELEASE</version>
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
       <directory>src/main/resources</directory>
       <includes>
         <include>**/*.xml</include>
         <include>**/*.properties</include>
       </includes>
     </resource>
   </resources>
  </build>
</project>
```

```java
@Test
public void test01(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s01/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("service");
    System.out.println(someService.getClass());
    String s = someService.doSome("张三",22);
    System.out.println(s);
}

@Test
public void test02(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s01/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("service");
    System.out.println(someService.getClass());//jdk动态代理对象
    someService.show();
}
```

---

![image-20220508141801419](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508141801419.png)

---

**通过AspectJ切换JDK动态代理与CGLib动态代理**

~~~xml
	<aop:aspectj-autoproxy ></aop:aspectj-autoproxy>  ===>默认是JDK动态代理,取时必须使用接口类型
    <aop:aspectj-autoproxy proxy-target-class="true"></aop:aspectj-autoproxy>  ==>设置为CGLib子类代理,可以使用接口和实现类接

~~~



JDK动态代理只能使用接口接，不能使用实现类接

记住:使用接口来接,永远不出错.



### 后置通知

​	@AfterReturning

​		**后置通知的方法的规范**

​				1)访问权限是public

​				2)方法没有返回值void

​				3)方法名称自定义

​				4)方法有参数(也可以没有参数,如果目标方法没有返回值,则可以写无参的方法,但一般会写有参,这样可以处理无参可以处理有参),**这个切面方法的参数就是					目标方法的返回值**

​				5)使用@AfterReturning注解表明是后置通知

​		参数:

​			value:指定切入点表达式

​			returning:**指定目标方法的返回值的名称,则名称必须与切面方法的参数名称一致.**



```java
/**
 *切面类
 */
@Aspect
@Component
public class MyAspect {
    @AfterReturning(value = "execution(* com.bjpowernode.s02.*.*(..))",returning = "obj")
    public void myAfterReturning(Object obj){
        System.out.println("后置通知功能实现..............");
        if(obj != null){
            if(obj instanceof String){
                obj = obj.toString().toUpperCase();
                System.out.println("在切面方法中目标方法的返回值:"+obj);
            }
            if(obj instanceof Student){
                Student stu = (Student) obj;
                stu.setName("李四");
                System.out.println("在切面方法中目标方法的返回值:"+stu);
            }
        }
    }
}
```

```java
/**
 *业务接口
 */
public interface SomeService {
    String doSome(String name,int age);
    Student change();
}
```



```java
/**
 *业务接口实现类
 */
@Service
public class SomeServiceImpl implements SomeService {
    @Override
    public String doSome(String name, int age) {
        System.out.println("doSome()业务方法被执行............");
        //后置通知的方法的入参可以接到目标方法的返回值【String是不可变字符串所以不可以改变值】
        return "abcd";
    }

    @Override
    public Student change() {
        System.out.println("change()方法被执行............");
       // System.out.println(1/0);
        return new Student("张三");
    }
}
```



```java
/**
 *实体类
 */
public class Student {
    private String name;
    //get、set方法
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
				    <!--核心配置文件-->

    <!--基于注解的访问要添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.s02"></context:component-scan>
    <!--绑定-->
    <aop:aspectj-autoproxy ></aop:aspectj-autoproxy>
</beans>
```



```java
@Test
public void test01(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s02/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("someServiceImpl");
    //调用的时候是张三，经过后置通知改变了值
    String s = someService.doSome("张三",22);
    System.out.println("在测试方法中目标方法的返回值:"+s);

}

@Test
public void test02(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s02/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("someServiceImpl");
    Student stu = someService.change();
    System.out.println("在测试为中目标方法的返回值是:"+stu);
}
```

---

![image-20220508150525607](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508150525607.png)

---



### 环绕通知

​	**环绕通知方法的规范**

​		1)访问权限是public

​		2)**切面方法有返回值**,**此返回值就是目标方法的返回值【Object】**

​		3)方法名称自定义

​		4)**方法可以包含一个 ProceedingJoinPoint  类型的参数**

​				接口 ProceedingJoinPoint  其有一个proceed()方法，用于执行目标方法。

​				若目标方法有返回值，则该方法的返回值就是目标方法的返回值。最后，环绕增强方法将其返回值返回。

​				该增强方法实际是拦截了目标方法的执行。

​		5)回避异常**Throwable**

​		6)使用@Around注解声明是**环绕通知**

​	参数:

​		value:指定切入点表达式

  它是通过拦截目标方法的方式 ,在目标方法前后增强功能的通知.它是功能最强大的通知,一般事务使用此通知.它可以轻易的改变目标方法的返回值.





---

![image-20220508151059018](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508151059018.png)

---

+ 案例

```java
/**
 *业务接口
 */
public interface SomeService {
    String doSome(String name,int age);
}
```



```java
/**
 *业务接口实现类
 */
@Service
public class SomeServiceImpl implements SomeService{
    @Override
    public String doSome(String name, int age) {
        System.out.println("doSome业务方法被执行...."+name);
        return "abcd";
    }
}
```



~~~java
/**
 *切面类
 */
@Aspect
@Component
public class MyAspect {

	//通知 + 切入点
    @Around(value = "execution(* com.bjpowernode.s03.*.*(..))")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
        //前切功能实现
        System.out.println("环绕通知中的前置功能实现............");
        //目标方法调用
        Object obj = pjp.proceed(pjp.getArgs());
        //后切功能实现
        System.out.println("环绕通知中的后置功能实现............");
        return obj.toString().toUpperCase();  //改变了目标方法的返回值【类型不变】
    }
}
~~~



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


    <!--基于注解的访问要添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.s03"></context:component-scan>
    <!--绑定-->
    <aop:aspectj-autoproxy ></aop:aspectj-autoproxy>
</beans>
```



```java
@Test
public void test01(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s03/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("someServiceImpl");
    String s = someService.doSome("张三",22);
    System.out.println("在测试方法中目标方法的返回值:"+s);

}
```

---

![image-20220508152025454](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508152025454.png)

---





### 定义切入点别名

**@Pointcut【切入点】**

当较多的通知增强方法使用相同的 execution 切入点表达式时，编写、维护均较为麻烦。

**AspectJ 提供了@Pointcut 注解，用于定义 execution 切入点表达式。**

其用法是，**将@Pointcut 注解在一个方法之上**，以后所有的 execution 的 value 属性值均可**使用该方法名作为切入点**。

代表的就是@Pointcut 定义的切入点。这个使用@Pointcut 注解的方法**一般使用 private 的标识方法**，即**没有实际作用的方法**。

~~~java
	//给切入点表达式起别名
    @Pointcut(value = "execution(* com.bjpowernode.s04.SomeServiceImpl.*(..))")
    public void mycut(){}
}
~~~



### 最终通知

​		@After

​		无论目标方法是否正常执行,最终通知的代码都会被执行.

**最终通知方法的规范**

​			1)访问权限是public

​			2)方法没有返回值

​			3)方法名称自定义

​			4)方法没有参数,**如果有也只能是JoinPoint** 包含【切入点表达式、方法签名、目标对象】

​			5)使用@After注解表明是最终通知

​	参数:

​		value:指定切入点表达式



**【当发生目标方法异常时，后置通知与环绕的后切功能都不能正常执行】**



- 案例

~~~java
/**
 *业务接口
 */
public interface SomeService {
    String doSome(String name,int age);
}
~~~



```java
/**
 *业务实现类
 */
@Service
public class SomeServiceImpl implements SomeService {
    @Override
    public String doSome(String name, int age) {
        System.out.println("doSome业务方法被执行...."+name);
        //System.out.println(1/0);
        return "abcd";
    }
}
```





```java
/**
 *切面类
 */
@Aspect
@Component
public class MyAspect {
	//最终通知
    @After(value = "mycut()")
    public void myAfter(){
        System.out.println("最终通知的功能........");
    }
    //前置通知
    @Before(value = "mycut()")
    public void myBefore(){
        System.out.println("前置通知的功能........");
    }
    //后置通知
    @AfterReturning(value = "mycut()",returning = "obj")
    public void myAfterReturning(Object obj){
        System.out.println("后置通知的功能........");
    }
    //环绕通知
    @Around(value = "mycut()")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕通知中的前置通知的功能........");
        Object obj = pjp.proceed(pjp.getArgs());
        System.out.println("环绕通知中的后置通知的功能........");
        return obj;
    }
    
    //给切入点表达式起别名
    @Pointcut(value = "execution(* com.bjpowernode.s04.*.*(..))")
    public void mycut(){}
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


    <!--基于注解的访问要添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.s04"></context:component-scan>
    <!--绑定-->
    <aop:aspectj-autoproxy ></aop:aspectj-autoproxy>
</beans>
```

```java
@Test
public void test01(){
    ApplicationContext ac = new ClassPathXmlApplicationContext("s04/applicationContext.xml");
    //取出代理对象
    SomeService someService = (SomeService) ac.getBean("someServiceImpl");
    String s = someService.doSome("张三",22);
    System.out.println("在测试方法中目标方法的返回值:"+s);

}
```

---

![image-20220508153415333](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508153415333.png)

---

## 多个通知间执行顺序

环绕通知中的前置通知

前置通知

业务方法

环绕通知中的后置通知

最终通知

后置通知



### 小结

什么是AOP

将一些公共的，重复的业务逻辑和功能抽取出来，写在切面中，并指定好切入点规定哪些方法在调用的时将切面织入；

大大减少重复代码；是功能业务更加专一，降低耦合；

底层使用动态代理





# SpringAOP与AspectJ的区别



---

![image-20220512191441810](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512191441810.png)

---



# Spring 集成 MyBatis



## 1.0 概念

将 MyBatis 与 Spring 进行整合，主要解决的问题就是。

所以，该整合只需要**将** **SqlSessionFactory 的对象生成器 SqlSessionFactoryBean 注册在 Spring 容器中**

再将由动态代理的xxxMapper对象注入给Service层

实现 Spring 与 MyBatis 的整合，常用的方式：扫描的 Mapper 动态代理。

Spring 像插线板一样，mybatis 框架是插头，可以容易的组合到一起。插线板 spring 插上 mybatis，两个框架就是一个整体。



## 2.0 Spring的事务管理

- 概念

​		事务原本是数据库中的概念，在实际项目的开发中，**进行事务的处理一般是在业务逻辑层**， 即 Service 层。

​		这样做是为了能够使用事务的特性来管理关联操作的业务。

- 在 **Spring** 中通常可以通过以下**两种方式**来实现对**事务的管理**：

​			（1）使用 Spring 的**事务注解管理事务**

​			（2）使用 **AspectJ** 的 AOP 配置管理事务(**声明式事务管理**)





## 3.0 Spring中事务的五大隔离级别

1. 未提交读(Read Uncommitted)：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

2. 提交读(Read Committed)：只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别 (不重复读)

3. 可重复读(Repeated Read)：可重复读。在同一个事务内的查询都是事务开始时刻一致的，InnoDB默认级别。

   在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻象读，但是innoDB解决了幻读

4. 串行读(Serializable)：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞

----

![image-20220508224110080](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508224110080.png)

---



## 4.0 不同数据库的隔离级别（面试点）

- MySQL：mysql默认的事务处理级别是'REPEATABLE-READ',也就是**可重复读**
- Oracle：oracle数据库支持READ COMMITTED 和 SERIALIZABLE这两种事务隔离级别。默认系统事务隔离级别是READ COMMITTED,也就是读已提交



##  5.0 Spring事务的传播特性



**多个事务之间的合并,互斥等都可以通过设置事务的传播特性来解决.**



  常用
  PROPAGATION_REQUIRED：必被包含事务
  PROPAGATION_REQUIRES_NEW：自己新开事务，不管之前是否有事务
  PROPAGATION_SUPPORTS：支持事务，如果加入的方法有事务，则支持事务，如果没有，不单开事务
  PROPAGATION_NEVER：不能运行中事务中，如果**包在事务中，抛异常**
  PROPAGATION_NOT_SUPPORTED：不支持事务，运行在非事务的环境**【即使外边有事务，自己也不受影响】**
  不常用
  PROPAGATION_MANDATORY：必须包在事务中，没有事务则抛异常
  PROPAGATION_NESTED：嵌套事务

---

![image-20220508224231315](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508224231315.png)

---





## 6.0 @Transactional的参数讲解

---

![image-20220508224527951](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508224527951.png)

---

~~~java
@Transactional(propagation = Propagation.REQUIRED,//事务的传播特性 常用五种
        noRollbackForClassName = "ArithmeticException", //指定发生什么异常不回滚,使用的是异常的名称 多个用{“”，“”}
        noRollbackFor = ArithmeticException.class,//指定发生什么异常不回滚,使用的是异常的类型
        rollbackForClassName = "",//指定发生什么异常必须回滚
        rollbackFor = ArithmeticException.class,//指定发生什么异常必须回滚
        timeout = -1, //连接超时设置,默认值是-1,表示永不超时
        readOnly = false, //默认是false,如果是查询操作,必须设置为true.
        isolation = Isolation.DEFAULT//使用数据库自已的隔离级别
)
~~~



- @Transactional有几点需要注意

​			**只能声明在public的method**。原因是spring是通过JDK代理或者CGLIB代理的，**生成的代理类**，只能处理public方法

​			注解放在类名称上面，这样你配置的这个@Transactional **对这个类中的所有public方法都起作用**

​			@Transactional 在方法名上，只对这个方法有作用，同样必须是public的方法。
​			不能被类内部方法调用。还是因为代理的原因，类内部自调用，不会经过代理类，所以@Transactional不会生效



## 7.0 MyBatis框架与Hibernate框架使用的事务管理器（面试点）

- Spring+MyBatis的事务管理器配置

~~~xml
	<!-- 定义事务管理器 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>

	<!--使用注解事务 注意：要在spring配置中添加tx命名约束 -->
	<!--注解驱动事务管理器-->
	<tx:annotation-driven transaction-manager="transactionManager" />
~~~



- Spring+ Hibernate的事务管理器配置

~~~xml
<!-- 事务管理器配置,单数据源事务 -->
	<bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory" />
	</bean>	

	<!-- 使用annotation定义事务 -->
	<tx:annotation-driven  transaction-manager="transactionManager" proxy-target-class="true" />
~~~



# 为什么添加事务管理器

~~~
  JDBC:  	 	 Connection  	 con.commit();   				con.rollback();
  MyBatis:  	SqlSession     	sqlSession.commit();      sqlSession.rollback();
  Hibernate:  Session    			session.commit();   		session.rollback();
~~~



  事务管理器用来生成 **相应技术的==连接== + 执行语句的对象.**

  如果使用MyBatis框架,必须使用DataSourceTransactionManager类完成处理

~~~xml
     <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--因为事务必须关联数据库处理,所以要配置数据源-->
        <property name="dataSource" ref="dataSource"></property>
     </bean>
~~~

  注意：

  **项目中的所有事务,必须添加到业务逻辑层上.**



# Spring中事务的实现

- Spring中事务的实现有两种方式

  - 一种是基于xml文件的实现
  - 一种是基于注解方式实现

  

​			在SSM的开发中，多使用注解方式实现事务的处理。



## 1.0 基于xml方式的实现

- 相关依赖

~~~xml
<dependencies>
  <!--单元测试-->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
  </dependency>
  <!--aspectj依赖-->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.2.5.RELEASE</version>
  </dependency>
  <!--spring核心ioc-->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.5.RELEASE</version>
  </dependency>
  <!--做spring事务用到的-->
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.2.5.RELEASE</version>
       </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.5.RELEASE</version>
  </dependency>
  <!--mybatis依赖-->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.1</version>
  </dependency>
  <!--mybatis和spring集成的依赖-->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.1</version>
  </dependency>
  <!--mysql驱动-->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.22</version>
  </dependency>
  <!--德鲁伊数据库连接池-->
  <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.12</version>
  </dependency>
</dependencies>
<build>
  <!--目的是把src/main/java目录中的xml文件包含到输出结果中。输出到classes目录中-->
  <resources>
    <resource>
      <directory>src/main/java</directory><!--所在的目录-->
      <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>false</filtering>
    </resource>
    <resource>
             <directory>src/main/resources</directory><!--所在的目录-->
      <includes>
          <!--包括目录下的.properties,.xml 文件都会扫描到-->
        <include>**/*.properties</include>
        <include>**/*.xml</include>
      </includes>
      <filtering>false</filtering>
    </resource>
  </resources>

</build>
~~~



- 引入aop名称空间

---

![image-20220508225812560](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508225812560.png)

---



- 引入tx名称空间

---

![image-20220508225852944](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508225852944.png)

---

![image-20220508225920281](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508225920281.png)

---

- 配置事务

~~~xml
<!--声明式事务的配置-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--使用xml方式声明事务-->
<!--配置切面的属性，哪些方法需要添加什么事务传播特性-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="get*" read-only="true"/>
        <tx:method name="select*" read-only="true"/>
        <tx:method name="find*" read-only="true"/>
        <tx:method name="search*" read-only="true"/>
        <tx:method name="add*" propagation="REQUIRED" />
        <tx:method name="save*" propagation="REQUIRED" no-rollback-for="ArithmeticException"/>
        <tx:method name="insert*" propagation="REQUIRED"  no-rollback-for="ArithmeticException"/>
        <tx:method name="delete*" propagation="REQUIRED"/>
        <tx:method name="remove*" propagation="REQUIRED"/>
        <tx:method name="clean*" propagation="REQUIRED"/>
        <tx:method name="update*" propagation="REQUIRED"/>
        <tx:method name="modify*" propagation="REQUIRED"/>
        <tx:method name="set*" propagation="REQUIRED"/>
        <tx:method name="change*" propagation="REQUIRED"/>
        <tx:method name="*" propagation="SUPPORTS"/>
    </tx:attributes>
</tx:advice>

<!--使用AOP的技术进行切入点织入-->
<aop:config>
    <!--切入点表达式：指定在哪个包下的哪些类中的哪些方法添加事务处理-->
    
    <aop:pointcut id="pointcat" expression="execution(* com.bjpowernode.service.*.*(..))"></aop:pointcut>
    
    <!--完成切面与切入点绑定-->
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcat"></aop:advisor>
</aop:config>
~~~



## 2.0 基于注解方式实现

- 导入相关依赖

~~~xml
  xmlns:tx="http://www.springframework.org/schema/tx"

    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd
~~~



- 配置注解驱动

~~~xml
	<!--声明式事务的配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

	<!--    配置注解驱动-->
    <tx:annotation-driven></tx:annotation-driven>
~~~



- 在对应的类上或方法上添加@Transactional设置传播特性

~~~java
@Service  //交给Spring接管,进行对象的创建,并且自动注入mapper
@Transactional( propagation = Propagation.REQUIRED  //必须添加事务，传播特性
                ,readOnly = true  //只读事务（用于查询操作）
                ,timeout = -1   //设置连接永不超时
                ,noRollbackForClassName = "ArithmeticException"  //遇到这个异常不回滚事务
                ,isolation = Isolation.DEFAULT  //使用数据库的隔离级别
              )
public class UsersServiceImpl implements UsersService {
~~~





# 编程式事务与声明式事务的区别

---

![image-20220512204019825](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220512204019825.png)

---







# Spring+MyBatis整合案例

## SM整合的必要依赖

~~~xml
      <!--单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <!--aspectj依赖-->
<!--        <dependency>-->
<!--            <groupId>org.springframework</groupId>-->
<!--            <artifactId>spring-aspects</artifactId>-->
<!--            <version>5.2.5.RELEASE</version>-->
<!--        </dependency>-->
        <!--spring核心ioc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <!--做spring事务用到的-->
<!--        <dependency>-->
<!--            <groupId>org.springframework</groupId>-->
<!--            <artifactId>spring-tx</artifactId>-->
<!--            <version>5.2.5.RELEASE</version>-->
<!--        </dependency>-->

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>


        <!--mybatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.1</version>
        </dependency>


        <!--mybatis和spring集成的依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>


        <!--mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>


        <!--阿里公司的数据库连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.12</version>
        </dependency>
~~~





## 1.0 添加db.properties文件

~~~properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123
~~~



## 2.0 添加SqlMapConfig.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?> <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--读取属性文件中数据库的配置 可以配置在spring的核心配置文件中-->
    <!--<properties resource="db.properties"></properties>-->
    
    <!--保留<setting>在mybatis的核心配置文件中-->
    <settings>
            <!--设置日志输出语句,显示相应操作的sql语名-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    
        <!--在spring核心配置文件中配置别名-->
    <!--<typeAliases>-->
        <!--<package name="com.bjpowernode.pojo"></package>-->
    <!--</typeAliases>-->
    
              <!--在spring核心配置文件中配置数据源-->
    <!--<environments default="development">-->
        <!--<environment id="development">-->
            <!--<transactionManager type="JDBC"/>-->
            <!--<dataSource type="POOLED">-->
                <!--<property name="driver" value="com.mysql.jdbc.Driver"/>-->
                <!--<property name="url"-->
                          <!--value="jdbc:mysql://localhost:3308/ssm?useSSL=false&amp;serverTimezone=UTC&amp;allowPublicKeyRetrieval=true"/>-->
                <!--<property name="username" value="root"/>-->
                <!--<property name="password" value="123456"/>-->
            <!--</dataSource>-->
        <!--</environment>-->
    <!--</environments>-->
    
            <!--在spring核心配置文件中配置扫描的mapper所在的包-->
    <!--<mappers>-->
        <!--<package name="mapper文件所在的包名"></package>-->
    <!--</mappers>-->
</configuration>
```



## 3.0 添加applicationContext_mapper.xml文件并实现功能

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--读取属性文件jdbc.properties 不在mybatis中配置-->
    <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
    
    <!--创建数据源 不在mybatis中配置-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
    
    
    <!--配置SqlSessionFactoryBean类 不用编写工具类获取SqlSession对象-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--配置数据源-->
        <property name="dataSource" ref="dataSource"></property>

        <!--配置MyBatis的核心配置文件 mybatis配置文件中还可以配置<setting>标签-->
        <property name="configLocation" value="SqlMapConfig.xml"></property>

        <!--注册实体类的别名 不在mybatis中配置-->
        <property name="typeAliasesPackage" value="com.bjpowernode.pojo"></property>
        
    </bean>

    <!--注册mapper.xml文件 不在mybatis中配置 通过动态代理生成数据访问层的对象，并加入到容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.bjpowernode.mapper"></property>
    </bean>

</beans>
```



## 声明式事务

  S**pring非常有名的事务处理方式**:声明式事务.

  要求项目中的方法命名有规范
  	1)完成增加操作包含    add  save  insert  set
  	2)更新操作包含   update   change  modify  
 	 3)删除操作包含   delete   drop    remove  clear
 	 4)查询操作包含   select   find    search  get 





## 4.0 添加applicationContext_service.xml文件并实现功能【注解驱动 事务】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--导入applicationContext_mapper.xml文件-->
    <import resource="applicationContext_mapper.xml"></import>
    
    <!--SM是基于注解的开发,所以添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.service.impl"></context:component-scan>
    
    <!--事务处理-->

    <!--1.添加事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--因为事务必须关联数据库处理,所以要配置数据源-->
        <property name="dataSource" ref="dataSource"></property>
     </bean>

    <!--2.添加事务的注解驱动-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
</beans>
```



## 5.0 添加applicationContext_trans.xml文件(xml配置文件方式)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd  http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--此配置文件与applicationContext_service.xml的功能一样,只是事务配置不同-->

    <!--导入applicationContext_mapper.xml-->
    <import resource="applicationContext_mapper.xml"></import>
    
    <!--添加包扫描-->
    <context:component-scan base-package="com.bjpowernode.service.impl"></context:component-scan>
    
    
    <!--添加事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--配置事务切面-->
    <tx:advice id="myadvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="select*" read-only="true"/>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="search*" read-only="true"/>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="insert*" propagation="REQUIRED" no-rollback-for="ArithmeticException"/>
            <tx:method name="add*" propagation="REQUIRED"/>
            <tx:method name="save*" propagation="REQUIRED" no-rollback-for="ArithmeticException"/>
            <tx:method name="set*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="change*" propagation="REQUIRED"/>
            <tx:method name="modify*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="remove*" propagation="REQUIRED"/>
            <tx:method name="drop*" propagation="REQUIRED"/>
            <tx:method name="clear*" propagation="REQUIRED"/>
            
            <!--配置事务切面时可以使用通配符*来匹配所有方法-->
            <tx:method name="*" propagation="SUPPORTS"/>
        </tx:attributes>
    </tx:advice>
    
    <!--绑定切面和切入点-->
    <aop:config>
        <aop:pointcut id="mycut" expression="execution(* com.bjpowernode.service.impl.*.*(..))"></aop:pointcut>
        <aop:advisor  advice-ref="myadvice" pointcut-ref="mycut"></aop:advisor>
    </aop:config>
</beans>
```



## 注解式事务与申明式事务可以同时使用

- 加入事务的注解驱动就行

---

![image-20220508202815034](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508202815034.png)

---





- 如果想要让注解式事务覆盖声明式事务 就多加一个order属性，值越大；越优先

![image-20220508202837258](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508202837258.png)



## 6.0 实体类

```java
/**
 *用户实体
 */
public class Users {
    private Integer userid;
    private String username;
    private String upass;
}
```

```java
/**
 *账户实体类
 */
public class Accounts {
    private Integer aid;
    private String aname;
    private String acontent;
}
```



## 7.0 数据访问层

```java
public interface UsersMapper {
    int insert(Users users);
}
```

```java
public interface AccountsMapper {
    //增加帐户
    int save(Accounts accounts);
}
```



## 8.0 业务逻辑层

```java
/**
 *
 */
@Service  //交给Spring去创建对象
//@Transactional(propagation = Propagation.REQUIRED)
public class UsersServiceImpl implements UsersService {
    //切记切记:在所有的业务逻辑层中一定会有数据访问层的对象
    /**
     *         //1.读取核心配置文件
     *         InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
     *         //2.创建工厂对象
     *         SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
     *         //3.取出sqlSession
     *         sqlSession = factory.openSession(true);//自动提交事务
     *         //4.取出动态代理的对象,完成接口中方法的调用,实则是调用xml文件中相的标签的功能
     *         uMapper = sqlSession.getMapper(UsersMapper.class);
     */
    @Autowired//一个注解，代替了通过sqlsession对象获取mapper动态代理对象
    UsersMapper usersMapper;

    @Autowired
    AccountsService accountsService;

    @Override
    public int insert(Users users) {
        int num = usersMapper.insert(users);
        System.out.println("用户增加成功!num="+num);

        //调用帐户的增加操作,调用的帐户的业务逻辑层的功能
        num = accountsService.save(new Accounts(300,"王五","帐户好的呢!"));
        return num;
    }
}
```



```java
/**
 *
 */
@Service
/*
@Transactional(propagation = Propagation.REQUIRED,//事务的传播特性
        noRollbackForClassName = "ArithmeticException", //指定发生什么异常不回滚,使用的是异常的名称
        noRollbackFor = ArithmeticException.class,//指定发生什么异常不回滚,使用的是异常的类型
        rollbackForClassName = "",//指定发生什么异常必须回滚
        rollbackFor = ArithmeticException.class,//指定发生什么异常必须回滚
        timeout = -1, //连接超时设置,默认值是-1,表示永不超时
        readOnly = false, //默认是false,如果是查询操作,必须设置为true.
        isolation = Isolation.DEFAULT//使用数据库自已的隔离级别
)
*/
@Transactional(propagation = Propagation.NEVER)
public class AccountsServiceImpl implements AccountsService {
    //一定会有数据访问层的对象
    @Autowired
    AccountsMapper accountsMapper;
    @Override
    public int save(Accounts accounts) {
        int num = 0;
        num = accountsMapper.save(accounts);
        System.out.println("增加帐户成功!num="+num);
        //手工抛出异常
        System.out.println(1/0);
        return num;
    }
}
```



- 测试结果

---

![image-20220508201355415](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26image-20220508201355415.png)

---





# 总结

----

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/26Spring.png)

---

