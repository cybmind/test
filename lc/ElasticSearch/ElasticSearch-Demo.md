参考资料：https://blog.csdn.net/u013613428/article/details/78134170#%E4%BD%95%E4%B8%BApainless

## 查询数据脚本demo
- 删除数据
```
DELETE hockey
```
- 创建数据
```
PUT hockey/player/_bulk?refresh
{"index":{"_id":1}}
{"first":"johnny","last":"gaudreau","goals":[9,27,1],"assists":[17,46,0],"gp":[26,82,1]}
{"index":{"_id":2}}
{"first":"sean","last":"monohan","goals":[7,54,26],"assists":[11,26,13],"gp":[26,82,82]}
{"index":{"_id":3}}
{"first":"jiri","last":"hudler","goals":[5,34,36],"assists":[11,62,42],"gp":[24,80,79]}
{"index":{"_id":4}}
{"first":"micheal","last":"frolik","goals":[4,6,15],"assists":[8,23,15],"gp":[26,82,82]}
{"index":{"_id":5}}
{"first":"sam","last":"bennett","goals":[5,0,0],"assists":[8,1,0],"gp":[26,1,0]}
{"index":{"_id":6}}
{"first":"dennis","last":"wideman","goals":[0,26,15],"assists":[11,30,24],"gp":[26,81,82]}
{"index":{"_id":7}}
{"first":"david","last":"jones","goals":[7,19,5],"assists":[3,17,4],"gp":[26,45,34]}
{"index":{"_id":8}}
{"first":"tj","last":"brodie","goals":[2,14,7],"assists":[8,42,30],"gp":[26,82,82]}
{"index":{"_id":39}}
{"first":"mark","last":"giordano","goals":[6,30,15],"assists":[3,30,24],"gp":[26,60,63]}
{"index":{"_id":10}}
{"first":"mikael","last":"backlund","goals":[3,15,13],"assists":[6,24,18],"gp":[26,82,82]}
{"index":{"_id":11}}
{"first":"joe","last":"colborne","goals":[3,18,13],"assists":[6,20,24],"gp":[26,67,82]}
```
- 使用脚本查询
将查询结果按照first.keyword 和 last.keyword的结果拼接进行排序
```
GET hockey/_search
{
  "size": 20,
  "query": {
    "match_all": {}
  },
  "sort": {
    "_script": {
      "type": "string",
      "order": "asc",
      "script": {
        "lang": "painless",
        "inline": "doc['first.keyword'].value + ' ' + doc['last.keyword'].value"
      }
    }
  }
}
```
查询出匹配用脚本传入的参数的数据 inline 为脚本内容 lang为脚本语言， params为要传入的参数， 脚本进行循环的时候会自动排序
```
GET /hockey/_search
{
  "query": {
    "match_all": {}
  },
  "script_fields": {
    "namecount": {
      "script": {
        "lang":"painless",
        "inline":"String result = ''; for (int i =0; i< doc['goals'].length; i++){result=result + doc['goals'][i].toString();} if (result.equals(params.key)) {return result;}",
        "params": {
          "key":"2714"
        }
      }
    }
  }
}
```
结果
```
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 11,
    "max_score": 1,
    "hits": [
      {
        "_index": "hockey",
        "_type": "player",
        "_id": "8",
        "_score": 1,
        "fields": {
          "namecount": [
            "2714"
          ]
        }
      },
      {
        "_index": "hockey",
        "_type": "player",
        "_id": "10",
        "_score": 1,
        "fields": {
          "namecount": [
            null
          ]
        }
      },
      {
        "_index": "hockey",
        "_type": "player",
        "_id": "5",
        "_score": 1,
        "fields": {
          "namecount": [
            null
          ]
        }
      }
}
```
- 这里都是_search操作，多个操作之间会形成管道，既query::match_all的输出会作为script_fields或者sort的输入。
- _search操作中所有的返回值，都可以通过一个map类型变量doc获取。和所有其他脚本语言一样，用[]获取map中的值。-这里要强调的是，doc只可以在_search中访问到。在下一节的例子中，你将看到，使用的是ctx。
- _search操作是不会改变document的值的，即便是script_fields，你只能在当次查询是能看到script输出的值。
doc['first.keyword']这样的写法是因为doc[]返回有可能是分词之后的value，所以你想要某个field的完整值时，请使用keyword

## 更新数据脚本
- 删除数据
```
DELETE hockey
```
- 创建数据
```
PUT hockey/player/_bulk?refresh
{"index":{"_id":1}}
{"first":"johnny","last":"gaudreau","goals":[9,27,1],"assists":[17,46,0],"gp":[26,82,1]}
{"index":{"_id":2}}
{"first":"sean","last":"monohan","goals":[7,54,26],"assists":[11,26,13],"gp":[26,82,82]}
{"index":{"_id":3}}
{"first":"jiri","last":"hudler","goals":[5,34,36],"assists":[11,62,42],"gp":[24,80,79]}
{"index":{"_id":4}}
{"first":"micheal","last":"frolik","goals":[4,6,15],"assists":[8,23,15],"gp":[26,82,82]}
{"index":{"_id":5}}
{"first":"sam","last":"bennett","goals":[5,0,0],"assists":[8,1,0],"gp":[26,1,0]}
{"index":{"_id":6}}
{"first":"dennis","last":"wideman","goals":[0,26,15],"assists":[11,30,24],"gp":[26,81,82]}
{"index":{"_id":7}}
{"first":"david","last":"jones","goals":[7,19,5],"assists":[3,17,4],"gp":[26,45,34]}
{"index":{"_id":8}}
{"first":"tj","last":"brodie","goals":[2,14,7],"assists":[8,42,30],"gp":[26,82,82]}
{"index":{"_id":39}}
{"first":"mark","last":"giordano","goals":[6,30,15],"assists":[3,30,24],"gp":[26,60,63]}
{"index":{"_id":10}}
{"first":"mikael","last":"backlund","goals":[3,15,13],"assists":[6,24,18],"gp":[26,82,82]}
{"index":{"_id":11}}
{"first":"joe","last":"colborne","goals":[3,18,13],"assists":[6,20,24],"gp":[26,67,82]}
```
- 使用脚本更新数据
```
POST /hockey/player/8/_update
{
  "script": {
    "lang": "painless",
    "inline": """
    ctx._source['first'] = params.first;
    ctx._source['last']  = params.last
    """,
    "params":{
      "first":"firstValue",
      "last":"lastValue"
    }
  }
}
```
- 查询数据结果
![](http://uninote.com.cn/docs/1079089832/__pic/it9Ggkhu.png)

## 批量数据脚本
- 删除数据
```
DELETE hockey
```
- 创建数据
```
PUT hockey/player/_bulk?refresh
{"index":{"_id":1}}
{"first":"johnny","last":"gaudreau","goals":[9,27,1],"assists":[17,46,0],"gp":[26,82,1]}
{"index":{"_id":2}}
{"first":"sean","last":"monohan","goals":[7,54,26],"assists":[11,26,13],"gp":[26,82,82]}
{"index":{"_id":3}}
{"first":"jiri","last":"hudler","goals":[5,34,36],"assists":[11,62,42],"gp":[24,80,79]}
{"index":{"_id":4}}
{"first":"micheal","last":"frolik","goals":[4,6,15],"assists":[8,23,15],"gp":[26,82,82]}
{"index":{"_id":5}}
{"first":"sam","last":"bennett","goals":[5,0,0],"assists":[8,1,0],"gp":[26,1,0]}
{"index":{"_id":6}}
{"first":"dennis","last":"wideman","goals":[0,26,15],"assists":[11,30,24],"gp":[26,81,82]}
{"index":{"_id":7}}
{"first":"david","last":"jones","goals":[7,19,5],"assists":[3,17,4],"gp":[26,45,34]}
{"index":{"_id":8}}
{"first":"tj","last":"brodie","goals":[2,14,7],"assists":[8,42,30],"gp":[26,82,82]}
{"index":{"_id":39}}
{"first":"mark","last":"giordano","goals":[6,30,15],"assists":[3,30,24],"gp":[26,60,63]}
{"index":{"_id":10}}
{"first":"mikael","last":"backlund","goals":[3,15,13],"assists":[6,24,18],"gp":[26,82,82]}
{"index":{"_id":11}}
{"first":"joe","last":"colborne","goals":[3,18,13],"assists":[6,20,24],"gp":[26,67,82]}
```
- 使用脚本批量更新数据
```
POST /hockey/_update_by_query
{
  "query":{
    "match_all":{
    }
  },
  "script":{
    "lang":"painless",
    "inline":"ctx._source['first']=params.first;ctx._source['last']=params.last",
    "params":{
      "first":"testfirst",
      "last":"testlast"
    }
  }
}
```

- 查询数据,查看结果
```
GET /hockey /_search
{
	"query":{
		"match_all":{}
	}
}
```
![](http://uninote.com.cn/docs/1079089832/__pic/WThtnKYP.png)




###  使用脚本进行自定义排序
- 创建数据
```
PUT /_bulk
{"index":{"_index":"user","_type":"doc","_id":1}}
{"name":"liaocheng","age":1}
{"index":{"_index":"user","_type":"doc","_id":2}}
{"name":"liaocheng","age":2}
{"index":{"_index":"user","_type":"doc","_id":3}}
{"name":"liaocheng","age":3}
{"index":{"_index":"user","_type":"doc","_id":4}}
{"name":"liaocheng","age":4}
```
- 如果按照正常的排序查询age只能从1到4，或者4到1进行排序，我们使用自定义排序，比如将2弄到最前面来,可以使用脚本修改它的相关度评分
```
GET /user/_search
{
  "query": {
    "function_score": {
      "script_score": {
        "script": {
          "lang": "painless",
          "inline": "long age=doc['age'].value;if (age == 2) {return age*params.value;}",
          "params": {
            "value":10
          }
        }
      }
    }
  }
}
```
- 查看结果
![](http://uninote.com.cn/docs/1079089832/__pic/yTi31iZ5.png)