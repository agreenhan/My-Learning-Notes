# Redis安装

- 官网：http://redis.io/
- 安装版本：Redis 6

## 下载安装最新版的gcc编译器

- 根据不同品牌的linux系统百度

## 测试GCC版本

- gcc --version

## 安装步骤

- 下载redis rar.gz放在选定目录下
- 解压命令：tar -zxvf redis-6.2.1.tar.gz
- 解压完成后进入目录：cd redis-6.2.1
- 在redis-6.2.1目录下再次执行make命令，将redis文件编译
- 如果没有安装好C语言环境，make会报错：Jemalloc/jemalloc.h：没有那个文件，解决方案运行make distclean
- 继续运行make install
- 默认安装目录：/usr/local/bin

## 默认安装目录

| 文件             | 作用                                                   |
| ---------------- | ------------------------------------------------------ |
| redis-benchmark  | 性能测试工具，可以在自己本子运行，看看自己本子性能如何 |
| redis-check-aof  | 修复有问题的AOF文件                                    |
| redis-check-dump | 修复有问题的dumo.rdb文件                               |
| redis-sentinel   | Redis集群使用                                          |
| redis-server     | Redis服务器启动命令                                    |
| redis-cli        | 客户端，操作入口                                       |

## 前台启动（不推荐）

- 命令：redis-server
- ![image-20220729094653067](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220729094653067.png)
- 如果窗口关闭，则不能使用redis，局限性较大

## 后台启动

1. 备份redis.conf文件，linux命令：cp redis.conf 复制文件名
2. 修改复制文件，后台启动设置daemonize no改为yes
3. Redis启动：redis-server 复制文件地址
4. 用客户端访问：redis-cli
5. Redis关闭(多种途径):
   1. 单实例关闭：redis-cli shutdown
   2. 杀redis进程（kill命令）
   3. 多实例关闭，指定端口号关闭：redis-cli -p 6379 shutdown

# Redis相关知识

## 数据库

- redis默认16个数据库，类似数组下标从0开始，初始默认使用0号库
- 使用select <dbid> 来切换数据库。如：select 8
- 统一密码管理，所有数据库用的是同一个密码
- dbsize查看当前数据库的key的数量
- flushdb清空当前数据库
- flushall通杀全部数据库

## 多路复用

- redis是单线程+多路IO复用技术

# 常用的五大数据类型

## Key（键）

### 常用命令

| 命令            | 作用                                                         |
| --------------- | :----------------------------------------------------------- |
| keys *          | 查看当前库所有key（匹配：keys *1)                            |
| exists key      | 判断某个key是否存在，0表示不存在，1表示存在                  |
| type key        | 查看key的类型                                                |
| del key         | 删除指定的key数据                                            |
| unlink key      | 根据value选择非阻塞删除（仅将keys从keyspace元数据中删除，真正的删除会在后续的异步操作） |
| expire key 时间 | 为给定的key设置过期时间，单位为秒                            |
| ttl key         | 查看还有多少秒过期，-1表示永不过期，-2表示已过期             |

## 字符串（String）

### 简介

- String是Redis最基本的类型
- String类型是二进制安全的，意味着Redis的String可以包含任何数据。比如jpg图片或者序列化的对象
- Redis中一个字符串value最多可以是512MB

### 常用命令

| 命令                                  | 作用                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| set <key> <value> [EX\|PX\|] [NX\|XX] | 添加键值对，<br />NX：当数据库中key不存在时，可以将key-value添加到数据库<br />XX：当数据库中key存在时，可以将key-value添加到数据库，与NX参数互斥<br />EX：key的超时秒数<br />PX：key的超时毫秒数，与EX互斥 |
| get <key>                             | 获取对应键的值                                               |
| append <key> <value>                  | 将给定的<value>追加到原值的末尾                              |
| strlen <key>                          | 获取值的长度                                                 |
| setnx <key> <value>                   | 只有当key不存在时，设置key的值                               |
| incr <key>                            | 将key中存储的数字值增1，只能对数字值操作，如果为空，新增值为1 |
| decr <key>                            | 将key中存储的数字值减1，只能对数字值操作，如果为空，新增值为-1 |
| incrby <key> <step>                   | 将key中存储的数字值增step，自定义步长                        |
| decrby <key> <step>                   | 将key中存储的数字值减step，自定义步长                        |
| mset <key1> <value> <key2>...         | 同时设置一个或多个key-value对                                |
| mget <key1> <value> <key2>...         | 同时获取一个或多个value                                      |
| msetnx <key1> <value> <key2> ...      | 同时设置一个或多个key-value对，当且仅当所有给定的key都不存在<br />（有一个失败则都失败） |
| getrange <key> <起始位置> <结束位置>  | 获取值的范围，类似java中的substring，前包，后包              |
| setrange <key> <起始位置> <value>     | 用<value>覆写<key>所存储的字符串值，从<起始位置>开始（索引从0开始） |
| setex <key> <过期时间> <value>        | 设置键值对，同时设置过期时间，单位秒                         |
| getset <key> <value>                  | 以旧换新，设置了新值同时获得旧值                             |

#### 原子性

- 即原子操作，不会被线程调度机制打断的操作
- 这种操作一旦开始，就一直运行到结束，中间不会有任何context switch（切换到另一个线程）
  - 在单线程中，能够在单条指令中完成的操作都可以认为是“原子操作”，因为中断只能发生于指令之间。
  - 在多线程中，不能被其他进程（线程）打断的操作叫原子操作。
- Redis单命令的原子性主要得益于Redis的单线程。

### 数据结构

- String的数据结构为简单动态字符串（Simple Dynamic String，SDS），是可以修改的字符串。
- 内部结构实现上类似java中的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。
- 当前字符串实际分配的空间capacity一般高于实际字符串长度。
- 当字符串长度小于1M时，每次扩容现有空间capacity的一倍；当字符串长度大于1M时，扩容一次只会最多扩容1M的空间。
- 字符串最大长度为512MB

## 列表（List）

### 简介

- 单键多值
- Redis列表是简单的字符串列表，按照插入顺序排序，可以添加一个元素到列表的头部或者尾部。
- 底层实际是一个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能差。

### 常用命令

| 命令                                          | 作用                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| lpush <key> <value1> <value2>....             | 从左边插入一个或多个值                                       |
| rpush <key> <value1> <value2>....             | 从右边插入一个或多个值                                       |
| lpop <key>                                    | 从左边弹出一个值（键在值在，键亡值亡）                       |
| rpop <key>                                    | 从右边弹出一个值（键在值在，键亡值亡）                       |
| rpoplpush <key1> <key2>                       | 从<key1>列表右边吐出一个值，插到<key2>列表左边               |
| lrange <key> <start> <stop>                   | 按照索引下标获得元素（从左到右）<br />取到所有值：lrange key 0 -1 |
| lindex <key> <index>                          | 按照索引下标获得元素（从左到右）                             |
| llen <key>                                    | 获取列表长度                                                 |
| linsert <key> before/after <value> <newvalue> | 在<value>的后面/前面插入<newvalue>插入值                     |
| lrem <key> <n> <value>                        | 从左边删除n个value（从左到右）                               |
| lset <key> <index> <value>                    | 将列表key下标为index的值替换为value                          |

### 数据结构

- 数据结构为：快速链表（quickList）
- 首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，即压缩列表。它将所有的元素紧挨着一起存储，分配的是一块连续的内存空间。
- 当数据量较多时，才会改用quickList，因为普通的链表需要的附加指针空间太大，会比较浪费时间。比如这个列表里存的只是int类型的数据，结构上还需要两个额外的指针prev和next。所以Redis将链表和ziplist结合起来组成了quicklist，也就是将多个ziplist使用双向指针串起来使用，这样既满足了快速的插入删除功能，又不会出现太大的空间冗余。

## 集合（Set）

### 简介

- Redis的set对提供的功能与list类似，是一个列表的功能。
- set是可以自动排重的，当需要存储一个列表数据，且不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，而list不能提供。
- Set是String类型的无序集合。
- 底层其实是一个value为null的hash表，所以添加、删除、查找的复杂度都是O(1) 

### 常用命令

| 命令                                 | 作用                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| sadd <key> <value1> <value2>...      | 将一个或多个member元素加入到集合key中，已经存在的member元素会被忽略 |
| smember <key>                        | 取出该集合的所有值                                           |
| sismember <key> <value>              | 判断集合<key>是否含有<value>，有返回1，无返回0               |
| scard <key>                          | 返回该集合的元素个数                                         |
| srem <key> <value1> <value2>...      | 删除集合中的某个或某些元素                                   |
| spop <key>                           | 随机从该集合中吐出一个值                                     |
| srandmember <key> <n>                | 随机从该集合中取出n个值，但取出的值不会从集合中删除          |
| smove <source> <destination> <value> | 把一个集合中的值移动到另一个集合                             |
| sinter <key1> <key2>                 | 返回两个集合的交集元素                                       |
| sunion <key1> <key2>                 | 返回两个集合的并集元素                                       |
| sdiff <key1> <key2>                  | 返回两个集合的差集元素（key1中的，但不包含key2中的）         |

### 数据结构

- 数据结构是dict字典，字典是用哈希表实现的
- Java中的HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也一样，它的内部也试用hash结构，所有的value都指向同一个内部值。

## 哈希（Hash）

### 简介

- Redis中的Hash是一个键值对集合
- 是一个string类型的field和value的映射表，hash特别适合用于存储对象，类似java中的Map<String,Object>

### 常用命令

| 命令                                                 | 作用                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| hset <key> <field> <value>                           | 给<key>集合中的<field>键赋值<value>                          |
| hget <key1> <field>                                  | 从<key1>集合<field>取出value                                 |
| hmset <key1> <field1> <value1> <field2> <value2>.... | 批量设置hash的值                                             |
| hexists <key1> <field>                               | 查看哈希表key中，给定域field是否存在                         |
| hkeys <key>                                          | 列出该hash集合的所有field                                    |
| hvals <key>                                          | 列出该hash集合的所有value                                    |
| hincrby <key> <field> <increment>                    | 为哈希表key中域field的值加上增量1 -1                         |
| hsetnx <key> <field> <value>                         | 将哈希表key中的域field的值设置为value，<br />当且仅当域field不存在 |

### 数据结构

- Hash类型对应数据结构有两种：ziplist（压缩列表）、hashtable（哈希表）
- 当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable

## 有序集合Zset（Set）

### 简介

- 与普通set非常相似，是一个没有重复元素的字符串集合，不同之处是有序集合的每个成员都关联了一个评分（score），这个评分（score）被用来按照从最低分到最高分的方式排序集合中的成员。
- 集合中的成员是唯一的，但是评分是可以重复的
- 因为元素是有序的，所以可以很快地根据评分（score）或者次序（position）来获取一个范围的元素
- 访问有序集合的中间元素也是非常快的，因此你能够使用有序集合作为一个没有重复成员的智能列表

### 常用命令

| 命令                                                         | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| zadd <key> <score1> <value1> <score2> <value2> ...           | 将一个或多个member元素及其score值加入到有序集key中           |
| zrange <key> <start> <stop> [WITHSCORES]                     | 返回有序集key中，下标在<start><stop>之间的元素<br />带有WITHSCORES，可以让分数一起和值返回到结果集 |
| zrangebyscore key min max [withscores] [limit offset count]  | 返回有序集key中，所有score值介于min和max之间<br />（包括min和max）的成员。有序集成员按score值递增次序排序 |
| zrevrangebyscore key max min [withscores] [limit offset count] | 同上，改为降序排序                                           |
| zincrby <key> <increment> <value>                            | 为元素的score加上增量                                        |
| zrem <key> <value>                                           | 删除该集合下，指定值的元素                                   |
| zcount <key> <min> <max>                                     | 统计该集合，分数区间内的元素个数                             |
| zrank <key> <value>                                          | 返回该值在集合中的排名，从0开始                              |

### 数据结构

- ScoredSet（zset）是Redis提供的一个非常特别的数据结构，一方面它等价与java的数据结构Map<String,Double>，可以给每一个元素value赋予一个权重score，另一方面它又类似TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。
- zset底层使用了两个数据结构
  - hash，作用是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。
  - 跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。

### 跳跃表（跳表）

# 配置文件

## Units单位

- 配置大小单位，开头定义了一些基本的度量单位，只支持bytes，不支持bit，大小写不敏感

## INCLUDE包含

- 用于包含其他文件内容

## 网络相关配置

- bind：配置可访问Redis的ip，默认是127.0.0.1，只能本机访问，注释后可以用其他地址访问
- protected-mode：是否开启保护模式，yes表示开启保护模式，只能本地访问。
- 端口号（Port）默认6379
- tcp-backlog
- time-out：默认是0，表示永不超时
- tcp-keepalive：检查心跳时间，如果不提供服务则释放连接

## GENERAL通用

- daemonize：是否为后台进程
- pidfile：存放pid文件的位置，每个实例会产生一个不同的pid文件
- loglevel：设置日志级别
- logfile：设置输出日志的文件路径
- database：默认的数据库数量

## SECURITY安全

- 设置密码：
  1. config get requirepass
  2. config set requirepass "123456"
  3. auth 123456

## LIMIT限制

- maxclients：
  - 设置redis同时可以和多少个客户端进行连接
  - 默认情况下为10000个客户端
  - 如果达到此限制，redis则会拒绝新的连接请求，并且向这些连接请求方发送出“max number of clients reached”以作回应
- ![image-20220729191944873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220729191944873.png)
- ![image-20220729192003463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220729192003463.png)

# 发布和订阅

## 什么是发布和订阅

- 发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接收消息。
- Redis客户端可以订阅任意数量的频道

## 发布订阅命令行实现

1. 打开一个客户端订阅channel1：SUBSCRIBE channel1
2. 打开另一个客户端，给channel1发布消息hello：publish channel1 hello
3. 打开第一个客户端可以看到发送的信息

# Redis6新数据类型

## Bitmaps

### 简介

- 这个类型可以实现对位的操作
  - 本身不是一种数据类型，实际上它是字符串（key-value），但是它可以对字符串的位进行操作
  - Bitmaps单独提供了一套命令，所以在Redis中使用Bitmaps和使用字符串的方法不太相同，可以把Bitmaps想象成一个以位为单位的数组，数组的每个单元只存储0和1，数组的下标在Bitmaps中叫做偏移量。

### 常用命令

| 命令                                     | 作用                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| bitop and(or/not/xor) <destkey> [key...] | 是一个复合操作，它可以做多个Bitmaps的and、or、not、xor操作<br />并将其结果保存在destkey中 |
| setbit <key> <offset> <value>            | 设置Bitmaps中某个偏移量的值（0或1）                          |
| getbit <key> <offset>                    | 获取Bitmaps中某个偏移量的值                                  |
| bitcount <key> [start end]               | 统计字符串被设置为1的bit数，start和end表示字节               |



## HyperLogLog

### 简介

- 用来做基数统计的运算
- 当输入元素的数量或体积非常大时，计算基数所需的空间总是固定的，并且很小。

### 常用指令

| 命令                                         | 作用                                              |
| -------------------------------------------- | ------------------------------------------------- |
| pfadd <key> <element> [element....]          | 添加指定元素到HyperLogLog中，成功返回1，失败返回0 |
| pfcount <key> [key...]                       | 计算HLL的近似基数，可以计算多个HLL                |
| pfmerge <destkey> <sourcekey> [sourcekey...] | 将一个或多个HLL合并后的结果存储在另一个HLL中      |

## Geospatial

### 简介

- 这种类型就是元素的二维坐标，在地图上就是经纬度
- redis基于该类型，提供了经纬度设置、查询、范围查询、距离查询、经纬度Hash等操作

### 常用命令

| 命令                                                         | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| geoadd <key> <longtitude> <latitude> <member> <br />[<longtitude> <latitude> <member>....] | 添加地理位置（经度、纬度、名称）<br />当坐标位置超出范围，则返回错误 |
| geopos <key> <member> [member...]                            | 获取指定地区的坐标值                                         |
| geodist <key> <member1> <member2> [m\|km\|ft\|mi]            | 获取两个位置之间的直线距离（默认米）                         |
| georadius <key> <longtitude> <latitude> radius m\|km\|ft\|mi | 以给定的经纬度为中心，找出某一半径内的元素                   |

# Jedis

- 用于在Java中操作Redis数据库

## 连接Jedis

- 首先修改redis.conf文件中的内容，详情见配置文件
- 如果配置文件没有问题，查看ip地址和端口是否正确
- 如果都没有问题，查看Linux系统的防火墙是否关闭

## 常用操作

# SpingBoot整合Redis





























