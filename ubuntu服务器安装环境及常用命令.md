
##linux常用命令
	1、查看安装的所有软件
		dpkg -l | grep ftp
	2、查看软件安装的路径
		dpkg -L | grep ftp
	3、查看软件版本
		aptitude show ftp
	
	4、复制：
		cp 文件1 文件2	可以写路径
		注意复制目录要加入递归参数， 即：cp -r 目录1 目录2

	5、删除文件（移除或删除文件，删除目录需要添加递归参数 -r）
		rm text.txt
	
	6、删除目录： 这个命令用来删除空目录，若要删除不是空目录，请用	rm -r 代替
		rmdir 空目录	
	
	7、创建目录
		mkdir 目录名称
	
	8、帮助
		man
	
	9、显示文件系统中还有多少剩余空间（这个命令显示所有已挂载设备的空间使用量）
		df (默认的是用千兆节K表示)		df -h (用兆字节M和千兆字节G来显示设备空间的使用量)
		![](http://i.imgur.com/Jgm7k41.png)

	10、显示目录中文件或目录的具体大小
		du -sh 文件名称/目录、
	
	11、显示内存使用情况，默认K表示，若要M表示，即free -m
			free	
			total:去掉为硬件和操作系统保留的内存后剩余的内存总量。许多人奇怪自己的电脑安装了一共8G的内存，但是显示总共只有七点几G的，现在应该没什么疑惑了把，不管Linux还是Windows都会有部分内存是保留给硬件和操作系统的！
			userd：当前已使用的内存总量。
			free：空闲的或可以使用的内存总量
			shared：共享内存大小，主要用于进程间通信
			buff(buffers):主要用于块设备数据缓冲，例如记录文件系统的metadata（目录、权限等等信息）。
			cache:主要用于文件内容缓冲
			available:可以使用的内存总量
	
	12、uname -a（显示所有的系统信息）: 输出系统所有信息，包括主机名，内核名字及版本,硬件信息等等。




##FTP 权限管理
	第一步：安装VSFTPD
		sudo apt-get install vsftpd
		安装完成后启动VSFTPD服务：
		service vsftpd start
	
	第二步：新建目录/home/uftp作为用户主目录
		sudo mkdir /home/uftp
	
	第三步：新建用户uftp，制定用户主目录和所用shell，并设置密码
		sudo useradd -d /home/uftp -s /bin/bash uftp
		![](http://i.imgur.com/4P56ySN.png)
		
		修改密码：
			sudo passwd uftp 		输入两次密码
		
		然后将目录/home/uftp的所属者和所属组都改为uftp：
			sudo chown uftp:uftp /home/uftp

	第四步：新建文件/etc/vsftpd.user_list，用于存放允许访问ftp的用户：
		sudo vi /etc/vsftpd.user_list
			uftp

	第五步：编辑VSFTPD配置文件 
		VSFTPD配置文件为/etc/vsftpd.conf，执行命令：
		sudo vi /etc/vsftpd.conf

		做如下修改： 
	　　打开注释 write_enable=YES 
	　　添加信息 userlist_file=/etc/vsftpd.user_list 
	　　添加信息 userlist_enable=YES 
	　　添加信息 userlist_deny=NO 
	　　修改完成后保存退出。

	第六步：重启vsftd服务
		service vsftpd restart
	

##FTP用户，相应的权限
	1、usermod -s /sbin/nologin test   #限定用户test不能telnet，只能ftp  
	2、usermod -s /bin/bash test   #用户test恢复正常  
	3、usermod -d /home/test test      #更改用户test的主目录为/test  
	
	
	4、限制用户只能访问/home/test，不能访问其他路径:
	
		修改/etc/vsftpd/vsftpd.conf
		chroot_list_enable=YES  #限制访问自身目录  
		chroot_list_file=/etc/vsftpd/vsftpd.chroot_list 

		编辑 vsftpd.chroot_list文件，将受限制的用户添加进去，每个用户名一行
		改完配置文件，不要忘记重启vsftpd服务器

	5、如果需要允许用户修改密码，但是又没有telnet登录系统的权限：
		usermod -s /usr/bin/passwd test      #用户telnet后将直接进入改密界面

	6、如果要删除用户，用下面代码		
		在root用户下：  
		userdel -r newuser 
		
		在普通用户下：  
		sudo userdel -r newuser  



	7、修改账户密码
		[sudo] passwd pwftp

	8、修改指定目录的权限
		
		chown -R pwftp(用户名).pwftp（用户名） /alidata/www/wwwroot（目录绝对路径）

	四.卸载
	sudo apt-get remove --purge vsftpd


##解决Ubuntu提示500 OOPS: vsftpd: refusing to run with writable root inside chroot()
	1.https://www.zhukun.net/archives/7654

	2.Ubuntu 12.04 64bit的完整解决方案
		$ apt-get install python-software-properties
		$ sudo add-apt-repository ppa:thefrontiergroup/vsftpd
		$ sudo apt-get update
		$ sudo apt-get install vsftpd
		$ vim /etc/vsftpd.conf and add the following
		  chroot_local_user=YES
		  allow_writeable_chroot=YES
		$ sudo service vsftpd restart



##Linux系统可以通过sshd的配置项，禁止某些用户sftp登陆
	1、打开sshd的配置文件
		vi /etc/ssh/sshd_config
	2、修改该配置文件，增加或修改
		# 禁止用户user1登陆，多个用户空格分隔
		DenyUsers user1
		# 禁止用户组group1的所有用户登录，多个空格分隔
		DenyGroups group

	3、保存配置后，重启sshd

	/etc/rc.d/init.d/sshd restart
	#完成上面的配置后，就可以禁止用户或用户组的用户进行SFTP登录




##服务器
	1.重启apache2
		sudo /etc/init.d/apache2 restart
		vi /etc/apache2/apache2.conf
	2.重启mysql
		sudo /etc/init.d/mysql restart

	3.ubuntu 用apt-get 安装apache 和php 之后php不能解析的问题
		sudo apt-get install apache2
		sudo apt-get install php7.0
		sudo apt-get install libapache2-mod-php //关键是这一步安装apache的php扩展
	4.ubuntu安装phpmyadmin
		sudo apt-get install phpmyadmin
		安装完成后，去服务器目录下检查，发现并没有phpmyadmin，这样的文件或者文件夹

		系统在安装软件时，默认将软件安装在了/usr/share/下，所以你的phpmyadmin在/usr/share下可以找到
		咱们必须建立一个软连接，使得第三步中显示的文件和/var/www/html下的某个文档链接起来，回到/var/www/html，输入一下代码
		sudo ln -s /usr/share/phpmyadmin phpmyadmin

	5.Ubuntu下卸载PHP7
		sudo apt-get --purge remove php7.0*
		(或者 sudo apt-get --purge remove php5* libapache2-mod-php5)
		sudo apt-get autoremove php7.0*(php5)
		
		如有还有php-fpm进程在跑，可以使用pkill命令结束进程



	查询卸载已安装PHP
	sudo aptitude purge dpkg -l | grep php| awk '{print $2}' |tr "\n" " "
	2.通过ppa安装
	sudo add-apt-repository ppa:ondrej/php
	
	3.
	sudo apt-get update
	sudo apt-get install php5.6或者sudo apt-get install php5.6-fpm php5.6-cli php5.6-mcrypt
	
	4.配置PHP
	sudo vim /etc/php/5.6/fpm/php.ini
	
	打开PHP配置文件，找到cgi.fix_pathinfo选项，去掉它前面的注释分号;，然后将它的值设置为0,如下
	cgi.fix_pathinfo=0
	
	5.启用php5-mcrypt:
	
	sudo phpenmod mcrypt
	
	6.重启php5-fpm:
	
	sudo service php5.6-fpm restart











##一、卸载删除 mysql
 
	1 sudo apt-get autoremove --purge mysql-server-5.0
	2 sudo apt-get remove mysql-server
	3 sudo apt-get autoremove mysql-server
	4 sudo apt-get remove mysql-common (非常重要)
	 
	上面的其实有一些是多余的，建议还是按照顺序执行一遍
	 
	清理残留数据：
	 
	dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
	sudo find /etc -name "*mysql*" |xargs  rm -rf 
	 
	最后用 dpkg -l | grep mysql 检查，如无返回即干净卸载


##二、卸载删除apache
	sudo apt-get --purge remove apache-common
	sudo apt-get --purge remove apache
	 
	找到没有删除掉的配置文件，一并删除
	 
	sudo find /etc -name "*apache*" |xargs  rm -rf 
	sudo rm -rf /var/www
	sudo rm -rf /etc/libapache2-mod-jk
	sudo rm -rf /etc/init.d/apache2
	sudo rm -rf /etc/apache2
	 
	删除关联，
	 
	dpkg -l |grep apache2|awk '{print $2}'|xargs dpkg -P
	 
	删除svn
	sudo apt-get remove subversion
	sudo apt-get remove libapache2-svn
	 
	最后用 dpkg -l | grep apache 和 dpkg -l | grep apache2检查，如无返回即干净卸载

##三、卸载删除php
	sudo apt-get –purge remove libapache2-mod-php5 php5 php5-gd php5-mysql
	sudo apt-get autoremove php5
	 
	删除关联，
	sudo find /etc -name "*php*" |xargs  rm -rf 
	 
	清楚残留信息
	dpkg -l |grep ^rc|awk ’{print $2}’ |sudo xargs dpkg -P
	 
	最后用 dpkg -l | grep php 和dpkg -l | grep php5 检查，如无返回即干净卸载



| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

ABCD | EFGH | IGKL
-----|------|----
a    | b    | c
d    | e    | f
g    | h    | i


##Ubuntu MYSQL操作
	//配置文件
	sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

	1.添加用户
		-- all : insert select delete update 
		-- *.* : 所有库，所有表
		-- %   : 任意地方登录
		grant select,insert,update on 库.*（所有表） to 用户名@'localhost'（主机） IDENTIFIED by '123456'（密码）;
	
	2.修改用户密码
		update `user` set `ysql_native_password` = password('123456') where user = 'bairen';
		-- 修改完用户密码后，请刷新
		flush privileges;

		ubuntu下:update mysql.user set authentication_string=password('123456') where user='bairen';

	3.删除用户
		delete from `user` where `user` = 'bairen';


	4.让普通用户看不到information_schema库
		INFORMATION_SCHEMA是信息数据库，其中保存着关于MySQL服务器所维护的所有其他数据库的信息。在INFORMATION_SCHEMA中，有数个只读表。它们实际上是视图，而不是基本表，因此，你将无法看到与之相关的任何文件。
		每位MySQL用户均有权访问这些表，但仅限于表中的特定行，在这类行中含有用户具有恰当访问权限的对象。

		在 /etc/phpmyadmin/config.inc.php 加上
		$cfg['Servers'][$i]['hide_db'] = 'information_schema';