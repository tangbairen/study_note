##Thinkphp5 基础部分

一、Thinkphp5 模块设计
	
	5.0版本对模块的功能做了灵活设计，默认采用多模块的架构，并且支持单一模块设计，所有模块的
	命名空间均以app作为根命名空间（可配置更改）（基本不需要改变它）
二、命名规范
	
	模块：
		模块在application目录下新建对应的目录：（以小写的形式命名）
			index\ 模块目录
				controller\ 控制器目录
					Index.php   控制器类名（大驼峰写法）
				model\ 模型目录
				view\ 视图目录
					
	类名：大驼峰写法，如Index.php,后面可以用下划线命名
	命名空间：
		namespace app\admin\controller;  app\模块名\控制器
		use app\common\controller\Index as commonIndex 
			引用	模块 某个控制 取个别名
			new commonIndex();
		也可直接继承该类：
			class User extends commonInde{}
							
	通用的共用模块无法直接通过url地址栏访问！
	
	可以修改application应用目录，在public/index.php 入口文件


三、Thinkphp5 配置

	1.惯例配置（默认配置）（基本不会改变惯例配置）
		config();这个函数，就是查询所有的配置
		在thinkphp目录下的convention.php文件下

	2.应用配置（修改，设置 对应的配置）
		在根目录下，新建一个conf目录下
			新建一个config.php
			<?php
				return [
					'app_email'=>'168168@qq.com',
					'app_debug'=>'true/false'
				
				];
			?>	
		
	3.扩展配置
		在conf目录下新建扩展目录 extra目录
			新建database.php文件里面都是return返回一个数组形式
		
		配置数据库也可以在conf目录下，新建一个database.php文件，return数组形式
	
			
	4、场景配置
		例如：本地测试数据库环境，或线上的配置文件
		在config.php文件中，加上  'app_status'='home'
		在conf目录下，新建home.php文件，
		return [
			'database'=>[对应数据库配置所有参数都要重写]
		];
	
	
	5.模块配置
		可以在conf 目录下，新建一个目录，index/  下，在建立一个config.php配置文件
		return 返回一个数组形式，它所对应的是 application 应用目录下的 index 目录模块

	6.动态配置
		在controller 控制器，方法中设置配置
		public function index(){
			config('before','befornAction');
		}


##四、Thinkphp5  config类和config助手函数
	Config 类在thinkphp/library/ 下
在控制器中使用config类，如：

	<?php
		namespace app\index\controller;
		use think\Config;
		class  Index{
			public function index(){
				$res=\think\Config::get(); //获取所有的配置  (最好使用原始类)
				config();  这个以防其他人改动了，还是使用原来的好（一样的效果）
								
				//获取配置文件的值
				Config::get('app_namespace'); //一种方式
				config('app_namespach'); //另一种方式

				//设置值
				Config::set('username','bairen');
				config('username','bairen');

				//设置作用域
				Config::set('username','bairen','index');
				Config::get('username','index');

				//判断配置是否设置
				Config::has('username'); //bool
				config('?username');

				dump($res);
	
			}


		}

		
##五、环境变量配置和使用  $_ENV 环境变量
在根目录下新建一个文件为  .env 文件

	可以写：		
		email=16565656@qq.com
		[database]
		hostname=localhost;
$_ENV 环境变量的使用

	dump($_ENV); 使用这个的话，读取某个值，要写前缀，如：$_ENV['PHP_EMAIL'];
	
	如果是内置Env 这个类
		use think\Env;
		Env::get('email'); //不用前缀
		Env::get('email','default');//如果不存在，返回default这个值

		Env::get('database_hostname');
		Env::get('database.hostname');

可以利用Env 类来配置，开发环境，测试环境，保持其他地方相同，改变 .env 文件就可以了
又利用环境的部署

##六、Thinkphp5 入口文件
	安全检测，过滤
##入口文件绑定
	在index.php 入口文件中定义：
		define('BIND_MODULE','admin'); //默认绑定到 admin 模块
		还可以指定到某个控制器

	 还可以配置开启自动绑定 ，在config.php配置文件中 ，定义
			
		
##Thinkphp5 路由
查看config.php 配置文件中，是否开启路由

	'url_route_on' =>true

配置路由，在conf配置目录下，新建一个route.php文件（固定名称）
	![](http://i.imgur.com/VK1CkLC.png)
访问地址：localhost/new/id/5.html,指定到index/index/info 方法

url（）助手函数，可以输出当前方法的地址

	url('index/index/info',['id'=>5]);

##Thinkphp5 请求对象获取 Request
thinkphp5 有三种方式获取到request对象
![](http://i.imgur.com/c12tj69.png)

##Thinkphp5 请求对象参数获取
	public function index(Request $request){
		$request->domain();//获取当前的域名
		$request->pathinfo();//获取当前url的路径，模块
		$request->path();//获取当前url，不含	后缀名称
		
		$request->method();//返回当前是什么请求方式

		$request->isGet();//是否GET请求
		$request->isPost();//是否POST请求 （跟Yii2.0 差不多）
		$request->isAjax();//是否ajax请求	
		
		//请求参数
		$request->get();//获取GET请求的参数
		
		$request->param();//获取
		
		$request->session();//获取session中的值
	}
	
![](http://i.imgur.com/C3b2LDB.png)

session中的应用
	
	如果要使用session,要在配置文件中，开启
	$request->session();
	也可以使用session（）助手函数
	session('name','bairen');

cookie()中的应用
	$request->cookie();
	cookie();助手函数

获取当前模块
	$request->module();

获取当前控制器：$request->controller();

获取当前操作（方法）：$request->action();

##Thinkphp5 助手函数input
input()函数可以获取，get,post,param,session, put, file,path 等等 的值

	input('get.id');//获取get请求的参数
	input('post.id');//获取post请求的参数
	还可以指定默认值，当值为null,不存在时
	input('get.id',100);

	还可以传递类型
	input('get.id',100,'intval');

	获取session 值
	input('session.email');

请求对象中，都是传递类型，来过滤接受的参数

##Thinkphp5 响应对象Response
Thinkphp5中，不建议die(),exit(); 最好用return返回

如果要返回json格式：
	动态配置：输出类型

		Config::set('default_return_type','json');
		return $res;//这就是json格式了
	
api接口编写，例如
![](http://i.imgur.com/uHTFSSV.jpg)

##Thinkphp5 视图view

view()助手函数

	public function index(){
		//默认模板地址
		//app/index/view/index/index.html
		return view();
	}
![](http://i.imgur.com/jT7kRcW.png)

还可以 继承controller 控制器,如：
	
	<?php
	namespace app\index\controller;
	
	use think\controller;	
	
	class Index extends controller
	{
		public function index()
		{
			$this->assign('assign','assign传递的值');//分配变量，到前台展示
			// 输出模板，传递变量，替换值
			return $this->fetch('index',
				['email'=>'11111@qq.com','user'=>'bairen'],
				['STATIC'=>'当前是static替换内容']);
		}
	}

![](http://i.imgur.com/OtVa4Xa.png)
![](http://i.imgur.com/PUZDEof.png)
	
配置变量替换，可以在模板中直接输出，可用于配置css,js 中的路径
![](http://i.imgur.com/PP3p9Go.png)

##Thinkphp5 模板中使用系统变量，原生标签
 使用系统变量
![](http://i.imgur.com/VDInhk9.jpg)

##Thinkphp5 模板标签中使用
![](http://i.imgur.com/h3Rvk0Y.jpg)

![](http://i.imgur.com/EBpWRlf.jpg)

##Thinkphp5 模板循环标签
![](http://i.imgur.com/8zELGhF.jpg)

![](http://i.imgur.com/yRf9Rwc.jpg)

##Thinkphp5 模板布局 包含 继承
include引用方式

![](http://i.imgur.com/seE5sc4.jpg)


extend 继承方式
![](http://i.imgur.com/Sdu9Gw7.jpg)

使用block
![](http://i.imgur.com/s4DrmFI.jpg)

![](http://i.imgur.com/lklXOV7.jpg)

在继承中，使用默认footer标签

![](http://i.imgur.com/gKtNqEo.jpg)

##layout布局，全局布局
在config配置中，开启

![](http://i.imgur.com/wQFMxcT.jpg)

在模板中写对应的代码就可以了
![](http://i.imgur.com/yxuLMnc.jpg)

可以使用include 引用，但是不能使用extend继承，layout布局！

