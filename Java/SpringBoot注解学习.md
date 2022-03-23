---
title: SpringBoot注解学习
date: 2021-3-28 16:05:00
categories: 
cover: /img/p15.jpg
tags: Java
---





* @Configuration:标注在类上，相当于该类为配置Spring上下文容器的配置类，用于构建Bean的定义，初始化Spring容器。

* @ConfigurationProperties(prefix=" ")：将配置文件中的对象注入到该注解所在的类，prefix为该对象的key。

* @Bean表示该方法返回一个对象注册到Spring上下文容器中。

* @Conditional(ClassCondition.class)表示条件，如果括号中的条件为true则创建该注解对应的Bean，为false则不创建Bean。其中ClassCondition.class为我们编写的条件类，该条件类继承了Condition接口。该方法还可以封装成自己的注解来动态的判断我们指定的条件来加载Bean。

* @Enable*:作用就是@Import来收集并注册特定的场景相关的bean，并且加入到IOC容器。@EnableAutoConfiguration就是借助@Import来收集所有符合自动配置条件的bean定义，并加载到IOC容器中。

* @Import:使用@Import导入的类会被Spring加载到IOC容器中。

* SpringBoot中提供了很多Enable开头的注解，这些注解都是用于动态启用某些功能的。而其底层原理是使用@Import注解导入一些配置类，实现Bean的动态加载

* @Autowired:将对象导入到类中，要求被注入的类需要被 Spring 容器管理比如：当我们在将一个类上标注@Component或者@Controller或@Service或@Repository注解之后，spring的组件扫描就会自动发现它，并且会将其初始化为spring应用上下文中的bean。

* @Component,@Repository,@Service, @Controller：被这些注解标识的类可以使用 `@Autowired` 注解让 Spring 容器帮我们自动装配 bean。

  * @Conponent:注解表明一个类会作为Spring组件类，并告知Spring要为这个类创建bean，能够被Spring所识别。就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。
  * @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
  * @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
  * @Controller : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

* @RestController注解相当于@ResponseBody ＋ @Controller合在一起的作用:表示这是个控制器 bean,并且是将函数的返回值直接填入 HTTP 响应体中；如果只是使用@RestController注解Controller，则Controller中的方法无法返回jsp页面，如果需要返回到指定页面，则需要用 @Controller配合视图解析器InternalResourceViewResolver才行；@ResponseBody的作用其实是将java对象转为json格式的数据。

* @ResponseBody的作用其实是将java对象转为json格式的数据。

* @GetMapping("users") 等价于@RequestMapping(value="/users",method=RequestMethod.GET)；访问路径为“/users”,请求方法为get方法。

* @PostMapping("users") 等价于@RequestMapping(value="/users",method=RequestMethod.POST)；访问路径为“/users”，请求方法为post。

* @PutMapping("/users/{userId}") 等价@RequestMapping(value="/users/{userId}",method=RequestMethod.PUT)

  其中{userId}为占位符

* @PathVariable:用于获取路径参数。

* @RequestParam:用于获取查询参数。

* @RequestBody:主要用来接收前端传递给后端的json字符串中的数据的(请求体中的数据的)；GET方式无请求体，所以使用@RequestBody接收数据时，前端不能使用GET方式提交数据，而是用POST方式进行提交。接收到数据之后会自动将数据绑定到 Java 对象上去。

* @value:使用 @Value("${key}") 读取配置文件中的信息；

* @Transactional在要开启事务的方法上使用`@Transactional`注解即可!

  * **作用于类**：当把`@Transactional 注解放在类上时，表示所有该类的`public 方法都配置相同的事务属性信息。
  * **作用于方法**：当类配置了`@Transactional`，方法也配置了`@Transactional`，方法的事务会覆盖类的事务配置信息。
  
* @PathVariable是spring3.0的一个新功能：接收请求路径中占位符的值.
