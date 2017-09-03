
##LAMP搭建

	卸载php7.0 安装 php5.6
	sudo aptitude purge `dpkg -l | grep php| awk '{print $2}' |tr "\n" " "`
	查询卸载已安装PHP	
	------------------------------------------------
	sudo apt-add-repository ppa:ondrej/php
	ubuntu16.04默认是php 7，要装php 5.x要添加第三方源：
	sudo apt-add-repository ppa:ondrej/php
	sudo apt-get update
	sudo apt-get install php5.6
	
##清除MySQL相关
	
	$ sudo apt-get remove --purge mysql*
	$ sudo rm -rvf /var/lib/mysql
	$ sudo apt-get autoremove
	$ sudo apt-get autoclean
	2.重启Ubuntu服务器
	
	$ sudo reboot
	3.重新安装MySQL
	
	$ sudo apt-get install mysql-server5.6

##Mysql重启
	sudo /etc/init.d/mysql restart | start | stop


##搭建LAMP服务器
	
	---------安装apache---------
	sudo apt install apache2

	---------安装PHP-----------
	sudo apt install php7.0-fpm
	sudo apt-get install php5.6

	------安装mysql-------------
	sudo apt install mysql-server5.6

##安装必要的php模块
	sudo apt install php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-gd php7.0-curl

##Apache解析PHP
	sudo apt install libapache2-mod-php7.0

##Apache 重启
	sudo service apache2 restart | start | stop



##Ubuntu下卸载PHP7
	sudo apt-get --purge remove php7.0*
	(或者 sudo apt-get --purge remove php5* libapache2-mod-php5)
	sudo apt-get autoremove php7.0*(php5)






##Ubuntu MYSQL操作
	1.添加用户
		-- all : insert select delete update 
		-- *.* : 所有库，所有表
		-- %   : 任意地方登录
		grant select,insert,update on 库.*（所有表） to 用户名@'localhost'（主机） IDENTIFIED by '123456'（密码）;
		grant all on test.* to test@'%' IDENTIFIED by 'test123456';
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




-------------------------------------------------------------------------------
-------------------------------------------------------------------------------


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




##ubuntu 配置虚拟主机

	vim /etc/apache2/apache2.conf
	cd /etc/apache2/sites-available/  	 （配置域名）
	

	一、打开：\etc\hosts文件：sudo vi \etc\hosts 
	添加：127.0.0.1 linuxidc.com
	
	二、打开： \etc\apache2\sites-available\000-default 
	在头部添加：NameVirtualHost *:80
	
	<VirtualHost 114.55.9.103:80>
	ServerName cy.quanzhuan.net
	ServerAdmin webmaster@cy.quanzhuan.net
	DocumentRoot /var/www/html/
	<Directory /var/www/html>
	    Options Indexes FollowSymLinks MultiViews
	    AllowOverride None
	    Order allow,deny
	    allow from all
	</Directory>
	ErrorLog /var/www/logs/error.log
	CustomLog /var/www/logs/access.log combined
	</VirtualHost>
	
	---------------配置---------------
	vim /etc/hosts
		
	添加：127.0.0.1 linuxidc.com


	重启service apache2 restart


oninput="if(value.length>5)value=value.slice(0,5)"



##安装PHP扩展

一、安装php5.6-xml 扩展

   2.1 安装php7.0-xml。

      在ubuntu中可以使用"sudo apt-get install php7.0-xml"进行安装。

   2.2 重启apache服务。

           在ubuntu中可以使用“sudo service apache2 restart”重启apache服务。



##如何查看端口 ubuntu 14.04
	
	netstat -tln 命令是用来查看linux的端口使用情况
	
	Proto Recv-Q Send-Q Local Address           Foreign Address         State      
	tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN     
	tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN     
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
	tcp6       0      0 :::22                   :::*                    LISTEN 

	查看某个端口：netstat -tln | grep 3306

## ubuntu 14.04
	apache配置文件：
	/alidata/server/httpd-2.4.9/conf