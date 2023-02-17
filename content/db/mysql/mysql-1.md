---
title: "mysql-1"
date: 2023-02-14T15:12:53+08:00
description: "mysql-1"
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
[mysql升级](https://my.oschina.net/u/3761438/blog/1843753)  
[mysql/java对emoji的支持](https://segmentfault.com/a/1190000000616820)  
[Linux高可用\(HA\)之Corosync+Pacemaker+DRBD+MySQL/MariaDB实现高可用](https://www.dwhd.org/20150530_014731.html)

**\#查询数据库大小**

```
select concat((sum(DATA_LENGTH)+sum(INDEX_LENGTH))/(1024*1024),'MB') as data from information_schema.TABLES where TABLE_SCHEMA='dbname'
```

**\#只保存表结构**

```
mysqldump -uroot -p -h127.0.0.1 -d els_dev_v2 common_card_history --single-transaction --hex-blob --log-error=mysqldump.log -r 14516_common.sql
```

**\#只保存数据，如果删除旧表再创建则去掉-t**

```
mysqldump -uroot -p -h127.0.0.1 -t els_dev_v2 common_card_history --single-transaction --hex-blob -w "car_id=14516" --log-error=mysqldump.log -r 14516_common.sql
```

```
默认有--tz-utc参数，结果会有8小时偏差，使用--skip-tz-utc跳过

mysqldump -uroot -p -h127.0.0.1 -t els_v7_prod device_opreation_his_v7 --single-transaction --hex-blob --skip-tz-utc -w "opertor_time<'2021-5-1'" --log-error=mysqldump.log -r device_opreation_his_v7_old.sql
```

[mydumper](https://github.com/maxbube/mydumper)

**\#只保存表结构**

```
#指定库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -B els_web_v3 -d -c -L log -o sbak
#所有库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -d -L log -o sbak
```

**\#只保存数据，并排除部分表**

```
#指定库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -B els_web_v3 -x '^(?!(els_web_v3.car_gps_position$|els_web_v3.els_car_gps_record$))' -m -c -L log -o dbak
#所有库
mydumper -u user -p pass -h 127.0.0.1 --less-locking -x '^(?!(.*car_gps_position|.*els_car_gps_record))' -m -L log -o dbak
```

```
myloader -u user -p pass -h 127.0.0.1 -d now
```

**mysql增量备份**

[mysql读写分离、分库分表、分区](http://blog.csdn.net/tjcyjd/article/details/11194489)

[mysql表空间迁移](http://www.ttlsa.com/mysql/mysql-backup-recovery-innodb-table/)     [-2-](https://www.cnblogs.com/gjc592/p/9257613.html)

**percona-toolkit**

**percona-xtrabackup**

[percona-release](https://www.percona.com/downloads/percona-release/redhat/latest/)

**MySQL 查看约束，添加约束，删除约束 添加列，修改列，删除列**

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

**mysql主备**

```
grant replication slave on *.* to dbsync@'10.10.10.12%' identified by 'redhat'

change master to master_host='10.10.10.123',master_user='dbsync',master_password='redhat',master_log_file='mybinlog.000001',master_log_pos=120

change master to master_host='10.10.10.122',master_user='dbsync',master_password='redhat',master_log_file='mybinlog.000001',master_log_pos=333
```



