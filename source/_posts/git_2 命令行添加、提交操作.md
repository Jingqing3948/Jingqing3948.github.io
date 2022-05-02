---
title: git_2 命令行添加、提交操作
date: 2022-05-02
tags: study
category: git
---

# 本地库初始化

git 的专属命令都是以 git 打头的。

在文件夹中右键 -- git bash here 启动 git 命令行。

| 命令         | 作用                                 |
| ------------ | ------------------------------------ |
| ll           | 当前目录下的资源                     |
| ls -lA       | 带隐藏资源                           |
| ls -l\|less  | 分频查看                             |
| pwd          | 当前位置路径                         |
| mkdir 目录名 | 新建目录                             |
| git init     | 初始化当前目录（并创建 .git 文件夹） |

**.git 里存放的是本地库相关的子目录和文件，不要删除和随便修改！！**

# 设置签名

需要设置用户名和 email 地址，唯一用处就是标识开发者身份。

**登录代码托管中心的账号密码（如 github、gitee）和签名的用户名、email 地址没有任何关系。**

项目/仓库级别：只对当前本地库范围内有效。

系统用户级别：对当前系统的登录账号（lenovo）有效。

同时存在的话，项目/仓库级别优先级更高。但是二者必须至少有一个。

```
git config user.name Jingqing			//项目级别
git config user.email Jingqing@163.com	//项目级别

git config --global user.name Jingqing			//系统用户级别
git config --global user.email Jingqing@163.com	//系统用户级别
```

项目级别的签名：在项目文件夹下输入 `cat .git/config`，会发现在签名就保存在 .git 文件夹的 config 目录下。

项目级别的签名：`cat ~/.gitconfig`跳转到家目录查看。

*开发时如果不是特殊需求，一般一个系统级别签名即可。*

# 查看状态

git status 查看工作区、暂存区状态

no commit yet：指本地库里没有内容，因为 commit 之后是会放到本地库里的。

nothing to commit：暂存区没有内容可以提交。

changes to be committed：暂存区有内容等待提交。

# 提交文件

```
//vim 新建文件
vim good.txt
//然后输入文本文档内容，按 esc 后按两次大写 Z 保存退出。
git add good.txt
//这里提示：warning, 行末的 LF 会被替换为 CRLF，这和安装时 git 的配置有关，影响不大
git rm --cached good.txt
//撤回暂存区的内容
git commit good.txt
//提交文件，然后会提示：请输入提交的信息备注，并且显示一个 vim 编辑器
//输入注释信息后按 esc 输入 冒号+wq 退出

//但是其实这样才是更常见的用法
git commit -m "信息备注" good.txt
```

第一次提交会显示 root commit 根提交