**windows server 2012 r2 standard 135,137,138,139,445等端口关闭**  
找到两种方法：  
1、防火墙  
2、ip策略

**bitlocker**

加锁:manage-bde -lock -forcedismount G:

**远程桌面**

1.修改连接数

安装远程桌面会话主机，再修改数量

2.修改端口

HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\Wds\rdpwd\Tds\tcp

HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp

两项下面的PortNumber

3.历史清除

1. 删除我的文档里的隐藏文件Default.rdp
2. 删除注册表HKCU\Software\Microsoft\Terminal Server Client下的相关记录

**powershell、cmd历史命令删除**

```
---powershell
Get-Content (Get-PSReadlineOption).HistorySavePath
Remove-Item (Get-PSReadlineOption).HistorySavePath

---cmd
doskey /h
doskey /reinstall
```

**包管理**

1. [chocolatey](https://github.com/chocolatey/choco)
2. [scoop](https://github.com/lukesampson/scoop)
3. [baulk](https://gitee.com/ipvb/baulk)

[**openssh**](https://github.com/PowerShell/openssh-portable)、微软[PowerToys](https://github.com/microsoft/PowerToys)小工具集合

[win10的Hyper-V与VirtualBox、VMware冲突导致蓝屏的解决方法](https://blog.csdn.net/qwsamxy/article/details/50533007/)

[win10](https://www.nsaneforums.com/topic/312871-windows-10-digital-license-hwid-kms38™-generation)

windows补丁相关命令

```
wuauclt.exe /resetauthorization /detectnow
dism /online /add-package /packagepath:D:\kb4016252-x64.cab
wusa /quiet /norestart D:\kb4016252-x64.msu
```

使用[winsw](https://github.com/kohsuke/winsw)创建服务

[WindowsAdminCenter下载](https://aka.ms/WACDownload)