##linux下redis安装及常规操作

###下载redis 源码包

	官网地址：https://redis.io
	
	//下载命令
	$ wget http://download.redis.io/releases/redis-4.0.1.tar.gz
	//解压
	$ tar xzf redis-4.0.1.tar.gz
	$ cd redis-4.0.1
	//编译
	$ make

### 复制相应的文件到 /usr/local/ 目录下  （一种方法）

	1.在 /usr/local/redis 建立redis 文件夹
	2.把 redis-cli ，redis-server ,redis.conf 文件复制到redis 目录下

	###设置redis 后端启动
		
		修改redis.conf配置文件
		################################# GENERAL #####################################
	
		# By default Redis does not run as a daemon. Use 'yes' if you need it.
		# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
		daemonize yes    ------ 将no 改为yes 
		
		然后运行,配置生效
	
		./redis-server redis.conf

##另外一种方式安装

	在redis-4.0.1 里面下 执行 make install 
	在实际运行redis 前 ，推荐使用 make test 命令测试redis是否编译正确

	如果在编译后执行了 make install 命令，这些程序会被复制到 /usr/local/bin 目录内

