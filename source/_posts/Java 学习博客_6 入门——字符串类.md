---
title: Java 学习博客_6 入门——字符串类
date: 2022-04-30
tags: study
category: java
---

以黑马程序员全套Java教程网课为主。

# API

JDK 中提供了实现各种功能的封装类，供我们使用。

就像灯的开关，我们只用知道按下开关就能开灯关灯，并不需要弄明白底层原理如何实现。

可以下载一个 java 帮助文档，在其中搜索这些类的具体使用方法。

（*点击索引，打开输入框*

*如果只有左侧目录显示，右侧不显示：*

*右键帮助文档-属性-常规-最下面解除锁定*

*重新打开帮助文档即可。*）

![AWwYX.png](https://s1.328888.xyz/2022/04/30/AWwYX.png)

Pakckage：该类所处的包下（除了 java.lang，其他包都要导包）

下拉还有构造方法、成员方法的详解。

## 字符串输入

查看 Scanner 的帮助文档可以得知，成员方法 nextLine() 可以获取一整行内容，可用于输入接收字符串。

```java
Scanner sc=new Scanner(System.in);
String line=sc.nextLine();
//这里只输入 sc.nextLine() 然后 alt enter 代码自动补全，就会自动生成左边 String line。
```

# String

String 类型代表字符串。其内容都是被双引号引住的。

在 java.lang 包下，不用导包。

```java
String s="abc";//直接赋值
String s1=new String();//空字符串

char[] c={'a','b','c'};
String s2=new String(c);//根据字符数组创建字符串

byte[] b={97,98,99};//a, b, c 对应的 ascii 码
String s3=new String(b);
```

字符串一旦创建不能再修改。不过多个字符串的值可以共享`s1=s2;`

字符串在效果上像字符数组 char[]，但（JDK9）以后底层实现方法其实是字节数组 byte[]。

## 字符串比较：== 和成员方法 equals()

用==判断的比较，是比较 s1 和 s2 的值（即：对应字符串的地址值）是否相同。

**基本类型 == 比较的是数据值是否相同，引用类型 == 比较的是地址是否相同。**

用字符串的成员方法 equals() 判断，是比较字符串内容是否相同。

```java
char[] c={'a','b','c'};
String s1=new String(c);
String s2=new String(c);
System.out.println(s1==s2);//输出 false，因为 s1 s2 地址不同，只有内容是一样的
System.out.println(s1.equals(s2));//输出 true

String s3="abc";
String s4="abc";
System.out.println(s3==s4);//输出 true，因为对于相同内容的字符串，JVM 会建立一个字符串对象（在堆内存的字符串池中）供它俩参考。
System.out.println(s3.equals(s4));//输出 true

System.out.println(s1==s3);//输出 false
System.out.println(s1.equals(s3));//输出 true
```

## 遍历字符串：成员方法 length() 和 charAt()

`s.length()`可以获取字符串长度

s.charAt(i) 可以获取索引为 i 处的字符

```java
for(int i=0;i<s.length;i++){
    System.out.println(s.charAt(i));
}
```

## 字符串拼接

字符串可以直接用 + 号拼接。

```java
String s1="Hello ";
String s2="World";
s1=s1+s2;//Hello World
```

在内存中，字符串发生拼接后会在堆内存中新建一个字符串（有"Hello "，"World"，"Hello World"三个字符串，而不是直接在"Hello "的位置上拼接"World"）。这样操作还是比较费时费空间的。之后介绍的另一个类——StringBuilder 可以更有效地解决这个问题。

## endsWith()

查看字符串是否以指定子串结尾。

```java
String s1="hello world";
s1.endsWith("world");
```

# StringBuilder 类

与 String 类相比，最主要的特点在于内容可变。

在 java.lang 包下，不用导包。

构造方法：

| 构造方法名              | 说明                                              |
| ----------------------- | ------------------------------------------------- |
| StringBuilder()         | 无参构造方法                                      |
| StringBuilder(String s) | 把给定的 String 字符串转换成 StringBuilder 类型的 |

| 成员方法名              | 说明                                          |
| ----------------------- | --------------------------------------------- |
| append(String s)        | 在结尾拼接上字符串 s                          |
| StringBuilder reverse() | 反转字符串                                    |
| String toString()       | 把 StringBuilder 类型转换为 String 类型并返回 |

用 StringBuilder 完成字符串拼接操作：

1. String 类型转换为 StringBuilder 类型
2. StringBuilder 类型通过 append() 成员方法拼接字符串
3. StringBuilder 类型通过 toString() 成员方法转换为 String 类型

```java
String s="Hello ";
StringBuilder sb=new StringBuilder(s);//或者先无参构造，再在结尾拼接 append(s)，但有点多此一举
sb.append("World");
String s1=sb.toString();

//骚操作：匿名对象，使用完立刻被垃圾回收器回收，建议少用
String s1=new StringBuilder(s).append("World").toString();
```

用 StringBuilder 完成字符串反转操作：

```java
String s="Hello World";
String sr=new StringBuilder(s).reverse().toString();
```

