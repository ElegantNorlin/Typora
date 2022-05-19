---
title: Benchmarksql压测数据库
date: 2022-05-10
tags: 
- Benchmarksql
---

### 1.工具介绍

BenchmarkSQL是开源数据库测试工具，内嵌了TPCC测试脚本，可以对Oracle、PostgreSQL、SQL Server直接进行测试，在BenchmarkSQL 5.0版本，也可以改写相关脚本使其适用MYSQL环境。

### 2.环境介绍

操作系统：CentOS Linux release 7.6.1810 (Core)

Postgresql：11.5

Benchmarksql：5.0

jdk:

```shell
[root@192-118-0-195 ~]# java -version
openjdk version "1.8.0_322"
OpenJDK Runtime Environment (build 1.8.0_322-b06)
OpenJDK 64-Bit Server VM (build 25.322-b06, mixed mode)
```

### 3.部署环境

#### 3.1部署jdk

##### 3.1.1下载jdk压缩包

从这个网站下载jdk

[https://www.oracle.com/java/technologies/javase-jdk8-downloads.html](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)

##### 3.1.2jdk上传到服务器并解压

[CentOS安装jdk](https://blog.csdn.net/you18131371836/article/details/111462905)

##### 3.1.3在.bash_profile文件中配置jdk环境变量

环境变量的配置根据具体的安装目录按照[教程](https://blog.csdn.net/you18131371836/article/details/111462905)来

#### 3.2安装ant编译工具

这里使用CentOS自带的yum工具安装，如果你的yum工具没有配置国内源请看这篇[文章](https://elegantnorlin.github.io/posts/%E7%BD%91%E6%98%93yum%E6%BA%90/)

```shell
yum install ant
```

#### 3.3下载并编译Benchmarksql5.0

##### 3.3.1下载B enchmarksql5.0压缩包

[https://sourceforge.net/projects/benchmarksql/](https://sourceforge.net/projects/benchmarksql/)

##### 3.3.2上传压缩包并解压

我这里测试的是postgresql所以我先把用户切换成postgres用户

```shell
# 切换成postgres用户
su postgres

# 解压benchmarksql-5.0.tar
tar -zxvf benchmarksql-5.0.tar
```

### 4.配置benchmarksql

#### 4.1修改配置文件

配置文件位于

```shell
.../benchmarksql/run/props.pg
```

编辑配置文件

```shell
vim .../benchmarksql/run/props.pg
```



以下为配置文件内容

除了中文注释的内容必须修改，其余内容根据当时的要求自行设定

```shell
# 数据库名称
db=postgres
# 数据库驱动
driver=org.postgresql.Driver
# 修改成相应的数据库
conn=jdbc:postgresql://192.168.10.16:3452/benchmarksql 

# 数据库用户名
user=tbase  <<< DB用户名

# 数据库密码
password=****  <<< DB用户密码



warehouses=200

#warehouses=1000

loadWorkers=60



terminals=50

//To run specified transactions per terminal- runMins must equal zero

runTxnsPerTerminal=0

# 测试时间，单位为分钟。注意：该值设置之后，runTxnsPerTerminal参数必须为0.这两个参数同时只能一个生效
runMins=30



//Number of total transactions per minute

limitTxnsPerMin=0



//Set to true to run in 4.x compatible mode. Set to false to use the

//entire configured database evenly.

terminalWarehouseFixed=true



//To run for specified mitbaseult percentages of 45, 43, 4, 4 & 4 match the TPC-C spec

newOrderWeight=45

paymentWeight=43

orderStatusWeight=4

deliveryWeight=4

stockLevelWeight=4



// Directory name to create for collecting detailed result data.

// Comment this out to suppress.

//resultDirectory=my_result_%tY-%tm-%td_%tH%tM%tS

//osCollectorScript=./misc/os_collector_linux.py 

//osCollectorInterval=1

//osCollectorSSHAddr=ssh_user@target_dbhost

//osCollectorDevices=net_eth0 blk_sda
```

#### 4.2创建测试数据库

```shell
# 命令行进入postgres数据库
psql

# 在postgres交互式命令行中创建benchmarksql数据库
create database benchmarksql;
```



#### 4.3执行建表脚本

```shell
# 进入到你的/benchmarksql/run
cd .../benchmarksql/run

# 执行建表脚本
./runDatabaseBuild.sh props.pg
```

#### 4.4执行benchmarksql测试脚本

```shell
./runBenchmark.sh props.pg
```

#### 4.5执行删除数据库测试数据脚本

```
./runDatabaseDestroy.sh props.pg
```

#### 4.6执行结果可视化生成脚本生成html文档

注意脚本后的参数是你的benchmarksql/run/下生成的文件夹，请换成你对应的文件夹名称，否则会报错

还有，执行该脚本需要使用R语言环境，安装R语言环请看这篇[文章](https://cloud.tencent.com/developer/article/1481902)。

```shell
./generateReport.sh my_result_**********
```



