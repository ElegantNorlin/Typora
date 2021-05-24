---
title: Anaconda常用命令
date: 2021-3-11 21:12:00
categories: 
cover: /img/p1.jpg
tags: Anaconda
---

### conda常用命令

* **conda list** 查看安装了哪些包
* **conda env list** 或 **conda info -e** 查看当前存在哪些虚拟环境
* **conda update -n base -c defaults conda** 检查更新conda
* **conda --version ** 查询conda的版本信息
* **conda -h** 查询conda的命令使用

### 创建python虚拟环境

* **conda create -n your_env_name python=x.x**  命令创建python版本为X.X、名字为your_env_name的虚拟环境。your_env_name文件可以在Anaconda安装目录envs文件下找到。

  注意：默认的情况下只安装了一些必须的包，并不会像我们安装anaconda时自动安装很多常用的包。要实现上面的功能，则须在末尾加上‘anaconda’，完整命令是：

### 切换虚拟环境

* **activate your_env_name** 切换到your_env_name虚拟环境
* **python --version** 查询当前虚拟环境下python的版本信息

### 包命令

#### 搜索包

* **conda search packagename** 搜索指定的包

#### 安装包

* **conda install packagename** 安装指定的包 
* **conda install packagename=版本号** 安装指定版本的包
* **conda install package1 package2** 安装多个包

#### 包更新

* **conda update packagename** 更新指定的包
* **conda update python** 更新python
* **conda update --all** 更新所有包

#### 包删除

* **conda remove packagename** 删除当前环境下的包
* **conda remove -n your_env_name packagename** 删除指定虚拟环境下的包
* **conda remove package1 package2** 删除多个包

### 删除环境

* **conda remove -n env_name --all**  删除环境





