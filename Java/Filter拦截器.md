---
title: Filter拦截器
date: 2022-04-26
tags: 
- Java
- SprinngBoot
---

### 1.Interceptor介绍

Filter 过滤器这个概念应该大家不会陌生，特别是对与从 Servlet 开始入门学 Java 后台的同学来说。那么这个东西我们能做什么呢？Filter 过滤器主要是用来过滤用户请求的，它允许我们对用户请求进行前置处理和后置处理，比如实现 URL 级别的权限控制、过滤非法请求等等。Filter 过滤器是面向切面编程——AOP 的具体实现（AOP切面编程只是一种编程思想而已）。

另外，Filter 是依赖于 Servlet 容器，`Filter`接口就在 Servlet 包下面，属于 Servlet 规范的一部分。所以，很多时候我们也称其为“增强版 Servlet”。

如果我们需要自定义 Filter 的话非常简单，只需要实现 `javax.Servlet.Filter` 接口，然后重写里面的 3 个方法即可！

### 2.过滤器（Filter）和拦截器（Interceptor）的区别

- 过滤器（Filter）：当你有一堆东西的时候，你只希望选择符合你要求的某一些东西。定义这些要求的工具，就是过滤器。
- 拦截器（Interceptor）：在一个流程正在进行的时候，你希望干预它的进展，甚至终止它进行，这是拦截器做的事情。

### 3.自定义Interceptor

自定义的Interceptor需要实现org.springframework.web.servlet.HandlerInterceptor接口

**知识点补充：**

HandlerInterceptor有三个方法可以重写init(),doFilter(),destroy()，其中doFilter()方法必须重写。

重写玩过滤内容，需要调用`filterChain.doFilter(servletRequest, servletResponse);`使其生效

```java
import javax.servlet.*;
import java.io.IOException;
//定义过滤路径
@WebFilter(urlPatterns = "/*")
//定义过滤顺序
@Order(1)
public class afilter implements Filter {
  	@Override
    public void init(FilterConfig filterConfig) {
        System.out.println("初始化过滤器：", filterConfig.getFilterName());
    }
  
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //对请求进行预处理
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        String requestUri = request.getRequestURI();
        System.out.println("请求的接口为：" + requestUri);
        long startTime = System.currentTimeMillis();
        //通过 doFilter 方法实现过滤功能
        filterChain.doFilter(servletRequest, servletResponse);
        // 上面的 doFilter 方法执行结束后用户的请求已经返回
        long endTime = System.currentTimeMillis();
        System.out.println("该用户的请求已经处理完毕，请求花费的时间为：" + (endTime - startTime));
    }
  
  	@Override
    public void destroy() {
        System.out.println("销毁过滤器");
    }
}
```

### 4.配置SpringBoot启动类扫描接口包路径

```java
@SpringBootApplication
@ServletComponentScan(basePackages = "com.demo.demo.filter")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

