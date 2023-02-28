---
title: "windows常见问题"
date: 2022-10-28T16:52:09+08:00
description: "windows常见问题"
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
### win10
## 远程桌面连接出现：身份验证错误，要求的函数不受支持的错误提示
**解决方法**
1. 组策略→管理模板→系统→凭据分配，再在右侧栏中双击打开加密Oracle认证（如果没有，那么选择“加密数据库修正”）， 将未配置改成已启用，再把保护级别从以缓解改成易受攻击，然后确定
2. Win10家庭版需要修改注册表
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\CredSSP\Parameters
新建DWORD（32位）值，然后重命名成AllowEncryptionOracle。双击打开AllowEncryptionOracle，将数值数据改成2，最后点击确定
```



