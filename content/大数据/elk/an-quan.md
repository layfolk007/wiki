#log4j漏洞
**相关漏洞**
* CVE-2021-44832（远程代码执行漏洞）
* CVE-2021-45105（拒绝服务攻击漏洞）
* CVE-2021-45046（拒绝服务攻击漏洞）
* CVE-2021-44228（远程代码执行漏洞）
* 信息泄漏漏洞（安全公司 Praetorian 发现）

**测试方法**
* 将要使用的测试命令`${java:os}`使用`urlencode`转码为`%24%7Bjava%3Aos%7D`，构造查询语句测试：

    `curl -u elastic 'http://localhost:9200/.security-7/_search?q=%24%7Bjava%3Aos%7D'`

* 在日志中确认是否有命令的执行结果：

    `tailf /data/es1/logs/es-test.log |grep 'security-7'`

**修复方式**
* 添加jvm参数

    `-Dlog4j2.formatMsgNoLookups=true`
* 从log4j中删除易受攻击的`JndiLookup`类

    `zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class`
* 使用[极限网关](https://elasticsearch.cn/article/14444)
* 升级到最新版本，[方法](https://cxybb.com/article/yonggeit/120994102#_10)