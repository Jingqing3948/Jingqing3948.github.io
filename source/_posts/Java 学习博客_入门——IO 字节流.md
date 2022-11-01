---
title: Java 学习博客_入门 IO 字节流
date: 2022-05-23
tags: study
category: java
---

# File 类

在 java.io 包下。

File 是文件和目录（文件夹）的路径名的抽象表示。

## 绝对路径和相对路径

绝对路径是完整的路径名，不需要其他信息就能定位其所表示的文件。如`D:\\文件夹\\java.txt`。

相对路径：必须使用取自其他路径名的信息进行解释。如从源文件目录下开始写：`myFile\\java.txt`。

## 构造方法

| 构造方法                          | 说明                                             |
| --------------------------------- | ------------------------------------------------ |
| File(String pathname)             | 通过将给定的路径名字符串转换为抽象路径名创建对象 |
| File(String parent, String child) | 父子两个路径名字符串                             |
| File(File parent, String child)   |                                                  |

路径形如：`D:\\文件夹\\java.txt`。也可以拆分成父`D:\\文件夹`和子`java.txt`。也可以第一个 File 对象的路径是`D:\\文件夹`，子 File 补充`java.txt`。

如果直接输出 File 类型，会得到路径。说明 File 类重写了 toString 方法。

## 创建功能

| 方法                           | 说明                                   |
| ------------------------------ | -------------------------------------- |
| public boolean createNewFile() | 若此文件名的文件不存在，则创建一个空的 |
| public boolean mkdir()         | 创建目录                               |
| public boolean mkdirs()        | 创建目录，包括任何必须的父目录         |

创建成功返回 true，创建失败（如已存在）返回 false。

如果调错方法了，可能会创建一个名为 java.txt 的文件夹！一定要正确使用创建文件的方法。

## 创建获取功能

| 方法                            | 说明                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| public boolean isDirectory()    | 是否是个目录                                                 |
| public boolean isFile()         | 是否是个文件                                                 |
| public boolean exists()         | 是否存在                                                     |
| public String getAbsolutePath() | 获取其绝对路径                                               |
| public String getPath()         | 获取其相对路径                                               |
| public String getName()         | 获取该文件或目录名                                           |
| public String[] list()          | 返回此抽象路径名表示的**目录**中的所有文件和目录的名称字符串数组 |
| public File[] listFiles()       | 返回此抽象路径名表示的**目录**中的文件和目录的 File 对象数组 |

## 删除功能

public boolean delete()

如果目录中有内容，不能直接删目录。要先删空内容。

## 递归

```java
public static int f(int n){
    if(n==1||n==2)return 1;
    else return f(n-1)+f(n-2);//斐波那契数列
}
```

如果未能跳出递归，方法不断压入堆栈空间，导致内存溢出。

# IO 流

IO 表示输入输出，流是对数据传输的统称。

常见应用：文件复制、上传、下载。

分为字节、字符两种数据类型的输入、输出流。（如果通过记事本打开，你看的懂内容，就是字符流；如果一串乱码，就是字节流）

## 字节流

java.io.InputStream，抽象类，是所有输入字节流的类的超类。

java.io.OutputStream，抽象类，是所有输出字节流的类的超类。

子类名称都是以父类作为后缀。

### FileOutputStream

数据写入 Stream。

**使用时需要抛出异常，main() throws FileNotFoundException。**

1. 创建字节输出流对象

   FileOutputStream(String name) 创建文件输出流，以指定的名称写入文件。name 是路径。如果没有该文件，会创建一个。

   1. 检查有无该文件，若没有自动创建；
   2. 创建 FileOutputStream 对象；
   3. FileOutputStream 对象指向该文件。

2. 调用写数据方法

   写入： void write(int b)。如b=97，就会写入一个字母a。

   void write(byte[] b) 将 b.length 字节从指定的字节数组中写入此文件输入流。（可以通过 String.getBytes() 方法获取字符串对应的字节数组）

   void write(byte[] b, int off, int len) 将从指定偏移量开始处指定长度的字节写入。

3. 释放资源

   **使用完后要 close()！**关闭该文件输出流并释放相关系统资源。



*1. 想添加换行内容就在字符串里加 \n 即可。但是 windows 中的换行是 \r\n，Linux 中的是 \n，Mac 中的是 \r.而 idea 直接打开文本文档，则这几种类型的换行都能识别。*

*2. 这种写法每次运行，不是在文档尾追加写入，而是覆盖。想要追加写入，就在创建 FileOutputStream 对象时加一个 boolean 类型的 append 的 true 值：FileOutputStream(String s, boolean append)，就表示追加写入。*



#### 异常处理

异常类： IOException

finally: 在异常处理时提供 finally 执行所有清除操作。其中的内容一定会执行，除非虚拟机退出。

```java
FileOutputStream f=null;
try{
    
}
catch(){
    
}
finally{
    //执行清除操作
    if(f!=null)f.close();//这里可能还会生成一个 try catch
}
```

## FileInputStream

1. 创建字节输入流对象

   构造方法： FileInputStream(File 或 String)

   1. 检查有无该文件，若没有自动创建；
   2. 创建 FileOutputStream 对象；
   3. FileOutputStream 对象指向该文件。

2. 调用读数据方法

   int read() 读取第一个数据。如果到达文档末尾，返回-1.

   ```java
   while((by=f.read())!=-1){
       System.out.println((char)by);
   }//读取文件中所有内容，包括换行符
   ```

   int read(byte[] b)返回读入缓冲区的字节总数。如果到达文档末尾，返回-1.

   ```java
   byte[] bys=new byte[1024];//一次读取一个字节数组
   int len;
   while((len=fis.read(bys))!=-1){
       System.out.print(new String(bys,0,len));
   }
   ```

   

3. 释放资源

#### 复制文本文档

写一个循环，读取复制文件中每个字节的同时，将每个字节写入粘贴文件中。

### 复制图像

复制图像：建议一次读取/写入一个字节数组。

```java
FileInputStream fis=new FileInputStream("要复制的图片路径\\文件名");
FileOutputStream fos=new FileOutputStream("要粘贴的文件路径\\文件名");
byte[] bys=new byte[1024];
int len;
while((len=fis.read(bys))!=-1)
{
    fos.write(bys);
}
fis.close();
fos.close();
```

## 字节缓冲流

java.io.BufferedOutputStream

之前 FileOutputStream 每次写入都会导致底层系统的调用，而字节缓冲流可以直接向底层输出流写入字节。实现时间更短。

```java
BufferedOutputStream bos=new BufferedOutputStream(new FileOutputStream("路径"));
//读写数据方法和之前一样。但是还得靠 File
bos.write("String".getBytes());
//记得释放资源
bos.close();

BufferedInputStream bis=new BufferedInputStream(new FileInputStream("路径"));
int len;
while((len=bis.read(bys)!=-1)){
    System.out.print(new String(bys,0,len));
}
```



