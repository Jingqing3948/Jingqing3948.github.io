---
title: SQL 语句——DQL 例题及注意事项
date: 2022-04-25
tags: study
category: database
---

学习于：b站 骆昊jackfrued 老师的网课

## 简单排序——查询最大值、次大只

1. 员工表中含有员工号、姓名、薪水、职位、补贴 、所在部门等信息。查询薪水最高的员工姓名和薪水值。

   做法①：最普通的子查询，先查出最大工资值，再筛选出员工表中工资值等于这个数的员工。

   做法②：limit 做法。先按薪水降序排序所有员工，再只 limit 1 取第一条员工信息。这种做法的局限性在 limit 介绍的时候我也有写，就是当最值不只一位的时候，这种方法只能查出1位员工。

   做法③：

   ```mysql
   select `ename`,`salary` from `tb_employee` t1 where(
       select count(*) from `tb_employee` t2 where `t2`.`salary`>`t1`.`salary`
   )=0;-- 结尾改成1，就是工资第二高的人
   ```

2. 查询除了 boss 外工资最高的人。

   在题1的基础上，用 where 排除掉 boss。

   ```mysql
   select `ename`,`salary` from `tb_employee` 
   where `salary`=(
       select max(salary) from `tb_employee` where `position`<>'boss'
   );
   ```

3. 查询月薪 top 3的人。

   这时出现了和题1一样的情况。用 limit ，可能会漏掉工资并列的第四人；因此，这里用题1的方法③最合适。

   ```mysql
   select `ename`,`salary` from `tb_employee` t1 where(
       select count(*) from `tb_employee` t2 where `t2`.`salary`>`t1`.`salary`
   )<3 order by `salary` desc;
   ```

4. 查询所有员工的姓名和年薪。年薪计算公式：月薪*12+补贴。

   题目很简单，但是**遇到计算一定要注意数据是否 Not Null，如果出现 Null 数据结果也会变成 Null。**要用 ifnull() 或 coalesce() 函数限制一下。

5. 查询所有部门名称及员工人数。

   部门名称在部门表中，每个部门的人数则需要根据员工表中部门号统计。显然这里要先查出员工表的员工号和统计员工信息，再把结果表和部门表做连接。**重点注意几种连接的不同。**比如此题，即便有的部门没有员工，也要显示出来，因此应该部门表 left join 派生表。

   ```mysql
   select dname,total from `tb_department` t1
   left join (
       select dname, ifnull(count(*), as total from `tb_department` t2 group by `stu_id`
   ) on t1.`stu_id`=t2.`stu_id`;
   ```

6. 查询每个部门比本部门平均薪水高的员工名及薪水。记得说清楚列属于哪个表，不然报错 ambigious

   先按部门分组，查询出平均成绩和员工号表；再通过部门号和员工表连接，并筛选出薪水值大于平均值的。

   ```mysql
   select sname,salary from `tb_employee` t1
   inner join (
   	select avg(salary),dno from `tb_employee` group by dno;
   )t2 on t1.dno=t2.dno and t1.salary > t2.avg(salary);-- 两个连表条件
   ```

7. 查询每个部门中薪水最高的员工的用户姓名、薪水、部门名称。

   派生表通过薪水值、部门号和员工表、部门表相连接。

8. 查询薪水排名4-6员工的薪水、姓名、**排名**。

   问题1：怎么查出排名？

   问题2：这题并没有想象中的简单。比如员工薪水前8名是5000,5000,4000,4000,3000,3000,3000,2000，那么其实第三、第四人两个人并列第三，4-6显示的排名值就应该是3，5，5

   解决：mysql 8的窗口函数可以解决排名 / top N 问题。

   ①不用窗口函数：

   系统变量一般不写@或写两个@@（可以通过 show variables 查询），自己定义的变量写一个@，赋值的方法：`set @a=0;`或`select@a:=0;`而且变量也可以通过 as 起别名。

   我们先定义一个变量，变量从0开始，每次选择＋1，就可以当做序号用了。

   ```mysql
   set @a=0;
   select row_num,ename,salary from (
   	select @a=@a+1 as `row_num`,ename,salary,(select @a:=0) -- @a 重新赋值为0
       from `tb_employee` order by salary desc
   ) where `row_num` between 4 and 6;-- 子查询中做了几次查询，@a 就加几次
   ```

   注意不要在括号里用 limit 3 offset 3，因为这样的话子查询就只会进行3次，@a 的值就只会从1到3.

   另外，每次查询时都要重新给@a 赋值为0，不然其值会累加。

9. 查询每个部门薪水排名前两名的员工。

   Top N 问题通过题1的做法③解决。

   ```mysql
   select eno,ename,salary,dno from `tb_employee` t1
   where (
   	select count(*) from `tb_employee` t2 where t1.`dno`=t2.`dno` and t2.salary>t1.salary
   )<2 order by t1.dno asc, t1.salary desc;
   ```

## 窗口函数

内容来自：[通俗易懂的学会：SQL 窗口函数 - 知乎](https://zhuanlan.zhihu.com/p/92654574)

应用于组内排名和 Top N 类问题。 一般是处理 where 和 group by

窗口函数不光是函数，有一套完整的语法。

```mysql
<窗口函数> over (partition by <用于分组的列名>
                order by <用于排序的列名>)
```

<窗口函数> 处放聚合函数或专用窗口函数。

窗口函数是以一个列的形式使用的。

### 专用窗口函数

rank、dese rank、row_number

![img](https://img-blog.csdnimg.cn/img_convert/18d3c41260cad2a0ecbae4732018948c.png)

### partition by 和 group by 的区别

![img](https://img-blog.csdnimg.cn/img_convert/6650ec247ab10db38a52d904bfecbd3c.png)

使用：

```mysql
select `ename`,`sal`,
rank() over (order by `sal` desc)as `r1`,
dense_rank() over (order by `sal` desc)as `r2`,
row_number() over (order by `sal` desc)as `r3`
from `tb_emp`;
```

第八题窗口函数做法：加一个 where r between 4 and 6 的条件。

至于要用哪种专用函数，就要看要求了。

第九题窗口函数做法：因为产生了分组，因此不能直接用 where r <=2。但是窗口函数的分组后的列做筛选，既不能直接用 where 也不能用 having。应该把窗口函数的查询结果作为一个派生表，再用 where 做选择。

```mysql
select `ename`,`sal`,`dno`
from(
    select `ename`,`sal`,`dno`,
    rank() over (partition by `dno` order by `sal` desc)as `r`
    from `tb_emp`
) `temp` where `r`<=2;-- 不能在派生表里直接筛选
```

*窗口函数性能还是比较差的，业务中不应使用，数据分析师可能会常用一些。*
