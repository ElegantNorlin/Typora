---
title: Windows10Anaconda环境下安装face_recognition
date: 2021-3-28 16:05:00
categories: 教程
cover: /img/p18.jpg
tags: 博客
---



### 安装cmake库

* conda install cmake
### 安装dlib库
* （推荐使用）第一种方法是直接下载dlib-19.7.0-cp36-cp36m-win_amd64.whl文件
命令行进入该文件所在的文件夹，pip install dlib-19.7.0-cp36-cp36m-win_amd64.whl安装上dlib库
* 第二种方法是直接使用命令安装dlib库
conda install -c menpo dlib=x.x.x(x.x)  
### 安装face_recognition库
* pip install face_recognition



