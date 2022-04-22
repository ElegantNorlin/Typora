---
title: 中标麒麟安装docker-ce
date: 2022-04-13
tags: 
- Linux
---



贴一个龙芯（中标麒麟）开源社区安装包下载地址：

http://ftp.loongnix.cn/os/loongnix-server/1.7/os/mips64el/Packages/

### 1.下载rpm安装包

```shell
wget http://ftp.loongnix.cn/os/loongnix-server/1.7/os/mips64el/Packages/docker-ce-18.06.3.ce-0.lns7.mips64el.rpm
```

### 2.安装

```shell
rpm -ivh docker-ce-18.06.3.ce-0.lns7.mips64el.rpm
```

在安装过程中一般会缺少依赖库，我们使用`yum install`根据提示信息（依赖库及版本号）安装相关的依赖库即可

安装完依赖库，重新运行安装命令

### 3.相关命令

* docker开机启动

  ```shell
  sudo systemctl enable docker
  ```

* 启动docker命令

  ```shell
  sudo systemctl start docker
  ```

* 查看docker版本

  ```shell
  docker -v
  ```

* 运行测试容器，测试docker是否安装成功

  ```shell
  docker run --rm hello-world
  ```

  若输出以下信息，则代表安装成功

  ```shell
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  b8dfde127a29: Pull complete
  Digest: sha256:308866a43596e83578c7dfa15e27a73011bdd402185a84c5cd7f32a88b501a24
  Status: Downloaded newer image for hello-world:latest
  
  Hello from Docker!
  This message shows that your installation appears to be working correctly.
  
  To generate this message, Docker took the following steps:
   1. The Docker client contacted the Docker daemon.
   2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
      (amd64)
   3. The Docker daemon created a new container from that image which runs the
      executable that produces the output you are currently reading.
   4. The Docker daemon streamed that output to the Docker client, which sent it
      to your terminal.
  
  To try something more ambitious, you can run an Ubuntu container with:
   $ docker run -it ubuntu bash
  
  Share images, automate workflows, and more with a free Docker ID:
   https://hub.docker.com/
  
  For more examples and ideas, visit:
   https://docs.docker.com/get-started/
  ```

  



