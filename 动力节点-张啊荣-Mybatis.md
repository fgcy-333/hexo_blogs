---
title: Mybatis
date: 2022-08-26 17:25:45
tags:
- 动力节点-张啊荣-Mybatis
categories: 
- 视频学习笔记
---

# 一、框架概述

## 1.软件开发常用结构

### 1.1.三层架构

什么是三层架构
  在项目开发中,遵循的一种形式模式.分为三层.
  界面层（User Interface layer）、业务逻辑层（Business Logic Layer）、数据访问层（Data access layer)
      1)界面层表示层，视图层):用来接收客 户端的输入,调用业务逻辑层进行功能处理,返回结果给客户端.过去的servlet就是界面层的功能.
      2)业务逻辑层:用来进行整个项目的业务逻辑处理,向上为界面层提供处理结果,向下问数据访问层要数据.
      3)数据访问层:专门用来进行数据库的增删改查操作,向上为业务逻辑层提供数据.

- **三层的处理请求的交互：**

~~~
客户端<--->界面层<--->业务逻辑层<--->数据访问层<--->数据库。
~~~

  各层之间的调用顺序是固定的,不允许跨层访问

<!--more-->

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261726037.png)

---



- **为什么要使用三层？**

1.结构清晰、耦合度低,  各层分工明确
2.可维护性高，可扩展性高
3.有利于标准化
4.开发人员可以只关注整个结构中的其中某一层的功能实现
5.有利于各层逻辑的复用



### 1.2常用框架

- **MyBatis 框架**

1、MyBatis 是一个优秀的基于 java 的持久层框架，内部封装了 jdbc，开发者只需要关注 sql 语句本身，而不需要处理加载驱动、创建连接、创建 statement、关闭连接，资源等繁杂的过程。

2、MyBatis 通过 xml 或注解两种方式将要执行的各种 sql 语句配置起来，并通过 java 对象和 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回



- **Spring 框架**

1、Spring 框架为了解决软件开发的复杂性而创建的。
2、Spring 使用的是基本的 JavaBean 来完成以前非常复杂的企业级开发。
3、Spring 解决了业务对象，功能模块之间的耦合，不仅在 javase,web 中使用， 大部分 Java 应用都可以从 Spring 中受益。
4、Spring 是一个轻量级控制反转(IoC)和面向切面(AOP)的容器



- **SpringMVC 框架**

Spring MVC 属于 SpringFrameWork 3.0 版本加入的一个模块，为 Spring 框架提供了构建 Web 应用程序的能力
现在可以用Spring 框架提供的 SpringMVC 模块实现 web 应用开发，在 web 项目中可以无缝使用 Spring 和 Spring MVC 框架



- **SSM**

 1、 Spring:它是整合其它框架的框架.它的核心是IOC和AOP.它由20多个模块构成.在很多领域都提供了很好的解决方案.是一个大佬级别的存在.
  2、SpringMVC:它是Spring家族的一员.专门用来优化控制器(Servlet)的.提供了极简单 数据提交, 数据携带, 页面跳转 等功能.
  3、MyBatis:是持久化层的一个框架.用来进行数据库访问的优化.专注于sql语句.极大的简化了JDBC的访问



- **什么是框架**

它是一个半成品软件.将所有的公共的,重复的功能解决掉,帮助程序快速高效的进行开发.它是可复用,可扩展的.



## 2. JDBC编程



### 2.1JDBC编程回顾

~~~java
public void findStudent() {
Connection conn = null; 
Statement stmt = null;
ResultSet rs = null;
try {
//注册 mysql 驱动 
  Class.forName("com.mysql.jdbc.Driver");
//连接数据的基本信息 url ，username，password
String url = "jdbc:mysql://localhost:3306/springdb"; 
String username = "root";
String password = "123456";
//创建连接对象 
conn = DriverManager.getConnection(url, username, password);
//保存查询结果 
List<Student> stuList = new ArrayList<>();
//创建 Statement, 用来执行 sql 语句 
stmt = conn.createStatement();
//执行查询，创建记录集， 
rs = stmt.executeQuery("select * from student");
while (rs.next()) {
Student stu = new Student(); 
stu.setId(rs.getInt("id"));
stu.setName(rs.getString("name"));
stu.setAge(rs.getInt("age"));
//从数据库取出数据转为 Student 对象，封装到 List 集合 
stuList.add(stu);}
  }catch(Exception e){
      e.printStackTrace();
}finally{
   try{
if(rs != null)
      rs.lose();
if(pstm != null)
      pstm.close();
   if(con != null)
      con.close();
   
}catch(Exception e){
  e.printStackTrace();
}
}
~~~

### 2.2使用JDBC的缺陷

1.代码比较多，开发效率低
2.需要关注 Connection ,Statement, ResultSet 对象创建和销毁
3.对 ResultSet 查询的结果，需要自己封装为 List
4.重复的代码比较多些
5.业务代码和数据库的操作混在一起



## 3.MyBatis框架概述

### **3.1概述**

MyBatis 本是 apache 的一个开源项目 iBatis
2010 年这个项目由 apache software foundation 迁移到了 google code，并且改名为 MyBatis 
2013 年 11 月迁移到 Github
iBATIS 一词来源于“internet”和“abatis”的组合，是一个基于 Java 的持久层框架。
iBATIS 提供的持久层框架包括 SQL Maps 和 Data Access Objects（DAOs）
当前，最新版本是 MyBatis 3.5.7 ，其发布时间是 2021 年 4月 7日



### 3.2**框架解决的主要问题**

1、减轻使用 JDBC 的复杂性，不用编写重复的创建 Connetion , Statement ; 不用编写关闭资源代码。
2、直接使用 java 对象，表示结果数据。让开发者专注 SQL的处理。 其他分心的工作由 MyBatis 代劳。

MyBatis 可以完成：

1.注册数据库的驱动，例如 Class.forName(“com.mysql.jdbc.Driver”))

2.创建 JDBC 中必须使用的 Connection  ， Statement， ResultSet 对象

3.从 xml 中获取 sql，并执行 sql 语句，把 ResultSet 结果转换 java 对象



~~~java
 List<Student> list = new ArrayLsit<>();
    ResultSet rs = state.executeQuery(“select * from student”); 
    while(rs.next){
    Student student = new Student();
    student.setName(rs.getString(“name”)); student.setAge(rs.getInt(“age”));
    list.add(student);
    }
//4.关闭资源
ResultSet.close() , Statement.close() , Conenection.close()
~~~

### 3.3MyBatis框架的结构



---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727004.png)****

---







# 二、MyBatis框架快速入门



## 1.0添加框架步骤

1. 新建库建表

~~~mysql
#创建数据库ssm
CREATE DATABASE IF NOT EXISTS ssm DEFAULT CHARSET utf8;

#使用(打开)ssm数据库
use ssm;

#创建表student
CREATE TABLE IF NOT EXISTS `student` (
`id` int(11)  AUTO_INCREMENT primary key ,
`name` varchar(255) DEFAULT NULL,
`email` varchar(255) DEFAULT NULL,
`age` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into student(name,email,age) values('张三','zhangsan@126.com',22);
insert into student(name,email,age) values('李四','lisi@126.com',21);
insert into student(name,email,age) values('王五','wangwu@163.com',22);
insert into student(name,email,age) values('赵六','zhaoliun@qq.com',24);
select * from student;
~~~

2. 新建maven项目,选quickstart模板
3. 修改目录,添加缺失的目录,修改目录属性
4. 修改pom.xml文件,添加MyBatis的依赖,添加mysql的依赖

~~~xml
     <!--添加MyBatis框架的依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
    </dependency>
    <!--添加mysql依赖-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.32</version>
    </dependency>
~~~

5. 修改pom.xml文件,添加资源文件指定

~~~xml
<!--
maven约定大于配置：所以要配置java目录下可以进行资源导出
--> 
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
            <!--配置了java目录下可以进行资源导出，就必须配置resouces目录下的文件的导出否则失效-->
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
~~~

6. 在idea中添加数据库的可视化

7. 在resources下添加jdbc.properties属性文件(数据库的配置)

~~~properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=root123
~~~

8. 添加SqlMapConfig.xml文件,MyBatis的核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--
	  取属性文件(jdbc.properties)
      属性:
         resources:从resources目录下找指定名称的文件加载
         url:使用绝对路径加载属性文件
    -->
    <properties resource="jdbc.properties"></properties>

    <!--设置日志输出底层执行的代码-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <!--注册实体类的别名-->
    <typeAliases>
        <!--单个实体类别名注册-->
        <!--<typeAlias type="com.bjpowernode.pojo.Student" alias="student"></typeAlias>-->
        <!--批量注册别名
          别名是类名的驼峰命名法(规范)
        -->
        <package name="com.bjpowernode.pojo"></package>
    </typeAliases>

    <!--
		配置数据库的环境变量(数据库连接配置)
        default:使用下面的environment标签的id属性进行指定配置
    -->
    <environments default="development">
        <!--
			开发时在公司使用的数据库配置
            id:就是提供给environments的default属性使用
        -->
        <environment id="development">
            <!--
				配置事务管理器
                 type:指定事务管理的方式
                  	JDBC:事务的控制交给程序员处理
                  	MANAGED:由容器(Spring)来管理事务
            -->
            <transactionManager type="JDBC"></transactionManager>
            <!--
			 配置数据源
               type:指定不同的配置方式
                  JNDI:java命名目录接口,在服务器端进行数据库连接池的管理
                  POOLED:使用数据库连接池
                  UNPOLLED:不使用数据库连接池
            -->
            <dataSource type="POOLED">
                <!--配置数据库连接的基本参数 连接池和非连接池都会复用这些属性
                    private String driver;
                    private String url;
                    private String username;
                    private String password;
                -->
                <property name="driver" value="${jdbc.driverClassName}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>

        <!--在家的数据库配置-->
        <!--<environment id="home">-->
            <!--<transactionManager type=""></transactionManager>-->
            <!--<dataSource type=""></dataSource>-->
        <!--</environment>-->

        <!--上线后的数据库配置-->
        <!--<environment id="online">-->
            <!--<transactionManager type=""></transactionManager>-->
            <!--<dataSource type=""></dataSource>-->
        <!--</environment>-->
    </environments>

    <!--注册mapper.xml文件
       resource:从resources目录下找指定名称的文件注册
       url:使用绝对路径注册
       class:动态代理方式下的注册
    -->
    <mappers>
        <mapper resource="StudentMapper.xml"></mapper>
    </mappers>
</configuration>
~~~

- sqlMapperConfig.xml的dtd约束

~~~xml
<!ELEMENT configuration 
(properties?, settings?, typeAliases?,
typeHandlers?, objectFactory?, objectWrapperFactory?
reflectorFactory?, plugins?, environments?,
databaseIdProvider?, mappers?)>
~~~

- sqlMapperConfig.xml【mybatis的核心配置文件】的约束文件

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727966.png)

---





- **事务管理器属性类型**

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261728494.png)

---

- **数据源管理配置**

----

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727703.png)

---



## 2.0 MyBatis对象分析

**1)Resources类**

~~~java
 	// 就是解析SqlMapConfig.xml文件,创建出java的对象
  InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
~~~

****



**2)SqlSessionFactory接口**

~~~~java
  //使用ctrl+h快捷键查看本接口的子接口及实现类
 // DefaultSqlSessionFactory是实现类
  //读取上一步创建的对象进行工厂初始化
  SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
~~~~

**3)SqlSession接口**

~~~
DefaultSqlSession是SqlSession接口的实现类
~~~



## 3.0 SqlMapConfig.xml文件的优化

- **为实体类注册别名**

~~~~xml
  1)单个注册
     <typeAlias type="com.bjpowernode.pojo.Student" alias="student"></typeAlias>
  2)批量注册
        <!--
          批量注册别名
          别名是类名的驼峰命名法(规范)
        -->
    <package name="com.bjpowernode.pojo"></package>
~~~~

- **设置日志输出**

~~~~xml
  <!--设置日志输出底层执行的代码-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
~~~~



## **在SqlMapConfig.xml件中注册XXMapper.xml**

(1)使用resource注册

~~~xml
    <mapper resource="com/bjpowernode/mapper/UsersMapper.xml"></mapper>
~~~

​    注意: UserMapper.xml是带后缀的,分隔符使用/

(2)使用class注册

~~~xml
    <mapper class="com.bjpowernode.mapper.UsersMapper"></mapper>
~~~

​    注意: class的值是接口的完全限定名称.没有后缀

(3)使用url注册

~~~xml
	<mapper url="file:///E:/UserMapper.xml"></mapper>
~~~

​	指定绝对路径注册,注意file后面是双杠.有后缀
​	
(4)使用packag>注册

~~~xml
	<package name="com.bjpowernode.mapper"/>
~~~

​	注意：可以写在resource中【同java之下的mapper包路径】，编译出来可以合并在java写的包下



**注意：以上四种都可以注册在resource中的mapper**

~~~xml
        <!--<package name="com.bjpowernode.mapper"></package>-->

        <!--<mapper class="com.bjpowernode.mapper.UsersMapper"/>-->

<!--        <mapper url="file:///D:\learn\动力节点\mybatis-张阿荣\04_project\04_project\mybatisall\mybatis_002_users\src\main\resources\com\bjpowernode\mapper\UsersMapper.xml"/>-->

<!--        <mapper resource="com/bjpowernode/mapper/UsersMapper.xml"/>-->
~~~



# 不使用动态代理的Mybatis开发

## pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.bjpowernode</groupId>
  <artifactId>mybatis_001_student</artifactId>
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
    <!--添加MyBatis框架的依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.6</version>
    </dependency>
    <!--添加mysql依赖-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.32</version>
    </dependency>
  </dependencies>

  <!--添加资源文件的指定-->
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

~~~





## mybatis核心配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--读取属性文件(jdbc.properties)
      属性:
         resources:从resources目录下找指定名称的文件加载
         url:使用绝对路径加载属性文件
         D:\course\16.MyBatis\04_project\mybatisall\mybatis_001_student\src\main\resources\jdbc.properties
    -->
    <properties resource="jdbc.properties"></properties>

    <!--设置日志输出底层执行的代码-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    
    <!--注册实体类的别名-->
    <typeAliases>
        <!--单个实体类别名注册-->
        <!--<typeAlias type="com.bjpowernode.pojo.Student" alias="student"></typeAlias>-->
        <!--
			批量注册别名
            别名是类名的小驼峰命名法(规范)
        -->
        <package name="com.bjpowernode.pojo"></package>
    </typeAliases>

    <!--
		配置数据库的环境变量(数据库连接配置)
        default:使用下面的environment标签的id属性进行指定配置
    -->
    <environments default="development">
        <!--开发时在公司使用的数据库配置
          id:就是提供给environments的default属性使用
        -->
        <environment id="development">
            <!--
				配置事务管理器
               	  type:指定事务管理的方式
                    JDBC:事务的控制交给程序员处理
                    MANAGED:由容器(Spring)来管理事务
            -->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源
               type:指定不同的配置方式
                  JNDI:java命名目录接口,在服务器端进行数据库连接池的管理
                  POOLED:使用数据库连接池
                  UNPOLLED:不使用数据库连接池
            -->
            <dataSource type="POOLED">
                <!--配置数据库连接的基本参数
                    private String driver;
                    private String url;
                    private String username;
                    private String password;
                -->
                <property name="driver" value="${jdbc.driverClassName}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>

        <!--在家的数据库配置-->
        <!--<environment id="home">-->
            <!--<transactionManager type=""></transactionManager>-->
            <!--<dataSource type=""></dataSource>-->
        <!--</environment>-->

        <!--上线后的数据库配置-->
        <!--<environment id="online">-->
            <!--<transactionManager type=""></transactionManager>-->
            <!--<dataSource type=""></dataSource>-->
        <!--</environment>-->
    </environments>

    <!--注册mapper.xml文件
       resource:从resources目录下找指定名称的文件注册
       url:使用绝对路径注册
       class:全限定类名  没有后缀
	   package：类路径下某个包 可以是java目录下，也可以是resource目录下 只是包名
    -->
    <mappers>
        <mapper resource="StudentMapper.xml"></mapper>
    </mappers>

</configuration>
~~~

## jdbc.properties

~~~properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:13306/ssm?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
jdbc.username=root
jdbc.password=root123
~~~



## 实体类

~~~java
public class Student {
    private Integer id;
    private String name;
    private String email;
    private Integer age;
}
~~~



## xxxMapper.xml

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
  mapper:是整个文件的大标签,用来开始和结束xml文件
  属性:
     namespace:指定命名空间(相当于包名),用来区分不同mapper.xml文件中相同的id属性

-->
<mapper namespace="zar">
    <!--
      完成查询全部学生的功能
      List<Student> getALL();
         resultType:指定查询返回的结果集的类型,如果是集合,则必须是泛型的类型
         parameterType:如果有参数,则通过它来指定参数的类型
    -->
    <select id="getAll" resultType="student" >
       select id,name,email,age
       from student
    </select>

    <!--
      按主键id查询学生信息
      Student getById(Integer id);
    -->
    <select id="getById" parameterType="int" resultType="student">
        select id,name,email,age
        from student
        where id=#{id}
    </select>

    <!--
      按学生名称模糊查询
      List<Student> getByName(String name);
    -->
    <select id="getByName" parameterType="string" resultType="student">
        select id,name,email,age
        from student
        where name like '%${name}%'
    </select>

    <!--
      增加学生
      int insert(Student stu);
      实体类:
        private Integer id;
        private String name;
        private String email;
        private Integer age;
    -->
    <insert id="insert" parameterType="student">
        insert into student (name,email ,age) values(#{name},#{email},#{age})
    </insert>
    <!--
      按主键删除学生
      int delete(Integer id);
    -->
    <delete id="delete" parameterType="int" >
        delete from student where id=#{id}
    </delete>
    <!--
      更新学生
      int update(Student stu);
    -->
    <update id="update" parameterType="student">
        update student set name=#{name},email=#{email},age=#{age}
        where id=#{id}
    </update>
</mapper>
~~~



## 测试类

~~~java
/**
 *
 */
public class MyTest {
    SqlSession sqlSession;

    @Before  //在所有的@Test方法执行前先执行的代码
    public void openSqlSession() throws IOException {
        //使用文件流读取核心配置文件SqlMapConfig.xml
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //创建SqlSessionFactory工厂
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //取出sqlSession的对象
        sqlSession = factory.openSession();
    }

    @After
    public void closeSqlSession(){
        //关闭sqlSession
        sqlSession.close();
    }

    @Test
    public void testGetAll() throws IOException {
        //完成查询操作
        List<Student> list = sqlSession.selectList("zar.getAll");
        list.forEach(student -> System.out.println(student));
    }

    @Test
    public void testGetById() throws IOException {
        //按主键查学生
        Student stu = sqlSession.selectOne("zar.getById",3);
        System.out.println(stu);
    }

    @Test
    public void testGetByName()throws IOException{

        //4.调用方法
        List<Student> list = sqlSession.selectList("zar.getByName","李");
        list.forEach(student -> System.out.println(student));

    }

    @Test
    public void testInsert()throws IOException{

        //4.调用方法
       int num = sqlSession.insert("zar.insert",new Student("haha666","haha@126.com",23));
       //切记切记切记:在所有的增删改后必须手工提交事务!!!
        sqlSession.commit();

    }

    @Test
    public void testDelete()throws IOException {

        //4.调用方法
        int num = sqlSession.delete("zar.delete",1);
        System.out.println(num);
        //切记切记切记:在所有的增删改后必须手工提交事务!!!
        sqlSession.commit();

    }

    @Test
    public void testUpdate()throws IOException {

        //4.调用方法
        int num = sqlSession.update("zar.update",new Student(3,"hehe","hehe@126.com",30));
        System.out.println(num);
        sqlSession.commit();

    }
}
~~~





# 三、动态代理

## 1.0 动态代理存在意义

  在三层架构中,业务逻辑层要通过接口访问数据访问层的功能.动态代理可以实现



---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727333.png)

---



业务逻辑层需要调用数据访问层的接口，此时需要动态代理将mapper.xml文件中的功能代理出来



## 2.0 动态代理的实现规范

- **引用**

MyBatis框架使用动态代理的方式来进行数据库的访问.
Mapper接口的开发相当于是过去的Dao接口的开发。由MyBatis框架根据接口定义创建动态代理对象，代理对象的方法体同Dao接口实现类的方法



- **规范**

  1)UsersMapper.xml文件与UsersMapper.java的接口必须同一个目录下.
  2)UsersMapper.xml文件与UsersMapper.java的接口的文件名必须一致,后缀不管.
  3)UserMapper.xml文件中标签的id值与与UserMapper.java的接口中方法的名称完全一致.
  4)UserMapper.xml文件中标签的parameterType属性值与与UserMapper.java的接口中方法的参数类型完全一致.
  5)UserMapper.xml文件中标签的resultType值与与UserMapper.java的接口中方法的返回值类型完全一致.
  6)UserMapper.xml文件中namespace属性必须是接口的完全限定名称com.fgcy.mapper.UsersMapper
  7)在SqlMapConfig.xml文件中注册mapper文件时,使用class=接口的完全限定名称com.fgcy.mapper.UsersMapper





## 4.0 #{}与${}

- ###### **#{}**     ==传参==

传参大部分使用#{}传参, 它的**底层使用的是PreparedStatement对象**,是安全的数据库访问  , **防止sql注入**.

如果入参是简单数据类型【即：parameterType= ‘8中基本数据类型 | String’ 】，#{}里可以任意写；【注意此时：parameterType中只能是一个类型参数】

但是如果入参是对象类型，则#{}里必须是对象的成员变量的名称，#{}可以有效防止sql注入。



- 演示

~~~xml
 <select id="getById" parameterType="int" resultType="users">  ===>入参类型是简单类型
        select id,username,birthday,sex,address
        from users
        where id=#{zar}  ===>随便写
  </select>

	<insert id="insert" parameterType="users" >  ===>入参是实体类
        insert into users (username, birthday, sex, address) values(#{userName},#{birthday},#{sex},#{address})  ==>成员变量名称
    </insert>
~~~

---

![image-20220513152156510](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727779.png)

---



- **${}**   ==字符串替换与拼接==

\${}主要是针对字符串拼接替换，如果入参是基本数据类型，${}里必须是value【mybatis3.5.1一下的版本】，否则随意

但是如果入参是对象类型，则${}里必须是对象的成员变量的名称。

${}还可以替换列名和表名，存在sql注入风险，尽量少用。



- 演示

~~~xml
	<select id="getByName" parameterType="string" resultType="users">  ===>入参是简单类型
        select id,username,birthday,sex,address
        from users
        where username like '%${zar}%'   ===>随便写
    </select> 
    


		//如果参数超过一个,则parameterType不写
       <select id="getByNameOrAddress" resultType="users">
        select id,username,birthday,sex,address
        from users
        where ${columnName} like concat('%',#{columnValue},'%')  ==>此处使用的是@Param注解里的名称
   	 </select> 
   	 
   
    List<Users> getByNameOrAddress(
            @Param("columnName")  ===>为了在sql语句中使用的名称
            String columnName,
            @Param("columnValue")   ===>为了在sql语句中使用的名称
            String columnValue);
~~~

---

![image-20220513152227214](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727962.png)

---



## 5.0 返回主键标签

在完成插入操作后，将生成的主键信息通过实体类对象返回，在进行后继关联插入操作时，不用再次访问数据库



- **主键回填**

在完成插入操作后，将生成的主键信息通过实体类对象返回，在进行后继关联插入操作时，不用再次访问数据库



- **自增id  【主键回填】**

~~~xml
  <!--在插入语句结束后, 返回自增的主键值到入参的users对象的id属性中.-->
  <insert id="insert" parameterType="users" >
	    <selectKey  keyProperty="id" resultType="int" order="AFTER">
	        select last_insert_id()
	    </selectKey>
        insert into users (username, birthday, sex, address) values(#{userName},#{birthday},#{sex},#{address})
  </insert>

<!--
  <selectKey>标签的参数详解:
        keyProperty: users对象的哪个属性来接返回的主键值
        resultType:返回的主键的类型
        order:在插入语句执行前,还是执行后返回主键的值 AFTER：先插入再返回主键 BEFORE: 先生成再完成插入
      	select last_insert_id()函数：返回最近一条插入记录的主键
-->
~~~

- **UUID 【生成主键】**

~~~XML
    <!--生成一个UUID作为主键，并回填到入参中-->
	<insert id="addUser" parameterType="users">
        <selectKey order="BEFORE" resultType="String" keyProperty="id">
            select uuid()
        </selectKey>
        insert into users (id,username, birthday, sex, address)
        values (#{id},#{username}, #{birthDate}, #{sex}, #{address})
    </insert>


<!--
这是一个全球唯一随机字符串,由36个字母数字中划线组.
  UUID uuid = UUID.randomUUID();
  System.out.println(uuid.toString().replace("-","").substring(20));

  select uuid()
-->
~~~



# 动态代理的Mybatis开发

## 新建项目添加依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.fgcy</groupId>
    <artifactId>mybatis_002</artifactId>
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

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.48</version>
        </dependency>
        
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
    </dependencies>

    
    <!--maven资源导出-->
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
                <directory>src/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
</project>

~~~

## 新建属性文件db.properties

~~~properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:13306/ssm?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=root123
~~~

## 新建环境配置文件(SqlMapConfig.xml)

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--读取jdbc.properties属性-->
    <properties resource="jdbc.properties"></properties>
    <!--设置日志输出-->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <!--注册实体类的别名-->
    <typeAliases>
        <package name="com.bjpowernode.pojo"></package>
    </typeAliases>
    <!--配置环境变量-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <!--
                    private String driver;
                    private String url;
                    private String username;
                    private String password;
                -->
                <property name="driver" value="${jdbc.driverClassName}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!--注册mapper.xml文件-->
    <mappers>

        <!--<package name="com.bjpowernode.mapper"></package>-->
        <!--<mapper class="com.bjpowernode.mapper.UsersMapper"/>-->
<!--        <mapper url="file:///D:\learn\动力节点\mybatis-张阿荣\04_project\04_project\mybatisall\mybatis_002_users\src\main\resources\com\bjpowernode\mapper\UsersMapper.xml"/>-->
<!--        <mapper resource="com/bjpowernode/mapper/UsersMapper.xml"/>-->
    </mappers>
</configuration>
~~~

## 新建可视化窗口



---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727322.png)

---





---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727891.png)

---



## 新建实体类

~~~java
package com.bjpowernode.pojo;

import java.util.Date;

/**
 *
 */
public class Users {
    private Integer id;
    private String userName;
    private Date birthday;
    private String sex;
    private String address;
}

~~~

## 新建mapper文件，完成数据库中的所有操作

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.bjpowernode.mapper.UsersMapper">

    <!--
      //查询全部用户信息
    List<Users> getAll();
    -->
    <select id="getAll" resultType="users" >
        select id,username,birthday,sex,address
        from users
    </select>

    <!--
      //根据用户主键查用户
    Users getById(Integer id);
    -->
    <select id="getById" parameterType="int" resultType="users">
        select id,username,birthday,sex,address
        from users
        where id=#{zar}
    </select>

    <!--
       //根据用户名模糊查询用户
    List<Users> getByName(String name);
    -->
    <select id="getByName" parameterType="string" resultType="users">
        select id,username,birthday,sex,address
        from users
        where username like '%${zar}%'
    </select>
    <!--
       //用户的更新
    int update(Users users);
    private Integer id;
    private String userName;
    private Date birthday;
    private String sex;
    private String address;
    -->
    <update id="update" parameterType="users" >
        update users set username = #{userName},birthday=#{birthday},sex=#{sex},address=#{address}
        where id=#{id}
    </update>


    <!--
      //增加用户
    int insert(Users users);
    -->
    <insert id="insert" parameterType="users" >
    <selectKey  keyProperty="id" resultType="int" order="AFTER">
        select last_insert_id()
    </selectKey>
        insert into users (username, birthday, sex, address) values(#{userName},#{birthday},#{sex},#{address})
    </insert>

    <!--
       //根据主键删除用户
    int delete(Integer id);
    -->
    <delete id="delete" parameterType="int" >
        delete from users
        where id=#{id}
    </delete>

    <!--
       //优化后的模糊查询
    List<Users> getByNameGood(String name);
    -->
    <select id="getByNameGood" parameterType="string" resultType="users">
        select id,username,birthday,sex,address
        from users
        where username like concat('%',#{name},'%')
    </select>

    <!--
    //模糊用户名和地址查询
    //如果参数超过一个,则parameterType不写
    List<Users> getByNameOrAddress(
            @Param("columnName")
            String columnName,
            @Param("columnValue")
            String columnValue);
    -->
    <select id="getByNameOrAddress" resultType="users">
        select id,username,birthday,sex,address
        from users
        where ${columnName} like concat('%',#{columnValue},'%')
    </select>
</mapper>
~~~

## 新建测试类

~~~java
/**
 *
 */
public class MyTest {
    SqlSession sqlSession;

    @Before  //在所有的@Test方法执行前先执行的代码
    public void openSqlSession() throws IOException {
        //使用文件流读取核心配置文件SqlMapConfig.xml
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //创建SqlSessionFactory工厂
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //取出sqlSession的对象
        sqlSession = factory.openSession();
    }

    @After
    public void closeSqlSession(){
        //关闭sqlSession
        sqlSession.close();
    }

    @Test
    public void testGetAll() throws IOException {
        //完成查询操作
        //namespace.id
        List<Student> list = sqlSession.selectList("zar.getAll");
        list.forEach(student -> System.out.println(student));
    }

    @Test
    public void testGetById() throws IOException {
        //按主键查学生
        Student stu = sqlSession.selectOne("zar.getById",3);
        System.out.println(stu);
    }

    @Test
    public void testGetByName()throws IOException{

        //4.调用方法
        List<Student> list = sqlSession.selectList("zar.getByName","李");
        list.forEach(student -> System.out.println(student));

    }

    @Test
    public void testInsert()throws IOException{

        //4.调用方法
       int num = sqlSession.insert("zar.insert",new Student("haha666","haha@126.com",23));
       //切记切记切记:在所有的增删改后必须手工提交事务!!!
        sqlSession.commit();

    }

    @Test
    public void testDelete()throws IOException {

        //4.调用方法
        int num = sqlSession.delete("zar.delete",1);
        System.out.println(num);
        //切记切记切记:在所有的增删改后必须手工提交事务!!!
        sqlSession.commit();

    }

    @Test
    public void testUpdate()throws IOException {

        //4.调用方法
        int num = sqlSession.update("zar.update",new Student(3,"hehe","hehe@126.com",30));
        System.out.println(num);
        sqlSession.commit();

    }
}

~~~



# 四、动态sql

## 1.0 概念

动态 SQL 是 MyBatis 的强大特性之一
可以定义代码片断,可以进行逻辑判断,可以进行循环处理(批量处理),使条件判断更为简单



#  \<sql>标签

**用来定义代码片断**,可以将所有的列名,或复杂的条件定义为代码片断,供使用时调用
当多种类型的查询语句的查询字段或者查询条件相同时，可以将其定义为**常量**，方便调用

~~~xml
   <!--定义代码片断-->
    <sql id="allColumns">
        id,username,birthday,sex,address
    </sql>
~~~



# \<include>标签

用来**引用\<sql>定义的代码**片断

~~~xml
	//引用定义好的代码片断
   <select id="getAll" resultType="users" >
        select <include refid="allColumns"></include>
        from users
    </select>
~~~



# \<if>标签

\<if>:进行条件判断
    属性：test条件判断的取值可以是**实体类的成员变量**,可以是**map的key**,可以是**@Param注解的名称**.

​	**属性test必须有值，里面的取值不需要${}或#{}，可以直接使用实体类的成员变量，map的key，有@Param("xxxx")标注的变量**

~~~xml
        <if test="userName != null and userName != ''">
           and username like concat('%',#{userName},'%')
        </if>
~~~



# \<where>标签

\<where>:进行**多条件拼接**,在查询,删除,更新中使用.

一般开发复杂业务的查询条件时，如果有多个查询条件，通常会使用\<where>标签来进行控制。 

**标签可以自动的将第一个条件前面的逻辑运算符 (or ,and) 去掉**，正如代码中写的，id 查询条件前面是有“and”关键字的

但是在打印出来的 SQL 中却没有，这就是\<where> 的作用。

**where标签可以过滤掉 多余的 and 和 or**

```xml
<if>配<where>

<select id="getByCondition" parameterType="users" resultType="users">
    select <include refid="allColumns"></include>
    from users
    <where>
        <if test="userName != null and userName != ''">
           and username like concat('%',#{userName},'%')
        </if>
        <if test="birthday != null">
           and birthday = #{birthday}
        </if>
        <if test="sex != null and sex != ''">
           and sex = #{sex}
        </if>
        <if test="address != null and address != ''">
            and address like concat('%',#{address},'%')
        </if>
    </where>
</select>
```



# \<set>标签

\<set>:有选择的进行更新处理,至少更新一列.能够保证如果没有传值进来,则数据库中的数据保持不变.

**使用set标签可以将动态的配置 SET 关键字**，并**剔除追加到条件末尾的任何不相关的逗号**。

使用 if+set 标签修改后，在进行表单更新的操作中，哪个字段中有值才去更新，如果某项为 null 则不进行更新，而是保持数据库原值。

切记：**至少更新一列。否则报错·······** 可以过滤掉多余的逗号

~~~xml
  	<if> 配 <set>

	<update id="updateBySet" parameterType="users">
        update users
        <set>
            <if test="userName != null and userName != ''">
               username = #{userName},
            </if>
            <if test="birthday != null">
                birthday = #{birthday},
            </if>
            <if test="sex != null and sex != ''">
                sex = #{sex},
            </if>
            <if test="address != null and address != ''">
                address =#{address} ,
            </if>
        </set>
        where id = #{id}
    </update>
~~~



# \<foreach>标签

## **属性详解**

\<foreach>主要用来进行集合或数组的遍历

主要有以下参数：

​	collection：入参类型 属性的值有三个分别是 **list、array、map 三种**，分别对应的参数类型为：**List、数组、map 集合**。【parameter中没有arry数组类型，						如果要使用数组，parameter必须不写】

​	item ：循环体中的具体对象或值。支持属性的点路径访问，如item.age,item.info.details，**在list和数组中是其中的对象**，**在map中是value**。

​	index ：**在list和数组中,index是元素的序号**，**在map中，index是元素的key，该参数可选**。

​	open ：表示该**语句以什么开始**

​	close ：表示该**语句以什么结束**

​	separator ：表示**元素之间的分隔符**

​			例如在in()的时候，separator=","会自动在元素中间用“,“隔开，避免手动输入逗号导致sql错误，如in(1,2,)这样。该参数可选



## **批量查找**

~~~xml
    <!--
    parameterType=''除了是实体类之外其他的都不用写
    8个基本数据类型 + String 当作是普通类型
    当参数是普通类型时，且只有一个，可写可不写
    当参数不止一个，接口中要用@Param注解
    当参数是 list map 时可以不用写
    不能这么写--》parameterType="array" 因为mybatis没有数组类型 当只有一个入参时，可以不写（除非这是实体类）
    -->
    <select id="getByIds" resultType="users">
        select
        <include refid="allColumns"/>
        from users
        where id in
        <foreach collection="array" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </select>
~~~

## **批量删除**

~~~xml
    <!--
		#{}占位符、从入参中取值
		${}字符串拼接、字符串替换
	-->
	<delete id="deleteBatch">
        delete
        from users
        where id in
        <foreach collection="array" open="(" close=")" separator="," item="id">
            #{id}
        </foreach>
    </delete>
~~~

## **批量添加**

~~~xml
   <!--
		#{}占位符、从入参中取值
		${}字符串拼接、字符串替换
	-->
	<insert id="addBatch">
        insert into users (username, birthday, sex, address) values
        <!--
 			批量添加不用open close 因为这是在整个循环之前和之后添加的
			而我们需要的是这种：insert into users (username, birthday, sex, address) values(?,?,?,?),(?,?,?,?),(?,?,?,?)
			而不是这种：insert into users (username, birthday, sex, address) values( ?,?,?,?,  ?,?,?,? ,  ?,?,?,? )
		-->
        <foreach collection="list" item="user" separator=",">
            (#{user.username},#{user.birthDate},#{user.sex},#{user.address})
        </foreach>
    </insert>
~~~

## **批量更新**

~~~xml
	<update id="updateBatch">
        <foreach collection="list" separator=";" item="user">
            update users
            <set>
                <if test="user.username!=null and user.username!=''">
                    username=#{user.username},
                </if>
                <if test="user.birthDate!=null">
                    birthday=#{user.birthDate},
                </if>
                <if test="user.sex!=null and user.sex!=''">
                    sex=#{user.sex},
                </if>
                <if test="user.address!=null and user.address!=''">
                    address=#{user.address}
                </if>
            </set>
            where id = #{user.id}
        </foreach>
    </update>
~~~

**注意：**

要使用批量更新，必须在jdbc.properties属性文件中的url中添加&allowMultiQueries=true，允许多行操作



# 指定参数位置



## 使用场景

如果入参是多个,可以通过指定参数位置进行传参. 
mybatis-3.3 版本和之前的版本使用#{0},#{1}方式， 从 mybatis3.4 开始使用#{arg0}方式。



## 指定参数位置获取值

~~~xml
例如:查询指定日期范围内的用户信息.
    <!--
     		//查询指定日期范围内的用户
    		List<Users> getByBirthday(Date begin, Date end);
    -->
    <select id="getByBirthday" resultType="users">
        select <include refid="allColumns"></include>
        from users
        where birthday between #{arg0} and #{arg1}
    </select>
~~~



## @Param指定参数名称

~~~xml
<!--
        List<Users> getByColunm(@Param("columnName") String columnName,
                             	@Param("columnValue") String columnValue);
-->

<select id="getByColunm" resultType="users">
    select <include refid="columns"></include>
    from users
    where ${columnName} =#{columnValue}
</select>
~~~



# 入参是map

## 使用场景

入参是map,是因为当传递的数据有多个,不适合使用指定下标或指定名称的方式来进行传参【使用下标的方式是语义不明；使用指定名称的方式比较麻烦，每个参数都要加注解】

又加上参数不一定与对象的成员变量一致,考虑使用map集合来进行传递

map使用的是键值对的方式.当在sql语句中使用的时候**#{键名},${键名}**,用的是键的名称.

  **如果入参超过一个以上,使用map封装查询条件,更有语义,查询条件更明确.**

~~~java
    @Test
    public void testGetUsersByDate() throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date begin = sdf.parse("2000-01-01");
        Date end = sdf.parse("2010-01-01");
        HashMap<String, Date> map = new HashMap<>();
        map.put("begin", begin);
        map.put("end", end);
        List<Users> users = mapper.getUsersByDates(map);
        users.forEach(System.out::println);
    }
~~~

~~~xml
    <select id="getUsersByDates" resultType="users">
        select
        <include refid="allColumns"/>
        from users
        where birthday between #{begin} and #{end}
    </select>
~~~



# 返回值是map



## **使用场景**

返回值是map的适用场景,如果数据不能使用对象来进行封装,可能查询的数据来自多张表中的某些列

这种情况下,使用map,但是map的返回方式破坏了对象的封装,返回来的数据是一个一个单独的数据, 之间不相关

**map使用表中的列名或别名做为键名进行返回数据.**



## **map封装返回值是一行**

~~~xml
    <select id="GetOneMap" resultType="map">
        select username, address
        from users
        where id = #{id}
    </select>
~~~



~~~java
@Test
public void testGetReturnMapOne(){
    Map<String,Object> map = mapper.GetOneMap(7);
    System.out.println(map);
    System.out.println(map.get("username"));
}
~~~



## **返回值是map多行**

~~~xml
    <!--这里resultType是泛型-->
	<select id="getMaps" resultType="map">
        select username, address
        from users
    </select>
~~~



~~~java
@Test
public void testGetReturnMap(){
    List<Map<String,Object>> list = mapper.getMaps();
    list.forEach(map-> System.out.println(map));
}
~~~



# 列名与类中成员变量名称不一致

## 解决方案一

~~~
使用列的别名，别名与类中的成员变量名一样，即可完成注入
~~~

~~~xml
<select id="getAll" resultType="book">
    select bookid id,bookname name
    from book
</select>
~~~





## 解决方案二

使用\<resultMap>标签进行映射

~~~xml
    <select id="getAllBook" resultMap="bookmap">
        select bookid, bookname
        from book
    </select>

    <resultMap id="bookmap" type="book">
        <id property="id" column="bookid"/>
        <id property="name" column="bookname"/>
    </resultMap>
~~~



# 五、表的关联关系

## 1.0 概念

我们通常说的关联关系有以下四种，一对多关联，多对一关联，一对一关联，多对多关联。关联关系是有方向的

**如果是高并发的场景中，不适合做表的关联**

**关联关系是有方向的.**

​	  1)一对多关联:一个老师可以教多个学生,多个学生只有一个老师来教,**站在老师方,就是一对多关联.**

​	  2)多对一关联:一个老师可以教多个学生,多个学生只有一个老师来教,**站在学生方,就是多对一关联.**

​	  3)一对一关联:一个学生对应一个学号，一个学号对应一个学生

 	4)多对多关联:一个学生可以选多门课，一门课可以被多个学生选



# 多对一关联

在多对一关联关系中，多方(订单)中持有一方(客户)的对象，要使用标签\<association>标签来映射一方的属性。

**\<association>在\<resultMap>标签中使用 使用javaType指定实体类的类型**

一个客户有多张订单 一张订单对应一个客户



实体类

~~~java
public class Customer {
    private Integer id;
    private String name;
    private Integer age;
    private List<Order> orders;
}
~~~

~~~java
public class Order {
    private Integer id;
    private String orderNumber;
    private Double orderPrice;
    private Customer customer;
}
~~~



mapper

~~~xml
 <resultMap id="orderMap" type="order">
        <id property="id" column="oid"/>
        <result property="orderNumber" column="orderNumber"/>
        <result property="orderPrice" column="orderPrice"/>
     
       <!--association 是多对一 -->
        <association property="customer" javaType="customer">
            <id property="id" column="cid"/>
            <result property="name" column="name"/>
            <result property="age" column="age"/>
        </association>
     
</resultMap>


    <select id="getOrderById" resultMap="orderMap">
        select c.id cid, c.name, c.age, o.id oid, o.orderNumber, o.orderPrice
        from orders o
                 inner join customer c on o.customer_id = c.id
        where o.id = #{oid}
    </select>
~~~

# 一对多关联

**某方持有一方集合**

在一对多关联关系中，一方(客户)中有多方(订单)的集合 ，所以要使用\<collection>标签来映射多方的属性

**\<collection>在\<resultMap>标签中使用   ； 使用ofType指定实体类的类型**



实体类

~~~java
public class Customer {
    private Integer id;
    private String name;
    private Integer age;
    private List<Order> orders;
}
~~~



~~~java
public class Order {
    private Integer id;
    private String orderNumber;
    private Double orderPrice;
    private Customer customer;
}
~~~



mapper

~~~xml
	<resultMap id="customermap" type="customer">
        <id property="id" column="cid"/>
        <result property="name" column="name"/>
        <result property="age" column="age"/>
        
        <!--collection 是一对多 -->
        <collection property="orders" ofType="order">
            <id property="id" column="oid"/>
            <result property="orderNumber" column="orderNumber"/>
            <result property="orderPrice" column="orderPrice"/>
        </collection>
        
    </resultMap>


    <select id="getCustomerById" resultMap="customermap">
        select c.id cid, c.name, c.age, o.id oid, o.orderNumber, o.orderPrice
        from customer c
                 left join orders o on c.id = o.customer_id
        where c.id = #{id}
    </select>
~~~

# 一对一关联

**某方持有一方对象**



实体类

~~~java
public class Classes {
    private int cid;
    private String cname;
    private Teacher teacher；
} 
~~~



mapper

~~~xml
<resultMap id="classesmap" type="classes">
    <id property="cid" column="cid"></id>
    <result property="cname" column= "cname "></result>
    
    <association property="teacher" javaType="teacher">
        <id property="tid" column="tid"></id>
        <result property="tname" column="tname"></result>
        <result property="age" column="tage"></result>
    </association>
    
</resultMap>

<select id="findById" parameterType="int" resultMap="classesmap">
    select c.*,t.* 
    from classes c, teacher t 
    where c.ctid=t.tid and c.cid=#{cid}
</select>
~~~



# 多对多关联

多对多关联中，需要通过中间表化解关联关系。

中间表描述两张主键表的关联。

**中间表没有对应的实体类**。但有表

Mapper.xml文件中也没有中间表的对应标签描述，只是在查询语句中使用中间表来进行关联 



一本书有多个分类，一个分类有多本书



- 实体类

~~~java
public class Book {
    private int bid;
    private String bname;
    //目的:就是在查询图书时,将图书所属的类型全部取出来
    private List<category> categoryList=new ArrayList<>();
}
~~~

- mapper

~~~xml
<resultMap id="bookmap" type="book">
    <id property="bid" column="bid"></id>
    <result property="bname" column="bname"></result>
    
    <collection property="categoryList" ofType="category">
        <id property="cid" column="cid"></id>
        <result property=" cname" column="cname "></result> 
        
    </collection>
</resultMap>

<select id="findALL" resultMap="bookmap">
select * 
    from book b inner join middle m 
    	on b.bid = m.m_bid 
    inner join category c 
    	on m.m_cid = c.cid
</select>
~~~



- book

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727356.png)

---





- category

----

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727947.png)

---





- 中间表(关联表)

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727510.png)

---





- 查询结果

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727015.png)

---



因为book表和category表中都没有存储外键关联其他表，所以只能借助中间表进行关联查询



# 嵌套查询

- mapper1

~~~xml
    <resultMap id="customermap2" type="customer">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="age" column="age"/>
        
    <!--
		property=“ordersList”表示的是实体类customer的属性ordersList
		ofType="orders"表示的是集合中的泛型是Order类型
		column="id"表示的是关联列，该值由使用该resultMap的select语句中提供
		select="selectordersByCustomerId"代表执行的是哪一个mapper下的select语句
	-->
   
    <!--column:是当前客户表的id,传给嵌套查询作为入参进行查询返回该客户名下的所有订单集合-->
   <collection property="orders" ofType="order" column="id"
                    select="com.fgcy.mapper.OrderMapper.getOrdersByCustomerId"/>
</resultMap> 

    <select id="getCustomerByIdPro" resultMap="customermap2">
        select id,name,age
        from customer
        where name like concat('%',#{name},'%')
    </select>
~~~

- mapper2

~~~xml
    <select id="getOrdersByCustomerId" resultType="order">
        select id, orderNumber, orderPrice
        from orders
        where customer_id = #{id}
    </select>
~~~



# 总结

无论是什么关联关系，如果某方持有另一方的**集合**，则使用\<collection>标签完成映射  **使用 ofType指定泛型类型**

如果某方持有另一方的**对象**，则使用\<association>标签完成映射。 **使用javaType指定对象类型**



# 六、事务



## 1.0 什么是事务

一个完整的逻辑单元，不可再分；

多个操作同时完成,或同时失败称为事务处理.

事务有四个特性:一致性,持久性,原子性,隔离性.



## 2.0 Mybatis中事务

在事务管理器中设置

Mybatis 框架是对 JDBC 的封装，所以 Mybatis 框架的事务控制方式有两种

一种是容器进行事务管理的【MANAGED】，一种是程序员手工决定事务的提交与回滚【JDBC】



在MyBatis框架中设置事务

~~~xml
  <transactionManager type="JDBC"></transactionManager>  ===>程序员自己控制处理的提交和回滚
~~~



  可设置为自动提交

~~~java
  sqlSession = factory.openSession();  ===>默认是手工提交事务,设置为false也是手工提交事务,如果设置为true,则为自动提交.
  sqlSession = factory.openSession(true);  ===>设置为自动提交,在增删改后不需要commit();
~~~



---

![image-20220513161716308](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727376.png)

---

Connection 对象的 setAutoCommit()方法来设置事务提交方式的。自动提交和手工提交。\<transactionManager>标签用于指定MyBatis 所使用的事务管理器。

MyBatis 支持两种事务管理器类型：JDBC、MANAGED

JDBC：使用 JDBC 的事务管理机制。即，通过 Connection 的 commit()方法提交，通过 rollback()方法回滚。

但默认情况下，MyBatis 将自动提交功能关闭了，改为了手动提交。即程序中需要显式的对事务进行提交或回滚。从日志的输出信息中可以看到。

MANAGED：由容器管理

**所以：切记切记进行完增删改后一定要sqlSession.commit();**



在获得SqlSession的时候,如果openSession()是无参或者是false,则必须手工提交事务



如果openSession(true),则为自动提交事务，在执行完增删改后无须commit(),事务自动提交。

session = factory.openSession(true);



# 七、缓存

## 1.0 缓存概念

将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询



从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题



mybatis提供查询缓存，用于减轻数据库压力，提高数据库性能



# 缓存执行机制

MyBatis框架提供两级缓存,一级缓存和二级缓存.默认开启一级缓存

缓存就是为了提交查询效率



## 使用mybatis查询数据的执行流程

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727327.png)

---

在进行数据库访问时，首先去访问缓存，如果缓存中有要访问的数据，则直接返回客户端；

如果没有则去访问数据库，在库中得到数据后，先在缓存放一份，再从缓存中返回客户端；

当数据库发生commit操作时，清空缓存



## 一级缓存与二级缓存的作用范围

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727546.png)

---



# 一级缓存

## 使用一级缓存查询数据执行流程

第一次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，如果没有，从数据库查询用户信息。

得到用户信息，将用户信息存储到一级缓存中。然后从缓存中返回；

如果sqlSession去执行commit操作（执行插入、更新、删除），则清空SqlSession中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，避免脏读。

第二次发起查询用户id为1的用户信息，先去找缓存中是否有id为1的用户信息，缓存中有，直接从缓存中获取用户信息。如果没有重复第一次查询操作

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261727686.png)

---



## 一级缓存验证(1)

~~~java
public class MyTe {
    private static SqlSession sqlSession;
    private static UsersMapper mapper;

    @BeforeClass
    public static void openSession() {
        try {
            final InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
            SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
            sqlSession = sessionFactory.openSession();
            mapper = sqlSession.getMapper(UsersMapper.class);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @AfterClass
    public static void closeSession() {
        sqlSession.close();
    }


    @Test
    public void test2() {
        System.out.println("FOR ONE");
        mapper.findAll().forEach(System.out::println);
        System.out.println("---------------------------------");
    }

    @Test
    public  void test1() {
        System.out.println("FOR TWO");
        mapper.findAll().forEach(System.out::println);
    }
}
~~~

- 结果1

~~~
Created connection 215219944.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==      Total: 9
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
FOR ONE
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
---------------------------------
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
Returned connection 215219944 to pool.
~~~



- 验证2

~~~java
public class MyTe {
    private static SqlSession sqlSession;
    private static UsersMapper mapper;

    @BeforeClass
    public static void openSession() {
        try {
            final InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
            SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
            sqlSession = sessionFactory.openSession();
            mapper = sqlSession.getMapper(UsersMapper.class);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @AfterClass
    public static void closeSession() {
        sqlSession.close();
    }


    @Test
    public void test1() {
        System.out.println("FOR ONE");
        mapper.findAll().forEach(System.out::println);
        System.out.println("---------------------------------");
    }

    @Test
    public void test2() {
        mapper.addUser(new Users("fff", new Date(), 1, "aaa"));
        sqlSession.commit();
    }

    @Test
    public void test3() {
        System.out.println("FOR TWO");
        mapper.findAll().forEach(System.out::println);
    }
}
~~~

- 结果2

~~~
Created connection 215219944.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==      Total: 9
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
---------------------------------
==>  Preparing: insert into users (id,username, birthday, sex, address) values (?,?, ?, ?, ?)
==> Parameters: null, fff(String), 2022-04-17 16:10:52.569(Timestamp), 1(Integer), aaa(String)
<==    Updates: 1
==>  Preparing: select last_insert_id()
==> Parameters: 
<==    Columns: last_insert_id()
<==        Row: 17
<==      Total: 1
Committing JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
FOR TWO
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==      Total: 10
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
Users{id=17, username='fff', birthDate=null, sex=1, address='aaa'}
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@cd3fee8]
Returned connection 215219944 to pool.
~~~



# 二级缓存

Mybatis默认开启一级缓存，但二级缓存不是；

mybaits的二级缓存是mapper范围级别，一级缓存是sqlSession级别

## 实现步骤

- 除了在SqlMapConfig.xml设置二级缓存的总开关

- 还要在具体的mapper.xml中开启二级缓存

- 并且要让实体类实现serializable接口

  

### 1 在核心配置文件加入设置

~~~xml
<settings>
    <setting name="cacheEnabled" value="true"/>
</settings>
~~~



### 2 在mapper.xml文件中二启二级缓存，使用

~~~xml
<mapper namespace="xxxx.xxxx.xxxx">
<!--<cache></cache>-->
<cache/>
.......
......
</mapper>
~~~



### 3 实体类必须实现java.io.serializable接口，保证实体可序列化



## 验证1

~~~java
public class TeMy {

    @Test
    public void test1() throws Exception {
        final InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
        SqlSession session1 = sessionFactory.openSession();
        SqlSession session2 = sessionFactory.openSession();
        SqlSession session3 = sessionFactory.openSession();
        UsersMapper mapper1 = session1.getMapper(UsersMapper.class);
        UsersMapper mapper2 = session2.getMapper(UsersMapper.class);
        UsersMapper mapper3 = session3.getMapper(UsersMapper.class);

        mapper1.findAll().forEach(System.out::println);
        session1.close();
        mapper3.findAll().forEach(System.out::println);
        session3.close();
    }
}
~~~

- 结果1

~~~
Created connection 1691185247.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==        Row: 17, fff, 2022-04-17, 1, aaa
<==      Total: 10
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Returned connection 1691185247 to pool.
As you are using functionality that deserializes object streams, it is recommended to define the JEP-290 serial filter. Please refer to https://docs.oracle.com/pls/topic/lookup?ctx=javase15&id=GUID-8296D8E8-2B93-4B9A-856E-0A65AF9B8C66
Cache Hit Ratio [com.fgcy.mapper.UsersMapper]: 0.5
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
~~~



- 验证2

~~~java
public class TeMy {

    @Test
    public void test1() throws Exception {
        final InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
        SqlSession session1 = sessionFactory.openSession();
        SqlSession session2 = sessionFactory.openSession();
        SqlSession session3 = sessionFactory.openSession();
        UsersMapper mapper1 = session1.getMapper(UsersMapper.class);
        UsersMapper mapper2 = session2.getMapper(UsersMapper.class);
        UsersMapper mapper3 = session3.getMapper(UsersMapper.class);

        mapper1.findAll().forEach(System.out::println);
        session1.close();

        mapper2.addUser(new Users("fff", new Date(), 1, "aaa"));
        session2.commit();
        session2.close();

        mapper3.findAll().forEach(System.out::println);
        session3.close();
    }
}
~~~

- 结果

~~~java
reated connection 1691185247.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==      Total: 10
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Returned connection 1691185247 to pool.
Opening JDBC Connection
Checked out connection 1691185247 from pool.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
==>  Preparing: insert into users (id,username, birthday, sex, address) values (?,?, ?, ?, ?)
==> Parameters: null, fff(String), 2022-04-17 16:40:34.247(Timestamp), 1(Integer), aaa(String)
<==    Updates: 1
==>  Preparing: select last_insert_id()
==> Parameters: 
<==    Columns: last_insert_id()
<==        Row: 20
<==      Total: 1
Committing JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Returned connection 1691185247 to pool.
Cache Hit Ratio [com.fgcy.mapper.UsersMapper]: 0.0
Opening JDBC Connection
Checked out connection 1691185247 from pool.
Setting autocommit to false on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
==>  Preparing: select id, username, birthday, sex, address from users
==> Parameters: 
<==    Columns: id, username, birthday, sex, address
<==        Row: 2, 张三, 2001-07-12, 1, 北京市
<==        Row: 3, 张小明, 1999-02-22, 1, 河南
<==      Total: 11
Users{id=2, username='张三', birthDate=null, sex=1, address='北京市'}
Users{id=3, username='张小明', birthDate=null, sex=1, address='河南'}

Resetting autocommit to true on JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Closing JDBC Connection [com.mysql.jdbc.JDBC4Connection@64cd705f]
Returned connection 1691185247 to pool.
~~~



# 八、ORM

## 1.0 ORM概念

(Object Relational Mapping):对象关系映射

  MyBatis框架是ORM非常优秀的框架.

**编写程序的时候，以面向对象的方式处理数据，保存数据的时候，却以关系型数据库的方式存储**



## 2.0 持久化操作

**持久化等同于ORM，将对象中的字段与数据库中的字段关联起来形成映射关系；**

包括两点：

​	**一是将内存中的对象保存到关系型数据库中**

​	**二是将数据库中的记录读取到内存中形成对象**

  MyBatis是持久化层优秀的框架.



---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261728556.png)

---

# 九、源码追踪

- **断点追踪**

~~~java
在这四个地方加断点：
	SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
    sqlSession = sessionFactory.openSession();
    mapper = sqlSession.getMapper(UsersMapper.class);
	List<Users> users = mapper.findAll();

使用ctrl+alt+ 《--

1）XMLConfigBuilder：进行sqlMapConfig.xml文件的解析的
=============》root的值
    <configuration>
    <properties resource="db.properties"/>    
    <settings>
        <setting name="cacheEnabled" value="true"/>        
        <setting name="lazyLoadingEnabled" value="true"/>        
        <setting name="mapUnderscoreToCamelCase" value="true"/>        
        <setting name="logImpl" value="STDOUT_LOGGING"/>        
    </settings>
    <typeAliases>
        <typeAlias alias="users" type="com.fgcy.pojo.Users"/>        
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>            
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driverClassName}"/>                
                <property name="url" value="${jdbc.url}"/>                
                <property name="username" value="${jdbc.username}"/>                
                <property name="password" value="${jdbc.password}"/>                
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper class="com.fgcy.mapper.UsersMapper"/>        
    </mappers>
</configuration>

2）解析properties
propertiesElement(root.evalNode("properties"));        
===============》context的值
    <properties resource="db.properties"/>
	context.getStringAttribute("resource");//db.properties
	this.parser.setVariables(defaults);
【
	defaults的值：
		"jdbc.url" -> "jdbc:mysql://localhost:13306/ssm?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true"
        "jdbc.username" -> "root"
        "jdbc.password" -> "root123"
        "jdbc.driverClassName" -> "com.mysql.jdbc.Driver"
 】
    this.configuration.setVariables(defaults);

3)解析typeAliases
===========>parent的值
    <typeAliases>
        <typeAlias alias="users" type="com.fgcy.pojo.Users"/>    
    </typeAliases>
===============》child值
    XNode child : parent.getChildren() //得到子节点
	<typeAlias alias="users" type="com.fgcy.pojo.Users"/>
     //使用别名和对象类型进行注册
     typeAliasRegistry.registerAlias(alias, clazz);

4）解析environments
==========》context的值
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>        
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>            
            <property name="url" 		      value="jdbc:mysql://localhost:13306/ssmuseUnicode=true&characterEncoding=utf8&allowMultiQueries=true"/>            
            <property name="username" value="root"/>            
            <property name="password" value="root123"/>            
        </dataSource>
    </environment>
</environments>

   environment = context.getStringAttribute("default");//development
	XNode child : context.getChildren() //遍历子节点
===========》第一个child的值
    <environment id="development">
    <transactionManager type="JDBC"/>    
    <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/>        
        <property name="url" value="jdbc:mysql://localhost:13306/ssm?allowMultiQueries=true"/>        
        <property name="username" value="root"/>        
        <property name="password" value="root123"/>        
    </dataSource>
</environment>
此时datasource对象初始成功
        
4）解析mapper
==============》parent的值        
        <mappers>
            <mapper class="com.fgcy.mapper.UsersMapper"/>    
        </mappers>
                
XNode child : parent.getChildren()//遍历子节点
==============》第一个child的值
  	<mapper class="com.fgcy.mapper.UsersMapper"/>
	//先手判断是不是标签名是不是package 优先级最高
    if ("package".equals(child.getName())) {
          String mapperPackage = child.getStringAttribute("name");
          configuration.addMappers(mapperPackage);
        } else {
          String resource = child.getStringAttribute("resource");
          String url = child.getStringAttribute("url");
          String mapperClass = child.getStringAttribute("class");
        //优先级第二resource
          if (resource != null && url == null && mapperClass == null) {
            ErrorContext.instance().resource(resource);
            InputStream inputStream = Resources.getResourceAsStream(resource);
            XMLMapperBuilder mapperParser = new XMLMapperBuilder(inputStream, configuration, resource, configuration.getSqlFragments());
            mapperParser.parse();
            //优先级第三url
          } else if (resource == null && url != null && mapperClass == null) {
            ErrorContext.instance().resource(url);
            InputStream inputStream = Resources.getUrlAsStream(url);
            XMLMapperBuilder mapperParser = new XMLMapperBuilder(inputStream, configuration, url, configuration.getSqlFragments());
            mapperParser.parse();
              //优先级第四class
          } else if (resource == null && url == null && mapperClass != null) {
            Class<?> mapperInterface = Resources.classForName(mapperClass);
              //将mapper注册给configuration
            configuration.addMapper(mapperInterface);
~~~

~~~java
6)SqlSession解析
alt + 7 列出本类或接口中的全部成员（属性+方法）
ctrl + h 列出本类或本接口中的实现类或子类，改变按钮可以的到父类或父接口

7）MapperProxy解析
	//拿到一个被动态代理的对象
    final MapperProxy<T> mapperProxy = new MapperProxy<>(sqlSession, mapperInterface, methodCache);
    return (T) Proxy.newProxyInstance(mapperInterface.getClassLoader(), new Class[] { mapperInterface }, mapperProxy);

8）MapperMethod：执行代理增强功能的类
    mapperProxy对象所属的类实现了InvocationHandler接口
    以下是invoke方法的具体逻辑：
            if (Object.class.equals(method.getDeclaringClass())) {
                return method.invoke(this, args);
            }

            if (this.isDefaultMethod(method)) {
                return this.invokeDefaultMethod(proxy, method, args);
            }
重点：
      MapperMethod mapperMethod = this.cachedMapperMethod(method);
      return mapperMethod.execute(this.sqlSession, args);
在MapperMethod类中：
        private final MapperMethod.SqlCommand command;//对象mapper.xml文件中的sql语句 静态内部类
    	private final MapperMethod.MethodSignature method;//对应mapper.java中的方法  静态内部类

    public static class SqlCommand {
        //sql语句的id属性值
        private final String name;
        //对象select标签
        private final SqlCommandType type;
    }
~~~

