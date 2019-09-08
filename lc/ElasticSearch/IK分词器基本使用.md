## Elasticsearch 安装ik分词器
[分词器下载地址,选择对应的版本下载](https://github.com/medcl/elasticsearch-analysis-ik/releases "分词器下载地址")
### 解压分词器到plugins下面，启动就es就可以了

###### 未使用ik分词器时, request
```
POST /_analyze
{ 
  "text": "mynote 真的是一个记笔记的好地方"
}
```

###### 未使用ik分词器时, response
```
{
  "tokens": [
    {
      "token": "mynote",
      "start_offset": 0,
      "end_offset": 6,
      "type": "<ALPHANUM>",
      "position": 0
    },
    {
      "token": "真",
      "start_offset": 7,
      "end_offset": 8,
      "type": "<IDEOGRAPHIC>",
      "position": 1
    },
    {
      "token": "的",
      "start_offset": 8,
      "end_offset": 9,
      "type": "<IDEOGRAPHIC>",
      "position": 2
    },
    {
      "token": "是",
      "start_offset": 9,
      "end_offset": 10,
      "type": "<IDEOGRAPHIC>",
      "position": 3
    },
    {
      "token": "一",
      "start_offset": 10,
      "end_offset": 11,
      "type": "<IDEOGRAPHIC>",
      "position": 4
    },
    {
      "token": "个",
      "start_offset": 11,
      "end_offset": 12,
      "type": "<IDEOGRAPHIC>",
      "position": 5
    },
    {
      "token": "记",
      "start_offset": 12,
      "end_offset": 13,
      "type": "<IDEOGRAPHIC>",
      "position": 6
    },
    {
      "token": "笔",
      "start_offset": 13,
      "end_offset": 14,
      "type": "<IDEOGRAPHIC>",
      "position": 7
    },
    {
      "token": "记",
      "start_offset": 14,
      "end_offset": 15,
      "type": "<IDEOGRAPHIC>",
      "position": 8
    },
    {
      "token": "的",
      "start_offset": 15,
      "end_offset": 16,
      "type": "<IDEOGRAPHIC>",
      "position": 9
    },
    {
      "token": "好",
      "start_offset": 16,
      "end_offset": 17,
      "type": "<IDEOGRAPHIC>",
      "position": 10
    },
    {
      "token": "地",
      "start_offset": 17,
      "end_offset": 18,
      "type": "<IDEOGRAPHIC>",
      "position": 11
    },
    {
      "token": "方",
      "start_offset": 18,
      "end_offset": 19,
      "type": "<IDEOGRAPHIC>",
      "position": 12
    }
  ]
}
```

###### 使用ik分词器时，并且使用的是粗粒度(ik_smart)的分词 request
```
POST /_analyze
{ 
  "analyzer": "ik_smart", 
  "text": "mynote 真的是一个记笔记的好地方"
}
```

###### 使用ik分词器时, 并且使用的是粗粒度(ik_smart)的分词 response
```
{
  "tokens": [
    {
      "token": "mynote",
      "start_offset": 0,
      "end_offset": 6,
      "type": "ENGLISH",
      "position": 0
    },
    {
      "token": "真的",
      "start_offset": 7,
      "end_offset": 9,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "是",
      "start_offset": 9,
      "end_offset": 10,
      "type": "CN_CHAR",
      "position": 2
    },
    {
      "token": "一个",
      "start_offset": 10,
      "end_offset": 12,
      "type": "CN_WORD",
      "position": 3
    },
    {
      "token": "记笔记",
      "start_offset": 12,
      "end_offset": 15,
      "type": "CN_WORD",
      "position": 4
    },
    {
      "token": "的",
      "start_offset": 15,
      "end_offset": 16,
      "type": "CN_CHAR",
      "position": 5
    },
    {
      "token": "好地方",
      "start_offset": 16,
      "end_offset": 19,
      "type": "CN_WORD",
      "position": 6
    }
  ]
}
```

###### 使用ik分词器时，并且使用的是细粒度(ik_max_word )的分词 request
```
POST /_analyze
{ 
  "analyzer": "ik_max_word", 
  "text": "mynote 真的是一个记笔记的好地方"
}
```

###### 使用ik分词器时，并且使用的是细粒度(ik_max_word )的分词 response
```
{
  "tokens": [
    {
      "token": "mynote",
      "start_offset": 0,
      "end_offset": 6,
      "type": "ENGLISH",
      "position": 0
    },
    {
      "token": "真的",
      "start_offset": 7,
      "end_offset": 9,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "是",
      "start_offset": 9,
      "end_offset": 10,
      "type": "CN_CHAR",
      "position": 2
    },
    {
      "token": "一个",
      "start_offset": 10,
      "end_offset": 12,
      "type": "CN_WORD",
      "position": 3
    },
    {
      "token": "一",
      "start_offset": 10,
      "end_offset": 11,
      "type": "TYPE_CNUM",
      "position": 4
    },
    {
      "token": "个",
      "start_offset": 11,
      "end_offset": 12,
      "type": "COUNT",
      "position": 5
    },
    {
      "token": "记笔记",
      "start_offset": 12,
      "end_offset": 15,
      "type": "CN_WORD",
      "position": 6
    },
    {
      "token": "笔记",
      "start_offset": 13,
      "end_offset": 15,
      "type": "CN_WORD",
      "position": 7
    },
    {
      "token": "的",
      "start_offset": 15,
      "end_offset": 16,
      "type": "CN_CHAR",
      "position": 8
    },
    {
      "token": "好地方",
      "start_offset": 16,
      "end_offset": 19,
      "type": "CN_WORD",
      "position": 9
    },
    {
      "token": "地方",
      "start_offset": 17,
      "end_offset": 19,
      "type": "CN_WORD",
      "position": 10
    }
  ]
}
```

