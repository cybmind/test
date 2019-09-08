- 创建controller `php think make:controller index/Article --plain`
- 创建model `php think make:model Users`
- 无法通过浏览器访问控制器的原因：
http://www.shop.com/?s=add 需要这样访问才行
https://blog.csdn.net/qq_24364529/article/details/80224521  这篇文章找到的答案

- 跳转视图并且传递参数
![](http://uninote.com.cn/docs/1079089832/__pic/sXaQyyL5.png)

- 查询
	- 查询所有数据: `类名::select()`
![](http://uninote.com.cn/docs/1079089832/__pic/0Cso4u3S.png)

	- 根据id查询数据 `类名::find()`
![](http://uninote.com.cn/docs/1079089832/__pic/gCvBcx8Y.png)

	- 根据指定条件查询数据
![](http://uninote.com.cn/docs/1079089832/__pic/PwFA8oit.png)

	- 根据多个条件查询
![](http://uninote.com.cn/docs/1079089832/__pic/YFnaEXet.png)

	- 通过Db类的sql语句查询
![](http://uninote.com.cn/docs/1079089832/__pic/y8PlUwDU.png)

- 数据验证
![](http://uninote.com.cn/docs/1079089832/__pic/V8BHuYZ2.png)
