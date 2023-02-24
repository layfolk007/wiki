---
title: "windows常用"
date: 2022-10-28T16:52:09+08:00
description: "windows常用"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["windows"]
series: ["windows"]
categories: ["windows"]
image:
---
### windows server 2012 r2 standard 135,137,138,139,445等端口关闭  
找到两种方法：  
1. 防火墙  
2. ip策略

### bitlocker
`加锁:manage-bde -lock -forcedismount G:`

### 远程桌面
1. 修改连接数
安装远程桌面会话主机，再修改数量
2. 修改端口
```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
```
两项下面的PortNumber
3. 历史清除
+ 删除我的文档里的隐藏文件Default.rdp
+ 删除注册表HKCU\Software\Microsoft\Terminal Server Client下的相关记录

### powershell、cmd历史命令删除
```
---powershell
Get-Content (Get-PSReadlineOption).HistorySavePath
Remove-Item (Get-PSReadlineOption).HistorySavePath

---cmd
doskey /h
doskey /reinstall
```

### windows补丁相关命令
```
wuauclt.exe /resetauthorization /detectnow
dism /online /add-package /packagepath:D:\kb4016252-x64.cab
wusa /quiet /norestart D:\kb4016252-x64.msu
```