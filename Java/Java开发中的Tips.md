---
title: Java开发中的Tips
date: 2021-03-28
tags: 
- Java
description: "记录了开发中遇到的一些奇怪的知识"
---



### 1.在定义返回给前端数据的Vo类时尽量id使用String引用类型，防止精度损失问题。

### 2.过滤器、拦截器和监听器的理解

过滤器（Filter）：当你有一堆东西的时候，你只希望选择符合你要求的某一些东西。定义这些要求的工具，就是过滤器。

拦截器（Interceptor）：在一个流程正在进行的时候，你希望干预它的进展，甚至终止它进行，这是拦截器做的事情。

监听器（Listener）：当一个事件发生的时候，你希望获得这个事件发生的详细信息，而并不想干预这个事件本身的进程，这就要用到监听器。

### 3.IDEA中maven启动失败报错

使用idea创建了一个springboot项目,运行build时发生如下报错

```
Error:(3, 32) java: 程序包org.springframework.boot不存在
Error:(4, 46) java: 程序包org.springframework.boot.autoconfigure不存在
Error:(5, 40) java: 程序包org.springframework.boot.builder不存在
Error:(6, 52) java: 程序包org.springframework.boot.web.servlet.support不存在
Error:(9, 34) java: 找不到符号
符号: 类 SpringBootServletInitializer
```

后来通过查阅资料得知新版IDEA需要在Setting里将Maven/runner/delegate IDE build/run actions to Maven勾选上即可.