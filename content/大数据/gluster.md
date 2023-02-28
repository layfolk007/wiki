---
title: "glusterfs"
date: 2022-10-28T16:52:09+08:00
description: "glusterfs"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["glusterfs"]
series: ["glusterfs"]
categories: ["glusterfs"]
image:
---
[glusterfs分布式文件系统详细原理](http://blog.csdn.net/yujin2010good/article/details/75268877)、[启用ssl](https://cloud.tencent.com/developer/article/1151958)
1. 安装
```
yum install centos-release-glusterXXX
yum install glusterfs-server
```
2. 启动服务
```
/etc/init.d/glusterd start
```
3. cluster管理
```
gluster peer probe host|ip
gluster peer status #查看除本机外的其他设备状态
gluster peer detach host|ip #如果希望将某设备从存储池中删除
```
4. volume管理  
卷类型：distributed、replicated、striped、distribute replication、distribute striped、striped replicated、distributed striped replicated
```
gluster volume create vname node1:/media node2:/media node3:/media
```
5. 启/停/删除卷
```
gluster volume start vname
gluster volume stop vname
gluster volume delete vname
```
6. 挂载
```
mount.glusterfs host|ip:/vname /dir
```

报错：volume create: dbbak: failed: /home/mysql_bak is already part of a volume
```
for i in `attr -lq .`; do setfattr -x trusted.$i .; done 
```