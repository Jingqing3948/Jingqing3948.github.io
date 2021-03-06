---
title: Mysql 数据类型
date: 2022-04-19
tags: study
category: database
---

数据类型可以通过 `? data types` 查看说明，每种数据类型也可以通过 `? 数据类型` 查看。

*除了data types 其他可以用?查看的内容可以输入 `? contents` 查看。*

| 数据类型                                            | 作用                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| 整数 (tinyint, smallint, mediumint, int, bigint)    | 都是整数型，表示范围为1,2,3,4,8 B（结尾还可以加 unsigned）   |
| 字符串 (char(位数), varchar(位数), text)            | char 长度固定，varchar 长度可变                              |
| 小数 (float, double, decimal(总位数, 小数点后位数)) | 不要用 float, double！                                       |
| 时间日期 (year, date, time, datetime, timestamp)    | date：年月日<br />time：时间<br />datetime：年月日时分秒<br />timestamp：时间戳，现在距离 1970-1-1 的毫秒数 |
| Mysql 中的枚举类型 (enum, boolean)                  |                                                              |

PS: 

1. 虽然 text 等类型可以存储很大的数据，不过大数据一般还是不会直接往数据库里存储。如图片，数据库中一般存储其路径、链接。如果非要存储，有 blob (binary large object)。

2. 金额等小数一般不会用小数的数据类型存储，而是整数类型/100使用。因为小数形式有误差，比如0.1+0.2=0.30000000004.

3. decimal 这种变长的数据，使用时速度比定长数据慢。而且 decimal 表示范围也有限，不如直接用 bigint。

4. 时间戳是有表示范围的，毕竟是个有上限的数，到2038年左右就不好用了。

   > 这里了解到了一个很有趣的[“千年虫”问题]([漫画：什么是“千年虫”问题？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/305603895))。
   >
   > Grace Murray Hopper，最早的现代编译器、商用编程语言发明者，Bug 和 Debug词汇的流行使用也与她有关。
   >
   > 早期计算机资源十分稀缺，内存空间必须精打细算。Grace Murray Hopper 采用6位数字组合来存储日期信息 （比如2022/04/19，就是22/04/19）
   >
   > 尽管节约了内存空间，但是40年后1999->2000年时，问题就出现了……对计算机来说，相当于99->00年，年份倒退了。小到银行存款利息变成负的，大到飞机、发电厂运作、核弹发射……都可能出现故障，后果不堪设想。
   >
   > 如果修改所有程序，是改不过来的。于是程序员们主要采取两种方法：
   >
   > 1. 只修改核心关键的医疗、航空、金融、军事领域的程序；
   > 2. 对于大多数不那么核心的程序，采用时间窗口的方式。1910年代表2010年，1920年代表2020年，暂缓问题。在这20年期间，大多数程序设备都已更新换代，现如今千年虫问题已经不那么严重了。

   回到刚才的话题，因此时间戳也并不推荐使用。

5.7 之后版本的 Mysql 支持 json 属性，以键值对的方式存储，内容相对灵活。因为虽然数据库结构相对严谨，但是很多时候并不是所有属性都能考虑得到（如二手交易平台，不同的售卖品属性差的很多，如自行车和冰箱）加入相对灵活的 json ，一定程度上就能解决这类问题。包括以前 Mysql 没有这个功能的时候，许多公司也会建一个 varchar() 字段来存储 json 字符串。
