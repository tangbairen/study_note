##Cent OS6.3 安装Apache  ,PHP 5.6.30, MySQL5.6.36
安装Apache 2.4.6
	
	1. Apache 安装 yum -y install httpd

Apache 服务
	
	1.Apache 服务 启动   service httpd start
	2.Apache 服务 重启 	service httpd restart
	3.Apache 服务 查看状态	service httpd status 
	4.Apache 服务 查看版本	httpd status
	
例如要开机后自动启动mysql,apache,vsftpd服务，用以下命令即可：

	chkconfig mysqld on
	chkconfig httpd on
	chkconfig vsftpd on

等级代码为：linux系统的运行级别。linux 将操作 环境分为以下7个等级，即

	0：关机
	1：单用户模式（单用户、无网络）
	2：无网络支持的多用户模式（多用户、无网络）
	3：有网络支持的多用户模式（多用户、有网络）
	4：保留，未使用
	5：有网络支持有X-Window支持的多用户模式（多用户、有网络、X-Window界面）
	6：重新引导系统，即重启
	先用chkconfig list查询apache和mysql服务是否存在，不存在则需要手动添加。
	添加apache服务项命令：
	chkconfig -add httpd
	添加完设置启动项：
	chkconfig --level 2345 httpd on
	chkconfig --level 2345 mysqld on

##Mysql 卸载

yum方式安装的mysql
	
	1、yum remove mysql mysql-server mysql-libs compat-mysql51
	
	2、rm -rf /var/lib/mysql
	
	3、rm /etc/my.cnf
	
	查看是否还有mysql软件：
	rpm -qa|grep mysql
	
	如果存在的话，继续删除即可


rpm方式安装的mysql
	
	a）查看系统中是否以rpm包安装的mysql：
	
	[root@localhost opt]# rpm -qa | grep -i mysql
	MySQL-server-5.6.17-1.el6.i686
	MySQL-client-5.6.17-1.el6.i686
	
	b)卸载mysql
	
	[root@localhost local]# rpm -e MySQL-server-5.6.17-1.el6.i686
	[root@localhost local]# rpm -e MySQL-client-5.6.17-1.el6.i686
	
	c)删除mysql服务
	
	[root@localhost local]# chkconfig --list | grep -i mysql
	[root@localhost local]# chkconfig --del mysql
	
	d)删除分散mysql文件夹
	
	[root@localhost local]# whereis mysql 或者 find / -name mysql
	
	mysql: /usr/lib/mysql /usr/share/mysql
	
	清空相关mysql的所有目录以及文件
	rm -rf /usr/lib/mysql
	rm -rf /usr/share/mysql
	
	rm -rf /usr/my.cnf
	
	通过以上几步，mysql应该已经完全卸载干净了

##Mysql 源码包
http://www.2cto.com/database/201611/560706.html


##linux 系统下载网址
http://www.linuxdown.net/download/

##Linux.中国-开源社区
https://linux.cn/

##Linux公社 - Linux系统门户网站

http://www.linuxidc.com/