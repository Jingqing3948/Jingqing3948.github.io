---
title: Python 爬虫入门_1
date: 2022-05-21
tags: study
category: Python
---

# 目标

爬取豆瓣 Top250 电影基本信息，包括名称、评分、评价数、电影概况、电影链接等。

# 准备工作

[豆瓣电影 Top 250 (douban.com)](https://movie.douban.com/top250)

爬虫是按照一定**规则**，**自动抓取**互联网信息的程序或脚本。如今主流策略是**根据用户需求定向抓取。**

*在百度指数中搜索关键词，可以看到该关键词的访问量。*

实际上很多网站不自己写内容，爬取其它网站的内容复制到自己这里就方便的多。

爬虫很早就有，但是最近计算能力上涨，如大数据等应用，使得爬虫的价值继续提升，所以最近火一些~

![image-20220520012210874](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220520012210874.png)

## url

当我们浏览豆瓣 top250电影，进行翻页时，会发现 url 变成了：

[https://movie.douban.com/top250?start=25&filter=](https://movie.douban.com/top250?start=25&filter=)

意思是从第26条记录开始显示（默认一页25条）。如果手动改成 start=31，就会从32条开始显示。filter 去掉也可以。

当我们发送 url 给服务器，服务器返回源代码（html），经过浏览器解析变成看得到的样子。

![image-20220520013155589](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220520013155589.png)

元素中可以看到网页 html 元素

![image-20220520013933832](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220520013933832.png)

network 中可以看到请求状况。（如果请求状况为空：点击 all 全部按钮)

network - header 中的general 和 response headers 的内容都是我们发给服务期端的，服务器返回的内容在 network - response 响应中。

其中，user agent 表明浏览器版本，cookie 中包含登陆后的信息。

## 找到要获取的文件所在位置

