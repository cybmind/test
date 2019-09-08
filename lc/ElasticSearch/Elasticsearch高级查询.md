##### 使用`dis_max`最佳匹配字段的分值作为整个查询的整体分值
```
GET /mynote/article/_search
{
  "query": {
    "dis_max": {
      "tie_breaker": 0.7,
      "boost": 1.2,
      "queries": [{
        "multi_match": {
          "query": "基础呀3",
          "fields": ["title", "abstract", "content", "tag"]
        }
      }]
    }
  }
}
  tie_breaker参数会让dis_max查询的行为更像是dis_max和bool的一种折中。它会通过下面的方式改变分值计算过程：
- 取得最佳匹配查询子句的_score。
- 将其它每个匹配的子句的分值乘以tie_breaker。
- 将以上得到的分值进行累加并规范化。
```

##### 不指定字段查询`query_string`
```
GET /mynote/article/_search
{
  "query": {
    "query_string": {
      "query":""
    }
  }
}
```
##### 索引数据移动
```
POST /_reindex
{
  "source": {
    "index": "mynote"
  },
  "dest": {
    "index": "mynote3"
  }
}
```

##### 脚本
```
GET /myapp/_search
{
  "script_fields": {
    "phone": {
      "script": {
        "lang": "expression",
        "source": "doc['id'] * multiplier",
        "params": {
          "multiplier" : 3
        }
      }
    }
  }
}



"script": {
    "lang":   "...",  
    "source" | "id": "...", 
    "params": { ... } 
  }
```

路由routing https://blog.csdn.net/cnweike/article/details/38531997
关联查询 https://blog.csdn.net/laoyang360/article/details/79774481
https://blog.csdn.net/u013613428/article/details/78134170#%E4%BD%95%E4%B8%BApainless