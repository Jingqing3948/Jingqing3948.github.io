---
title: C 关于gets()和scanf()处理空白字符的问题
date: 2022-02-26
tags: problems
category: clanguage
---

## scanf()

scanf()如果读入字符、字符串，就要注意回车、空格、制表符（统称为空白符）的问题。
字符还比较好处理，比如要输入abc三个字符，中间就直接不加入任何空白符，直接连着输入”abc”即可。
scanf()使用%s读入字符串，读到空白符时会自动结束，所以不能读入英文句子(“hello world”中间有空格，一次只能读一个单词)
想要读入带有空格的句子，一种方法是用scanf读入一个个字符串（单词）。字符串以空白符结尾，且当空白符在开头时%s是不会读取空白符的，会跳过这里的空白符（和scanf读入整数或浮点数时类似）
```c
scanf("%s%s",str1,str2);//输入"hello world"
printf("%s%s\n",str1,str2);//输出"helloworld"
printf("%s %s\n",str1,str2);//输出"hello world"
```
只要补齐空格就可以了。当然这种方法用来读入输出英文句子时局限性很大，每个单词都要定义一个字符串变量（或许可以定义一个char的二维数组/指向char一维数组的指针/指向指针的指针），而且还要自己补空格。
另一个有趣的地方是scanf()里可以放空格，意思是跳过这里的空白符。
```c
scanf("%s %s",str1,str2);//输入"hello world"
printf("%s%s\n",str1,str2);//输出"helloworld"
```
如果还是上例，其实没有影响，因为就算scanf里没有这个空格，str2也会自动跳过开头的空格从后面的w开始读入。不过这种方法在读入字符的时候处理输入的空白符，以及下文中和gets()的结合中就比较有用。

## gets()

gets()就可以读入空白符，空格、回车、制表符都能读入，并且读入回车时结束。因此可以借助gets()读入英文短句式的字符串（带有空格）
```c
gets(str);//输入"hello world\n"
printf("%s",str);//输出"hello world"
```
但是要注意回车的问题。
当gets()读入的第一个数据是回车，则停止继续读入，并且str的内容就是回车。
当gets()读入"hello world\n",回车是不会被gets()读入的，还留在缓冲区。
所以连着使用两个gets()，而中间不想办法处理掉回车，就会出错。
```c
gets(str1);//abc\n
gets(str2);//abc\n
gets(str3);//abc\n
//输出的结果是：abc\nabc,str2读到的是回车。
```
此时可以用getchar()读掉中间的回车。
在做翁恺老师C语言程序设计的PTA习题时，碰到了这样一道题：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bf40d6f06ca4f999f51eefe9bea5cfd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2p0d3F3cQ==,size_16,color_FFFFFF,t_70)
要交替输入浮点数和带空格的字符串，一开始没有细想回车的问题：
```c
//循环执行：
scanf("%lf",book[i].price);//读入浮点数
gets(books[i].name);//读入字符串
```
后来经一番查询理解了两种函数对空白符的处理，也写了这篇博文。
这道题有趣的地方是scanf和gets的结合，因此除了getchar()读掉中间的回车的办法，还可以在scanf末尾加空格：
```c
//循环执行：
scanf("%lf ",book[i].price);//读入浮点数，并读掉结尾的空白符（回车）
gets(books[i].name);//读入字符串
```
非常巧妙。
以上是本人经试验、查询资料后得出的结论，也欢迎读者多多尝试，如有错误，还请读者雅正！