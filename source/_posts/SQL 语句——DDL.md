---
title: SQL 语句——DDL
date: 2022-04-19
tags: study
categories: database
---

学习于：b站 骆昊jackfrued 老师的网课

| SQL 语句 | 作用                | 使用示例                                                     |
| -------- | ------------------- | ------------------------------------------------------------ |
| create   | 创建数据库 / 表     | create database \`数据库名\`; / create table \`表名\` ( 字段描述 ); |
| drop     | 删除数据库 / 表     | drop database \`数据库名\`; / drop table \`表名\`;           |
| use      | 选中数据库          | use \`数据库名\`;                                            |
| alter    | 更改数据库 / 表信息 | alter table \`表名\` add constraint \`约束名\` unique (\`字段名\`); |

PS： 

1. 修改表的引擎 / 更改自增约束初始值是在 create table \`表名\` () 后面添加的。

   ```mysql
   create table `表名`(
   
   )engine = innodb auto_increment=2 comment '表示例';
   ```

   

## 约束

### 主键约束

```mysql
primary key (`列名`),
```

*复合主键语法上没错，但是开发使用时非常不好用，需要至少两条字段才能唯一确定一条记录。一般不会用。*

*如选课表，有课程号和学生学号字段，合在一起作为复合主键可以唯一确定记录。但是一般会在新建一个选课id之类的可以唯一确定所有选课记录的字段作为主键。*

### 自增约束、非空约束

都在字段描述后面写即可

```mysql
`col_name` int auto_increment not null comment '列名',
```

以下三种可以在创建表时写，也可以之后写`alter table 表名 add constraint...`。

### 唯一约束

```mysql
constraint `uk_col_name` unique (`col_name`),
```

类似主键约束，唯一约束也可以设定多个字段，再括号里多写几个字段就行（如选课记录，一个学生不能重复选课，学生学号和课程编号不能重复）

### 检查约束

检查约束可以帮忙检查数据合法性，但是相对的，肯定对性能有损耗。

```mysql
constraint `ck_col_sex` check (`col_sex`='M' or `col_sex`='F'),
```

### 外键约束

首先在需要加外键的表中添加和另一个表的主键一样格式（名称可以不一样）的字段。

外键约束也会损耗性能，要检查对应表中对应字段是否对应。很多公司都不用外键约束，会通过其他方法确定两表特定字段是否对应。

```mysql
constraint `fk_col_id` foreign key (`在本表中的字段名`) references `另一个表名` (`在该表中的主键名`),
```

*一对多时，多的一方需要加外键约束。*
