---
title: CentOS挂载磁盘
date: 2022-05-07
tags: 
- Linux
---

首先得需要一块新的磁盘

在我的理解中，这些新的磁盘就代表分区

我们需要将分区挂载到Linux目录下，挂载完之后，我们通过目录就能访问磁盘下的数据了

### 1.查看电脑是否有新的磁盘可以挂载呢

```shell
fdisk -l
```

![](https://github.com/ElegantNorlin/Images/blob/main/images/fdisk.png?raw=true)

我们可以看到已经挂载的磁盘在上边显示，如/dev/vda1

下面的/dev/vdb磁盘是没有被挂载的磁盘

### 2.格式化磁盘

````shell
mkfs.ext4 /dev/vdb
````

### 3.挂载磁盘

```shell
mount /dev/vdb /home
```



### 4.列出所有块设备查看是否挂载成功

```shell
lsblk
```

![](https://github.com/ElegantNorlin/Images/blob/main/images/lsblk.png?raw=true)
