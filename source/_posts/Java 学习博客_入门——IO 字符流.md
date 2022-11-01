---
title: Java 学习博客_入门 IO 字符流
date: 2022-05-23
tags: study
category: java
---

如果按照之前读取字节流的方法，我们要得到输出内容时需要用 (char) 强制转换。

如果内容中包含中文，就会输出乱码。因为汉字存储在 GBK 编码下占用2 byte，UTF-8 编码下占用3 byte。所以每次读写一个字节，就会把字节拆开，就输出不了汉字了。

字符流就会方便很多。

# 字符流

字符流=字节流+编码表。

汉字存储时，不管采用哪种编码方法，第一个字节都是负数，以此判断。

## 编码表

计算机中的信息都是二进制数表示的。

按照一定规则把字符存储到计算机中，叫编码。

按照一定规则把字符从计算机中解析，叫解码。

## 字符集

一个系统支持的所有字符集合。如 ASCII GBKXXX Unicode 等。

**ASCII码** —ASCII编码表由一个字节表示，128个字符，包含字母、数字、常用符号等。其拓展集128~255方便支持欧洲常用字符。

**GBXXX**—一个小于127的字符意义与原来相同（半角字符）但两个大于127的字符相连，表示一个汉字。甚至一些数学符号、罗马希腊字母、日文假名、原来的数字、标点、字母都被重新编了二字节的编码，即全角字符。

其中 GBK 是最常用的中文码表，还支持繁体、日韩汉字。

*GB18030：最新的中文码表，每个字可以由1、2、4个字节组成，支持少数名族文字。*

**Unicode**—表达任意语言的任意字符。最常见的就是 UTF-8 编码。前128和 ASCII 一样，1个字节；拉丁文字符2个字节；大部分常用字（包括中文）3个字节；其他辅助字符4个字节。

## 字符串的编码解码

编码：byte[] getBytes() 或 byte[] getBytes(String charsetName) 指定字符集。

解码：String(byte[] bytes) 或 String(byte[] bytes, String charsetName) 构造方法。

IDEA 平台默认字符集为 UTF-8。

在字符流中的解码、编码对应 InputStreamReader、OutputStreamWriter，

构造方法：InputStreamReader(FileInputStream, String charsetName) 

OutputStreamReader(FileOutputStream, String charsetName) 也可以指定字符集。然后就正常使用 read() write() 方法。

如果使用 UTF-8 编码就可以直接写入中文字符串。否则需要同样编码方法的读取方法才能正确读取。

| 写数据方法                               | 说明                     |
| ---------------------------------------- | ------------------------ |
| void write(int c)                        | 写一个字符               |
| void write(char cbuf)                    | 写入一个字符数组         |
| void write(char cbuf, int off, int len)  | 写入一个字符数组的一部分 |
| void write(String str)                   | 写入一个字符串           |
| void write(String str, int off, int len) | 写入一个字符串的一部分   |

这样写完，数据还是在缓冲区中。需要通过 void flush() 方法刷新。

不过 close() 方法关闭前也会自动刷新一下。只是关闭方法后就不能再写入数据了~

| 读数据方法            | 说明               |
| --------------------- | ------------------ |
| int read()            | 读一个字符         |
| int read(char[] cbuf) | 一次读一个字符数组 |

FileReader 和 FileWriter 是两个字符流解码、编码的便捷类。构造方法参数为路径字符串或 File 对象。

## 字符缓冲流

BufferedReader(Reader in)

BufferedWriter(Writer out)

两者自带成员方法：public String readLine() 读一行 和 void newLine() 写一行。

