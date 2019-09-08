#### MySql作为一个关系型数据库，现在市场上大部分公司都是采用该数据库，那么这里就介绍一下Linux CentOS 6.9 如何快速安装一个MySQL， 安装不上，提头来见😳。

#### 第一步：准备一台Centos 6.9的系统，咱们这里使用的是VMware来安装的CentOS 6.9,这里连接就先不介绍了。
#### 第二步：使用 `yum -y install mysql-server` 这个命令来进行安装， -y参数表示所有需要用户确认的地方全部选y
```
[root@localhost logstash-6.6.1]# yum -y install mysql-server
已加载插件：fastestmirror, refresh-packagekit, security
设置安装进程
Loading mirror speeds from cached hostfile
 * base: centos.ustc.edu.cn
 * extras: centos.ustc.edu.cn
 * updates: centos.ustc.edu.cn
解决依赖关系
--> 执行事务检查
---> Package mysql-server.x86_64 0:5.1.73-8.el6_8 will be 安装
--> 处理依赖关系 mysql = 5.1.73-8.el6_8，它被软件包 mysql-server-5.1.73-8.el6_8.x86_64 需要
--> 处理依赖关系 perl-DBI，它被软件包 mysql-server-5.1.73-8.el6_8.x86_64 需要
--> 处理依赖关系 perl-DBD-MySQL，它被软件包 mysql-server-5.1.73-8.el6_8.x86_64 需要
--> 处理依赖关系 perl(DBI)，它被软件包 mysql-server-5.1.73-8.el6_8.x86_64 需要
--> 执行事务检查
---> Package mysql.x86_64 0:5.1.73-8.el6_8 will be 安装
---> Package perl-DBD-MySQL.x86_64 0:4.013-3.el6 will be 安装
---> Package perl-DBI.x86_64 0:1.609-4.el6 will be 安装
--> 完成依赖关系计算

依赖关系解决

============================================================================================================================================
 软件包                               架构                         版本                                    仓库                        大小
============================================================================================================================================
正在安装:
 mysql-server                         x86_64                       5.1.73-8.el6_8                          base                       8.6 M
为依赖而安装:
 mysql                                x86_64                       5.1.73-8.el6_8                          base                       895 k
 perl-DBD-MySQL                       x86_64                       4.013-3.el6                             base                       134 k
 perl-DBI                             x86_64                       1.609-4.el6                             base                       705 k

事务概要
============================================================================================================================================
Install       4 Package(s)

总下载量：10 M
Installed size: 29 M
下载软件包：
(1/4): mysql-5.1.73-8.el6_8.x86_64.rpm                                                                               | 895 kB     00:01     
(2/4): mysql-server-5.1.73-8.el6_8.x86_64.rpm                                                                        | 8.6 MB     00:02     
(3/4): perl-DBD-MySQL-4.013-3.el6.x86_64.rpm                                                                         | 134 kB     00:00     
(4/4): perl-DBI-1.609-4.el6.x86_64.rpm                                                                               | 705 kB     00:00     
--------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                        2.3 MB/s |  10 MB     00:04     
warning: rpmts_HdrFromFdno: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
Importing GPG key 0xC105B9DE:
 Userid : CentOS-6 Key (CentOS 6 Official Signing Key) <centos-6-key@centos.org>
 Package: centos-release-6-9.el6.12.3.x86_64 (@anaconda-CentOS-201703281317.x86_64/6.9)
 From   : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
运行 rpm_check_debug 
执行事务测试
事务测试成功
执行事务
  正在安装   : perl-DBI-1.609-4.el6.x86_64                                                                                              1/4 
  正在安装   : perl-DBD-MySQL-4.013-3.el6.x86_64                                                                                        2/4 
  正在安装   : mysql-5.1.73-8.el6_8.x86_64                                                                                              3/4 
  正在安装   : mysql-server-5.1.73-8.el6_8.x86_64                                                                                       4/4 
  Verifying  : perl-DBD-MySQL-4.013-3.el6.x86_64                                                                                        1/4 
  Verifying  : mysql-server-5.1.73-8.el6_8.x86_64                                                                                       2/4 
  Verifying  : mysql-5.1.73-8.el6_8.x86_64                                                                                              3/4 
  Verifying  : perl-DBI-1.609-4.el6.x86_64                                                                                              4/4 

已安装:
  mysql-server.x86_64 0:5.1.73-8.el6_8                                                                                                      

作为依赖被安装:
  mysql.x86_64 0:5.1.73-8.el6_8               perl-DBD-MySQL.x86_64 0:4.013-3.el6               perl-DBI.x86_64 0:1.609-4.el6              

完毕！
[root@localhost logstash-6.6.1]# 
```

#### 第三步：使用 `service mysqld start` 命令来启动mysql
```
[root@localhost logstash-6.6.1]# service mysqld start
Initializing MySQL database:  Installing MySQL system tables...
OK
Filling help tables...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system

PLEASE REMEMBER TO SET A PASSWORD FOR THE MySQL root USER !
To do so, start the server, then issue the following commands:

/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h localhost.localdomain password 'new-password'

Alternatively you can run:
/usr/bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the manual for more instructions.

You can start the MySQL daemon with:
cd /usr ; /usr/bin/mysqld_safe &

You can test the MySQL daemon with mysql-test-run.pl
cd /usr/mysql-test ; perl mysql-test-run.pl

Please report any problems with the /usr/bin/mysqlbug script!

                                                           [  OK  ]
Starting mysqld:                                           [  OK  ]
[root@localhost logstash-6.6.1]# mysql

```

#### 第四步：使用 `mysql` 命令进入数据库
```
[root@localhost logstash-6.6.1]# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.1.73 Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

#### 第五步：修改mysql密码, 先使用`use mysql`进入mysql这个库，然后输入 `update user set password=password('root') where user='root';` 来更改数据库密码。
```
[root@localhost logstash-6.6.1]# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.1.73 Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set password=password('root') where user='root';
Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql>\q 
输入\q 可以退出
```
#### 第六步：可以直接使用`mysql -uroot -p` 然后输入你刚才数据的密码就可以进入数据库了。
```
[root@localhost logstash-6.6.1]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.1.73 Source distribution

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> \q
Bye
[root@localhost logstash-6.6.1]# 

```

