## Kali Linux基本设置
1. 启动apache 服务
	使用命令
```
service apache2 start
```
	输入`ip addr` 查看本地ip
	在浏览器里面输入ip查看apache是否启动

2. 启动ssh服务
	使用命令
	```
	service ssh start
	```
	查看22端口是否被监听
	```
	netstat -tpan | grep 22
	```
	![](http://uninote.com.cn/docs/1079089832/__pic/61QtNc9S.png)