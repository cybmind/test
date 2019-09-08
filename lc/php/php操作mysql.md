### PDO （PHP DATA OBJECT）
| 方法  |描述   |
| ------------ | ------------ |
| exec方法  | 执行一条sql语句，并返回受影响的行数  |
|  query方法 |  执行一条sql语句，并返回一个PDOStatement对象 |
| prepare方法 |  准备要执行的sql语句，并返回一个PDOStatement对象 |
|  quote方法 |  返回一个添加引号的字符串，用于sql中 |
|  lastInsertId |  返回最后插入行的id |
| setAttribute |  设置数据库链接属性 |
|  getAttribute |  获取数据库链接属性 |
|  getAttribute |  获取数据库链接属性 |
|  errorCode |  获取错误码 |
|  errorInfo |  获取错误详细信息 |
|  rowCount |  返回查询结果数量，或者受影响的记录条数 |

php对数据库操作的一个封装，解决了不同数据库需要调用的api不同，和切换数据库的时候的复杂度。
- exec():方法
	 执行一条sql语句，返回受影响的行数（select除外），适用于增删改
	 ```php
	 <?php
	/**
	 * Created by PhpStorm.
	 * User: liaocheng
	 * Date: 2019-01-31
	 * Time: 15:48
	 */
	// exec 执行一个sql语句（select语句除外）返回受影响的行数
	require_once "connectDB.php";
	$pdo = connectDataBases();
	$sql = 'insert into user (id,name,age,password) values (8, "yinya", 15,"'.md5(123456).'")';
	echo $pdo->exec($sql);
	 ```
- query():方法
	 执行一个查询语句
	 ```
	 <?php
/**
 * Created by PhpStorm.
 * User: liaocheng
 * Date: 2019-02-04
 * Time: 22:09
 */
try{
    $DNS = "mysql:host=localhost;dbname=myapp";
    $USERNAME = "root";
    $PASSWORD = "root";
    $con = new PDO($DNS, $USERNAME, $PASSWORD);
    $sql = "select * from user";

    // 执行query方法，返回PDStatement对象
    $result = $con->query($sql);
    var_dump($result);

    foreach ($result as $row) {
        echo "id".$row['name'];
    }
}catch (PDOException $exception) {
    echo $exception->getMessage();
}
	 ```
- prepare():方法
	 将sql放入预处理里面，使用excute来执行预处理语句
- ***mysqli_connect*** 链接数据库
``` php
	//  在php7.0已经不推荐使用mysql_connect链接数据库了，推荐使用mysqli_connect来链接数据库。如果链接成功会返回链接的相关信息，如果链接失败会返回false；
	$conne = @mysqli_connect("localhost","root","root","myapp","6379","");
	if ($conne) {
		//  使用query来查询一个语句。
		$result = $conne->query("select * from user ");
		// 使用fetch_array();
		$mysqlResult = $result->fetch_assoc();
		print_r(json_encode($mysqlResult));
	} else {
		echo "链接数据库失败！";
	}
```