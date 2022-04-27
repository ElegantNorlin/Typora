---
title: @PathVariable 和 @RequestParam的区别
date: 2022-04-26
tags: 
- Java
- SprinngBoot
---

### 1.@PathVariable 和 @RequestParam的区别

#### 1.1相同点:

这两个都是用来处理前端传递过来的请求参数

#### 1.2不同点:

不同的是RequestParam处理的是请求参数，而PathVariable处理的是路径变量。

这样说更容易理解,RequestParam是将对应请求路径下的请求参数值映射到处理器参数上,而PathVariable是将请求路径变量的值映射到处理器参数上

只看文字可能难以理解，接下来看一个例子帮助理解

### 2.例子

#### 2.1@RequestParam

```java
http://localhost/mdeditor/chen?id=39请求参数和请求变量的区别
对于该请求参数的接收我们可以这样
//将请求参数映射到处理器参数上
@RequestMapping("/mededitor/chen")
public String getId(@RequestParam( value="id",requested=false)String Pid){
	return id;
}
```

#### 2.2@PathVariable

```java
//如下，设置chen为路径变量
http://localhost/mdeditor/chen
//将请求的路径变量，映射到处理器参数中
@RequestMapping("/mededitor/{page}")
public String getId(@PathVariable( value="page",requested=false)String Page){
return page;
}

//会输出chen

//同时，在路径变量名和处理器参数名一致的时候，我们可以省去PathVariable中的value值，如下
http://localhost/mdeditor/cc
@RequestMapping("/mededitor/{page}")
public String getId(@PathVariable String page){
return page;
}
//输出cc
```



