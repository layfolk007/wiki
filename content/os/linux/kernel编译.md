---
title: "kernel编译"
date: 2022-10-28T16:52:09+08:00
description: 简述kernel的编译步骤
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["linux"]
series:
categories: ["linux"]
image:
---

以centos7为例
```shell
1、安装依赖
gcc openssl-devel elfutils-libelf-devel rsync
如果提示gcc版本低，可安装启用scl的devtoolset

2、配置(使用当前系统的/boot/config-xxxx作为模板并添加新内核的默认设置)
yes '' | make oldconfig

3、编译为rpm
make -j`nproc` INSTALL_MOD_STRIP=1 rpm-pkg
```
