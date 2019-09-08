### 1，查看系统Swap空间使用
```
[root@es-test ~]# free -m
             total       used       free     shared    buffers     cached
			 Mem:          2006        158       1847          0          8         82
			 -/+ buffers/cache:         68       1937
			 Swap:          976          0        976
[root@es-test ~]# 
```

### 2, 创建swap文件
```
[root@es-test swap]# dd if=/dev/zero of=/usr/swap/swapfile2 bs=1M count=4096
记录了4096+0 的读入
记录了4096+0 的写出
4294967296字节(4.3 GB)已复制，37.6603 秒，114 MB/秒
[root@es-test swap]# 
```

### 3, 查看创建文件的大小
```
[root@es-test swap]# du -sh /usr/swap/swapfile2
4.1G	/usr/swap/swapfile2
[root@es-test swap]# 
```

### 4, 将目标文件设置为swap分区文件
```
[root@es-test swap]# mkswap /usr/swap/swapfile2
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=180b640c-7329-493f-b6f1-4ccdb1313882
[root@es-test swap]# 
```

### 5, 激活swap，立即启用交换分区文件
```
[root@es-test swap]# swapon /usr/swap/swapfile2
```

### 6, 设置开机自动启用虚拟内存,在/etc/fstab文件中加入如下命令：
```
/usr/swap/swapfile2 swap swap defaults 0 0
```

### 7, 使用reboot命令重启查看内存
```
[root@es-test ~]# free -m
             total       used       free     shared    buffers     cached
			 Mem:          2006        144       1862          0          7         65
			 -/+ buffers/cache:         70       1935
			 Swap:         4095          0       4095
[root@es-test ~]# 
```

### 8，查看所有的swap分区 
```
[esyonghu@localhost elasticsearch-6.4.0]$ swapon -s
Filename				Type		Size	Used	Priority
/usr/swap/swapfile                      file		4194300	1776692	-1
[esyonghu@localhost elasticsearch-6.4.0]$ 
```

### 9，停用某个分区` swapoff 分区名`