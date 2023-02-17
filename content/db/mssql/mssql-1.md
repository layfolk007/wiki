---
title: "mssql授权"
date: 2023-02-14T15:12:53+08:00
description: "mssql授权"
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
**方法一**

```
create database test
go
use test
go
execute sp_addlogin 'redhat','_red_hat38','test'
go
execute sp_grantdbaccess 'redhat','redhat'
go
execute sp_addrolemember 'db_owner','redhat'
```

**方法二**

```
create database test
go
use test
go
create login redhat with password='_red_hat38', default_database=test
go
create user redhat for login redhat with default_schema=dbo
go
exec sp_addrolemember 'db_owner', 'redhat'
```

**第二个库授权**

```
use mydb2
go
create user redhat for login redhat with default_schema=dbo
go
exec sp_addrolemember 'db_owner', 'redhat'
```

**去掉看到所有库**

```
use master
go
deny view any database to public
go
alter authorization on DATABASE::test to redhat
```

**查看权限**

```
use db
go
exec sp_helprotect @username='redhat'
```

**存储过程**

```
EXEC sp_MSforeachtable @command1='alter table ? nocheck constraint all'
```
