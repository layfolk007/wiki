---
title: "weblogic"
date: 2022-10-28T16:52:09+08:00
description: "weblogic"
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
启动managed server时报错:outofmemory error  perm size

控制台修改启动参数
```
-Xms1024m -Xmx1024m -XX:MaxPermSize=512m -Djava.awt.headless=true
```