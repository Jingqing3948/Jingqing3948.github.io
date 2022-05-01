---
title: Java 学习博客_8 入门——继承
date: 2022-05-01
tags: study
category: java
---

以黑马程序员全套Java教程网课为主。



有一些类有许多相同属性和方法（如猫和狗，有动物的共同特征）重复写两遍相对麻烦一些。可以把这些类的共同部分提取出来作为父类，这些类继承父类中的共有部分后再添加自己特有的内容。

```java
public class Animal{//父类，基类，超类
    String name;
	int age;
	boolean tail;
	String color;
}

public class Cat extends Animal{//子类，派生类
    public void meow(){
        System.out.println("Meow!");
    }
}

public class Dog extends Animal{
    public void bark(){
        System.out.println("Bark!");
    }
}
```

提高了代码的复用性，但是同时也提高了耦合性，如果父类修改子类也要跟着修改。

当子类属于父类的一种时，可以使用继承。

## 变量访问特点

范围越小，权限越高。

访问优先级：子类中局部变量>子类中成员变量>父类中成员变量>找不到就报错

如果想访问父类中的元素：super 关键字（使用方法类似 this，只是 this 是访问本类中的成员， super 是访问父类中的成员）

![AmPt7.png](https://s1.328888.xyz/2022/05/01/AmPt7.png)

## 构造方法的访问特点

即使访问子类的构造方法，也会优先访问父类的无参构造方法，再访问子类的构造方法。因为毕竟子类由父类而来，需要先构造父类再构造子类。

而在之前介绍过，如果在类中只定义了带参构造方法，就会覆盖系统默认给的无参构造方法，这时类中就没有无参构造方法了。这时再尝试使用类中的无参构造方法，就会报错。

因此，如果在父类中只定义了带参构造方法，子类使用构造方法就会报错（因为先尝试使用父类无参构造方法却发现没有）

解决方法：1. 父类中补上无参构造方法

2. 在子类构造方法中手动访问父类的带参构造方法来代替

```java
public Fu(int age)
{
    this.age=age;
}

public Zi()
{
	super(20);
}
public Zi(int age)
{
    super(age);//子类的带参构造方法就这样传递给父类即可
}
//不支持一个子类继承多个父类，但是允许多层基层（爷->父->子）
```

## 成员方法的访问特点

先在子类中寻找，找不到再去父类中寻找。

## 权限修饰符

public > protected > 默认 > private

![Awiam.png](https://s1.328888.xyz/2022/05/01/Awiam.png)

## 状态修饰符

有 final 和 static。

### final

成员变量和成员方法被 final 修饰后，不可以被重新赋值 / 重写。

类被 final 修饰后，也不可以作为父类。

*引用类型被 final 修饰后，不可以被重新赋值，指的是地址不能改变，指向的内容还是可以变的。（除了字符串类型，字符串类型内容不可以改变）*

### static

被 static 修饰的成员变量在所有对象里值都是一样的。

```java
public static String university;//在学生类中的静态成员变量

Student.unievrsity="北大";//统一赋值
Strudent s1=new Student();//university 变量的值是 "北大"
Strudent s2=new Student();//university 变量的值是 "北大"
```

**静态成员方法只能访问静态成员变量 / 方法！**

## 包 Package

文件夹，用于存放编译后生成的 class 文件，方便管理。

在 IDEA 中新建的 java 文件会自动生成包路径。如果要在 cmd 窗口中运行带有包路径的文件，编译照常，执行需要注明 class 文件的路径。

```java
package com.ithema;

//命令行根据路径建包
javac -d . HelloWorld.java

//编译
javac HelloWorld.java

//运行
java com.ithema.HelloWorld
```

## 导包 import

如果测试类在 src 文件夹的A文件夹下，要调用的类在 src 文件夹的B文件夹下，可以通过导包的方式来调用

`import B文件夹.要调用的类名;`

## 方法重写

子类中重新定义了父类中的方法。

子类如果想访问父类中的方法，可以通过 super 来指定。

```java
@Override//注解，可以检查方法重写的正确性
public void function()
{
    System.out.println("子类中重写方法");
}
```

1. 父类的方法如果是 private 的，子类中就访问不到，也构不成方法重写一说了。
2. 子类的重写方法权限必须高于父类方法。（public > protected > 默认 > private）
