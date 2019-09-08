## 介绍
本文将介绍index是如何分配shard的，一个index中如何设置shard以及replica数量，增减节点是shard在集群中是如何的进行负载均衡，一篇文档可以存在多个primary shard和replica shard中吗？初始创建primary shard的数量是多少？replica shard的默认数量又是多少？primary shard和replica shard的数量指定了过后还可以更改吗？如果不能更改是为什么？在单个node节点中index的分片是如何分配的。

## 目录
- **index是如何分配shard的**

- **一个index中如何设置primary shard以及replica shard数量**

- **增减节点是shard在集群中是如何的进行负载均衡**

- **一篇文档可以存在多个primary shard或者replica shard中吗？**

- **初始创建primary shard的数量是多少？replica shard的默认数量又是多少？**

- **primary shard和replica shard的数量指定了过后还可以更改吗？如果不能更改是为什么？**

- ** 在单个node节点中index的分片是如何分配的**

###index是如何分配shard的

在ES中每个shard都是一个最小的工作单元，他们底层其实每一个shard都是一个lucene实例，如果说我们现在有一个index，里面的数据是3T，ES集群一共有3个节点，每个节点的数据是1T，这个时候index会将数据拆成3份，每份是1T, 然后放在不同的节点上面(这个就是数据分片)，每台服务器上面又分别存储了不同分片的primary shard 和 replica shard（前面的文章中已经介绍了，parimary shard 和 replica shard他们是不能存在一台服务器上面的）
![](http://uninote.com.cn/docs/1079089832/__pic/mbeIoYQs.png)
### 一个index中如何设置primary shard以及replica shard数量

在ES中我们可以在创建index的时候，指定settings里面的分片数量，这个副本分片数量是指每个primary shard分片的数量是几个副分片，这里的意思是一共有5个primary shard，每个priamry shard的replica shard分片数量是1个。
```
PUT /myindex
{
  "settings": {
    "number_of_shards": 5, // 主分片数量
    "number_of_replicas": 1 // 副本分片数量
  }
}
```

### 增减节点是shard在集群中是如何的进行负载均衡
在ES中我们增加和减少节点的时候ES集群都会将我们的所有shard进行负载均衡，比如现在我们有7个分片，但是只有3个节点，在集群中负载均衡会让其中某一个节点负载会重一些。
![](http://uninote.com.cn/docs/1079089832/__pic/x7PDc9Gz.png)
### 一篇文档可以存在多个primary shard或者replica shard中吗？
在ES一个document只能属于一个primary shard 或者 replica shard，当想要搜索一篇文章时，可以访问任意一个节点，不管该节点上面是否有该数据的分片，或者是该数据的primary shard异或是replica shard，因为在ES中所有节点是对等的，你访问其中任意一个节点，这个节点会自动帮你到存储了该节点数据的节点上面去获取数据。
### 初始创建primary shard的数量是多少？replica shard的默认数量又是多少？
ES默认创建的primary shard数量是5个，replica shard默认数量是1个，也就是不在settings里面指定number_of_shards和number_of_replicas的时候。
### primary shard数量指定了过后还可以更改吗？为什么不能更改？
ES里面当你指定了primary shard过后它的数量是不能进行更改的，因为这里设计到一个路由算法，当ElasticSearch索引一篇文章时，他存储的分片是通过，主分片数量 % hash(文章id，或者其他属性值)来确定文章是存放在那个地方的，比如我们的primary shard 数量是4，通过ElasticSearch的hash算法来计算的文章id的值为3，那么这个数据就应该存放到（4%3 = 1）1这个分片上面，如果我们修改了主分片数量那么就将找不到我们文档存储的位置了，因为在ES中文档是不能存在多个primary shard 或者 replica shard 中的。

### 在单个node节点中index的分片是如何分配的
在单个node节点中，不管有多少个分片，所有分片都只有主分片存储在了节点上面，这个时候如果节点宕机了，那么就获取不到数据了，这也是为什么它的集群状态的yellow的原因。因为有部分的或者所有的replica shard 并没有分配。