- yii框架使用composer创建项目
		composer create-project --prefer-dist yiisoft/yii2-app-advanced  yii23
		创建时会等一会儿，需要下载相关的依赖包。
- composer切换国内镜像源
		composer config -g repo.packagist composer https://packagist.phpcomposer.com
- yii框架创建controller文件需要注意的地方
		 yii框架创建controller必须以controller结尾。比如 IndexController
		 这个IndexController又影响到了view层，如果想调用view下面的模版必须创建一个Index包，然后将代码放到这个index包里面。
		代码如下---------------------------------------------------------------------------------------------------------------------：
		namespace app\controllers;
		// 这里必须引入yii\web\Controller文件
		use yii\web\Controller;
		class IndexController extends Controller{
		}
- yii框架在controller里面创建一个路由控制器需要注意的地方
		 yii框架创建路由控制器必须使用action开头。比如 actionIndex
		 代码如下--------------------------------------------------------------------------------------------------------------------：
		namespace app\controllers;
		use yii\web\Controller;
		class IndexController extends Controller{
		
			public function actionIndex() {
			}
			
		}
-  yii框架如何在路由控制器里面调用views里面的模版
		 可以使用$this->render("模版名称");方法来调用,想要将结果返回到页面必须使用return。
		 代码如下---------------------------------------------------------------------------------------------------------------------：
		namespace app\controllers;
		use yii\web\Controller;
		class IndexController extends Controller{
		
			public function actionIndex() {
			
				return $this->render("index");
				
			}
			
		}
- yii框架去除默认的头尾样式
		 方法一：使用 $this->layout=false;
		代码如下---------------------------------------------------------------------------------------------------------------------：
		 public function actionIndex() {
				$this->layout=false;
				return $this->render("index");
			}
			
		方法二：使用 $this->renderPartial("index");
		代码如下---------------------------------------------------------------------------------------------------------------------：
			public function actionIndex() {
				return $this->renderPartial("index");
			}

- yii 框架layouts布局的使用
		首先需要在views里面的layouts里面创建一个php文件，然后将需要布局的头或者尾放到里面，中间写上
		<?php echo $content; ?>
		代码如下---------------------------------------------------------------------------------------------------------------------：
		    <header>...
			<?php echo $content;?>
			<footer>...
		使用layout布局，需要在controller里面指明要使用哪一个layout。使用 $this->layout = "上面layouts目录里面的那个文件";
		代码如下---------------------------------------------------------------------------------------------------------------------：
		namespace app\controllers;
		use yii\web\Controller;

		class IndexController extends Controller{
			public $layout = "layout1";
			public function actionIndex() {
				return $this->render("index");
			}
		}
- yii 开启gii模块，相当于模仿了一个MVC模型，将他们都放在了同一个目录下面。
		想要使用gii模块，需要到config目录下面的web.php里面修改配置文件。
		if (YII_ENV_DEV) {
			// configuration adjustments for 'dev' environment
			$config['bootstrap'][] = 'debug';
			$config['modules']['debug'] = [
				'class' => 'yii\debug\Module',
				// uncomment the following to add your IP if you are not connecting from localhost.
				//'allowedIPs' => ['127.0.0.1', '::1'],
			];

			$config['bootstrap'][] = 'gii';
			$config['modules']['gii'] = [
				'class' => 'yii\gii\Module',
				// uncomment the following to add your IP if you are not connecting from localhost.
				// 这里需要打开，默认是关闭的。也就是设置访问人ip限制
				'allowedIPs' => ['127.0.0.1', '::1'],
			];
		}
		前往http://localhost/index.php?r=gii 创建一个modules，并且在config目录下的web.php里面设置如下内容。
		在73行添加；
		
			// 开启modles访问
			$config['modules']['admin'] = [
				'class' => 'app\modules\admin',
			];
		添加完成就可以前往http://lcoalhost/index.php?r=admin/default/index 看到内容了。

- yii 框架设置默认控制器，也就是打开index.php定位的页面
		 默认控制器是通过/vendor/yiisoft/yii2/web/Application.php里面的public $defaultRoute = 'site'; 来设置的
		 但是不推荐直接修改源码，可以通过修改配置文件的方式来修改。
		 在config目录下面的web.php里面的config里面添加同级属性
		 'defaultRoute' => 'index', 这里的index是指向的是views里面的index目录，这个目录下面有index.php文件，比如这里写的是'defaultRoute' => 'admin'，那么这里的admin指向的就是views里面的admin文件夹，那么views里面的admin目录下面就必须又一个index.php文件。
- yii 框架的controller传递参数到views里面，也就是通过render调用的时候传递参数过去
		 下面展示的是传一个变量名为model的参数到login页面去。login页面可以直接通过$model来获取值。
		代码如下
		class PublicController extends Controller {
			public function actionLogin() {
				$this->layout = false;
				// 这里的第二个参数接受的是一个数组，不管是什么样的数据都可以发放到这个数组里面。
				return $this->render("login", ['model' => 'value',]);
			}
		}

- yii 获取get请求的请求参数
		class HelloController extends  Controller{

			public function actionIndex() {

				// 通过yii获取get请求参数
				$request = \YII::$app->request;
				// 这里就是传递的id值，如果没有传，可以给第二个参数，这个参数的值就是id的值
				echo $request->get("id");
			}

		}

- yii 判断是否是get请求
		 isGet
		 
		 if ($request->isGet)

- yii 转义javascript代码
		可以使用一个Html类里面的encode方法来转义
		Html::encode("代码");
- yii rules常用规则
		public function rules()
		{
			return array(
				//必须填写
				array('email, username, password,agree,verifyPassword,verifyCode', 'required'),
				//检查用户名是否重复
				array('email','unique','message'=>'用户名已占用'),
				//用户输入最大的字符限制
				array('email, username', 'length', 'max'=>64),
				//限制用户最小长度和最大长度
				array('username', 'length', 'max'=>7, 'min'=>2, 'tooLong'=>'用户名请输入长度为4-14个字符', 'tooShort'=>'用户名请输入长度为2-7个字'),
				//限制密码最小长度和最大长度
				array('password', 'length', 'max'=>22, 'min'=>6, 'tooLong'=>'密码请输入长度为6-22位字符', 'tooShort'=>'密码请输入长度为6-22位字符'),
				//判断用户输入的是否是邮件
				array('email','email','message'=>'邮箱格式错误'),
				//检查用户输入的密码是否是一样的
				array('verifyPassword', 'compare', 'compareAttribute'=>'password', 'message'=>'请再输入确认密码'),
				//检查用户是否同意协议条款
				array('agree', 'required', 'requiredValue'=>true,'message'=>'请确认是否同意隐私权协议条款'),
				//判断是否是日期格式
				array('created', 'date', 'format'=>'yyyy/MM/dd/ HH:mm:ss'),
				//判断是否包含输入的字符
				array('superuser', 'in', 'range' => array(0, 1)),
				//正则验证器：       
				array('name','match','pattern'=>'/^[a-z0-9\-_]+$/'),
				//数字验证器：              
				array('id', 'numerical', 'min'=>1, 'max'=>10, 'integerOnly'=>true),
				//类型验证 integer,float,string,array,date,time,datetime                
				array('created', 'type', 'datetime'),
				//文件验证：       
				array('filename', 'file', 'allowEmpty'=>true, 'types'=>'zip, rar, xls, pdf, ppt','tooLarge'=>'图片不要超过800K'),
					  array('url', 
						'file',    //定义为file类型 
						'allowEmpty'=>true,  
						'types'=>'jpg,png,gif,doc,docx,pdf,xls,xlsx,zip,rar,ppt,pptx',   //上传文件的类型 
						'maxSize'=>1024*1024*10,    //上传大小限制，注意不是php.ini中的上传文件大小 
						'tooLarge'=>'文件大于10M，上传失败！请上传小于10M的文件！' 
					), 
		 } );
