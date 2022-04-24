---
title: Linux命令粗略记载
date: 2022-04-13
tags: 
- Linux
---

### 1.服务器版本相关命令

#### 1.1查看CPU信息

```shell
cat /proc/cpuinfo
```

查询结果中，model name所对应的字段为CPU处理器名称

#### 1.2查看内存信息

* 方法一：

```shell
cat /proc/meminfo
```

* 方法二：

```shell
free -m
```

#### 1.3查看CPU位数、系统版本

* 方法一：

```shell
getconf LONG_BIT
cat /etc/redhat-release
cat /etc/system-release
```

* 方法二：

```shell
uname [option]
```

> -a：打印出系统的全部信息
>
> -n：打印出当前系统的主机名，相当于执行 hostname 命令
>
> -r：显示操作系统的发行编号
>
> -s：打印出操作系统的名称
>
> -v：打印出操作系统的版本

#### 1.4查看网卡信息

```shell
ifconfig
```

### 2.tar命令

tar是用来压缩文件、解压缩文件的命令

* 压缩命令

  > tar -zcvf pack.tar.gz pack/ #打包压缩为一个.gz格式的压缩包
  >
  > tar -jcvf pack.tar.bz2 pack/ #打包压缩为一个.bz2格式的压缩包
  >
  > tar -Jcvf pack.tar.xz pack/ #打包压缩为一个.xz格式的压缩包

* 解压缩命令

  > tar -zxvf pack.tar.gz /pack #解包解压.gz格式的压缩包到pack文件夹
  >
  > tar -jxvf pack.tar.bz2 /pack #解包解压.bz2格式的压缩包到pack文件夹
  >
  > tar -Jxvf pack.tar.xz /pack #解包解压.xz格式的压缩包到pack文件夹

### 3.Linux内核

永久修改操作系统参数的方法

编辑 /etc/sysctl.conf文件

#### 查看所有内核参数及其数值

```shell
sysctl --a
```

### 4.find

```shell
find -name "*.jar"
```

### 5.which

```shell
# 查看java安装在哪个地方
[root@node1 postgresql-11.5]# which java
/usr/bin/java
```

### 6.jobs

```shell
# 查看当前shell环境中已经后台启动的作业
jobs
-l：显示进程号；
-p：仅任务对应的显示进程号；
-n：显示任务状态的变化；
-r：仅输出运行状态（running）的任务；
-s：仅输出停止状态（stoped）的任务。
```

### 7.netstat

```shell
# 查看网络进程占用的端口
netstat -ntlp
```

### 8.wget

wget是一款下载指定url资源的工具

特点：

1.支持断点续传

2.wget支持ftp和http协议下载

3.wget支持添加代理

```shell
# 安装wget工具
yum install wget
brew install wget
```

#### 8.1直接下载

```shell
wget https://i0.hdslb.com/bfs/article/aa6eb6ae976ca5833c12076ad874f606d3f79eca.jpg@942w_287h_progressive.webp
```

#### 8.2指定保存路径

```shell
wget -O ./aa.jpg https://i0.hdslb.com/bfs/article/aa6eb6ae976ca5833c12076ad874f606d3f79eca.jpg@942w_287h_progressive.webp
```

### 9.yum

#### 9.1查看yum源（仓库）

```shell
yum repolist
```

#### 9.2查看yum安装的依赖

```shell
yum list
```

#### 9.3yum安装依赖

```shell
yum install packagename
```

#### 9.4
