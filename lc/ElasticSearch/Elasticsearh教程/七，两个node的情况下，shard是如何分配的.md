## 背景
在中小型企业开发中，一般的采用的是两个节点来作为集群（也有可能是单机集群），我们现在一个indexß，需要存储在2个节点中。
## 介绍
本文将介绍在两个node情况下，shard是如果分配的，这里只说两个node是因为两个node已经可以组成一个完整的集群了，所以只要是两个或者两个以上的node都是像本文一样进行分配shard的。

###所有主分片在一台机器上
先来介绍一下所有paimary shard都在一个节点上面的情况，replica shard在另一台节点上面，如图有两个节点，一个索引，索引被分成5个primary shard和一个replica shard
![](http://uninote.com.cn/docs/1079089832/__pic/AVJQeE5I.png)
下图将展示所有分片分在同一个node上面的情况, 相当于所有的主分片存储在一台机器上面，副分片也存在一台机器上面，这个并没有违背primary shard 和 replica shard 不能存在同一台机器上面的原则。
![](http://uninote.com.cn/docs/1079089832/__pic/ySwcI2Dm.png)

###主分片不在一台机器上
这里就介绍一下所有的主分片不在同一台机器上面的情况，所有的主分片不在同一台机器上面的时候，那么也就意味着这里是可能主副分片存放在一起，还是先看下没有进行分片的情况下的样子。
![](http://uninote.com.cn/docs/1079089832/__pic/AVJQeE5I.png)
下图就是primary shard 和 replica shard 不在同一台节点上面分片过后可能产生的状态。
![](http://uninote.com.cn/docs/1079089832/__pic/LbvK17bD.png)
