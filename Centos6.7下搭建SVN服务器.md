##Centos6.7下搭建SVN服务器

1.Centos6.7下搭建SVN服务器

	Subversion是一个自由，开源的版本控制系统。Subversion将文件存放在中心版本库里。这个版本库很像一个普通的文件服务

	器，不同的是，它可以记录每一次文件和目录的修改情况。这样就可以籍此将数据恢复到以前的版本，并可以查看数据的更改细节。

	Subversion是Apache基金会下的一个项目，官网 https://subversion.apache.org/ 。

2.yum 源 安装

	yum -y install subversion

3.创建SVN版本库

	mkdir -p /opt/svn/repos/svn1                      ##创建目录
	svnadmin create /opt/svn/repos/svn1               ##创建SVN版本库

4.配置版本库

	cd /opt/svn/repos/svn1/conf 并且 vim passwd（添加用户）：
	复制代码
	[users]
	# harry = harryssecret
	# sally = sallyssecret
	#
	user1 = 123     用户名 = 密码
	user2 = 123

vim authz（添加权限）：

	[svn1:/]
	user1 = rw
	user2 = rw

	#svn1是具体版本库的标签

vim svnserve.conf(取消一些注释)：

	[general]
	anon-access = none                    #非授权用户无法访问
	auth-access = write                   #授权用户有写权限
	password-db = passwd                  #密码数据所在目录
	authz-db = authz  

5.启动SVN

	svnserve -d -r /opt/svn/repos/    #注意目录，不包含svn1

	ps aux | grep svnserve            #查看服务是否启动

6.测试SVN的服务器

	svn://192.168.1.1/svn1

7.如果想创建多个版本库

	复制代码
	mkdir -p /opt/svn/repos/svn2                      ##创建目录

	svnadmin create /opt/svn/repos/svn2

	重复步骤4的配置方法

	killall svnserve                                  #关闭svn服务
	svnserve -d -r /opt/svn/repos/　　　　　　　　　　　　#启动svn，注意目录，不包含svn2


8.删除版本库

	rm -rf svn2/

 ９．同个ｓｖｎ库下根据不同的权限访问不同的目录

	复制代码
　	[groups]
　	chanpin = user1,user2
　	yanfa = user3,user4

	[svn1:/]
	test = rw
	other = rw
	anyone = rw
	@chanpin = rw
	@yanfa = rw

	[svn1:/chanpin]
	other = rw
	@chanpin = rw
	* =

[svn1:/yanfa]
anyone = rw
@yanfa = rw
* =



# 卸载SVN服务器

检查是否安装了低版本的SVN

[root@Linux /]# rpm -qa subversion

如果存储旧版本，卸载旧版本SVN

[root@Linux modules]# yum remove subversion

安装SVN

[root@Linux modules]# yum install subversion

Linux下 利用find命令删除所有.svn目录

	find / -name svn 


#MySQL 操作
	
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

	11、开放3306端口

	  [root@root ~]#  firewall-cmd --permanent --add-port=3306/tcp
	  [root@root ~]#  firewall-cmd --reload

		如果出现 FirewallD is not running	
		执行：systemctl start firewalld.service 启动防火墙、然后再试

检测svn项目：

	svn checkout svn://192.168.0.3/