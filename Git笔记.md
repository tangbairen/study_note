##Git常用笔记
##Git学习网站 
	https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

git clone 
	
	 获取一个url对应的远程Git repo, 创建一个local copy.
 	一般的格式是git clone [url].

git status

	 查询repo的状态.

git add .

	会递归地添加当前工作目录中的所有文件

git commit
	
	 提交已经被add进来的改动.
 	 git commit -m “the commit message"

git push

	提交到远程分支
	git push origin master

git pull
	
	git pull origin master获取远程分支


##使用git pull文件时和本地文件冲突怎么办？

在使用git pull代码时，经常会碰到有冲突的情况，提示如下信息：
	
	error: Your local changes to 'c/environ.c' would be overwritten by merge.  Aborting.
	Please, commit your changes or stash them before you can merge.

这个意思是说更新下来的内容和本地修改的内容有冲突，先提交你的改变或者先将本地修改暂时存储起来。

处理的方式非常简单，主要是使用git stash命令进行处理，分成以下几个步骤进行处理

1、先将本地修改存储起来
		$ git stash
	这样本地的所有修改就都被暂时存储起来 。是用git stash list可以看到保存的信息：

		git stash暂存修改
		git stash暂存修改
	其中stash@{0}就是刚才保存的标记。

2、pull内容

	暂存了本地修改之后，就可以pull了。
	
	$ git pull

3、还原暂存的内容

	$ git stash pop stash@{0}

系统提示如下类似的信息：

	Auto-merging c/environ.c
	CONFLICT (content): Merge conflict in c/environ.c

意思就是系统自动合并修改的内容，但是其中有冲突，需要解决其中的冲突。
	
4、解决文件中冲突的的部分

	打开冲突的文件，会看到类似如下的内容：

	其中Updated upstream 和=====之间的内容就是pull下来的内容，====和stashed changes之间的内容就是本地修改的内容。碰到这种情况，git也不知道哪行内容是需要的，所以要自行确定需要的内容。
	解决完成之后，就可以正常的提交了。


##Git安装

在linux下安装Git 
	
	可以输入 git , 看看系统有没有安装Git
	
	The program 'git' is currently not installed. You can install it by typing:
安装命令
	
	yum -y install git 

安装完成后，还需要最后一步设置，在命令行输入：

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"

	因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和
	Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，
	首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

	注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的
	Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

##创建版本库

第一步，选择一个合适的地方，创建一个空目录

	mkdir test
	cd test 
	pwd 
	/home/git/test

第二步，通过git init 命令把这个目录变成Git可以管理的仓库
	
	[root@localhost test]# git init
	Initialized empty Git repository in /home/git/test/.git/

	瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，
	这个目录
	是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

	如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。
	