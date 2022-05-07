---
title: SQL 语句——DQL
date: 2022-04-25
tags: study
category: database
---

学习自b站骆昊jackfrued 老师的网课。

查询语句。

*本节中使用到的例子：主要包含 tb_student 学生表（学号 stu_id，姓名 stu_name，地址 stu_address,所属学院号 col_id）*

*tb_record 记录表，连接学生和所选的课 id 及课程成绩（学号 stu_id，课程号）*

## Select

查询所有学生的所有信息

```mysql
select * from `表名`;-- * 号表示所有字段
```

但是这样有一点影响效率，会先查出学生表中有哪些列，再查询这些列的数据。

实际使用一般都是手动写上所有要查询的字段。（投影 Projection，只查询某几列）

```mysql
select `stu_id`,`stu_sex`,`stu_name`,`stu_address` from `tb_student`;
```

如果只查询部分列，就只写那几列就行。

## 别名

给字段或表起别名—— alias，简写为 as。

```mysql
select `stu_id` as `学号` from `tb_student`;
```

as 可以省略，不过还是写上可读性高一些。*一般字段不省略，表省略。具体还得看公司编程规范的要求，没有就看个人喜好了。*

## 条件

限制查询记录的条件——where（选择 Selection，只查询某几行）

```mysql
select * from table where `stu_sex`='M' or `stu_name`='Jingqing';
```

多个条件之间用 or 连接就是或者的关系，满足一个即可；用 and 连接就是和的关系，同时满足。

*性能问题，一般不用 or 而使用 union，结果取并集。*

```mysql
select * from table where `stu_sex`='M' 
union
select * from table where `stu_name`='Jingqing';
```

***如果在 union 后面加 all，意思是并集不会去掉重复的部分，相交的部分会显示两遍。***

类似 10<x<30 的形式不可以直接写两个等式，要拆成 x>10 and x<30 。

这里的字段如果是数字类型的，也可以进行 + - * / % mod（也是取余）以及 < >  = >= <= <> 等形式的运算。

还有一种条件写法是 `between …… and ……` 

```mysql
select * from table where `stu_age` between 10 and 30;-相当于 <=30 and >=10
```

## 分支结构

如果 sex 是布尔值，1代表男，0代表女，怎么把0和1处理成对应的性别？

```mysql
select if(`stu_sex`,'男','女') as '性别' from `tb_student`;-- 有点类似三目运算符 ?:
```

if 中第一项可以是表达式，如 age > 10.

**这个是 Mysql 数据库的方言，只能在 Mysql 数据库中生效。**比如 Oracle 数据库中对应的函数是 decode，不能通用。

通用的分支结构：

```mysql
select case `sex` when 1 then '男' else '女' end as '性别' from `tb_student`;-- end 表示条件判断结束
```

## 模糊查询

如：查询所有姓王的学生。

```mysql
select * from `student` where `stu_name` like '王%';-- % 代表零个或多个任意字符，表示查询姓的且名字只有2个字的学生
select * from `student` where `stu_name` like '王_';-- _ 代表一个任意字符，表示查询姓王的且名字只有2个字的学生
```

这里和正则表达式有一定联系，之后会单独学习。

事实上 Mysql 语句也支持正则表达式的，正则表达式也比这两种模糊查询的通配符功能强大很多。

```mysql
select * from `student` where `字段名` regexp '正则表达式';
```

模糊查询，特别是 % 在左侧的时候，性能还是比较差的，尽量避免。

## 空值处理，以及三值逻辑

**下面两种写法是错误的！**

```mysql
select * from `student` where `stu_address` = null;
select * from `student` where `stu_address` <> null;
```

因为表达式产生的值有三种（三值逻辑），true, false, unknown。和空值做运算的时候，就会得到 unknown 的结果。

正确做法：

```mysql
select * from `student` where `stu_address` is null;
select * from `student` where `stu_address` is not null;
```

## 去重

```mysql
select distinct `字段名` from `表名`;
```

## 排序

```mysql
 select `字段名` from `表名` order by `字段名1` asc, `字段名2` desc; -- asc: 默认，ascending，升序；desc：descending，降序
```

order by 后面跟多个字段，就是先按字段1排序，字段1相等时再按字段2排序。

## 当前日期

curdate()获取当前日期，使用 datediff() 函数可以和出生日期做差获取年龄。

now()获取当前年月日时分秒（datetime）。

## 取整

floor() 函数是下取整。floor(3.99) = 3.

ceil() 函数是向上取整，ceil(3.1) = 4.

round() 函数是四舍五入，第二个参数是保留几位小数的意思，round(3.5,0)=4

```mysql
select floor(datediff(curdate(),`date`)/365) from `staff`;
```

*可以通过？ functions 查看函数一览。还会有相应的例子提示~*

## 聚合函数

描述性统计信息：包括集中趋势和离散趋势。

集中趋势：平均值，中位数等。

离散趋势：方差，标准差等。

聚合函数属于 SQL 语句，所有 DBMS 都能用。

- min(字段名)

- max(字段名)

- avg(字段名) *做计算的时候会忽略 null 值*

- sum(字段名)

- count(字段名) *做计算的时候会忽略 null 值*

  ***如果利用 sum()和count() 做除法求平均值，要考虑空值对结果的影响。***

  *比如有10个学生，但有一个学生的成绩为空，如果忽略掉成绩为空的学生， sum(score) / count(stu_id) 就是错误的，因为是9个人的成绩 / 10.* 

  *如果成绩为空的学生视为 = 0，就要用 sum(score) / count(stu_id)，或者对 avg() 函数做如下处理：*

  ```mysql
  select avg(ifnull(`score`,0)) from student;
  ```

  *ifnull() 函数也是 mysql 的方言，类似 if。在 Oracle 中就是 nvl。*

  coalesce() 也可以处理空值，是标准数据库的函数，它会取第一个非空值。如：coalesce(score, 0)。

- std(字段名)，计算标准差，越小说明越稳定。

  + std(), stddev() 和 stddev_pop()：所有数据总体的标准差
  + stddev_samp()：样本标准差，抽样计算。

- variance(字段名)，计算方差，标准差的平方。

  - variance(), var_pop()
  - var_samp()

## 分组

聚合函数通常和分组一起使用。分组是非常重要的操作。

```mysql
select * from `student` group by `sex`;
```

PS： Excel 的数据透视表有同样功能：

插入-数据透视表-选择所有数据范围（选中左上角的单元格，Ctrl + Shift + →，Ctrl + Shift + ↓选中所有有数据的单元格）-选择放置数据表的位置（选择某一列的第一行的单元格，如 J1 或 K1 ）-确认

行里选择要分组的列（如：销售区域）值选择要分组运算的对应值（如：销售额），值默认做的就是求和运算。

```mysql
select * from `student` group by `sex` with rollup; -- with rollup: 在最后一列求个总和
```

group by 后面也可以跟多个字段，先按第一个分组，然后在每个组里再按第二个分组。

**如果进行了条件查询，用到了 group by 得到的结果，不能直接用 where，要使用 having.**

```mysql
select `stu_id`,avg(score) from `Score` where avg(score)>90 group by `stu_id`; -- 错误
select `stu_id`,avg(score) from `Score` group by `stu_id` having avg(score)>90; -- 正确
```

分组以前的筛选： where, 写在分组后

分组以后的筛选： having, 写在分组后。

*其实这里个人有一个小误区：select 不用非得查出 avg() 才能做 having 条件判断。比如要查询平均成绩大于90分的学生学号，就可以改成：*

```mysql
select `stu_id` from `tb_record` group by `stu_id` having avg(score)>90;
```

*也是没有问题的。*

查询 1111,2222,3333三门课程平均成绩大于90分的学生的平均成绩：

```mysql
select `stu_id`,avg(score) from `Score` where `cou_id` in (1111,2222,3333) group by `stu_id` having avg(score)>90;
```

## 子查询

子查询 (subquery) 的用途：

- 集合成员资格，判断某一元素是否是某一个集合的成员
- 集合之间的比较，某一个集合是否包含另一个集合等
- 集合基数的测试，测试集合是否为空，测试集合是否存在重复元组。

查询年龄最大的学生的姓名。

可以先查询出最大年龄，再查询学生表中年龄等于这个值的学生姓名。

一种方法是嵌套子查询：

```mysql
select `stu_name` from `tb_student` 
where `stu_birth` = (
    select min(`stu_birth`) from `tb_student`
);
```

另一种方法是定义变量：mysql 定义变量，需要加@（但是不能把多个值赋给一个变量！）

```mysql
set @a =(select min(`stu_birth`) from `tb_student`);
select @a; -- 可以查看一下 @a 的值。
```

如果子查询有很多结果，满足其中一个即可：不能用等号，要用 **in** 。

```mysql
select `name` from `tb_student` 
where `stu_id` in (
    select `stu_id` from `tb_record` group by `stu_id` where count(*)>=2
); -- 查询出所有至少选了2门课的学生姓名
```

如果用了等号，报错： subquery returns more than 1 row.

## 多表连接

### 笛卡尔积

```mysql
select `stu_name`,`stu_id`,`col_name`,`tb_college`.`col_id` from `tb_student`,`tb_college`;
```

如果不加条件的从两个表中投影出数据，就会获得两个表中所有记录的笛卡尔积，即排列组合。

本例中，学生表和学员表中都有学院号 col_id 字段，因此投影该字段的时候需要指明是从那个表中得到的，用 表名.字段 指定。

### 自然连接

1. 有外键约束：利用外键连接，不用加条件，自动连接。

```mysql
select `stu_name`,`stu_id`,`col_name`,`tb_college`.`col_id` 
from `tb_student` natural join `tb_college`;
```

2. 没有外键，但是两个表中有同名的列（如本例中两个表中都有 col_id 列）也可以进行自然连接。*注意：不管有几个同名的列，所有列都会作为连接的条件！*
3. 如果没有外键也没有同名列，就只会得到笛卡尔积的结果。

### 内 / 外连接

另一种连接方式是 inner join / outer join 

```mysql
select `stu_name`,`stu_id`,`col_name`,`tb_college`.`col_id` 
from `tb_student` inner join `tb_college` 
on `tb_student`.`col_id`=`tb_college`.`col_id`; -- 查询所有学生的姓名、学号、对应的学院名、学院号
```

inner join：只有左表中有的记录，而且在右表中能找到对应的记录才会呈现出来。

left outer join：左表中所有内容都会呈现出来，右表中如果没有对应的内容补 null。

right outer join：右表中所有内容都会呈现出来，左表中如果没有对应的内容补 null。

full outer join：左右表数据全拿出来，没有对应的内容都补 null。但是 mysql 并不支持全外连接，可以用左外连接 union 右外连接代替。

### θ 连接

添加条件使得两个表中的数据相互对应：

```mysql
select `stu_name`,`stu_id`,`col_name`,`tb_college`.`col_id` from `tb_student`,`tb_college` 
where `tb_student`.`col_id`=`tb_college`.`col_id`; -- 查询所有学生的姓名、学号、对应的学院名、学院号
```

### 三表连接

链接条件用多个条件筛选。

```mysql
select `stu_name`,`tb_student`.`stu_id`,`tb_course`.`cou_name`,`tb_course`.`cou_id` from `tb_student`,`tb_course`, `tb_record`
where `tb_course`.`cou_id`=`tb_record`.`cou_id` 
and `tb_student`.`stu_id`=`tb_record`.`stu_id`;

select `stu_name`,`tb_student`.`stu_id`,`tb_course`.`cou_name`,`tb_course`.`cou_id` from `tb_student`
inner join `tb_record`
on `tb_student`.`stu_id`=`tb_record`.`stu_id`
inner join `tb_course`
on `tb_course`.`cou_id`=`tb_record`.`cou_id`
where `tb_course`.`cou_name` is not null; -- where 写在最后

select `stu_name`,`tb_student`.`stu_id`,`tb_course`.`cou_name`,`tb_course`.`cou_id` from `tb_student`
natural join `tb_record`
natural join `tb_course`;
```

## 查询小技巧

百度搜索：filetype:pdf python 搜索带 python 名的 pdf 文件

python -推广链接 不想看到广告推送

site:zhihu.com python 只搜索知乎里的 python 内容

## 分页查询

limit 是 mysql 的方言。

```mysql
select * from `tb_student` order by `stu_id` desc limit 5;-- 只显示前五条数据.如果不足5条，就有几条显示几条。
select * from `tb_student` order by `stu_id` desc limit 5 offset 3;-- 只显示4-8条数据(跳过前3条数据)
select * from `tb_student` order by `stu_id` desc limit (3,5);-- 只显示4-8条数据(跳过前3条数据)
```

## 派生表

select 的返回值也是一个关系。（关系运算的封闭性，关系的运算仍然是关系）

查询学生姓名和平均成绩。

查询学生学号和平均成绩的话，可以在 tb_record 表里根据学生学号分组，然后投影出来。但查询学生姓名有需要拿着对应的学号去学生表里连接，要怎么把学生姓名和学生平均成绩关联起来呢？

先通过一个查询，得到一个派生表：

```mysql
select `stu_id`,avg(score) from `tb_record` group by `stu_id`;
```

然后把其结果作为一个新表，和学生表做关联。

```mysql
select `stu_name`,`avg(score)`from `tb_student`
natural join (select `stu_id`,avg(score) from `tb_record` group by `stu_id`) `tb_derived`;-- 最后是起别名的意思
```

**临时表必须要起别名！！！**不然报错。

**注意：查询学生姓名和选课数 和 查询每个学生姓名和选课数的区别！**因为有的同学没有选课，如果用 natural join，inner join，就只能查询到选了课的学生，**没选课的学生就不会查出来。**

如果要查询每个学生，就要用到外连接。没选课的学生选课数显示 null。（当然可以用 ifnull(字段, 0) 把 null 替换成 0。记得 ifnull 是 mysql 方言）。