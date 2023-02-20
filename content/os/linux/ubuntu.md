---
title: "ubuntu"
date: 2022-10-28T16:52:09+08:00
description: "ubuntu"
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
apt-fast 是一款替代 apt-get 提升下载速度的软件，安装软件时，通过增加线程使下载软件速度加快

```
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get install apt-fast
sudo apt-fast update
```



