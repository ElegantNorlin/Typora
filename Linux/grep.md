---
title: grep
date: 2022-04-02
tags: 
- Linux
- Shell
---

### 1.grep introduce

`grep`全称Global Regular Expression Print，表示全局正则表达式,是一个强大的文本搜索工具，采用正则匹配。

#### 1.1grep命令格式

```shell
grep [options] String files
```

#### 1.2grep主要参数

* -c:只输出匹配行的数目
* -i:不区分大小写
* -n:显示匹配行及行号
* -l:查询多文件时只输出包含匹配字符的文件名
* -v:反向匹配，即显示不匹配的行
* -h:查询的时候不适用文件名
* -s:不显示错误信息
* -r:递归对目录及其子目录下的文件进行grep

#### 1.4部分正则表达式

* \      反义字符
* ^$   表示开始和结束
* []     匹配单个字符  
* [ - ]  匹配一个范围，[0-9a-zA-Z]匹配所有数字和字母
* \*      前面的字符出现了0次或者多次
* \+      前面的字符出现了1次或者多次
* .       任意字符

### 2.grep使用案例

#### 2.1`grep`结合`ps`和`|`查找对应进程并写入文件

```shell
ps aux|grep ssh > a.txt
```

#### 2.2递归当前目录及其子目录的文件grep

```shell
grep r keyword
```





