## windows下的环境搭建配置redis
网址链接：http://blog.csdn.net/spring21st/article/details/11176723

下载地址：https://github.com/dmajkic/redis/downloads 下载下来的包里有两个

一个是32位的，一个是64位的。根据自己的实情情况选择，我的是32bit，

把这个文件夹复制到其它地方，比如D：\redis 目录下。

	1.打开一个cmd窗口  使用cd命令切换目录到d:\redis  运行 redis-server.exe
	2.如果想方便的话，可以把redis的路径加到系统的环境变量里，这样就省得再输路径了，
		后面的那个redis.conf可以省略，如果省略，会启用默认的
	3.这时候另启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了
	4.切换到redis目录下运行 redis-cli.exe -h 127.0.0.1 -p 6379 



##Yii 安装redis扩展
	1.通过composer进行安装，到项目根目录cmd运行（推荐）
			php composer.phar require --prefer-dist yiisoft/yii2-redis
			下载扩展网址：https://github.com/yiisoft/yii2-redis/archive/master.zip
	2.或者添加
		"yiisoft/yii2-redis": "~2.0.0" 到对应项目的composer.json文件中

	3.手动安装
		把下载的扩展文件放到vendor/yiisoft/下，命名为yii2-redis
	
修改vender/yiisoft/下的extensions.php，加入redis扩展

	'yiisoft/yii2-redis' =>
    array (
        'name' => 'yiisoft/yii2-redis',
        'version' => '2.2.0.0',
        'alias' =>
        array (
            '@yii/redis' => $vendorDir . '/yiisoft/yii2-redis',
        ),
    ),


配置Yii的component   (config/main.php)

	'redis' => [
        'class' => 'yii\redis\Connection',
        'hostname' => 'localhost',
        'port' => 6379,
        'database' => 0,
	],

这样我们的redis就配置完成了，接下来就是验证了

	public function actionIndex()
	{   
	    Yii::$app->redis->set('test','111');  //设置redis缓存
	    echo Yii::$app->redis->get('test');   //读取redis缓存
	    exit;
	    return $this->render('index');
	}




##linux下的环境搭建redis
要在 Ubuntu 上安装 Redis，打开终端，然后输入以下命令：

	$sudo apt-get update
	$sudo apt-get install redis-server

这将在您的计算机上安装Redis
	启动 Redis

	$redis-server

这将打开一个 Redis 提示符，如下图所示：

	redis 127.0.0.1:6379>

在上面的提示信息中：127.0.0.1 是本机的IP地址，6379是 Redis 服务器运行的端口。现在输入 PING 命令，如下图所示：

	redis 127.0.0.1:6379> ping
	PONG

这说明现在你已经成功地在计算机上安装了 Redis。