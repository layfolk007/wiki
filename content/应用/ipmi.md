---
title: "ipmi"
date: 2022-10-28T16:52:09+08:00
description: "ipmi"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["应用"]
series: ["应用"]
categories: ["应用"]
image:
---
```
安装
yum install ipmitool OpenIPMI

查看ipmi信息
ipmitool mc info
查看管理卡信息
ipmitool lan print
列出当前用户
ipmitool user list 1
修改第二用户密码
ipmitool user set password 2 111111
网络类型
ipmitool lan set 1 ipsrc static
配置网关
ipmitool lan set 1 defgw ipaddr 192.168.1.1
配置IP
ipmitool lan set 1 ipaddr 192.168.1.70
配置掩码
ipmitool lan set 1 netmask 255.255.255.0

######
ipmitool -I lanplus -U root -H 192.168.113.11 power reset
ipmitool -I lanplus -U root -H 192.168.113.11 sol activate (无输出，没有设置串口重定向到com2)
```

windows无法使用[ipmitool](https://github.com/ipmitool/ipmitool)调本地接口，只能远程，使用[ipmiutil](http://ipmiutil.sourceforge.net/)时，有的服务器出现微软ipmidrv.sys有问题\(可能是硬件驱动未安装，不确定\)，不是running状态，其他驱动未测试

想在esxi上使用ipmitool，可以在centos上自己编译，再复制到esxi上运行

```
./configure LDFLAGS=-static  (此命令后面可以不用加)
make LDFLAGS=-all-static CFLAGS=-static
```



