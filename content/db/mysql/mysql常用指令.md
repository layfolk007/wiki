---
title: "mysql常用指令"
date: 2023-02-14T15:12:53+08:00
description: "mysql常用指令"
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
+ 移除分区(数据不丢)
```
alter table car_gps_position remove partitioning;
```
+ 运行sql时临时关闭binlog
```
set sql_log_bin=0;
```
+ 生成修改视图definer的语句
```
select concat("alter DEFINER=`els_user`@`10.156.222.76` SQL SECURITY DEFINER VIEW ",TABLE_SCHEMA,".",TABLE_NAME," as ",VIEW_DEFINITION,";") from information_schema.VIEWS where DEFINER = 'els_user@%';
```
+ 查询字符集
```
select * from information_schema.tables where table_schema='els_v7_prod' and table_collation='utf8mb4_0900_ai_ci'
select * from information_schema.columns where table_schema='els_v7_prod' and collation_name='utf8mb4_0900_ai_ci'
```
+ 修改字符集
```
alter table test convert to character set utf8mb4 collate utf8mb4_general_ci 
```
+ 生成修改表字符集的语句
```
select distinct 
  concat("alter table `", table_name,"` convert to character set utf8mb4 collate utf8mb4_general_ci;") 
  as target_tables
  from information_schema.tables
  where table_schema="els_v7_prod"
  and table_collation='utf8mb4_0900_ai_ci'
```
+ 查询数据库大小
```
select concat((sum(DATA_LENGTH)+sum(INDEX_LENGTH))/(1024*1024),'MB') as data from information_schema.TABLES where TABLE_SCHEMA='dbname'
```
+ mysqldump只保存表结构
```
mysqldump -uroot -p -h127.0.0.1 -d els_dev_v2 common_card_history --single-transaction --hex-blob --log-error=mysqldump.log -r 14516_common.sql
```
+ mysqldump只保存数据，如果删除旧表再创建则去掉-t
```
mysqldump -uroot -p -h127.0.0.1 -t els_dev_v2 common_card_history --single-transaction --hex-blob -w "car_id=14516" --log-error=mysqldump.log -r 14516_common.sql
```

+ 默认有--tz-utc参数，结果会有8小时偏差，使用--skip-tz-utc跳过
```
mysqldump -uroot -p -h127.0.0.1 -t els_v7_prod device_opreation_his_v7 --single-transaction --hex-blob --skip-tz-utc -w "opertor_time<'2021-5-1'" --log-error=mysqldump.log -r device_opreation_his_v7_old.sql
```
+ mydumper只保存表结构
```
#指定库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -B els_web_v3 -d -c -L log -o sbak
#所有库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -d -L log -o sbak
```
+ mydumper只保存数据，并排除部分表
```
#指定库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -B els_web_v3 -x '^(?!(els_web_v3.car_gps_position$|els_web_v3.els_car_gps_record$))' -m -c -L log -o dbak
#所有库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -x '^(?!(.*car_gps_position|.*els_car_gps_record))' -m -L log -o dbak
```
+ mydumper恢复
```
myloader -u user -p pass -h 127.0.0.1 -d now
```

+ 操作约束、列

```
添加主键约束：
alter table 表名 add constraint 主键 （形如：PK_表名） primary key 表名(主键字段);
添加外键约束：
alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);
删除主键约束：
alter table 表名 drop primary key;
删除外键约束：
alter table 表名 drop foreign key 外键（区分大小写）;
修改表名：
alter table t_book rename to bbb;
添加列：
alter table 表名 add column 列名 varchar(30);
删除列：
alter table 表名 drop column 列名;
修改列名：
alter table bbb change nnnnn hh int;
修改列属性：
alter table t_book modify name varchar(22);
禁用外键约束：
SET FOREIGN_KEY_CHECKS=0;
启动外键约束：
SET FOREIGN_KEY_CHECKS=1;
```
+ mysql主备
```
grant replication slave on *.* to dbsync@'10.10.10.12%' identified by 'redhat'
change master to master_host='10.10.10.123',master_user='dbsync',master_password='redhat',master_log_file='mybinlog.000001',master_log_pos=120
change master to master_host='10.10.10.122',master_user='dbsync',master_password='redhat',master_log_file='mybinlog.000001',master_log_pos=333
```
+ 复制表结构
```
# AUTO_INCREMENT会丢失
create table t1 select * from t where 1=2   (不会复制key)
create table t1 like t

# 使用show create table查看建表语句，推荐

# 重命名
rename table t to t2, t1 to t
```

+ 如何在删除ibdata1和ib_logfile的情况下恢复MySQL数据库(不一定成功)
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
+ MySQL使用LVM快照实现备份
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