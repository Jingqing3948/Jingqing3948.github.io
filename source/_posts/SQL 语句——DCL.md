---
title: SQL 语句——DCL
date: 2022-04-26
tags: study
category: database
---

学习于：b站 骆昊jackfrued 老师的网课

Data Control Language，给用户授权，不同用户能操作数据权限也是不一样的。

```mysql
create user '人名'@'域名' identified by '密码';-- 添加一个用户，并且限制这个人只能通过这个域名访问
alter user '人名'@'域名' identified by '密码';
drop user '人名'@'域名';
```

如果数据库本来就是在局域网里，那么即便改了权限，公网也是访问不到的。

授权：grant

```mysql
grant select on hrs.* to '人名'@'%';-- 这个人对 hrs 数据库下所有表都有查询权限，但是不能增删改
grant insert, delete, update on hrs.* to '人名'@'%';
grant all privileges on *.* to '人名'@'%';-- 这个人对所有数据库所有对象拥有所有权限
grant all privileges on *.* to '人名'@'%' with grant option;-- 这个人不仅有所有权限，还能授予权限给别人
```

召回权限：revoke

```mysql
revoke select on hrs.* to '人名'@'%';
```

添加权限之后最好 Reconnect 刷新一下。或者执行：

```mysql
flush privileges;
```

Workbench - Administration - Management - Users and privileges - 选中某个用户 - Schema Privileges，可以查看这个人的权限。

至此，SQL 语句基本内容就已经学完啦~接下来涉及 Python 数据持久化的内容。
