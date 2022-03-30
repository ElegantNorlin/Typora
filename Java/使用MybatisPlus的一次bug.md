---
title: 使用MybatisPlus的一次bug
date: 2021-3-28 16:05:00
tags: Java
toc: true
description: "本文记录了使用MybatisPlus遇到一次bug"
---



记一次使用MybatisPlus出现的问题，在使用last方法实现limit功能的时候，字符串的最后一定要留空格，不然一定会报错的。

```java
queryWrapper.last("limit "+limit);
```

