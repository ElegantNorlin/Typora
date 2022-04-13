---
title: Swagger2入门
date: 2022-04-13
tags: 
- Java
- Swagger
---

### 1.Swagger和SpringBoot环境搭建

#### 1.1依赖引入

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

#### 1.2 编写Swagger配置类

```java
@Configuration
//开启swagger2配置
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .pathMapping("/")
                .select()
          			//扫描接口的包，包中的接口信息就会被自动扫描出来，无需在接口中配置，这是最简单的
                .apis(RequestHandlerSelectors.basePackage("com.baizhi.controller"))
                .paths(PathSelectors.any())
                .build().apiInfo(new ApiInfoBuilder()
                        .title("SpringBoot整合Swagger")
                        .description("SpringBoot整合Swagger，详细信息......")
                        .version("9.0")
                        //contact代表联系人的信息	
                        .contact(new Contact("xiaochen","www.baizhiedu.xin","xiaochen@163.com"))
                        .license("The Baizhi License")
                        .licenseUrl("http://www.baizhiedu.com")
                        .build());
    }
}
```

#### 1.3启动SpringBoot,访问SwaggerUI界面

访问Swagger2提供的UI界面: http://localhost:8080/swagger-ui.html

### 2.Swagger构建接口文档

#### 2.1开发Controller接口

```java
@RestController
@RequestMapping("user")
public class UserController {
    @GetMapping("findAll")
    public Map<String,Object> findAll(){
        HashMap<String, Object> map = new HashMap<>();
        map.put("msg","查询所有成功");
        map.put("state",true);
        return map;
    }   
}
```

#### 2.2重启SpringBoot项目访问SwaggerUI界面

### 3.Swagger的注解使用说明

#### 3.1@Api

- 作用: 用来指定接口的描述文字，对应在文档界面对Controller的解释描述

- 修饰范围: 用在类上

  ```java
  @RequestMapping("user")
  @Api(tags = "用户模块接口规范说明")
  public class UserController {
  	....
  }
  ```

#### 3.2 @ApiOperation

- 作用: 用来对接口中具体方法做描述，对应在文档界面对接口的解释描述

- 修饰范围: 用在方法上

  * value:接口名称
  * notes:对接口的详细描述

  ```java
  @GetMapping("findAll")
  @ApiOperation(value = "查询所有用户接口",
                notes = "<span style='color:red;'>描述:</span>&nbsp;用来查询所有用户信息的接口")
  public Map<String,Object> findAll(){
    Map<String, Object> map = new HashMap<String,Object>();
    map.put("success","查询所有成功!");
    map.put("state",true);
    return map;
  }
  ```

  **value: 用来对接口的说明**

  **notes:用来对接口的详细描述**

#### 3.3 @ApiImplicitParams

- 作用: 用来接口的中参数进行说明描述

- 修饰范围: 用在方法上

  ```java
  @ApiImplicitParams({
    @ApiImplicitParam(name="id",value = "用户id",dataType = "String",defaultValue = "21"),
    @ApiImplicitParam(name="name",value = "用户姓名",dataType ="String",defaultValue = "张三")
  })
  public Map<String, Object> save(String id, String name) {
    .....
  }
  ```

#### 3.4 @ApiResponses

- 作用：用于请求的方法上，表示一组响应，自定义状态码的注解

- 修饰范围: 用在方法上

  ```java
  @ApiResponses({
              @ApiResponse(code = 400, message = "请求参数没填好"),
              @ApiResponse(code = 404, message = "请求路径没有或页面跳转路径不对")
  })
  public Map<String, Object> save(String id, String name) {
    .....
  }
  ```

  
