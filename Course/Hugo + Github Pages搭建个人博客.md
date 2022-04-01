---
title: Hugo + Github Pages搭建个人博客
date: 2022-04-01 10:11:00
tags: 
- Git
- Hugo
- Course
---



### 1.安装Hugo

直接使用homebrew安装hugo

```shell
brew install hugo
```

### 2.创建博客

```shell
hugo new site your_blog
```

其中“your_blog”为博客文件夹的名称，执行该命令会在当前文件夹下生成”your_blog“文件夹

````shell
cd your_blog
````

进入your_blog文件夹

```shell
hugo new post/xxx.md
```

### 3.选择一款喜欢的hugo博客主题

hugo的theme很多，每一款theme的配置都大同小异，我们只需要按照说明配置好即可。

### 4.本地启动博客



配置好theme之后，我们先在本地启动起来看一下是否成功。

```shell
hugo serve -D
```

若启动成功会让你访问本地端口的地址，页面展示没问题就是成功了。

若启动失败，请网上翻阅教程寻找答案。

### 5.部署到GitHub Pages

我们可以将博客免费部署到GitHub Pages和Gitee Pages，这里我选择使用GitHub Pages

这里我假设看教程的你有GitHub账号，并且已经配置了本机的SSH。

接下来直接创建一个GitHub仓库，GitHub Pages仓库是有规定的。

* 仓库名称必须是`GitHub用户名.github.io`
* 不创建README.md文件
* public

接下来使用下面的命令在your_blog文件夹下生成public文件夹

```shell
hugo --theme=your_theme_name --baseUrl="https://GitHub用户名.github.io/" --buildDrafts
```

进入public并提交到刚刚创建的GitHub仓库

```shell
cd public
git add .
git commit -m "commit describe"
git remote add origin https://github.com/用户名/用户名.github.io.git
git push -u origin master
```

以后更新的时候我们只需要使用下面的命令提交到GitHub即可

```shell
git add .
git commit -m "commit describe"
git push
```

提交成功之后等几分钟

然后打开浏览器输入`https://Github用户名.github.io`访问就能看到你的博客啦。

我们现在的浏览器都比较智能，我们在访问博客的时候只需要输入`Github用户名.github.io`即可访问。

### 6.更新文章重新部署

当我们把新的文章添加到/content/posts中，需要使用下面的命令重新生成public中的内容

```shell
hugo --theme=your_theme_name --baseUrl="https://GitHub用户名.github.io/" --buildDrafts
```

然后进入public

```shell
cd public
git add .
git commit -m "commit describe"
git remote add origin https://github.com/用户名/用户名.github.io.git
git push -u origin master
```

