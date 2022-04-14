---
title: Java 学习博客_1 介绍与安装
date: 2022-04-14
tags: code
category: study
---

以黑马程序员网课为主。

## 介绍

Java是一个可以跨平台的语言，借助Java虚拟机（Java Virtual Machine, JVM）能够在任意操作系统（operating system, OS）上运行。

JVM: Java Virtual Machine，在 JRE 的 bin 目录下。JVM 本质上是一个程序，使得 Java 在不同平台上运行时不需要重新编译，只需要执行保存在某字节码文件（.class）中的指令，不管什么平台，只要装有相应平台的 JVM ，字节码文件就可以在该平台上运行。

JRE: Java Runtime Environment，运行只需要 JRE 就够了。

JDK: Java Development Kit，Java 程序开发工具包。

![123](https://s1.328888.xyz/2022/04/14/iUcNe.png)

尽管 JRE 对于运行 java 文件已经足够，作为学习肯定还是要下载 JDK 的~

JDK 的安装目录如下：

| JDK目录名称 | 说明                                |
| ----------- | ----------------------------------- |
| bin         | 存放工具命令，如：javac, java, etc. |
| conf        | 配置文件                            |
| include     | 某些平台特定的头文件                |
| jmods       | 模块                                |
| legal       | 授权文档                            |
| lib         | 补充 JAR 包                         |
| 其他        | 说明型文档                          |

## 在 DOS 窗口下操作命令

在一开始没有使用 IDEA 等集成开发环境的时候，直接在 DOS ( Disk Operating System ) 窗口运行。Windows 通过 `win+R` 打开运行窗口，输入 `cmd` 进入 DOS 窗口。

常用的DOS窗口命令：

| 操作                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `盘符名称`+`:`，如`D:` | 切换到该盘                                                   |
| `dir`                  | 查看当前路径下的内容                                         |
| `cd 目录`，如`cd java` | 进入当前路径下的一个单级目录（cd 是 change directory 的意思） |
| `cd ..`                | 回退到上一级目录                                             |
| `cd 目录1\目录2\..`    | 一次性进入多级目录                                           |
| `cd \`                 | 回退到盘符目录                                               |
| `cls`                  | 清屏                                                         |
| `exit`                 | 退出 DOS 窗口                                                |

## 配置 PATH 环境变量

不得不说虽然之前学习其他语言的时候也做过很多次配置 PATH 环境变量的操作，但是这次才算理解一些意义。

开发 Java 的时候是肯定会用到 JDK 里的开发工具的，比如编译的 `javac` ，运行的 `java`。

但是没有配置环境变量的时候，cmd 无法直接使用 javac 文件，因为不知道 `javac.exe` 的路径。

所以需要输入 `"JDK文件的目录\bin\javac.exe" 需要编译的文件的目录\需要编译的文件.java` （可以把 javac 和 java 文件直接拖进去，就会自动生成目录）相当麻烦。

配置环境变量之后，直接在 cmd 窗口里输入 `javac 需要编译的文件.java` 就能编译。

配置方法：（ Windows 系统）

① 此电脑 -- 属性 -- 高级系统设置 -- 环境变量，新建一个用户变量（建议命名和 Java 相关），并放入 JDK 文件夹的路径

② 在下方系统变量中选中 Path 变量 -- 编辑 -- 新建 -- 命名（建议命名和 Java 相关），并放入 JDK 内 bin 文件夹的路径。

最后在DOS中输入`javac`，如果显示使用 javac 的提示信息说明配置成功。