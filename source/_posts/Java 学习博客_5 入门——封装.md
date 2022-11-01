---
title: Java 学习博客_5 入门——封装
date: 2022-04-30
tags: study
category: java
---

以黑马程序员全套Java教程网课为主。

Java 的三大特性：封装、继承、多态

# 封装 packaging

之前定义的成员变量都可以直接对值进行修改，存在安全隐患（比如设置 age=-30）

因此我们要添加一些限制。

## private 修饰符

可以修饰成员变量、成员方法不被其他类使用。

```java
private String name;
private int age;
```

被 private 修饰的成员变量有两种赋值（访问）方式：构造方法和 get / set 方法。

## 构造方法

写在类中，作为类的方法。主要用于对象初始化。声明变量时`Cat c=new Cat();`Cat() 就是一种无参构造方法。

每个类定义时系统都会给一个默认无参构造方法。如果自己给了一个无参构造方法，就会覆盖系统默认的。**建议无论是否用到构造方法，都写一个无参构造方法。**

```java
public Cat(){}//我们自己加的无参构造方法，会覆盖系统默认的
public Cat(String name){//写一部分参数的构造方法也可以
    this.name=name;//通过 this 赋给成员变量
}
public Cat(String name, int age){//写全参数的构造方法也可以
    this.name=name;
    this.age=age;
}
//main 中构造对象：
Cat c1=new Cat();
Cat c2=new Cat("小黑");
Cat c3=new Cat("小白",2);//这些都可以，与自己写的构造方法的参数相对应。
```

this被哪个对象调用，就代表哪个对象。

![AlrRQ.png](https://img-blog.csdnimg.cn/img_convert/e84298f42ee41e72ca8573855a60fb93.png)

然后把"林青霞" 字符串类型传入堆内存中。

## get / set

无参构造方法后用 setXxx() 方法创建对象。

```java
public void setName(String name){//赋值
	this.name=name;
}
public void setAge(int age){
    if(age>=0&&age<=20)//在 set 中可以添加一些限制，处理
	this.age=age;
}

public void getName(){//获取值
	return name;
}
public void getAge(){
	return age;
}

//在 main 方法中赋值并获取值示例：
Cat c=new Cat();
c.setName("小黑");
System.out.print(c.getName);
```

## 总结

封装将类的某些信息隐藏在类内部，不允许外部程序直接访问，而是通过该类提供的方法实现对隐藏信息的操作和访问，提高了代码安全性（在 setXxx() 方法中可以对数据进行校验）和代码复用性（封装方法可以复用）。
