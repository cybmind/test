input-jdbc 参数详解
```
input{
    jdbc{
    #jdbc sql server 驱动,各个数据库都有对应的驱动，需自己下载
    jdbc_driver_library => "/etc/logstash/driver.d/sqljdbc_2.0/enu/sqljdbc4.jar"
    #jdbc class 不同数据库有不同的 class 配置
    jdbc_driver_class => "com.microsoft.sqlserver.jdbc.SQLServerDriver"
    #配置数据库连接 ip 和端口，以及数据库   
    jdbc_connection_string => "jdbc:mysql:/localhost:3306;databaseName=test_db"
    #配置数据库用户名
    jdbc_user =>   
    #配置数据库密码
    jdbc_password =>
    #上面这些都不重要，要是这些都看不懂的话，你的老板估计要考虑换人了。重要的是接下来的内容。
    # 定时器 多久执行一次SQL，默认是一分钟
    # schedule => 分 时 天 月 年  
    # schedule =>  22     表示每天22点执行一次
    schedule => "  *"
    #是否清除 last_run_metadata_path 的记录,如果为真那么每次都相当于从头开始查询所有的数据库记录
    clean_run => false
    #是否需要记录某个column 的值,如果 record_last_run 为真,可以自定义我们需要表的字段名称，
    #此时该参数就要为 true. 否则默认 track 的是 timestamp 的值.
    use_column_value => true
    #如果 use_column_value 为真,需配置此参数. 这个参数就是数据库给出的一个字段名称。当然该字段必须是递增的，可以是 数据库的数据时间这类的
    tracking_column => create_time
    #是否记录上次执行结果, 如果为真,将会把上次执行到的 tracking_column 字段的值记录下来,保存到 last_run_metadata_path 指定的文件中
    record_last_run => true
    #们只需要在 SQL 语句中 WHERE MY_ID > :last_sql_value 即可. 其中 :sql_last_value 取得就是该文件中的值
    last_run_metadata_path => "/etc/logstash/run_metadata.d/my_info"
    #是否将字段名称转小写。
    #这里有个小的提示，如果你这前就处理过一次数据，并且在Kibana中有对应的搜索需求的话，还是改为true，
    #因为默认是true，并且Kibana是大小写区分的。准确的说应该是ES大小写区分
    lowercase_column_names => false
    #你的SQL的位置，当然，你的SQL也可以直接写在这里。
    #statement => SELECT * FROM tabeName t WHERE  t.creat_time > :sql_last_value
statement_filepath => "/etc/logstash/statement_file.d/my_info.sql" #数据类型，标明你属于那一方势力。单了ES哪里好给你安排不同的山头。 type => "my_info" } #注意：外载的SQL文件就是一个文本文件就可以了，还有需要注意的是，一个jdbc{}插件就只能处理一个SQL语句， #如果你有多个SQL需要处理的话，只能在重新建立一个jdbc{}插件。 }
```

例子：
```

输入配置
input {
        jdbc {
                jdbc_driver_library => "/pkg/elk/logstash-6.6.1/mysql-connector-java-5.1.46/mysql-connector-java-5.1.46-bin.jar"
                jdbc_driver_class => "com.mysql.jdbc.Driver"
                jdbc_connection_string => "jdbc:mysql://0.0.0.0:3306/myapp"
                jdbc_user => "root"
                jdbc_password => "root"
                schedule => "* * * * *"
                statement => "select * from user"
                use_column_value => true
                tracking_column_type => "timestamp"
                tracking_column => "update_time"
                last_run_metadata_path => "syncpoint_table"

}
}

输出到Es 配置
output {
  elasticsearch {
    hosts => ["192.168.136.138"]
    index => "myapp"
    document_id => "%{id}"
  }
}
```