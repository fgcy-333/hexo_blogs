---
title: SpringBoot
date: 2022-08-26 17:39:59
tags:
- 动力节点-王鹤-SpringBoot
categories: 
- 视频学习笔记
---

# 第一章  JavaConfig

1. 为什么要使用 Spring Boot

   因为Spring， SpringMVC 需要使用的大量的配置文件 （xml文件）

   还需要配置各种对象，把使用的对象放入到spring容器中才能使用对象

   需要了解其他框架配置规则

   

2. SpringBoot 就相当于 **不需要配置文件的Spring+SpringMVC**。 **常用的框架和第三方库都已经配置好了**。

   拿来就可以使用了**。【开箱即用】**

   

3. SpringBoot开发效率高，使用方便多了



<!--more-->

## 1.1 JavaConfig

JavaConfig: **使用java类作为xml配置文件**的替代， 是**配置spring容器的纯java的方式**【之前是根据xml配置创建容器】

在这个java类这可以创建java对象，把对象放入spring容器中（注入到容器）



使用两个注解：

1）@Configuration ： 放在一个类的上面，**表示这个类是作为配置文件使用的**。

2）@Bean：**声明对象，把对象注入到容器中**。



```java
例子：
package com.bjpowernode.config;

import com.bjpowernode.vo.Student;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Configuration:表示当前类是作为配置文件使用的。 就是用来配置容器的
 *       位置：在类的上面
 *
 *  SpringConfig这个类就相当于beans.xml
 */
@Configuration
public class SpringConfig {

    /**
     * 创建方法，方法的返回值是对象。 在方法的上面加入@Bean
     * 方法的返回值对象就注入到容器中。
     *
     * @Bean: 把对象注入到spring容器中。 作用相当于<bean>
     *     位置：方法的上面
     *     说明：@Bean,不指定对象的名称，默认是方法名是 id
     */
    @Bean
    public Student createStudent(){
        Student s1  = new Student();
        s1.setName("张三");
        s1.setAge(26);
        s1.setSex("男");
        return s1;
    }


    /**
     * 指定对象在容器中的名称（指定<bean>的id属性）
     * @Bean的name属性，指定对象的名称（id）
     */
    @Bean(name = "lisiStudent")
    public Student makeStudent(){
        Student s2  = new Student();
        s2.setName("李四");
        s2.setAge(22);
        s2.setSex("男");
        return s2;
    }
}

```



~~~java
	/**
     * 使用AnnotationConfigApplicationContext
     */
    @Test
    public void test02(){
        ApplicationContext ctx  = new AnnotationConfigApplicationContext(SpringConfig.class);
        Student student = (Student) ctx.getBean("createStudent");//根据名字获取bean
        System.out.println("使用JavaConfig创建的bean对象："+student);
    }

    @Test
    public void test03(){
        ApplicationContext ctx  = new AnnotationConfigApplicationContext(SpringConfig.class);
        Student student = (Student) ctx.getBean("lisiStudent");
        System.out.println("使用JavaConfig创建的bean对象："+student);
    }

~~~







## 1.2 @ImporResource

@ImportResource 作用导入其他的xml配置文件， 等于 在xml 

```xml
<import resources="其他配置文件"/>
```

例如：

- 配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="myCat" class="com.bjpowernode.vo.Cat">
        <property name="name" value="tom猫"/>
        <property name="age" value="2" />
        <property name="cardId" value="uw532423422"/>
    </bean>
</beans>
~~~



- java类

```java
@Configuration
//@ImportResource(value ={ "classpath:applicationContext.xml","classpath:beans.xml"})可以导入多个
@ImportResource(value ={ "classpath:applicationContext.xml"})//<import resource="classpath:beans.xml" />
public class SpringConfig {
    
}
```

- 测试类

~~~java
    @Test
    public void test04(){
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        Cat cat  = (Cat) ctx.getBean("myCat");
        System.out.println("cat=="+cat);
    }
~~~



- 小结：

​	@ImportResource将xml文件(spring的核心配置文件)导入到java类中(该类是配置类)；

​	可以通过配置类加载spring容器，要使用AnnotationConfigApplicationContext(注解配置应用容器)





## 1.3 @PropertyResource

@PropertyResource: **读取properties属性配置文件**。 使用属性配置文件可以实现外部化配置 ，

在程序代码之外提供数据。



步骤：

1. 在resources目录下，创建properties文件， 使用k=v的格式提供数据
2. 在PropertyResource 指定properties文件的位置
3. 使用@Value（value="${key}"）



- 装载数据的实体类

~~~java
@Component("tiger")//在配置类中配置包扫描，扫这个包；将其交给容器管理
public class Tiger {

    @Value("${tiger.name}")//从配置文件中获取值装配到属性中
    private String name;
    @Value("${tiger.age}")
    private Integer age;

    @Override
    public String toString() {
        return "Tiger{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
~~~

- 配置类

```java
@Configuration
//<context:property-placeholder location="classpath:config.properties" />
@PropertySource(value = "classpath:config.properties") 
//<context:component-scan base-package="com.bjpowernode.vo" />
@ComponentScan(basePackages = "com.bjpowernode.vo")
public class SpringConfig {
    
}




```

- properties文件

~~~properties
tiger.name=拉欧
tiger.age=3
~~~

- 测试类

~~~java
    @Test
    public void test05(){
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
       Tiger tiger = (Tiger) ctx.getBean("tiger");
        System.out.println("tiger=="+tiger);
    }
~~~



- 小结

​	@PropertiesResource，将properties文件导入到java类中(该类得是配置类),通过该类创建容器；

​	使用@Value("${xxx.xxx}")获取属性，当然要使用@Value 要保证该类被容器接管





# 第二 章 Spring Boot

## 2.1 介绍

SpringBoot是Spring中的一个成员， 可以**简化Spring，SpringMVC的使用**。 他的**核心还是IOC容器**。



特点：

- Create stand-alone Spring applications

  创建spring应用

  

- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)

  内嵌的tomcat， jetty ， Undertow 

  

- Provide opinionated 'starter' dependencies to simplify your build configuration

  提供了**starter起步依赖**，**简化应用的配置**。   

  比如使用MyBatis框架 ， 需要**在Spring项目中，配置MyBatis的对象 SqlSessionFactory** ， Dao的代理对象

  在SpringBoot项目中，在pom.xml里面, 加入一个 mybatis-spring-boot-starter依赖

  

- Automatically configure Spring and 3rd party libraries whenever possible

  尽可能去配置spring和第三方库。叫做自动配置（就是把**spring中的，第三方库中的对象都创建好，放到容器中**， 开发人员可以直接使用）

  

- Provide production-ready features such as metrics, health checks, and externalized configuration

  提供了健康检查， 统计，外部化配置

  

- Absolutely no code generation and no requirement for XML configuration

  不用生成代码， 不用使用xml，做配置



## 2.2 创建Spring Boot项目

### 2.2.1 第一种方式， 使用Spring提供的初始化器， 就是向导创建SpringBoot应用



使用的地址： https://start.spring.io



SpringBoot项目的结构：

---

![image-20210115152427829](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261743053.png)

---





### 2.2.2  使用国内的地址下载模板

> https://start.springboot.io/





### 2.2.3 下载文件

1、打开这个：

> https://start.springboot.io



---

![image-20220516201553523](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261743314.png)

---



2、下载生成的文件

3、idea导入



### 2.1.4使用基本的maven框架搭建springboot项目

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--SpringBoot的父项目 管理SpringBoot的依赖和版本-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.2</version>
        <relativePath/> 
    </parent>

    <!--当前项目的gav-->
    <groupId>com.bjpowernode</groupId>
    <artifactId>004-springboot-three</artifactId>
    <version>1.0.0</version>

    <!--SpringBootweb的启动依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
~~~

- SpringBoot的基本依赖

---

![image-20220516203752901](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261742204.png)

---

SpringBoot的启动依赖

json的启动依赖

内置tomcat的启动依赖

spring-web依赖

spring-webmvc依赖



- 项目结构

---

![image-20220516203909787](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261742087.png)

---





## 2.3  注解的使用

@SpringBootApplication

复合注解

由

~~~
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
~~~

组成



@SpringBootConfiguration


~~~java
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
~~~

说明：使用了@SpringBootConfiguration注解标注的类，可以作为配置文件使用的，可以使用@Bean声明对象，注入到容器

可以将主启动类当做是配置类，在里面声明方法：将对象交给容器管理【@bean】



@EnableAutoConfiguration

启用自动配置， 把java对象配置好，注入到spring容器中。例如可以把mybatis的对象创建好，放入到容器中



@ComponentScan

@ComponentScan 扫描器，找到某个包下的注解，根据注解的功能创建对象，给属性赋值等等。
**默认扫描的包：有该注解 @ComponentScan所在的类所在的包和子包**



## 2.4 SpringBoot的配置文件

配置文件名称： application

扩展名有： properties( key=value) ;  yml ( key: value)

使用application.properties,   application.yml



例1：application.properties设置 端口和上下文

```properties
#设置端口号
server.port=8082
#设置访问应用上下文路径， contextpath
server.servlet.context-path=/myboot
```



例2： application.yml

```yaml
server:
  port: 8083
  servlet:
    context-path: /myboot2
```

注意：

​	层级关系：空两个tab

​	键之后：空一格



## 2.5 多环境配置

有开发环境， 测试环境， 上线的环境。

每个环境有不同的配置信息， 例如端口， 上下文件， 数据库url，用户名，密码等等



**使用多环境配置文件，可以方便的切换不同的配置**。

使用方式： 创建多个配置文件， **名称规则**： application-**环境名称**.properties(yml)

创建开发环境的配置文件： application-dev.properties(  application-dev.yml )

创建测试者使用的配置： application-test.properties



- 在application.properties配置激活某个配置文件

~~~properties
#spring.profiles.active=dev

spring.profiles.active=test
~~~



## 2.6 @ConfigurationProperties

**@ConfigurationProperties: 把配置文件的数据映射为java对象**。

**属性：prefix 配置文件中的某些key的开头的内容**。**【必须要加前缀】**

- 实体类

```java
@Component
@ConfigurationProperties(prefix = "school")
public class SchoolInfo {

    private String name;

    private String website;

    private String address;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getWebsite() {
        return website;
    }

    public void setWebsite(String website) {
        this.website = website;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "SchoolInfo{" +
                "name='" + name + '\'' +
                ", website='" + website + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

```



- application.properties

```properties
#配置端口号
server.port=8082
#context-path
server.servlet.context-path=/myboot

#自定义key=value
school.name=动力节点
school.website=www.bjpowernode.com
school.address=北京的大兴区

site=www.bjpowernode.com
```



- 控制器

~~~java
@Controller
public class HelloController {

    @Resource//先byName再byType
    private SchoolInfo info;

    @RequestMapping("/info")
    @ResponseBody
    public String  queryInfo(){
        return "SchoolInfo对象=="+info.toString();
    }
}

~~~

- 小结：

  ​	将@configurationproperties注解加到普通java文件中，就可以将核心配置文件中的以某个字符开头的属性映射到有该注解的java类中

  ​	同样的，该类需要被spring管理





## 2.6.1使用@value()可以直接使用application中的kv键值对

- application.properties

~~~properties
#配置端口号
server.port=8082
#context-path
server.servlet.context-path=/myboot

#自定义key=value
school.name=动力节点
school.website=www.bjpowernode.com
school.address=北京的大兴区

site=www.bjpowernode.com
~~~



- 控制器

~~~java
@Controller
public class HelloController {


    @Value("${server.port}")
    private Integer port;

    @Value("${server.servlet.context-path}")
    private String contextPath;

    @Value("${school.name}")
    private String name;

    @Value("${site}")
    private String site;


    @RequestMapping("/data")
    @ResponseBody
    public String  queryData(){
        return name+",site="+site+", 项目的访问地址="+contextPath+",使用的端口="+port;
    }

}

~~~



注意：使用@value("${}")所在的类需要交由spring管理



## 2.7  使用jsp

**SpringBoot不推荐使用jsp** ，而是**使用模板技术代替jsp**



使用jsp需要配置：

1） 加入一个处理jsp的依赖。 负责编译jsp文件

```xml
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
```



2) 如果需要使用servlet， jsp，jstl的功能

```xml
<!--java的标准标签库依赖-->
<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>jstl</artifactId>
</dependency>

<dependency>
	<groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
</dependency>

<!--要使用jsp的九大内置对象和四大作用域；且想要代码提示；需加上jsp依赖-->
<dependency>
<groupId>javax.servlet.jsp</groupId>
	<artifactId>javax.servlet.jsp-api</artifactId>
	<version>2.3.1</version>
</dependency>

```



3) 创建一个存放jsp的目录，一般叫做webapp

 

---

![image-20220517150219981](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261742794.png)

---



-    index.jsp

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jsp文件</title>
</head>
<body>
    <h3>使用jsp，显示Controller中的数据 ${data}</h3>
</body>
</html>

~~~



4) 需要在pom.xml指定jsp文件编译后的存放目录。 编译后的存放目录 META-INF/resources

~~~xml
<build>
		<!--指定jsp编译后的存放目录-->
		<resources>
			<resource>
				<!--jsp原来的目录-->
				<directory>src/main/webapp</directory>
				<!--指定编译后的存放目录-->
				<targetPath>META-INF/resources</targetPath>
				<!--指定处理的目录和文件-->
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
		</resources>
	</build>

~~~



5）创建Controller， 访问jsp

~~~java
@Controller
public class JspController {

    /*
    public String doJsp(HttpServletRequest request){

        request.setAttribute("data","SpringBoot使用Jsp");
        //视图的逻辑名称
        return "index";
    }
    */


    @RequestMapping("/myjsp")
    public String doJsp(Model model){

        //把数据放入到request作用域
        model.addAttribute("data","SpringBoot使用Jsp");
        //request.setAttribute("data","SpringBoot使用Jsp");

        //视图的逻辑名称
        return "index";
    }
}

~~~



6）在application.propertis文件中**配置视图解析器**

~~~properties
#/ = src/main/webapp
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
~~~



- pom文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.bjpowernode</groupId>
	<artifactId>009-springboot-jsp</artifactId>
	<version>0.0.1-SNAPSHOT</version>


	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>

		<!--处理jsp依赖-->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
        
	</dependencies>

	<build>

		<!--指定jsp编译后的存放目录-->
		<resources>
			<resource>
				<!--jsp原来的目录-->
				<directory>src/main/webapp</directory>
				<!--指定编译后的存放目录-->
				<targetPath>META-INF/resources</targetPath>
				<!--指定处理的目录和文件-->
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
		</resources>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

~~~





## 2.8 使用容器

你想通过代码，从容器中获取对象。

通过SpringApplication.run(Application.class, args); 返回值获取容器。

```java
	//主启动类
	public static void main(String[] args) {
        //查看该静态方法的返回值
		SpringApplication.run(Application.class, args);
	}
	
	//SpringApplication.run(Application.class, args); 方法的返回值是 ConfigurableApplicationContext
	public static ConfigurableApplicationContext run(Class<?> primarySource, String... args) {
        return run(new Class[]{primarySource}, args);
	}

	//ConfigurableApplicationContext接口，是ApplicationContext的子接口也即是说
	//SpringApplication.run(Application.class, args); 方法的返回值是容器
	public interface ConfigurableApplicationContext extends ApplicationContext
```



- 直接获取容器调用业务方法

~~~java
@SpringBootApplication//标注主启动类的注解
public class Application {
	public static void main(String[] args) {

		//获取容器对象
		//ConfigurableApplicationContext ctx  = SpringApplication.run(Application.class, args);
		ApplicationContext ctx  = SpringApplication.run(Application.class, args);

		//从容器者获取对象
		UserService userService = (UserService) ctx.getBean("userService");

		userService.sayHello("李四");
	}
}
~~~

用途：

测试某个方法是某正常；而不用联合tomcat一起运行



## 2.9 CommandLineRunner 接口   ApplicationRunner接口

这两个接口都 有一个run方法。 执行时间在容器对象创建好后， 自动执行run（）方法



给程序员提供一个特殊的时机；容器创建好后



可以完成自定义的在容器对象创建好的一些操作。

- CommandLineRunner

~~~java
@FunctionalInterface
public interface CommandLineRunner {
    void run(String... args) throws Exception;
}
~~~

- ApplicationRunner

```java
@FunctionalInterface
public interface ApplicationRunner {
    void run(ApplicationArguments args) throws Exception;
}
```



不同：

只有run方法的参数不同；其他相同



- 使用

~~~java
@SpringBootApplication
public class Application implements CommandLineRunner {

	@Resource
	private HelloService helloService;

	public static void main(String[] args) {
		System.out.println("准备创建容器对象");
		//创建容器对象
		SpringApplication.run(Application.class, args);
		System.out.println("容器对象创建之后");
	}

	@Override
	public void run(String... args) throws Exception {

		String str  = helloService.sayHello("lisi");
		System.out.println("调用容器中的对象="+str);
		//可做自定义的操作，比如读取文件， 数据库等等
		System.out.println("在容器对象创建好，执行的方法");
	}
}
~~~

~~~java
准备创建容器对象//运行主方法，还没创建容器
 :: Spring Boot ::                (v2.4.2) // 创建容器
调用容器中的对象=你好：lisi					//创建完容器后自动执行CommandLineRunner接口的run方法 且该方法的参数中包含容器中的全部对象
在容器对象创建好，执行的方法
容器对象创建之后
~~~





- 两个依赖的区别

~~~xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
</dependency>
~~~

~~~xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

spring-boot-starter具有core container的核心依赖；不通过接口访问可以使用该注解

但没有springmvc的依赖和内置tomcat的依赖





# 第三章 Web组件

讲三个内容： 拦截器， Servlet ，Filter





## 3.1 拦截器

拦截器是**SpringMVC中一种对象**，能拦截对Controller的请求。

拦截器框架中**有系统的拦截器**， 还可以**自定义拦截器**。

  实现**对请求预先处理**



- Spring中



实现自定义拦截器：

1. 创建类实现SpringMVC框架的HandlerInterceptor接口

   ```java
   public interface HandlerInterceptor {
    
   default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
   }
   
   default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
   }
   
   default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exceptionex) throws Exception {
    }
       
   }
   ```



2.需在SpringMVC的配置文件中，声明拦截器

```xml
<mvc:interceptors>
	<mvc:interceptor>
    	<mvc:path="url" />
        <bean class="拦截器类全限定名称"/>
    </mvc:interceptor>
</mvc:interceptors>
```



SpringBoot中注册拦截器：

- 创建一个配置类代替xml中配置拦截器

```java
@Configuration
public class MyAppConfig implements WebMvcConfigurer {

    //添加拦截器对象， 注入到容器中
    @Override
    public void addInterceptors(InterceptorRegistry registry) {

        //创建拦截器对象
        HandlerInterceptor interceptor = new LoginInterceptor();

        //指定拦截的请求uri地址
        String path []= {"/user/**"};
        //指定不拦截的地址
        String excludePath  [] = {"/user/login"};
        registry.addInterceptor(interceptor)
                .addPathPatterns(path)
                .excludePathPatterns(excludePath);
    }
}
```



- 实现HandleInterceptor接口 自定义拦截器

~~~java
/**
 * 自定义拦截器
 */
public class LoginInterceptor implements HandlerInterceptor  {

    /**
     *
     * @param request
     * @param response
     * @param handler  被拦截器的控制器对象
     * @return boolean
     *    true： 请求能被Controller处理
     *    false: 请求被截断
     */
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler) throws Exception {
        System.out.println("执行了LoginInterceptor的preHandle");
        return true;
    }
}

~~~



- 控制器

~~~java
@Controller
public class BootController {

    @RequestMapping("/user/account")
    @ResponseBody
    public String userAccount(){
        return "访问user/account地址";
    }

    @RequestMapping("/user/login")
    @ResponseBody
    public String userLogin(){
        return "访问user/login地址";
    }
}
~~~





## 3.2  Servlet

在SpringBoot框架中使用Servlet对象。

使用步骤：



- 创建Servlet类。 创建类继承HttpServlet

~~~java
//创建Servlet类
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //使用HttpServletResponse输出数据，应答结果
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out  = resp.getWriter();
        out.println("===执行的是Servlet==");
        out.flush();
        out.close();
    }
}

~~~



- 使用配置类，注册Servlet ，让框架能找到Servlet

~~~java
@Configuration
public class WebApplictionConfig {

    //定义方法， 注册Servlet对象
    @Bean
    public ServletRegistrationBean servletRegistrationBean(){

        //有参构造
        //public ServletRegistrationBean(T servlet, String... urlMappings)
        //第一个参数是 Servlet对象， 第二个是url地址
		//ServletRegistrationBean bean = new ServletRegistrationBean( new MyServlet(),"/myservlet");

        //无参构造
        ServletRegistrationBean bean = new ServletRegistrationBean();
        bean.setServlet( new MyServlet());
        bean.addUrlMappings("/login","/test"); // <url-pattern>
        return bean;
    }
}
~~~







## 3.3 过滤器Filter

Filter是Servlet规范中的过滤器，可以处理请求， 对请求的参数， 属性进行调整。 **常常在过滤器中处理字符编码**

在框架中使用过滤器：

1. 创建自定义过滤器类
2. 注册Filter过滤器对象



- 创建过滤器

```java
// 自定义过滤器
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行了MyFilter，doFilter ");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```



- 注册过滤器到容器中

```java
@Configuration
public class WebApplicationConfig {

    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean bean  = new FilterRegistrationBean();
        bean.setFilter( new MyFilter());
        bean.addUrlPatterns("/user/*");
        return bean;
    }
}
```



## 3.4 字符集过滤器

CharacterEncodingFilter : 解决post请求中乱码的问题

在SpringMVC框架， 在web.xml 注册过滤器。 配置他的属性。 





注意：

在springboot中使用servlet；他放回给服务器默认是ISO-8859-1【response.setContentType="text/html;charset=UTF-8"】

第一种方式：

使用步骤：

1. 配置字符集过滤器

   ```java
   @Configuration
   public class WebSystemConfig {
   
       //注册Servlet
       @Bean
       public ServletRegistrationBean servletRegistrationBean(){
           MyServlet myServlet = new MyServlet();
           ServletRegistrationBean reg = new ServletRegistrationBean(myServlet,"/myservlet");
           return reg;
       }
   
   
       //注册Filter
       @Bean
       public FilterRegistrationBean filterRegistrationBean(){
           FilterRegistrationBean reg = new FilterRegistrationBean();
   
           //使用框架中的过滤器类
           CharacterEncodingFilter filter  = new CharacterEncodingFilter();
           //指定使用的编码方式
           filter.setEncoding("utf-8");
           //指定request ， response都使用encoding的值
           filter.setForceEncoding(true);
   
           reg.setFilter(filter);
           //指定 过滤的url地址
           reg.addUrlPatterns("/*");
   
   
           return reg;
       }
   }
   ```

2. 修改application.properties文件， 让自定义的过滤器起作用

```properties
#SpringBoot中默认已经配置了CharacterEncodingFilter。 编码默认ISO-8859-1
#设置enabled=false 作用是关闭系统中配置好的过滤器， 使用自定义的CharacterEncodingFilter
server.servlet.encoding.enabled=false
```



第二种方式

修改application.properties文件

不用配置过滤器；

```properties
server.port=9001
server.servlet.context-path=/myboot

#让系统的CharacterEncdoingFilter生效 默认就为false
server.servlet.encoding.enabled=true

#指定使用的编码方式
server.servlet.encoding.charset=utf-8

#强制request，response都使用charset属性的值
server.servlet.encoding.force=true
```





# 第四章 ORM 操作 MySQL



使用MyBatis框架操作数据，  在SpringBoot框架集成MyBatis

使用步骤：



- mybatis起步依赖 ： 完成mybatis对象自动配置， 对象放在容器中

~~~xml
        <!--
        	mybatis起步依赖
        	包含mybatis依赖、spring-mybatis依赖
        -->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.1.4</version>
		</dependency>
~~~





- 资源导出  

  pom.xml 指定把src/main/java目录中的xml文件包含到classpath中

~~~xml
		<!--resources插件-->
		<resources>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>
		</resources>
~~~



- 创建实体类Student

~~~java
public class Student {

    private Integer id;
    private String name;
    private Integer age;
    //get set toString
}

~~~



- 创建Dao接口 StudentDao , 创建一个查询学生的方法 

~~~java
/**
 * @Mapper：告诉MyBatis需要创建此接口的代理对象。
 *     位置：在类的上面
 */
@Mapper
public interface StudentDao {

    Student selectById(@Param("stuId") Integer id);
}
~~~



- 创建Dao接口对应的Mapper文件， xml文件， 写sql语句

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.bjpowernode.dao.StudentDao">
    
    <!--定义sql语句-->
    <select id="selectById" resultType="com.bjpowernode.model.Student">
        select id,name,age from student where id=#{stuId}
    </select>

</mapper>
~~~



创建Service层对象， 创建StudentService接口和他的实现类。 去dao对象的方法。完成数据库的操作

~~~java
@Service
public class StudentServiceImpl implements StudentService {

    @Resource
    private StudentDao studentDao;

    @Override
    public Student queryStudent(Integer id) {
        Student student = studentDao.selectById(id);
        return student;
    }
}

~~~



- 创建Controller对象，访问Service

~~~java
@Controller
public class StudentController {

    @Resource
    private StudentService studentService;

    @RequestMapping("/student/query")
    @ResponseBody
    public String queryStudent(Integer id){
        Student student = studentService.queryStudent(id);
        return student.toString();
    }
}

~~~



写application.properties文件  配置数据库的连接信息

~~~properties
server.port=9001
server.servlet.context-path=/orm

spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=root123

~~~



- pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.bjpowernode</groupId>
	<artifactId>017-springboot-mapper</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<!--weby起步依赖 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!--mybatis起步依赖-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.1.4</version>
		</dependency>

		<!--mysql驱动-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>

		<!--测试-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<!--resources插件-->
		<resources>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>
		</resources>


		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

~~~





### 第一种方式 ： @Mapper

@Mapper：放在dao接口的上面， 每个接口都需要使用这个注解。

```java
/**
 * @Mapper：告诉MyBatis这是dao接口，创建此接口的代理对象。
 *     位置：在类的上面
 */
@Mapper
public interface StudentDao {

    Student selectById(@Param("stuId") Integer id);
}
```





### 第二种方式  @MapperScan



```java
/**
 * @MapperScan: 找到Dao接口和Mapper文件
 *     basePackages：Dao接口所在的包名
 */
@SpringBootApplication
@MapperScan(basePackages = {"com.bjpowernode.dao","com.bjpowernode.mapper"})
public class Application {
}
```



### 第三种方式： Mapper文件和Dao接口分开管理

 现在把Mapper文件放在resources目录下

1）在resources目录中创建子目录 （自定义的） ， 例如mapper

2）把mapper文件放到 mapper目录中

3）在application.properties文件中，指定mapper文件的目录

**【包扫描扫的是mapper.java文件】**

```properties
#指定mapper文件的位置
mybatis.mapper-locations=classpath:mapper/*.xml

#指定mybatis的日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

4) 在pom.xml中指定 把resources目录中的文件 ， 编译到目标目录中

```xml
<!--resources插件-->
<resources>
   <resource>
      <directory>src/main/resources</directory>
      <includes>
         <include>**/*.*</include>
      </includes>
   </resource>
</resources>
```



## mybatis代码生成器整合



- pom文件中加插件

~~~xml
  		<!--mybatis代码自动生成插件-->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.6</version>
                <configuration>
                    <!--配置文件的位置-->
                    <configurationFile>GeneratorMapper.xml</configurationFile>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
~~~



- 在根目录下添加配置文件【与src同级】

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!-- 指定连接数据库的JDBC驱动包所在位置，指定到你本机的完整路径 -->
    <classPathEntry location="D:\sofeware\Idea\apache-maven-3.6.1\repository\mysql\mysql-connector-java\8.0.16\mysql-connector-java-8.0.16.jar"/>

    <!-- 配置table表信息内容体，targetRuntime指定采用MyBatis3的版本 -->
    <context id="tables" targetRuntime="MyBatis3">

        <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!-- 配置数据库连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/ssm?serverTimezone=Asia/Shanghai"
                        userId="root"
                        password="root123">
        </jdbcConnection>

        <!-- 生成model类，targetPackage指定model类的包名， targetProject指定生成的model放在eclipse的哪个工程下面-->
        <javaModelGenerator targetPackage="com.fgcy.pojo"
                            targetProject="D:\learn\动力节点\04-SpringBoot\王鹤SpringBoot\代码\reverse_project\src\main\java">
            <property name="enableSubPackages" value="false" />
            <property name="trimStrings" value="false" />
        </javaModelGenerator>

  <!-- 生成MyBatis的Mapper.xml文件，targetPackage指定mapper.xml文件的包名， targetProject指定生成的mapper.xml放在eclipse的哪个工程下面 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

  <!-- 生成MyBatis的Mapper接口类文件,targetPackage指定Mapper接口类的包名， targetProject指定生成的Mapper接口放在eclipse的哪个工程下面 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.fgcy.mapper" targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 数据库表名及对应的Java模型类名 -->
        <table tableName="stu" domainObjectName="Student"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
<!--
        <table tableName="b_bid_info" domainObjectName="BidInfo"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>


        <table tableName="b_income_record" domainObjectName="IncomeRecord"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>


        <table tableName="b_recharge_record" domainObjectName="RechargeRecord"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>


        <table tableName="u_user" domainObjectName="User"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>


        <table tableName="u_finance_account" domainObjectName="FinanceAccount"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
			   -->
    </context>

</generatorConfiguration>
~~~



- 使用maven插件生成

---

![image-20220517213442550](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261742181.png)

---



### 第四个  事务

Spring框架中的事务：

1） 管理事务的对象： 事务管理器（接口， 接口有很多的实现类）

​      例如：使用Jdbc或mybatis访问数据库，使用的事务管理器：DataSourceTransactionManager【mybatis】



2 ) 声明式事务：  在xml配置文件或者使用注解说明事务控制的内容

​     控制事务： 隔离级别，传播行为， 超时时间



3）事务处理方式：

​      1） Spring框架中的@Transactional

​      2)    aspectj框架可以在xml配置文件中，声明事务控制的内容



​    

SpringBoot中使用事务： 上面的两种方式都可以**【一下演示注解的方式】**

1）在业务方法的上面加入@Transactional ,  加入注解后，方法有事务功能了。

2）明确的在 主启动类的上面 ，加入@EnableTransactionManager

 

- 在业务逻辑层添加@Trancesactional注解

```java
/**
 * @Transactional: 表示方法的有事务支持
 *       默认：使用库的隔离级别， REQUIRED 传播行为； 超时时间  -1
 *       抛出运行时异常，回滚事务
 */
@Transactional
@Override
public int addStudent(Student student) {
    System.out.println("业务方法addStudent");
    int rows  =  studentDao.insert(student);
    System.out.println("执行sql语句");

    //抛出一个运行时异常， 目的是回滚事务
    //int m   = 10 / 0 ;

    return rows;
}
```



- 在主启动类中添加开启事务的注解

~~~java
/**
 * @EnableTransactionManagement： 启用事务管理器
 */
@SpringBootApplication
@EnableTransactionManagement
@MapperScan(basePackages = "com.bjpowernode.dao")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
~~~





# 第五章 接口架构风格 —RESTful



接口： API（Application Programming Interface，应用程序接口）是一些预先定义的接口（如函数、HTTP接口），或指[软件系统](https://baike.baidu.com/item/软件系统/224122)不同组成部分衔接的约定。 用来提供[应用程序](https://baike.baidu.com/item/应用程序)与开发人员基于某[软件](https://baike.baidu.com/item/软件)或硬件得以访问的一组[例程](https://baike.baidu.com/item/例程/2390628)，而又无需访问源码，或理解内部[工作机制](https://baike.baidu.com/item/工作机制/9905789)的细节。



接口（API）： 可以指访问servlet， controller的url，   调用其他程序的 函数



**架构风格： api组织方式（样子）**

   就是一个传统的：    http://localhost:9002/mytrans/addStudent?name=lisi&age=26 

​                                      在地址上提供了 访问的资源名称addStudent, 在其后使用了get方式传递参数。

### 5.1  REST

RESTful架构风格

1)REST :  （英文： Representational State Transfer , 中文： 表现层状态转移)。

   REST：是**一种接口的架构风格**和设计的理念，不是标准。

   优点： 更简洁，更有层次



   表现层状态转移: 

​         表现层就是视图层， 显示资源的， 通过视图页面，jsp等等显示操作资源的结果。

​          状态： 资源变化

​         转移： 资源可以变化的。 资源能创建，new状态，  资源创建后可以查询资源， 能看到资源的内容，

这个资源内容 ，可以被修改， 修改后资源 和之前的不一样。  





2）REST中的要素：

   用REST表示资源和对资源的操作。  在互联网中，表示一个资源或者一个操作。 

   资源使用url表示的， 在互联网， 使用的图片，视频， 文本，网页等等都是资源。

   资源是用名词表示。



  对资源： 

​        查询资源： 看，通过url找到资源。 

​        创建资源： 添加资源

​        更新资源：更新资源 ，编辑

​        删除资源： 去除

​       

 资源使用url表示，通过名词表示资源。

​     在url中，使用名词表示资源， 以及访问资源的信息,  在url中，使用“ / " 分隔对资源的信息

​     http://localhost:8080/myboot/student/1001

 使用http中的动作（请求方式）， 表示对资源的操作（CURD）

   GET:  查询资源  --  sql select

​                 处理单个资源： 用他的单数方式

​                  http://localhost:8080/myboot/student/1001

​                 http://localhost:8080/myboot/student/1001/1

​                处理多个资源：使用复数形式

​                  http://localhost:8080/myboot/students/1001/1002

​                

   POST: 创建资源  -- sql insert

​                http://localhost:8080/myboot/student

​                在post请求中传递数据

```html
<form action="http://localhost:8080/myboot/student" method="post">
	姓名：<input type="text" name="name" />
    年龄：<input type="text" name="age" />
  </form>
```




   DELETE: 删除资源  -- sql delete

    <a href="http://localhost:8080/myboot/student/1">删除1的数据</a>




 需要的分页，  排序等参数，依然放在  url的后面， 例如 

 http://localhost:8080/myboot/students?page=1&pageSize=20

   `

  

3） 一句话说明REST： 

​    **使用url表示资源 ，使用http动作操作资源**。



4) 注解

  @PathVariable :  从url中获取数据

  @GetMapping:    支持的get请求方式，  等同于 @RequestMapping( method=RequestMethod.GET)

  @PostMapping:  支持post请求方式 ，等同于 @RequestMapping( method=RequestMethod.POST)

  @PutMapping:  支持put请求方式，  等同于 @RequestMapping( method=RequestMethod.PUT)

   @DeleteMapping: 支持delete请求方式，  等同于 @RequestMapping( method=RequestMethod.DELETE)

  

  @RestController:  复合注解， 是@Controller 和@ResponseBody组合。

​               在类的上面使用@RestController ， 表示当前类者的所有方法都加入了 @ResponseBody



5. Postman : 测试工具

   使用Postman : 可以测试 get ，post ， put ，delete 等请求





### 5.2  在页面中或者ajax中，支持put，delete请求

在SpringMVC中 有一个过滤器， 支持post请求转为put ,delete



过滤器： org.springframework.web.filter.HiddenHttpMethodFilter

作用： 把请求中的post请求转为 put ， delete



实现步骤：

1. application.properties(yml) : 开启使用 HiddenHttpMethodFilter 过滤器
2. 在请求页面中，包含 _method参数， 他的值是 put， delete  ，  发起这个请求使用的post方式

~~~properties
#用 HiddenHttpMethodFilter 这个过滤器， 支持 post 请求转为 put，delete 
spring.mvc.hiddenmethod.filter.enabled=true
~~~



 PUT： 更新资源  --  sql  update

   ```xml
<form action="http://localhost:8080/myboot/student/1" method="post">
	姓名：<input type="text" name="name" />
    年龄：<input type="text" name="age" />
         <input type="hidden" name="_method" value="PUT" />
  </form>
   ```





# 第六章 Redis

Redis ： 一个NoSQL数据库，  常用作 缓存使用 （cache）

Redis的数据类型： string  ,  hash  ,set  ,zset , list



Redis是一个中间件： 是一个独立的服务器。

java中著名的客户端： Jedis ，  lettuce ， Redisson



Spring,SpringBoot中有 一个RedisTemplate（StringRedisTemplate） ，处理和redis交互  



## 6.1  配置Windows版本的redis

Redis-x64-3.2.100.rar 解压缩到一个 **非中文 的目录**

redis-server.exe：**服务端**， 启动后，不要关闭

redis-cli.exe：**客户端**， 访问redis中的数据



redisclient-win32.x86_64.2.0.jar : Redis图形界面客户端

执行方式： 在这个文件所在的目录，  执行 java -jar redisclient-win32.x86_64.2.0.jar





**RedisTemplate 使用的  lettuce 客户端库**

```xml
<!--redis起步依赖： 直接在项目中使用RedisTemplate(StringRedisTemplate)-->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!--
    data-redis使用的-------》lettuce 客户端库-----》redis服务器
    在程序中使用RedisTemplate类的方法 操作redis数据， 实际就是调用的lettuce 客户端的中的方法
-->
```



## springboot整合redis(1)

- 起步依赖

~~~xml
		<!--
			redis起步依赖： 直接在项目中使用RedisTemplate(StringRedisTemplate)
			不需要在配置类中配置
		-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
		</dependency>
~~~



- spring核心配置文件

~~~properties
server.port=9001
server.servlet.context-path=/myredis

spring.redis.host=localhost
spring.redis.port=6379
#spring.redis.password=123

~~~



- 控制器

~~~java
@RestController
public class RedisController {

    /**
     * 注入RedisTemplate
     *
     * RedisTemplate 泛型
     * RedisTemplate<String,String>
     * RedisTemplate<Object,Object>
     * RedisTemplate
     *
     * 注意： RedisTemplate对象的名称 redisTemplate
     */
    @Resource
    private RedisTemplate redisTemplate;


    @Resource
    private StringRedisTemplate stringRedisTemplate;

    //==================================RedisTemplate
    
    
    // 添加数据到redis
    @PostMapping("/redis/addstring")
    public String addToRedis(String name, String value){

        // 操作Redis中string类型的数据， 先获取ValueOperations对象
        ValueOperations valueOperations = redisTemplate.opsForValue();

        //添加数据到redis
        valueOperations.set("myname","lisi");

        return "向redis添加string类型的数据";
    }


    // 从redis获取数据
    @GetMapping("/redis/getk")
    public String getData(String k){
        ValueOperations valueOperations = redisTemplate.opsForValue();
        Object v = valueOperations.get(k);
        return "key是"+k+",他的值是："+v;
    }


    
    //=================================StringRedisTemplate
    
    @PostMapping("/redis/{k}/{v}")
    public String addStringKV(@PathVariable  String k,
                              @PathVariable  String v){

        // 使用StringRedisTemplate对象
        stringRedisTemplate.opsForValue().set(k,v);
        return "使用StringRedisTemplate对象";
    }

    @GetMapping("/redis/getstr/{k}")
    public String getStringValue(@PathVariable  String k){

        String v  = stringRedisTemplate.opsForValue().get(k);
        return "k的value:"+v;
    }

}
~~~



## springboot整合redis（2）

- pom

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.7</version>
        <relativePath/>
    </parent>

    <groupId>com.ice</groupId>
    <artifactId>lab22</artifactId>
    <version>0.0.1-SNAPSHOT</version>


    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--redis的起步依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <!--springboot的测试依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

</project>

~~~



- 实体类

~~~java
import org.springframework.data.annotation.Id;
import org.springframework.data.redis.core.RedisHash;
import org.springframework.data.redis.core.index.Indexed;

/**
 * 班级类
 */
@RedisHash("Class")
public class Class{

    @Id
    private Integer classId;//班级ID
    @Indexed
    private String className;//班名

    private Integer classSize;//班级人数

    private Integer classSurplus;//班级剩余人数

    private Integer deptId;//系别ID

    private String majorId;//专业ID

    private String tutorId;//学习导师ID

    private String instructorId;//辅导员ID

    private String bz;//备注

    //添加 无参构造函数、getter、setter、toString
}

~~~



- 数据访问层接口

~~~java
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;
import java.util.List;

/*
 *
 * @since: 1.8
 * @description：
 * @author: fgcy
 */
@Repository
public interface ClassRepository extends CrudRepository<Class,Integer> {
    List<Class>  findClassByClassName(String className);
}

~~~



- spring核心配置文件

~~~yml
spring:
  redis:
    host: 127.0.0.1
    port: 6379
~~~



- 测试方法

~~~java
import com.ice.lab22.repository.ClassRepository;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.Iterator;
import java.util.List;

@SpringBootTest
class Lab22ApplicationTests {

    @Autowired
    ClassRepository classRepository;

    @Test
    public void save() {
        Class aClass = new Class();
        aClass.setClassId(2);
        aClass.setClassName("19web2");
        aClass.setClassSize(1000);
        Class save = classRepository.save(aClass);
        System.out.println(save);
    }

    @Test
    void findClassByClassName() {
        List<Class> web = classRepository.findClassByClassName("19web2");
        for (Class aClass : web) {
            System.out.println(aClass);
        }
    }


    @Test
    public void update() {
        Class aClass = new Class();
        aClass.setClassId(2);
        aClass.setClassName("19web2");
        aClass.setClassSize(1000);
        aClass.setBz("备注");
        classRepository.save(aClass);
    }

    @Test
    public void testDelete() {
        Class aClass = new Class();
        aClass.setClassId(2);
        aClass.setClassName("19web2");
        aClass.setClassSize(1000);
        classRepository.delete(aClass);
    }

    @Test
    public void testSelectAll() {
        Iterable<Class> all = classRepository.findAll();
        Iterator<Class> iterator = all.iterator();

        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }

}

~~~





## 6.2 对比 StringRedisTemplate 和 RedisTemplate 

StringRedisTemplate ： 把k，v 都是作为String处理， 使用的是String的序列化 ， 可读性好

RedisTemplate ： 把k，v 经过了序列化存到redis。 k，v 是序列化的内容， 不能直接识别.

​                                 默认使用的jdk序列化， 可以修改为前提的序列化



 序列化：把**对象**转化为**可传输的字节序列**过程称为序列化。

反序列化：把字节序列还原为对象的过程称为反序列化。



为什么需要序列化

序列化最终的目的是为了对象可以跨平台存储，和进行网络传输。而我们进行跨平台存储和网络传输的方式就是IO，而我们的IO支持的数据格式就是字节数组。我们必须在把对象转成字节数组的时候就制定一种规则（序列化），那么我们从IO流里面读出数据的时候再以这种规则把对象还原回来（反序列化）。



什么情况下需要序列化

通过上面我想你已经知道了凡是需要进行“跨平台存储”和”网络传输”的数据，都需要进行序列化。

本质上存储和网络传输 都需要经过 把一个对象状态保存成一种跨平台识别的字节格式，然后其他的平台才可以通过字节信息解析还原对象信息。



序列化的方式

序列化只是一种拆装组装对象的规则，那么这种规则肯定也可能有多种多样，比如现在常见的序列化方式有：

JDK（不支持跨语言）、JSON、XML、Hessian               Kryo（不支持跨语言）、Thrift、Protofbuff、



Student( name=zs, age=20)   ----  { "name":"zs", "age":20 }



java的序列化： 把java对象转为byte[], 二进制数据

json序列化：json序列化功能将对象转换为 JSON 格式或从 JSON 格式转换对象。例如把一个Student对象转换为JSON字符串{"name":"李四", "age":29} )，反序列化(将JSON字符串 {"name":"李四", "age":29} 转换为Student对象)



## 设置RedisTemplate的序列化



设置key或者value的序列化方式

```java
// 使用RedisTemplate ，在存取值之前，设置序列化
// 设置 key 使用String的序列化
redisTemplate.setKeySerializer( new StringRedisSerializer());

// 设置 value 的序列化
redisTemplate.setValueSerializer( new StringRedisSerializer());

redisTemplate.opsForValue().set(k,v);
```



- 控制器

~~~java
    /** 设置 RedisTemplate 序列化
     *  可以设置 key 的序列化， 可以设置value的序列化
     *  可以同时设置 key 和 value的序列化
     */
    @PostMapping("/redis/addstr")
    public String  addString(String k,String v){
        // 使用RedisTemplate
        // 设置 key 使用String的序列化
        redisTemplate.setKeySerializer( new StringRedisSerializer());

        // 设置 value 的序列化
        redisTemplate.setValueSerializer( new StringRedisSerializer());

        redisTemplate.opsForValue().set(k,v);
        return "定义RedisTemplate对象的key，value的序列化";

    }


~~~







## IDEA设置序列化版本号

---

![image-20220518095811744](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202208261742623.png)

---



## 设置json序列化

~~~java
    /**
     * 使用json 序列化， 把java对象转为json存储
     */

    @PostMapping("/redis/addjson")
    public String  addJson(){
        Student student  = new Student();
        student.setId(1001);
        student.setName("zhangsan");
        student.setAge(20);

        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // 把值作为json序列化
        redisTemplate.setValueSerializer( new Jackson2JsonRedisSerializer(Student.class) );

        redisTemplate.opsForValue().set("mystudent", student);

        return "json序列化";
    }


    @PostMapping("/redis/getjson")
    public String  getJson(){


        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // 把值作为json序列化
        redisTemplate.setValueSerializer( new Jackson2JsonRedisSerializer(Student.class) );

        Object obj  = redisTemplate.opsForValue().get("mystudent");

        return "json反序列化="+ obj; 
    }
~~~







# 第七章 SpringBoot集成Dubbo

## 7.1  看 SpringBoot集成Dubbo的文档

官网文档：

>  https://github.com/apache/dubbo-spring-boot-project/blob/master/README_CN.md



## 7.2  公共项目

独立的maven项目： 定义了接口和数据类

```java
public class Student implements Serializable {
    private static final long serialVersionUID = 1901229007746699151L;

    private Integer id;
    private String name;
    private Integer age;
}

public interface StudentService {

    Student queryStudent(Integer id);
}

```



## 7.3  提供者

创建SpringBoot项目

1） pom.xml

```xml
<dependencies>

   <!--加入公共项目的gav-->
   <dependency>
      <groupId>com.bjpowernode</groupId>
      <artifactId>022-interface-api</artifactId>
      <version>1.0.0</version>
   </dependency>

   <!--dubbo依赖-->
   <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-spring-boot-starter</artifactId>
      <version>2.7.8</version>
   </dependency>


   <!--zookeeper依赖-->
   <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-dependencies-zookeeper</artifactId>
      <version>2.7.8</version>
      <type>pom</type>
      <exclusions>
          
         <!-- 排除log4j依赖 多次加载log4jjar包出错-->
         <exclusion>
            <artifactId>slf4j-log4j12</artifactId>
            <groupId>org.slf4j</groupId>
         </exclusion>
      </exclusions>
       
   </dependency>
    
</dependencies>
```



2）实现接口  【在公共某块写接口，在pom文件中引入gav】

```java
/**
 * 使用dubbo中的注解暴露服务
 * @Component 可以不用加
 */
@DubboService(interfaceClass = StudentService.class,version = "1.0",timeout = 5000)
public class StudentServiceImpl implements StudentService {
    @Override
    public Student queryStudent(Integer id) {
        Student student  = new Student();
        if( 1001 == id){
            student.setId(1001);
            student.setName("------1001-张三");
            student.setAge(20);
        } else if(1002  == id){
            student.setId(1002);
            student.setName("#######1002-李四");
            student.setAge(22);
        }
        return student;
    }
}
```



3）application.properties

```properties
#配置服务名称 dubbo:application name="名称"
spring.application.name=studentservice-provider

#配置扫描的包， 扫描的@DubboService
dubbo.scan.base-packages=com.bjpowernode.service

#配置dubbo协议
#dubbo.protocol.name=dubbo
#dubbo.protocol.port=20881

#注册中心
dubbo.registry.address=zookeeper://localhost:2181
```



4)在启动类的上面

```java
/**
 * @EnableDubbo 启用Dubbo 复合注解
 * 		@EnableDubboConfig
 * 		@DubboComponentScan
 */
@SpringBootApplication
@EnableDubbo//启用dubbo
public class ProviderApplication {

	public static void main(String[] args) {
		SpringApplication.run(ProviderApplication.class, args);
	}

}
```



## 7.4消费者



创建SpringBoot项目

1） pom.xml

```xml
<dependencies>

   <!--加入公共项目的gav-->
   <dependency>
      <groupId>com.bjpowernode</groupId>
      <artifactId>022-interface-api</artifactId>
      <version>1.0.0</version>
   </dependency>

   <!--dubbo依赖-->
   <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-spring-boot-starter</artifactId>
      <version>2.7.8</version>
   </dependency>


   <!--zookeeper依赖-->
   <dependency>
      <groupId>org.apache.dubbo</groupId>
      <artifactId>dubbo-dependencies-zookeeper</artifactId>
      <version>2.7.8</version>
      <type>pom</type>
      <exclusions>
         <!-- 排除log4j依赖 -->
         <exclusion>
            <artifactId>slf4j-log4j12</artifactId>
            <groupId>org.slf4j</groupId>
         </exclusion>
      </exclusions>
   </dependency>
</dependencies>
```



2)  创建了Controller 或者 Service都可以

```java
@RestController
public class DubboController {

    /**
     * 引用远程服务， 把创建好的代理对象，注入给studentService
     */
    //@DubboReference(interfaceClass = StudentService.class,version = "1.0")

    /**
     * 没有使用interfaceClass，默认的就是 引用类型的 数据类型
      */
    @DubboReference(version = "1.0")//引用远程服务
    private StudentService studentService;

    @GetMapping("/query")
    public String queryStudent(Integer id){
        Student student   = studentService.queryStudent(id);
        return "调用远程接口，获取对象："+student;
    }
}
```



3）application.properties

```properties
#指定服务名称
spring.application.name=consumer-application
#指定注册中心
dubbo.registry.address=zookeeper://localhost:2181
```



注意：

==先启动zookeeper 再启动服务提供者 最后启动服务消费者==





## 7.5 练习

使用的技术： SpringBoot ,Dubbo, Redis, MyBatis



Student表：

~~~mysql
CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8_bin DEFAULT NULL,
  `phone` varchar(11) COLLATE utf8_bin DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
~~~

1) 注册学生

​     phone必须唯一， 如果已经存在了手机号， 注册失败的。 

​                 int addStudent(Student student);

​                返回值：int

​                 1： 注册成功

​                 2 ： 手机号已经存在  

​                                    

​      name至少两个字符，

​      age 必须 大于 0 



2） 查询学生，根据id查询，此学生。 

​     先到redis查询学生， 如果redis没有此学生，从数据库查询， 把查询到的学生放入到redis。

​      后面再次查询这个学生应该从redis就能获取到。

​        Student  queryStudent(Integer id);



3) 使用Dubbo框架，    addStudent, queryStudent 是有服务提供者实现的。

​    消费者可以是一个Controller ， 调用提供者的两个方法。  实现注册和查询。



4）页面使用html和ajax，jquery。

​       在html页面中提供 form 注册学生， 提供文本框输入id，进行查询。

​      注册和查询都使用ajax技术。



​    html，jquery.js都放到resources/static目录中

 





# 第八章  打包

## 8.1 打包war

- 控制器

~~~java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class JspController {

    @RequestMapping("/main")
    public String main(Model model){
        model.addAttribute("data","SpringBoot打包为war文件");
        return "index";
    }
}
~~~



2.修改pom.xml

 1)指定打包后的文件名称

```xml
<build>
   <!--打包后的文件名称-->
   <finalName>myboot</finalName>
</build>
```



2)指定jsp编译目录

```xml
<!--resources插件， 把jsp编译到指定的目录-->
<resources>
   <resource>
      <directory>src/main/webapp</directory>
      <targetPath>META-INF/resources</targetPath>
      <includes>
         <include>**/*.*</include>
      </includes>
   </resource>

   <!--使用了mybatis ，而且mapper文件放在src/main/java目录-->
   <resource>
      <directory>src/main/java</directory>
      <includes>
         <include>**/*.xml</include>
      </includes>
   </resource>

   <!--把src/main/resources下面的所有文件，都包含到classes目录-->
   <resource>
      <directory>src/main/resources</directory>
      <includes>
         <include>**/*.*</include>
      </includes>
   </resource>
    
</resources>
```



3）执行打包是war

```xml
<!--打包类型-->
<packaging>war</packaging>
```



- pom文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
    
	<groupId>com.bjpowernode</groupId>
	<artifactId>025-springboot-war</artifactId>
	<version>1.0.0</version>
	<!--打包类型-->
	<packaging>war</packaging>


	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>

		<!--加入处理jsp的依赖-->
        <!--
			使用的是独立的tomcat不用加此依赖
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
		-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<version>2.3.7.RELEASE</version>
		</dependency>

	</dependencies>

	<build>
		<!--打包后的文件名称-->
		<finalName>myboot</finalName>

		<!--resources插件， 把jsp编译到指定的目录-->
		<resources>
			<resource>
				<directory>src/main/webapp</directory>
				<targetPath>META-INF/resources</targetPath>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>

			<!--使用了mybatis ，而且mapper文件放在src/main/java目录-->
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>

			<!--把src/main/resources下面的所有文件，都包含到classes目录-->
			<resource>
				<directory>src/main/resources</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
		</resources>

		<plugins>
            <!--当运行maven打包的命令，项目会被打包成一个可以直接运行的jar包，使用"java -jar"可以直接运行-->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

~~~





4）主启动类继承SpringBootServletInitializer

```java
/**
 * SpringBootServletInitializer: 继承这个类， 才能使用独立tomcat服务器
 */
@SpringBootApplication
public class JspApplication  extends SpringBootServletInitializer  {

   public static void main(String[] args) {
      SpringApplication.run(JspApplication.class, args);
   }

   @Override
   protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
      return builder.sources(JspApplication.class);
   }
}
```



5）部署war

 把war放到tomcat等服务器的发布目录中。  tomcat为例， myboot.war放到tomcat/webapps目录。

使用的是tomcat8.5.76





注意：

此时application中配置的关于内嵌tomcat的配置其全部失效

~~~properties
server.port=9001   						//8080
server.servlet.context-path=/myjsp     // /myboot

spring.mvc.view.prefix=/              //从webapp下找xxx.jsp文件
spring.mvc.view.suffix=.jsp

~~~





## 8.2 打包为jar

- 控制器

~~~java
package com.bjpowernode.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HelloController {

    @RequestMapping("/hello")
    public ModelAndView hello(ModelAndView modelAndView) {

        modelAndView.addObject("data", "SpringBoot打包为jar");
        modelAndView.setViewName("main");
        return modelAndView;
    }
}

~~~



2.修改pom.xml

​     1) 指定打包后的文件名称

```xml
<build>
   <!--打包后的文件名称-->
   <finalName>myboot</finalName>
</build>
```



    2) 指定springboot-maven-plugin版本

```xml
<plugins>
   <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
      <!--打包jar， 有jsp文件时，必须指定maven-plugin插件的版本是 1.4.2.RELEASE-->
      <version>1.4.2.RELEASE</version>
   </plugin>
</plugins>
```





- pom文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.bjpowernode</groupId>
	<artifactId>026-springboot-jar</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>

		<!--tomcat依赖，处理jsp-->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<!--打包后的文件名称-->
		<finalName>myboot</finalName>
		<!--加入resources插件 -->
		<!--指定编译jsp到META-INF/resources-->
		<resources>
			<resource>
				<directory>src/main/webapp</directory>
				<targetPath>META-INF/resources</targetPath>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>

			<!--如果使用mybatis，同时把xml文件放在了src/main/java目录-->
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>


			<!--把src/main/resources中的所有文件编译到classpath目录中-->
			<resource>
				<directory>src/main/resources</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
		</resources>

		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<!--打包jar， 有jsp文件时，必须指定maven-plugin插件的版本是 1.4.2.RELEASE-->
				<version>1.4.2.RELEASE</version>
			</plugin>
		</plugins>
	</build>

</project>

~~~



- application

~~~properties
#\u7AEF\u53E3
server.port=9002
server.servlet.context-path=/myboot

spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
~~~





3）最后执行 maven clean package

​       在target目录中，生成jar 文件， 例子是myboot.jar



执行独立的springboot项目  在cmd中 java  -jar  myboot.jar 【使用的是内嵌的tomcat，配置文件生效】



- 在linux上运行：



在任意位置拆功能键一个文件：

~~~shell
#!/bin/sh
java -jar myboot.jar
# 说明该文件与myboot.jar在同级目录
~~~



赋权

~~~shell
chmod 777 run.sh
~~~



执行文件

~~~shell
./run.sh
~~~





# 第九章 Thymeleaf 模板引擎



Thymeleaf： 是使用java开发的模板技术， 在服务器端运行。 把处理后的数据发送给浏览器。

​        				模板是作视图层工作的。  显示数据的。  Thymeleaf是基于Html语言。 

​						Thymleaf语法是应用在html标签中 。 **SpringBoot框架集成Thymealeaf,**  **使用Thymeleaf代替jsp**。



Thymeleaf 的官方网站：http://www.thymeleaf.org

Thymeleaf 官方手册：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html



## 初使用



- pom文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.bjpowernode</groupId>
	<artifactId>027-thymeleaf-first</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>027-thymeleaf-first</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<!--模板引擎起步依赖-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

~~~





- 控制器

~~~java
package com.bjpowernode.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import javax.servlet.http.HttpServletRequest;

@Controller
public class HelloThymeleafController {

    @GetMapping("/hello")
    public String helloThymeleaf(Model model,HttpServletRequest request){
        //添加数据到request作用域， 模板引擎可以从request中获取数据
        request.setAttribute("data","欢迎使用Thymeleaf模板引擎");

        //使用model存放对象
        model.addAttribute("mydata","model中的数据");

        //指定视图（模板引用使用的页面（html））
        //逻辑名称  classpath:/templates/hello.html
        return "hello";
    }
}

~~~



- application

~~~properties
#默认开启缓存，开发阶段建议关闭
spring.thymeleaf.cache=false

#默认使用utf-8
spring.thymeleaf.encoding=utf-8

#默认模板是html文件
spring.thymeleaf.mode=HTML

#默认模板的前缀 classpath:/templates
spring.thymeleaf.prefix=classpath:/templates/

#默认模板的后缀
spring.thymeleaf.suffix=.html
~~~



- hello.html

~~~html
<!DOCTYPE html>

<!--前缀引入：xmlns:th="http://www.thymeleaf.org"-->

<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>hello.html</title>
</head>
<body>
    <h3>使用Thymeleaf的例子</h3>
    <p th:text="${data}">想显示数据</p>
    <p th:text="${mydata}">数据</p>
</body>
</html>
~~~



# 语法

 

## 9.1 表达式

1. 标准变量表达式 

   语法：  ${key} 

   作用： 获取key对于的文本数据，  key 是**request作用域**中的key ， 使用request.setAttribute(), model.addAttribute()

   在页面中的 html标签中， 使用 th:text="${key}" 

   功能和 EL 中的 ${} 相同

​		th:text={$xxx}  这个可以当成是标签中的属性；

​		如没有经过服务器解析；而直接有浏览器解析，那么**会显示标签中的值**；而不是\${xxx}的值

​		当${xxx}中的值不存在时，会像el表达式一样不输出【标签中的内容也不输出】

​	注意：th:text="" 是 Thymeleaf 的一个属性，用于文本的显示

```html
<div style="margin-left: 400px">
    <h3>标准变量表达式:  ${key}</h3>
    <p th:text="${site}">key不存在</p>
    <br/>
    <p>获取SysUser对象 属性值</p>
    <!--本质上调用的还是myuser.getId()-->
    <p th:text="${myuser.id}">id</p>
    
    <p th:text="${myuser.name}">姓名</p>
    <p th:text="${myuser.sex}">姓名：m男</p>
    <p th:text="${myuser.age}">年龄</p>
    <p th:text="${myuser.getName()}">获取姓名使用getXXX</p>
</div> 
```

模版文件（html）修改后，可以使用 idea—Build 菜单-Recompile 编译文件。重新 Recompile即可生效

2. 选择变量表达式（ 星号变量表达式）

   语法：  *{key}

   作用： 获取这个key对应的数据，   ***{key}**	需要和	**th:object** 	这个属性一起使用。

   目的是**简化获取对象的属性值**  	

   选择表达式首先使用 th:object 来绑定后台传来的对象，然后使用 * 来代表这个对象，后面 {} 中的值是此对象中的属性

   选择变量表达式在执行时是在选择的对象上求解，而${...}是在上下文的变量 model 上求解 

   ```html
   <p>使用 *{} 获取SysUser的属性值</p>
   <div th:object="${myuser}">
       <p th:text="*{id}"></p>
       <p th:text="*{name}"></p>
       <p th:text="*{sex}"></p>
       <p th:text="*{age}"></p>
   
   </div>
   
   <p>使用*{}完成的表示 对象的属性值</p>
   <p th:text="*{myuser.name}" ></p>
   ```

3. 链接表达式

   ==注意：==

   在thymeleaf中，@{/}    “/”代表上下文路径

   在html页面中 “/” 就真的只是代表“/”；加了这个“/” 上下文路径就会被干掉

   

   语法： @{url}

   作用： 表示链接， 可以

   ```html
   <script src="..."> ,
           
   <link href="...">,
       
   <a href=".."> ,
       
   <form action="...">
       
   <img src="...">
   ```



- html【有themeleaf模板】

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h3>index.html---学习Thymeleaf语法</h3>
    <!--html路径不以/开头 否则不给你带上下文路径-->
    <a href="tpl/link">链接表达式</a><br/>
</body>
</html>
~~~



~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>链接表达式</title>
</head>
<body>

    <h3>链接绝对路径</h3>
    <a th:href="@{http://www.baidu.com}">链接到百度</a>

    <h3>链接的是相对地址</h3>
    <a th:href="@{/tpl/queryAccount}">相对地址，没有参数</a>
    <h3>链接的相对地址，使用字符串链接传递参数</h3>
    <a th:href="@{'/tpl/queryAccount?id='+ ${userId} }">获取model中数据</a>
    <h3>推荐使用的传参数的方式</h3>
    <a th:href="@{/tpl/queryAccount(id=${userId})}">传参数</a>
    <h3>传递多个参数</h3>
    <a th:href="@{/tpl/queryUser(name='lisi',age=20)}">传多个参数</a>

</body>
</html>
~~~

\<a th:href="@{/admin/tags(page=\${page.pageNum}-1)}" class="  item" th:unless="${page.isFirstPage}">上一页\</a>-->



@{/admin/tags/{id}(id=*{id})}

- 控制器

~~~java
  	@Controller
	@RequestMapping("/tpl")
	public class ThymeleafController {
        
	//链接表达式
    @GetMapping("/link")
    public String link(Model model){
        model.addAttribute("userId",1002);
        return "link";
    }


    //测试链接表达式的地址
    @GetMapping("/queryAccount")
    @ResponseBody
    public String queryAccount(Integer id){
        return "queryAccount,参数id="+id;
    }


    //有两个参数的地址
    @GetMapping("/queryUser")
    @ResponseBody
    public String queryUser(String name, Integer age){
        return "queryUser,有两个参数：name="+name+",age="+age;
    }
}
~~~



 

## 9.2  Thymeleaf属性

属性是放在html元素中的，就是html元素的属性，加入了th前缀。  属性的作用不变。   

 加入上th， 属性的值由模板引擎处理了。  在属性可以使用变量表达式



例如：

```xml
<form action="/loginServlet" method="post"></form>

<form th:action="/loginServlet" th:method="${methodAttr}"></form>
```





- 控制器

~~~java
    //使用模板的属性
    @GetMapping("/property")
    public String useProperty(Model model){
        model.addAttribute("methodAttr","post");
        model.addAttribute("id","2342");

        model.addAttribute("paramname","name");
        model.addAttribute("uservalue","lisi");

        model.addAttribute("textcolor","color:blue");

        return "html-property";
    }
~~~





- html-properties

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>属性</title>
    <script th:src="@{/js/jquery-3.4.1.js}" type="text/javascript"></script>
</head>
<body>
    <div style="margin-left: 400px">
        <h3>属性使用， 在html元素的属性上加入th</h3>
        <form th:action="@{/loginServlet}" th:method="${methodAttr}">
            <input th:type="text" th:name="${paramname}" th:value="${uservalue}"> <br/>
            <input type="button" id="btn" th:onclick="btnClick()" value="按钮">
        </form>
        
		<!--动态样式-->
        <p th:style="${textcolor}">这是模板的例子</p>
        
    </div>
</body>
<script type="text/javascript">
    function btnClick(){
        alert("按钮单击了");
    }


    $(function(){
        $("#btn").click(function(){
            alert("click===jquery")
        })
    })
</script>
</html>
~~~





## 9.3 each

each循环， 可以循环List，Array

语法：

在一个html标签中，使用th:each

```xml
<div th:each="集合循环成员,循环的状态变量:${key}">
    <p th:text="${集合循环成员}" ></p>
</div>

集合循环成员,循环的状态变量:两个名称都是自定义的。 “循环的状态变量”这个名称可以不定义，默认是"集合循环成员Stat"
```





each循环Map

在一个html标签中，使用th:each

```html
<div th:each="集合循环成员,循环的状态变量:${key}">
    <p th:text="${集合循环成员.key}" ></p>
    <p th:text="${集合循环成员.value}" ></p>
</div>

集合循环成员,循环的状态变量:两个名称都是自定义的。 “循环的状态变量”这个名称可以不定义，默认是"集合循环成员Stat"

th:each="user, iterStat : ${userlist}"中的 ${userList} 是后台传过来的集合
    interStat 是循环体的信息，通过该变量可以获取如下信息
    index: 当前迭代对象的 index（从 0 开始计算）
    count: 当前迭代对象的个数（从 1 开始计算）这两个用的较多
    size: 被迭代对象的大小
    current: 当前迭代变量  ${iterStat.current.getXxx}
    even/odd: 布尔值，当前循环是否是偶数/奇数（从 0 开始计算）
    first: 布尔值，当前循环是否是第一个
    last: 布尔值，当前循环是否是最后一个
    注意：循环体信息 interStat 也可以不定义，则默认采用迭代变量加上 Stat 后缀，即
    userStat




key:map集合中的key
value：map集合key对应的value值

```





**循环List**



- 控制器

~~~java
    //循环List
    @GetMapping("/eachList")
    public String eachList(Model model){
        List<SysUser> users  = new ArrayList<>();
        users.add( new SysUser(1001,"张三","m",20));
        users.add( new SysUser(1002,"关羽","m",70));
        users.add( new SysUser(1003,"张飞","m",60));
        users.add( new SysUser(1004,"刘备","m",80));
        users.add( new SysUser(1005,"孙尚香","f",50));
        model.addAttribute("myusers",users);
        return "eachList";
    }
~~~



- eachList.html

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>each循环</title>
</head>
<body>

   <div style="margin-left: 400px">
       <!--
		<div th:each="user,userStat:${myusers}">
            <p th:text="${user.id}"></p>
            <p th:text="${user.name}"></p>
            <p th:text="${user.sex}"></p>
            <p th:text="${user.age}"></p>
       </div>
		-->
       <br/>
       <br/>

       <table border="1" cellpadding="0" cellspacing="0">
           <thead>
             <tr>
                 <td> 编号</td>
                 <td> id 序号 </td>
                 <td> name </td>
                 <td> sex </td>
                 <td> age </td>
                 <td>姓名</td>
                 <td>是否是第一行</td>
             </tr>
           </thead>
           <tbody>
               <!--th:each="user, iterStat : ${userlist}"中的 ${userList} 是后台传过来的集合-->
             <tr th:each="user:${myusers}">
                 <!--注意：在thymeleaf中，如需再“”中使用字符 需要：   +‘xxxx’+    -->
                 <td th:text="${userStat.count}+'/'+${userStat.size}"></td>
                 <td th:text="${user.id}"></td>
                 <td th:text="${user.name}"></td>
                 <td th:text="${user.sex}"></td>
                 <td th:text="${user.age}"></td>
                 <td th:text="${userStat.current.name}"></td>
                 <td th:text="${userStat.first}"/>
             </tr>
           </tbody>
       </table>
   </div>

</body>
</html>
~~~





**循环数组**



- 控制器

~~~java
    //循环数组
    @GetMapping("/eachArray")
    public String eachArray(Model model){
        SysUser userarray[] = new SysUser[3];
        userarray[0] = new SysUser(1001,"马超","男",20);
        userarray[1] = new SysUser(1002,"黄忠","男",26);
        userarray[2] = new SysUser(1003,"赵云","男",22);
        model.addAttribute("userarray",userarray);

        return "eachArray";
    }
~~~



- eachArray

~~~html
 <!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>each循环</title>
</head>
<body>

   <div style="margin-left: 400px">
       <!--数组里面封对象-->
      <div th:each="user:${userarray}">
          <p th:text="${user.id}"></p>
          <p th:text="${user.name}"></p>
          <p th:text="${user.sex}"></p>
          <p th:text="${user.age}"></p>
          <br/>
          <hr/>
      </div>

   </div>

</body>
</html>
~~~





**循环Map**



- controller

~~~java
    //循环Map
    @GetMapping("/eachMap")
    public String eachMap(Model model){
        //字符串，对象
        Map<String,SysUser> map = new HashMap<>();
        map.put("user1",new SysUser(11001,"马超","男",20));
        map.put("user2",new SysUser(11002,"黄忠","男",26));
        map.put("user3",new SysUser(11003,"赵云","男",22));
        model.addAttribute("mymap",map);

		//字符串，字符串
        Map<String,String>  strMap  = new HashMap<>();
        strMap.put("str1","Hello SpringBoot");
        strMap.put("str2","MyBatis MySQL");
        model.addAttribute("strmap",strMap);

      
    }
~~~



- eachMap

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>循环Map</title>
</head>
<body>
    <div style="margin-left: 400px">
        <!--循环 Map 字符串，对象-->
        <div th:each="map,userStat:${mymap}">
            <p th:text="${map.key}"></p>
            
            <!--没有给toString 给的是地址值-->
            <p th:text="${map.value}" ></p>
            
            <p th:text="${map.value.id}"></p>
            <p th:text="${map.value.name}"></p>
            <p th:text="${userStat.index}"></p>
            <br/>
            <hr/>
        </div>


        <br/>
        <br/>
        <!--循环Map字符串，字符串-->
        <div th:each="strmap:${strmap}">
            
            <p th:text="${strmap.key}"></p>
            <p th:text="${strmap.value}"></p>
            
        </div>

    </div>
</body>
</html>
~~~





**循环List\<Map<String,Suser>>**



- 控制器

~~~java
  	@GetMapping("/eachMap")
    public String eachMap(Model model){
		//复杂的List<Map<String,Suser>>
        List<Map<String,SysUser>> listmap  =  new ArrayList<>();
        listmap.add(map);

        Map<String,SysUser> map1 = new HashMap<>();
        map1.put("sanguo1",new SysUser(21001,"典韦","男",22));
        map1.put("sanguo2",new SysUser(21002,"夏侯惇","男",28));
        map1.put("sanguo3",new SysUser(21003,"关胜","男",16));
        listmap.add(map1);
        model.addAttribute("listmap",listmap);
        return "eachMap";
    }
~~~



- eachListMap

~~~html
        <h3>循环List《Map》  Map<String,SysUser> lm: listmap.get(0)</h3>
        <div th:each="lm:${listmap}">
            
            <!--循环map中的所有key，value-->
            <!--先遍历list中的map-->
            <div th:each="m:${lm}">
                <p th:text="${m.key}"></p>
                <p th:text="${m.value.id}"></p>
                <p th:text="${m.value.name}"></p>
                <p th:text="${m.value.age}"></p>
                <br/>
                <hr/>
            </div>
            <br>
            <hr>
        </div>
~~~







## 9.4 th:if

"th:if"  : 判断语句， 当条件为true， 显示html标签体内， 反之不显示

​			  没有else语句



==注意：==

空字符串在thymeleaf中代表真



```xml
语法：
<div th:if=" 10 > 0 "> 显示文本内容 </div>
```



还有一个 th:unless  和 th:if相反的行为

```xml
语法：
<div th:unless=" 10 < 0 "> 当条件为false显示标签体内容 </div>
```





- 控制器

~~~java
    // ifunless
    @GetMapping("/ifunless")
    public String ifunless(Model model){
        model.addAttribute("sex","m");
        model.addAttribute("isLogin",true);
        model.addAttribute("age",20);
        model.addAttribute("name","");
        model.addAttribute("isOld",null);
        return "ifunless";

    }
~~~





例子：if   “”中为真显示数据据

```xml
<div style="margin-left: 400px">
        <h3> if 使用</h3>
        <p th:if="${sex=='m'}">性别是男</p>
        <p th:if="${isLogin}">已经登录系统</p>
        <p th:if="${age > 20}">年龄大于20</p>
        <!--""空字符是true-->
        <p th:if="${name}">name是“”</p>
        <!--null是false-->
        <p th:if="${isOld}"> isOld是null</p>
 </div>

```



例子： unless    “”中为假显示数据据

```html
 <div style="margin-left: 400px">
        <h3>unless: 判断条件为false，显示标签体内容</h3>
        <p th:unless="${sex=='f'}">性别是男的</p>
        <p th:unless="${isLogin}">登录系统</p>
        <p th:unless="${isOld}"> isOld是null </p>
 </div>
```









## 9.5  th:switch

th:switch 和 java中的swith一样的



```html
语法：
<div th:switch="要比对的值">
    <p th:case="值1">
        结果1
    </p>
    <p th:case="值2">
        结果2
    </p>
    <p th:case="*">
        默认结果
    </p>
    以上的case只有一个语句执行
    
</div>
```



- switch

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>switch</title>
</head>
<body>
    <div style="margin-left: 400px">
        <h3> switch 使用</h3>
            <!--值匹配-->
        <div th:switch="${sex}">
            <p th:case="f">性别是女</p>
            <p th:case="m">性别是男</p>
            <p th:case="*">未知</p>
        </div>
    </div>


</body>
</html>
~~~



- 控制器

~~~java
    //switch
    @GetMapping("/switch")
    public String doSwitch(Model model){
        model.addAttribute("sex","m");
        return "switch";

    }
~~~







## 9.6 th:inline

1. 内联text：  在html标签外，获取表达式的值



- 相比于表达式${} 好处：

  - 可以不用依赖标签    \<p th:text="${xxx}"/>

  - 可以嵌套文字写

  - ~~~
    <p>我去[[${xxx}]]你他嘛的[[${aaaaa]]</p>
    >>我去辣鸡你他嘛的傻狗
    
    
    我去<span th:text="${xxx}"></span>你他嘛的<span th:text="${aaa}"></span>
    >>我去辣鸡你他嘛的傻狗
    ~~~



- 控制器

~~~java
    // 内联 text
    @GetMapping("/inline")
    public String inline(Model model){
        model.addAttribute("sex","m");
        model.addAttribute("age",20);
        model.addAttribute("name","张三");
        model.addAttribute("myuser" ,new SysUser(1005,"周峰","男",20));

        return "inline";

    }
~~~

- inline

```xml
<p>显示姓名是：[[${key}]]</p>

 <div style="margin-left: 400px">
        <h3>内联 text, 使用内联表达式显示变量的值</h3>
     
    	 <!--th:inline="text" 这个可写可不写-->
        <div th:inline="text">
            <p>我是[[${name}]]，年龄是[[${age}]]</p>
            我是<span th:text="${name}"></span>,年龄是<span th:text="${age}"></span>
        </div>

        <div>
            <p>使用内联text</p>
            <p>我是[[${name}]],性别是[[${sex}]]</p>
        </div>
     
</div>
```





## 内联javascript【在js中使用，动态数据】

```html
例子：
 <script type="text/javascript" th:inline="javascript">
         var myname = [[${name}]];
         var myage = [[${age}]];

         //alert("获取的模板中数据 "+ myname + ","+myage)

        function fun(){
            alert("单击事件，获取数据 "+ myname + ","+ [[${sex}]])
        }
    </script>
```





## 9.7  字面量



文本字面量: **使用单引号括起来的字符串**

注意：**字面量与${}连用   要用+连接**



例子：

```html
 <div style="margin-left: 400px">
       <h3>文本字面量: 使用单引号括起来的字符串</h3>
       <p th:text="'我是'+${name}+',我所在的城市'+${city}">数据显示</p>

       <h3>数字字面量</h3>
        <p th:if="${20>5}"> 20大于 5</p>

        <h3>boolean字面量</h3>
        <p th:if="${isLogin == true}">用户已经登录系统</p>

        <h3>null字面量</h3>
        <p th:if="${myuser != null}">有myuser数据</p>
    </div>
```



## 9.8  字符串连接



**连接字符串有两种语法**



1） 语法使用 单引号括起来字符串  ， 使用 + 连接其他的 字符串或者表达式

```html
  <p th:text="'我是'+${name}+',我所在的城市'+${city}">数据显示</p>
```



2）语法：使用双竖线， |字符串和表达式|

```html
<p th:text="|我是${name},我所在城市${city|">
    显示数据
</p>
```



- 控制器

~~~java
    //字符串连接
    @GetMapping("/strjoin")
    public String strJoin(Model model){
        model.addAttribute("sex","m");
        model.addAttribute("age",20);
        model.addAttribute("name","李思");
        model.addAttribute("city","上海");
        model.addAttribute("isLogin",true);
        model.addAttribute("myuser" ,new SysUser(1005,"周峰","男",20));
        return "strjoin";

    }
~~~



例子：

```html
    <div style="margin-left: 400px">
       <h3>字符串连接方式1：使用单引号括起来的字符串</h3>
       <p th:text="'我是'+${name}+',我所在的城市'+${city}">数据显示</p>
        <br/>
        <br/>
        <h3>字符串连接方式2：|字符串和表达式|</h3>
        <p th:text="|我是${name},所在城市${city},其他人${myuser.name}|"></p>
    </div>
```





## 9.9 运算符



```xml
算术运算： + , - - , * , / , %
关系比较 : > , < , >= , <= ( gt , lt , ge , le )
相等判断： == , != ( eq , ne )

三元运算符：
 表达式  ？ true的结果 : false的结果

三元运算符可以嵌套
```



- 控制器

~~~java
    //运算符使用
    @GetMapping("/sym")
    public String sym(Model model){
        model.addAttribute("sex","m");
        model.addAttribute("age",20);
        model.addAttribute("name","李思");
        model.addAttribute("city","上海");
        model.addAttribute("isLogin",true);
        model.addAttribute("myuser" , null);
        return "sym";

    }
~~~



- sym

~~~html
<div style="margin-left: 400px">
        <h3>使用运算符</h3>
        <p th:text="${age > 10}">年龄大于 10 </p>
        <p th:text="${ 20 + 30 }">显示运算结果</p>
        <p th:if="${myuser == null}">myuser是null</p>
        <p th:if="${myuser eq null}">myuser是null</p>
        <p th:if="${myuser ne null}">myuser不是null</p>

        <p th:text="${isLogin == true ? '用户已经登录' : '用户需要登录'}"></p>
        <p th:text="${isLogin == true ? ( age > 10 ? '用户是大于10的' : '用户年龄比较小') : '用户需要登录'}"></p>

    </div>
~~~





## 9.10 内置对象



文档地址：

> https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#web-context-namespaces-for-requestsession-attributes-etc.



#request 表示 HttpServletRequest

#session 表示 HttpSession对象

session 表示Map对象的， 是#session的简单表示方式， 用来获取session中指定的key的值

​               #session.getAttribute("loginname") == session.loginname

这些是内置对象，可以在模板文件中直接使用



- 控制器

~~~java
    //模板内置对象
    @GetMapping("/baseObject")
    public String baseObject(Model model, HttpServletRequest request,
                             HttpSession session){
        //添加数据
        model.addAttribute("myname","李思");
        request.setAttribute("requestData","request作用域中的数据");
        request.getSession().setAttribute("sessionData","session作用域中的数据");
        //直接使用session
        session.setAttribute("loginname","zhangsan");
        return "baseObject";
    }
~~~



- baseObject

#request,#session，session 

```html
 <div style="margin-left: 350px">
        <h3>内置对象#request,#session，session的使用</h3>
        <p>获取作用域中的数据</p>
        <p th:text="${#request.getAttribute('requestData')}"></p>
        <p th:text="${#session.getAttribute('sessionData')}"></p>
        <p th:text="${session.loginname}"></p>

        <br/>
        <br/>
        <h3>使用内置对象的方法</h3>
        getRequestURL=<span th:text="${#request.getRequestURL()}"></span><br/>
        getRequestURI=<span th:text="${#request.getRequestURI()}"></span><br/>
        getQueryString=<span th:text="${#request.getQueryString()}"></span><br/>
        getContextPath=<span th:text="${#request.getContextPath()}"></span><br/>
        getServerName=<span th:text="${#request.getServerName()}"></span><br/>
        getServerPort=<span th:text="${#request.getServerPort()}"></span><br/>
</div>
```



param对象：封装请求参数的对象

直接获取请求的参数

~~~html
        <h3>param对象：表示请求的参数集合</h3>
        name参数的值：<span th:text="${param.name}"></span><br/>
        参数的数量：<span th:text="${param.size()}"></span><br/>
~~~



#servletContext对象

~~~html
<span th:text="${#servletContext.getAttribute('xxx')}"></span>


<span th:text="${#servletContext.contextPath}"></span>
~~~



##  9.11 内置工具类

内置工具类型： Thymeleaf自己的一些类，提供对string， date ，集合的一些处理方法

#dates: 处理日器的工具类

#numbers:处理数字的

#lists: 处理list集合的





- 控制器

~~~java
   //内置的工具类对象
    @GetMapping("/utilobject")
    public String utilObject(Model model){
        model.addAttribute("mydate",new Date());
        model.addAttribute("mynum",26.695);
        model.addAttribute("mystr","bjpowernode");

        List<String> list = Arrays.asList("a","b","c");
        model.addAttribute("mylist",list);


        //创建Zoo对象
        Zoo zoo = new Zoo();
        Cat cat = new Cat();
        cat.setName("短腿");

        Dog dog = new Dog();
        dog.setName("二哈");

        zoo.setCat(cat);
        //zoo.setDog(dog); zoo.dog = null

        model.addAttribute("zoo",zoo);

        return "utilObject";
    }

~~~





```xml
<div style="margin-left: 350px">
      <h3>日期类对象 #dates</h3>
      <p th:text="${#dates.format(mydate)}"></p>
      <p th:text="${#dates.format(mydate,'yyyy-MM-dd')}"></p>
      <p th:text="${#dates.format(mydate,'yyyy-MM-dd HH:mm:ss')}"></p>
      <p th:text="${#dates.year(mydate)}"></p>
      <p th:text="${#dates.month(mydate)}"></p>
      <p th:text="${#dates.monthName(mydate)}"></p>
      <p th:text="${#dates.createNow()}"></p>
      <br/>

      <h3>内置工具类#numbers，操作数字的</h3>
   	  <!--带￥的数字-->
      <p th:text="${#numbers.formatCurrency(mynum)}"></p>
      <p th:text="${#numbers.formatDecimal(mynum,5,2)}"></p>

      <br/>
      <h3>内置工具类#strings,操作字符串</h3>
      <p th:text="${#strings.toUpperCase(mystr)}"></p>
      <p th:text="${#strings.indexOf(mystr,'power')}"></p>
      <p th:text="${#strings.substring(mystr,2,5)}"></p>
      <p th:text="${#strings.substring(mystr,2)}"></p>
      <p th:text="${#strings.concat(mystr,'---java开发的黄埔军校---')}"></p>
      <p th:text="${#strings.length(mystr)}"></p>
      <p th:text="${#strings.length('hello')}"></p>
      <p th:unless="${#strings.isEmpty(mystr)}"> mystring 不是 空字符串  </p>

      <br/>
      <h3>内置工具类#lists,操作list集合</h3>
      <p th:text="${#lists.size(mylist)}"></p>
      <p th:if="${#lists.contains(mylist,'a')}">有成员a</p>
      <p th:if="!${#lists.isEmpty(mylist)}"> list 集合有多个成员</p>

      <br/>
      <h3>处理null</h3>
      <p th:text="${zoo?.dog?.name}"></p>

  </div>
```



## 9.12 自定义模板



模板是内容复用， 定义一次，在其他的模板文件中多次使用。

模板使用：

1.定义模板

2.使用模板



**模板定义**语法：

```html
th:fragment="模板自定义名称"

例如：
<div th:fragment="head">
    <p>
        动力节点-java开发
    </p>
    <p>
        www.bjpowernode.com
    </p>
</div>
```



引用模板语法：

```html
1) ~{templatename :: selector}
   templatename:  文件名称
   selector： 自定义模板名称
2）templatename :: selector
   templatename:  文件名称
   selector： 自定义模板名称

对于使用模板：有包含模板（th:include）， 插入模板(th:insert)
```





- head.html

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    <div th:fragment="top">
        <p>北京动力节点</p>
        <p>网站 www.bjpowernode.com</p>
    </div>


    <div th:fragment="menu">
        <p>文档|下载|blog</p>

    </div>
</body>
</html>
~~~



- customtpl.html

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>使用模板</title>
</head>
<body>

    <div style="margin-left: 350px">

        <h3>使用其他目录中的模板文件</h3>
        <div th:insert="common/left :: html"></div>
        <br/>
        <br/>
        <div th:include="common/left"></div>




        <div th:insert="head :: menu"></div>


        <h3>插入模板 th:insert</h3>
        <div th:insert="~{ head :: top }">
            在这里放置插入的内容，当让此处的东西会被替换
        </div>

        <br/>
        <br/>
        <h3>插入模板 第二种格式</h3>
        <p th:insert="head :: top">  在这里放置插入的内容，当让此处的东西会被替换</p>


        <br/>
        <br/>
        <h3>包含模板，第一种语法</h3>
        <div th:include="~{ head :: top}">
			这个div会被替换掉
        </div>


        <br/>
        <br/>
        <h3>包含模板，第二种语法</h3>
        <div th:include="head :: top">
			这个div会被替换掉
        </div>


        <br/>
        <br/>
        
        
        <h3>使用整个文件作为复用的内容（整个文件作为模板使用）</h3>
        <div th:include="footer :: html"></div>
        <br/>
        <div th:include="footer" ></div>
        <br/>
        <div th:insert="footer"></div>
    </div>
</body>
</html>
~~~







# 第十章 总结



## 10.1 注解

- Spring + SpringMVC + SpringBoot 



创建对象的：
		@Controller: 放在类的上面，创建控制器对象，注入到容器中

​		@RestController: 放在类的上面，创建控制器对象，注入到容器中。
​             作用：复合注解是@Controller , @ResponseBody, 使用这个注解类的，里面的控制器方法的返回值是json数据

​		@Service ： 放在业务层的实现类上面，创建service对象，注入到容器

​		@Repository : 放在dao层的实现类上面，创建dao对象，放入到容器。没有使用这个注解，是因为现在使用MyBatis框 架，  dao对象是MyBatis通过代理生成									的。 不需要使用@Repository、 所以没有使用

​		@Component:  放在类的上面，创建此类的对象，放入到容器中。 







赋值的：

@Value ： **简单类型的赋值**

~~~java
//在属性的上面使用
@Value("李四") 
private String name；
~~~

可以不写setter，也可以写在setter上

~~~java
@Value("李四") 
public void setName(String name){
    this.name=name;
}
~~~

还可以使用@Value**,获取配置文件者的数据**（properties或yml）。 

~~~java
@Value("${server.port}") 
private Integer port
~~~



@Autowired: 引用类型赋值自动注入的，支持byName, byType. **默认是byType** 。 放在属性的上面，也可以放在构造 方法的上面。 推荐是放在构造方法的上面

@Qualifer:  给引用类型赋值，使用byName方式。 配合@Autowired使用：指定注入某个名字的bean  

@Autowird, @Qualifer都是Spring框架提供的



@Resource ： 来自jdk中的定义， javax.annotation。 实现引用类型的自动注入， 支持byName, byType.
             **默认是byName**, 如果byName失败， 再使用byType注入。 在属性上面使用





配置类相关：
@Configuration ： 放在类的上面，表示这是个配置类，相当于xml配置文件

@Bean：放在配置类中方法的上面， 把方法的返回值对象，注入到spring容器中



加载配置文件到容器中：

@ImportResource ： 加载其他的xml配置文件， 把文件中的对象注入到spring容器中

@PropertySource ： 读取其他的properties属性配置文件





@ComponentScan： 扫描器 ，指定包名，扫描注解的

@ResponseBody: 放在方法的上面，表示方法的返回值是json数据， 不是视图

@RequestBody : 把请求体中的数据，读取出来， 转为java对象使用

@ControllerAdvice:  控制器增强， 放在类的上面， 表示此类提供了方法，可以对controller增强功能。

@ExceptionHandler : 处理异常的，放在方法的上面

@Transcational :  处理事务的， 放在service实现类的public方法上面， 表示此方法有事务



SpringBoot中使用的注解 
@SpringBootApplication ： 放在启动类上面， 包含了@SpringBootConfiguration 配置类 @EnableAutoConfiguration自动装配， @ComponentScan组件扫描



MyBatis相关的注解

@Mapper ： 放在类【数据访问层接口】的上面 ， 让MyBatis找到接口， 创建他的代理对象  

@MapperScan :放在主类【主启动类】的上面 ， 指定扫描的包， 把这个包中的所有接口都创建代理对象。 对象注入到容器中

@Param ： 放在dao接口的方法的形参前面， 作为命名参数使用的。用#{名字}接参数



Dubbo注解
@DubboService: 在提供者端使用的，**暴露服务**【将服务注册给zookeeper】的， 放在接口的**实现类**上面
@DubboReference:  在**消费者端**使用的， 引用远程服务， 放在属性上面使用
@EnableDubbo : 放在主类【主启动类】上面， 表示当前引用启用Dubbo功能

 
