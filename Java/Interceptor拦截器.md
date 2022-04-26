---
title: Interceptor拦截器
date: 2022-04-26
tags: 
- Java
- SprinngBoot
---

### 1.Interceptor介绍

**拦截器(Interceptor)同** Filter 过滤器一样，它俩都是面向切面编程——AOP 的具体实现（AOP切面编程只是一种编程思想而已）。

你可以使用 Interceptor 来执行某些任务，例如在 **Controller** 处理请求之前编写日志，添加或更新配置......

在 **Spring中**，当请求发送到 **Controller** 时，在被**Controller**处理之前，它必须经过 **Interceptors**（0或更多）。

**Spring Interceptor**是一个非常类似于**Servlet Filter** 的概念 。

### 2.过滤器（Filter）和拦截器（Interceptor）的区别

- 过滤器（Filter）：当你有一堆东西的时候，你只希望选择符合你要求的某一些东西。定义这些要求的工具，就是过滤器。
- 拦截器（Interceptor）：在一个流程正在进行的时候，你希望干预它的进展，甚至终止它进行，这是拦截器做的事情。

### 3.自定义Interceptor

自定义的Interceptor需要实现org.springframework.web.servlet.HandlerInterceptor接口

**知识点补充：**

HandlerInterceptor有三个方法可以重写（用到哪个就重写哪个，不需要全部重写）

>  **preHandle**方法是进行处理器拦截用的，顾名思义，该方法将在Controller处理之前进行调用。
>
> 
>
> **postHandle**是进行处理器拦截用的，它的执行时间是在处理器进行处理之后，也就是在Controller的方法调用之后执行，但是它会在DispatcherServlet进行视图的渲染之前执行，也就是说在这个方法中你可以对ModelAndView进行操作。
>
> 
>
> **afterCompletion**方法将在整个请求完成之后，也就是DispatcherServlet渲染了视图执行。
>
> (这个方法的主要作用是用于清理资源的)
>
> 

```java
package com.mszlu.blog.handler;

import com.alibaba.fastjson.JSON;
import com.mszlu.blog.dao.pojo.SysUser;
import com.mszlu.blog.service.LoginService;
import com.mszlu.blog.utils.UserThreadLocal;
import com.mszlu.blog.vo.ErrorCode;
import com.mszlu.blog.vo.Result;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {

    @Autowired
    private LoginService loginService;

    //Controller方法处理之前会调用该方法
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
    }
		
  	//postHandle在Controller的方法调用之后执行，且于DispatcherServlet进行视图的渲染之前执行
  	@Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        
    }
  
    //DispatcherServlet进行视图的渲染之后才会执行该方法
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}

```

### 4.配置拦截器

在MVC配置类的addInterceptors方法中配置拦截器（可以配置多个拦截器，形成拦截器链）

```java
import com.mszlu.blog.handler.LoginInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMVCConfig implements WebMvcConfigurer {
    @Autowired
    private LoginInterceptor loginInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //添加拦截器，设置拦截路径
        registry.addInterceptor(loginInterceptor)
                .addPathPatterns("/test")
                .addPathPatterns("/comments/create/change")
                .addPathPatterns("/articles/publish");
    }

}
```

