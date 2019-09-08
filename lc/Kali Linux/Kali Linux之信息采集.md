## 环境
Kali Linux
## 介绍
渗透测试最重要的阶段之一就是信息采集，为了启动渗透测试，用户需要收集目标主机的基本信息。用户得到的信息越多，渗透成功的几率也就越高，Kali Linux上面也提供了一些工具，可以帮组用户整理和搜集目标主机信息，使用户得到更好的后期侦查。
## 枚举服务
枚举是一类程序，它允许用户从网络中搜集某一类的所有相关信息，本文将介绍基于`DNS`和`SNMP`的枚举服务信息收集，DNS可以帮助用户获取目标主机的相关信息，如用户名，计算机名和IP地址等，为了获取这些信息，可以使用`DNSenum`工具, 要晋西`SNMP`枚举，可以使用`SNMPemun`,`SnmpEnum`是一个强大的SNMP枚举工具，它允许用户分析一个网络内的`SNMP`信息传输

### DNS枚举工具DNSenum
DNSenum是一个非常强大的域名信息搜集工具，它能够通过谷歌或者字典文件猜测可能存在的域名，并对一个网段进行反向查询，它不仅可以查询网站的主机地址信息，域名服务器和邮件交换记录，还可以在域名服务器上面执行axfr请求，然后通过谷歌脚本得到扩展域名信息，提取子域名并查询，最后c类地址执行whois查询，执行烦心啊个查询，将地址段写入文件。
在终端下执行该命令`dnsenum --enum baidu.com`
```
root@kali:~# dnsenum --enum badui.com
Smartmatch is experimental at /usr/bin/dnsenum line 698.
Smartmatch is experimental at /usr/bin/dnsenum line 698.
dnsenum VERSION:1.2.4
Warning: can't load Net::Whois::IP module, whois queries disabled.
Warning: can't load WWW::Mechanize module, Google scraping desabled.

-----   badui.com   -----


Host's addresses:
__________________

badui.com.                               5        IN    A        67.23.129.4


Name Servers:
______________

ns2.netfirms.com.                        5        IN    A        66.96.142.145
ns1.netfirms.com.                        5        IN    A        65.254.254.157


Mail (MX) Servers:
___________________

q1.netfirms.com.                         5        IN    A        67.23.128.57
q0.netfirms.com.                         5        IN    A        67.23.128.56


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for badui.com on ns1.netfirms.com ... 
AXFR record query failed: NOTAUTH

Trying Zone Transfer for badui.com on ns2.netfirms.com ... 
AXFR record query failed: NOTAUTH

brute force file not specified, bay.
root@kali:~#
```
输出的信息显示了DNS服务的详细信息，其中包括目标主机地址，域名地址，邮件服务地址。

### DNS枚举工具fierce
fierce和DNSenum差不多，fierce是要是扫描和搜集目标的子域名信息，使用方式如下
执行命令
```
fierce -dns baidu.com
```
响应信息
![](http://uninote.com.cn/docs/1079089832/__pic/Sm3Q2yOK.png)
可以从上图看到一些baidu.com的子域名信息，由于篇幅原因就先停止扫描

### SNMP枚举工具SNMPWALK
snmpwalk是一个应用程序，它使用snmp的GETNEXT请求，可以用来搜集主机的所有信息。

举例：列出目标主机所有的信息。比如列出是否安装了git
执行命令
```
snmpwalk -c 192.168.31.63 -v 1|grep git
```
响应结果
![](http://uninote.com.cn/docs/1079089832/__pic/4UUago4T.png)
可以看到这个主机是安装了git的，而且还能获取安装位置
