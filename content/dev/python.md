---
title: "python"
date: 2023-02-14T15:12:53+08:00
description: "python"
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
**Python 生成项目的 requirements.txt 文件**
1、pip freeze(它生成的**requirements.txt**文件包含当前环境的完全列表，不相关的依赖包也会包含进来)
2、pipreqs(只会包含项目import的包，包含列表不是很完全)
```
pip install pipreqs
pipreqs --force <project-path>
```
3、pigar(输出信息比pipreqs详细)
```
pip install pigar
```