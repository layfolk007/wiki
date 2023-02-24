---
title: "提升效率的bash快捷键"
date: 2022-10-28T16:52:09+08:00
description: "提升效率的bash快捷键"
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
### 1、编辑命令
* Ctrl + a ：移到命令行首 
* Ctrl + e ：移到命令行尾 
* Ctrl + f ：按字符前移（右向） 
* Ctrl + b ：按字符后移（左向） 
* Alt + f ：按单词前移（右向） 
* Alt + b ：按单词后移（左向） 
* Ctrl + xx：在命令行首和光标之间移动 
* Ctrl + u ：从光标处删除至命令行首 
* Ctrl + k ：从光标处删除至命令行尾 
* Ctrl + w ：从光标处删除至字首 
* Alt + d ：从光标处删除至字尾 
* Ctrl + d ：删除光标处的字符 
* Ctrl + h ：删除光标前的字符 
* Ctrl + y ：粘贴至光标后 
* Alt + c ：从光标处更改为首字母大写的单词 
* Alt + u ：从光标处更改为全部大写的单词 
* Alt + l ：从光标处更改为全部小写的单词 
* Ctrl + t ：交换光标处和之前的字符 
* Alt + t ：交换光标处和之前的单词 
* Alt + Backspace：与 Ctrl + w 相同类似，分隔符有些差别

### 2、重新执行命令
* Ctrl + r：逆向搜索命令历史 
* Ctrl + g：从历史搜索模式退出 
* Ctrl + p：历史中的上一条命令 
* Ctrl + n：历史中的下一条命令 
* Alt + .：使用上一条命令的最后一个参数 

### 3、控制命令
* Ctrl + l：清屏 
* Ctrl + o：执行当前命令，并选择上一条命令 
* Ctrl + s：阻止屏幕输出 
* Ctrl + q：允许屏幕输出 
* Ctrl + c：终止命令 
* Ctrl + z：挂起命令 

### 4、Bang(!)命令
* !!：执行上一条命令
* !:0：不带参数的前一条命令
* !^：前一条命令的第一个参数
* !:n：前一条命令的第n个参数
* !blah：执行最近的以blah开头的命令，如 !ls
* !blah:p：仅打印输出，而不执行
* !?blah：最近一条包含blah的命令
* !$：上一条命令的最后一个参数，与 Alt + . 相同
* !$:p：打印输出 !$ 的内容
* !*：上一条命令的所有参数
* !*:p：打印输出 !\* 的内容
* ^blah：删除上一条命令中的blah
* ^blah^foo：将上一条命令中的blah替换为foo
* ^blah^foo^：将上一条命令中所有的blah都替换为foo
* !:gs/blah/foo：将上一条命令的所有blah替换为foo（包含命令名和参数）
* !n：第n条命令
* !-n：倒数第n条命令

### 友情提示
1. 以上介绍的大多数 Bash 快捷键仅当在 emacs 编辑模式时有效，若你将 Bash 配置为 vi 编辑模式，那将遵循 vi 的按键绑定。Bash 默认为 emacs 编辑模式。如果你的 Bash 不在 emacs 编辑模式，可通过 set -o emacs 设置。 
2. ^S、^Q、^C、^Z 是由终端设备处理的，可用 stty 命令设置。 
