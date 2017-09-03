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

##SVN 相关命令操作

![](http://i.imgur.com/hSksECJ.jpg)

![](http://i.imgur.com/onNqBPO.jpg)


##版本库配置

![](http://i.imgur.com/kvhhlpE.jpg)


##SVN 更新 发现冲突

	[root@localhost demo1]# svn up
	在 “index.php” 中发现冲突。
	选择: (p) 推迟，(df) 显示全部差异，(e) 编辑,
	        (mc) 我的版本, (tc) 他人的版本,
	        (s) 显示全部选项: 





