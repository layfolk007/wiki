---
title: "linux常用"
date: 2022-10-28T16:52:09+08:00
description: "linux常用"
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
### centos7修改网卡名及禁用ipv6
1. 在/etc/default/grub里GRUB_CMDLINE_LINUX中添加参数ipv6.disable=1 net.ifnames=0 biosdevname=0并用grub2-mkconfig -o /boot/grub2/grub.cfg重启生成启动配置文件
2. 将/etc/sysconfig/network-scripts下的网卡配置文件改名为想要更改的名字，并将文件里的NAME和DEVICE也改为对应的名字
3. 重启系统

### systemd
```
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=root
PIDFile=/root/.vnc/%H:%i.pid
PAMName=login
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

### 占用swap最多的进程
```
for i in $(ls /proc | grep "^[0-9]"); do awk '/Swap:/{a=a+$2}END{print '"$i"',a/1024"M"}' /proc/$i/smaps;done| sort -k2nr | head
```

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

### 特殊权限
管理员也无法删除的文件 
修改权限chattr + - 
参数  i 不可修改，不可删除；a 只可追加 
查看lsattr

### shell脚本中获取source和.的脚本路径
1. bash中有BASH_SOURCE变量
2. csh/tcsh
```
/usr/sbin/lsof -p $$|grep -oE "/.*/xxxxx"
```

### _变量
1. csh/tcsh中记录上次执行的命令
2. bash中记录上次执行命令的最后一个参数

### 给yum添加变量，可以在repo仓库配置中调用，例：
```
cat /etc/redhat-release 
vim /etc/yum/vars/full_releasever 

# 查看所有可用变量
python -c 'import yum, pprint; yb = yum.YumBase();pprint.pprint(yb.conf.yumvar, width=1)'
```
### apt-fast
apt-fast是一款替代apt-get提升下载速度的软件，安装软件时，通过增加线程使下载软件速度加快
```
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get install apt-fast
sudo apt-fast update
```