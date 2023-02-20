---
title: "设置dns"
date: 2022-10-28T16:52:09+08:00
description: "设置dns"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["linux"]
series: ["linux"]
categories: ["linux"]
image:
---
CetnOS

```
1、可以在/etc/sysconfig/network设置
RES_OPTIONS="rotate timeout:1 retries:1"

2、可以在/etc/resolv.conf设置
options rotate timeout:1 retries:1

3、可以通过命令nmcli设置
nmcli con mod eth0 +ipv4.dns-options rotate
```



