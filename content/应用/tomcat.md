---
title: "tomcat"
date: 2022-10-28T16:52:09+08:00
description: "tomcat"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["应用"]
series: ["应用"]
categories: ["应用"]
image:
---
## iis7.5和tomcat7整合
注：64位系统要用64位的isapi_redirect.dll
1. 在IIS管理器中的“ISAPI和CGI限制”里面右键添加，路径选择为tomcat安装目录下tk文件夹的isapi_redirect.dll,并设定“允许执行扩展路径”，描述名可取jakarta
2. 接着单击站点，选择ISAPI筛选器，并右键添加筛选器，名称可取jakarta，可执行文件选择tomcat安装目录下conf文件夹isapi_redirect.dll
3. 在站点上右键"添加虚拟目录"，别名取jakarta（必须是jakarta名称，名称必须和isapi_redirect.properties里"extension_uri"的值的名字一致），路径指向tomcat安装目录下tk文件夹，即isapi_redirect.dll所在目录。
4. 点击虚拟目录jakarta，双击“处理程序映射”,最右边（第三分栏里面）选择“编辑功能权限...”将所有权限（执行权限）选上。

## server.xml
```
<Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"
           connectionTimeout="20000" enableLookups="false"
           redirectPort="8443" URIEncoding="UTF-8"/>
```

## catalina
```
当在linux上处理图形时，需要添加参数-Djava.awt.headless=true
java6、7
JAVA_OPTS="-server -Xms512m -Xmx1g -XX:PermSize=256M -XX:MaxPermSize=512m"

java8
JAVA_OPTS="-server -Xms512m -Xmx1g -XX:MetaspaceSize=256M -XX:MaxMetaspaceSize=512m"
```