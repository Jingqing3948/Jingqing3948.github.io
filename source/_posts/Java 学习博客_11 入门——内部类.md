---
title: Java 学习博客_11 入门——内部类
date: 2022-5-5
tags: study
category: java
---



以黑马程序员全套Java教程网课为主。

在类中定义一个类。

# 内部类和外部类的互相使用

无论外部类的成员变量的权限修饰符是什么（private, public……）内部类都可以直接访问外部类的全部成员变量。

而外部类如果想访问内部类的成员，需要先创建一个内部类对象再访问。

*因为都在同一个类中，所以即使权限修饰符是 private 也可以访问。*

# 外界使用内部类

外界使用内部类：`Outer.Inner oi=new Outer().new Inner();`

如果 inner 是私有的，或者是局部内部类，则不能通过上述方法调用。但是可以在 outer 里定义一个公开的方法，在方法中 new 一个 inner 并使用其中的成员方法。对于外部的调用者来说，他只需调用 outer 的这个方法，就可以访问 inner 的成员方法。不过对使用者来说他并不知道内部结构如此。

# 内部类的分类

在类的成员位置定义：成员内部类

在类的局部定义：局部内部类。

## 局部内部类

不能从外界直接创建对象，只能通过调用方法来创建类

```java
public class Outer {
	private int num=10;
    public void method() {
        class Inner {
			public void show(){
                System.out.println(num);//局部内部类可以访问外部类的成员变量，或 method 方法中的局部变量
            }
        }
        Inner i=new Inner();//在方法中创建内部对象
        i.show();//创建对象之后，记得调用内部方法的成员变量
    }
}
```

## 匿名内部类

继承了另一个类，或实现了接口的子类匿名对象。

首先有一个父类/接口，其中有具体的/抽象的方法，然后匿名内部类对其进行重写/实现。

```java
//接口文件 inter.java 中：
public interface inter {
    public void show();//抽象方法
}

//外部类文件 Outer.java 中
public class Outer {
	private int num=10;
    public void method() {
        new inter {
            @Override
            public void show(){
                System.out.println("匿名内部类");
            }
        }.show();//创建匿名对象之后，记得调用成员变量
        
        //另一种方法：
        inter i=new inter {
            @Override
            public void show(){
                System.out.println("匿名内部类");
            }
        }
        i.show();
    }
}
```

使用示例：假如我们有一个 jump 接口，有一个 jumpoperator 类，其中包含 method(Jump j) 执行j的 jump 。

我们要让不同动物都能 jump，就要在不同动物类中 implements jump 接口，并重写其中的抽象方法；

然后用堕胎的方式`jump c=new cat(); jump d=new dog();` 新建对象并传给 method 方法。

但是可以通过匿名内部类的方法：

```java
jo.method(new jump(){
    @Override
    public void jump()
    {
        System.out.println("猫跳高");
    }
});

jo.method(new jump(){
    @Override
    public void jump()
    {
        System.out.println("狗跳高");
    }
});
```

直接穿一个匿名内部类参数给 method 方法，就不用新建文件重新定义类了。