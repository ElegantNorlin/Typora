---
title: JWT
date: 2022-3-31 16:05:00
tags: Java
toc: true
description: "Json Web Token"
---

![](https://github.com/ElegantNorlin/Images/blob/main/images/jeticon.png?raw=true)

## 1.JWT是什么？

JWT简称JSON Web Token,也就是通过JSON形式作为Web应用中的令牌,用于在各方之间安全地将信息作为JSON对象传输。在数据传输过程中还可以完成数据加密、签名等相关处理。JWT通常为:xxxxx.yyyyy.zzzzz格式的字符串，其中各部分分别代表Header.Payload.Signature。

## 2.JWT能做什么呢？

### 2.1授权

这是使用JWT的最常见方案。一旦用户登录，每个后续请求将包括JWT，从而允许用户访问该令牌允许的路由，服务和资源。单点登录是当今广泛使用JWT的一项功能，因为它的开销很小并且可以在不同的域中轻松使用。

### 2.2信息交换

JSON Web Token是在各方之间安全地传输信息的好方法。因为可以对JWT进行签名（例如，使用公钥/私钥对），所以您可以确保发件人是他们所说的人。此外，由于签名是使用标头和有效负载计算的，因此您还可以验证内容是否遭到篡改。

## 3.为什么要使用JWT？而不是Cookie and Session?

### 3.1基于传统的Session认证

#### 3.1.1认证流程

我们知道，http协议本身是一种无状态的协议，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行，因为根据http协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器，告诉其保存为cookie,以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了,这就是传统的基于session认证。

![](https://github.com/ElegantNorlin/Images/blob/main/images/cookiesession.png?raw=true)



#### 3.1.2暴露出的问题

1.每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别，通常而言session都是保存在内存中，而随着认证用户的增多，服务端的开销会明显增大

2.用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，这意味着用户下次请求还必须要请求在这台服务器上,这样才能拿到授权的资源，这样在分布式的应用上，相应的限制了负载均衡器的能力。这也意味着限制了应用的扩展能力。

3.因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。

4.在前后端分离系统中就更加痛苦:如下图所示
也就是说前后端分离在应用解耦后增加了部署的复杂性。通常用户一次请求就要转发多次。如果用session 每次携带sessionid 到服务	器，服务器还要查询用户信息。同时如果用户很多。这些信息存储在服务器内存中，给服务器增加负担。还有就是CSRF（跨站伪造请求攻	击）攻击，session是基于cookie进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。还有就是	     sessionid就是一个特征值，表达的信息不够丰富。不容易扩展。而且如果你后端应用是多节点部署。那么就需要实现session共享机制。	不方便集群应用。

![](https://github.com/ElegantNorlin/Images/blob/main/images/overviewofJavaWeb.png?raw=true)



### 3.2基于JWT认证

#### 3.2.1认证流程

* 首先，前端通过Web表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加密的传输（https协议），从而避免敏感信息被嗅探。

* 后端核对用户名和密码成功后，将用户的id等其他信息作为JWT Payload（负载），将其与头部分别进行Base64编码拼接后签名，形成一个JWT(Token)。形成的JWT就是一个形同lll.zzz.xxx的字符串。 token head.payload.singurater

* 后端将JWT字符串作为登录成功的返回结果返回给前端。前端可以将返回的结果保存在localStorage或sessionStorage上，退出登录时前端删除保存的JWT即可。

* 前端在每次请求时将JWT放入HTTP Header中的Authorization位。(解决XSS和XSRF问题) HEADER

* 后端检查是否存在，如存在验证JWT的有效性。例如，检查签名是否正确；检查Token是否过期；检查Token的接收方是否是自己（可选）。

* 验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，返回相应结果。

#### 3.2.2 JWT的优势

- 简洁(Compact): 可以通过URL，POST参数或者在HTTP header发送，因为数据量小，传输速度也很快

- 自包含(Self-contained)：负载中包含了所有用户所需要的信息，避免了多次查询数据库

- 因为Token是以JSON加密的形式保存在客户端的，所以JWT是跨语言的，原则上任何web形式都支持。

- 不需要在服务端保存会话信息，特别适用于分布式微服务。

![](https://github.com/ElegantNorlin/Images/blob/main/images/jwt.png?raw=true)



## 4.JWT的结构是什么呢？

### 4.1令牌的组成

- 1.标头(Header)
- 2.有效载荷(Payload)
- 3.签名(Signature)

因此，JWT通常如下所示:xxxxx.yyyyy.zzzzz   分别对应  Header.Payload.Signature

### 4.2Header

- 标头通常由两部分组成：令牌的类型（即JWT）和所使用的签名算法，例如HMAC SHA256或RSA。它会使用 Base64 编码组成 JWT 结构的第一部分。
- 注意:Base64是一种编码，也就是说，它是可以被翻译(解码)回原来的样子来的。它并不是一种加密过程。

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 4.3Payload

- 令牌的第二部分是有效负载，其中包含声明。声明是有关实体（通常是用户）和其他数据的声明。同样的，它会使用 Base64 编码组成 JWT 结构的第二部分

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### 4.4Signature

- 前面两部分都是使用 Base64 进行编码的，即前端可以解开知道里面的信息。Signature 需要使用编码后的 header 和 payload 以及我们提供的一个密钥，然后使用 header 中指定的签名算法（HS256）进行签名。签名的作用是保证 JWT 没有被篡改过
- 如:
	HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret);

#### 4.4.1Signature签名的目的

* 最后一步签名的过程，实际上是对头部以及负载内容进行签名，防止内容被窜改。如果有人对头部以及负载的内容解码之后进行修改，再进行编码，最后加上之前的签名组合形成新的JWT的话，那么服务器端会判断出新的头部和负载形成的签名和JWT附带上的签名是不一样的。如果要对新的头部和负载进行签名，在不知道服务器加密时用的密钥的话，得出来的签名也是不一样的。

#### 4.4.2信息安全问题

- 在这里大家一定会问一个问题：Base64是一种编码，是可逆的，那么我的信息不就被暴露了吗？

- 是的。所以，在JWT中，不应该在负载里面加入任何敏感的数据。在上面的例子中，我们传输的是用户的User ID。这个值实际上不是什么敏	感内容，一般情况下被知道也是安全的。但是像密码这样的内容就不能被放在JWT中了。如果将用户的密码放在了JWT中，那么怀有恶意的第	三方通过Base64解码就能很快地知道你的密码了。因此JWT适合用于向Web应用传递一些非敏感信息。JWT还经常用于设计用户认证和授权系	统，甚至实现Web应用的单点登录。

![](https://github.com/ElegantNorlin/Images/blob/main/images/JWTStructure.png?raw=true)

### 4.5JWT组成总结

- 输出是三个由点分隔的Base64-URL字符串，可以在HTML和HTTP环境中轻松传递这些字符串，与基于XML的标准（例如SAML）相比，它更紧凑。
- 简洁(Compact)
	可以通过URL, POST 参数或者在 HTTP header 发送，因为数据量小，传输速度快
- 自包含(Self-contained)
	负载中包含了所有用户所需要的信息，避免了多次查询数据库

## 5.使用JWT

### 5.1引入JWT依赖

在maven配置文件pom.xml中引入相关依赖

```xml
<!--引入jwt-->
<dependency>
  <groupId>com.auth0</groupId>
  <artifactId>java-jwt</artifactId>
  <version>3.4.0</version>
</dependency>
```

### 5.2生成token令牌

```java
HashMap<String,Object> map = new HashMap<>();
Calendar instance = Calendar.getInstance();
instance.add(Calendar.SECOND, 90);
//生成令牌
//使用JWT工具类链式生成token
//.withHeader()可以不写，该参数有默认值
//如果Payload有多个参数，使用withArrayClaim()设置参数
String token = JWT.create()
  .withHeader(map)
  .withClaim("username", "张三")//设置自定义用户名
  .withExpiresAt(instance.getTime())//设置过期时间
  .sign(Algorithm.HMAC256("token!Q2W#E$RW"));//设置签名 保密 复杂
//输出令牌
System.out.println(token);
```

### 5.3根据令牌和签名解析数据

```java
//创建验证对象
JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256("token!Q2W#E$RW")).build();
//调用验证对象验证接收到（正常应该是从前端传过来的）的的token
DecodedJWT decodedJWT = jwtVerifier.verify(token);
//取出令牌负荷信息中设置的用户名
System.out.println("用户名: " + decodedJWT.getClaim("username").asString());
//取出令牌负荷信息中设置的过期时间
System.out.println("过期时间: "+decodedJWT.getExpiresAt());
```

### 5.4常见异常信息

```json
- SignatureVerificationException:				签名不一致异常
- TokenExpiredException:    						令牌过期异常
- AlgorithmMismatchException:						算法不匹配异常
- InvalidClaimException:								失效的payload异常
```

## 6.封装工具类

```java
public class JWTUtils {
  	//设置加密盐，用于签名的生成和签名的校验，加密盐仅能让通信的双方知道，设置成什么都可以并不是固定的。
    private static String TOKEN = "token!Q@W3e4r";
    /**
     * 生成token
     * @param map  //传入payload
     * @return 返回token
     */
  	//生成token的方法
  	//map中存放的是存入负载Payload中的键值对
    public static String getToken(Map<String,String> map){
        JWTCreator.Builder builder = JWT.create();
        map.forEach((k,v)->{
            builder.withClaim(k,v);
        });
      	//生成Calendar对象
        Calendar instance = Calendar.getInstance();
      	//设置Calendar对象的信息
        instance.add(Calendar.SECOND,7);
      	//instance.getTime()获得builder.withExpiresAt()需要的Date对象
        builder.withExpiresAt(instance.getTime());
        return builder.sign(Algorithm.HMAC256(TOKEN)).toString();
    }
    /**
     * 验证token的合法性
     * @param token
     * @return
     */
    public static void verify(String token){
        JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);
    }
    /**
     * 获取token中payload
     * @param token
     * @return
     */
    public static DecodedJWT getTokenInfo(String token){
      	//直接将该对象返回给调用者，调用者根据该方法即可获取相应的值
        return JWT.require(Algorithm.HMAC256(TOKEN)).build().verify(token);
    }
}
```







