## 介绍
本文译至《网络是怎样连接的》提供读者参考。
[书籍链接](http://docs.uninote.com.cn/result/network.html "原文链接")
http://docs.uninote.com.cn/books/networkswork/index.html#[49,%22XYZ%22,0,612,null]
## 第一章 浏览器生成消息--探索浏览器内部
- [1.1 生成http请求消息](http://docs.uninote.com.cn/books/networkswork/index.html#[30,%22XYZ%22,0,612,null] "1.1 生成http请求消息")
	- [1.1.1 探索之旅从输入网址开始](http://docs.uninote.com.cn/books/networkswork/index.html#[30,%22XYZ%22,0,612,null])
	- [1.1.2 浏览器先要解析 URL](http://docs.uninote.com.cn/books/networkswork/index.html#[32,%22XYZ%22,0,612,null] "1.1.2 浏览器先要解析 URL")
	- [1.1.3 省略文件名的情况](http://docs.uninote.com.cn/books/networkswork/index.html#[34,%22XYZ%22,0,612,null] "1.1.3 省略文件名的情况")
	- [1.1.4 HTTP 的基本思路](http://docs.uninote.com.cn/books/networkswork/index.html#[35,%22XYZ%22,0,612,null] "1.1.4 http的基本思路")
	- [1.1.5 生成HTTP 请求消息](http://docs.uninote.com.cn/books/networkswork/index.html#[39,%22XYZ%22,0,612,null] "生成HTTP 请求消息")
	- [1.1.6 发送消息后会收到响应](http://docs.uninote.com.cn/books/networkswork/index.html#[45,%22XYZ%22,0,612,null] "1.1.6 发送消息后会收到响应")

- [1.2 向 DNS 服务器查询 Web 服务器的 IP 地址](http://docs.uninote.com.cn/books/networkswork/index.html#[49,%22XYZ%22,0,612,null] "1.2 向 DNS 服务器查询 Web 服务器的 IP 地址")
	- [1.2.1 IP 地址的基本知识](http://docs.uninote.com.cn/books/networkswork/index.html#[49,%22XYZ%22,0,612,null] "1.2.1 IP 地址的基本知识")
	- [1.2.2 域名和 IP 地址并用的理由](http://docs.uninote.com.cn/books/networkswork/index.html#[53,%22XYZ%22,0,612,null] "1.2.2 域名和 IP 地址并用的理由")
	- [1.2.3 Socket 库提供查询 IP 地址的功能](http://docs.uninote.com.cn/books/networkswork/index.html#[55,%22XYZ%22,0,612,null] "1.2.3 Socket 库提供查询 IP 地址的功能")
	- [1.2.4 通过解析器向 DNS 服务器发出查询](http://docs.uninote.com.cn/books/networkswork/index.html#[56,%22XYZ%22,0,612,null] "1.2.4 通过解析器向 DNS 服务器发出查询")
	- [1.2.5 解析器的内部原理](http://docs.uninote.com.cn/books/networkswork/index.html#[57,%22XYZ%22,0,612,null] "1.2.5 解析器的内部原理")

- [1.3  全世界 DNS 服务器的大接力](http://docs.uninote.com.cn/books/networkswork/index.html#[60,%22XYZ%22,0,612,null] "1.3  全世界 DNS 服务器的大接力")