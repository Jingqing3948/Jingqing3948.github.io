---
title: Java 学习博客_3 入门——基础语法
date: 2022-04-29
tags: study
category: java
---

以[黑马程序员全套Java教程](https://www.bilibili.com/video/BV18J411W7cE?p=1)网课为主。

# 数据输入

输入通过 Scanner 类实现。Scanner 类在 java.util 包下，要先导包才能使用。

```java
import java.util.Scanner;//导包
Scanner sc=new Scanner(System.in);//创建对象。这句话中除了 sc 是变量名可以修改，其他的都不能改。
int i=sc.nextInt();//接收数据
String s=sc.nextLine();
```

# 分支、循环语句

if - else 语句：和 C 语言一样。

switch 语句：表达式的取值可以是 byte、short、int、char，JDK5之后可以是枚举，JDK7之后可以是 String。其他 case break default 用法和 C 一样。

for, while, do - while 语句，及 break continue 和 C 一样。

# 随机数

Random 类，在 java.util 包下，需要导包。

```java
import java.util.Random;
Random r=new Random();
int n=r.nextInt(10);//[0,10)的范围内取随机数
```

# IDEA

集成环境：能够把代码编写、编译、执行、调试等功能集合到一起的开发工具。IDEA  就是 java 的集成环境之一。

创建项目-项目内创建模块-模块 src 下创建包-包下创建类-类中编写代码。

其中，.class 文件都会放在模块同级的 out 文件夹中。

<img src="https://img-blog.csdnimg.cn/img_convert/a43b3155d7887b00874a916b9d6f7a47.png" alt="AJdw2.png" style="zoom: 50%;" />

可以在 File - Settings - Editor - Font 中修改字体

## 快捷键

psvm + Tab：public static void main

sout + Tab：System.out.println()

Ctrl + Alt + Space：自动补全代码

Ctrl + /：单行注释

Ctrl + Shift + /：多行注释

Ctrl + Alt + L：代码格式化

## 模块操作

新增模块： File - Project Structure - Modules - New Module - Java - 命名，修改路径

删除模块：右键模块 - Remove Module

​	不过删除后在文件目录中仍然存在。

导入模块：File - Project Structure - Modules - (+) - Import Modules - 选择模块。

​	如果显示 project SDK is not defined：点 set up.

## 断点

DeBug 调试，断点调试，可以查看程序运行细节。

添加断点：在行号前点一下，出现红点。

调试：右键，不是 run ,而是 debug。

Debugger 窗口下的Frames：查看代码运行到了哪里，也可以查看方法调用情况。

Debugger 窗口下的Variables：查看各变量运行过程中值的变化

Console 窗口：查看代码运行过程中的输出变化。如果代码中出现需要输入的情况，需要先在 Console 窗口中输入才能继续执行。

step info 按钮：下一步

stop：停止

再次点击红点，取消该断点。

批量取消断点：点击两个红点的图案，选择 Java line breakpoints，点击 - 号即可全部删除。

# 数组

一次性声明大量**同类型**变量。

```java
int[] arr;//推荐。定义了一个 int 型的数组，数组名是 arr
int arr[];//定义了一个 int 型变量，变量名是 arr 数组

int[] arr=new int[]{1,2,3};//静态初始化
int[] arr={1,2,3};//静态初始化简化版
int[] arr=new int[3];//动态初始化，只申请了空间，系统赋予初始值
//数字类型：初始值为0.0
//布尔类型：初始值为 false
//字符串类型：初始值为""
//引用类型：初始值为 null
```

java 程序运行时需要在内存中分配空间，为了提高效率，内存空间也被划分为不同的部分。

## 栈内存

定义的方法中的变量放在栈内存中，使用完直接消失。

如`int a`，以及上文中定义的数组名 arr（其值是指向堆内存中数组内容的地址）。

## 堆内存

实体、对象等的定义放在堆内存中，使用完会在垃圾回收器空闲时进行回收。

如 new 一个对象，以及上文中数组 arr 中的具体数据内容（arr[0]=1, arr[1]=2……）

访问数组中的内容，首先根据栈内存中数组的地址找到相应的堆内存中的位置（以及移动相应的索引步长）然后访问数据。

因此，当多个数组指向相同地址时，其中的内容是一样的，修改其中一个，另一个也会改变。

```java
int[] arr1={1,2,3};
int[] arr2=arr1;
arr2[0]=11;
arr2[1]=22;
arr2[2]=33;//这时访问 arr1[]，发现其中的数据也变成了11,22,33
```

## 数组常见问题

1. 数组越界问题，ArrayIndexOutOfException。
2. 空指针异常问题，NullPointerException。（`arr=null`，表示数组不指向任何有效对象）

## Array.length

数组自带属性 length，通过`arr.length`就能获得数组长度。

# 方法

java 中的方法类似 C 中的函数。只是涉及类和对象的问题，有一些小不同。

像函数一样，是有独立功能的代码块组成的集合，可以拿去调用。

```java
public static 返回值类型 方法名(形参){//和 main 方法同级
    方法体
    return 返回值;
}//定义

方法名(实参);//在 main 方法中调用。有返回值类型的方法建议用变量接收调用
```

方法不能嵌套定义。

## 方法重载

多个方法在一个类中，有相同的方法名，但参数不完全相同。

```java
public static int max(int a, int b)
public static int max(int a, int b, int c)
```

**注意：返回值不能作为判断方法是否重载的标准！**只有方法名和参数可以。

调用时，JVM 根据传入参数不同，来得知调用的是哪个方法。

**形参值修改不会对实参造成影响。**main() 方法存储在栈内存中，当 main() 方法调用其它方法时，其他方法进入栈内存

![A8wxg.png](https://img-blog.csdnimg.cn/img_convert/7c3113349748d453f61377bc4c5caa29.png)

但是其中的形参的值不对 main() 中的实参造成影响（除非是像数组、指针之类引用类型，根据地址去堆内存中修改数据），当 change() 方法执行完后就直接出栈了。

![A8Kh1.png](https://img-blog.csdnimg.cn/img_convert/0c3e37209c8e3ab328096a6e480a614e.png)

如图，如果是数组单元的值被修改了，实际上是堆内存中的内容被修改了， main() 方法中数组对应的地址中的内容也会被修改。