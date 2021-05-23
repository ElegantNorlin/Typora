---
title: PicGo + Gitee(码云)实现markdown图床
date: 2021-2-21 21:12:00
categories: 图床
cover: /img/p7.jpeg
tags: 博客
---

**前言**：深感在线博客的编辑器坑太多了，文档丢失、必须联网、可移植性太差，所以开始寻找可替代的方案。

### [markdown](https://link.zhihu.com/?target=https://baike.baidu.com/item/markdown/3245829?fr=aladdin)是一门易于上手能帮助作者专心写作的文档编辑语言，它的好处太多了，建议想自己动手做笔记写博客的朋友都可以学一学，10分钟上手（我昨天晚上还不会用，今天就开始用它写博客了。。足以证明它是真的很简单）

### [Tpyora](https://link.zhihu.com/?target=https://www.typora.io/)是一款优雅的markdown编辑器，也推荐给大家，至于安装和配置，比安装word还简单，就不赘述了

但是，这都不是重点，重点是咱们写博客的时候，总是需要**插入图片**的，图片存在本地的话上传到博客网站去就没法显示了，就算一个图一个图的复制粘贴上去，想移植到其他的博客网站，图就会失效，我们就需要图床

> 图床是干什么的？
> 图床就是一个便于在博文中插入在线图片连接的个人图片仓库。设置图床之后，在自己博客中插入的图片链接就可以随时随地在线预览了，并且不会因为任何意外原因无法查看，除非自己亲自删除。

### 神奇的PicGo就是为了解决这个问题诞生的，它可以将图片上传到指定的图床上，然后返回markdown链接，直接粘贴到你的文档中，就搞定啦

问题又来了，网上推荐七牛云阿里云都是要租赁服务器的，太麻烦还要钱，微博现在挂链接又很厉害。大部分人选择用github，但是github虽好却是国外的网站，速度终究比不上国内网站，研究了小半天，终于发现完美的解决方案。

## 最终决定使用PicGo + 国内的github - [码云](https://link.zhihu.com/?target=https://gitee/com)来实现markdown图床

废话说到这里，开始进入正题

------

## 1. 安装

- PicGo
- picgo-plugin-gitee-uploader插件

### 首先打开[picgo官网](https://link.zhihu.com/?target=https://github.com/Molunerfinn/PicGo)，下载安装包

![img](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/v2-6a5d78ebb1910843ff4d2872580d21a5_1440w.png) 

速度可能有点慢，希望你会科学上网❀鸡

### 安装之后打开主界面

### ![img](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/v2-6a5d78ebb1910843ff4d2872580d21a5_1440w.png) 

### 选择最底下的插件设置，搜索**gitee**

### 点击右边的gitee-uploader 1.1.2开始安装

> 这里注意一下，必须要先安装[node.js](https://link.zhihu.com/?target=https://nodejs.org/en/)才能安装插件，没装的自己装一下，然后重启就行。

这个地方有两个插件，我试了一遍，两个都能用，大家看心情选择，先说一下右边这个**gitee-uploader 1.1.2**，用不了的同学就选左边那个，我都会讲一遍配置

------

## 2. 建立gitee（码云）图床库

注册码云的方法很简单，网站引导都是中文，不多说了，我们直接建立自己的图床库。

### 点击右上角的+号，新建仓库

新建仓库的要点如下：

1. 输入一个仓库名称
2. 其次将仓库设为公开
3. 勾选使用Readme文件初始化这个仓库

**这个选项勾上，这样码云会自动给你的仓库建立master分支，这点很重要!!!** 我因为这点折腾了很久，因为使用github做图床picgo好像会自动帮你生成master分支，而picgo里的gitee插件不会帮你自动生成分支。

![img](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/v2-44a4581b8e0ac9a0bc6747ee9b507a0e_1440w.jpg) 

点击创建进入下一步

------

## 3. 配置PicGo

安装了**gitee-uploader 1.1.2**插件之后，我们开始配置插件

### 配置插件的要点如下：

![img](https://gitee.com/wanwanzh/imagebed/raw/master/pictures/v2-44a4581b8e0ac9a0bc6747ee9b507a0e_1440w.jpg) 

* repo：用户名/仓库名称，比如我自己的仓库leonG7/blogImage，找不到的可以直接复制仓库的url
* branch：分支，这里写上master
* token：填入码云的私人令牌
* path：路径，也就是你仓库里用来存放图片的文件夹的名称
* customPath：提交消息，这一项和下一项customURL都不用填。在提交到码云后，会显示提交消息，插件默认提交的是 `Upload 图片名 by picGo - 时间`

### 这个token怎么获取，下面登录进自己的码云

* 打开gitee，点击头像，进入设置
* 找到左边安全设置里面的私人令牌
* 点击`生成新令牌`，把**projects**这一项勾上，其他的不用勾，然后提交
* 这里需要验证一下密码，验证密码之后会出来一串数字，这一串数字就是你的token，将这串数字复制到刚才的配置里面去。

> 注意：这个令牌只会明文显示一次，建议在配置插件的时候再来生成令牌，直接复制进去，搞丢了又要重新生成一个。

### 现在保存你刚才的配置，然后将它设置为默认图床，大功告成。

### 设置Typora图像默认上传