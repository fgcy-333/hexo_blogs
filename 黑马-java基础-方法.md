---
title: 方法
date: 2022-08-24 23:11:56
tags:
- 黑马程序员-徐磊-java基础
categories: 
- 视频学习笔记
---

## 一、什么是方法

### 1.0 方法概念

它可以把一段代码封装成一个功能，以方便重复调用



### 2.0 好处

1. 提高代码复用
2. 逻辑清晰

<!--more-->

### 3.0 定义格式

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/Snipaste_2022-03-04_10-48-19.png)

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/Snipaste_2022-03-04_10-54-15.png)

---

1. 权限修饰符：pulic 所有类都可以访问，protected同一个包下的类一级该类的子类，不写（缺省）包权限同一个包内有效，private本类有效
2. static修饰代表这是一个静态方法，随类加载一次
3. 如果方法申明了具体的返回值类型，内部必须使用return返回对应类型的数据。
4. 如果是void 就直接return; 方法中看到return就会结束方法(会先返回返回值)
5. return语句下面，不能编写代码，因为永远执行不到，属于无效的代码。
6. 形参列表可以有多个，甚至可以没有； 如果有多个形参，多个形参必须用","隔开，且不能给初始化值。
7. 同一个类中的同名方法的不同形参列表构成的两个以上的方法称为方法重载（方法名，形参列表）
8. 用final修饰的入参不赋初始值，入参不能赋初值



### 4.0 方法常见问题

1. 方法的编写顺序无所谓。
2. 方法与方法之间是平级关系，不能嵌套定义。可以嵌套使用
3. 方法不调用就不执行, 调用时必须严格匹配方法的参数情况。
4. 有返回值的方法调用时可以选择定义变量接收结果，或者直接输出调用，甚至直接调用；无返回值方法的调用只能直接调用。
5. 有static修饰符修饰的是静态成员方法，一般通过类名调用；也可以通过该类实例调用(不推荐)



## 二、方法内存图

---

![](https://cdn.jsdelivr.net/gh/fgcy-333/gitnote-images/Snipaste_2022-03-04_11-12-56.png)

---

字节码文件存放在方法区中，在字节码文件中会暴露访问静态方法的入口
但方法是运行在栈内存中的，存放在方法中的局部变量也是存放在栈中





## 三、方法参数转递机制（都是值传递）

- 基本类型的参数传递形式

​    传输实参给方法的形参的时候，并不是传输实参变量本身， 而是传输实参变量中存储的数据值，这就是值传递



- 引用数据类型的参数传递形式

​    传输实参给方法的形参的时候，并不是传输实参变量本身， 而是传输实参变量中存储的地址值，这就是值传递



- Java的参数传递机制是什么

​	值传递，传输的是实参存储的值。



- 打印整型数组内容

~~~java
    /**
     * 数组打印
     */
    @Test
    public void test5() {
        int[] array = {};
        System.out.println(toString(array));
    }

    public String toString(int[] array) {
        String temp = "";
        if (array == null) {
            return null;
        } else {
            temp += "[";
            if (array.length > 0) {
                for (int i = 0; i < array.length; i++) {
                    temp += i == array.length - 1 ? array[i] : array[i] + ",";
                }
            }
            temp += "]";
        }
        return temp;
    }
~~~



- 从数组中查询指定元素的索引

~~~java
    /**
     * 返回索引
     */
    @Test
    public void test6() {
//        int[] array = {4,2,1,4};
        int[] array = null;
        System.out.println(indexOf(array, 4));
    }

    public Integer indexOf(int[] ints, int target) {
        if (ints == null) {
            return null;
        } else {
            for (int i = 0; i < ints.length; i++) {
                if (ints[i] == target) {
                    return i;
                }
            }
            return -1;
        }
    }
~~~



- 比较两个数组内容是否一致

~~~java
    /**
     * 比较数组是否一致
     */
    @Test
    public void test7() {
        int[] array = {4, 2, 1, 4};
        int[] array1 = {4, 2, 4};
        System.out.println(equals(array, array1));
    }

    public boolean equals(int[] ints1, int[] ints2) {
        if (ints1 == null || ints2 == null) {
            return false;
        }
        if (ints1.length != ints2.length) {
            return false;
        }
        for (int i = 0; i < ints1.length; i++) {
            if (ints1[i] != ints2[i]) {
                return false;
            }
        }
        return true;
    }
~~~



## 四、方法的其他形势、技术

### 1.0 方法重载

1. 同一个类中，出现多个方法名称相同，但是形参列表是不同的，那么这些方法就是重载方法
2. 调用方法的时候,会通过参数的不同来区分调用的是哪个方法
3. 方法重载的作用
   可读性好，方法名称相同提示是同一类型的功能，通过形参不同实现功能差异化的选择，这是一种专业的代码设计.(同一功能的差异化实现)
4. 提高开发效率（调用兄弟方法）



- 代码体现

~~~java
    public static void main(String[] args) {
        fire();
        fire("中国");
        fire("一百", "中国");
    }

    public static void fire(String numbers, String location) {
        System.out.println("发射" + numbers + "枚导弹给" + location);
    }

    public static void fire(String location) {
        fire("一", location);
    }


    public static void fire() {
        fire("美国");
    }
~~~





### 2.0 方法重载的识别技巧

1. 形参列表不同指的是：形参的个数、类型、顺序不同，不关心形参的名称 (顺序 个数 类型)
2. 只要是同一个类中，方法名称相同、形参列表不同，那么他们就是重载的方法，其他都不管！（如：修饰符，返回值类型都无所谓）



~~~java
private void open(){}

protece void open(int a){}

public void open(int a,int b){}

void open(int a,int b){}//不是

public void open(double a,int b){}

public void open(int a,double b){}
~~~



### 3.0 return关键字

作用:结束当前方法



## 五、综合案例

### 1.0 买飞机票

~~~java
public static void main(String[] args) {
        final Scanner sc = new Scanner(System.in);
        System.out.print("请输入月份：");
        final int month = sc.nextInt();
        System.out.print("请输入飞机票类型（头等舱，经济舱）：");
        final String type = sc.next();
        System.out.print("请输入飞机票价格：");
        final double price = sc.nextDouble();
        System.out.println(calc(month, type, price));
    } 

    private static double calc(int month, String type, double price) {
        if (month >= 5 && month <= 10) {
            switch (type) {
                case "头等舱":
                    price *= 0.9;
                    break;

                case "经济舱":
                    price *= 0.85;
                    break;
                default:
                    System.out.println("机票类型输入有误！");
                    price = -1;
            }
        } else if (month == 11 || month == 12 || month >= 1 && month <= 4) {
            switch (type) {
                case "头等舱":
                    price *= 0.7;
                    break;

                case "经济舱":
                    price *= 0.65;
                    break;
                default:
                    System.out.println("机票类型输入有误！");
                    price = -1;
            }
        } else {
            System.out.println("月份输入有误");
            price = -1;
        }
        return price;
    }
~~~



### 2.0 找素数

~~~java
  /**
     * ClassName: MethodDemo2
     * Description:素数判断
     * date:2022/3/4
     *
     * @author fgcy
     * @since JDK 1.8
     */

    @Test
    public void test1() {
        for (int i = 101; i <= 200; i++) {
            //标志位，默认判断这个数是素数，当有一个数（范围2-sqrt（数））可以被这个数整除，则这个数不是素数
            boolean flags = true;
            for (int j = 2; j <= Math.sqrt(i); j++) {
                if (i % j == 0) {
                    //这个数不是素数
                    flags = false;
                    //可以判断下一个数
                    break;
                }
            }
            if (flags) {
                System.out.print(i + " ");
            }
        }
    }
~~~

~~~
101 103 107 109 113 127 131 137 139 149 151 157 163 167 173 179 181 191 193 197 199 
~~~



### 3.0 验证码

~~~java
    /**
     * ClassName: MethodDemo2
     * Description:验证码
     * date:2022/3/4
     *
     * @author fgcy
     * @since JDK 1.8
     */

    @Test
    public void test2() {
        System.out.println(createCode(5));
    }

    private String createCode(int i) {
        final Random random = new Random();
        String code = "";
        for (int j = 0; j < i; j++) {
            final int type = random.nextInt(3);
            switch (type) {
                //大写字母
                case 0:
                    code += (char) (random.nextInt(26) + 65);//65----->65+25
                    break;
                //小写字母
                case 1:
                    code += (char) (random.nextInt(26) + 97);//97----->97+25
                //0----9数字
                case 2:
                    code += random.nextInt(10);
            }
        }
        return code;
    }
~~~

~~~
0Dp34C
~~~





### 4.0 数组数据加密

~~~java
    @Test
    public void test3() {
        int[] arrays = {1, 9, 8, 3};
        System.out.println("原始数据：" + printAray(arrays));
        System.out.println("加密后数据：" + md5(arrays));
    }

    /**
     * Description:数据加密
     * date:2022/3/4
     */

    private String md5(int[] arrays) {
        for (int i = 0; i < arrays.length; i++) {
            arrays[i] = (arrays[i] + 5) % 10;
        }
        return printAray(arrays);
    }

    /**
     * Description:int类型数组反转
     * date:2022/3/4
     */
    private int[] reverce(int[] arrays) {
        for (int i = 0, j = arrays.length - 1;
             i < j;
             i++, j--) {
            int temp = arrays[i];
            arrays[i] = arrays[j];
            arrays[j] = temp;
        }
        return arrays;
    }


    /**
     * Description:数组打印
     * date:2022/3/4
     */

    public String printAray(int[] arrays) {
        String s = "";
        if (arrays == null) {
            return null;
        } else {
            s += "[";
            for (int i = 0; i < arrays.length; i++) {
                s += i == arrays.length - 1 ? arrays[i] : arrays[i] + ",";
            }
            s += "]";
        }
        return s;
    }
~~~





### 5.0 抽奖

~~~java
    /**
     * Description: 抽奖
     * date:2022/3/4
     */

    @Test
    public void test6() {
        int[] arrays = {2, 588, 888, 1000, 10000};
        int[] indexs = {-1, -1, -1, -1, -1};
        final Random r = new Random();

        while (true) {
            boolean flag = true;
            final int index = r.nextInt(5);
            for (int i = 0; i < indexs.length; i++) {
                if (indexs[i] == index) {
                    flag = false;//有人抽到过
                    break;
                }
            }
            if (flag) {//没人抽到过
                for (int i = 0; i < indexs.length; i++) {
                    if (indexs[i] == -1) {
                        indexs[i] = index;
                        System.out.println("已经抽取" + arrays[index] + "￥的红包");
                        break;
                    }
                }
            }
            if (indexs[indexs.length - 1] != -1) {
                break;
            }
        }
    }
~~~





### 6.0 双色球

~~~java
 public static void main(String[] args) {
        judge(createLuckNumbers(), userNumber());
    }


    /**
     * ClassName: MethodDemo3
     * Description:判断是否中奖
     * date:2022/3/4
     */

    private static void judge(int[] luckNumbers, int[] userNumber) {
        int blue = 0;
        int red = 0;

        //0 1 2 3 4 5

        for (int i = 0; i < userNumber.length - 1; i++) {
            for (int i1 = 0; i1 < luckNumbers.length - 1; i1++) {
                if (userNumber[i] == luckNumbers[i1]) {
                    red++;
                    break;
                }
            }
        }
        blue = luckNumbers[luckNumbers.length - 1] == userNumber[userNumber.length - 1] ? 1 : 0;

        System.out.print("您的号码：");
        printAray(userNumber);
        System.out.print("中奖号码:");
        printAray(luckNumbers);
        if (blue == 1 && red <= 2) {
            System.out.println("五鸡");
        } else if (red == 4 && blue == 0 || red == 3 && blue == 1) {
            System.out.println("十鸡");
        } else if (red == 5 && blue == 0 || red == 4 && blue == 1) {
            System.out.println("两百");
        } else if (red == 5 && blue == 1) {
            System.out.println("三千");
        } else if (red == 6 && blue == 0) {
            System.out.println("五百万");
        } else if (red == 6 && blue == 1) {
            System.out.println("一百万");
        } else {
            System.out.println("恭喜，又没中奖！");
        }
    }

    /**
     * ClassName: MethodDemo2
     * Description:从键盘中获取数据
     */

    private static int[] userNumber() {
        final int[] ints = new int[7];
        final Scanner sc = new Scanner(System.in);
        System.out.print("请输入六个红球号码（1--33）用逗号隔开：");
        final String s = sc.nextLine();
        final String[] strings = s.split(",");
        for (int i = 0; i < ints.length - 1; i++) {
            ints[i] = Integer.valueOf(strings[i]);
        }
        System.out.print("请输入一个蓝球（1----16）:");
        ints[ints.length - 1] = sc.nextInt();
        return ints;
    }


    /**
     * ClassName: MethodDemo20
     * Description: 生成双色球
     */

    private static int[] createLuckNumbers() {
        final int[] luckNumbers = new int[7];
        final Random r = new Random();
        for (int i = 0; i < luckNumbers.length - 1; i++) {
            while (true) {
                boolean flag = true;
                final int tempNumber = r.nextInt(33) + 1;
                for (int j = 0; j < i; j++) {
                    if (luckNumbers[j] == tempNumber) {
                        flag = false;
                        break;
                    }
                }
                if (flag) {
                    luckNumbers[i] = tempNumber;
                    break;
                }
            }
        }
        luckNumbers[luckNumbers.length - 1] = r.nextInt(17) + 1;
        return luckNumbers;
    }


    /**
     * ClassName: MethodDemo2
     * Description:数组打印
     */

    public static void printAray(int[] arrays) {
        String s = "";
        if (arrays == null) {
            System.out.println("null");
            return;
        } else {
            s += "[";
            for (int i = 0; i < arrays.length; i++) {
                s += i == arrays.length - 1 ? arrays[i] : arrays[i] + ",";
            }
            s += "]";
        }
        System.out.println(s);
    }
~~~

