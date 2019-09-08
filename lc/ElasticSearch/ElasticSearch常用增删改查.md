## 查询数据
### DSL 关键字功能
```
query-->all:查询
match-->5:匹配单个
match_all-->4:匹配所有
match_phrase-->9:完全相同
multi_match-->13:将单个值带到多个字段上面去查询
range-->14:对查询出来的结果控制在一个范围里面
term-->15:不会分词，相当于文档必须包含整个值，把query string当成一整个串到倒排索引里面去找
terms-->16:和term一样，只不过同时匹配多个词 
constant_score -->17:可以再不适用bool的时候适用filter
bool-->12:布尔
filter-->17:是对数据的过滤，不会计算相关度分数
must-->12:文档必须满足里面的条件
must_not-->12:必须不包含
should-->12:可以包含也可以不包含
minimum_should_match-->12:至少几条数据
sort-->18:排序
from-->6:从第几条开始
size-->6:查询几条记录
_source-->7:查询结果只显示指定字段
highlight-->10:搜索结果高亮显示
```

#### 1.通过id进行查询
##### 请求
```
GET /index/type/id
```
##### 返回
```
{
  "_index": "ecommerce",
  "_type": "product",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "name": "gaolujie yagao",
    "desc": "gaoxiao meibai",
    "price": 30,
    "producer": "gaolujie producer",
    "tags": [
      "meibai",
      "fangzhu"
    ]
  }
}
```

#### 2.查询全部数据
##### 请求
```
GET /index/type/_search
```
##### 返回
```
took:查询耗时
timed_out：是否超时
_shards：请求的分片
hits.tatal:总共document文档的数量
hits.max_source:最大相关对评分
hits.hits：所有的document文档
{
  "took": 8,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 3,
    "max_score": 1,
    "hits": [
      {
        "_index": "ecommerce",
        "_type": "product",
        "_id": "2",
        "_score": 1,
        "_source": {
          "name": "jiajieshi yagao",
          "desc": "youxiao fangzhu",
          "price": 25,
          "producer": "jiajieshi producer",
          "tags": [
            "fangzhu"
          ]
        }
      },
      {
        "_index": "ecommerce",
        "_type": "product",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "gaolujie yagao",
          "desc": "gaoxiao meibai",
          "price": 30,
          "producer": "gaolujie producer",
          "tags": [
            "meibai",
            "fangzhu"
          ]
        }
      },
      {
        "_index": "ecommerce",
        "_type": "product",
        "_id": "3",
        "_score": 1,
        "_source": {
          "name": "zhonghua yagao",
          "desc": "caoben zhiwu",
          "price": 40,
          "producer": "zhonghua producer",
          "tags": [
            "qingxin"
          ]
        }
      }
    ]
  }
}
```
#### 3.DSL查询
DSL:Domian Specified Language，Elasticserach专用查询语言，可以构建各种复杂的语法
#### 4.查询所有商品
##### 请求
```
GET /ecommerce/product/_search
{
  "query": {
    "match_all": {}
  }
}
```

#### 5.查询商品名字包含牙膏，并且按照价格降序
```
GET /ecommerce/product/_search
{
  "query": {
    "match": {
      "name": "yagao"
    }
  },
  "sort": [
    {
      "price":"desc"
    }
  ]
}
```

#### 6.分页查询
```
GET /ecommerce/product/_search
{
  "query": {
    "match_all": {}
  },
  "from":1,
  "size": 1
}
```
#### 7.查询结果只显示指定字段
```
GET /ecommerce/product/_search
{
  "query": {
    "match_all": {}
  },
  "_source":["name", "price"]
}
```

#### 8.查询名称必须包含牙膏，并且价格大于25
```
GET /ecommerce/product/_search
{
  "query":{
    "bool": {
      "must": [             必须
        {
          "match": {        匹配
            "name": "yagao" 
          }
        }
      ],
      "filter": {           过滤
        "range": {          返回
          "price": {
            "gt": 25        大于
          }
        }
      }
    }
  }
}
```
#### 9.短语搜索,match_phrase,必须完全匹配搜索的值
```
GET /ecommerce/product/_search
{
  "query": {
    "match_phrase": {
      "producer": "zhonghua producer"
    }
  }
}
```

#### 10.将搜索结果高亮显示 highlight
```
GET /ecommerce/product/_search
{
  "query": {
    "match": {
      "producer": "zhonghua producer"
    }
  },
  "highlight": {
    "fields": {
      "producer": {}
    }
  }
}
```

#### 11.批量查询mget
```
方式1：不指定任何索引和type
GET /_mget
{
  "docs":[
      {
        "_index":"test_index",
        "_type":"test_type",
        "_id":1
      },
      {
        "_index":"test_index",
        "_type":"test_type",
        "_id":2
      }
    ]
}

方式2：指定了索引
GET /test_index/_mget
{
  "docs":[
      {
        "_type":"test_type",
        "_id" : 1
      },
      {
        "_type":"test_type",
        "_id" : 2
      }
    ]
}

方式3：指定了索引和类型，可以直接使用ids

GET /test_index/test_type/_mget
{
  "ids":[1, 2]
}
```

#### 12.DSL组合查询,多条件组合查询必须要有bool
```
去匹配值得始终只有那几个关键字，match，match_all 等等

GET /website/article/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "tille": "elasticsearch"
        }}
      ],
      "should": [
        {
          "match": {
            "tille": "elasticsearch"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "author_id": "111"
          }
        }
        ],
        "minimum_should_match": 1  --至少有一条
    }
  }
}
```
#### 13.mutli_match查询使用
###### 功能：查询单个值，然后使用多个字段去匹配
```
GET /website/article/_search
{
  "query": {
    "multi_match": {
      "query": "elasticsearch",
      "fields": ["tille", "content"]
    }
  }
}
```

#### 14.range查询
###### 功能：对查询出来的结果控制在一个范围里面
```
GET /website/article/_search
{
  "query": {
    "range": {
      "author_id": {
        "gt": 110
      }
    }
  }
}
```
#### 15.term查询
###### 功能：相当于把my hadoop看成一个字符串，到倒排索引里面去找,不进行分词
```
GET /website/article/_search
{
  "query": {
    "term": {
      "tille": "my hadoop"
    }
  }
}
```

#### 16.terms查询
###### 功能：和term一样，只不过是同时匹配多个值
```
GET /website/article/_search
{
  "query": {
    "terms": {
      "content": [
        "goods"
      ]
    }
  }
}
```

#### 17.constant_score:可以再不适用bool的时候适用filter
```
GET /website/article/_search
{
  "query": {
    "constant_score": {
      "filter": {
       "range": {
         "author_id": {
           "gte": 111
         }
       } 
      }
    }
  }
}
```
#### 18.sort:对查询结果进行自定义排序
```
GET /website/article/_search
{
  "query": {
    "range": {
      "author_id": {
        "gte": 10
      }
    }
  },
  "sort": [
    {
      "author_id": {
        "order": "desc"
      }
    }
  ]
}
```
## 修改数据
### 1.替换
###### 替换的方式有一个不好，就是他必须把所有的_source内容都替换了
##### 请求
```
PUT /ecommerce/product/1
{
   "name": "jiaqiangban gaolujie yagao",
    "desc": "gaoxiao meibai",
    "price": 30,
    "producer": "gaolujie producer",
    "tags": [
      "meibai",
      "fangzhu"
    ]
}
```
##### 返回
```
{
  "_index": "ecommerce",
  "_type": "product",
  "_id": "1",
  "_version": 2,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 2,
  "_primary_term": 1
}
```
#### 2.更新
##### 请求
```
POST /ecommerce/product/1/_update
{
  "doc": {
    "name":"gaolujie yagao"
  }
}
```
##### 返回
```
{
  "_index": "ecommerce",
  "_type": "product",
  "_id": "1",
  "_version": 4,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 3,
  "_primary_term": 1
}
```

#### 3.使用groovy内置脚本更新数据
```
POST /test_index/test_type/1/_update
{
    "script":"ctx._source.num+=1"
}
```

#### 4.使用groovy外部脚本更新数据
```
在elasticserach的config/scripts目录下面新建文件(文件名要和file取的值一样),里面的内容是
ctx._source.字段名+=值
ctx._source.tags+=newtags
在外面调用的params参数里面就需要写脚本里面+=后面的值为key，value值就是增加的数据
POST /test_index/test_type/1/_update
{
  "script": {
    "lang": "groovy",
    "file": "test-add-tags",
    "params": {
      "newtags":"testTags"
    }
  }
}
```
#### 5.如果使用post请求的时候数据不存在会报错，那么久使用upsert操作，需要结合doc
```
如果存在就修改（执行doc），如果不存在就新增（执行upsert）
POST /test_index/test_type/2/_update
{
  "doc": {
    "num": 1,
    "name" : "lisi"
  }, 
  "upsert": {
    "num" : 0,
    "name" : "zhangsan"
  }
}
```

#### 6.并发控制时修改数据如果失败进行重试的机制retry_on_conflict
```
post 在更新数据的时候，es底层回自动携带一个版本过去，如果更新的时候版本和文档的当前版本对不上，那么久会报错，使用retry_on_conflict可以控制重试次数
POST /test_index/test_type/2/_update?retry_on_conflict=5
{
  "doc": {
    "age":"xxxx"
  }
}
```
## 添加数据
#### 1.新建一个索引
##### 请求
```
使用PUT 协议
PUT /indexName
```
##### 返回
```
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "test"
}
```
#### 2.新增文档
##### 请求
```
使用PUT 协议
PUT /index/type/id
{
    json数据
}
如果不指定id，es会自动生成一个以base64转码过后的24位id
```
##### 返回
```
{
  "_index": "ecommerce",    索引名
  "_type": "product",       类型名
  "_id": "1",               id
  "_version": 1             版本
  "result": "created",      创建结果
  "_shards": {              分片信息
    "total": 2,             请求分片存储数据信息，默认分片是5个，但是只放在了一个分片上面，这里请求了这个分片的primary shard 和 replica shard，所以total是2
    "successful": 1,        存储成功结果，只有primaryshard成功了，因为当前是单节点，replica所在的节点没有启动
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```

#### 3.文档已经存在的时候强制创建文档，不需要更新的方式
##### 请求
```
方式1
PUT /index/type/id?op_type=create
{
    json数据
}
方式2
PUT /index/type/id/_create
{
    json数据
}
```

#### 4.给文档添加单个字段
```
源文档数据
{
  "_index": "test_index",
  "_type": "test_type",
  "_id": "2",
  "_version": 3,
  "found": true,
  "_source": {
    "num": 1,
    "name": "lisi",
    "age": "xxx"
  }
}

执行添加命令
POST /test_index/test_type/2/_update
{
    "doc":{
        "sex":"nan"
    }
}

新文档结果
{
  "_index": "test_index",
  "_type": "test_type",
  "_id": "2",
  "_version": 4,
  "found": true,
  "_source": {
    "num": 1,
    "name": "lisi",
    "age": "xxx",
    "sex": "nan"
  }
}
```
## 删除数据
#### 1.删除一个索引
##### 请求
```
DELETE /indexName
```
##### 返回
```
{
  "acknowledged": true
}
```

#### 2.删除文档
##### 请求
```
DELETE /indexName/type/id
```
##### 返回
```
{
  "_index": "ecommerce",
  "_type": "product",
  "_id": "1",
  "_version": 9,
  "result": "deleted",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 8,
  "_primary_term": 1
}
```

