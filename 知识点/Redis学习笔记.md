#一.基本数据结构学习
##1.1.链表结构（双向链表结构）
- 链表节点

	    typedef struct listNode{
			struct listNode *prev;
			strcut listNode *next;
			void *value;
		}listNode;
- 链表

    	typedef struct list{
			listNode *head;
			listNode *tail;
			unsigned long len;
			void *(*dup)(void *ptr);
			void (*free)(void *ptr);
			int (*match)(void *ptr,void *key);
		}list;
###1.1.1.利用list表头管理链表信息
##1.2.简单动态字符串（SDS）
###1.2.1.表头信息用来存放sds的信息
    struct sdshdr { 
		int len; //buf中已占用空间的长度 
		int free; //buf中剩余可用空间的长度 
		char buf[]; //初始化sds分配的数据空间，而且是柔性数组（Flexible array member） 
	};
- 通过未使用空间，SDS使用了优化策略
	- 空间预分配，减少连续执行字符串增长操作所需的内存重分配次数。
	- 惰性空间释放：当要缩短SDS保存的字符串时，程序并不立即使用内存充分配来回收缩短后多出来的字节，而是使用表头的free成员将这些字节记录起来，并等待将来使用。

###1.2.4.杜绝缓冲区溢出（先进行内存扩展，在进行追加）


##1.3.字典结构（哈希表，链接法解决冲突）
- 使用哈希表作为底层实现

    	typedef struct dictht{
			//哈希表数组
			dictEntry **table;
		 	//哈希表大小
			unsigned long size;
			//哈希表大小掩码，用于计算索引值
			unsigned long sizemask;
			//该哈希表已有节点的数量
			unsigned long used;
		}dictht;
- 哈希表节点

    	typedef struct dictEntry{
			void *key;
			union{
				void *val;
				uint64 _tu64;
				int64 _ts64;
			}v;
			struct dictEntry *next;
		}dictEntry;
- 字典

    	typedef struct dict{
			//类型特定函数
			dictType *type;
			//私有数据
			void *privdata;
			//哈希表
			dictht ht[2];
			//rehash索引
			int trehashidx;
		}dict;

###1.3.1.两张hash表
###1.3.2.rehash操作步骤
- 1.扩展或收缩 （扩展：ht[1]的大小为第一个大于等于ht[0].used * 2的 2n次幂；收缩：ht[1]的大小为第一个大于等于ht[0].used的 2的n次幂）；
- 2.将所有的ht[0]上的节点rehash到ht[1]上；
- 3.释放ht[0]，将ht[1]设置为第0号表，并创建新的ht[1]）

> 渐进式rehash



##1.4.跳跃表
- 跳跃表节点

    	typedef struct zskiplistNode{
			//层
			struct zskiplistLevel{
				//前进指针
				struct zskiplistNode *forward;
				//跨度
				unsigned int span;
			}level[];
			//后退指针
			struct zskiplistNode *backward;
			//分值
			double score;
			//成员对象
			robj *obj;
		}zskiplistNode;

- 跳跃表

    	typedef struct zskiplist{
			struct zskiplistNode *header,*tail;
			//表中节点的数量
			unsigned long length;
			//表中层数最大的节点的层数
			int level;
		}zskiplist;

- 跳跃表的结构与查询过程

![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E8%B7%B3%E8%A1%A8%E6%9F%A5%E8%AF%A2%E8%BF%87%E7%A8%8B.png)
###1.4.1.幂次定律：返回一个随机层数值，随机算法所使用的幂次定律。
###1.4.2.redis中实现有序集合的办法是：跳跃表+哈希表（1.跳跃表元素有序，而且可以范围查找，且比平衡树简单；2.哈希表查找单个key时间复杂度性能高）




##1.5.整数集合
    typedef strcut intset{
		uint32_t encoding;
		uint32_t length;
		int8_t contents[];
	}intset;
###1.5.1.数据的升级（内存）
    


##1.6.压缩列表
###1.6.1.当保存的对象是小整数值，或者是长度较短的字符串，那么redis就会使用压缩列表来作为哈希键的实现。
###1.6.2.特点（1.压缩列表ziplist结构本身就是一个连续的内存块，由表头、若干个entry节点和压缩列表尾部标识符zlend组成，通过一系列编码规则，提高内存的利用率，使用于存储整数和短字符串；2.压缩列表ziplist结构的缺点是：每次插入或删除一个元素时，都需要进行频繁的调用realloc()函数进行内存的扩展或减小，然后进行数据”搬移”，甚至可能引发连锁更新，造成严重效率的损失）
##1.7.快速列表
##1.8.对象系统：对数据结构的封装，构建统一的对象结构




##1.9.stream
- XADD:插入消息

		XADD key [MAXLEN maxlen] ID field string [field string ...]
	- 消息ID(时间戳-序号) ：保证消息ID是递增的（redis内部会为每个stream维护一个latest_generated_id变量，该变量标记最新的消息ID,如果发现时间戳倒退，redis会使用latest_generated_id中的时间戳，然后只递增seq部分）    	
- 消费分组











#二.发布订阅模型
##2.1.订阅与发布功能（pub/sub）功能，来接收那些以某种方式改动了Redis数据集的事件。
##2.2.发送即忘（fire and forget）的策略，当订阅事件的客户端断线时，它会丢失所有在断线期间分发给它的事件。
#三.持久化（RDB/AOF）
##3.1.RDB持久化：RDB持久化是把当前进程数据生成时间点快照（point-in-time snapshot）保存到硬盘的过程，避免数据意外丢失。
###3.1.1.优点（1.RDB是一个紧凑压缩的二进制文件，代表Redis在某个时间点上的数据快照。非常适用于备份，全景复制等场景；2.Redis 加载RDB恢复数据远远快于AOF的方式）；缺点（1.RDB没有办法做到实时持久化或秒级持久化，成本较高；2.RDB文件使用特定的二进制格式保存，Redis版本演进的过程中，有多个RDB版本，这导致版本兼容的问题）
##3.2.AOF持久化：以独立日志的方式记录每次写命令，重启时在重新执行AOF文件中的命令达到恢复数据的目的
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/AOF%E5%90%8E%E5%8F%B0%E9%87%8D%E5%86%99%E5%A4%84%E7%90%86.png)
###3.2.1.由于Redis是单线程响应命令，所以每次写AOF文件都直接追加到硬盘中，那么写入的性能完全取决于硬盘的负载，所以Redis会将命令写入到缓冲区中，然后执行文件同步操作，再将缓冲区内容同步到磁盘中，这样就很好的保持了高性能。
###3.2.2.缓冲区同步到文件
###3.2.3.重写机制（fork机制以及缓冲区）





#四.事件处理（事件驱动（I/O多路复用））
##4.1.文件事件:将所有产生事件的Socket放到一个队列中
![](https://github.com/HelloWucq/working-knowledge-point/blob/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Redis%E6%96%87%E4%BB%B6%E4%BA%8B%E4%BB%B6.png)
##4.2.时间事件：放在一个链表中（周期性事件；定时事件）
#五.redis的通信协议
#六.单机服务器
##6.1.内存释放策略：1）LRU：优先删除最近最少使用的键；2）TTL：优先删除生存时间最短的键；3）ANDOM：随机删除




#七.redis集群

##7.1.拓扑结构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/redis%20cluster%E6%8B%93%E6%89%91%E7%BB%93%E6%9E%84.png)

##7.2.配置一致性
###7.2.1.配置信息数据结构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/redis%20cluster%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.png)
- clusterState:记录了从集群中某个节点视角，来看集群配置状态
- currentEpoch:表示整个集群中最大的版本号，集群信息每变更一次，该版本号都会自增
- nodes是一个列表，包含本节点所感知的，集群所有节点的信息，也包含自身的信息
- clusterNode记录了每个节点的信息，其中包含了节点本身的版本Epoch;自身的信息描述：节点对应的数据分片范围、为master时的slave列表，为slave时的master等
- 每个节点包含一个全局唯一的NodeId

###7.2.2.信息交互

##7.3.一致性的达成：当Cluster结构不发生变化时，每个节点通过gossip协议在几轮交互之后，便可以得知cluster的结构信息，达到一致性的状态，但集群结构发生变化时，优先得知变更的节点通过Epoch变量，将自己的最新信息扩展到cluster,并最终达到一致
- 当某个节点率先知道了变更时，将自身的currentEpoch 自增，并使之成为集群中的最大值。再用自增后的currentEpoch 作为新的Epoch 版本；
- 当某个节点收到了比自己大的currentEpoch时，更新自己的currentEpoch；
- 当收到的Redis Cluster Bus 消息中的某个节点的Epoch > 自身的时，将更新自身的内容；
- 当Redis Cluster Bus 消息中，包含了自己没有的节点时，将其加入到自身的配置中。

##7.4.sharding
###7.4.1.数据分片
###7.4.2.客户端的路由
###7.4.3.分片的迁移



##7.5.failover
###7.5.1.failover的状态变迁
- 故障发现：当某个master宕机时，宕机时间如何被集群其他节点感知
- 故障确认：多个节点就某个master是否宕机如何达成一致
- slave选举：集群确认了某个master宕机后，如何将它的slave升级成新的master；如果有多个slave，如何选择升级
- 集群结构变更：成功选举成为master后，如何让整个集群知道，以更新Cluster结构信息

##7.6.可用性和性能
###7.6.1.读写分离
###7.6.2.master单点保护









#七.分布式与高可用
##7.1.复制：冗余备份
###7.1.1.复制的实现：1）主从关系的建立；2）主从网络连接建立；3）发送PING命令；4）认证权限；5）发送端口号；6）发送 IP 地址；7）发送能力；8）发送PSYNC命令（同步：全量同步；部分同步）；9）发送输出缓冲区数据；10）命令传播
###7.1.2.心跳机制
###7.1.3.复制积压缓冲区
##7.2.Sentinel
###7.2.1.执行和初始化：1）检查是否开启哨兵模式；2）初始化哨兵的配置；3）载入配置文件；4）开启哨兵模式
###7.2.2.sentinel互相通信，通过gossip协议
###7.2.3.Sentinel的“仲裁会”
###7.2.4.TITL模式：保护模式

##7.3.cluster
###7.3.1.设计原则：性能；水平扩展；可用性
###7.3.2.执行过程12/28/2018 4:12:09 PM ：1）准备节点；2）节点握手；3）分配槽位
###7.3.3.集群的伸缩
##7.4.事务机制：

#注意：
##1.缓存雪崩：1）事前：redis高可用，主从+哨兵，redis cluster，避免全盘崩溃；2）事中：本地ehcache缓存 + hystrix限流&降级，避免MySQL被打死；3）事后：redis持久化，快速恢复缓存数据
##2.缓存穿透：查询不到的数据也放到缓存
##3.缓存雪崩就是缓存失效，请求全部全部打到数据库，数据库瞬间被打死。缓存穿透就是查询了一个一定不存在的数据，并且从存储层查不到的数据没有写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义

#八.Redis常见问题
##8.1.Redis为什么快：纯内存操作；单线程操作，避免了频繁的上下文切换；采用了非阻塞I/O多路复用
![](https://i.imgur.com/nNvarrS.png)
##8.2.Redis的过期以及内存淘汰机制：Redis 采用的是定期删除+惰性删除策略。
##8.3.Redis和数据库双写一致性问题：
##8.4.Redis的并发竞争Key问题：若对Key操作，不要求顺序，准备一个分布式锁，大家抢锁；若对Key操作要求顺序，数据库写入数据库是保存一个时间戳或利用队列

#九.Redis架构图
![](https://i.imgur.com/iFR8vu8.png)
##9.1.架构细节
###9.1.1.所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协议优化传输速度和带宽.
###9.1.2.节点的fail是通过集群中超过半数的节点检测失效时才生效.
###9.1.3.客户端与redis节点直连,不需要中间proxy层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可
###9.1.4.redis-cluster把所有的物理节点映射到[0-16383]slot上,cluster 负责维护node<->slot<->value
###9.1.5.Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。


#十.算法
##10.1.Gossip算法

#十一.Memcached
##11.1.特点：协议简单（基本文本协议或二进制协议）、基于libevent的事件处理、内置内存存储方式、客户端分布式
##11.2.问题：无法备份，重启无法恢复；无法查询；没有提供内置的安全机制；单点故障failover
##11.3.内存存储
###11.3.1.Slab Allocation机制(解决了内存碎片问题，带来了内存浪费问题)
![](https://i.imgur.com/zDLuKDJ.png)
###11.3.2.Growth Factor进行调优
##11.4.典型问题
###11.4.1.容量问题
###11.4.2.服务高可用
###11.4.3.扩展问题

##11.6.一些拓展


#十一.客户端/服务器协议





#注意点
##1.Redis进程模型
![](https://i.imgur.com/b3yIzup.png)
##2.缓存和数据库一致性解决方案
###2.1.采用延时双删策略（结合双删策略+缓存超时设置，这样最差的情况就是在超时时间内数据存在不一致，而且又增加了写请求的耗时）
- 先删除缓存
- 再写数据库
- 休眠500毫秒
- 再次删除缓存
###2.2.异步更新缓存(基于订阅binlog的同步机制)
- 读Redis：热数据基本都在Redis
- 写MySQL:增删改都是操作MySQL
- 更新Redis数据：MySQ的数据操作binlog，来更新到Redis