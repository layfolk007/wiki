---
title: "mysql"
date: 2023-02-14T15:12:53+08:00
description: "mysql"
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
# mysql

**移除分区\(数据不丢\)**

```
alter table car_gps_position remove partitioning;
```

**运行sql时临时关闭binlog**

```
set sql_log_bin=0;
```

[mysql备份工具所需要的权限](https://blog.51cto.com/l0vesql/2062630)、[mysql物理复制表](https://www.cnblogs.com/gjc592/p/9257613.html)、[mysql optimiz](https://blog.51cto.com/dadaman/1957229)

[mysql内存说明](https://www.cnblogs.com/zengkefu/p/5685515.html)

[mysql优化](https://www.cnblogs.com/zjxiang/p/9123190.html)、[多表查询](http://www.zsythink.net/archives/1105/)、[索引解析](https://www.cnblogs.com/kkbill/p/11354685.html)

[mysql定义者和安全性definer/invoker](https://blog.csdn.net/michaelwoshi/article/details/104034670)

    # 生成修改视图definer的语句
    select concat("alter DEFINER=`els_user`@`10.156.222.76` SQL SECURITY DEFINER VIEW ",TABLE_SCHEMA,".",TABLE_NAME," as ",VIEW_DEFINITION,";") from information_schema.VIEWS where DEFINER = 'els_user@%';
    
    # 查询字符集
    select * from information_schema.tables where table_schema='els_v7_prod' and table_collation='utf8mb4_0900_ai_ci'
    select * from information_schema.columns where table_schema='els_v7_prod' and collation_name='utf8mb4_0900_ai_ci'
    # 修改字符集
    alter table test convert to character set utf8mb4 collate utf8mb4_general_ci 
    # 生成修改表字符集的语句
    select distinct 
    concat("alter table `", table_name,"` convert to character set utf8mb4 collate utf8mb4_general_ci;") 
    as target_tables
    from information_schema.tables
    where table_schema="els_v7_prod"
    and table_collation='utf8mb4_0900_ai_ci'

[在线DDL工具](https://www.cnblogs.com/zhoujinyi/p/9187421.html)

[部署工具](https://github.com/datacharmer/dbdeployer)

