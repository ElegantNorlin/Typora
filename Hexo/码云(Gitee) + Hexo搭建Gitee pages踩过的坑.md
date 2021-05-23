---
title: 码云(Gitee) + Hexo搭建Gitee pages踩过的坑
date: 2021-3-28 16:04:00
categories: 教程
cover: /img/p12.jpg
tags: 博客
---



**在Github感人速度支配下（自己搭建的🪜也解决不了感人问题），我还是选择了gitee来搭建个人博客！** **废话不多说，直接上干货！**


### 第一次部署（hexo d）的时候可能会踩的坑
可能会遇到以下错误：remote: error: GE007: Your push would publish a private email address.
读一下知道什么意思，但是并不知道怎么解决。经过百度之后原来是码云（gitee）会默认把你的邮箱设置为私有
可以通过下面的操作把邮箱改为公开

**进入设置界面**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220022021312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU4Njg0NQ==,size_16,color_FFFFFF,t_70)
**点击邮箱管理**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220022235337.png)
**去掉不公开我的邮箱✅**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220022433924.png)
这样错误就迎刃而解。



### _cinfig.yml中url是http还是https一定要和gitee pages中部署的相对应起来

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220022835991.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210220022846326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDU4Njg0NQ==,size_16,color_FFFFFF,t_70)
**一定要一致，不然博客可能显示不正常**







### Markdown中的一些符号可能导致hexo g出现问题

比如以下的这张图片，我没有直接用文字，就是因为其中一些符号会导致hexo g出错（具体哪些符号我没研究），所以我用图片代替。

![image-20210328163425428](https://gitee.com/wanwanzh/imagebed-windows/raw/master/pictures/image-20210328163425428.png)

原因是hexo在5.0之后把swig给删除了需要自己手动安装
```
npm i hexo-renderer-swig
```
### 新主题文件夹的名字要和_config.yml文件中主题对应的名字一致。





