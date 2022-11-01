# 导包

```python
import random
x=random.randint(0,2)#包含0,1,2
```

# 函数

```python
def 函数名():# 括号内也可以有参数
    函数体
    return x,y# 返回值可以是多个
x,y=函数名()# 调用
```

# 全局、局部变量

```python
a = 100
def function():
    a = 200# 局部变量，和原来的a地址不同。不会互相影响
    b = 200# 局部变量，作用域只在函数内，在外面使用b会报错
def function1():
    global a# 但如果这样声明了，就是使用的全局变量。更改的话全局范围内的a值也会变。
```

# 文件操作

```python
f = open('1.txt','w')# w模式下，如果同级目录中没有找到对应文件，就会新建一个。一执行，同级目录下立刻就会新建1.txt
# 若非w模式下没找到对应文件，会报错
f.write("hello world!")# 往该文件中写入内容
f.close()# 关闭文件

f1 = open('1.txt','r')# r模式代表只读文件，不存在该文件则报错。
content1 = f1.read(5)# 读取前五个字符
content2 = f1.read(5)# 读取6~10个字符
contentline=f1.readlines()# 读取文件中所有行，每行作为列表 contentline 中的一项（字符串元素），并以\n结尾
```

w：写入，已存在该文件则覆盖，不存在则新建。指针一开始在文件开头，逐渐后移。

r：读取。指针一开始在文件开头，逐渐后移。

rb：二进制方式打开文件（只读）

wb：二进制方式写入文件

重命名文件、删除文件

```python
import os# 重命名和删除文件都要引入 os 模块
os.rename('1.txt','2.txt')# 重命名
os.remove('2.txt')# 删除文件
```

# 异常处理

就像 java 中，异常会中断程序，可以提前处理。

比如：

```python
try:
    f.open('123.txt','r')# 不存在该文件
except IOError:# 文件没找到属于 IOError 输入输出异常
    pass# 不做任何处理

try:
    print(name)#没有定义的变量
except NameError:
    print("name不存在！")

# 同时捕获多个异常：except (IOError, NameError): 如果发生了其中的一个异常，try 内代码就会停止运行并处理
# 其实所有的异常都可以用一个 except Exception 捕获。

# 获取错误描述：
except Xxx as result:
    print(result)
    
# try...finally finally 中的内容是必须执行的
import time
try:
    f.open('123.txt','r')# f在 try 中定义的，是局部变量。因此如果 finally 要关闭f的话，应该写在 try 内
    try:
        while True:
            content = f.readline()
            if(len(content)==0):
                break
            time.sleep(2)# 打印行与行之间间隔2s
            print(content)
    finally:
        f.close()# 强制关闭，一定执行
        print("文件关闭")
except Exception:
    pass
finally:
    pass# 如果打开文件失败了，就说明没有这个文件，自然也不用关闭这个文件了
```

