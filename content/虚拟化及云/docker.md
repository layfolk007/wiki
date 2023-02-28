---
title: "docker"
date: 2022-10-28T16:52:09+08:00
description: "docker"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["虚拟化及云"]
series: ["虚拟化及云"]
categories: ["虚拟化及云"]
image:
---
[同时运行多个进程](https://cloud.tencent.com/developer/article/1683445)、[mosquitto](http://blog.itpub.net/28624388/viewspace-1439881/)  
[docker实战里的案例](https://github.com/docker-in-practice)

## 查询docker images 的tag
```
curl -s https://registry.hub.docker.com/v2/repositories/ceph/daemon/tags/ | jq '."results"[] .name'
```
[分页](https://registry.hub.docker.com/v2/repositories/ceph/daemon/tags/?page=2)

## docker registry(带用户登录功能)  
[Portus](https://github.com/SUSE/Portus)(用examples里的compose可以部署)  
[harbor](https://github.com/vmware/harbor)(下载release里的部署包)

## docker迁移
主要的区别是：save是可以回滚以前的配置，export只是当前的。
1. save命令用于持久化镜像（不是容器），保存所有历史和层
```
-----save镜像：
docker save IMAGENAME | bzip2 -9 -c > img.tbz2
```
```
-----load镜像：
cat img.tbz2 | docker load  或 docker load -i img.tbz2
```
2. export命令用于持久化容器（不是镜像），只保存当前状态，会丢失所有的历史，导入时可设置tag
```
-----export容器：
docker export CONTAINER | bzip2 -9 -c > c.tbz2
```
```
-----import容器：
cat c.tbz2 | docker import - xxx:yyy
```

## 注意
+ 使用docker的服务器上注意iptables不能随便重启，会打乱docker的规则(都是临时的，并没有保存)让网络失效(安装wdcp时遇到)
