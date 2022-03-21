



# The article is for mac!

### Redis的启动和关闭

启动redis服务（跑起来放一边就可以了）

```shell
redis-server
```

启动命令行redis客户端

```shell
redis-cli
```

关闭redis服务，关闭redis命令行客户端就会关闭redis服务，直接在客户端输入

```shell
shutdown
```

### Redis常用的数据类型

* string          string
* hash            HashMap
* list               LinkedList
* set               HashSet
* sorted_set  TreeSet





### String类型

* 添加/修改数据

  ```shell
  set key value
  ```

* 添加/修改多个数据

  ```shell
  mset key1 value1 key2 value2 ... 
  ```

* 获取数据

  ```shell
  get key
  ```

* 获取多个数据

  ```shell
  mget key1 key2 ...
  ```

* 删除数据

  ```shell
  del key
  ```

* 获取数据字符个数（字符串长度）

  ```shell
  strlen key
  ```

* 追加数据到原始信息后面，如果原始信息不存在则创建

  ```shell
  append key value
  ```

* String数据类型的扩展

  ```
  //增长指令，只有当value为数字时才能增长
  incr key  
  incrby key increment  
  incrbyfloat key increment 
  
  //减少指令，有当value为数字时才能减少
  decr key  
  decrby key increment
  
  //设置数据的生命周期，单位 秒
  setex key seconds value
  //设置数据的生命周期，单位 毫秒
  psetex key milliseconds value
  ```

  

### Hash

* 基本操作

  ```shell
  //添加/修改单个数据（如果已存在同名的field，会被覆盖）
  hset key field value
  //添加/修改多个数据
  hmset key field1 value1 field2 value2...
  //插入（如果已存在同名的field，不会被覆盖）
  hsetnx key field value
  
  //取出
  hget key field
  //取出全部字段
  hgetall key
  
  //删除
  hdel key field1 field2...
  
  //获取哈希表中字段field数量
  hlen key
  
  //查看是否存在置顶字段
  hexists key field
  
  //获取哈希表中所有的字段名或字段值 
  hkeys key
  hvals key
  
  //设置指定字段的数值数据增加指定范围的值 
  hincrby key field increment 
  hdecrby key field increment
  ```

* tips

  - hash类型下的value**只能存储字符串**，不允许存储其他数据类型，**不存在嵌套现象**。如果数据未获取到， 对应的值为（nil）
  - 每个 hash 可以存储 2^32 - 1 个键值
  - hash类型十分贴近对象的数据存储形式，并且可以灵活添加删除对象属性。但hash设计初衷不是为了存储大量对象而设计的，**切记不可滥用**，更**不可以将hash作为对象列表使用**
  - hgetall 操作可以获取全部属性，如果内部field过多，遍历整体**数据效率就很会低**，有可能成为数据访问瓶颈

### List

- 数据存储需求：存储多个数据，并对数据进入存储空间的顺序进行区分
- 需要的存储结构：一个存储空间保存多个数据，且通过数据可以体现进入顺序
- list类型：保存多个数据，底层使用双向链表存储结构实现
- **元素有序，且可重**

* 基本操作

  ```shell
  //添加修改数据,lpush为从左边添加，rpush为从右边添加
  lpush key value1 value2 value3...
  rpush key value1 value2 value3...
  
  //查看数据, 从左边开始向右查看. 如果不知道list有多少个元素，end的值可以为-1,代表倒数第一个元素
  //lpush先进的元素放在最后,rpush先进的元素放在最前面
  lrange key start end
  //得到长度
  llen key
  //取出对应索引的元素
  lindex key index
  
  //获取并移除元素（从list左边或者右边移除）
  lpop key
  rpop key
  ```

* 拓展操作

  ```shell
  //规定时间内获取并移除数据,b=block,给定一个时间，如果在指定时间内放入了元素，就移除
  blpop key1 key2... timeout
  brpop key1 key2... timeout
  
  //移除指定元素 count:移除的个数 value:移除的值。 移除多个相同元素时，从左边开始移除
  lrem key count value
  ```

* 注意事项

  - list中保存的数据都是string类型的，数据总容量是有限的，最多2^32 - 1 个元素 (4294967295)。
  - list具有索引的概念，但是操作数据时通常以**队列**的形式进行入队出队(rpush, rpop)操作，或以**栈**的形式进行入栈出栈(lpush, lpop)操作
  - 获取全部数据操作结束索引设置为-1 (倒数第一个元素)
  - list可以对数据进行分页操作，通常第一页的信息来自于list，第2页及更多的信息通过数据库的形式加载

### set

* 添加元素

  ```shell
  sadd set_name member1 member2 ...
  ```

* 获取全部元素

  ```shell
  smembers set_name
  ```

* 删除元素

  ```shell
  srem set_name member1 member2 ...
  ```

* 获取集合数据总量

  ```
  scard set_name
  ```

* 判断集合中是否包含指定数据

  ```
  sismember set_name member
  ```

* 随机获取集合中指定数量(count)的数据

  ```
  srandmember set_name count
  ```

* 随机获取集合中的count个数据并将该数据移出集合

  ```
  spop set_name count
  ```

* 求两个集合的交、并、差集

  ```
  sinter set1_name set2_name
  sunion set1_name set2_name
  sdiff set1_name set2_name
  ```

* 求两个集合的交、并、差集并存储到指定集合中

  ```
  sinterstore destination set1_name set2_name
  sunionstore destination set1_name set2_name
  sdiffstore destination set1_name set2_name
  ```

* 将指定数据从原始集合中移动到目标集合中

  ```
  smove source destination new_set_name
  ```

### sorted_set

* 添加数据

  ```
  zadd key score1 member1 score2 member2
  ```

* 获取全部数据

  ```
  zrange sorted_set_name start_index stop_index WITHSCORES
  zrevrange sorted_set_name start_index stop_index WITHSCORES
  ```

* 删除数据

  ```
  zrem key member member ...
  ```

* 按条件获取数据

  ```
  zrangebyscore key min max WITHSCORES LIMIT
  zrevrangebyscore key max min WITHSCORES
  ```

* 条件删除数据

  ```
  zremrangebyrank key start stop zremrangebyscore key min max
  ```

* 获取集合数据总量

  ```
  zcard key 
  zcount key min max
  ```

* 集合交、并操作

  ```
  zinterstore destination numkeys key [key ...] 
  zunionstore destination numkeys key [key ...]
  ```

* 获取数据对应的索引（排名）

  ```
  zrank key member zrevrank key member
  ```

* score值获取与修改

  ```
  zscore key member zincrby key increment member
  ```

* 获取当前系统时间

  ```
  time
  ```

### 集合的通用操作

* 删除指定集合

  ```
  del name
  ```

* 查询指定的集合是否存在

  ```
  exists name
  ```

* 获取集合的类型

  ```
  type name
  ```

* 为指定的集合设置有效期

  ```
  expire key seconds 
  pexpire key milliseconds 
  expireat key timestamp 
  pexpireat key milliseconds-timestamp
  ```

* 获取集合的有效时间

  ```
  ttl key 
  pttl key
  ```

* 将状态为时效性的集合转换为永久性

  ```
  persist key
  ```

* 查询集合

  ```
  keys pattern
  ```

  查询规则

  "*" : 匹配任意数量的任意符号

  "?" : 配合一个任意符号

  "[]" : 匹配一个指定符号

  ```
  keys * keys it* keys *heima
  
  keys ??heima keys user:?
  
  keys u[st]er:1
  
  查询所有 查询所有以it开头 查询所有以heima结尾 查询所有前面两个字符任意，后面以heima结尾 查询所有以user:开头，最后一个字符任意 查询所有以u开头，以er:1结尾，中间包含一个字母，s或t
  ```

* 重命名集合

  ```
  rename key newkey 
  renamenx key newkey
  ```

* 对所有的集合进行排序

  ```
  sort
  ```



### 数据库通用操作

**redis为每个服务提供有16个数据库，编号从0到15**

* 切换数据库

  ```
  select index
  ```

* 其他操作

  ```
  quit 
  ping 
  echo message
  ```

* 数据移动

  ```
  move key db
  ```

* 数据清除

  ```
  dbsize 
  flushdb 
  flushall
  ```



### Redis持久化

#### RDB

* RDB启动--save：手动执行一次保存操作

  ```
  save
  ```

  - dbfilename dump.rdb  设置本地数据数据库文件名，默认为dump.rdb
  - dir 设置存储.rdb文件的路径
  - rdbcompression yes 设置存储值本地数据库时是否压缩数据，默认为yes，采用LZF压缩
  - rdbchecksum yes 设置是否进行RDB文件格式校验，该校验过程在写文件和读文件过程均进行

* RDB启动--bgsave:手动启动并后台保存，不是立即执行

  ```
  //启动
  bgsave
  //满足限定范围内key的变化数量达到指定数量即进行持久化second：监控时间范 changes：监控key的变化量
  save second changes
  ```

* 优点

  * RDB是一个紧凑压缩的二进制文件，存储效率较高
  * RDB内部存储的是redis在某个时间点的数据快照，非常适合用于数据备份，全量复制等场景
  * RDB恢复数据的速度要比AOF快很多
  * 应用：服务器中每X小时执行bgsave备份，并将RDB文件拷贝到远程机器中，用于灾难恢复

* 缺点

  * RDB方式无论是执行指令还是利用配置，无法做到实时持久化，具有较大的可能性丢失数据
  * bgsave指令每次运行要执行fork操作创建子进程，要牺牲掉一些性能
  * Redis的众多版本中未进行RDB文件格式的版本统一，有可能出现各版本服务之间数据格式无法兼容现象

#### AOF

AOF（append only file）持久化：以独立日志的方式记录每次写命令，启动时在重新执行AOF文件中命令，以达到恢复数据的目的，与RDB相比可以简单描述为改记录数据为记录数据产生的过程AOF的主要作用是解决了数据持久化的实时性，目前已经是redis持久化的主流方式

* AOF功能开启

  ```
  //是否开启AOF持久化功能，默认为不开启状态
  appendonly yes|no
  
  //AOF写策略
  appendfsync always|everysec|no
  ```

* AOF写数据的三种策略

  * always
    每次写入操作均同步到AOF文件中，数据零误差，性能较低，不建议使用
  * everysec
    每秒及那个缓冲区中的指令同步到AOF文件中，数据准确性较高，性能较高，建议使用，也是默认配置
    在系统突然宕机的情况下丢失1秒内的数据
  * no
    有操作系统控制每次同步到AOF文件的周期，整体过程不可控

* AOF重写

  AOF重写
  Redis可以在 AOF体积变得过大时，自动地在后台（Fork子进程）对 AOF进行重写。重写后的新 AOF文件包含了恢复当前数据集所需的最小命令集合。 所谓的“重写”其实是一个有歧义的词语， 实际上， AOF 重写并不需要对原有的 AOF 文件进行任何写入和读取， 它针对的是数据库中键的当前值。

  Redis 不希望 AOF 重写造成服务器无法处理请求， 所以 Redis 决定将 AOF 重写程序放到（后台）子进 程里执行， 这样处理的最大好处是：

  1、子进程进行 AOF 重写期间，主进程可以继续处理命令请求。
  2、子进程带有主进程的数据副本，使用子进程而不是线程，可以在避免锁的情况下，保证数据的安全性。不过， 使用子进程也有一个问题需要解决： 因为子进程在进行 AOF 重写期间， 主进程还需要继续处理命令， 而新的命令可能对现有的数据进行修改， 这会让当前数据库的数据和重写后的 AOF 文件中的数据不一致。为了解决这个问题， Redis 增加了一个 AOF 重写缓存， 这个缓存在 fork 出子进程之后开始启用，Redis 主进程在接到新的写命令之后， 除了会将这个写命令的协议内容追加到现有的 AOF 文件之外， 还会追加到这个缓存中。

  AOF重写方式

  ```
  //手动重写
  bgrewriteaof
  
  //自动重写
  auto-aof-rewrite-min-size size 
  auto-aof-rewrite-percentage percentage
  ```

  



### 事务

* 开启事务

  ```
  multi
  ```

* 执行事务

  ```
  exec
  ```

**注意：加入事务的命令暂时进入到任务队列中，并没有立即执行，只有执行exec命令才开始执行**

* 取消事务:终止当前事务的定义，发生在multi之后，exec之前

  ```
  discard
  ```

  





