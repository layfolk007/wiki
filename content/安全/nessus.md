---
title: "nessus无IP限制"
date: 2022-11-04T11:22:40+08:00
description: nessus无IP限制
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: []
series:
categories: []
image:
---
[激活码](https://www.tenable.com/products/nessus/activation-code)、[插件下载](https://plugins.nessus.org/v2/offline.php)  
1、从第2个页面下载all-2.0.tar.gz  
2、初始化
```
2.1、设置Managed Scanner->Tenable.sc
2.1、设置帐号密码
```
3、关闭nessusd服务  
4、导入插件  
`/opt/nessus/sbin/nessuscli update /yourpath/all-2.0.tar.gz`  
5、创建文件并根据4返回的数字串设置PLUGIN_SET
```
/opt/nessus/var/nessus/plugin_feed_info.inc
PLUGIN_SET = "202204151200";
PLUGIN_FEED = "ProfessionalFeed (Direct)";
PLUGIN_FEED_TRANSPORT = "Tenable Network Security Lightning";
设置文件只读chattr +i plugin_feed_info.inc
```
6、设置/opt/nessus/lib/nessus/plugins下的文件只读
```
chattr +i /opt/nessus/lib/nessus/plugins/*  # 或使用find方式
find /opt/nessus/lib/nessus/plugins -name "*.*" -exec chattr +i {} \;
```
7、启动nessusd服务