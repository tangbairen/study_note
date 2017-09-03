##FTP 安装
	1.which vsftpd #检测是否已经安装vsftpd
	2.sudo apt-get install vsftpd	安装
	3.sudo service vsftpd start #开启ftp服务
	4.service vsftpd status #查看FTP的状态
	5.sudo service vsftp stop #停止ftp服务
	6.sudo service vsftp restart #重启ftp服务
	7.sudo /etc/init.d/vsftpd restart #倘若上面的不行就使用路径的形式直接执行  
	8.sudo pkill vsftpd #有时候停止失败就干掉吧
	9.查看软件安装的路径
    	dpkg -L | grep ftp
##卸载FTP
	sudo apt-get remove --purge vsftpd   有依赖的，都删了
	sudo apt-get remove vsftpd

##FTP 权限管理
	新建目录/home/uftp作为用户主目录
	如：sudo mkdir /home/share
	
	增加share用户     sudo useradd -d /home/share share
	为用户添加密码       sudo passwd share
	删除share用户     sudo userdel share
	更改用户的权限       sudo usermod -s /sbin/nologin share  #用户share不能telnet 只能FTP

	然后将目录/home/uftp的所属者和所属组都改为uftp：
        sudo chown uftp:uftp /home/uftp


##解决Ubuntu提示500 OOPS: vsftpd: refusing to run with writable root inside chroot()
	1.https://www.zhukun.net/archives/7654

	2.Ubuntu 12.04 64bit的完整解决方案
		$ apt-get install python-software-properties
		$ sudo add-apt-repository ppa:thefrontiergroup/vsftpd
		$ sudo apt-get update
		$ sudo apt-get install vsftpd
		$ vim /etc/vsftpd.conf and add the following
		  chroot_local_user=YES
		  allow_writeable_chroot=YES  （加多一句）
		$ sudo service vsftpd restart



##Linux系统可以通过sshd的配置项，禁止某些用户sftp登陆
	1、打开sshd的配置文件
		vi /etc/ssh/sshd_config
	2、修改该配置文件，增加或修改
		# 禁止用户user1登陆，多个用户空格分隔
		DenyUsers user1
		# 禁止用户组group1的所有用户登录，多个空格分隔
		DenyGroups group

	3、保存配置后，重启sshd2

	/etc/rc.d/init.d/sshd restart
	service sshd restart
	#完成上面的配置后，就可以禁止用户或用户组的用户进行SFTP登录



##遇到的问题: 登陆的时候一直出现530 Login incorrect
	可以重新来安装
	sudo apt-get remove vsftpd
	sudo rm /etc/pam.d/vsftpd
	sudo apt-get install vsftpd 


isten_port=23456，然后保存，重启ftp服务就可以



	