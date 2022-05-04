---
title: html_css_案例_1
date: 2022-05-04
tags: study
category: html+css
---

[黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=1)



![Av2CA.png](https://s1.328888.xyz/2022/05/02/Av2CA.png)

以上为学成在线网页案例。本文主要涉及老师讲解的 Header 部分。

# 分析

建议样式设置遵循以下步骤：

1. **布局定位属性：display / position / float / clear / visibility / overflow**
2. **自身属性：width / height / margin / padding / border / background**
3. **文本属性：color / font / text-decoration / text-align / vertical-align / white-space / break-word**
4. **其他属性：content / cursor / border-radius / box-shadow / text-shadow / background: linear-gradient……**

设计思路：

1. 确定版心（在页面最中央的），宽度一旦确定不能更改
2. 确定行模块（标准流）和列模块（浮动）。*网页布局第一准则*
3. 确定浮动元素中每个列的大小和位置。*网页布局第二准则*
4. **先理清布局结构，再写代码。**

# 写代码

首先记得清除全局样式

```css
* {
    margin: 0;
    padding: 0;
}
```

### 版心

利用 Ps 测量知，版心部分宽1200像素，并且全部居中对齐。

因为涉及到好几个块级元素都是这样的样式，所以用类选择器。

```css
.w {/*版心的宽度、居中样式*/
    width: 1200px;
    margin: auto;/*设置 margin-left 和 right 都是 auto，就会居中*/
}
```

然后设计顺序是先把一行设计完（包括这行内的所有元素），然后再设计下一行。



## Header

![AEZUX.png](https://s1.328888.xyz/2022/05/02/AEZUX.png)

从左到右依次为：logo，导航栏 nav，搜索 search，头像 user 四列。

四列有着同样的高度，设置这一行的盒子的样式第一种做法是一整个盒子把这一行四列包起来，这个盒子和这四个元素一样高，然后设置上下外边距；（老师的方法）

第二种做法是一整个盒子包住这四个元素，但是这个盒子更大一些，在盒子内部设置内边距留有一些空间；解法很多。

记得 header 盒子要同时选中 header 和版心 w 两个样式类。

```html
<div class="header w">头部盒子</div>
```

测量得知：logo 高度42像素，上下各有30像素的空白。

```css
.header {/*包裹 header 一行的盒子的样式*/
	height: 42px;
    margin: 30px auto;/*表示：上下30px，左右水平居中*/
}
```

#### logo：学成在线

```html
<img src="image/logo.png" />
```

logo 图标高42像素，宽200像素。此外记得，这几个盒子横向放在一行中，所以都要加 float 属性。

```css
.logo {
    float:left;
    width: 200px;
    height:42px;
}
```

#### 导航栏

**实际开发中，导航栏是采用 li 包裹 a 标签的方法实现的**，这样使得链接标签结构更加清晰，而且如果堆叠 a 标签，有可能被搜索引擎怀疑恶意堆砌关键字而降权。

```html
<div class="nav">
	<ul>
        <li><a href="#">首页</a></li>
        <li><a href="#">课程</a></li>
        <li><a href="#">职业规划</a></li>
    </ul>
</div>
```

先把页面中的所有 li 标签前面的圆点去掉，然后写 nav 类选择器。

测量得知，其距离左边的 logo 图标60像素。

```css
li {
    line-style: none;
}
.nav {
    float:left;
    margin-left: 60px;
}
```

对于具体的三个导航栏，浮动要给 li 标签设置而不是 a 标签设置。每个导航栏按钮之间相距30像素。

```css
.nav li {
    float: left;
}
```

然后，真正想实现点击跳转的链接效果，点击范围不只局限于文字区域，而是整块盒子可以点击。所以要给 a 设置为块级元素属性。

三个导航栏长度随其内容的长度而变化，所以要设置水平方向上内边距 padding 而不能设置盒子具体大小。

```css
a {
    text-decoration: none;/*去掉链接标签自带的下划线*/
}

.nav li a {
    display: block;
    height: 42px;
    padding: 0 10px;
    line-height: 42px;/*使得文字内容垂直居中对齐*/
    font-size: 18px;
    color: black;
}
```

li 标签不用设置宽度，一方面是浮动的块级元素的特性：其宽度会和内容一样宽；

一方面考虑到以后可能还会添加新的导航栏。

最后是期望鼠标经过导航栏链接元素的时候，文字变成蓝色，且盒子底部会出现蓝色的线提示鼠标悬停在该块上。用 hover 完成。

```css
.nav li a:hover {
	border-bottom: blue;/*线宽和文字内容一样宽*/
    color: blue;
}
```

#### 搜索框 

![hdRMC.png](https://s1.328888.xyz/2022/05/03/hdRMC.png)

一个大盒子包住，左侧：输入文本框；右侧：按钮。

```html
<div class="search">
    <input / value="输入关键词">
    <button />
</div>
```

```css
.search {
    float: left;
    width: 410px;
    height: 42px;
    margin-left: 50px;
}

.search input {/*虽然相邻的行内块元素会放在同一行，但是行内块元素之间有默认空隙，所以要加浮动。*/
    float: left;
    width: 345px;/*其实整个输入框宽360px。只是因为盒子设置了宽度，再有了 padding-left 会撑大盒子。因此要360-15*/
    height: 42px;
    padding-left: 15px;
    border: 1px solid blue;
    border-right: 0;/*因为右边是搜索按钮，可以不用边框*/
    color: #bfbfbf;
    font-size: 14px;/*Ps 中测量得到的 pt 单位和 px 一样大*/
}

.search button {/*虽然相邻的行内块元素会放在同一行，但是行内块元素之间有默认空隙，所以要加浮动。*/
    float: left;
    wdith； 50px;
    height: 42px;
    button: 0;/*去掉按钮默认的边框*/
    background: url(image/button.png);
}
```

#### 登录状态

![hlga2.png](https://s1.328888.xyz/2022/05/04/hlga2.png)

```html
<div class="user">
    <img url="" alt="" />
    qq-leishui
</div>
```

```css
.user {
    float: right;
    line-height: 42px;
    margin-right: 30px;
    font-size: 14px;
    color: #666;
}
```

图片文字部分居中：之后的课程涉及到。
