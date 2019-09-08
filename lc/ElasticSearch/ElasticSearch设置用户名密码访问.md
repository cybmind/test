### 1, 启动trial license（30天试用）
```
curl -H "Content-Type:application/json" -XPOST  http://127.0.0.1:19200/_xpack/license/start_trial?acknowledge=true
```

###2，设置用户名和密码
```
bin/elasticsearch-setup-passwords interactive
```

###3，如果发现如下错误提示
```
org.elasticsearch.ElasticsearchException: Security must be explicitly enabled when using a trial license. Enable security by setting [xpack.security.enabled] to [true] in the elasticsearch.yml file and restart the node
```
###4, 那么就需要在配置文件中开启x-pack验证, 修改config目录下面的elasticsearch.yml文件，在里面添加如下内容,并重启
```
 xpack.security.enabled: true
```

###5，再次执行设置用户名和密码的命令,这里需要为4个用户分别设置密码，elastic, kibana, logstash_system,beats_system
```
bin/elasticsearch-setup-passwords interactive
```
```
Initiating the setup of passwords for reserved users elastic,kibana,logstash_system,beats_system.
You will be prompted to enter passwords as the process progresses.
Please confirm that you would like to continue [y/N]y
Enter password for [elastic]: 
passwords must be at least [6] characters long
Try again.
Enter password for [elastic]: 
Reenter password for [elastic]: 
Passwords do not match.
Try again.
Enter password for [elastic]: 
Reenter password for [elastic]: 
Enter password for [kibana]: 
Reenter password for [kibana]: 
Enter password for [logstash_system]: 
Reenter password for [logstash_system]: 
Enter password for [beats_system]: 
Reenter password for [beats_system]: 
Changed password for user [kibana]
Changed password for user [logstash_system]
Changed password for user [beats_system]
Changed password for user [elastic]
```

###6，修改密码命令如下
```
curl -H "Content-Type:application/json" -XPOST -u elastic 'http://127.0.0.1:9200/_xpack/security/user/elastic/_password' -d '{ "password" : "123456" }'
```


###7，修改kibana配置文件,config下的kibana.yml,添加如下内容
```
elasticsearch.username: "elastic"
elasticsearch.password: "123456"
```