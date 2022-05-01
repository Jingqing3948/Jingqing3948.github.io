---
title: Java 学习博客_7 入门——集合类
date: 2022-04-30
tags: study
category: java
---

以黑马程序员全套Java教程网课为主。



集合和数组相比，就像 StringBuilder 之于 String，数组长度固定，集合长度可变。

ArrayList 就是集合的一种。

# ArrayList\<E\>

在 java.util 包下，需要导包。

\<E\> 表示泛型，随便加一种数据类型。

```java
ArrayList<String> array=new ArrayList<String>();
```

| 方法                        | 说明                                   |
| --------------------------- | -------------------------------------- |
| ArrayList\<E\>()            | 无参构造方法                           |
| boolean add(E e)            | 结尾添加一个元素，成功返回true         |
| void add(index i, E e)      | 在指定索引处添加一个元素（不能越界！） |
| boolean remove(Object o)    | 删除指定对象，成功返回 true            |
| E remove(int index)         | 删除指定索引处的值，返回该值           |
| E set(int index, E element) | 修改指定索引处值，返回修改后的值       |
| E get(int index)            | 返回指定索引处元素                     |
| int size()                  | 返回集合元素个数                       |

```java
array.add(1);
array.add(3);
array.add(4);
array.add(1,2);
System.out.println(array);//输出 array：1，2，3，4
```

