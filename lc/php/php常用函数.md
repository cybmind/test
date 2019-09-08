- `isset`: 检测变量是否设置
	如果变量不存在则返回FALSE
	如果变量存在且其值为NULL，也返回FALSE
	如果变量存在且值不为NULL，则返回TURE
	如果同时检查多个变量时，必须要每个变量不为空时才返回TRUE,否则结果为FALSE
	```php
	// 检查一个文件是否为空　
	if (isset($_FILES["file"])) {
		echo "不为空　";
	}else   {
		echo "为空";
	}
	```

- `empty`:检查一个变量是否为空
	 如果为空返回true,如果不为空返回false;
	 若变量存在且值为""、0、"0"、NULL、、FALSE、array()、var $var;以及没有任何属性的对象，则返回TURE
	 若变量存在且值不为""、0、"0"、NULL、、FALSE、array()、var $var;以及没有任何属性的对象，则返回FALSE
	 ```php
	 <?php
	/**
	 * Created by PhpStorm.
	 * User: liaocheng
	 * Date: 2019-01-30
	 * Time: 16:07
	 */
	$i = 0;
	if (empty($i)) {
		echo "空";
	}else{
		echo "非空";
	}
	//  最终输出结果为空
	 ```

- header:使用header加上location可以实现页面跳转
	```php
	// 跳转到index.php并且携带参数name=lc
	header("Location:index.php?name=lc");
	```

- cookie:cookie是存在浏览器本地的，可以用来做不同页面的数据交互
	```php
	// 设置cookie
	setcookie("name","value");
	
	// 获取cookie
	$)_COOKIE["cookieName"]
	```

- session:session是存在服务器端的，可以用来做不同页面的数据交互。
```php
<?php
// 启动session
session_start();
// 获取sessionid
echo session_id();
// session设置值
$_SESSION["name"] = "liaocheng";
// session获取值
$_SESSION["name"]
// 销毁session
session_destroy();
// 销毁指定session值
unset($_SESSION["name"]);
// 将session置为空
$_SESSION = array();
```
- class_exists:检查类是否存在
```php
<?php
/**
 * Created by PhpStorm.
 * User: liaocheng
 * Date: 2019-02-04
 * Time: 23:07
 */
if (class_exists("PDO")) {
    echo "存在";
} else {
    echo "不存在";
}
```