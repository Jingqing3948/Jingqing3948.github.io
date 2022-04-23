---
title: SQL 语句——DML
date: 2022-04-23
tags: study
categories: database
---

## 插入

```mysql
insert into `表名` values (对应字段值)(对应字段值)(对应字段值);//写多个括号，可以一次填写多行
insert into `表名` (要填写的字段) values (对应字段值); //省略不写的一定是有默认值的或者可以非空的
```

注意：对应字段值一定要和字段相匹配。如果有默认值的字段也不要省略不写，要写上 `default` ，不然系统也难以分辨哪一项使用默认值。

*日期可以写字符串 2002-01-01，会自动转换*

插入完后显示：1 row(s) affected（如果写了多个括号，一次插入多行，会显示多个 row(s) affected）是影响了几行的意思。

**如果主键、unique 的记录重复会报错。**报错显示： `duplicate key for key '定义的约束键名称'`

**填写了规定的 check 以外的内容会报错。**报错显示：`check constraint '定义的约束键名称' is violated.` 不过字符串不区分大小写，规定性别只能填 'F' 'M' 的话，填 'f' 'm' 也行。

**如果对应的外键在原表中没有对应的记录会报错。** 报错显示：`cannot add or update a child row: a foreign constraint fails` 比如学生表的学院号参照了学院表，但是有学院表中不存在的学院号时。

**如果没有给 NOT NULL且没有默认值的字段赋值时会报错。** 报错显示：`Field '字段名' doesn't have a default value.` 尽量不要出现空列，之后处理空值会带来很多麻烦。哪怕用空字符串、0、1代替也更好一些。

## 删除

```mysql
delete from `表名`;//这可不兴用啊！
delete from `表名` where `字段名`='要删除的记录的对应字段值';//不等于可以用<>，有的 DBMS 支持!=
delete from `表名` where `字段名` = '字段值' or `字段名` = '字段值';//删除多条语句
delete from `表名` where `字段名` in ('字段值','字段值');//删除多条语句
```

但是如果要删除的这条记录在其他表里是外键，就无法删除。报错显示： `Cannot delete or update a parent row` 。

删除了外键约束之后就可以随意修改两个表对应的值而不报错了。

另一种删除表的方式是：

```mysql
truncate table `表名`;
```

截断表，这个比 delete 还要危险。delete 了表之后，如果有日志，还是可以找回原数据的。截断就算有日志也没法恢复。

## 更新

```mysql
update `表名` set `列名` = '值';//把这一列的数据全都改成这个值，不常用
update `表名` set `列名` = '值',`列名` = '值',`列名` = '值' where `字段`='值';//限制条件，只修改某几个记录
```

