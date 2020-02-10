# reids基础知识

##  redis基本类型及特点
    1.string:
    2.hash(哈希):适用于存储对象
    3.list(列表,string有序集合):按着插入顺序排序
    4.set(集合,string无序集合):无顺序,哈希表实现,元素不允许需重复
    5.sorted set:有序集合(按分数的从小到大排序的),元素不允许需重复,从小到大排序
   
 ## 基本练习命令
 ## 1.String: 
    ```
    1. setnx key value 在key不存在的情况执行,失败:0,成功:1
    2. set key value ex time(秒):设置key过期时间
    3. setex key time(秒) value: 设置过期时间的简写
    4. ttl key:查看过期剩余时间
    ```
 ## 2.Hash:
   ```
    1. hset myhash field value：设置myhash的field为value
    2. hsetnx myhash field value：不存在的情况下设置myhash的field为value
    3. hkeys myhash：返回hash所有的field
    4. hvals myhash：返回hash所有的value
    5. hgetall myhash：获取某个hash中全部的field及value
    6. hdel myhash field：删除指定的field
    7. hmset myhash field1 value1 field2 value2：同时设置多个field  就是将java集合Map 存储到redis里的Hash里面
   ```
 ## 3.Set:
   ```
    1. sadd key value1 value2：往集合里面添加元素
　　2. smembers key：获取集合所有的元素
　　3.srem key value：删除集合某个元素
　　4.spop key：返回并删除集合中1个随机元素（可以坐抽奖，不会重复抽到某人）　　　
　　5.srandmember key：随机取一个元素
　　6.sismember key value：判断集合是否有某个值
　　7.scard key：返回集合元素的个数
　　8.smove source dest value：把source的value移动到dest集合中
　　9.sinter key1 key2 key3：求key1 key2 key3的交集
　  10.sunion key1 key2：求key1 key2 的并集
　　11.sdiff key1 key2：求key1 key2的差集
　　12.sinterstore res key1 key2：求key1 key2的交集并存在res里     
   
   ```
 
## redis总结</br>
  1.redis持久化策略之RDB和AOF:</br>
  2.redis主从复制原理分析:</br>
  3.redis哨兵机制:</br>
  4.高可用的redis集群架构:</br>
  5.redis性能优化和注意事项:</br>
  6.高并发,高可用,海量数据,备份,随时可以恢复问题</br>

## redis持久化:</br>

1.RDB持久化:</br>
  RDB持久化机制，对redis中的数据执行周期性的持久化</br>
2.AOF机制:</br>
  AOF机制对每条写入命令作为日志，以append-only的模式写入一个日志文件中，在redis重启的时候，可以通过回放AOF日志中的写入指令来重新构建整个数据集</br>

### RDB持久化机制的优点</br>

1.RDB会生成多个数据文件，每个数据文件都代表了某一个时刻中redis的数据，这种多个数据文件的方式，非常适合做冷备，可以将这种完整的数据文件发送到一些远程的   安全存储上去，比如说Amazon的S3云服务上去，在国内可以是阿里云的ODPS分布式存储上，以预定好的备份策略来定期备份redis中的数据</br>
2.RDB对redis对外提供的读写服务，影响非常小，可以让redis保持高性能，因为redis主进程只需要fork一个子进程，让子进程执行磁盘IO操作来进行RDB持久化即可</br>
3.相对于AOF持久化机制来说，直接基于RDB数据文件来重启和恢复redis进程，更加快速</br>

### RDB持久化的缺点</br>
1.如果想要在redis故障时，尽可能少的丢失数据，那么RDB没有AOF好。一般来说，RDB数据快照文件，都是每隔5分钟，或者更长时间生成一次，这个时候就得接受一旦     redis进程宕机，那么会丢失最近5分钟的数据</br>

2.RDB每次在fork子进程来执行RDB快照数据文件生成的时候，如果数据文件特别大，可能会导致对客户端提供的服务暂停数毫秒，或者甚至数秒</br>

### AOF持久化机制的优点</br>

1.AOF可以更好的保护数据不丢失，一般AOF会每隔1秒，通过一个后台线程执行一次fsync操作，最多丢失1秒钟的数据

2.AOF日志文件以append-only模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复

3.AOF日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在rewrite log的时候，会对其中的指导进行压缩，创建出一份需要恢复数据的最小日志出来。再创建新日志文件的时候，老的日志文件还是照常写入。当新的merge后的日志文件ready的时候，再交换新老日志文件即可。

4.AOF日志文件的命令通过非常可读的方式进行记录，这个特性非常适合做灾难性的误删除的紧急恢复。比如某人不小心用flushall命令清空了所有数据，只要这个时候后台rewrite还没有发生，那么就可以立即拷贝AOF文件，将最后一条flushall命令给删了，然后再将该AOF文件放回去，就可以通过恢复机制，自动恢复所有数据



### AOF持久化机制的缺点</br>

1.对于同一份数据来说，AOF日志文件通常比RDB数据快照文件更大</br>
2.AOF开启后，支持的写QPS会比RDB支持的写QPS低，因为AOF一般会配置成每秒fsync一次日志文件，当然，每秒一次fsync，性能也还是很高的</br>
3.以前AOF发生过bug，就是通过AOF记录的日志，进行数据恢复的时候，没有恢复一模一样的数据出来。所以说，类似AOF这种较为复杂的基于命令日志/merge/回放的方   式，比基于RDB每次持久化一份完整的数据快照文件的方式，更加脆弱一些，容易有bug。不过AOF就是为了避免rewrite过程导致的bug，因此每次rewrite并不是基于   旧的指令日志进行merge的，而是基于当时内存中的数据进行指令的重新构建，这样健壮性会好很多。</br>

## AOF持久化的配置</br>

AOF持久化，默认是关闭的，默认是打开RDB持久化

appendonly yes，可以打开AOF持久化机制，在生产环境里面，一般来说AOF都是要打开的，除非你说随便丢个几分钟的数据也无所谓

打开AOF持久化机制之后，redis每次接收到一条写命令，就会写入日志文件中，当然是先写入os cache的，然后每隔一定时间再fsync一下

而且即使AOF和RDB都开启了，redis重启的时候，也是优先通过AOF进行数据恢复的，因为aof数据比较完整

可以配置AOF的fsync策略，有三种策略可以选择，一种是每次写入一条数据就执行一次fsync; 一种是每隔一秒执行一次fsync; 一种是不主动执行fsync

always: 每次写入一条数据，立即将这个数据对应的写日志fsync到磁盘上去，性能非常非常差，吞吐量很低; 确保说redis里的数据一条都不丢，那就只能这样了</br>

mysql -> 内存策略，大量磁盘，QPS到多少，一两k。QPS，每秒钟的请求数量</br>
redis -> 内存，磁盘持久化，QPS到多少，单机，一般来说，上万QPS没问题</br>

everysec: 每秒将os cache中的数据fsync到磁盘，这个最常用的，生产环境一般都这么配置，性能很高，QPS还是可以上万的</br>

no: 仅仅redis负责将数据写入os cache就撒手不管了，然后后面os自己会时不时有自己的策略将数据刷入磁盘，不可控了</br>


## 企业级的数据备份和各种灾难下的数据恢复，是怎么做得呢？</br>

## redis如何通过读写分离来承载读请求QPS超过10万+?</br>
## redis 读写分离 支持高并发</br>

# redis主从架构->读写分离架构->可支持水平扩展的读高并发架构</br>

# redis主从架构</br>
### redis主从的核心机制</br>
  1.redis采用异步方式复制数据到slave节点，不过redis 2.8开始，slave node会周期性地确认自己每次复制的数据量</br>
  2.一个master node是可以配置多个slave node的</br>
  3.slave node也可以连接其他的slave node</br>
  4.slave node做复制的时候，是不会block master node的正常工作的</br>
  5.slave node在做复制的时候，也不会block对自己的查询操作，它会用旧的数据集来提供服务</br>
  6.slave node主要用来进行横向扩容，做读写分离，扩容的slave node可以提高读的吞吐量</br>

## master持久化对于主从架构的安全保障的意义</br>

如果采用了主从架构，那么建议必须开启master node的持久化！</br>

不建议用slave node作为master node的数据热备，因为那样的话，如果你关掉master的持久化，可能在master宕机重启的时候数据是空的，然后可能一经过复制，salve node数据也丢了

master -> RDB和AOF都关闭了 -> 全部在内存中</br>

master宕机，重启，是没有本地数据可以恢复的，然后就会直接认为自己IDE数据是空的

master就会将空的数据集同步到slave上去，所有slave的数据全部清空

100%的数据丢失

master节点，必须要使用持久化机制

第二个，master的各种备份方案，要不要做，万一说本地的所有文件丢失了; 从备份中挑选一份rdb去恢复master; 这样才能确保master启动的时候，是有数据的</br>

即使采用了后续讲解的高可用机制，slave node可以自动接管master node，但是也可能sentinal还没有检测到master failure，master node就自动重启了，还是可能导致上面的所有slave node数据清空故障</br>

## redis主从复制原理、断点续传、无磁盘化复制、过期key处理

## redis哨兵主备切换的数据丢失问题：异步复制、集群脑裂
要求至少有1个slave，数据复制和同步的延迟不能超过10秒
如果说一旦所有的slave，数据复制和同步的延迟都超过了10秒钟，那么这个时候，master就不会再接收任何请求了
上面两个配置可以减少异步复制和脑裂导致的数据丢失

1.减少异步复制的数据丢失</br>
有了min-slaves-max-lag这个配置，就可以确保说，一旦slave复制数据和ack延时太长，就认为可能master宕机后损失的数据太多了，那么就拒绝写请求，这样可以把master宕机时由于部分数据未同步到slave导致的数据丢失降低的可控范围内

2.减少脑裂的数据丢失</br>
如果一个master出现了脑裂，跟其他slave丢了连接，那么上面两个配置可以确保说，如果不能继续给指定数量的slave发送数据，而且slave超过10秒没有给自己ack消息，那么就直接拒绝客户端的写请求
这样脑裂后的旧master就不会接受client的新数据，也就避免了数据丢失
上面的配置就确保了，如果跟任何一个slave丢了连接，在10秒后发现没有slave给自己ack，那么就拒绝新的写请求
因此在脑裂场景下，最多就丢失10秒的数据
