---
title: SpringBoot整合Spring Security
date: 2022-04-09
tags: 
- Java
- SpringBoot
- Spring Security
---

### 1.导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 2.启动项目、访问任意端口

启动SpringBoot项目后，控制台会打印一串密码：

```
//启动会在控制台出现
Using generated security password: 98687887-01bf-41b5-bb6e-63009367be0f
```

同时访问http://localhost:8080/interface ,会出现一个登陆页面

用户名输入user，密码输入上方控制台打印的密码，即可登录，并且正常访问接口。

![](https://github.com/ElegantNorlin/Images/blob/main/images/login.png?raw=true)

### 3.配置登录的用户名和密码

对登录的用户名/密码进行配置，有三种不同的方式:

#### 3.1在 application.properties 中进行配置

```
spring.security.user.name=admin
spring.security.user.password=mszlu
```

#### 3.2通过 Java 代码配置在内存中

创建一个配置类，在配置类中配置用户名和密码

```java
package com.mszlu.union.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //下面这两行配置表示在内存中配置了两个用户
        auth.inMemoryAuthentication()
                .withUser("admin").roles("admin").password("$2a$10$2UaOufuypWR1TASuso2S6.u6TGL7nuAGCsb4RZ5X2SMEuelwQBToO")
                .and()
                .withUser("user").roles("user").password("$2a$10$2UaOufuypWR1TASuso2S6.u6TGL7nuAGCsb4RZ5X2SMEuelwQBToO");
    }
    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    public static void main(String[] args) {
        String mszlu = new BCryptPasswordEncoder().encode("mszlu");
        System.out.println(mszlu);
    }
}
```

#### 3.3通过 Java 从数据库中加载

该方法在后面的章节中记录，请继续往下看。

### 4.配置登录拦截、放行等，也可以自行配置登录页面

在配置类中配置

```java
@Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests() //开启登录认证
                .antMatchers("/user/findAll").hasRole("admin") //访问接口需要admin的角色
                .antMatchers("/login").permitAll()
                .anyRequest().authenticated() // 其他所有的请求 只需要登录即可
                .and().formLogin()
                .loginPage("/login.html") //自定义的登录页面
                .loginProcessingUrl("/login") //登录处理接口
                .usernameParameter("username") //定义登录时的用户名的key 默认为username
                .passwordParameter("password") //定义登录时的密码key，默认是password
                .successHandler(new AuthenticationSuccessHandler() {
                    @Override
                    public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {
                        httpServletResponse.setContentType("application/json;charset=utf-8");
                        PrintWriter out = httpServletResponse.getWriter();
                        out.write("success");
                        out.flush();
                    }
                }) //登录成功处理器
                .failureHandler(new AuthenticationFailureHandler() {
                    @Override
                    public void onAuthenticationFailure(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, AuthenticationException e) throws IOException, ServletException {
                        httpServletResponse.setContentType("application/json;charset=utf-8");
                        PrintWriter out = httpServletResponse.getWriter();
                        out.write("fail");
                        out.flush();
                    }
                }) //登录失败处理器
        .permitAll() //通过 不拦截，更加前面配的路径决定，这是指和登录表单相关的接口 都通过
        .and().logout() //退出登录配置
        .logoutUrl("/logout") //退出登录接口
        .logoutSuccessHandler(new LogoutSuccessHandler() {
            @Override
            public void onLogoutSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {
                httpServletResponse.setContentType("application/json;charset=utf-8");
                PrintWriter out = httpServletResponse.getWriter();
                out.write("logout success");
                out.flush();
            }
        }) //退出登录成功 处理器
        .permitAll() //退出登录的接口放行
        .and()
        .httpBasic()
        .and()
        .csrf().disable(); //csrf关闭 如果自定义登录 需要关闭

    }
```

自定义的登录页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>自定义登录页面</title>
    <!-- 引入样式 -->
    <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
</head>
<body>
<div id="app">
    用户名： <el-input v-model="username" placeholder="请输入内容"></el-input>
    密码：  <el-input v-model="password" placeholder="请输入内容"></el-input>
           <el-button type="success" @click="login()">提交</el-button>
</div>
<!-- 引入组件库 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    new Vue({
        el: "##app",
        data:{
            username:"",
            password:""
        },
        methods:{
            login(){
                    axios.post("/login?username="+this.username+"&password="+this.password).then((res)=>{
                        if (res.data == "success"){
                            this.$message.success("登录成功");
                        }else{
                            this.$message.error("用户名或密码错误");
                        }
                    })
            }
        }
    });
</script>
</body>
</html>
```

### 5.数据库访问认证和授权

#### 5.1数据库认证

管理员表

```mysql
CREATE TABLE `admin_user`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `password` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `create_time` bigint(0) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;

INSERT INTO `admin_user`(`id`, `username`, `password`, `create_time`) VALUES (1, 'admin', '$2a$10$2UaOufuypWR1TASuso2S6.u6TGL7nuAGCsb4RZ5X2SMEuelwQBToO', 1622711132975);

```

管理员表对应的实体类

```java
package com.mszlu.union.pojo;

import lombok.Data;

@Data
public class AdminUser {

    private  Long id;

    private String username;

    private String password;

    private Long createTime;
}
```

操作管理员表的Mapper接口.

说一下为什么要操作数据库？

所谓的数据库认证在我理解就是去数据库验证是否有这个用户，验证用户就要查询用户，所以要创建Mapper接口

```java
package com.mszlu.union.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.union.pojo.AdminUser;

public interface AdminUserMapper extends BaseMapper<AdminUser> {
}
```

实现UserDetailService接口

```java
package com.mszlu.union.security;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.union.mapper.AdminUserMapper;
import com.mszlu.union.pojo.AdminUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class SecurityUserService implements UserDetailsService {
    @Autowired
    private AdminUserMapper adminUserMapper;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        LambdaQueryWrapper<AdminUser> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(AdminUser::getUsername,username).last("limit 1");
        AdminUser adminUser = this.adminUserMapper.selectOne(queryWrapper);
        if (adminUser == null){
            throw new UsernameNotFoundException("用户名不存在");
        }
        List<GrantedAuthority> authorityList = new ArrayList<>();
      	//组装成User对象，这里的User是Spring Security提供的
        UserDetails userDetails = new User(username,adminUser.getPassword(),authorityList);
				//把UserDetails对象返回，剩下的认证工作由security框架完成
        return userDetails;
    }
}
```

在**配置类**中添加使用SecurityUserService

如果只有一个UserDetailsService实现类，这一步可以去掉，如果有多个的话需要配置

```java
@private SecurityUserService securityUserService;

@Override
protected void configure(HttpSecurity http) throws Exception {
  http.userDetailsService(securityUserService);
  //...
}
```

#### 5.2数据库授权

##### 5.2.1第一种授权

角色表结构

```mysql
DROP TABLE IF EXISTS `role`;
CREATE TABLE `role`  (
  `id` int(0) NOT NULL AUTO_INCREMENT,
  `role_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `role_desc` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `role_keyword` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of role
-- ----------------------------
INSERT INTO `role` VALUES (1, '管理员', '管理员', 'ADMIN');
INSERT INTO `role` VALUES (2, '运营', '运营部门', 'BUSINESS');

DROP TABLE IF EXISTS `user_role`;
CREATE TABLE `user_role`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `user_id` bigint(0) NOT NULL,
  `role_id` int(0) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `user_id`(`user_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of user_role
-- ----------------------------
INSERT INTO `user_role` VALUES (1, 1, 1);
```

角色表实体类

```java
package com.mszlu.union.pojo;

import lombok.Data;

@Data
public class Role {
    private Integer id;

    private String roleName;

    private String roleDesc;
		
  	//定义角色关键字
    private String roleKeyword;
}
```

权限表结构

```sql
DROP TABLE IF EXISTS `permission`;
CREATE TABLE `permission`  (
  `id` int(0) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `desc` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `permission_keyword` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `path` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of permission
-- ----------------------------
INSERT INTO `permission` VALUES (1, '查询全部', '查询全部', 'USER_FINDALL', '/user/findAll');
INSERT INTO `permission` VALUES (2, '年龄查询', '年龄查询', 'USER_FINDAGE', '/user/findAge');

DROP TABLE IF EXISTS `role_permission`;
CREATE TABLE `role_permission`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `role_id` int(0) NOT NULL,
  `permission_id` int(0) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `role_id`(`role_id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of role_permission
-- ----------------------------
INSERT INTO `role_permission` VALUES (1, 1, 1);
```

权限表实体类

```java
package com.mszlu.union.pojo;

import lombok.Data;

@Data
public class Permission {
    private Integer id;

    private String name;

    private String desc;
		
  	//定义权限关键字：方法关键词
    private String permissionKeyword;
		
  	//定义权限路径
    private String path;
}
```

创建Mapper接口及对应的xml文件

```java
package com.mszlu.union.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.union.pojo.Permission;
import com.mszlu.union.pojo.Role;

import java.util.List;

public interface PermissionMapper extends BaseMapper<Permission> {


    List<Permission> findPermissionByRole(Integer roleId);
}
```

```java
package com.mszlu.union.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.union.pojo.Role;

import java.util.List;

public interface RoleMapper extends BaseMapper<Role> {

    List<Role> findRoleListByUserId(Long userId);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis配置文件-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mszlu.union.mapper.PermissionMapper">
    <resultMap id="perMap" type="com.mszlu.union.pojo.Permission">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="desc" property="desc"/>
        <result column="permission_keyword" property="permissionKeyword"/>
        <result column="path" property="path"/>
    </resultMap>

    <select id="findPermissionByRole" parameterType="int" resultMap="perMap">
        select * from permission where id in (select permission_id from role_permission where role_id=##{roleId})
    </select>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis配置文件-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mszlu.union.mapper.RoleMapper">
    <resultMap id="roleMap" type="com.mszlu.union.pojo.Role">
        <id column="id" property="id"/>
        <result column="role_name" property="roleName"/>
        <result column="role_desc" property="roleDesc"/>
        <result column="role_keyword" property="roleKeyword"/>
    </resultMap>

    <select id="findRoleListByUserId" parameterType="long" resultMap="roleMap">
        select * from role where id in (select role_id from user_role where user_id=##{userId})
    </select>
</mapper>
```

在UserDetailService的接口实现中，查询用户的权限

```java
package com.mszlu.union.security;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.union.mapper.AdminUserMapper;
import com.mszlu.union.mapper.PermissionMapper;
import com.mszlu.union.mapper.RoleMapper;
import com.mszlu.union.pojo.AdminUser;
import com.mszlu.union.pojo.Permission;
import com.mszlu.union.pojo.Role;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

//该方法分为两部分
	//第一部分：数据库的认证
	//第二部分：授权

@Component
public class SecurityUserService implements UserDetailsService {
    @Autowired
    private AdminUserMapper adminUserMapper;
    @Autowired
    private RoleMapper roleMapper;
    @Autowired
    private PermissionMapper permissionMapper;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
      	//创建条件查询条件
        LambdaQueryWrapper<AdminUser> queryWrapper = new LambdaQueryWrapper<>();
      	//条件查询语句
        queryWrapper.eq(AdminUser::getUsername,username).last("limit 1");
      	//开始查询
        AdminUser adminUser = this.adminUserMapper.selectOne(queryWrapper);
      	//判断数据库是否有这个用户
        if (adminUser == null){
            throw new UsernameNotFoundException("用户名不存在");
        }
      	//创建一个ArrayList来接受角色列表
        List<GrantedAuthority> authorityList = new ArrayList<>();
      	
        //第二部分：查询角色和角色对应的权限 并赋予当前的登录用户，并告知spring security框架
      	
      	//根据用户id查询对应的角色列表
        List<Role> roleList = roleMapper.findRoleListByUserId(adminUser.getId());
        //第一层for循环：遍历每个角色
        for (Role role : roleList) {
          	//根据角色id查询对应的权限，封装到权限列表中
            List<Permission> permissionList = permissionMapper.findPermissionByRole(role.getId());
          	//添加角色
          	//注意：添加角色时，需要加上ROLE_前缀，所以这里每个权限前面都要加ROLE_。否在最后验证不会通过
            authorityList.add(new SimpleGrantedAuthority("ROLE_"+role.getRoleKeyword()));
          	//遍历每个权限
            for (Permission permission : permissionList) {
              	//添加权限对应的关键词
                authorityList.add(new SimpleGrantedAuthority(permission.getPermissionKeyword()));
            }
        }
        UserDetails userDetails = new User(username,adminUser.getPassword(),authorityList);
        return userDetails;
    }
}
```

在接口上配置权限检查注解

```java
@GetMapping("findAll")
@PreAuthorize("hasAuthority('USER_FINDALL')")
public List<User> findAll(){
  return userService.findAll();
}

@GetMapping("findAge")
@PreAuthorize("hasAuthority('USER_FINDAGE')")
public List<User> findAge(){
  return userService.findAge();
}

@GetMapping("findById")
@PreAuthorize("hasRole('ADMIN')")
public User findById(@RequestParam("id") Long id){
  return userService.findById(id);
}
```

在配置上开启权限认证

```java
@Configuration
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {}
```

##### 5.2.2第二种授权方式

修改UserDetailService中 授权的实现，实现自定义的授权类，增加path属性

```java
package com.mszlu.union.security;

import org.springframework.security.core.GrantedAuthority;

public class MySimpleGrantedAuthority implements GrantedAuthority {
    private String authority;
    private String path;

    public MySimpleGrantedAuthority(){}

    public MySimpleGrantedAuthority(String authority){
        this.authority = authority;
    }

    public MySimpleGrantedAuthority(String authority,String path){
        this.authority = authority;
        this.path = path;
    }

    @Override
    public String getAuthority() {
        return authority;
    }

    public String getPath() {
        return path;
    }
}
```

```java
package com.mszlu.union.security;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.union.mapper.AdminUserMapper;
import com.mszlu.union.mapper.PermissionMapper;
import com.mszlu.union.mapper.RoleMapper;
import com.mszlu.union.pojo.AdminUser;
import com.mszlu.union.pojo.Permission;
import com.mszlu.union.pojo.Role;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

//该方法分为两部分
	//第一部分：数据库的认证
	//第二部分：授权

@Component
public class SecurityUserService implements UserDetailsService {
    @Autowired
    private AdminUserMapper adminUserMapper;
    @Autowired
    private RoleMapper roleMapper;
    @Autowired
    private PermissionMapper permissionMapper;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
      	//创建条件查询条件
        LambdaQueryWrapper<AdminUser> queryWrapper = new LambdaQueryWrapper<>();
      	//条件查询语句
        queryWrapper.eq(AdminUser::getUsername,username).last("limit 1");
      	//开始查询
        AdminUser adminUser = this.adminUserMapper.selectOne(queryWrapper);
      	//判断数据库是否有这个用户
        if (adminUser == null){
            throw new UsernameNotFoundException("用户名不存在");
        }
      	//创建一个ArrayList来接受角色列表
        List<GrantedAuthority> authorityList = new ArrayList<>();
      	
        //第二部分：查询角色和角色对应的权限 并赋予当前的登录用户，并告知spring security框架
      	
      	//根据用户id查询对应的角色列表
        List<Role> roleList = roleMapper.findRoleListByUserId(adminUser.getId());
        //第一层for循环：遍历每个角色
        for (Role role : roleList) {
          	//根据角色id查询对应的权限，封装到权限列表中
            List<Permission> permissionList = permissionMapper.findPermissionByRole(role.getId());
          	//添加角色
          	//注意：添加角色时，需要加上ROLE_前缀，所以这里每个权限前面都要加ROLE_。否在最后验证不会通过
            //使用MySimpleGrantedAuthority的第一种构造方法
          	authorityList.add(new MySimpleGrantedAuthority("ROLE_"+role.getRoleKeyword()));
          	//遍历每个权限
            for (Permission permission : permissionList) {
              	//添加权限对应的关键词,使用MySimpleGrantedAuthority的第二种构造方法
                authorityList.add(new MySimpleGrantedAuthority(permission.getPermissionKeyword()));
            }
        }
        UserDetails userDetails = new User(username,adminUser.getPassword(),authorityList);
        return userDetails;
    }
}
```

编写AuthService的auth方法

```java
package com.mszlu.union.service;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.union.mapper.AdminUserMapper;
import com.mszlu.union.mapper.PermissionMapper;
import com.mszlu.union.mapper.RoleMapper;
import com.mszlu.union.mapper.UserMapper;
import com.mszlu.union.pojo.AdminUser;
import com.mszlu.union.pojo.Permission;
import com.mszlu.union.pojo.Role;
import com.mszlu.union.security.MySimpleGrantedAuthority;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import javax.servlet.http.HttpServletRequest;
import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

@Service
public class AuthService {

    @Autowired
    private AdminUserMapper adminUserMapper;
    @Autowired
    private RoleMapper roleMapper;
    @Autowired
    private PermissionMapper permissionMapper;

    public boolean auth(HttpServletRequest request, Authentication authentication){
        String requestURI = request.getRequestURI();
      	//获取用户的用户名，虽然是object接收的对象，但仍是字符串
        Object principal = authentication.getPrincipal();
      	//判断用户名是否为匿名或者空字符串，如果是直接返回false，false为不放行
        if (principal == null || "anonymousUser".equals(principal)){
            //未登录
            return false;
        }
      	//如果已经登录，我们将用户名强转为UserDetails对象。
        UserDetails userDetails = (UserDetails) principal;
      	//返回用户的权限，使用Collection<? extends GrantedAuthority>集合接收
        Collection<? extends GrantedAuthority> authorities = userDetails.getAuthorities();
      	//遍历集合
        for (GrantedAuthority authority : authorities) {
          	//将每个GrantedAuthority权限接口强转为我们自定义的MySimpleGrantedAuthority权限实现类
            MySimpleGrantedAuthority grantedAuthority = (MySimpleGrantedAuthority) authority;
          	//获取真实的URI路径（不带参数）
            String[] paths = StringUtils.split(requestURI, "?");
          	//从实现类中获取路径，判断URI路径是否与数据库的Path相同，若相同则放行
            if (paths[0].equals(grantedAuthority.getPath())){
                return true;
            }
        }
      	//拦截
        return false;
    }
}
```

修改配置类

```java
	@Override
protected void configure(HttpSecurity http) throws Exception {       
    http.authorizeRequests() //开启登录认证
            .antMatchers("/login").permitAll()
            //使用访问控制 自己实现authService处理，会接收两个参数 ，参数名称要对应起来
            .anyRequest().access("@authService.auth(request,authentication)")
}
```

