## 介绍
由于ElasticSearch本身没有权限管理模块，只要获取服务器的地址和端口，任何人都可以随意读写ElasticSearch的API并获取数据,这样非常不安全。所以本文就介绍一下如何规避这种问题，实现方式其实有几种，本文将介绍使用nginx的basic_auth来控制的方式。
- 好处
使用nginx的好处是轻便，不用涉及到Elasticsearch 或者 Kibana本身的配置上，
- 坏处
可被暴力破解

## 环境
Centos 6.9
Elasticsearch 版本 6.4.0
Kibana 版本 6.4.0
Nginx 版本 1.11.6

## 安装nginx
1. 下载nginx
```
[root@localhost ~]# wget http://nginx.org/download/nginx-1.11.6.tar.gz
```

2. 解压ngxin
```
tar -zxvf nginx-1.11.6.tar.gz 
```

3. 进入nginx目录
```
cd nginx-1.11.6
```

4. 执行configure检查配置
```
./configure
```
	4.1. 发现错误, 缺少 pcre 包
	```
	./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
	```

5. 安装pcre包
```
yum -y install pcre-devel
```

6. 再次执行configure检查配置
```
./configure
```
	6.1 发现还缺少一个zlib的包
	```
	./configure: error: the HTTP gzip module requires the zlib library.
You can either disable the module by using --without-http_gzip_module
option, or install the zlib library into the system, or build the zlib library
statically from the source with nginx by using --with-zlib=<path> option.
	```

7. 安装zlib包
```
 yum -y install zlib-devel
```

8. 执行configure
```
./configure
```

9. 执行make install
```
make install
```

10. 进入nginx目录启动nginx
```
cd  /usr/local/nginx/
```
启动nginx
```
./sbin/ngxin
```
可以进入你的浏览器输入你的ip地址看看是否能访问

## 配置ngxin
下面就开始对ngxin的配置文件做修改，修改/conf目录下面的nginx.conf
```
vim /conf/nginx.conf
```
将里面的
```
location / {
            root   html;
            index  index.html index.htm;
        }
```
改成
```
location / {
            proxy_pass  http://0.0.0.0:5601;
            auth_basic "登陆验证";
            auth_basic_user_file /usr/local/nginx/htpasswd;
        }
```
然后使用htpasswd命令生成密码文件
```
[root@test102 conf.d]# htpasswd -cm /usr/local/nginx/htpasswd kibana     #/usr/local/nginx/htpasswd就是配置文件里面配置的密码文件，kibana就是用户名
New password:     #输入密码
Re-type new password:     #再次输入密码，回车
Adding password for user crystal
```
进入浏览器访问你的ip，会让你输入用户名和密码才能进入kibana了

![](http://uninote.com.cn/docs/1079089832/__pic/Gcu6LFoy.png)

输入用户名和密码

![](http://uninote.com.cn/docs/1079089832/__pic/4MQfwwtE.png)

点击登陆就进入了kibana

![](http://uninote.com.cn/docs/1079089832/__pic/qjRrYyb6.png)