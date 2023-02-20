---
title: "linux-1"
date: 2022-10-28T16:52:09+08:00
description: "linux-1"
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
### curl返回状态码

```
curl -I -m 10 -o /dev/null -s -w %{http_code} url
```

### 端口重定向

```
iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
```

### centos图形terminal

当图形terminal的字很难看时，检查是否安装了dejavu相关字体

```
yum install dejavu*
```

### **特殊权限**

管理员也无法删除的文件

修改权限chattr + -

参数  i 不可修改，不可删除；a 只可追加

查看lsattr

### **shell脚本中获取source和.的脚本路径**

1. bash中有BASH\_SOURCE变量
2. csh/tcsh

```
/usr/sbin/lsof -p $$|grep -oE "/.*/xxxxx"
```

### **\_变量**

1. csh/tcsh 中记录上次执行的命令
2. bash 中记录上次执行命令的最后一个参数

### **更灵活的Linux Shell历史命令调用方法**

除了使用history命令查看历史命令外，Linux系统还提供了非常灵活的Shell历史命令调用方法，我们可以在Shell命令提示符或者Shell脚本中使用它们：

```
　　!!　　　　前一条命令；
　　!:0 　　　不带参数的前一条命令名；
　　!^ 　　　前一条命令的第一个参数；
　　!:n 　　　前一条命令的第n个参数；
　　!$ 　　　 前一条命令的最后一个参数；
　　!* 　　　 前一条命令的所有参数，命令名除外；
　　!n 　　　 第n条命令；
　　!-n 　　　倒数第n条命令；
　　!str　　　 最近一条以str开头的命令；
　　!?str　 　 最近一条包含str的命令；
　　^a^b　　将上一条命令名中的a替换为b；
　　!:gs/a/b　将上一条命令的所有a替换为b（包含命令名和参数）。
```

dump

restore

dump2fs /dev/sda5 查看super block信息

[ext3 superblock故障](http://www.cppblog.com/dancefire/archive/2011/03/09/fix-bad-superblock-in-linux.html)

reiserFS/JFS文件系统

让linux死机的代码

```
#include<unistd.h>
int main(void)
{
        while(1)
       {
               fork();
       }

        return 0;
}
```



