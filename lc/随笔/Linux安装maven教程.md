- Linux安装maven教程
	- 1，下载maven，在Linux里面使用命令
	`wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz`
	或者[点击这里下载
	![](http://on.rongyipiao.com//link/take/lib/php/../uploads/zXvHpqA0.png)](http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz "点击这里下载")
	- 2，解压Maven，使用命令
	`tar -zxvf apache-maven-3.6.0-bin.tar.gz`
	![](http://on.rongyipiao.com//link/take/lib/php/../uploads/pet2WfxU.png)
	- 3，设置环境变量，使用vim编辑器打开/etc/profile文件,加入如下命令
```
export MAVEN_HOME=/root/maven/apache-maven-3.6.0
export PATH=$PATH:$MAVEN_HOME/bin
```
	![](http://on.rongyipiao.com//link/take/lib/php/../uploads/WMbEsHxe.png)
	- 4， 检查是否安装完成，输入 `mvn -v`, 如果显示了如下信息就安装完成了。
	![](http://on.rongyipiao.com//link/take/lib/php/../uploads/caDiSM8E.png)