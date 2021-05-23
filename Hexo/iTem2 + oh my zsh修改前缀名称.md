---
title: iTem2 + oh my zsh修改前缀名称
date: 2021-2-28 20:29:00
categories: mac
cover: /img/p4.jpg
tags: 博客
---

首先要清楚安装 **oh my zsh** 之后，前缀名称是有主题中的配置文件（主题名称.zsh-theme）决定的。

**方法一：**

我们修改前缀名称应该使用下面的命令：

```
# 进入oh my zsh存放主题的目录
cd ~/.oh-my-zsh/theme
# 使用vi命令打开主题配置文件
vi 主题名称.zsh-theme
```

具体的修改规则可以到网上搜索🔍。

**方法二：**

直接修改主题，每个主题都会自带不同的前缀名称，命令如下：

```
# 打开~/.zshrc文件修改
open ~/.zshrc
# 修改文件ZSH_THEME="tjkirch_mod"直接更换双引号内的名称即可
```

