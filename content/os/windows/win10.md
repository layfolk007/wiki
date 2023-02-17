**问题**

![](/assets/import.png)

**解决方法**

1、组策略→管理模板→系统→凭据分配，再在右侧栏中双击打开加密Oracle认证（如果没有，那么选择“加密数据库修正”）， 将未配置改成已启用，再把保护级别从以缓解改成易受攻击，然后确定

2、 Win10家庭版需要修改注册表

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\CredSSP\Parameters
新建DWORD（32位）值，然后重命名成AllowEncryptionOracle。双击打开AllowEncryptionOracle，将数值数据改成2，最后点击确定
```



