---
title: SpringBoot统一异常处理
date: 2022-04-08
tags: 
- Java
- SpringBoot
---

### 使用@ControllerAdvice实现异常统一处理

下面的代码对Exception类异常进行全局异常处理

规则：我们处理全局异常时，一个方法对一个异常进行处理

```java
package com.mszlu.blog.handler;

import com.mszlu.blog.vo.Result;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

//对加了@Controller注解的方法进行拦截处理 AOP的实现
@ControllerAdvice
public class AllExceptionHandler {
    //进行异常处理，处理Exception.class的异常
    @ExceptionHandler(Exception.class)
    @ResponseBody //返回json数据
    public Result doException(Exception ex){
      	//这里可以用日志记录
        ex.printStackTrace();
      	//返回给前端信息，以供前端展示异常信息
        return Result.fail(-999,"系统异常");
    }
}

```

不需要在controller接口做异常处理。
