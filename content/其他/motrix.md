---
title: "Motrix"
date: 2023-02-15T11:17:45+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: []
series:
categories: []
image:
---
其会调用aria2下载，可能会导致下载无速度且一段时间后报错#19或下载错误等(ctrl+shift+i可调出开发者工具查看)，原因为aria2的异步dns问题，可在配置文件中加`async-dns=false`参数关闭