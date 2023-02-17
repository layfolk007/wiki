---
title: "mysql-2"
date: 2023-02-14T15:12:53+08:00
description: "mysql-2"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["mysql"]
series: ["mysql"]
categories: ["mysql"]
image:
---
## 复制表结构

```
# AUTO_INCREMENT会丢失
create table t1 select * from t where 1=2   (不会复制key)
create table t1 like t

# 使用show create table查看建表语句，推荐

# 重命名
rename table t to t2, t1 to t
```

如何在删除ibdata1和ib\_logfile的情况下恢复MySQL数据库\(不一定成功\)

```
恢复的基本步骤
1. 将原来的数据文件COPY到其它目录下
2. 创建同名表，表结构必须保持一致
3. 导出表空间
mysql> ALTER TABLE t DISCARD TABLESPACE;
4. 将原来的数据文件COPY回来
5. 导入表空间
mysql> ALTER TABLE t IMPORT TABLESPACE;
```

MySQL使用LVM快照实现备份

```
1、锁表并备份相关信息
# mysql -e 'flush tables with read lock;' 锁表
# mysql -e 'flush logs;'     对日志进行滚动，
# mysql -e 'show master status;' > /root/back.$(date +%F+%T)
# ls
back.2016-07-13+10:14:29

2、创建快照，容量大小最好一样
lvcreate -L 1G -n mysqlback -p r -s /dev/mydata/mydatalv

3、解锁
mysql -e 'unlock tables;'

快照可挂载后复制，也可以直接用lvconvert --merge还原，但还原后会删除
```



