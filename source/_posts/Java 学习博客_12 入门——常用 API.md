---
title: Java 学习博客_12 入门——常用 API
date: 2022-5-5
tags: study
category: java
---



以黑马程序员全套Java教程网课为主。

# Math

不用导包，成员变量、方法都带有static修饰，可以直接通过类名访问。

| 方法                                        | 说明                                     |
| ------------------------------------------- | ---------------------------------------- |
| public static int abs(int a)                | 绝对值，支持 double  float int long 类型 |
| public static double ceil(double a)         | 向上取整，返回一个 double 值             |
| public static double floor(double a)        | 向下取整，返回一个 double 值             |
| public static int round(float a)            | 四舍五入                                 |
| public static int max(int a,int b)          |                                          |
| public static int min(int a,int b)          |                                          |
| public static double pow(double a,double b) | a 的 b 次幂                              |
| public static double random()               | 随机数，范围 [0.0,1.0)                   |

随机数想要其他范围可以做算术运算。如 [0.0, 100.0)就用`random()*100`。

# System

不用导包，不能实例化，static

| 方法                                   | 说明                                               |
| -------------------------------------- | -------------------------------------------------- |
| public static void exit(int status)    | 终止运行 JVM，非零表示非正常终止                   |
| public static long currentTimeMillis() | 返回当前时间到1970年1月1日的时间间隔，以毫秒为单位 |

currentTimeMillis() 可以整除求得当前年份；可以用两个 currentTimeMillis() 值做差求时间间隔等。

# Object

Object 类是所有类的根类。

| 方法                              | 说明                                                   |
| --------------------------------- | ------------------------------------------------------ |
| public String toString()          | 返回对象的字符串表示形式。建议子类全部重写该方法       |
| public boolean equals(Object obj) | 比较对象是否相等（默认比较地址）。重写可以比较对象内容 |



## toString() 方法

Object 中包含 toString 方法

```java
public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

输出类名@+一串哈希值。

一般重写 toString() 方法，用于输出简明扼要的类信息。IDEA 中可以像构造方法和 getter / setter 一样自动生成。类似：

```java
//手动实现
public String toString(){
return "{ Person:name=" + name +" age=" + age+"}";// name 和 age 都是本类中的成员变量
}
```

## equals

`s1.equals(s2)`本质上是比较两个对象的地址，肯定是 false。

一般也会重写/自动生成可以比较两个对象内容是否一致的方法。

```java
public boolean equals(Object o) {
    if (this==o) return true; //同一个对象
    if(o==null||getClass()!=o.getClass()) return false;//o是空对象，或这两个对象不属于一个类
    Student student=(Student) o;//因为调用 equals 传参的时候向上转型了，o 变成了 Object 类，要转回来
    if (age!=student.age)return false; 
    return name!=null?name.equals(student.name):student.name==null;//要么 name 都是空字符串，要么相同。equals() 方法只有非空字符串才能调用
}
```

# Arrays 类

需要导包 `java.util.Arrays`，不能创建对象，成员是 static的。

| 方法                                   | 说明                              |
| -------------------------------------- | --------------------------------- |
| public static String toString(int[] a) | 返回形如 "[a, b, c]" 的字符串形式 |
| public static void sort(int[] a)       | 按数字顺序排列指定的数组          |

# 补充：工具类

以上工具类的特点都是：不能创建对象，但是成员方法都是 static 修饰，可以直接用类名访问。

不能创建对象的方法是：

```java 
public Math(){};
public Arrays(){};
public System(){};
```

自己写了无参构造方法，会把系统默认的无参构造方法覆盖。而该方法又是私有的，因此外界无法访问创建对象。

# 基本类型包装类

其中有一些方法能更好地处理基本数据类型。

包括：Byte, Short, Integer, Long, Float, Double, Character, Boolean

## 方法示例

| 方法                                    | 说明                          |
| --------------------------------------- | ----------------------------- |
| public static Integer valueOf(int i)    | 把 int 转换为 Integer 类型    |
| public static Integer valueOf(String s) | 把 String 转换为 Integer 类型 |

public String[] split(String regex) 把给定字符串用 regex 分隔开。

如给定字符串 s 是"1 2 3 4"，`s.split(" ")`就会得到4个字符串："1""2""3""4"。

## 基本类型转换

int -> String：

① 空字符串""+int 类型

② String s=String.valueOf(i)

String -> int：

① Integer in=Integer.valueOf(s);

int i=intValue(in);

② int i=Integer.parseInt(s);

## 自动装箱和拆箱

装箱：基本类型->对应的包装类型

拆箱：包装类型->基本类型

```java
Integer i=Integer.valueOf(100);//装箱
Integer ii=100;//自动装箱

ii=ii.intValue()+200;//拆箱
ii+=200;//自动拆箱

Integer i3=null;
i3+=300;//NullPointerException。因此对象使用之前最好先判断是否为 null
```

# 日期类

## Date

Date 类，需要导包。**注意是 java.util 下的，而不是 java.sql 下的！**java.util 下的 Date 类非最终类，所以可以有子类， java.sql 下的 Date 类就是其子类。

| 方法                           | 说明                                                   |
| ------------------------------ | ------------------------------------------------------ |
| public Date()                  | 初始化一个 Date 对象，值为其被分配时的时间，精确到毫秒 |
| public Date(long Date)         | 分配一个 Date 对象，值为从标准时间起指定的毫秒数       |
| public long getTime()          | 获取日期对象从标准时间到现在的毫秒数                   |
| public void setTime(long time) | 设置时间，单位为毫秒                                   |

```java
//输出 Date 对象，得到：星期 月份 日期 时 分 秒 CST 年份

long d=1000*60*60;
Date da=new Date(d);//标准时间往后1h，即1970.1.1 上午9点（不是1点，因为 CST 有时差）

long time=System.currentTimeMillis();//可以这样获取当前时间，然后传给 Date() 或 setTime()
```

## SimpleDateFormat

可以自己设置日期格式。

| 方法                                    | 说明                                 |
| --------------------------------------- | ------------------------------------ |
| public SimpleDateFormat()               | 构造方法，并使用系统默认的 Date 格式 |
| public SimpleDateFormat(String pattern) | 构造方法，并使用自己规定的格式       |
| public final String format(Date d)      | 将日期对象转换为相应格式的字符串     |
| public final Date parse(String s)       | 将字符串转换为日期                   |

```java
public static void main(String[] args) throws Parse Exception{//结尾要加一些东西，在 IDEA 中可以自动补全
    Date d=new Date;//被分配时的时间
    SimpleDateFormat sdf=new SimpleDateFormat("yyyy年-MM月-dd日 HH:mm:ss");
    String s=sdf.format(d);
    System.out.println(s);//输出格式就形如 2022年-05月-05日 10:24:00
    
    String ss="2022-05-05 10:24:00";
    SimpleDateFormat sdf1=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date d1=sdf1.parse(ss);
}
```

y 年 M 月 d 日

H 时 m 分 s 秒

（实际运用的时候，也可以让用户自己指定格式。比如单独写一个日期工具类，有两个方法，把Date转换为String和把String转换为Date；这两个方法可以设置两个传入参数：

public static String dateToString(Date d, String format)

public static String StringTodate(String s, String format)

在方法内根据传入的字符串格式，利用SimpleDateFormat的方法进行转换。）

## Calendar 类

需要导包`java.util.Calendar`，是一个抽象类。

为某一时刻和某些日历字段的转换，以及操作日历字段提供了一些方法。

Calendar 类有一个类方法可以获取 Calendar 对象。可是 Calendar 类不是抽象类吗？

```java
Calendar rightnow=Calendar.getInstance();
```

其实这个方法和 Calendar 的直接子类有关，所以这个实例化相当于多态。

| 方法                                                 | 说明                    |
| ---------------------------------------------------- | ----------------------- |
| public int get(int field)                            | 获取给定日历字段的值    |
| public abstract void add(int field, int amount)      | 添加/减去给定的日历字段 |
| public final void set(int year, int month, int date) | 设置日历的年月日        |

```java
y=c.get(Calendar.YEAR);
m=c.get(Calendar.MONTH)+1;//month 是从0开始的，所以要+1
d=c.get(Calendar.DATE);

//例：求2020年2月有多少天?
Calendar c=Calendar.getInstance();
c.set(2020,2,1);//设置为3月1日
c.add(Calendar.DATE,-1);//倒退1天，就是2月的最后一天
System.out.println("2020年2月有"+c.get(Calendar.DATE)+"天");
```

