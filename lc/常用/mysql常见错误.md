- Mysql常见错误汇总
	- mysql表名大小写问题
		- 原因：Mysql在不同的系统中配置文件的值不同，在windows里面默认是忽略大小写的，但是在Linux里面是不忽略大小写的。
		- 解决方案：设置mysql忽略表名大小写 在***my.conf*** 里面的```mysqld```下面添加```lower_case_table_names=1```
	- Mysql导出数据时的错误 `mysqldump: Got error: 1066: Not unique table/alias: 'account' when using LOCK TABLES`
	
		- 原因：/etc/my.conf里面的 lower_case_table_names=1 让mysql区分了大小写
		- 解决方案：把它注释掉就可以了