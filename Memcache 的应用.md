##Memcache 内存对象缓存系统

1.windows下安装memcache 

	安装到服务进程中：
		1.将服务端文件放到指定位置
		2.通过命令进入指定位置
		3.执行命令
			-d :　安装到进程
			memcached.exe -d install
		4.查看
			cmd --> services.msc 


	卸载：
		1.将服务端文件放到指定位置
		2.通过命令进入指定位置
		3.执行命令
			-d :　安装到进程
			memcached.exe -d uninstall
		4.查看
			cmd --> services.msc 

	启动命令：
		memcached.exe -d start
	停止命令：
		memcached.exe -d stop

memcached 是神马？

	高性能的，分布式的内存对象缓存系统，用于在动态应用中减少数据库负载，提升访问速度!

	memcached
		可以存储任何数据

	memcached 
		hash 地址，通过你给的参数，动态计算出来 
		C/S 结构，必须有客户端，服务器！


	特点：
		1.在内存中缓存数据
		2.数据形态以key->value结构
		3.安全性非常差的（无须用户名）


	telnet 不是内部命令：
		开启：控制面板 --> 卸载程序 --> 打开关闭 window 功能
				把Telnet客户端开启

	黑窗口链接：
		telnet 127.0.0.1 11211

2.在黑窗口，命令行操作memcache
	
	命令格式：
		<命令> <键> <标记> <有效期> <数据长度>
		描述：
			<键>		获取数据的键
			<标记> 		0，不压缩，2，压缩
			<有效期>	缓存时间，单位是秒
			<数据长度>	要准确

	// add 命令不能覆盖
	add name 0 0 5

	// set 会覆盖
	set name 0 0 4
	
	// 2.删除数据
	delete name

	// 加减操作
	incr n 2  // +2

	// 追加数据(后入)
	append n 0 0 2

	// 追加数据(前入)
	prepend n 0 0 3

	// 清空所有数据
	flush_all

	// 获取数据
	get name

	// 获取多个数据
	gets n m 

	// 查看所有键的步骤
		1. stats items // 查看序号
		2. stats cachedump 1 0 // 1:表示序号 , 0 表示查看所有

	重点记住：
		set , get 
	注意：
		memcache 是存储在内存，内存是易失性存储器，关闭或者断电，数据即销毁！


##PHP安装memcache 扩展以及基本操作

1、 检查 D:\wamp\bin\php\php5.5.12\ext 扩展库中是否存在 php_memcache.dll ，如果没有去下载对应PHP版本的memcache

2、将扩展文件添加到 D:\wamp\bin\php\php5.5.12\ext 中

3、去php.ini 开启	扩展 extension=php_memcache.dll


##PHP使用memcache

	<?php
	
	// 1.实例化
	$me = new Memcache();
	// 2.连接
	$me->connect('127.0.0.1',11211);

	// 3.设置或者获取
	// $me->set('list',$list , 0 , 10);

	// 获取
	$res = $me->get('list');
	// 判断思路：先看看缓存是否存在
	if(!$res){
		echo '从数据库中获取数据<br>';
		// 从数据库中获取了数据
		$list = [
			['name'=>'lili1'],
			['name'=>'lili2'],
			['name'=>'lili3'],
		];

		$res = $list;
	}

	echo '<pre>';
		print_r($res);
	echo '</pre>';

	// 4.关闭
	$me->close();








