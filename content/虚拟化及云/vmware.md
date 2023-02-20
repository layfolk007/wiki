---
title: "vmware"
date: 2022-10-28T16:52:09+08:00
description: "vmware"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["虚拟化及云"]
series: ["虚拟化及云"]
categories: ["虚拟化及云"]
image:
---
### vsphere

虚机的VT开启条件：硬件为9

[删除任务、事件和历史性能数据](https://kb.vmware.com/s/article/2115749?lang=zh_CN)

```
hypervisor.cpuid.v0 = FALSE
"hypervisor.cpuid.v0"设置成FALSE的意义为欺骗虚拟Windows系统没有运行在虚拟机中
```

esxi6开始是web方式管理，如果没有，可能通过下面命令安装

```
esxcli software vib install -v https://download3.vmware.com/software/vmw-tools/esxui/esxui-signed-12086396.vib
```

#### ESXi升级

```
1.主机开启SSH
2.主机进入维护模式
3.
离线方式(建议)
zip包上传到主机可以访问的存储
esxcli software sources profile list -d <Offline Bundle的所在绝对路径>

在线方式
esxcli network firewall ruleset set -e true -r httpClient
esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml

显示数个profile，请选择要安装的版本（通常都是ESXI-6.0.0-2XXXXXX-standardd）把名字记录下来

4.
离线方式(建议)
esxcli software profile update -p <上一步记录下来的profile名称> -d <Offline Bundle的所在绝对路径>
在线方式
esxcli software profile update -p <上一步记录下来的profile名称> -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
esxcli network firewall ruleset set -e false -r httpClient

6.升级成功后，重启esxi

执行完成之后重启服务器，就可以更新完成，可能会出现虚拟机丢失情况，可以用这个方法移除虚拟机
vim-cmd vmsvc/getallvms
得到skip主机，然后
vim-cmd vmsvc/unregister 7 #数字是失效虚拟机的编号
```



