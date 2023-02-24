---
title: "添加新数据库到MySQL主从复制列表"
date: 2023-02-14T15:12:53+08:00
description: "添加新数据库到MySQL主从复制列表"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["mysql"]
series: ["mysql"]
categories: ["db","mysql"]
image:
---
MySQL主从复制有时会设置需要同步的数据库，使用参数配置选项binlog-do-db，可以在master上指定需要同步的数据库，replicate-do-db在从数据看上指定需要同步的数据库。（一般只设定master上的binlog-do-db即可，不需要两个同时设定。以防万一，在slave也可以加上replicate-ignore-db）

问题是，在master上面新增了一个数据库，这个时候如何把新加的这个数据库添加到MySQL的主从复制链里？（即不重新复制整个库的情况下，重新设置主从复制）

### 1. 从服务上，停掉slave数据库
```
stop slave;
```
### 2. 主服务器上，导出新数据库
```
mysqldump --master-data --single-transaction -R --databases newdb > newdb.sql
```
### 3. 修改主服务器my.cnf文件
主服务器上，修改my.cnf文件，添加新库到binlog-do-db参数，重启mysql。从设置replicate-do-db
### 4. 查找当前的日志文件以及位置
在导出的newdb.sql里面查找当前的日志文件以及位置（change master to …\) 然后让slave服务器执行到这个位置
```
start slave until MASTER_LOG_FILE="mysql-bin.000001", MASTER_LOG_POS=1222220;
```
其中MASTER\_LOG\_FILE以及MASTER\_LOG\_POS在导出的数据库newdb.sql顶部位置查找
### 5. 导入新库到从服务器上
```
mysql < newdb.sql
```
### 6. 启动从服务器
```
start slave
```
其中比较重要的是在主服务器上导出新库时的日志位置（position A），这个点很重要，以这个点做为分界线，导入新库。这种方法也同样适用于某个数据库或者某个数据表不同步的情况，比如主从数据库有一个表由于某些原因数据不一致，那么上面的方法只需要去掉重启数据库一步，其他的操作基本都是相同的