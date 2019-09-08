### document文档的数据格式和面向对象
ElasticSearch 是一种面向文档的搜索引擎，既然是面向文档的，那么如果面向对象是什么样的呢？这里举一个基于关系型数据库的面向对象的例子。

- 首先这里有一个员工类，员工类里面有另一个对象，员工的详细信息
```
public class Employee {
			// 邮箱
			private String email;
			// 姓
			private String firstName;
			// 名
			private String lastName;
			// 员工详细信息
			private EmployeeInfo employeeInfo;
}
```
- 创建员工详细信息类
```
public class EmployeeInfo {
			// 性别
			private String gener;
			// 年龄
			private Integer age;
			// 兴趣爱好
			private String[] interests;
}
```
- 这里如果如果需要将员工信息存储在数据库的话就要创建两张表，**Employee** 和 **EmployeeInfo** 表, 来分别存储员工和员工的详细信息。那么面向文档呢？其实面向文档和面向对象的对象层级关系是一样的，也可以嵌套存储，不管多么复杂的对象关系，都可以通过一篇文档来存储，完全不用像关系型数据库一样将数据给拆开来，分成多张表，这里展示一下如果是面向文档的话，数据内容将会是如何显示的。
```
{
    "email":"lc",
    "firstName":"l",
    "lastName":"c",
    "employeeInfo":{
        "gener":"man",
        "age":"23",
        "interests":""
    }
}
```

### 简单的集群管理
- 1，快速检查集群建健康状况，ElasticSearch 提供了一套`cat`api，可以用来查看es中各种各样的数据，比如查看集群的健康状态。
	
	- 1.1，可以使用以下语句来查看集群健康状态：
		- 请求
		```
		GET /_cat/health
		```
		- 响应：
		```
		1551767501 22:31:41 elasticsearch yellow 1 1 15 15 0 0 15 0 - 50.0%
		```

	- 1.2，那么如何快速了解集群的健康状态呢？**green，yellow，red**
		- **green**：表示集群的每个索引**primary shard**（主分片） 和 **replica shard**（副分片） 都是**active**的，如果不知道什么是主分片和副分片可以看这片文档 http://uninote.com.cn/book/1079089832#962
		
		- **yellow**：表示集群每个索引的**primary shard** 都是active的，但是有部分 **replica shard**不可用，或者所有的**replica shard**都不可用
		
		- **red**：表示集群里面不是所有索引的**primary shard**都是active的，可能会有部分数据丢失了。

	- 1.3，为什么我们现在会处于**yellow**状态呢？
		- 因为我们当前是一台服务器，由于ElasticSearch的副本机制是不能让索引的primary shard 和 replica shard 分片在同一个节点上面，所有我们的机器上面只有primary shard 的数据，没有replica shard的数据。

- 2，快速查看集群中有那些索引。
	- 2.1 可以使用以下语句来查看集群中有那些索引。
		
		- 请求
		```
		GET /_cat/indices
		```
		- 响应
		```
		health status index      uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   mynote2 g853jqXYTiiDV7L   5   1          1            0     10.8kb         10.8kb
yellow open   myapp    Myw-uNr5QEmbPoO4ao_iFA   5   1          3            0     23.5kb         23.5kb
yellow open   mynote   iygNJirZSIazKuQzEPDDsw   5   1          5            0     32.4kb         32.4kb
		```