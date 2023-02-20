---
title: "os"
date: 2023-02-15T15:19:58+08:00
description: "os"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: []
series:
categories: []
image:
---
**资源监控**

[glances](https://github.com/nicolargo/glances)

**homebrew及linuxbrew**

```
Homebrew这级目录应该可以去掉，未尝试
# homebrew安装，可使用国内源
https://mirrors.tuna.tsinghua.edu.cn/help/homebrew
https://mirrors.tuna.tsinghua.edu.cn/help/homebrew-bottles
git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew ~/.homebrew/Homebrew
git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core ~/.homebrew/Homebrew/Library/Taps/homebrew/homebrew-core
git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask ~/.homebrew/Homebrew/Library/Taps/homebrew/homebrew-cask
mkdir ~/.homebrew/bin
ln -s ~/.homebrew/Homebrew/bin/brew ~/.linuxbrew/bin
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
echo 'eval $(~/.homebrew/bin/brew shellenv)' >> ~/.bash_profile

# linuxbrew安装
git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew ~/.linuxbrew/Homebrew
git clone https://github.com/Homebrew/linuxbrew-core.git ~/.linuxbrew/Homebrew/Library/Taps/homebrew/homebrew-core
linuxbrew-core注意不是homebrew-core，用github源，目前没有找到国内源
linuxbrew-cask目前没有
```

[ultimateshell](https://github.com/G3G4X5X6/ultimateshell)