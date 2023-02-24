---
title: "linux实用工具"
date: 2022-10-28T16:52:09+08:00
description: "linux实用工具"
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
### 实用工具
```
autojump：一键直达
yum install autojump

ag：比grep、ack更快的递归搜索文件内容
yum install the_silver_searcher

axel：多线程下载工具，下载文件时可以替代curl、wget
yum install axel

script：记录会话输出

htop: 提供更美观、更方便的进程监控工具，替代top命令
yum install htop

thefuck：用途是每次命令行打错了以后，打一句fuck就会自动更正命令。比如apt-get打成了aptget。fuck以后自动变成apt-get。但还是没加sudo。再fuck，成功
pip install thefuck

you-get: 非常强大的媒体下载工具，支持youtube、google+、优酷、芒果TV、腾讯视频、秒拍等视频下载
pip3 install you-get

tldr: 如果你经常不想详读man文档，那么你应该试试这个小工具，有很多种语言版本
pip install tldr
cheat: 号称比tldr强大
curl cht.sh/<cmd>

rlwrap：历史命令
yum install rlwrap

tig：字符模式下交互查看git项目，可以替代git命令
yum install tig

mycli：mysql客户端，支持语法高亮和命令补全，效果类似ipython，可以替代mysql命令；类似的有pgcli；由于关系到帐号密码，谨用
pip install mycli pgcli

shellcheck：shell脚本静态检查工具，能够识别语法错误以及不规范的写法
yum install ShellCheck

yapf：Google开发的python代码格式规范化工具，支持pep8以及Google代码风格
pip install yapf

mosh：基于UDP的终端连接，可以替代ssh，连接更稳定，即使IP变了，也能自动重连
yum install mosh

fzf：命令行下模糊搜索工具，能够交互式智能搜索并选取文件或者内容，配合终端ctrl-r历史命令搜索简直完美
https://github.com/junegunn/fzf

cloc：代码统计工具，能够统计代码的空行数、注释行、编程语言
yum install cloc

ccache：高速C/C++编译缓存工具，反复编译内核非常有用。使用起来也非常方便
yum install ccache
```