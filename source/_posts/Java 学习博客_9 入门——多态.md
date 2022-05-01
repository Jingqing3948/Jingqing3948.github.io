---
title: Java 学习博客_9 入门——多态
date: 2022-05-01
tags: study
category: java
---

以黑马程序员全套Java教程网课为主。



同一个对象在不同时刻表现出的形态也不同。

```java
Cat c=new Cat();
Animal a=new Cat();//父类引用指向子类对象
```

a 在内存中是 Cat 的大小，但是编译时被视作 Animal 类型。

## 多态中成员的访问特点

访问成员变量时：优先看等式左侧。如 Animal 和 Cat 中都对 age 赋值了，访问 age 得到的是 Animal 中的 age。*因为成员变量不会被重写*

访问成员方法时：优先看等式右侧。如 Cat 中重写了 eat 方法，使用 eat 方法就是使用 Cat 中的 eat 方法。

例：Animal 父类，其中含有 eat() 成员方法；Cat Dog Pig 子类，其中都重写了 eat() 成员方法。

```java
public class AnimalOperator(Animal a)
{
    a.eat();
}

//使用
Cat c=new Cat();
Dog d=new Dog();
Pig p=new Pig();
AnimalOperator(c);//多态
AnimalOperator(d);//多态
AnimalOperator(p);//多态
```

调用的都是子类中重写的方法。

多态提高了程序的拓展性。对于子类重写父类中的方法，我们只需要一个操作类就能访问（无论子类具体是哪一种）。

但是多态无法访问子类中特有的方法。

解决这一弊端的方法就是转型。

```java
Animal a=new Cat();//向上转型，a 不能访问 Cat 类中特有的方法
Cat c=(Cat)a;//向下转型，这样 c 就可以访问 Cat 类中特有的方法了。
```

使用转型的时候注意不能随便向下转型。如本例中 a 在内存中是 Cat 类，不能转换成 Dog 类。*ClassCastException*
