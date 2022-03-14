---
title:Maven子工程不能引入父工程定义的依赖
date: 2021-3-28 16:05:00
categories: 
cover: /img/p15.jpg
tags: Java
---



可能我的方法不一定适用于你，但可以进行参考：

我把父工程中管理依赖的`<dependencyManagement></dependencyManagement>`标签删除，重新倒入依赖，问题就解决了。



