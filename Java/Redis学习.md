



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
  //插入（如果已存在同名的field，会被覆盖）
  hset key field value
  hmset key field1 value1 field2 value2...
  //插入（如果已存在同名的field，不会被覆盖）
  hsetnx key field value
  
  //取出
  hget key field
  hgetall key
  
  //删除
  hdel key field1 field2...
  
  //获取field数量
  hlen key
  
  //查看是否存在
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

