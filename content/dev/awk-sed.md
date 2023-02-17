---
title: "awk和sed"
date: 2023-02-14T15:12:53+08:00
description: "awk和sed"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["dev"]
series: ["dev"]
categories: ["dev"]
image:
---
```
ps aux|grep java|grep -v grep|awk '{out=$2;for(i=11;i<=NF;i++){out=out" "$i};print out"\n"}'
```



