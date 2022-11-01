---
title: Python——语法基础1
date: 2022-04-28
tags: study
category: Python
---

今天开始跟着 Alex老师 [Python开发96天0基础到大神(2021最新，冲刺全网最佳教程)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Z64y1S72C?p=1)的课程学习Python~

关于编译器解释器的部分、Python 安装及 Path 配置就不再赘述了。从计算机三大件的部分开始学习。

# 计算机三大件

## 硬盘

数据的永久存储。

## 内存

数据的临时存储。处理数据速度远快于硬盘，所以才有存在的意义。处理数据应该尽量避免对硬盘的直接读写。

## CPU

进行运算。计算数据来自于内存。

计算时，数据从硬盘传到内存，再传给 CPU 进行运算。

保存数据后，数据再保存到硬盘。

# 变量

先定义，后使用。

定义规则和 C 一样，字母数字下划线，数字不能开头，不能是关键字。

可以是中文名，但很 low~ 尽量不要用。

```python
name = "Jingqing"
name = "Rename" #直接重新赋值，地址不变
```

可以通过 id(变量名) 方法获取变量地址。

# 数据类型

| 类型               | 含义     |
| ------------------ | -------- |
| int / long / float | 数字类型 |
| str                | 字符串   |
| list               | 列表     |
| bool               | 布尔     |
| set                | 集合     |
| dict               | 字典     |
| bytes              | 字节     |

## 数字类型

python 3之后没有 long 类型了，全是 int.

字符串类型不可以修改。如果已经把字符串类型赋给了一个变量，再重新赋值成其他字符串值，会发现地址变了，说明没法对原来的字符串做修改。（`name[2]='A'`无效）

## 字符串

字符串像数组一样，可以依照从0开始的索引访问。也可以切片访问`name[2:4]`，取得[2]~[3]的字符串。

多行字符串：'''三个单引号/双引号后输入的内容，如果包含回车，也会把回车内容记录在字符串内。

*有一种注释方法就是三个单引号/双引号之间的内容，严格来说这算不上注释，只是一个没有做任何处理的字符串*

可以在字符串中使用转义字符\，这在爬虫中很重要，因为爬到的内容可能有一些会引发问题的字符。

字符串拼接：直接做加法或乘法。

字符串截取：

```python
name="JingQing"
print(name)# JingQing
print(name[0])# J
print(name[0:5])# 从第0位开始，5是结束位置（不包括第5位）。即 JingQ
print(name[0:5:2])# 类似 for 循环，起始位置、终止位置、补进值，输出 J n Q
print(name[1:])# 从第1位到最后一位
print(name[:5])# 从第0位显示到第4位
```



字符串外部连接：

```python
name = "Jingqing"
msg="%s"%name
#多个占位："%s%s"%(name,age)
```

%s：字符串占位符

%d：数字占位符

%f：浮点占位符

字符串中有几个%s，结尾的括号中就应该有几个变量。

python 3 之后有另一种处理方法：

```python
msg=f"{name} is me."
```

在字符串开头输入 f，会把字符串中的大括号内容都替换成变量。

在字符串开头输入r，会显示原始字符串，不转义，如\n就会输出\n字符。**爬虫常用**

## 列表

相当于数组，是 python 中的核心类型。

列表中的数据类型可以混合。

```python
list=[]# 空列表
name_list=["Jingqing","Fuwu","Fugu"]
print(name_list[1])
```

增：name_list.append(x) （在结尾插入） insert(i,x)（在第 i 位插入）

删：del name_list 或`del name_list[i]`删除制定下标的元素；或`列表名.remove(x)`删除指定元素（重复的元素只会删除第一个指定元素）；或`列表名.pop()`弹出末尾的元素，即出栈。

改：直接`name_list[i]="要修改的字符串"`

查：

​	`list[index]`根据下标查找第几个元素。-1代表最后一个元素。

​	`name_list.index("x")` 根据内容查索引号；

​	`name_list.index("x",l,r)`在 l~r 范围内查找是否存在x元素，返回指定数据的下标，找不到会报错；

​	`name_list.count("x")`看看列表中有几个x元素；

​	`if(x in list)` in 和 not in 判断给定元素是否在列表中。

切片：和字符串截取一样，可以有开始、结束位置，以及步进。

嵌套：列表里面的数据是个列表。形如：[1,2,[3,4]]。（`list1.append(list2)`这种拼接方式就会造成列表的嵌套）name_list [0] [1] 访问嵌套的列表。

列表拼接：`list1.extend(list2)`，将 list2 列表中的所有元素逐一添加到 list1 中。

反转：`name_list.reverse()`。

排序：`name_list.sort()`升序，`name_list.sort(reverse=True)`降序。

## 元组

相比于 list，tuple 里的元素不能修改。但是里面可以包含可以改变的对象。如 tuple 中的 list 中的内容可以改变。

tuple 用小括号 ()。定义哪怕只有一个元素的元组也要加逗号`tuple=(1, )`。因为如果写成`(1)`，这里的括号会被解析成一个表达式，就不是元组了。为了判断是否是元组，一个元素的时候结尾必须加逗号。

增（连接）：

```python
tup1=(1,2,3)
tup2=(4,5,6)
tup=tup1+tup2# (1,2,3,4,5,6)
```

删：`del tup`删除整个元组变量。没法删除单个元组项。

改：元组不能改，不能直接`tuple[1]="abc"`直接修改。

查：`tup[1]`直接访问。也可以使用 in 和 count 方法。

## 字典

字典是无序的对象集合，使用键值对存储。用大括号{}.

```python
d={'name':"XiaoMing",'age':18,'sex':"male"}# 键必须唯一，而且必须使用不可变的类型
print(d['name'])
# 直接访问不存在的键会报错。可以使用 get()
print(d.get("school"))# 没找到会返回 None，不报错
print(d.get("school","没找到"))# 可以设定没找到的默认值为：没找到
```

增：`d["id"]=newId`

删：`del d["name"]`删除一项，`del d`删除整个字典变量

​	`d.clear()`清空内容

改：直接通过键访问值。键不可变，值可变。

查：`d.keys()`是所有键的列表，`d.values()`是所有值的列表，`d.items()`是拿出每一对键值对。

```python
for key, value in d.items():# for 循环每次可以遍历多个变量。相当于每个列表中第一个元素是键，第二个元素是值
    print(key, value)
```

## 集合

set，里面只有 key 而没有 value，无序且不能重复，出现重复内容会自动去重。

## 常用运算符

+、-、*、/（带小数）、//（整除）、**（幂）、%。

<、>、<=、>=、==、!=、<>（在 python 3中取消了）。

+=、-=、*=、/=、%=、//=、**=。

and、or、not。（and 的优先级高于 or）

in、not in（查找字符串中有无给定子串 / 列表中有无给定成员，子串 in / not in 总数据）（in 不能测试数字类型）

# 输入

name=input("请输入姓名：")

input().strip()：去掉输入时两侧的空格

**input() 接收的都是字符串形式**，不能直接拿去做运算。可以通过 int(input()) 强制转换。

```python
if age.isdigit():
    age=int(age)
else:
    print("输入类型不是数字")
```

# 条件控制

```python
# 单分支
if 条件: # 顶级代码。非零或非空的值都是 true
    表达式 # 必须缩进，属于子级代码
    表达式 # 同一级代码缩进必须一致，尽量都是 Tab
# 双分支
if 条件:
    表达式
else:
    表达式
# 多分支
if 条件:
    表达式
elif 条件:
    表达式
elif 条件:
    表达式
else:
    表达式
```

这里很离谱的地方是：表达式必须缩进（大概是因为不像 C 和 Java 有大括号，可以确定范围）

**if 可以嵌套，最好不要超过4层。**

# 循环

```python
for i in range(10) # 输出0~9
	print(i)
for i in range(100,50,-1) # 从100输出到50，每次-1，左闭右开
	print(i)
for i in ["hello","world"] # 输出hello\nworld
	print(i)
for i in "helloworld" # 输出h\ne\nl\nl\no\nw\no\nr\nl\nd
	print(i)
for i in range(len(arr)):# 这时的i不是数组的每一项，而是索引
    print(i,arr[i])
for i, x in enumerate(list):# 自动给列表中每个元素一个下标，从0开始
    print(i,x)
```

print() 默认参数：`print(end:"\n")`，表示每次print输出后都会换行。如果把 end 的实参修改，就不会换行。

for 输出的内容必须是一个集合，比如字符串，列表。数字不行。

i 的作用域只在 for 内。

for 循环体内的内容和 if 一样，需要缩进。

**for 可以嵌套，最好不要超过4层。**

break、continue、exit()（退出整个程序）

输出时可以通过字符串*数字来输出指定次数。

```python
print("*"*3) # 输出3次 *
```

for...else... 当循环正常结束时执行 else 后面的内容。即没有中途 break 或 exit()

```python
for i in range(1,10):
    print(i)
    if(i==5)
    	break # 不会输出 else 里的内容
else
	print("循环正常结束，i=10")
```

例如，判断一个数字是否为素数，正常做法就是先定义一个 flag 变量，然后用小于该数的所有数整除它，看余数是否为0。如果出现了余数为0，就令 flag =1.最后循环结束时如果 flag==0，说明该数为素数。

用 for else 或 while else 就可以判断循环是正常结束的还是中途跳出的。

```python
for i in range(2,n-1):
    if(n%i==0)
    	print("n不是素数")
    	break
else
	print("n是素数")
```

循环中还有一个语句：pass，意为什么都不做，占位语句。

# 输出

```python
# print()中 sep 参数指的是输出的几个项之间的分隔符，默认为空格；end 参数指的是输出的最后一项结尾加的东西，默认\n
print("aaa","bbb","ccc")# 输出 aaa bbb ccc\n
print("aaa","bbb","ccc",sep="")# 输出 aaabbbccc\n
print("aaa",end="123")# 输出 aaa123
```

