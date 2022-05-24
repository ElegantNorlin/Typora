---
title: Benchmarksql5.0压力测试MySQL
date: 2022-05-24
tags:
- Linux
- MySQL
---

Benchmarksql5.0取消了对MySQL的支持，但是通过修改部分代码仍然可以实现Benchmarksql对MySQL的支持。

本教程是作者根据网上资料和自身实现的时候遇到的问题综合得到的。



#### 1\. 环境:

*   CentOS 7.6
*   benchmarksql 5.0
*   MySQL5.7

#### 2\. 下载编译benchmarksql源码

2.1 首先安装java开发环境，具体步骤略过，本文涉及到的操作在 java 1.8.0 环境下测试通过。

2.2 安装ant工具。
yum install ant

2.3 下载benchmarksql 5.0 源码，解压。
[https://sourceforge.net/projects/benchmarksql/](https://sourceforge.net/projects/benchmarksql/)
cd benchmarksql-5.0

2.4 编译
ant

此时会编译出一个版本 benchmarksql-5.0/dist/BenchmarkSQL-5.0.jar，但是该版本并不支持MySQL的TPC-C测试，需要做如下的修改。

#### 3 修改benchmarksql源码

3.1 修改benchmarksql-5.0/src/client/jTPCC.java，增加mysql相关部分，如下所示：

```java
if (iDB.equals("firebird"))
        dbType = DB_FIREBIRD;
    else if (iDB.equals("oracle"))
        dbType = DB_ORACLE;
    else if (iDB.equals("postgres"))
        dbType = DB_POSTGRES;
    else if (iDB.equals("mysql"))
        dbType = DB_UNKNOWN;
    else
    {
        log.error("unknown database type '" + iDB + "'");
        return;
    }
```



3.2 修改benchmarksql-5.0/src/client/jTPCCConnection.java， SQL子查询增加”AS L”别名，如下所示：

```java
default:
        stmtStockLevelSelectLow = dbConn.prepareStatement(
            "SELECT count(*) AS low_stock FROM (" +
            "    SELECT s_w_id, s_i_id, s_quantity " +
            "        FROM bmsql_stock " +
            "        WHERE s_w_id = ? AND s_quantity < ? AND s_i_id IN (" +
            "            SELECT ol_i_id " +
            "                FROM bmsql_district " +
            "                JOIN bmsql_order_line ON ol_w_id = d_w_id " +
            "                 AND ol_d_id = d_id " +
            "                 AND ol_o_id >= d_next_o_id - 20 " +
            "                 AND ol_o_id < d_next_o_id " +
            "                WHERE d_w_id = ? AND d_id = ? " +
            "        ) " +
            "    )AS L");
        break;
```



3.3 编译修改后的源码，此时得到的benchmarksql版本 benchmarksql-5.0/dist/BenchmarkSQL-5.0.jar 已经支持MySQL的TPC-C测试。

```shell
cd benchmarksql-5.0
ant
```

#### 4 修改相关脚本，支持MySQL

4.1. 在benchmarksql-5.0/run目录下，创建文件prop.mysql，内容如下：

```
//数据库名称
db=mysql
//mysql驱动
driver=com.mysql.jdbc.Driver
//mysql主机信息
conn=jdbc:mysql://127.0.0.1:3306/benchmarksql
//mysql的用户名
user=benchmarksql
//用户名密码
password=123456
//仓库数量
warehouses=1
//家在线程
loadWorkers=4
//客户端并发访问数量
terminals=1
//To run specified transactions per terminal- runMins must equal zero
runTxnsPerTerminal=0
//To run for specified minutes- runTxnsPerTerminal must equal zero
//这个参数与runTxnsPerTerminal参数只能同时生效一个，另一个参数需要设置为0，runMins单位为分钟
runMins=10
//Number of total transactions per minute
limitTxnsPerMin=300
//Set to true to run in 4.x compatible mode. Set to false to use the
//entire configured database evenly.
terminalWarehouseFixed=true
//The following five values must add up to 100
//The default percentages of 45, 43, 4, 4 & 4 match the TPC-C spec
newOrderWeight=45
paymentWeight=43
orderStatusWeight=4
deliveryWeight=4
stockLevelWeight=4
// Directory name to create for collecting detailed result data.
// Comment this out to suppress.
resultDirectory=my\_result\_%tY-%tm-%td\_%tH%tM%tS
osCollectorScript=./misc/os\_collector\_linux.py
osCollectorInterval=1
//远程连接mysql主机进行TPCC性能测试
//osCollectorSSHAddr=user@dbhost
osCollectorDevices=net\_eth0 blk\_sda
```

4.2. 修改 文件：benchmarksql-5.0/run/funcs.sh，添加mysql 数据库类型。

```shell
function setCP()
{
    case "$(getProp db)" in
    firebird)
        cp="../lib/firebird/*:../lib/*"
        ;;
    oracle)
        cp="../lib/oracle/*"
        if [ ! -z "${ORACLE_HOME}" -a -d ${ORACLE_HOME}/lib ] ; then
        cp="${cp}:${ORACLE_HOME}/lib/*"
        fi
        cp="${cp}:../lib/*"
        ;;
    postgres)
        cp="../lib/postgres/*:../lib/*"
        ;;
    mysql)
        cp="../lib/mysql/*:../lib/*"
        ;;
    esac
    myCP=".:${cp}:../dist/*"
    export myCP
}

...省略

case "$(getProp db)" in
    firebird|oracle|postgres|mysql)
    ;;
    "") echo "ERROR: missing db= config option in ${PROPS}" >&2
    exit 1
    ;;
    *)  echo "ERROR: unsupported database type 'db=$(getProp db)' in ${PROPS}" >&2
    exit 1
    ;;
esac
```



4.3 添加mysql java connector驱动，mysql-connector-java-5.1.45.jar 需自行下载。

```shell
mkdir -p benchmarksql-5.0/lib/mysql
cp mysql-connector-java-5.1.45.jar benchmarksql-5.0/lib/mysql/
```

4.4 修改benchmarksql-5.0/run/runDatabaseBuild.sh，去掉`extraHistID`

`AFTER\_LOAD="indexCreates foreignKeys extraHistID buildFinish"
修改为：
AFTER\_LOAD="indexCreates foreignKeys buildFinish"`

#### 5 测试MySQL TPC-C

```shell
cd benchmarksql-5.0/run
//执行建表脚本
./runDatabaseBuild.sh props.mysql
//执行TPCC测试脚本
./runBenchmark.sh props.mysql
```



