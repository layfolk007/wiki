---
title: "sftp问题"
date: 2022-11-07T17:56:47+08:00
description:
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
```
openssh8.2及之后版本
原因：目标服务器ssh版本比堡垒机或客户端版本高导致

在目标端配置：
sshd_config 文件 可以在最下方 配置
KexAlgorithms diffie-hellman-group1-sha1,diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group-exchange-sha256,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group1-sha1,curve25519-sha256@libssh.org
```