- 执行maven命令时产生的错误
	- Plugin org.apache.maven.plugins:maven-clean-plugin:2.6.1 or one of its dependencies could not be resolved: Failed to read artifact descriptor for org.apache.maven.plugins:maven-clean-plugin:jar:2.6.1: Could not transfer artifact org.apache.maven.plugins:maven-clean-plugin:pom:2.6.1 from/to central (https://repo.maven.apache.org/maven2): Received fatal alert: protocol_version
![](http://on.rongyipiao.com//link/take/lib/php/../uploads/vRZuX1mB.png)
		- 原因：其实就是中央仓库必须要TLS1.2版本才能访问。
		- 解决方案：更新jdk为1.8

