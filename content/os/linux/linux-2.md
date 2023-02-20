---
title: "linux-2"
date: 2022-10-28T16:52:09+08:00
description: "linux-2"
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
1、Debian/Ubuntu：

```
$ sudo echo "loop" >> /etc/modules
```

2、CentOS/Redhat/Fedora：

```
$ sudo echo "modprobe loop" >> /etc/rc.modules
$ sudo chmod +x /etc/rc.modules
```

或者

```
$ sudo echo "modprobe loop" >> /etc/sysconfig/modules/loop.modules
$ sudo chmod +x /etc/sysconfig/modules/loop.modules
```

/etc/rc.modules 和/etc/sysconfig/modules/\*.modules 都是在 rc.sysinit 中被执行。所以，直接将 modprobe 指令写到 rc.sysinit 中也是可以的。这几个位置加载的时间都要比rc.local早。在CentOS7中，尽管在systemd里面已经没有 rc.sysinit，仍然兼容上面两种方式加载模块。

3、systemd（CentOS7/Redhat7/Fedora） 和upstart （Debian/Ubuntu）：

```
在下列 module-load.d 目录（之一）创建一个 *.conf 文件，将模块的名字写入该文件即可。
/etc/modules-load.d/
/lib/modules-load.d/
/run/modules-load.d/
/usr/lib/modules-load.d/
/usr/local/lib/modules-load.d/
例如：
$ sudo echo "loop" > /etc/modules-load.d/loop.conf
```

4、[redhat/centos软件集SCL\(一些软件的新版本\)](https://wiki.centos.org/AdditionalResources/Repositories/SCL)

5、centos6.x以下升级到centos7.2

```
cat <<EOF >/etc/yum.repos.d/upgradetool.repo
[upg]
name=CentOS-$releasever - Upgrade Tool
#baseurl=http://buildlogs.centos.org/centos/6/upg/x86_64
baseurl=http://dev.centos.org/centos/6/upg/x86_64/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
EOF

yum -y install redhat-upgrade-tool preupgrade-assistant-contents
```

升级前可行性分析

preupg -l   \# 列出预升级的可用内容，多半是"CentOS6\_7"

preupg -s CentOS6\_7   \# 这里的"CentOS6\_7"是上个命令的输出

\# 上面这个命令生成的报告需要看看，主要是关于升级的风险的，升级前尽量将非官方的rpm安装的软件都删掉，安装的第三方的rpm包越少，升级的风险越小

问题：

```
I/O warning : failed to load external entity "/usr/share/openscap/xsl/security-guide.xsl"
......
OpenSCAP Error:: Could not parse XSLT file '/usr/share/preupgrade/xsl/preup.xsl' [oscapxml.c:416]
......
```

此错误为openscap 软件包的版本过高，可安装低版本后再重新安装preupg：

```
yum erase openscap
yum install http://dev.centos.org/centos/6/upg/x86_64/Packages/openscap-1.0.8-1.0.1.el6.centos.x86_64.rpm
yum install redhat-upgrade-tool preupgrade-assistant-contents
```

开始升级\(\#需要注意的是，这个镜像目录下需要有文件.treeinfo，且只有7.2.1511的内容才符合要求\)

```
rpm --import http://mirrors.aliyun.com/centos-vault/7.2.1511/os/x86_64/RPM-GPG-KEY-CentOS-7
centos-upgrade-tool --network 7 \
     --instrepo=http://mirrors.aliyun.com/centos-vault/7.2.1511/os/x86_64/
或
centos-upgrade-tool --iso xxx.iso
```



