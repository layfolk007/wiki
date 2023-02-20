---
title: "wwn"
date: 2022-10-28T16:52:09+08:00
description: "wwn"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["os"]
series:
categories: ["os"]
image:
---
主机查看hba卡的wwn([1](https://forum.huawei.com/enterprise/zh/thread-763255.html), [2](https://www.cnblogs.com/linxizhifeng/p/11126812.html))

以下ps代码得到的是wwnn
```
Get-WmiObject -class MSFC_FCAdapterHBAAttributes -namespace "root\WMI" | ForEach-Object {(($_.NodeWWN) | ForEach-Object {"{0:x2}" -f $_}) -join ":"}
```
win2012及以后有自带ps命令Get-InitiatorPort