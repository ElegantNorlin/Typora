---
title: SpringBoot整合log4j2
date: 2022-05-11
tags: 
- Java
- SpringBoot
- log4j2
---

直接上代码！

### 1.pom添加依赖

```xml
<dependency> <!-- 引入log4j2依赖 -->            
  <groupId>org.springframework.boot</groupId>            
  <artifactId>spring-boot-starter-log4j2</artifactId>        
</dependency>
```

### 2.排除 spring-boot-starter-logging

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <!--排除SpringBoot自带的日志框架logback spring-boot-starter-logging-->
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

### 3.添加log4j2-spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <properties>
        <property name="LOG_HOME">./logs</property>
        <property name="FILE_NAME">mylog</property>
        <property name="log.sql.level">debug</property>
    </properties>
 
    <!--输出源设置-->
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <!-- %d:时间 %p:日志级别 %processId:进程数 %t:进程名 %highlight:突出显示 %c:类全路径 %L:行数 %msg:信息 %n: 换行-->
            <PatternLayout
                    pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} %-5p %processId --- [%t] %highlight{%-40c{1.1}} :%-4L - %msg%n"/>
        </Console>
        <!-- 输出到文件设置-->
        <RollingRandomAccessFile name="RollingRandomAccessFile" fileName="${LOG_HOME}/${FILE_NAME}.log"
                                 filePattern="${LOG_HOME}/${date:yyyy-MM}/${FILE_NAME}-%d{yyyy-MM-dd}-%i.log">
            <!-- 输出格式设置 -->
            <PatternLayout
                    pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} %-5p %processId --- [%t] %-40c{1.1} :%-4L - %msg%n"/>
            <!-- 生成策略 -->
            <Policies>
                <!-- 基于时间的触发Rolling策略,关键点在于 filePattern后的日期格式，以及TimeBasedTriggeringPolicy的interval，日期格式精确到哪一位，interval也精确到哪一个单位-->
                <TimeBasedTriggeringPolicy modulate="true" interval="1"/>
                <!-- 基于文件大小的触发Rolling策略,单位(这个配置需要和filePattern结合使用)-->
                <SizeBasedTriggeringPolicy size="10 MB"/>
            </Policies>
            <!-- 如果不做配置，默认是7，这个7指的是上面i的最大值，超过了就会覆盖之前的 -->
            <DefaultRolloverStrategy max="20"/>
        </RollingRandomAccessFile>
    </Appenders>
 
    <Loggers>
        <Root level="info">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="RollingRandomAccessFile"/>
        </Root>
        <!-- 配置 cn.xx.bapi 这个包地下的日志级别为debug -->
        <Logger name="cn.xx.bapi" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="RollingRandomAccessFile"/>
        </Logger>
        <!-- 打印dao层 sql跟踪信息  -->
        <Logger name="cn.xx.dao" level="${log.sql.level}" additivity="false">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="RollingRandomAccessFile"/>
        </Logger>
    </Loggers>
</Configuration>
```

### 4.plication.yml

```yml
logging:
  config: classpath:log4j2-spring.xml
```

### 5.使用logger类记录日志

```java
//首先引入Slf4j注解,下面就可以往日志文件中打印想要打印的日志了
@Slf4j
public class LogExampleOther {
  
  public static void main(String... args) {
    log.error("Something else is wrong here");
  }
}
```

