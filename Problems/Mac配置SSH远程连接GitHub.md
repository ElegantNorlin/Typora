---
title: Mac配置SSH远程连接GitHub
date: 2021-4-13 22:23:00
tags: mac
toc: true
description: "mac自带SSH，所以不需要安装SSH"
---

### 获取SSH远程连接密钥

mac自带SSH，所以不需要安装SSH

下列命令在`~/.ssh`目录下会生成SSH密钥`id_rsa`(私钥)、`id_rsa.pub`(公钥)和`known_hosts`(不要删除，留着不用动就好了)

```shell
$ ssh-keygen -t rsa -C "youremail"
```

上述代码执行过程需要输入直接回车！

上述命令中的`youremail`是GitHub的账号，也就是注册用的邮箱📮。

### 复制公钥

使用下列命令查看SSH公钥：

```shell
cat ~/.ssh/id_rsa.pub
```

复制公钥

### GitHub添加公钥

打开GitHub，按照`settings/SSH and GPG keys`点击`New SSH key`按钮新建一个公钥，密钥标题写什么都可以，内容直接粘贴上刚才复制的密钥。

### 查看公钥是否添加成功

执行下列代码

```shell
ssh -T git@github.com
```

如果出现下列输出则说明连接成功

```shell
Hi **********! You've successfully authenticated, but GitHub does not provide shell access.
```

