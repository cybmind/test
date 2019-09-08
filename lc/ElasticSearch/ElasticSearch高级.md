### 1，ElasticSearch核心概念
###### 1.1,Near RealTime (NRT),近实时，两个意思，从写入数据到读取数据可以被搜索到有一个小延迟（大概一秒）；基于es执行搜索和分析可以达到秒级
###### 1.2,Cluster：集群，包含多个节点，每个节点属于哪个集群是通过一个配置（集群名称，默认是elasticserach）来决定的
###### 1.3，Node：节点，集群集群中的一个节点，节点也有一个名称（默认是随机分配的），节点名称很重要（在执行运维管理操作的时候），默认节点回去加入一个名称为elasticserach的集群，如果直接启动一堆节点，那么他们会自动组成一个elasticserach集群，一个节点也可以组成一个elasticserach集群
###### 1.4，Document：文档，es中最小的数据单元，一个document可以使一条客户数据，一条商品分类数据，一条订单数据，同城用JSON数据结构表示，每个index下的type中，都可以存储多个document，一个document里面可以有多个filed，每个filed就是一个字段
```
例如：一个user的document
{
    "userName":"张三",
    "userPass":"123456",
    "userPhone":"18111215205"
}
这就是一个document
```
###### 1.5，Index，索引，其实就相当于一个数据库，里面包含了一堆有相似结构的文档数据，比如可以有一个客户索引，商品分类索引，订单索引，索引有一个名称，一个index包含很多document。
###### 1.6，Type，类型，其实就相当于数据库中的表，一个type属于一个索引下面，一个索引下面可以有多个type，可以用type来管理索引下的document，其实就是给document分类了，比如有一个用户索引，下面有一个用户分类，也可以是一个用户账号分类
```
/user/userinfo
{
    "userId":"12345",
    "userName":"张三",
    "userPass":"123456",
    "userPhone":"18111215205"
}
第一个/后面的是索引，第二个斜杠后面的是分类
```
###### 1.6，primary Shard，分片，单台服务器无法存储大量数据，es可以将一个索引中的数据且分为多个shard，分布在多台服务器上存储，有了shard就可以横向扩展，存储更多数据，让收缩和分析操作分布到多台服务器上去执行，提升吞吐量和性能（比附1t数据分布到3台服务器，相当于就是每个服务器上面只有1t数据，如果单台服务器每秒支持的QPS是2000，那么3台服务器就变成了每秒6000）
###### 1.7，replica shard，任何一个服务器随时都有可能故障和宕机，此时shard就可能会丢失，因此可以为每个shard创建多个replica副本，replica可以在shard故障时提供备用服务，保证数据不丢失，多个replica还可以提升搜索操作的吞吐量和性能（只能是搜索操作才能使用replica副本）primary shard（建立索引时一次设置，不能修改，默认为5个），replica shard（也就是每个primary shard的默认数量，默认1个），默认每个索引10个shard，5个primary shard，5个replica shard，最小的高可用配置，是两台服务器
```
注：es规定primary shard和replica shard不能在一个节点上，
所以单台服务器上面新建的索引只有5个primary shard
```

### 2.ES最基础的分布式架构
###### 2.1 ElasticSearch对复杂分布式机制的透明隐藏特性
```
es对所有分布式里面的复杂操作都做了隐藏,客户端调用就可以了，不用去管这些复杂的操作
```
###### 2.2 ElasticSerach的垂直扩容和水平扩容
```
扩容方式一般是采用水平扩容（加机器）
```
###### 2.3 增加或减少节点时的数据rebalance
```
节点扩容时会自动将节点的数据进行负载均衡（平均分配）
```
###### 2.4 master节点
```
es里面节点里面都有一个master节点，主要负责全局配置的操作，比如增加节点时对数据的分配，索引的元数据管理
```
###### 2.5 节点对等的分布式架构
```
es里面的所有节点都是对等的（功能一样），比如用户端获取一个数据访问一个节点，这个节点上没有数据，那么这个节点就会自动路由到有这个数据的节点上面去，然后有数据的节点就会将数据响应给用户访问的初始节点，初始节点在将数据响应给用户，这个过程是里面每个节点都有的功能
```

#### 3.elasticsearch数据路由算法
```
shards = hash(rothing) % primary_shards_number
最后存储的位置就在shards这个分片上面
默认情况下 rothing值 = _id

手动指定rothing值
PUT /index/type/id?rothing=userId
手动指定rothing值是很有用的，可以将指定类型的值存储到指定的primarky shards分片上面
```

#### 4.primary shard不可以变的原因
```
因为路由公式的原因，如果数量变了那么就找不到原来的文档位置了。
比如原来公式结果值为 21 ， primary shard数量为3,那么这个文档的数据就存在 21 % 3 =0 p0上面，
如果修改了paimary shard数量为4 那么就是21 % 4 = 1 那么es就会到p1上面去找了
```

#### 5.document增删改实现原理
```
客户端访问es节点的时候会随机访问一个节点，
这个被访问的节点就作为协调节点，然后使用
hash函数的路由算法去计算应该讲数据交给那
个节点分片去处理，处理的完成又会将新的数
据复制给replice shard，最后在通知协调节
点，协调节点在通知客户端
```
#### 6.Es的写一致性和quorum机制
```
(1) consistency, one(primary shard), all(all shard), quorum(default)
我们在发送任何一个增删改查操作的时候，比如说
put /index/type/id 
都可以带上一个consistency参数，指明我们想要的一致性是什么
put /index/type/id?consistency=quorum
one: 要求我们这个写操作，只要有一个primary shard是acvice活跃可用的，就可以执行
all: 要求我们这个鞋操作，必须所有的primary shard是active活跃可用的，就可以执行
quorum: 默认的值， 要求所有的shard中，必须是大部分的shard都是活跃的，可用的，才可以执行和这个写操作
(2) quorum机制，写之前必须确保大多数shard都可用,int ( (primary + number_of_replicas) / 2 ) + 1 ,大于1时菜生效
(3) quorum不齐全时，wait，默认为1分钟，timeout，100，30s
(4) 写的时候可以指定超时时间
put /index/type/id?timeout=30 
```

#### 7.deep paging深度分页
```
用户请求数据的时候会通过路由节点，然后路由节点去其他的节点上面去获取数据回来，比如1000 - 1010条数据，一个3个节点，那么路由节点就会存3000条数据，在对这3000条数据进行排序，然后在取出对应的位置的10条数据
```