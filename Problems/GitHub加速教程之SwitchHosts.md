---
title: GitHub加速教程之SwitchHosts
date: 2021-4-13 22:23:00
tags: Git
toc: true
description: "GitHub 加速方法"
---



[转载](https://brew.idayer.com/guide/github)

# GitHub 加速方法

[GitHub Hosts](https://github.com/ineo6/hosts) 仓库提供最新的`GitHub hosts`地址。

你可以自行配置`hosts`，但是最佳实践是使用 [SwitchHosts!](https://oldj.github.io/SwitchHosts/#cn) 管理你的 `hosts`。

可以阅读文章 [SwitchHosts! 还能这样管理 hosts，后悔没早点用](https://mp.weixin.qq.com/s/A37XnD3HdcGSWUflj6JujQ) 了解详情，里面有介绍以及各个平台刷新 `DNS` 缓存的方法。

安装好 `SwitchHosts!` 后，新建一个规则：

- 方案名：GitHub（可以自行命名）
- 类型：远程
- URL 地址：https://cdn.jsdelivr.net/gh/ineo6/hosts/hosts 
- 自动更新：1 小时

这样就可以和仓库中最新的`hosts`保持同步。 
