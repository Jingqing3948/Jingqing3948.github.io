---
title: ER 和 EER 模型
date: 2022-04-18
tags: problems
category: database
---

本文思路主要来源于[骆昊jackfrued 老师的网课](https://www.bilibili.com/video/BV1rP4y157jW?spm_id_from=333.999.0.0)
仅供本人学习参考，未做其他用途！

在此也建议读者通过老师的课程学习。

## 介绍

为什么要有 ER 图和 EER 图的存在？

**真正做项目、设计数据库时，**实际并没有这么简单，比如还有很多用户看不到、但为了方便 DBA 使用而创建的字段。如 id，一般还会有两条 Date 字段（一表示这条记录被创建的时间，二表示这条记录最后更新的时间），以及预留一个 VARCHAR / json 字段；还有一些其他注意事项（比如 auto-increment 约束其实开发中不常用，更多使用算法比如分布式 ID 生成算法（如 SnowFlake……）当然对课程来说这并不重要）**是不可能一上来就写 SQL 语句的，需要先设计表的结构和表之间的关系。**

ER 图（Entity Relationship, 实体关系）因此出现。下图是一个 ER 图的示例，其中矩形框代表：表（也就是实体），椭圆框代表：表中的字段（实体的属性），菱形框代表：关系，在连接线上表明了关系的重数。

![百度百科图片](https://img-blog.csdnimg.cn/img_convert/e154bec557baf860b9da8b406aa2b411.png)

相较于大段的 SQL 建表语句，只要能看懂 ER 图，表的结构、关系一目了然。

Workbench 等工具支持画 ER 图，甚至画好后可以自动生成 SQL 语句建表。不过 Workbench 中的图是 EER 图（扩展的ER 图）

![EER 图示例](https://img-blog.csdnimg.cn/img_convert/b7d05f2614049fce76e68e0121c805a0.png)

相比普通 ER 图确实感觉清晰了很多，而且每个字段可以添加 PK、NN、UQ、DEFAULT、FOREIGN KEY 等。

在 EER 图下，点击 DATABASE - FORWARD ENGINEER 正向工程，可以选择要生成的表、字段，生成 SQL 语句建立表。

*自动生成的 SQL 语句中，外键下方有两句话`ON DELETE NO ACTION` `ON UPDATE NO ACTION`，意为：当外键参考的主键修改/删除时，外键所在的表会受到什么样的影响？——不采取任何行动。建议去掉这两句话。如果去掉，就不能随便修改/删除外键在使用的主键。*

同样地，在 SQL 语句页面，点击DATABASE - REVERSE ENGINEER 反向工程，可以根据表的结构生成 EER 图。

*Power Designer 建模工具，也支持正 / 反向工程，可以生成 SQL 方言。完整版付费。*