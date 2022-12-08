---
title: xml与设计模式
date: 2022-12-02 00:10:24
tags:
- 黑马程序员-徐磊-java基础
categories: 
- 视频学习笔记
index_img: /images/java.png
---

# XML

## XML概述：

XML是可扩展标记语言（eXtensible Markup Language）的缩写，它是是一种数据表示格式，可以描述非常复杂的数据结构，常用于传输和存储数据。

可扩展标记语言 、一种表示数据的格式 、用于自定义数据格式

例如：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<data>
    <sender>张三</sender>
    <receiver>李四</receiver>
    <src>
        <addr>北京</addr>
        <date>2022-11-11 11:11:11</date>
    </src>
    <current>武汉</current>
    <dest>广州</dest>
</data>
~~~

## XML的几个特点和使用场景

一是纯文本，默认使用UTF-8编码；二是可嵌套；如果把XML内容存为文件，那么它就是一个XML文件。

- XML的使用场景：XML内容经常被当成消息进行网络传输，或者作为配置文件用于存储系统的信息。 作为：配置文件、数据存储、数据传输

- 特点、纯文本、utf-8、可嵌套







## XML的创建、语法规则

XML的创建：就是创建一个XML类型的文件，要求文件的后缀必须使用xml



- **语法规则**：XML文件的后缀名为：xml 、文档声明必须是第一行

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
~~~

version：XML默认的版本号码、该属性是必须存在的
encoding：本XML文件的编码



XML的标签(元素)规则:

- 标签由一对尖括号和合法标识符组成: `<name></name>`，必须存在一个根标签，有且只能有一个。

- 标签必须成对出现，有开始，有结束: `<name></name>`特殊的标签可以不成对，但是必须有结束标记，如:`<br/>`

- 标签中可以定义属性，属性和标签名空格隔开,属性值必须用引号引起来`<student id = “1"></name>`标签需要正确的嵌套



XML的其他组成

- 文件后缀必须是xml
- 文档声明必须是第一行
- 必须存在一个根标签，有且只能有一个
- XML文件中可以定义注释信息：`<!-- 注释内容 -->`
- 标签必须成对出现，有开始，有结束标签: `<name></name>`
- 必须能够正确的嵌套

- XML文件中可以存在CDATA区: `<![CDATA[   …内容…  ]]>`

- XML的特殊字符

~~~text
&lt;    < 小于
&gt;    >  大于
&amp;   &  和号
&apos;  '  单引号
&quot;  "  引号
~~~



----

<img src="https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282259674.png" alt="image-20221128225837320" style="zoom:80%;" />

----

浏览器打开内容如下：

---

![image-20221128225920754](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282259949.png)

---



## XML文档约束

什么是文档约束？

----

![image-20221128230108577](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282301411.png)

-----

> 问题：由于XML文件可以自定义标签，导致XML文件可以随意定义，程序在解析的时候可能出现问题。

文档约束：是用来限定xml文件中的标签以及属性应该怎么写。以此强制约束程序员必须按照文档约束的规定来编写xml文件。

文档约束的分类:DTD、schema





### 方式一:DTD文档约束

需求：利用DTD文档约束，约束一个XML文件的编写。

分析：
①：编写DTD约束文档，后缀必须是.dtd `data.dtd`

~~~dtd
<!ELEMENT 书架 (书+)>
<!ELEMENT 书 (书名,作者,售价)>
<!ELEMENT 书名 (#PCDATA)>
<!ELEMENT 作者 (#PCDATA)>
<!ELEMENT 售价 (#PCDATA)>
~~~

②：在需要编写的XML文件中导入该DTD约束文档 `data.xml`

③：按照约束的规定编写XML文件的内容。

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--导入约束-->
<!DOCTYPE 书架 SYSTEM "data.dtd">
<书架>
    <书>
        <书名>java</书名>
        <作者>111</作者>
        <售价>无价</售价>
    </书>
    <书>
        <书名>js</书名>
        <作者>222</作者>
        <售价>不买</售价>
    </书>
</书架>
~~~



---

![image-20221128230853769](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282308939.png)

----

问题：

- 可以约束XML文件的编写。
- 不能约束具体的数据类型





### 方式二:schema约束

schema可以约束具体的数据类型，约束能力上更强大。

schema本身也是一个xml文件，本身也受到其他约束文件的要求，所以编写的更加严谨

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282312448.png)

----

需求：利用schema文档约束，约束一个XML文件的编写。
分析：
①：编写schema约束文档，后缀必须是.xsd，具体的形式到代码中观看。`data.xsd`

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.fgcy.cn"
        elementFormDefault="qualified">

    <!--targetNamespace:申明约束文档地址（命名空间）-->
    <element name="书架">
        <complexType>
            <!--写子元素-->
            <!--maxOccurs：书架下的子元素可以有任意多个-->
            <sequence maxOccurs="unbounded">
                <element name="书">
                    <!--写子元素-->
                    <complexType>
                        <sequence>
                            <element name="书名" type="string"/>
                            <element name="价格" type="string"/>
                            <element name="售价" type="double"/>
                        </sequence>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>
</schema>

~~~

②：在需要编写的XML文件中导入该schema约束文档
③：按照约束内容编写XML文件的标签。

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<书架 xmlns="http://www.fgcy.cn"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.fgcy.cn data.xsd">
    <书>
        <书名>神雕侠侣</书名>
        <价格>不买</价格>
        <售价>123.111</售价>
    </书>

    <书>
        <书名>神雕侠侣2</书名>
        <价格>不买3</价格>
        <售价>123</售价>
    </书>
</书架>
~~~



可以约束XML文件的标签内容格式，以及具体的数据类型。
本身也是xml文件，格式更严谨。





## XML解析技术概述

XML的数据的作用是什么，最终需要怎么处理？

存储数据、做配置信息、进行数据传输。最终需要被程序进行读取，解析里面的信息。



什么是XML解析?

使用程序读取XML中的数据

---

<img src="https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282330210.png" alt="image-20221128233015676" style="zoom:80%;" />

----



**两种解析方式**

- SAX解析(一行一行解析)

- DOM解析(整个文档一齐解析 常用)





Dom常见的解析工具

----

<img src="https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282331859.png" alt="image-20221128233059230" style="zoom:80%;" />

----



DOM解析解析文档对象模型:

----

![image-20221128233444855](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282334966.png)

----

文档对象模型

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202211282335625.png)

---

XML的数据的作用是什么，最终需要怎么处理？

- 存储数据、做配置信息、进行数据传输。

- 最终需要被程序进行读取，解析里面的信息





## Dom4j解析XML-得到Document对象

**SAXReader类**

----

![image-20221201222138025](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212012221213.png)

-----



~~~java
    @Test
    public void test1() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        //File
        File file = new File("src/main/resources/Contacts.xml");//这里是maven项目
        //File file = new File("xmlDemo/src/main/resources/Contacts.xml");如果是一个空项目就这要取
        Document doc = saxReader.read(file);
        System.out.println(doc);//org.dom4j.tree.DefaultDocument@512ddf17

        //InputStream
        InputStream inputStream = Demo1.class.getResourceAsStream("/Contacts.xml");//读取编译后类路径的文件
        Document document = saxReader.read(inputStream);
        System.out.println(document);//org.dom4j.tree.DefaultDocument@77556fd

    }
~~~



**Dom4j**

~~~xml
        <!-- https://mvnrepository.com/artifact/org.dom4j/dom4j -->
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.1</version>
        </dependency>
~~~





---

![image-20221201223909935](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212012239134.png)

----





## Dom4J解析XML文件

**Document类**

----

![image-20221201222334968](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212012223194.png)

----

![image-20221201223832817](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212012238818.png)

----



```java
    @Test
    public void bbbb() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        InputStream inputStream = Demo1.class.getResourceAsStream("/Contacts.xml");//读取编译后类路径的文件
        Document doc = saxReader.read(inputStream);

        Element rootElement = doc.getRootElement();//获取根元素
        System.out.println(rootElement.getName());//contactList

        List<Element> elements = rootElement.elements();//获取该元素下的所有儿子节点
        elements.stream().forEach(obj -> System.out.print(obj.getName() + "、"));//contact、contact、contact、user、
        System.out.println();

        Element contact = rootElement.element("contact");//获取某一个子元素，默认第一个
        System.out.println(contact.getName());//contact
        System.out.println(contact.elementText("name"));//   潘金莲
        Attribute idAttribute = contact.attribute("id");
        System.out.println(idAttribute.getName() + " = " + idAttribute.getValue());//id = 1
        Element email = contact.element("email");
        System.out.println(email.getText());//panpan@itcast.cn
    }
```



Contacts.xml：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
    </user>
</contactList>
~~~



Dom4J的解析思想？

- 得到文档对象Document，从中获取元素对象和内容



## XML解析案例

Contacts.xml：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
    </user>
</contactList>
~~~



实体类Contact：

~~~java
/**
 * @Author fgcy
 * @Date 2022/12/1
 */
public class Contact {
    private Integer id;
    private String name;
    private Boolean vip;
    private char gender;
    private String email;

    @Override
    public String toString() {
        return "Contact{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", vip=" + vip +
                ", gender=" + gender +
                ", email='" + email + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public Contact setId(Integer id) {
        this.id = id;
        return this;
    }

    public String getName() {
        return name;
    }

    public Contact setName(String name) {
        this.name = name;
        return this;
    }

    public Boolean getVip() {
        return vip;
    }

    public Contact setVip(Boolean vip) {
        this.vip = vip;
        return this;
    }

    public char getGender() {
        return gender;
    }

    public Contact setGender(char gender) {
        this.gender = gender;
        return this;
    }

    public String getEmail() {
        return email;
    }

    public Contact setEmail(String email) {
        this.email = email;
        return this;
    }
}
~~~

解析xml文档：

~~~java
import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.util.ArrayList;
import java.util.List;

/**
 * @Author fgcy
 * @Date 2022/12/1
 */
public class Dom4jTest {
    public static void main(String[] args) throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(Dom4jTest.class.getResourceAsStream("/Contacts.xml"));
        Element root = document.getRootElement();

        ArrayList<Contact> contacts = new ArrayList<>();
        List<Element> elementList = root.elements("contact");
        for (Element element : elementList) {
            Contact contact = new Contact();
            String id = element.attribute("id").getValue();
            String vip = element.attribute("vip").getValue();
            String name = element.elementTextTrim("name");
            String gender = element.elementTextTrim("gender");
            String email = element.elementTextTrim("email");
            contact.setEmail(email);
            contact.setGender(gender.charAt(0));
            contact.setName(name);
            contact.setId(Integer.valueOf(id));
            contact.setVip("true".equals(vip));
            contacts.add(contact);
        }
        contacts.forEach(System.out::println);
    }
}
~~~



## XML检索技术：Xpath

如果需要从XML文件中检索需要的某个信息（如name）怎么解决？

- Dom4j需要进行文件的全部解析，然后再寻找数据。

- Xpath技术更加适合做信息检索。



XPath在解析XML文档方面提供了一独树一帜的路径思想，更加优雅，高效

XPath使用路径表达式来定位XML文档中的元素节点或属性节点。



通过dom4j的SAXReader获取Document对象

利用XPath提供的API,结合XPath的语法完成选取XML文档元素节点进行解析操作



**Document中与Xpath相关的API如下：**

----

![image-20221201232517691](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212012325169.png)

----

Xpath:

~~~xml
        <!-- https://mvnrepository.com/artifact/jaxen/jaxen -->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.1.6</version>
        </dependency>
~~~



### Xpath的四大检索方案

**绝对路径:**

采用绝对路径获取从根节点开始逐层的查找/contactList/contact/name节点列表并打印信息

----

![image-20221202000551042](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212020005402.png)

---



Contact2.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
        <contact>
            <info>

                <name>我是西门庆</name>
            </info>
         </contact>
    </user>
</contactList>
~~~



~~~java
    @Test
    public void aaa() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(XpathDemo.class.getResourceAsStream("/Contacts2.xml"));

        //检索所有contact下的名称
        List<Node> nameNodes = document.selectNodes("/contactList/contact/name");
        nameNodes.forEach(obj -> System.out.print(((Element) obj).getTextTrim() + " "));//潘金莲 武松 武大狼

    }
~~~





**相对路径:**

先得到根节点contactList再采用相对路径获取下一级contact 节点的name子节点并打印信息

---

![image-20221202000610742](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212020006526.png)

----



Contact2.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
        <contact>
            <info>

                <name>我是西门庆</name>
            </info>
         </contact>
    </user>
</contactList>
~~~



~~~java
    @Test
    public void bbb() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(XpathDemo.class.getResourceAsStream("/Contacts2.xml"));

        //检索所有contact下的名称
        Element rootElement = document.getRootElement();
        List<Node> nameNodes = rootElement.selectNodes("./contact/name");//.代表当前元素
        nameNodes.forEach(obj -> System.out.print(((Element) obj).getTextTrim() + " "));//潘金莲 武松 武大狼
    }
~~~









**全文检索**

直接全文搜索所有的name元素并打印

---

![image-20221202000631101](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212020006574.png)

---



Contact2.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
        <contact>
            <info>

                <name>我是西门庆</name>
            </info>
         </contact>
    </user>
</contactList>
~~~



~~~java
    @Test
    public void ccc() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(XpathDemo.class.getResourceAsStream("/Contacts2.xml"));

        //检索所有名称
        List<Node> nameNodes = document.selectNodes("//name");
        nameNodes.forEach(obj -> System.out.print(((Element) obj).getTextTrim() + " "));//潘金莲 武松 武大狼 我是西门庆
        System.out.println(" ");

        //检索所有contact下一级的名称
        List<Node> nodeList = document.selectNodes("//contact/name");
        nodeList.forEach(obj -> System.out.print(((Element) obj).getTextTrim() + " "));//潘金莲 武松 武大狼

        System.out.println();

        //检索所有contact下的名称
        List<Node> nodes = document.selectNodes("//contact//name");
        nodes.forEach(obj -> System.out.print(((Element) obj).getTextTrim() + " "));//潘金莲 武松 武大狼 我是西门庆
    }
~~~





**属性查找**

在全文中搜索属性，或者带属性的元素

----

![image-20221202000655565](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202212020006845.png)

----



Contact2.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
    <contact id="1" vip="true">
        <name>   潘金莲  </name>
        <gender>女</gender>
        <email>panpan@itcast.cn</email>
    </contact>
    <contact id="2" vip="false">
        <name>武松</name>
        <gender>男</gender>
        <email>wusong@itcast.cn</email>
    </contact>
    <contact id="3" vip="false">
        <name>武大狼</name>
        <gender>男</gender>
        <email>wuda@itcast.cn</email>
    </contact>
    <user>
        <contact>
            <info>
                <name id="555">我是西门庆</name>
            </info>
         </contact>
    </user>
</contactList>

~~~

~~~java
    @Test
    public void dddd() throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(XpathDemo.class.getResourceAsStream("/Contacts2.xml"));
        //检索属性
        List<Node> nodes = document.selectNodes("//@id");
        nodes.forEach(obj -> System.out.print(((Attribute) obj).getName() + "=" + ((Attribute) obj).getValue() + " "));//id=1 id=2 id=3 id=555
        System.out.println();

        //检索包含某个属性的元素
        //Node node = document.selectSingleNode("//name[@id]");
        Node node = document.selectSingleNode("//name[@id=555]");//精准定位
        System.out.println(((Element) node).getTextTrim());//我是西门庆
    }
~~~



Xpath作用，四大类。

检索XML文件中的信息绝对路径： /根元素/子元素/孙元素

相对路径：./子元素/孙元素

全文检索：//contact

属性查找：//@属性名 、//元素[@属性名]、//元素//[@属性名=‘值’]



# 设计模式

23种

## 工厂模式



什么是工厂设计模式？

之前我们创建类对象时, 都是使用new 对象的形式创建,在很多业务场景下也提供了不直接new的方式 。工厂模式（Factory Pattern）是 Java 中最常用的设计模式之一，  这种类型的设计模式属于创建型模式，它提供了一种获取对象的方式



**工厂设计模式的作用：**

- 工厂的方法可以封装对象的创建细节，比如：为该对象进行加工和数据注入。

- 可以实现类与类之间的解耦操作（核心思想）。

---



## 装饰模式

什么是装饰设计模式？

创建一个新类，包装原始类，从而在新类中提升原来类的功能。



装饰设计模式的作用：

作用：装饰模式指的是在不改变原类的基础上, 动态地扩展一个类的功能。







