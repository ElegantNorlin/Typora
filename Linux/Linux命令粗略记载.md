---
title: Linux命令粗略记载
date: 2022-04-13
tags: 
- Linux
---

## 1.服务器版本相关命令

### 1.1查看CPU信息

```shell
cat /proc/cpuinfo
```

查询结果中，model name所对应的字段为CPU处理器名称

### 1.2查看内存信息

* 方法一：

```shell
cat /proc/meminfo
```

* 方法二：

```shell
free -m
```

### 1.3查看CPU位数、系统版本

* 方法一：

```shell
getconf LONG_BIT
cat /etc/redhat-release
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

### 1.4查看网卡信息

```shell
ifconfig
```



## 2.tar命令

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

## 3.Linux内核

永久修改操作系统参数的方法

编辑 /etc/sysctl.conf文件

#### 查看所有内核参数及其数值

```shell
sysctl --a
```

