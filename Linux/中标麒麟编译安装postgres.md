---
title: 中标麒麟编译安装postgres
date: 2022-04-25
tags:
- Linux
- postgres
---

### 环境介绍

* CPU：在Loongson-3B3000处理器
* OS：NeoKylin Linux Server release 7.0 (loongson)
* postgresQL source编译安装（非二进制）

### 执行configure脚本

```shell
cd postgresql-11.5
# 执行脚本
./configure
```

**在龙芯的服务器上执行上面的脚本时，会报缺少依赖库的错误，找到错误，直接安装依赖库即可解决该问题**

### 编译

```shell
# 脚本执行成功后开始编译
# make大概用了10分钟左右
make
make install
```

### 创建用户及用户组 给data目录分配权限

```shell
cd /usr/local/pgsql/
mkdir data
groupadd postgres
useradd -g postgres postgres
chown postgres:postgres data
```

### 配置环境变量

```shell
vim /etc/profile

export PGHOME=/usr/local/pgsql
export PGDATA=/usr/local/pgsql/data
export PGLIB=/usr/local/pgsql/lib
export PATH=$PGHOME/bin:$PATH

source /etc/profile
```

### 切换到postgres用户,initdb初始化

```shell
su postgres
initdb -D /usr/local/pgsql/data/
```

### 启动和停止服务

```shell
# 启动服务
pg_ctl start

# 停止服务
pg_ctl stop
```

### 修改配置文件

```shell
cd  /usr/local/pgsql/data
vim postgresql.conf
# 设置所有人监听
listen_addresses = '*'
vim pg_hba.conf
# IPv4 local connections:   
# 连接方式	连接的数据库	连接的用户	连接的主机IP	认证方式
host   all      all       0.0.0.0/0         trust
```

重启服务

### 相关命令

```shell
# 查看运行的postgres进程
ps -ef | grep postgres
```

### 修改linux系统postgres用户密码

首先切换到root用户

```shell
su root
```

删除系统用户postgres密码

```shell
sudo -u postgres passwd
```

系统提示输入新密码

```shell
Enter new UNIX password:

Retype new UNIX password:

passwd: password updated successfully
```

### 修改数据库默认用户密码

postgres数据库安装后会有一个默认用户postgres

密码为空，因为后面一些操作会用到密码，所以必须要设置postgres数据库用户的密码

那么怎么设置密码呢？

```shell
# 登录pg数据库客户端
sudo -u postgres psql

# 在客户端中修改postgres数据库用户的密码
ALTER USER postgres WITH PASSWORD 'postgres';

#退出postgres客户端
/q
```

