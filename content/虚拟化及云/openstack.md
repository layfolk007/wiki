---
title: "openstack"
date: 2022-10-28T16:52:09+08:00
description: "openstack"
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
[vmware虚拟机安装openstack无法启动实例](https://www.imooc.com/article/details/id/28873)
**openstack安装方法(juju、openstack-ansible、packstack、devstack、fuel)**

**maas conjure-up(juju)**  
两个网卡，均要能上外网，网卡名称为eno1，eno2或者修改bundle.yml后用juju deploy bundle.yml；还可以在conjure-up里直接修改neutron-gateway的配置

**vnc(juju安装，默认没有)**  
1、在nova-cloud-controller上安装nova-consoleauth、nova-novncproxy并启动  
2、在nova-compute上修改/etc/nova/nova.conf
```
vnc_enabled = True  
vncserver_listen=0.0.0.0  
vncserver_proxyclient_address=10.10.10.3 (nova-compute的内网ip)  
novncproxy_base_url=http://10.10.10.12:6080/vnc_auto.html (controller的外网ip，由于此处是lxd容器，只有一个网卡)
```