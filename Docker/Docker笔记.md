---

---





### 安装Docker

```shell
# 1、yum 包更新到最新 
yum update
# 2、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的 
yum install -y yum-utils device-mapper-persistent-data lvm2
# 3、 设置yum源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 4、 安装docker，出现输入的界面都按 y 
yum install -y docker-ce
# 5、 查看docker版本，验证是否验证成功
docker -v
```



### Docker服务相关命令

* 启动docker服务

  ```
  systemctl start docker
  ```

* 停止docker服务

  ```
  systemctl stop docker
  ```

* 重启docker服务

  ```
  systemctl restart docker
  ```

* 查看docker服务状态

  ```
  systemctl status docker
  ```

### Docker镜像相关命令

* 查看镜像：查看本地所有镜像

  ```
  docker images
  # 查看所有镜像的id
  docker images -q
  ```

* 搜索镜像：从网络中查找需要的镜像

  ```
  docker search 镜像名称
  ```

* 拉取镜像：从Docker仓库下载镜像到本地，镜像名称格式为“名称:版本号”，如果版本号不指定则是最新的版本。如果不知道镜像版本，可以去docker hub搜索对应镜像查看。

  ```
  docker pull 镜像名称
  ```

* 删除镜像：删除本地镜像

  ```
  # 删除指定的本地镜像
  docker rmi 镜像id
  
  # 删除本地所有镜像
  docker rmi "docker images -q"
  ```

### Docker容器相关命令

* 查看容器

  ```
  # 查看正在运行的容器
  docker ps
  
  # 查看所有容器
  docker ps -a
  ```

* 创建并启动容器

  ```
  docker run 参数
  ```

  参数说明：

  * -：保持容器运行。通常与-t同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容器自动关闭。
  * -t：为容器重新分配一个伪输入终端，通常与-i同时使用。
  * -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用dockerexec 进入容器。退出后，容器不会关闭。
  * -it 创建的容器一般称为式互式容器，-id 创建的容器一般称为守护式
  * -name：为创建的容器命名。

* 进入容器

  ```
  # 退出容器，容器不会关闭
  docker exec 参数
  ```

* 停止容器

  ```
  docker stop 容器名称
  ```

* 启动容器

  ```
  docker start 容器名称
  ```

* 删除容器：如果容器是运行状态则删除失败，需要停止容器才能删除

  ```
  docker rm 容器名称
  ```

* 查看容器信息

  ```
  docker inspect 容器名称
  ```

### Docker容器的数据卷

同一个数据卷（📁）可以同时挂载到两个容器上

* 创建容器时，使用-v参数设置数据卷

  ```
  docker run ... -v 宿主机目录:容器目录 ...
  ```

  例子：

  ```shell
  docker run -it --name=c1 -v /root/data:/root/data_container centos:7 /bin/bash
  ```
  
  注意事项：
  
  * 目录必须是绝对路径
  
* **配置数据卷容器：将多个容器同时挂载到数据卷容器上**

  1.创建启动c3数据卷容器，是用-v参数设置数据卷，其中/volume为容器内的目录，我们可以将其他的容器挂载到这个数据卷容器上。
  
  ```
  docker run -it --name=c3 -v /volume centos:7 /bin/bash
  ```

  2.创建c1 c2容器，使用--vloume-from参数设置数据卷

  ```
  docker run -it --name=c1 --vloume-from c3 centos:7 /bin/bash
  docker run -it --name=c2 --vloume-from c3 centos:7 /bin/bash
  ```
  
  

### Docker部署MySQL

### 一、部署MySQL

1. 搜索mysql镜像

```shell
docker search mysql
```

2. 拉取mysql镜像

```shell
docker pull mysql:5.6
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir ~/mysql
cd ~/mysql
```

```shell
docker run -id \
-p 3307:3306 \
--name=c_mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
mysql:5.6
```

- 参数说明：
  - **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
  - **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。配置目录
  - **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
  - **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
  - **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。



4. 进入容器，操作mysql

```shell
docker exec –it c_mysql /bin/bash
```

5. 使用外部机器连接容器中的mysql



### 部署Tomcat

1. 搜索tomcat镜像

```shell
docker search tomcat
```

2. 拉取tomcat镜像

```shell
docker pull tomcat
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir ~/tomcat
cd ~/tomcat
```

```shell
docker run -id --name=c_tomcat \
-p 8080:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat 
```

- 参数说明：

  - **-p 8080:8080：**将容器的8080端口映射到主机的8080端口

    **-v $PWD:/usr/local/tomcat/webapps：**将主机中当前目录挂载到容器的webapps



4. 使用外部机器访问tomcat

   访问宿主机的ip和端口以及访问路径



### 部署Nginx

1. 搜索nginx镜像

```shell
docker search nginx
```

2. 拉取nginx镜像

```shell
docker pull nginx
```

3. 创建容器，设置端口映射、目录映射


```shell
# 在/root目录下创建nginx目录用于存储nginx数据信息
mkdir ~/nginx
cd ~/nginx
mkdir conf
cd conf
# 在~/nginx/conf/下创建nginx.conf文件,粘贴下面内容
vim nginx.conf
```

```shell
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}


```




```shell
docker run -id --name=c_nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx
```

- 参数说明：
  - **-p 80:80**：将容器的 80端口映射到宿主机的 80 端口。
  - **-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf**：将主机当前目录下的 /conf/nginx.conf 挂载到容器的 :/etc/nginx/nginx.conf。配置目录
  - **-v $PWD/logs:/var/log/nginx**：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目录

4. 使用外部机器访问nginx



### 部署Redis

1. 搜索redis镜像

```shell
docker search redis
```

2. 拉取redis镜像

```shell
docker pull redis:5.0
```

3. 创建容器，设置端口映射

```shell
docker run -id --name=c_redis -p 6379:6379 redis:5.0
```

4. 使用外部机器连接redis

```shell
./redis-cli.exe -h 192.168.149.135 -p 6379
```



### Dockerfile

#### Docker镜像制作

##### 容器转为镜像

```
//打包镜像
docker commit 容器id 镜像名称:版本号
//压缩镜像文件，用于传输镜像
docker save -o 压缩文件名称 镜像名称:版本号
//加载镜像文件
docker load -i 压缩文件名称
```



##### Dockerfile

* dockerfile是一个文本文件
* 包含了一条一条的指令
* 每一条指令构建一层，基于基础镜像，最终构建出一个新的镜像 
* 对于开发人员：可以为开发团队提供一个完全一致的开发环境
* 对于测试人员：可以直接拿开发时所构建的镜像或者通过Dockerfile文件构建一个新的镜像开始工作
* 对于运维人员：在部署时，可以实现应用的无缝移植

## 

| 关键字      | 作用                     | 备注                                                         |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| FROM        | 指定父镜像               | 指定dockerfile基于那个image构建                              |
| MAINTAINER  | 作者信息                 | 用来标明这个dockerfile谁写的                                 |
| LABEL       | 标签                     | 用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看 |
| RUN         | 执行命令                 | 执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"] |
| CMD         | 容器启动命令             | 提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"] |
| ENTRYPOINT  | 入口                     | 一般在制作一些执行就关闭的容器中会使用                       |
| COPY        | 复制文件                 | build的时候复制文件到image中                                 |
| ADD         | 添加文件                 | build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务 |
| ENV         | 环境变量                 | 指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value |
| ARG         | 构建参数                 | 构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数 |
| VOLUME      | 定义外部可以挂载的数据卷 | 指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"] |
| EXPOSE      | 暴露端口                 | 定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp |
| WORKDIR     | 工作目录                 | 指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径 |
| USER        | 指定执行用户             | 指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户 |
| HEALTHCHECK | 健康检查                 | 指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制 |
| ONBUILD     | 触发器                   | 当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大 |
| STOPSIGNAL  | 发送信号量到宿主机       | 该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。       |
| SHELL       | 指定执行脚本的shell      | 指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell            |

