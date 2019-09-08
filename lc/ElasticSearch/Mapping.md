### 掘金文章mapping
```
PUT /article
{
  "mappings": {
      "type": {
        "properties": {
          "author": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              },
              "pinyin": {
                "type": "text",
                "analyzer": "pinyin"
              }
            },
            "analyzer": "ik_max_word"
          },
          "commentNum": {
            "type": "integer"
          },
          "content": {
            "type": "text",
            "fields": {
              "pinyin": {
                "type": "text",
                "analyzer": "pinyin"
              }
            },
            "analyzer": "ik_max_word"
          },
          "createTime": {
            "type": "date"
          },
          "link": {
            "type": "keyword"
          },
          "praise": {
            "type": "integer"
          },
          "readNum": {
            "type": "integer"
          },
          "title": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword"
              },
              "pinyin": {
                "type": "text",
                "analyzer": "pinyin"
              }
            },
            "analyzer": "ik_max_word"
          }
        }
      }
    }
}
```

### 掘金用户mapping
```
PUT /user
{ 
  "mappings": {
    "type":{
      "properties":{
        "username":{
          "type":"text",
          "analyzer":"ik_max_word",
          "fields":{
            "keyword":{
              "type":"keyword"
            },
            "pinyin":{
              "type": "text",
              "analyzer": "pinyin"
            }
          }
        },
        "registerTime":{
          "type":"date"
        },
        "tag":{
          "type":"text"
        },
        "brief":{
          "type":"text",
          "analyzer":"ik_max_word",
          "fields":{
            "pinyin":{
              "type": "text",
              "analyzer": "pinyin"
            }
          }
        },
        "publishedArticlesNum":{
          "type":"integer"
        },
        "hotNum":{
          "type":"integer"
        },
        "shareNum":{
          "type":"integer"
        },
        "initiatePraise":{
          "type":"integer"
        },
        "publishedBookNum":{
          "type":"integer"
        },
        "praiseCountNum":{
          "type":"integer"
        },
        "articleReadCountNum":{
          "type":"integer"
        },
        "attentionPeopleNum":{
          "type":"integer"
        },
        "fans":{
          "type":"integer"
        },
        "collectArticleNum":{
          "type":"integer"
        },
        "attentionTagNum":{
          "type":"integer"
        }
      }
    }
  }
}
```

### mynote mapping
```
{
    "settings":{
        "number_of_shards":5,
        "number_of_replicas":1
    },
    "mappings":{
        "article":{
            "properties":{
                "abstract":{
                    "type":"text",
                    "analyzer":"ik_max_word"
                },
                "article_id":{
                    "type":"keyword"
                },
                "content":{
                    "type":"text",
                    "fields":{
                        "pinyin":{
                            "type":"text",
                            "analyzer":"pinyin"
                        }
                    },
                    "analyzer":"ik_max_word"
                },
                "id":{
                    "type":"long"
                },
                "recommend":{
                    "type":"byte"
                },
                "sort":{
                    "type":"long"
                },
                "status":{
                    "type":"byte"
                },
                "tag":{
                    "type":"keyword"
                },
                "thum":{
                    "type":"text"
                },
                "time":{
                    "type":"date"
                },
                "title":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "type":"keyword"
                        },
                        "pinyin":{
                            "type":"text",
                            "analyzer":"pinyin"
                        }
                    },
                    "analyzer":"ik_max_word"
                },
                "updata_time":{
                    "type":"date"
                },
                "username":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "type":"keyword"
                        },
                        "pinyin":{
                            "type":"text",
                            "analyzer":"pinyin"
                        }
                    },
                    "analyzer":"ik_max_word"
                }
            }
        }
    }
}
```

mynote拼音高亮和多音字搜索
```
{
    "settings": {
        "analysis": {
            "analyzer": {
                "ik_pinyin_analyzer": {
                    "type":"custom",
                    "tokenizer": "ik_smart",
                    "filter": ["my_pinyin","word_delimiter"]
                }
            },
            "filter": {
                "my_pinyin": {
                  "keep_joined_full_pinyin": "true",
			      "keep_none_chinese_in_first_letter": "false",
			      "lowercase": "true",
			      "keep_original": "false",
			      "keep_first_letter": "false",
			      "trim_whitespace": "true",
			      "type": "pinyin",
			      "keep_none_chinese": "false",
			      "limit_first_letter_length": "16",
			      "keep_full_pinyin": "false"
                }
            }
        }
    },
    "mappings":{
        "article":{
            "properties":{
                "abstract":{
                    "type":"text",
                    "analyzer":"ik_max_word"
                },
                "article_id":{
                    "type":"keyword"
                },
                "content":{
                    "type":"text",
                    "fields":{
                        "pinyin":{
                            "type":"text",
                            "analyzer":"ik_pinyin_analyzer"
                        }
                    },
                    "analyzer":"ik_max_word"
                },
                "id":{
                    "type":"long"
                },
                "recommend":{
                    "type":"byte"
                },
                "sort":{
                    "type":"long"
                },
                "status":{
                    "type":"byte"
                },
                "tag":{
                    "type":"keyword"
                },
                "thum":{
                    "type":"text"
                },
                "time":{
                    "type":"date"
                },
                "title":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "type":"keyword"
                        },
                        "pinyin":{
                            "type":"text",
                            "analyzer":"ik_pinyin_analyzer"
                        }
                    },
                    "analyzer":"ik_max_word"
                },
                "updata_time":{
                    "type":"date"
                },
                "username":{
                    "type":"text",
                    "fields":{
                        "keyword":{
                            "type":"keyword"
                        },
                        "pinyin":{
                            "type":"text",
                            "analyzer":"ik_pinyin_analyzer"
                        }
                    },
                    "analyzer":"ik_max_word"
                }
            }
        }
    }
}
```