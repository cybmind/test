## 介绍
### Kali Linux 的由来
想要知道Kali Linux的由来那么就必须知道什么是渗透测试
**渗透测试**：渗透测试并没有一个标准的定义，国外一个安全租住达成共识的通用说法是，渗透测试是模拟恶意黑客的攻击方法，来评估计算机网络安全的一种评估方法，这个过程包括对系统的任何弱点，技术缺陷进行分析。这个分析是从一个攻击者可能存在的位置进行的，并且从这个位置有条件利用安全漏洞。

渗透测试有黑盒和白盒两种渗透测试方法，黑盒就是在不知道主机的任何信息的情况下，模拟黑客的攻击方式来对系统进行漏洞的检查，白盒就是已经掌握了要攻击的这个系统的任何信息，然后在之情的情况下进行攻击。

渗透测试所需的工具在流行的Linux里面都能找到，为了方便用户进行渗透方面的工作，所以有人就将这些渗透工具给统一的封装到了一个系统里面那就是Kali Linux。

该系统主要用户渗透测试，它预装了很多的渗透测试软件，包活nmap端口扫描器，WireShark（数据包分析器），John the Ripper（密码破解）及Aircrack-ng（一套对无线局域网进行渗透测试的软件）

###### 关注我：后续带来更多Kali Linux的教程
## 在Mac 版 VMware里面安装Kali Linux
这里只是举例在mac上面如何安装，其实在windows上面安装也差不多
1. 下载Kali Linux
种子链接:https://pan.baidu.com/s/1yq4gA2o_iIRGwvIFBRcmNQ  密码:qaeu
镜像链接:https://pan.baidu.com/s/1GnM_2lhy1vycq3_0wrT8Qw  密码:yegs

2. 打开mac的vmware
	1. 点击左上角的文件->新建
	
	![](http://uninote.com.cn/docs/1079089832/__pic/C0AXvwyC.png)
	
	2. 出现下面这个界面，然后选择从光盘或镜映像安装,点击继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/9MENKi7J.png)
	
	3. 点击继续过后将出现如下界面
	
	![](http://uninote.com.cn/docs/1079089832/__pic/msmiWRuh.png)
	
	4. 然后点击使用其他光盘或光盘映像，选择你刚才下载的镜像然后导入进来
	
	![](http://uninote.com.cn/docs/1079089832/__pic/aPzFWvtW.png)
	![](http://uninote.com.cn/docs/1079089832/__pic/cleLo9b3.png)
	
	5. 点击继续，将出现如下让你选择操作系统的界面
	
	![](http://uninote.com.cn/docs/1079089832/__pic/8tQtJj2E.png)
	
	6. 选择`Debina 7.x 64位`操作系统，然后点击继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/WFSdoxWG.png)
	
	7. 悬着传统BIOS, 点击继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/H0EiVIem.png)
	
	8. 然后点击完成
	
	![](http://uninote.com.cn/docs/1079089832/__pic/czLL8B74.png)
	
	9. 在等待一会儿过后会出现如下选择，选择里面的 Craphical install, 然后回车
	
	![](http://uninote.com.cn/docs/1079089832/__pic/9XojjZsm.png)
	
	10. 选择你想用的语言，然后点击continue
	
	![](http://uninote.com.cn/docs/1079089832/__pic/hMQn13pP.png)
	
	11. 选择你的地区，然后继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/wIBR2K79.png)
	
	12. 配置键盘，然后继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/70sIUxzf.png)
	
	13. 等待自动配置就行了
	
	![](http://uninote.com.cn/docs/1079089832/__pic/sezUQ7Tg.png)
	
	14. 配置网络，主机名随便输，可以是你自己的名字
	
	![](http://uninote.com.cn/docs/1079089832/__pic/OORoxKeb.png)
	
	15. 配置域名，这个可以填0.0.0.0
	
	![](http://uninote.com.cn/docs/1079089832/__pic/OGGTzBaM.png)
	
	16. 设置root密码,然后等待后续的任务完成
	
	![](http://uninote.com.cn/docs/1079089832/__pic/kSVPbqff.png)
	
	17. 磁盘分区，选择第一个 使用整个磁盘
	
	![](http://uninote.com.cn/docs/1079089832/__pic/rkH22UNa.png)
	
	18. 点击继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/hIasJV97.png)
	
	19. 选择第一个
	
	![](http://uninote.com.cn/docs/1079089832/__pic/9yNPgPMj.png)
	
	20. 直接点击继续
	
	![](http://uninote.com.cn/docs/1079089832/__pic/yIIUArL7.png)
	
	21. 选择是, 然后等待后续系统自动安装
	
	![](http://uninote.com.cn/docs/1079089832/__pic/wYrVL2Tc.png)
	
	22. 配置软件包管理器, 选择是
	
	![](http://uninote.com.cn/docs/1079089832/__pic/I8gi2rsj.png)
	
	23. 可以不填， 然后点击继续，等待系统自动安装
	
	![](http://uninote.com.cn/docs/1079089832/__pic/pfX83s8C.png)
	
	24. 选择是
	
	![](http://uninote.com.cn/docs/1079089832/__pic/jwsMuXmC.png)
	
	25. 选择/dev/sda, 然后等待结束安装
	
	![](http://uninote.com.cn/docs/1079089832/__pic/GWAhXH3a.png)
	
	26. 安装完成，点击继续，等待系统自动运行
	
	![](http://uninote.com.cn/docs/1079089832/__pic/FKLDxeyh.png)
	
	27. 进入系统，输入用户名为root
	
	![](http://uninote.com.cn/docs/1079089832/__pic/S3b3Bwtd.png)
	
	28. 输入密码，你刚才设置的密码，点击登陆，进入系统
	
	![](http://uninote.com.cn/docs/1079089832/__pic/TRMQ5t7A.png)
	
	29. 完成