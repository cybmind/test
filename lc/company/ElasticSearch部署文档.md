## ES版本信息
es 版本为 6.4.0
ik 分词器版本 6.4.0
pinyin 分词器 6.4.0
## JDK安装
jdk下载链接:https://pan.baidu.com/s/1I8MT59OZ3igUXNyZ1I9lLQ  密码:9kds
Linux安装：
解压文件到指定目录，修改系统配置文件/etc/profile
添加以下内容
```
export JAVA_HOME=你的jdk解压目录(bin的上层目录)
export PATH=$JAVA_HOME/bin:$PATH
```
## ElasticSearch安装
[安装教程链接](http://uninote.com.cn/book/1079089832#565 "教程链接")
## 服务器部署
### 安装分词器插件
1. 安装ik分词器插件
链接:https://pan.baidu.com/s/1YDW8x3gfFYU2rcPfAJ0Zrg  密码:tb2c
将分词器插件解压到plugins里面
2. 安装pinyin分词器插件
链接:https://pan.baidu.com/s/18tU7rB2ArD0a8OKadCzDFA  密码:4j6b
将分词器插件解压到plugins里面

## 启动ES
切换到esyonghu,然后执行bin目录下面的elasticsearch文件

## 给ES建立文章索引
使用postman这类的工具发送请求，或者使用curl的方式建立
postman:
打开postman，在url里面输入以下信息,将途中的ip替换成你的服务器ip,mynote_dev是索引名字
![](http://uninote.com.cn/docs/1079089832/__pic/UMxIhkmw.png)
发送参数为：
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
			      "keep_full_pinyin": "true"
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
点击send即可创建文件
