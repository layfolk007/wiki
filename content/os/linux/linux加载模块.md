---
title: "linux加载模块"
date: 2022-10-28T16:52:09+08:00
description: "linux加载模块"
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
1、Debian/Ubuntu
```
$ sudo echo "loop" >> /etc/modules
```

2、CentOS/Redhat/Fedora
```
$ sudo echo "modprobe loop" >> /etc/rc.modules
$ sudo chmod +x /etc/rc.modules
```
或者
```
$ sudo echo "modprobe loop" >> /etc/sysconfig/modules/loop.modules
$ sudo chmod +x /etc/sysconfig/modules/loop.modules
```
/etc/rc.modules 和/etc/sysconfig/modules/*.modules 都是在rc.sysinit中被执行。所以，直接将modprobe指令写到rc.sysinit中也是可以的。这几个位置加载的时间都要比rc.local早。在CentOS7中，尽管在systemd里面已经没有rc.sysinit，仍然兼容上面两种方式加载模块。

3、systemd（CentOS7/Redhat7/Fedora）和upstart（Debian/Ubuntu）
```
在下列 module-load.d 目录（之一）创建一个 *.conf 文件，将模块的名字写入该文件即可。
/etc/modules-load.d/
/lib/modules-load.d/
/run/modules-load.d/
/usr/lib/modules-load.d/
/usr/local/lib/modules-load.d/
例如：
$ sudo echo "loop" > /etc/modules-load.d/loop.conf
```