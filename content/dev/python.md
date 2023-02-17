---
title: "python"
date: 2023-02-14T15:12:53+08:00
description: "python"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["dev"]
series: ["dev"]
categories: ["dev"]
image:
---
**linux下多版本管理**

[pyenv](https://github.com/pyenv/pyenv.git)

环境变量：

```
PYENV_ROOT="$HOME/.pyenv"
PYTHON_CFLAGS="-fPIC"
PYTHON_CONFIGURE_OPTS="--enable-shared"
PATH="$PYENV_ROOT/bin:$PATH"

export PATH PYENV_ROOT PYTHON_CFLAGS PYTHON_CONFIGURE_OPTS

if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```

centos5编译

`gcc gcc-c++ openssl-devel readline-devel zlib-devel bzip2-devel ncurses-devel sqlite-devel`

更新证书/etc/pki/tls/certs/ca-bundle.crt

**Python 生成项目的 requirements.txt 文件**

1、pip freeze\(它生成的**requirements.txt**文件包含当前环境的完全列表，不相关的依赖包也会包含进来\)

2、pipreqs\(只会包含项目import的包，包含列表不是很完全\)

```
pip install pipreqs
pipreqs --force <project-path>
```

3、pigar\(输出信息比pipreqs详细\)

```
pip install pigar
```

**爬虫**

```
requests + re
requests + bs4
requests + lxml
selenium + phantomjs
框架scrapy，等等
```

**例子\(**[**12306**](https://github.com/testerSunshine/12306)**、**[**py12306**](https://github.com/pjialin/py12306)**\)**

