---
title: 微信开发者工具和gitee实现多人协作
date: 2022-03-27
tags: problems
category: projects
---

将代码上传到码云实现多人合作开发。

<h1>1. gitee</h1>

首先进入gitee，注册一个账号。

新建一个仓库：

![image-20220327012843219](https://img-blog.csdnimg.cn/5abc8daf23a342a3906c4111e0b2d950.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

输入仓库名称，设置为私有，初始化、模板、分支模型都可以不添加。

点击创建，就建好了一个新的仓库。

![image-20220327013057263](https://img-blog.csdnimg.cn/0f17a2379dd44d23a084ad3df7373c6d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

这里可以看到仓库的https地址，一会会用到。

因为一会直接将小程序代码文件放进来，暂时先不用添加文件。

<h1>2. git</h1>

git是一个开源的分布式版本控制系统，可以高效地实现版本控制。

<h2>下载</h2>

[从官网上下载git](https://git-scm.com/downloads)。

<h2>注册</h2>

下载完成后在任意目录下右键，都会出现git GUI here和git Bash here.

点击git Bash here，在当前目录下开启命令行：

![image-20220327013749178](https://img-blog.csdnimg.cn/2896c2473b5c4d428995b6deb13f0e81.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_14,color_FFFFFF,t_70,g_se,x_16#pic_center)

输入：

```
git config --global user.name '要注册的git用户名，不可以是中文'
git config --global user.email 'gitee的邮箱'
```

注册完成。

<h2>克隆远程仓库</h2>

首先新建一个想要放克隆下来的仓库内容的文件夹，进入该文件夹下。可以直接双击打开文件夹后右键git Bash here，也可以在命令行中使用cd进入文件夹目录下。

复制新建仓库的https地址：

```
git clong '仓库的https地址，如https://github.com/username/try.git'
```

如果克隆的是有内容的仓库，这时就应该可以看到文件夹内出现了仓库里的文件。

仓库是空，可能会出现warning字样，问题不大。

然后就可以对内容进行一些修改，项目的开发。

<h2>推送</h2>

修改完后要将本地的代码推送到远程仓库。首先提交到本地仓库。通过add添加要提交的文件：

```
git add .
```

add后面加.的意思是把所有做过修改的文件都添加。如果只想添加部分新修改的文件，add后面根具体的文件名即可。

再将添加的文件提交：

```
git commit -m '备注，如：提交了app.json文件'
```

若出现`1 file changed`之类的字样，说明成功提交到了本地仓库。

最后输入以下代码提交：

```
git push origin master
```

gitee默认分支是master，github默认分支是main，需要先修改分支为main后提交`git branch -M main`

出现'done'的字样说明成功。

回到码云仓库，刷新一下，就可以看到新增的文件，以及还会显示commit的内容。

![image-20220327015550566](https://img-blog.csdnimg.cn/97f5848e7a34432fbc24d38f3846595c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)

*这里出现了不同的commit，是博主在不同时间做的不同修改。*

<h1>3. 微信开发者工具</h1>

通过微信开发者工具，可以不使用命令行操作，直接拉取、推送代码。

打开对应小程序文件夹，点击右上角版本管理，左侧栏如下：

![image-20220327015735281](https://img-blog.csdnimg.cn/d0e8f12d8b954f11bdd936a70434fe2a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_7,color_FFFFFF,t_70,g_se,x_16#pic_center)

首先要在设置中进行认证。点击设置--网络和认证

![image-20220327015821950](https://img-blog.csdnimg.cn/ab9f812446f34fab95604669296032c3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_19,color_FFFFFF,t_70,g_se,x_16#pic_center)

输入码云的gitee账号和密码。

博主和朋友尝试将代码上传到github上时，一直在这里有问题。明明用户名和密码正确，推送却会卡住或者显示认证失败。而换成码云就非常顺利。因此更建议使用码云新建仓库、上传代码。

然后在 远程 中新建仓库信息

![image-20220327020101940](https://img-blog.csdnimg.cn/812100ec678b42e1985baf1e9357ab2d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_11,color_FFFFFF,t_70,g_se,x_16#pic_center)

名称就是仓库名称，url是https的仓库地址。

设置完成后就可以点击左上角抓取远程仓库内容到本地仓库。



对于修改后的文件以及提交，在工作区进行：

![image-20220327020242959](https://img-blog.csdnimg.cn/d24162c54f604021b31b77586716a8c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

这里右边上面勾选文件，就相当于git里的add添加文件。下面的提交就相当于commit。输入信息后点击提交，就上传到本地仓库了。

然后点击左上角推送，**不要推送到新的分支，勾选中间项：推送到一下远程分支**。这一部相当于git的push。

![image-20220327020458850](https://img-blog.csdnimg.cn/ec5c7bc3581a457cb4ccf654aa10e995.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6JCo56eR5aGU6LWE5rex5bmy5ZGY,size_15,color_FFFFFF,t_70,g_se,x_16#pic_center)

点击确定，出现对号就说明成功。接着可以在仓库中刷新看到新的修改。

