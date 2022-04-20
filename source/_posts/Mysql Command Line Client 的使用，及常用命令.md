---
title: Mysql Command Line Client 的使用，及常用命令
Date: 2022-04-18
tags: study
category: tools
---

Mysql Command Line Client 是官方提供的客户端。之前一直直接用 Windows 的命令提示符窗口输入 Mysql 语句，不知道两者具体区别在哪里。不过还是重新安装了一个尝试一下。

## 安装

MYSQL 文件夹里自带有 Installer，在里面选中对应电脑版本的 Mysql Server 下载即可。

## 配置

由于安装目录和 Mysql 不一致，缺少配置文件 my.ini 导致 Mysql Command Line Client 一开始无法使用。直接把 Mysql 里的 my.ini 复制到 Mysql Server 目录下就可以了。

## 常用命令

其实在 cmd 窗口中登录 mysql 时也会给出相应的命令提示，只是当时没有仔细研究。正好借使用 Mysql Command Line Client 的机会了解一下常用的命令。

开始菜单中出现了两种不同的 Mysql Command Line Client 窗口，其中一种支持 Unicode ，一种不支持。打开后即会提示输入登录密码，登录后就和 cmd 窗口中操作基本一致。

| 命令                             | 作用                                                |
| -------------------------------- | --------------------------------------------------- |
| \h, \?, ?                        | 获取帮助                                            |
| \c                               | 清除前面输入的内容（内容输入有误时使用）            |
| \R                               | 修改 每次输入命令前左侧的提示样式（默认：'mysql>'） |
| ? 需要查看帮助的命令;            | 显示该命令的帮助（如：? show）                      |
| show databases;                  | 查看所有数据库                                      |
| use '数据库名';                  | 选中某个数据库                                      |
| （选中某个数据库后）show tables; | 查看当前数据库中所有表                              |
| exit / quit                      | 退出                                                |