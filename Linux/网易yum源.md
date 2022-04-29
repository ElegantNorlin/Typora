---
title: 配置网易yum源
date: 2022-04-27
tags: 
- Linux
- yum
---

### 1.安装wget

```shell
yum install wget
```

### 2.备份原yum源

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

### 3.下载网易到yum仓库文件夹

```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
```

### 4.清理缓存

```shell
yum clean all
```

### 5.重新生成缓存

```shell
yum makecache
```



