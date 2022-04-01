

## 命令行

Mac OS 是基于unix os开发的，和Linux是一个“祖宗”，Mac 和Linux的命令行操作很类似。 



⚠️

* 终止一个发疯的或者错误的命令，可以使用command + C
* tab键补全路径
* command + L清屏
* command + space 聚焦（全局搜索）

### 常用命令

* 目录操作

  > * mkdir dirname 创建目录
  > * rmdir dirname 删除文件夹
  > * mvdir dir1 dir2 移动文件夹
  > * cd dirname 切换路径
  > * pwd 显示当前工作路径
  > * ls 显示当前目录下的内容（文件和文件夹）
  >   * ls -la显示隐藏文件
  > * dircmp dir1 dir2 比较两个目录的内容

* 文件操作

  > * cat filename 显示或连接文件
  > * pg filename 分页显示
  > * more filename 分屏显示文件内容
  > * od -c filename 显示非文本文件的内容
  > * rm filename 删除文件
  > * mv filename 移动文件
  > * file filename 显示文件类型
  > * open filename  (open .打开当前目录)
  >   * open -a application.app 打开应用

* 选择操作

  >* head -20 filename 查看文件的前20行
  >* tail -20 filename 查看文件的后20行
  >* cut 显示文件每一行中的某些域
  >* diff 比较并显示两个文件的差异
  >* sort 排序或归并文件
  >* wc 统计文字的字符数、词数、行数

* 安全操作

  > * passwd 修改用户密码
  > * chmod 改变文件或目录的权限
  > * umask 定义创建文件的权限掩码
  > * chown 改变文件或目录的属主
  > * chgrp 改变文件或目录的所属组

* 进程操作

  > * ps u 显示进程当前状态
  > * kill 杀死进程

* 时间操作

  > * date 显示系统的当前日期和时间
  > * cal 显示日历

* 网络与通信操作

  > * ping 给一个网络主机发送回应请求

* 其他命令

  > * uname -a 显示操作系统的有关信息
  > * clear 清除屏幕或窗口内容
  > * env 显示当前所有设置过的环境变量
  > * who 列出当前登录的所有用户
  > * whoami 显示当前正在操作的用户
  > * tty 显示终端或伪终端的名称
  > * du 查询磁盘的使用情况
  > * df 显示文件系统的总空间和可用空间
  > * w 显示当前系统活动的总信息

### zsh执行.sh文件的方法

* 给.sh文件x权限（即可执行权限）
* cd 到.sh文件所在的文件夹
* 命令行输入 **./xxx.sh**

### smartmontools工具查看ssd读写情况

* 安装smartmontools

  ````shell
  brew install smartmontools
  ````

* 查看SSD读写情况

  ```shell
  smartctl --all /dev/disk0
  ```

  

### 清理DNS缓存

有些时候某些网站变更了DNS域名服务器地址，而我们的电脑DNS记录仍然是已经失效的DNS地址，这样我们访问该网站就无法访问，这时候需要清理DNS缓存，让我们的电脑直接从网站获取新的DNS信息。

* mac清理DNS缓存

  ```
  sudo killall -HUP mDNSResponder
  ```

* win清理DNS缓存

  ```
  ipconfig /flushdns
  ```

* chrome清楚DNS缓存

  ```
  chrome://net-internals/#dns
  ```

  然后点击[clear host cache]

### 应用文件损坏问题

如果在安装应用的时候出现文件损坏，请移入废纸篓的提示，出现可能有两种原因：

* 默认不允许使用第三方软件，需要开启安装任意源应用的权限。
* 可能是解压文件时把文件损坏了，可以尝试使用mac自带的“归档实用工具”

### 命令行结束进程

* 查看进程信息包括端口信息

  ```shell
  lsof -i
  ```

* 杀死进程

  ```shell
  kill -9 PID
  ```


### Xcode

* 查看Swift版本

  ```shell
  xcrun swift -version
  ```

* 查看Xcode的位置

  ```shell
  xcrun --find swift
  ```


### homebrew软件管理常用的命令

* 更新软件包

  ```shell
  brew upgrade
  ```

* 更新homebrew

  ```shell
  brew update
  ```

* 清理brew安装的旧版本的软件包

  ```shell
  brew cleanup
  ```

* 或者直接一条命令执行3条shell语句

  ```shell
  brew upgrade && brew update && brew cleanup
  ```


### 查看隐藏文件

`command` + `shift` + `.`

### 命令行显示、隐藏`隐藏文件`

```shell
# 隐藏命令
defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder

# 显示命令
defaults write com.apple.finder AppleShowAllFiles -boolean true; killall Finder
```

### Item2更改前缀名字

vi打开ohmyzsh主题，找到prompt_context()，更改前缀名字即可

```shell
prompt_context() {

  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then

    prompt_segment black default "前缀名字"

  fi

}
```





