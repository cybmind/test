## 介绍
在我们自己的购买的服务器环境中，一般是买的1g的内存，但是当服务器里面的东西装的比较多的时候就会导致内存不够用了，本文将模拟一个真实的内存不够用的情况下，如何通过修改虚拟内存来让系统正常运行，我们这里的环境是搭建一个ElasticSearch搜索的环境，但是我们的服务器内存只有1g，下面将演示如何在将1g的虚拟内存修改为4G。

## 搭建ElasticSearch环境
现在我们的服务器环境是空的，什么都没有，我们这里先将ElasticSearch上传到服务器，然后将jdk和ElasticSearch安装好。
### 安装jdk
安装链接

### 安装ElasticSearch
安装链接

### 启动ElasticSearch
1. 启动ElasticSearch，会发现启动的时候报错了，原因是我们的服务器现在的内存并不能满足ElasticSearch需要的内存。
```
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch 
[1] 3228
[esyonghu@localhost elasticsearch-6.4.0]$ Java HotSpot(TM) 64-Bit Server VM warning: INFO: os::commit_memory(0x000000008a660000, 1973026816, 0) failed; error='Cannot allocate memory' (errno=12)
#
# There is insufficient memory for the Java Runtime Environment to continue.
# Native memory allocation (mmap) failed to map 1973026816 bytes for committing reserved memory.
# An error report file with more information is saved as:
# logs/hs_err_pid3228.log
[esyonghu@localhost elasticsearch-6.4.0]$ 
```
2. 查看我们的服务器的内存，使用命令 `free`,可以看到我们服务器的内存是1g, 这个时候就需要我们修改虚拟内存来解决该问题了。
```
[esyonghu@localhost elasticsearch-6.4.0]$ free -m
             total       used       free     shared    buffers     cached
Mem:           980        582        397          2         23        245
-/+ buffers/cache:        313        667
Swap:            0          0          0
[esyonghu@localhost elasticsearch-6.4.0]$ 
```

### 创建swap文件
1. 进入`/usr`目录
```
[root@localhost usr]$ pwd
/usr
[root@localhost usr]$ 
```

2. 创建`swap`文件夹,并进入该文件夹
```
[root@localhost usr]# mkdir swap
[root@localhost usr]# cd swap/
[root@localhost swap]# pwd
/usr/swap
[root@localhost swap]# 
```

3. 创建`swapfile`文件,使用命令`dd if=/dev/zero of=/usr/swap/swapfile bs=1M count=4096`
```
[root@localhost swap]# dd if=/dev/zero of=/usr/swap/swapfile bs=1M count=4096
记录了4096+0 的读入
记录了4096+0 的写出
4294967296字节(4.3 GB)已复制，15.7479 秒，273 MB/秒
[root@localhost swap]#
```

### 查看swap文件

1. 使用命令`du -sh /usr/swap/swapfile`,可以看到我们创建的这个swap文件为4g
```
[root@localhost swap]# du -sh /usr/swap/swapfile
4.1G	/usr/swap/swapfile
[root@localhost swap]# 
```

### 将目标设置为swap分区文件
1. 使用命令`mkswap /usr/swap/swapfile`将swapfile文件设置为swap分区文件
```
[root@localhost swap]# mkswap /usr/swap/swapfile
mkswap: /usr/swap/swapfile: warning: don't erase bootbits sectors
        on whole disk. Use -f to force.
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=5bd241ff-5375-449d-9975-5fdd429df784
[root@localhost swap]#
```

### 激活swap区，并立即启用交换区文件
1. 使用命令`swapon /usr/swap/swapfile`
```
[root@localhost swap]# swapon /usr/swap/swapfile
[root@localhost swap]#
```
2. 使用命令`free -m` 来查看现在的内存,可以看到里面的Swap分区变成了4095M，也就是4G内存。
```
[root@localhost swap]# free -m
             total       used       free     shared    buffers     cached
Mem:           980        910         70          3          8        575
-/+ buffers/cache:        326        654
Swap:         4095          0       4095
[root@localhost swap]#
```

### 设置开机自动启用虚拟内存，在`etc/fstab`文件中加入如下命令
1. 使用vim编辑器打开/etc/fstab文件

2. 在文件中加入如下内容
```
/usr/swap/swapfile2 swap swap defaults 0 0
```

### 使用reboot命令重启服务器
1. 输入`reboot` 命令来重启
```
	[root@localhost swap]# reboot

	Broadcast message from liaocheng@localhost.localdomain
		(/dev/pts/1) at 3:56 ...

	The system is going down for reboot NOW!
	[root@localhost swap]# Connection to 192.168.136.142 closed by remote host.
	Connection to 192.168.136.142 closed.
	[进程已完成]
```

2. 重启完成过后使用free -m 命令来查看现在的内存是否挂在上了。
```
[root@localhost swap]# free -m
             total       used       free     shared    buffers     cached
Mem:           980        910         70          3          8        575
-/+ buffers/cache:        326        654
Swap:         4095          0       4095
```

### 再次启动ElasticSearch看看是否还会报内存不足的错误
1. 还是切换到esyonghu去启动（这里为什么要使用es用户启动就先不介绍了，这是elasticsearch里面的知识，这里只是用elasticsearch来模拟内存不足的情况）,可以看到已经不会有内存不足的问题了。
```
[esyonghu@localhost elasticsearch-6.4.0]$ ./bin/elasticsearch &
[1] 2898
[esyonghu@localhost elasticsearch-6.4.0]$ [2019-03-06T04:00:24,841][INFO ][o.e.n.Node               ] [] initializing ...
[2019-03-06T04:00:24,928][INFO ][o.e.e.NodeEnvironment    ] [dMy5nR5] using [1] data paths, mounts [[/ (rootfs)]], net usable_space [7.6gb], net total_space [17.3gb], types [rootfs]
[2019-03-06T04:00:24,928][INFO ][o.e.e.NodeEnvironment    ] [dMy5nR5] heap size [1.9gb], compressed ordinary object pointers [true]
[2019-03-06T04:00:25,018][INFO ][o.e.n.Node               ] [dMy5nR5] node name derived from node ID [dMy5nR5fThaBb-Q2T0txdA]; set [node.name] to override
[2019-03-06T04:00:25,018][INFO ][o.e.n.Node               ] [dMy5nR5] version[6.4.0], pid[2898], build[default/tar/595516e/2018-08-17T23:18:47.308994Z], OS[Linux/2.6.32-696.el6.x86_64/amd64], JVM[Oracle Corporation/Java HotSpot(TM) 64-Bit Server VM/1.8.0_181/25.181-b13]
[2019-03-06T04:00:25,018][INFO ][o.e.n.Node               ] [dMy5nR5] JVM arguments [-Xms2g, -Xmx2g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch.24Q3S9AE, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -XX:+PrintGCDetails, -XX:+PrintGCDateStamps, -XX:+PrintTenuringDistribution, -XX:+PrintGCApplicationStoppedTime, -Xloggc:logs/gc.log, -XX:+UseGCLogFileRotation, -XX:NumberOfGCLogFiles=32, -XX:GCLogFileSize=64m, -Des.path.home=/home/esyonghu/elasticsearch-6.4.0, -Des.path.conf=/home/esyonghu/elasticsearch-6.4.0/config, -Des.distribution.flavor=default, -Des.distribution.type=tar]
[2019-03-06T04:00:28,022][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [aggs-matrix-stats]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [analysis-common]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [ingest-common]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [lang-expression]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [lang-mustache]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [lang-painless]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [mapper-extras]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [parent-join]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [percolator]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [rank-eval]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [reindex]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [repository-url]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [transport-netty4]
[2019-03-06T04:00:28,023][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [tribe]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-core]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-deprecation]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-graph]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-logstash]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-ml]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-monitoring]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-rollup]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-security]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-sql]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-upgrade]
[2019-03-06T04:00:28,024][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded module [x-pack-watcher]
[2019-03-06T04:00:28,025][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded plugin [analysis-ik]
[2019-03-06T04:00:28,025][INFO ][o.e.p.PluginsService     ] [dMy5nR5] loaded plugin [analysis-pinyin]
[2019-03-06T04:00:31,315][INFO ][o.e.x.s.a.s.FileRolesStore] [dMy5nR5] parsed [0] roles from file [/home/esyonghu/elasticsearch-6.4.0/config/roles.yml]
[2019-03-06T04:00:32,017][INFO ][o.e.x.m.j.p.l.CppLogMessageHandler] [controller/2947] [Main.cc@109] controller (64 bit): Version 6.4.0 (Build cf8246175efff5) Copyright (c) 2018 Elasticsearch BV
[2019-03-06T04:00:32,495][DEBUG][o.e.a.ActionModule       ] Using REST wrapper from plugin org.elasticsearch.xpack.security.Security
[2019-03-06T04:00:32,768][INFO ][o.e.d.DiscoveryModule    ] [dMy5nR5] using discovery type [zen]
[2019-03-06T04:00:33,628][INFO ][o.e.n.Node               ] [dMy5nR5] initialized
[2019-03-06T04:00:33,628][INFO ][o.e.n.Node               ] [dMy5nR5] starting ...
[2019-03-06T04:00:33,860][INFO ][o.e.t.TransportService   ] [dMy5nR5] publish_address {192.168.136.142:9300}, bound_addresses {[::]:9300}
[2019-03-06T04:00:33,884][INFO ][o.e.b.BootstrapChecks    ] [dMy5nR5] bound or publishing to a non-loopback address, enforcing bootstrap checks
[2019-03-06T04:00:36,995][INFO ][o.e.c.s.MasterService    ] [dMy5nR5] zen-disco-elected-as-master ([0] nodes joined)[, ], reason: new_master {dMy5nR5}{dMy5nR5fThaBb-Q2T0txdA}{ldgTZ1XZSfOpda9uP4treA}{192.168.136.142}{192.168.136.142:9300}{ml.machine_memory=1028210688, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true}
[2019-03-06T04:00:37,003][INFO ][o.e.c.s.ClusterApplierService] [dMy5nR5] new_master {dMy5nR5}{dMy5nR5fThaBb-Q2T0txdA}{ldgTZ1XZSfOpda9uP4treA}{192.168.136.142}{192.168.136.142:9300}{ml.machine_memory=1028210688, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true}, reason: apply cluster state (from master [master {dMy5nR5}{dMy5nR5fThaBb-Q2T0txdA}{ldgTZ1XZSfOpda9uP4treA}{192.168.136.142}{192.168.136.142:9300}{ml.machine_memory=1028210688, xpack.installed=true, ml.max_open_jobs=20, ml.enabled=true} committed version [1] source [zen-disco-elected-as-master ([0] nodes joined)[, ]]])
[2019-03-06T04:00:37,058][INFO ][o.e.x.s.t.n.SecurityNetty4HttpServerTransport] [dMy5nR5] publish_address {192.168.136.142:9200}, bound_addresses {[::]:9200}
[2019-03-06T04:00:37,058][INFO ][o.e.n.Node               ] [dMy5nR5] started
[2019-03-06T04:00:37,177][INFO ][o.w.a.d.Monitor          ] try load config from /home/esyonghu/elasticsearch-6.4.0/config/analysis-ik/IKAnalyzer.cfg.xml
[2019-03-06T04:00:37,179][INFO ][o.w.a.d.Monitor          ] try load config from /home/esyonghu/elasticsearch-6.4.0/plugins/ik/config/IKAnalyzer.cfg.xml
[2019-03-06T04:00:37,888][INFO ][o.e.m.j.JvmGcMonitorService] [dMy5nR5] [gc][4] overhead, spent [486ms] collecting in the last [1.2s]
[2019-03-06T04:00:38,435][WARN ][o.e.x.s.a.s.m.NativeRoleMappingStore] [dMy5nR5] Failed to clear cache for realms [[]]
[2019-03-06T04:00:38,469][INFO ][o.e.l.LicenseService     ] [dMy5nR5] license [c91cae39-79d7-4a0e-b40b-b1918a45f80c] mode [trial] - valid
[2019-03-06T04:00:38,477][INFO ][o.e.g.GatewayService     ] [dMy5nR5] recovered [5] indices into cluster_state
[2019-03-06T04:00:38,902][WARN ][o.e.x.s.a.s.m.NativeRoleMappingStore] [dMy5nR5] Failed to clear cache for realms [[]]
[2019-03-06T04:00:39,106][INFO ][o.e.c.r.a.AllocationService] [dMy5nR5] Cluster health status changed from [RED] to [YELLOW] (reason: [shards started [[mynote2][2]] ...]).
```
2. 现在使用`free -m`来查看内存使用情况, 可以看到swap已经被使用了1.7G
```
[esyonghu@localhost elasticsearch-6.4.0]$ free -m
             total       used       free     shared    buffers     cached
Mem:           980        916         64          0          3         33
-/+ buffers/cache:        880        100
Swap:         4095       1735       2360
[esyonghu@localhost elasticsearch-6.4.0]$
```