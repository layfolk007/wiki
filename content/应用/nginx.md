---
title: "nginx"
date: 2022-10-28T16:52:09+08:00
description: "nginx"
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
[nginx https反向tomcat http注意事项](https://blog.csdn.net/gggauss/article/details/79400665)

### location块

| 配置格式 | 作用 |
| :--- | :--- |
| location = /uri | 表示精确匹配 |
| location ^~ /uri | 匹配以uri前缀开头的请求，不支持正则表达式 |
| location ~ | 区分大小写的匹配，属于正则表达式 |
| location ~\* | 不区分大小写的匹配，属于正则表达式 |
| location /uri | 表示前缀匹配，不带修饰符，但是优先级没有正则表达式高 |
| location / | 通用匹配，默认找不到其他匹配时，会进行通用匹配 |
| location @ | 命名空间，不提供常规的请求匹配 |

1. “=”优先级最高
2. 如果“=”匹配不到，会和“^~”进行匹配；
3. 继而是“~”，如果有多个“~”，则按照在文件里的先后顺序进行匹配；
4. 如果还匹配不到，则与“/uri”进行匹配；
5. 通用匹配“/”的优先级最低，如果找不到其他配置，就会进行通用匹配；
6. “@”表示命名空间的位置，通常在重定向时，进行匹配，且不会改变URL的原始请求。

只能在location块中的配置项：

* internal：表示该location块只支持Nginx内部的请求访问，如支持rewrite、error\_page等重定向，但不能通过外部的HTTP直接访问。
* limit\_except：限定该location块可执行的HTTP方法，如GET、POST
* alias：定义指定位置的替换

```
    location /a/ {
        alias /a/b  #如果匹配到/a/index.php请求，在进入location块后，会将请求变成/a/b/index.php
    }
```



