---
title: Mysql ER 和 EER 模型
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

### ER 模型

ER 图（Entity Relationship, 实体关系）因此出现。下图是一个 ER 图的示例，其中矩形框代表：表（也就是实体），椭圆框代表：表中的字段（实体的属性），菱形框代表：关系，在连接线上表明了关系的重数。

![百度百科图片](https://img-blog.csdnimg.cn/img_convert/e154bec557baf860b9da8b406aa2b411.png)

相较于大段的 SQL 建表语句，只要能看懂 ER 图，表的结构、关系一目了然。

### EER 模型

相较 ER 模型多了泛化层次、汇集层次、弱实体等概念。

#### 泛化层次

包括 generalization 和 specialization （泛化和特化）、父类（superclass）和子类（subclass）的概念。

**子类父类**就是类似 Java 的继承，如动物是父类，猫、狗是其子类。

**泛化**又叫归纳，就是将几个类的共同属性提取出来作为父类；

**特化**又叫演绎，就是在父类的基础上添加各自特殊的属性作为子类。

其中，子类和超类又有两个关系属性：mandatory 和 optional、disjoint 和 overlapping。

**mandatory / optional**：父类中的所有属性都必须包含在每一个子类中 / 不用全部继承，选择部分继承即可（完全性限制）

**disjoint / overlapping**：继承同一个父类的几个子类之间是否可以有相交的属性（相交性限制）

![img](https://pic1.zhimg.com/80/v2-11dfbdfea0c8705a7e425554b66ef610_1440w.jpg)

图中 运输工具是父类，飞机、火车、汽车是子类。圆圈中写 D / O，表示 disjoint / overlapping。父类和圆圈之间是双实线，表示是 mandatory 完全性继承。

#### 汇集层次

Aggregation, 汇集层次不再有父类子类的区别，而是由……组成的区别。

![img](https://pic4.zhimg.com/80/v2-bcf4d6b1f89bc0b8e12f7f835518537b_1440w.jpg)

如图，房间、门窗、电脑、投影仪等是教室的组成部分，不是继承关系。

#### 弱实体

一种实体只有另一种实体存在的时候才有意义。如父母和子女，少了一方另一方就没有意义了。

![img](https://pic4.zhimg.com/80/v2-2891cd3fb7b84a5b98e158fa77cd3cdb_1440w.jpg)

Workbench 等工具支持画 ER 图，甚至画好后可以自动生成 SQL 语句建表。 Workbench 中的图是 EER 图（扩展的ER 图）

![EER 图示例](https://img-blog.csdnimg.cn/img_convert/b7d05f2614049fce76e68e0121c805a0.png)

在 EER 图下，点击 DATABASE - FORWARD ENGINEER 正向工程，可以选择要生成的表、字段，生成 SQL 语句建立表。

*自动生成的 SQL 语句中，外键下方有两句话`ON DELETE NO ACTION` `ON UPDATE NO ACTION`，意为：当外键参考的主键修改/删除时，外键所在的表会受到什么样的影响？——不采取任何行动。建议去掉这两句话。如果去掉，就不能随便修改/删除外键在使用的主键。*

同样地，在 SQL 语句页面，点击DATABASE - REVERSE ENGINEER 反向工程，可以根据表的结构生成 EER 图。

*Power Designer 建模工具，也支持正 / 反向工程，可以生成 SQL 方言。完整版付费。*