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

多行字符串：'''三个单引号后输入的内容，如果包含回车，就会把回车内容记录在字符串内。

字符串拼接：直接做加法。

字符串外部连接：

```python
name = "Jingqing"
msg="%s" % (name)
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

## 列表

相当于数组

```python
name_list=["Jingqing","Fuwu","Fugu"]
print(name_list[1])
```

增：name_list.append(x) （在结尾插入） insert(i,x)（在第 i 位插入）

删：del name_list 或del name_list[i] 或列表名.remove(x)

改：name_list[i]="要修改的字符串"

查：name_list.index(x) 根据内容查索引号

切片：第 i 到 j 位：name_list[i:j+1]，如果有一边省略，就是取到头。比如 name_list[0:] 就是取所有数据。

（切片也可以倒着切。如最后两位：name_list[-2:]）

步长：name_list[i:j:2]，最后一位表示每几个数切一次，默认是1，也就是每个数都取。2就是隔一个取一个。

嵌套：列表里面的数据是个列表。name_list [0] [1] 访问嵌套的列表。

## 常用运算符

+、-、*、/（带小数）、//（整除）、**（幂）、%。

<、>、<=、>=、==、!=、<>（在 python 3中取消了）。

+=、-=、*=、/=、%=、//=、**=。

and、or、not。（and 的优先级高于 or）

in、not in（查找字符串中有无给定子串 / 列表中有无给定成员，子串 in / not in 总数据）（in 不能测试数字类型）

# 输入

name=input("请输入姓名：")

input().strip()：去掉输入时两侧的空格

input() 接收的都是字符串形式，不能直接拿去做运算。可以通过 int(input()) 强制转换。

```python
if age.isdigit():
    age=int(age)
else:
    print("输入类型不是数字")
```

# 条件控制

```python
# 单分支
if 条件: # 顶级代码
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

