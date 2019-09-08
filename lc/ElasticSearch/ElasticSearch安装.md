### ElasticSearch安装
#### 1, 官网下载ElasticSearch
```url
官网地址：
https://www.elastic.co/cn/downloads/past-releases/
https://www.elastic.co/products/elasticsearch
```

#### 2, 将文件上传到Linux,可以使用第三方工具filezilla

#### 3, 解压elasticsearch-6.4.0.tar.gz`tar -zxvf elasticsearch-6.4.0.tar.gz `
```
[root@localhost elasticsearch]# tar -zxvf elasticsearch-6.4.0.tar.gz 
elasticsearch-6.4.0/
...
```
#### 4, 进入elasticsearch目录查看文件夹内容 `cd elasticsearch-6.4.0`
```
[root@localhost elasticsearch]# cd elasticsearch-6.4.0
[root@localhost elasticsearch-6.4.0]# ll
总用量 436
drwxr-xr-x.  3 root root   4096 8月  29 17:52 bin
drwxr-xr-x.  2 root root    148 8月  18 07:23 config
drwxr-xr-x.  3 root root   4096 8月  18 07:23 lib
-rw-r--r--.  1 root root  13675 8月  18 07:11 LICENSE.txt
drwxr-xr-x.  2 root root      6 8月  18 07:22 logs
drwxr-xr-x. 27 root root   4096 8月  18 07:23 modules
-rw-r--r--.  1 root root 401465 8月  18 07:22 NOTICE.txt
drwxr-xr-x.  2 root root      6 8月  18 07:22 plugins
-rw-r--r--.  1 root root   8511 8月  18 07:11 README.textile
[root@localhost elasticsearch-6.4.0]# 

```
#### 5, 运行bin下面的ealashicsearch`./bin/elasticsearch`
```
root@localhost elasticsearch-6.4.0]# ./bin/elasticsearch
[2018-08-29T18:07:52,437][WARN ][o.e.b.ElasticsearchUncaughtExceptionHandler] [] uncaught exception in thread [main]
org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:140) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:127) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:124) ~[elasticsearch-cli-6.4.0.jar:6.4.0]
	at org.elasticsearch.cli.Command.main(Command.java:90) ~[elasticsearch-cli-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:93) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:86) ~[elasticsearch-6.4.0.jar:6.4.0]
Caused by: java.lang.RuntimeException: can not run elasticsearch as root
	at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:104) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:171) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:326) ~[elasticsearch-6.4.0.jar:6.4.0]
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:136) ~[elasticsearch-6.4.0.jar:6.4.0]
	... 6 more
[root@localhost elasticsearch-6.4.0]# 

```
#### 6, 出错是因为不能以root用户的方式启动，必须新建一个用户和组
```
创建es组
[root@localhost elasticsearch-6.4.0]# groupadd eszu
创建es用户
[root@localhost elasticsearch-6.4.0]# useradd esyonghu
指定用户密码
[root@localhost elasticsearch-6.4.0]# passwd esyonghu
更改用户 esyonghu 的密码 。
新的 密码：
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。
[root@localhost elasticsearch-6.4.0]# 

修改文件所属用户
[root@localhost elasticsearch]# chown -R esyonghu elasticsearch-6.4.0
修改文件所属组
[root@localhost elasticsearch]# chgrp -R eszu elasticsearch-6.4.0
```
#### 7, 切换到esyonghu`su esyonghu`, 启动elasticserach`./bin/elasticsearch`
```
[root@localhost elasticsearch-6.4.0]# su esyonghu
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch
[2018-08-30T10:20:25,387][INFO ][o.e.n.Node               ] [] initializing ...
[2018-08-30T10:20:25,919][INFO ][o.e.e.NodeEnvironment    ] [cRjfMu5] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [14.8gb], net total_space [16.9gb], types [rootfs]
[2018-08-30T10:20:25,920][INFO ][o.e.e.NodeEnvironment    ] [cRjfMu5] heap size [1015.6mb], compressed ordinary object pointers [true]
[2018-08-30T10:20:25,925][INFO ][o.e.n.Node               ] [cRjfMu5] node name derived from node ID [cRjfMu5qTwmlfTzHHWSAiQ]; set [node.name] to override
[2018-08-30T10:20:25,926][INFO ][o.e.n.Node               ] [cRjfMu5] version[6.4.0], pid[1331], build[default/tar/595516e/2018-08-17T23:18:47.308994Z], OS[Linux/3.10.0-862.el7.x86_64/amd64], JVM[Oracle Corporation/Java HotSpot(TM) 64-Bit Server VM/1.8.0_181/25.181-b13]
[2018-08-30T10:20:25,943][INFO ][o.e.n.Node               ] [cRjfMu5] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.CT7q3O6F, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -XX:+PrintGCDetails, -XX:+PrintGCDateStamps, -XX:+PrintTenuringDistribution, -XX:+PrintGCApplicationStoppedTime, -Xloggc:logs/gc.log, -XX:+UseGCLogFileRotation, -XX:NumberOfGCLogFiles=32, -XX:GCLogFileSize=64m, -Des.path.home=/home/elasticsearch/elasticsearch-6.4.0, -Des.path.conf=/home/elasticsearch/elasticsearch-6.4.0/config, -Des.distribution.flavor=default, -Des.distribution.type=tar]
[2018-08-30T10:20:32,189][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [aggs-matrix-stats]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [analysis-common]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [ingest-common]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-expression]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-mustache]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-painless]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [mapper-extras]
[2018-08-30T10:20:32,190][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [parent-join]
[2018-08-30T10:20:32,191][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [percolator]
[2018-08-30T10:20:32,191][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [rank-eval]
[2018-08-30T10:20:32,191][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [reindex]
[2018-08-30T10:20:32,191][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [repository-url]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [transport-netty4]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [tribe]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-core]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-deprecation]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-graph]
[2018-08-30T10:20:32,192][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-logstash]
[2018-08-30T10:20:32,193][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-ml]
[2018-08-30T10:20:32,193][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-monitoring]
[2018-08-30T10:20:32,193][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-rollup]
[2018-08-30T10:20:32,193][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-security]
[2018-08-30T10:20:32,193][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-sql]
[2018-08-30T10:20:32,194][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-upgrade]
[2018-08-30T10:20:32,194][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-watcher]
[2018-08-30T10:20:32,194][INFO ][o.e.p.PluginsService     ] [cRjfMu5] no plugins loaded
[2018-08-30T10:20:41,918][INFO ][o.e.x.s.a.s.FileRolesStore] [cRjfMu5] parsed [0] roles from file [/home/elasticsearch/elasticsearch-6.4.0/config/roles.yml]
[2018-08-30T10:20:44,164][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/1379] [Main.cc@109] controller (64 bit): Version 6.4.0 (Build cf8246175efff5) Copyright (c) 2018 Elasticsearch BV
[2018-08-30T10:20:45,321][DEBUG][o.e.a.ActionModule       ] Using REST wrapper from plugin org.elasticsearch.xpack.security.Security
[2018-08-30T10:20:45,935][INFO ][o.e.d.DiscoveryModule    ] [cRjfMu5] using discovery type [zen]
[2018-08-30T10:20:47,619][INFO ][o.e.n.Node               ] [cRjfMu5] initialized
[2018-08-30T10:20:47,620][INFO ][o.e.n.Node               ] [cRjfMu5] starting ...
[2018-08-30T10:20:48,736][INFO ][o.e.t.TransportService   ] [cRjfMu5] publish_address {127.0.0.1:9300}, bound_addresses {[::1]:9300}, {127.0.0.1:9300}
[2018-08-30T10:20:48,772][WARN ][o.e.b.BootstrapChecks    ] [cRjfMu5] max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2018-08-30T10:20:48,773][WARN ][o.e.b.BootstrapChecks    ] [cRjfMu5] max number of threads [3802] for user [esyonghu] is too low, increase to at least [4096]
[2018-08-30T10:20:48,773][WARN ][o.e.b.BootstrapChecks    ] [cRjfMu5] max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2018-08-30T10:20:51,982][INFO ][o.e.c.s.MasterService    ] [cRjfMu5] zen-disco-elected-as-master ([0] nodes joined)[, ], reason: new_master {cRjfMu5}{cRjfMu5qTwmlfTzHHWSAiQ}{oJoeh0w-RAijeCMwTKLe-w}{127.0.0.1}{127.0.0.1:9300}{ml.machine_memory=1021906944, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true}
[2018-08-30T10:20:51,993][INFO ][o.e.c.s.ClusterApplierService] [cRjfMu5] new_master {cRjfMu5}{cRjfMu5qTwmlfTzHHWSAiQ}{oJoeh0w-RAijeCMwTKLe-w}{127.0.0.1}{127.0.0.1:9300}{ml.machine_memory=1021906944, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true}, reason: apply cluster state (from master [master {cRjfMu5}{cRjfMu5qTwmlfTzHHWSAiQ}{oJoeh0w-RAijeCMwTKLe-w}{127.0.0.1}{127.0.0.1:9300}{ml.machine_memory=1021906944, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true} committed version [1] source [zen-disco-elected-as-master ([0] nodes joined)[, ]]])
[2018-08-30T10:20:52,052][WARN ][o.e.x.s.a.s.m.NativeRoleMappingStore] [cRjfMu5] Failed to clear cache for realms [[]]
[2018-08-30T10:20:52,066][INFO ][o.e.x.s.t.n.SecurityNetty4HttpServerTransport] [cRjfMu5] publish_address {127.0.0.1:9200}, bound_addresses {[::1]:9200}, {127.0.0.1:9200}
[2018-08-30T10:20:52,066][INFO ][o.e.n.Node               ] [cRjfMu5] started
[2018-08-30T10:20:52,242][INFO ][o.e.g.GatewayService     ] [cRjfMu5] recovered [0] indices into cluster_state
[2018-08-30T10:20:52,846][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.triggered_watches] for index patterns [.triggered_watches*]
[2018-08-30T10:20:52,902][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.watches] for index patterns [.watches*]
[2018-08-30T10:20:52,967][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.watch-history-9] for index patterns [.watcher-history-9*]
[2018-08-30T10:20:53,016][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.monitoring-logstash] for index patterns [.monitoring-logstash-6-*]
[2018-08-30T10:20:53,106][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.monitoring-es] for index patterns [.monitoring-es-6-*]
[2018-08-30T10:20:53,148][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.monitoring-beats] for index patterns [.monitoring-beats-6-*]
[2018-08-30T10:20:53,196][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.monitoring-alerts] for index patterns [.monitoring-alerts-6]
[2018-08-30T10:20:53,255][INFO ][o.e.c.m.MetaDataIndexTemplateService] [cRjfMu5] adding template [.monitoring-kibana] for index patterns [.monitoring-kibana-6-*]
[2018-08-30T10:20:53,489][INFO ][o.e.l.LicenseService     ] [cRjfMu5] license [29b596fe-89fc-47bb-9ed5-01827361589d] mode [basic] - valid

```
#### 8, 这时候elasticsearch已经成功启动，我们来测试一下`curl localhost:9200`,如果得到了这些数据就说明成功了，但是在浏览器里面并不能访问，需要修改配置文件
```
新打开一个窗口,输入curl localhost:9200

[root@localhost /]# curl localhost:9200
{
  "name" : "cRjfMu5",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "UhsXc7AVTaSCjSxLYNrRfw",
  "version" : {
    "number" : "6.4.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "595516e",
    "build_date" : "2018-08-17T23:18:47.308994Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```
#### 9, 配置elasticsearch端口和ip
进入config文件夹，里面有一个elasticsearch.yml文件，使用`vim elasticsearch.yml`命令来编辑该文件，添加如下内容 
```
network.host: 192.168.145.129
http.port: 9200
```
#### 10, 再次启动elasticsearch,会发现已经启动不了了
```
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch
[2018-08-30T11:29:22,930][INFO ][o.e.n.Node               ] [] initializing ...
[2018-08-30T11:29:23,380][INFO ][o.e.e.NodeEnvironment    ] [cRjfMu5] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [14.8gb], net total_space [16.9gb], types [rootfs]
[2018-08-30T11:29:23,381][INFO ][o.e.e.NodeEnvironment    ] [cRjfMu5] heap size [1015.6mb], compressed ordinary object pointers [true]
[2018-08-30T11:29:23,382][INFO ][o.e.n.Node               ] [cRjfMu5] node name derived from node ID [cRjfMu5qTwmlfTzHHWSAiQ]; set [node.name] to override
[2018-08-30T11:29:23,382][INFO ][o.e.n.Node               ] [cRjfMu5] version[6.4.0], pid[1762], build[default/tar/595516e/2018-08-17T23:18:47.308994Z], OS[Linux/3.10.0-862.el7.x86_64/amd64], JVM[Oracle Corporation/Java HotSpot(TM) 64-Bit Server VM/1.8.0_181/25.181-b13]
[2018-08-30T11:29:23,382][INFO ][o.e.n.Node               ] [cRjfMu5] JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.o8Uhb9Ro, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -XX:+PrintGCDetails, -XX:+PrintGCDateStamps, -XX:+PrintTenuringDistribution, -XX:+PrintGCApplicationStoppedTime, -Xloggc:logs/gc.log, -XX:+UseGCLogFileRotation, -XX:NumberOfGCLogFiles=32, -XX:GCLogFileSize=64m, -Des.path.home=/home/elasticsearch/elasticsearch-6.4.0, -Des.path.conf=/home/elasticsearch/elasticsearch-6.4.0/config, -Des.distribution.flavor=default, -Des.distribution.type=tar]
[2018-08-30T11:29:31,350][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [aggs-matrix-stats]
[2018-08-30T11:29:31,351][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [analysis-common]
[2018-08-30T11:29:31,352][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [ingest-common]
[2018-08-30T11:29:31,352][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-expression]
[2018-08-30T11:29:31,352][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-mustache]
[2018-08-30T11:29:31,353][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [lang-painless]
[2018-08-30T11:29:31,353][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [mapper-extras]
[2018-08-30T11:29:31,354][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [parent-join]
[2018-08-30T11:29:31,354][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [percolator]
[2018-08-30T11:29:31,354][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [rank-eval]
[2018-08-30T11:29:31,355][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [reindex]
[2018-08-30T11:29:31,355][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [repository-url]
[2018-08-30T11:29:31,356][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [transport-netty4]
[2018-08-30T11:29:31,356][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [tribe]
[2018-08-30T11:29:31,356][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-core]
[2018-08-30T11:29:31,357][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-deprecation]
[2018-08-30T11:29:31,357][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-graph]
[2018-08-30T11:29:31,357][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-logstash]
[2018-08-30T11:29:31,358][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-ml]
[2018-08-30T11:29:31,358][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-monitoring]
[2018-08-30T11:29:31,359][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-rollup]
[2018-08-30T11:29:31,359][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-security]
[2018-08-30T11:29:31,359][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-sql]
[2018-08-30T11:29:31,370][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-upgrade]
[2018-08-30T11:29:31,370][INFO ][o.e.p.PluginsService     ] [cRjfMu5] loaded module [x-pack-watcher]
[2018-08-30T11:29:31,372][INFO ][o.e.p.PluginsService     ] [cRjfMu5] no plugins loaded
[2018-08-30T11:29:43,859][INFO ][o.e.x.s.a.s.FileRolesStore] [cRjfMu5] parsed [0] roles from file [/home/elasticsearch/elasticsearch-6.4.0/config/roles.yml]
[2018-08-30T11:29:45,757][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/1810] [Main.cc@109] controller (64 bit): Version 6.4.0 (Build cf8246175efff5) Copyright (c) 2018 Elasticsearch BV
[2018-08-30T11:29:47,089][DEBUG][o.e.a.ActionModule       ] Using REST wrapper from plugin org.elasticsearch.xpack.security.Security
[2018-08-30T11:29:47,915][INFO ][o.e.d.DiscoveryModule    ] [cRjfMu5] using discovery type [zen]
[2018-08-30T11:29:50,345][INFO ][o.e.n.Node               ] [cRjfMu5] initialized
[2018-08-30T11:29:50,345][INFO ][o.e.n.Node               ] [cRjfMu5] starting ...
[2018-08-30T11:29:51,185][INFO ][o.e.t.TransportService   ] [cRjfMu5] publish_address {192.168.145.129:9300}, bound_addresses {192.168.145.129:9300}
[2018-08-30T11:29:51,351][INFO ][o.e.b.BootstrapChecks    ] [cRjfMu5] bound or publishing to a non-loopback address, enforcing bootstrap checks
ERROR: [3] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [3802] for user [esyonghu] is too low, increase to at least [4096]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2018-08-30T11:29:51,486][INFO ][o.e.n.Node               ] [cRjfMu5] stopping ...
[2018-08-30T11:29:51,570][INFO ][o.e.n.Node               ] [cRjfMu5] stopped
[2018-08-30T11:29:51,571][INFO ][o.e.n.Node               ] [cRjfMu5] closing ...
[2018-08-30T11:29:51,633][INFO ][o.e.n.Node               ] [cRjfMu5] closed
[2018-08-30T11:29:51,634][INFO ][o.e.x.m.j.p.NativeController] Native controller process has stopped - no new native processes can be started
[esyonghu@localhost elasticsearch-6.4.0]$ 
```
#### 11, 修改错误，可以看到控制台打印的信息里面有3个错误
```
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [3802] for user [esyonghu] is too low, increase to at least [4096]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
第一个错误需要将当前用户的软硬调用限制调大
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
切换到root用户,然后执行`vim /etc/security/limits.conf`
```
[root@localhost elasticsearch-6.4.0]# vim /etc/security/limits.conf
```
在文件末尾加上
```
esyonghu soft nofile 65536
esyonghu hard nofile 131072
esyonghu soft nproc 2048
esyonghu hard nproc 4096
```

第二个错误需要修改/etc/security/limits.d/20-nproc.conf 文件
[2]: max number of threads [3802] for user [esyonghu] is too low, increase to at least [4096]

修改/etc/security/limits.d/20-nproc.conf 文件，这里需要注意一下，可能不叫20-nproc.conf 也可能是90-nproc.conf
```
vim /etc/security/limits.d/20-nproc.conf
```
在文件中添加
```
esyonghu   soft    nproc     4096
```
第三个错误需要修改/etc/sysctl.conf
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
使用命令 `vim /etc/sysctl.conf`来编辑该文件，在文件中加入如下内容
```
加上vm.max_map_count=262144
```
需要使改文件生效，执行sysctl -p
```
[root@localhost etc]# sysctl -p
vm.max_map_count = 262144
```


#### 12,再次切换到esyonghu去elasticsearch文件目录启动elasticsearch
```
[root@localhost elasticsearch-6.4.0]# su esyonghu
[esyonghu@localhost elasticsearch-6.4.0]$ 
[esyonghu@localhost elasticsearch-6.4.0]$ cd /home/elasticsearch/elasticsearch-6.4.0
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch
现在启动成功了，使用浏览器访问试一下
```
#### 13,关闭防火墙
```
再次访问的时候发现还是不行，这时候需要关闭防火墙
检查防火墙状态
先切换到root用户
[esyonghu@localhost elasticsearch-6.4.0]$ su root
密码：
[root@localhost elasticsearch-6.4.0]# 

查看防火墙状态
[root@localhost elasticsearch-6.4.0]# firewall-cmd --state
running
[root@localhost elasticsearch-6.4.0]# 

状态是running,需要关闭防火墙
执行systemctl stop firewalld.service
[root@localhost elasticsearch-6.4.0]# systemctl stop firewalld.service

再次检查防火墙状态，就是为运行了
[root@localhost elasticsearch-6.4.0]# firewall-cmd --state
not running
[root@localhost elasticsearch-6.4.0]# 
```


#### 14, 切换到esyonghu再次启动
```
[root@localhost elasticsearch-6.4.0]# su esyonghu
[esyonghu@localhost elasticsearch-6.4.0]$ 
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch
[2018-08-30T12:09:59,267][INFO ][o.e.n.Node               ] [] initializing ...
启动成功，使用浏览器访问就可以了
```
###### 以后台方式启动elasticsearch
```
./bin/elasticsearch -d

[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch -d
[esyonghu@localhost elasticsearch-6.4.0]$ 
 
```
###### 结束elastsearch
```
输入jps命令
杀死elasticsearch进程
```

###### 如果绑定本地ip失败，那么在elasticsearch.yml里面加上
```
	bootstrap.memory_lock: false
	bootstrap.system_call_filter: false
	network.host: 0.0.0.0
	http.port: 9200
```