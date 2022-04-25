---
title: Docker启动postgres
date: 2022-04-25
tags: 
- Docker
- postgres
toc: true
---



### 1.拉取postgres镜像

```shell
# 拉取最新版本
docker pull postgres

# 拉取11.5版本的postgres
docker pull postgres:11.5
```

### 2.创建容器

```shell
docker run --name postgres_demo -e POSTGRES_PASSWORD=password -p 54321:5432 -d postgres:11.5
```

* run: 创建并运行一个容器；

* --name: 指定创建的容器的名字；

* -e POSTGRES_PASSWORD=password: 设置环境变量，指定数据库的登录口令为password；

* -p 54321:5432: 端口映射将容器的5432端口映射到外部机器的54321端口；

* -d postgres:11.4: 指定使用postgres:11.4作为镜像。

**注意：**

* postgres镜像默认的用户名为postgres，

* 登陆口令为创建容器是指定的值。
