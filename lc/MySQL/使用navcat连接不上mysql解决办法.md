#### 解决Navicat 报错:1130-host is not allowed MySQL不允许从远程访问的方法

#### 1，进入mysql数据库执行如下命令即可访问
```
1、mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION //赋予任何主机访问数据的权限
2、mysql>FLUSH PRIVILEGES //修改生效
3、mysql>EXIT //退出MySQL服务器
```