---
title: gitee如何添加SSH公钥
date: 2021-2-20 20:29:00
categories: 教程
cover: /img/p3.png
tags: 博客
---

* 下载安装git，本文重在讲解如何添加公钥，如何安装git请自行Google

* 使用下列命令在命令行中生成sshkey:

  ```
  ssh-keygen -t rsa -C "xxxxx@xxxxx.com"
  ```

* 按照提示完成三次回车，即可生成 ssh key。通过查看 ~/.ssh/id_rsa.pub 文件内容，获取到你的 public key

  ![image-20210316010406392](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/image-20210316010406392.png) 
  
* 打开gitee，在设置中点击SSH公钥，然后把刚才复制的public key复制进去保存下来。

* 下面的命令是检查能否成功通过SSH连接到gitee

  ```
  ssh -T git@gitee.com
  ```

  若返回 Hi XXX! You’ve successfully authenticated, but Gitee.com does not provide shell access. 内容，则证明添加成功。