---
title: "svn"
date: 2022-10-28T16:52:09+08:00
description: "svn"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["scm"]
series: ["scm"]
categories: ["scm"]
image:
---
[visualsvn证书问题](https://www.visualsvn.com/support/topic/00056/)

### subversion+httpd
服务器要通过http，https访问svn则需要安装serf。如果只通过svn则可以不安装，用yum安装httpd，apr等。
1. httpd
```
./configure --prefix=/webserver/httpd --sysconfdir=/webserver/httpd/conf --enable-dav --enable-so --enable-rewirte --enable-ssl --enable-cgi --enable-cgid --enable-modules=most --enable-modules-shared=most --enable-mpms-shared=all --with-mpm=worker --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr
```
2. serf
2.1、当版本为1.2.1时
```
./configure --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr --with-openssl=/usr/include/openssl
```
2.2、当版本为1.3之后，安装scons后，在serf目录运行以下命令
```
scons PREFIX=/usr/local/serf APR=/usr/local/apr APU=/usr/local/apr OPENSSL=/usr/bin
scons install
```
3. subversion1.8.9
```
./configure --with-apxs=/usr/sbin/apxs --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr --with-serf=/usr/local/serf --with-openssl --with-zlib --enable-maintainer-mode --enable-mod-activation
```

### linux中subversion客户端，密码加密方式
1. gnome-keyring    gnome环境使用，需要安装subversion-gnome
2. kwallet        kde环境使用
3. gpg-agent        需要安装gnupg，还要设置环境变量，可source gpg-agent.bashrc
```
#!/bin/bash
#gpg-agent需要安装gnupg

if ps aux|grep gpg-[a]gent &> /dev/null
then
  echo 'gpg-agent is running...'
else
  #gpg-agent --enable-ssh-support --daemon --write-env-file ${HOME}/.gpg-agent-info "$@"
  gpg-agent --daemon --write-env-file ${HOME}/.gpg-agent-info "$@"
fi

if [ -f "${HOME}/.gpg-agent-info" ]; then
  . "${HOME}/.gpg-agent-info"
  export GPG_AGENT_INFO
  #export SSH_AUTH_SOCK
  #export SSH_AGENT_PID
fi
```

### svn瘦身
SVN版本库位置：D:\SVNRepository，占地3.34G

步骤：
1. 准备工作，打开命令行提示符，输入命令： `svnlook youngest d:\SVNRepository`，查看当前最新的版本号，显示最新版本记录为755。
2. 备份版本库（很重要，以免过程中出现意外而导致数据丢失或者版本库损坏），把D盘的版本库，备份到C盘，同时清除历史日志，输入命令：`svnadmin hotcopy --clean-logs d:\SVNRepository c:\SVNRepository`，这样备份后版本库从3.34G为3.24G。（这里可选择是否清除历史日志）
3. dump需要保留的版本，选择保留700-755的版本，输入：`svnadmin dump c:\SVNRepository -r 745:755 > d:\repo_dump_745_755.dmp`。
4. 删除就版本库（这一步是供选择，不删除亦无碍），输入命令：`rmdir /s/q d:\SVNRepository`，删除旧版本库。也可以直接在资源管理器里删除。
5. 创建空的版本库，输入命令：`svnadmin create d:/SVNRepository`，检查空的版本库大概31.2K大小。
6. 把dump文件导入版本库，输入命令：`svnadmin load d:\SVNRepository < d:\repo_dump_745_755.dmp`，这时屏幕上会显示正在载入版本库中的文件或正在提交/装载的版本。完成后，用命令`svnlook youngest d:\SVNRepository`查看，显示当前版本库最新版本号是11，整个版本库大小501M。至此，SVN版本库瘦身成功，腾出空间2.7G，大致相当于腾出原SVN库近5/6的空间！