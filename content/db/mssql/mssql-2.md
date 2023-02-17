---
title: "mssql用户问题"
date: 2023-02-14T15:12:53+08:00
description: "mssql用户问题"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: []
series:
categories: ["mssql"]
image:
---
SQL Server2005中已有登录名A，附加某数据库后，该数据库中也有用户名A，但是无法通过登录名A访问该数据库。

需要在SQL Server中设置登录名与数据库用户名的映射，在登录名---属性中，但是提示“用户、组或角色 在当前数据库中已存在”。

如果反过程先附加数据库，然后添加登录名会出现同样问题，引发问题的原因是“存在孤立用户”。

创建用户映射的方法为：

```
use 数据库
go--这个必须有，如果是SQL 语句就可以没有
sp_change_users_login 'update_one', '登录用户名', '用户名'
```

参数：

登录用户名  为  SQL Server 2005 中的登录名

用户名     为数据库中的用户名



sql server一些目录修改：


DefaultData			可在管理界面修改，也可以在注册表中修改

DefaultLog			可在管理界面修改，也可以在注册表中修改

BackupDirectory	       在注册表中修改