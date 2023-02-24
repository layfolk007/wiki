---
title: "pgsql"
date: 2023-02-14T15:12:53+08:00
description: "pgsql"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["postgresql"]
series: ["postgresql"]
categories: ["db","postgresql"]
image:
---
**导出**
```
# 有\和没有是有区别的，带\的普通用户可执行
\COPY (select * from test) TO 'a.csv' csv header
```