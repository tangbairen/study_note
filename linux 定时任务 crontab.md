fileclear.sh

	复制代码
	tamcdir=${HOME}/ora/user_projects/domains/tamc
	cd ${tamcdir}
	echo rm -f `ls heapdump*.phd`
	rm -f heapdump*.phd
	echo rm -f `ls javacore*.txt`
	rm -f javacore*.txt
	echo rm -f `ls Snap*.trc`
	rm -f Snap*.trc
	cd bin
	echo cp /dev/null nuhup.out
	cp /dev/null nuhup.out
	cd ${tamcdir}/pxbak
	echo rm -rf `ls 20*`
	rm -rf 20*
	cd ${tamcdir}/webapps/tamcx/fileLoad
	echo rm -f `find /weblogic/ora/user_projects/domains/tamc/webapps/tamcx/fileLoad/ -mtime +1`
	find /weblogic/ora/user_projects/domains/tamc/webapps/tamcx/fileLoad/ -mtime +1 -exec rm -f {} \;
	复制代码
	task.crontab

#web服务端日志、临时文件清理
	10 1 * * * ksh $HOME/tools/clearweblogic.sh >>/weblogic/ora/user_projects/domains/tamc/webapps/tamcx/log/crontab.log 2>>/weblogic/ora/user_projects/domains/tamc/webapps/tamcx/log/crontab.log
 

task.null.crontab是一个没有内容的空文件

开启定时任务 

	crontab /weblogic/tools/task.crontab
	停止定时任务
	
	crontab /weblogic/tools/task.null.crontab
	 
	
	crontab [-u username] [-l|-e|-r]

参数：

	-u: 只有root才能进行这个任务，也即帮其他用户新建/删除crontab工作调度;
	
	-e: 编辑crontab 的工作内容;
	
	-l: 查阅crontab的工作内容;
	
	-r: 删除所有的crontab的工作内容，若仅要删除一项，请用-e去编辑。
	
	 
	
	范例一：用dmtsai的身份在每天的12：00发信给自己

crontab -e

#此时会进入vi的编辑界面让你编辑工作。注意到，每项工作都是一行。
	
	0    12   *   *   *   mail dmtsai -s "at 12:00" < /home/dmtsai/.bashrc
	#分  时   日   月  周  |《==============命令行=======================》|
 

代表意义	分钟	小时	日期	月份	周	命令
数字范围	0~59	0~23	1~31	1~12	0~7	就命令啊
周的数字为0或7时，都代表“星期天”的意思。另外，还有一些辅助的字符，大概有下面这些：
	
	特殊字符  	代表意义
	*(星号)	代表任何时刻都接受的意思。举例来说，范例一内那个日、月、周都是*，就代表着不论何月、何日的礼拜几的12：00都执行后续命令的意思。
	,(逗号)	
	代表分隔时段的意思。举例来说，如果要执行的工作是3：00与6：00时，就会是：
	
	0 3,6 * * * command
	
	时间还是有五列，不过第二列是 3,6 ，代表3与6都适用
	
	-(减号)	
	 代表一段时间范围内，举例来说，8点到12点之间的每小时的20分都进行一项工作：
	
	20 8-12 * * * command
	
	仔细看到第二列变成8-12.代表 8,9,10,11,12 都适用的意思
	
	/n(斜线)	
	 那个n代表数字，即是每隔n单位间隔的意思，例如每五分钟进行一次，则：
	
	*/5 * * * * command
	
	用*与/5来搭配，也可以写成0-59/5，意思相同

 

为当前用户创建cron服务

	1.  键入 crontab  -e 编辑crontab服务文件
	
	      例如 文件内容如下：
	
	*/2 * * * * /bin/sh /home/admin/jiaoben/buy/deleteFile.sh 
	     保存文件并并退出
	
	 */2 * * * * /bin/sh /home/admin/jiaoben/buy/deleteFile.sh
	    */2 * * * * 通过这段字段可以设定什么时候执行脚本
	
	      /bin/sh /home/admin/jiaoben/buy/deleteFile.sh 这一字段可以设定你要执行的脚本，这里要注意一下bin/sh 是指运行  脚本的命令  后面一段时指脚本存放的路径
	
	 
	
	2. 查看该用户下的crontab服务是否创建成功， 用 crontab  -l 命令  
	
	 
	
	3. 启动crontab服务 
	
	      一般启动服务用  /sbin/service crond start 若是根用户的cron服务可以用 sudo service crond start， 这里还是要注意  下 不同版本linux系统启动的服务的命令也不同 ，像我的虚拟机里只需用 sudo service cron restart 即可，若是在根用下直接键入service cron start就能启动服务
	
	 
	
	4. 查看服务是否已经运行用 ps -ax | grep cron 
	
	5. crontab命令
	
	      cron服务提供crontab命令来设定cron服务的，以下是这个命令的一些参数与说明:
	
	      crontab -u //设定某个用户的cron服务，一般root用户在执行这个命令的时候需要此参数  
	　　crontab -l //列出某个用户cron服务的详细内容
	　　crontab -r //删除没个用户的cron服务
	　　crontab -e //编辑某个用户的cron服务
	　　比如说root查看自己的cron设置:crontab -u root -l
	　　再例如，root想删除fred的cron设置:crontab -u fred -r
	　　在编辑cron服务时，编辑的内容有一些格式和约定，输入:crontab -u root -e
	　　进入vi编辑模式，编辑的内容一定要符合下面的格式:*/1 * * * * ls >> /tmp/ls.txt
	        任务调度的crond常驻命令
	        crond 是linux用来定期执行程序的命令。当安装完成操作系统之后，默认便会启动此  
	
	       任务调度命令。crond命令每分锺会定期检查是否有要执行的工作，如果有要执行的工
	
	       作便会自动执行该工作。
	
	 
	
	6. crontab命令选项:
	
	     -u指定一个用户
	
	     -l列出某个用户的任务计划
	
	     -r删除某个用户的任务
	
	     -e编辑某个用户的任务
	
	7. cron文件语法:
	
	      分     小时    日       月       星期     命令
	
	      0-59   0-23   1-31   1-12     0-6     command     (取值范围,0表示周日一般一行对应一个任务)
	
	     记住几个特殊符号的含义:
	
	         “*”代表取值范围内的数字,
	         “/”代表”每”,
	         “-”代表从某个数字到某个数字,
	         “,”分开几个离散的数字
	
	8. 任务调度设置文件的写法
	      可用crontab -e命令来编辑,编辑的是/var/spool/cron下对应用户的cron文件,也可以直接修改/etc/crontab文件
	     具体格式如下：
	      Minute Hour Day Month Dayofweek   command
	      分钟     小时   天     月       天每星期       命令
	     每个字段代表的含义如下：
	     Minute             每个小时的第几分钟执行该任务
	     Hour               每天的第几个小时执行该任务
	     Day                 每月的第几天执行该任务
	     Month             每年的第几个月执行该任务
	     DayOfWeek     每周的第几天执行该任务
	     Command       指定要执行的程序
	     在这些字段里，除了“Command”是每次都必须指定的字段以外，其它字段皆为可选

    字段，可视需要决定。对于不指定的字段，要用“*”来填补其位置。
    举例如下：   

 

	复制代码
	5      *       *         *     *     ls             指定每小时的第5分钟执行一次ls命令
	30     5       *         *     *     ls             指定每天的 5:30 执行ls命令 
	30     7       8         *     *     ls             指定每月8号的7：30分执行ls命令
	30     5       8         6     *     ls             指定每年的6月8日5：30执行ls命令 
	30     6       *         *     0     ls             指定每星期日的6:30执行ls命令[注：0表示星期天，1表示星期1， 以此类推，
	　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　也可以用英文来表示，sun表示星期天，mon表示星期一等。]
	30     3       10,20     *     *     ls             每月10号及20号的3：30执行ls命令[注：“，”用来连接多个不连续的时段]
	25     8-11    *         *     *     ls             每天8-11点的第25分钟执行ls命令[注：“-”用来连接连续的时段]
	*/15   *       *         *     *     ls             每15分钟执行一次ls命令 [即每个小时的第0 15 30 45 60分钟执行ls命令 ]
	30     6       */10      *     *     ls             每个月中，每隔10天6:30执行一次ls命令[即每月的1、11、21、31日是的6：30执行一次ls 命令。 ]
	                                              每天7：50以root 身份执行/etc/cron.daily目录中的所有可执行文件
	50     7       *         *     *     root     run-parts     /etc/cron.daily   [ 注：run-parts参数表示，执行后面目录中的所有可执行文件。 ]

复制代码
 

	9. 新增调度任务
	
	     新增调度任务可用两种方法：
	       1)、在命令行输入: crontab -e 然后添加相应的任务，wq存盘退出。
	        2)、直接编辑/etc/crontab 文件，即vi /etc/crontab，添加相应的任务。
	
	10. 查看调度任务
	        crontab -l //列出当前的所有调度任务
	        crontab -l -u jp   //列出用户jp的所有调度任务
	
	11. 删除任务调度工作
	         crontab -r   //删除所有任务调度工作
	
	12. 任务调度执行结果的转向
	       例1：每天5：30执行ls命令，并把结果输出到/jp/test文件中
	            30 5 * * * ls >/jp/test 2>&1
	            注：2>&1 表示执行结果及错误信息。
	      编辑/etc/crontab 文件配置cron  
	
	     cron服务每分钟不仅要读一次/var/spool/cron内的所有文件，还需要读一次 /etc/crontab,因此我们配置这个文件也能运用cron服务做一些事情。用crontab配置是针对某个用户的，而编辑/etc/crontab是针对系统的任务。此文件的文件格式是:  
	
	　　SHELL=/bin/bash  
	
	　　PATH=/sbin:/bin:/usr/sbin:/usr/bin 
	
	　　MAILTO=root //如果出现错误，或者有数据输出，数据作为邮件发给这个帐号  
	
	　　HOME=/ //使用者运行的路径,这里是根目录  
	　　# run-parts  
	
	01   *   *   *   *     root run-parts /etc/cron.hourly         //每小时执行/etc/cron.hourly内的脚本  
	02   4   *   *   *     root run-parts /etc/cron.daily           //每天执行/etc/cron.daily内的脚本  
	22   4   *   *   0     root run-parts /etc/cron.weekly       　　//每星期执行 /etc/cron.weekly内的脚本  
	42   4   1   *   *     root run-parts /etc/cron.monthly     　　//每月去执行/etc/cron.monthly内的脚本  
	
	　 大家注意”run-parts”这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是文件夹名了
	
	    例如：
	
	     1) 在命令行输入: crontab -e 然后添加相应的任务，wq存盘退出。
	
	      2)直接编辑/etc/crontab 文件，即vi /etc/crontab，添加相应的任务

	11 2 21 10 * rm -rf /mnt/fb  
	在UNIX下怎样实现和Windows下“计划任务”一样的功能
	$crontab -e 编辑脚本
	$crontab -l 察看脚本  
	用$crontab -e 编辑脚本，加入下列行
	：分 小时 星期 月 命令  
	Linux下crontab命令的用法
	任务调度的crond常驻命令
	crond 是linux用来定期执行程序的命令。当安装完成操作系统之后，默认便会启动此任务调度命令。crond命令每分锺会定期检查是否有要执行的工作，如果有要执行的工作便会自动执行该工作。而linux任务调度的工作主要分为以下两类：
	1、系统执行的工作：系统周期性所要执行的工作，如备份系统数据、清理缓存
	2、个人执行的工作：某个用户定期要做的工作，例如每隔10分钟检查邮件服务器是否有新信，这些工作可由每个用户自行设置
	3、Crontab是UNIX系统下的定时任务触发器，其使用者的权限记载在下列两个文件中：文件含义
	/etc/cron.deny 该文件中所列的用户不允许使用Crontab命令
	/etc/cron.allow 该文件中所列的用户允许使用Crontab命令
	/var/spool/cron/ 是所有用户的crontab文件
	/var/spool/cron/crontabs

4、Crontab命令的格式为：crontab –l|-r|-e|-i [username]，

其参数含义如表一： 参数名称   含义    示例
-l 显示用户的Crontab文件的内容
crontabl –l 

-i 删除用户的Crontab文件前给提示
crontabl -ri 

-r 
从Crontab目录中删除用户的Crontab文件

crontabl -r 
-e 

编辑用户的Crontab文件
crontabl -e 

5、用户所建立的Crontab文件存于/var/spool/cron中，其文件名与用户名一致。它的格式共分为六段，前五段为时间设定段，第六段为所要执行的命令段，格式如下：* * * * * 
其时间段的含义如表二： 段        含义        取值范围
第一段   代表分钟        0—59 
第二段   代表小时        0—23 
第三段   代表日期        1—31 
第四段   代表月份        1—12 
第五段   代表星期几   0代表星期日
名称 : crontab 
使用权限 : 所有使用者
使用方式 : 
crontab [ -u user ] file 
crontab [ -u user ] { -l | -r | -e } 
说明 : 
crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。-u user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。
餐数 : 
-e : 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe) 

-r : 删除目前的时程表
-l : 列出目前的时程表
时程表的格式如下 : 
f1 f2 f3 f4 f5 program 
其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。
当 f1 为 * 时表示每分钟都要执行 program，f2 为 * 时表示每小时都要执行程序，其馀类推
当 f1 为 a-b 时表示从第 a 分钟到第 b 分钟这段时间内要执行，f2 为 a-b 时表示从第 a 到第 b 小时都要执行，其馀类推
当 f1 为 */n 时表示每 n 分钟个时间间隔执行一次，f2 为 */n 表示每 n 小时个时间间隔执行一次，其馀类推
当 f1 为 a, b, c,... 时表示第 a, b, c,... 分钟要执行，f2 为 a, b, c,... 时表示第 a, b, c...个小时要执行，其馀类推

使用者也可以将所有的设定先存放在档案 file 中，用 crontab file 的方式来设定时程表。 例子 : 
每月每天每小时的第 0 分钟执行一次 /bin/ls : 

0    7    *    *    *    /bin/ls 
在 12 月内, 每天的早上 6 点到 12 点中，每隔 20 分钟执行一次 /usr/bin/backup : 

0　　6-12/3　　*　　12　　*　　/usr/bin/backup 
周一到周五每天下午 5:00 寄一封信给 alex@domain.name : 

0　　17　　*　　*　　1-5　　mail -s "hi" alex@domain.name   /dev/null 2>&1
 即可

例：如果用户的Crontab文件的内容是：29 19 * * * echo its dinner time，则系统每天的19:29显示‘its dinner time’
示例(创建一个cron全过程,每分钟都会在test.txt里输入当前时间): 
1.     以普通用户登录linux系统(我用的是CentOS4.1) 
2.     $crontab –e
说明:系统默认的编辑器是VIM,如果不是请加上以下shell:
$EDITOR=vi
$export EDITOR 
3.     输入”*/1 * * * * date >> $HOME/test.txt”,save and exit VIM 
4.     $su root 
5.     $cd /etc/init.d 
6.     ./crond restart 

下面看看看几个具体的例子：

复制代码
0 　　*/2　　*　　　　* 　　* 　　/sbin/service httpd restart   意思是每两个小时重启一次apache 
50　　7　　  *　　　　* 　　* 　　/sbin/service sshd start   意思是每天7：50开启ssh服务
50　　22　　 *　　　　* 　　* 　　/sbin/service sshd stop   意思是每天22：50关闭ssh服务
0　　 0　　  1,15　　* 　　* 　　fsck /home   每月1号和15号检查/home 磁盘
1　　 *　　  *　　　　* 　　* 　　/home/bruce/backup   每小时的第一分执行 /home/bruce/backup这个文件
00　　03 　　*　　　　*　　1-5　　find /home "*.xxx" -mtime +4 -exec rm {} \;   每周一至周五3点钟，在目录/home中，查找文件名为*.xxx的文件，并删除4天前的文件。
30　　6　　  */10　　* 　　*　　 ls   意思是每月的1、11、21、31日是的6：30执行一次ls命令
复制代码
 

在linux平台上如果需要实现任务调度功能可以编写cron脚本来实现。

以某一频率执行任务

linux缺省会启动crond进程，crond进程不需要用户启动、关闭。
crond进程负责读取调度任务并执行，用户只需要将相应的调度脚本写入cron的调度配置文件中。
cron的调度文件有以下几个：

1.   crontab 

2.   cron.d 

3.   cron.daily 

4.   cron.hourly 

5.   cron.monthly 

6.   cron.weekly 

如果用的任务不是以hourly monthly weekly方式执行，则可以将相应的crontab写入到crontab 或cron.d目录中。

示例：
每隔一分钟执行一次脚本 /opt/bin/test-cron.sh 
可以在cron.d新建脚本 echo-date.sh 
内容为

*/1　　*　　*　　*　　*　　root /opt/bin/test-cron.sh
 

在指定的时间运行任务

也可以通过at命令来控制在指定的时间运行任务

如：

at -f test-cron.sh -v 10:25 
其中-f 指定脚本文件， -v 指定运行时间

quote:ea946d690b="lophyxp"]首先用
contab -l &gt;contabs.tmp
导出contab的配置。
然后编辑contabs.tmp文件。以一下格式添加一行：
分钟 小时 天 月 星期 命令
比如
10 3 * * 0,6 hello
就是每周六、周日的3点10分执行hello程序。
15 4 * * 4-6 hello
就是从周四到周六的4点15点执行hello程序。
然后用
contab contabs.tmp
命令导入新的配置。
一般不建议直接修改/etc/下的相关配置文件。

启动cron进程的方法：/etc/init.d/crond start
开机就启动cron进程的设置命令：chkconfig --add crond

方法二：

把cron加入到启动脚本中：

# rc-update add vixie-cron default

crontab -l #查看你的任务

crontab-e#编辑你的任务

crontab-r#删除用户的crontab的内容

实例讲解二：

系统cron设定：/etc/crontab
    通过 /etc/crontab 文件，可以设定系统定期执行的任务，当然，要想编辑这个文件，得有root权限

0 　　7　　*　　*   *    root    mpg123 ~/wakeup.mp3 
分 时 日 月 周

示例：

0 　　4 　　* 　　* 　　0　　root　　emerge --sync && emerge -uD world              #每周日凌晨4点，更新系统
0 　　2 　　1 　　* 　　*　　root   rm -f /tmp/*                                    #每月1号凌晨2点，清理/tmp下的文件
0 　　8 　　6 　　5 　　*　　root　　mail robin < /home/galeki/happy.txt             #每年5月6日给robin发信祝他生日快乐
假如，我想每隔2分钟就要执行某个命令，或者我想在每天的6点、12点、18点执行命令，诸如此类的周期，可以通过 “ / ” 和 “ , ” 来设置：

*/2　　*　　　　　 *　　*　　*　　root      ...............      #每两分钟就执行........ 
0　　  6,12,18　　*　　*　　*　　root      ...............      #每天6点、12点、18点执行........
每两个小时

0　　*/2　　*　　*　　*　　echo "have a break now." >&gt; /tmp/test.txt
晚上11点到早上8点之间每两个小时，早上八点

0　　23-7/2，8　　*　　*　　*　　echo "have a good dream：）" &gt;&gt; /tmp/test.txt
每个月的4号与每个礼拜的礼拜一到礼拜三的早上11点

0　　11　　4　　*　　1-3　　command line
1月1日早上4点

0　　4 1 1 * command line
 

 
linux下定时执行任务的方法 
在LINUX中你应该先输入crontab -e，然后就会有个vi编辑界面，再输入0 3 * * 1 /clearigame2内容到里面 :wq 保存退出。

 

在LINUX中，周期执行的任务一般由cron这个守护进程来处理[ps -ef|grep cron]。cron读取一个或多个配置文件，这些配置文件中包含了命令行及其调用时间。
cron的配置文件称为“crontab”，是“cron table”的简写。

一、cron在3个地方查找配置文件：
1、/var/spool/cron/ 这个目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名，比如tom建的crontab任务对应的文件就是/var/spool/cron/tom。
一般一个用户最多只有一个crontab文件。

二、/etc/crontab 这个文件负责安排由系统管理员制定的维护系统以及其他任务的crontab。

三、/etc/cron.d/ 这个目录用来存放任何要执行的crontab文件或脚本。

四、权限
crontab权限问题到/var/adm/cron/下一看，文件cron.allow和cron.deny是否存在
用法如下： 
1、如果两个文件都不存在，则只有root用户才能使用crontab命令。 
2、如果cron.allow存在但cron.deny不存在，则只有列在cron.allow文件里的用户才能使用crontab命令，如果root用户也不在里面，则root用户也不能使用crontab。 
3、如果cron.allow不存在, cron.deny存在，则只有列在cron.deny文件里面的用户不能使用crontab命令，其它用户都能使用。 
4、如果两个文件都存在，则列在cron.allow文件中而且没有列在cron.deny中的用户可以使用crontab，如果两个文件中都有同一个用户，
以cron.allow文件里面是否有该用户为准，如果cron.allow中有该用户，则可以使用crontab命令。

 

五、cron服务
　　cron是一个linux下 的定时执行工具，可以在无需人工干预的情况下运行作业。
　　/sbin/service crond start    //启动服务
　　/sbin/service crond stop     //关闭服务
　　/sbin/service crond restart  //重启服务
　　/sbin/service crond reload   //重新载入配置
　　/sbin/service crond status   //查看服务状态 

在crontab文件中如何输入需要执行的命令和时间。该文件中每行都包括六个域，其中前五个域是指定命令被执行的时间，最后一个域是要被执行的命令。
    每个域之间使用空格或者制表符分隔。格式如下： 
　 minute hour day-of-month month-of-year day-of-week commands 
    合法值 00-59 00-23 01-31 01-12 0-6 (0 is sunday) commands（代表要执行的脚本）
    除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。

几个例子： 
每天早上6点 

0　　6　　*　　*　　*　　echo "Good morning." >> /tmp/test.txt //注意单纯echo，从屏幕上看不到任何输出，因为cron把任何输出都email到root的信箱了。
每两个小时 

0　　*/2　　*　　*　　*　　echo "Have a break now." >> /tmp/test.txt  
晚上11点到早上8点之间每两个小时和早上八点 

0　　23-7/2，8　　*　　*　　*　　echo "Have a good dream" >> /tmp/test.txt
每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点 

0　　11　　4　　*　　1-3　　command line
1月1日早上4点 
0 4 1 1 * command line SHELL=/bin/bash PATH=/sbin:/bin:/usr/sbin:/usr/bin MAILTO=root //如果出现错误，或者有数据输出，数据作为邮件发给这个帐号 HOME=/ 

每小时执行/etc/cron.hourly内的脚本

01　　*　　*　　*　　*　　root run-parts /etc/cron.hourly
每天执行/etc/cron.daily内的脚本

02　　4　　*　　*　　*　　root run-parts /etc/cron.daily 
每星期执行/etc/cron.weekly内的脚本

22　　4　　*　　*　　0　　root run-parts /etc/cron.weekly 
每月去执行/etc/cron.monthly内的脚本 

42　　4　　1　　*　　*　　root run-parts /etc/cron.monthly 
注意: "run-parts"这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是文件夹名。 　 
每天的下午4点、5点、6点的5 min、15 min、25 min、35 min、45 min、55 min时执行命令。 

5,15,25,35,45,55　　16,17,18　　*　　*　　*　　command
每周一，三，五的下午3：00系统进入维护状态，重新启动系统。

00　　15　　*　　*　　1,3,5　　shutdown -r +5
每小时的10分，40分执行用户目录下的innd/bbslin这个指令： 

10,40　　*　　*　　*　　*　　innd/bbslink 
每小时的1分执行用户目录下的bin/account这个指令： 

1　　*　　*　　*　　*　　bin/account
每天早晨三点二十分执行用户目录下如下所示的两个指令（每个指令以;分隔）： 

20　　3　　*　　*　　*　　（/bin/rm -f expire.ls logins.bad;bin/expire$#@62;expire.1st）　
每年的一月和四月，4号到9号的3点12分和3点55分执行/bin/rm -f expire.1st这个指令，并把结果添加在mm.txt这个文件之后（mm.txt文件位于用户自己的目录位置）。 

12,55　　3　　4-9　　1,4　　*　　/bin/rm -f expire.1st$#@62;$#@62;mm.txt 
at命令实现定时任务
　　假如我们只是想要让特定任务运行一次,那么，这时候就要用到at监控程序了。
    at类似打印进程，会把任务放到/var/spool/at目录中，到指定时间运行它 。at命令相当于另一个shell，运行at time命令时，它发送一个个命令，可以输入任意命令或者程序。

    at命令执行流程如下

　　# at 2:05 tomorrow

　　at>/home/kyle/do_job

　　at> Ctrl+D

　　AT Time中的时间表示方法

　　-----------------------------------------------------------------------

　　时 间 例子 说明

　　-----------------------------------------------------------------------

　　Minute    at now + 5 minutes   任务在5分钟后运行

　　Hour      at now + 1 hour      任务在1小时后运行

　　Days      at now + 3 days      任务在3天后运行

　　Weeks     at now + 2 weeks     任务在两周后运行

　　Fixed     at midnight          任务在午夜运行

　　Fixed     at 10:30pm           任务在晚上10点30分

　　注意：linux默认为不启动，而ubuntu默认为启动的。检查是否启动，用service atd检查语法，用service atd status检查atd的状态，用service atd start启动atd服务。

　　查看at执行的具体内容：一般位于/var/spool/at目录下面， 用vi打开，在最后一部分就是你的执行程序



参数详解
-V : 印出版本编号 
-q : 使用指定的伫列(Queue)来储存，at 的资料是存放在所谓的 queue 中，使用者可以同时使用多个 queue，而 queue 的编号为 a, b, c... z 以及 A, B, ... Z 共 52 个 
-m : 即使程序/指令执行完成后没有输出结果, 也要寄封信给使用者 
-f file : 读入预先写好的命令档。使用者不一定要使用交谈模式来输入，可以先将所有的指定先写入档案后再一次读入 
网络应用


-l : 列出所有的指定 (使用者也可以直接使用 atq 而不用 at -l) 
-d : 删除指定 (使用者也可以直接使用 atrm 而不用 at -d) 
-v : 列出所有已经完成但尚未删除的指定 

删除任务
atrm 2


三天后的下午 5 点锺执行 /bin/ls : 
at 5pm 3 days /bin/ls 

三个星期后的下午 5 点锺执行 /bin/ls : 
at 5pm 2 weeks /bin/ls 

明天的 17:20 执行 /bin/date : 
at 17:20 tomorrow /bin/date 

1999 年的最后一天的最后一分钟印出 the end of world ! 
at 23:59 12/31/1999 echo the end of world !

 

cron是一个linux下的定时执行工具，可以在无需人工干预的情况下运行作业。由于Cron 是Linux的内置服务，但它不自动起来，可以用以下的方法启动、关闭这个服务：

/sbin/service crond start //启动服务

　　/sbin/service crond stop //关闭服务

　　/sbin/service crond restart //重启服务

　　/sbin/service crond reload //重新载入配置

 

 

　　你也可以将这个服务在系统启动的时候自动启动：

 

　　在/etc/rc.d/rc.local这个脚本的末尾加上：

　　/sbin/service crond start

 

　　现在Cron这个服务已经在进程里面了，我们就可以用这个服务了

 

-------------------------------------

 

 

以Linux下定时备份mysql为例说明下

写一个简单的mysql备份shell脚本

vi

#!/bin/sh
da=`date +%Y%m%d%H%M%S`
mysqldump -u root -pdongjj --all-database>/root/mysqlbakup/$da

保存为 mysqlbak.sh

然后crontab-e

 0 3 * * * /root/mysqlbak.sh 

保存退出

 

相关命令----------------

crontab file [-u user]-用指定的文件替代目前的crontab。 
crontab-[-u user]-用标准输入替代目前的crontab. 
crontab-1[user]-列出用户目前的crontab. 
crontab-e[user]-编辑用户目前的crontab. 
crontab-d[user]-删除用户目前的crontab. 
crontab-c dir- 指定crontab的目录。 
crontab文件的格式：M H D m d cmd. 
M: 分钟（0-59）。 
H：小时（0-23）。 
D：天（1-31）。 
m: 月（1-12）。 
d: 一星期内的天（0~6，0 表示星期天)
　  除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"*/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。

 

 

每次编辑完某个用户的cron设置后，cron自动在/var/spool/cron下生成一个与此用户同名的文件，此用户的cron信息都记录在这　个文件中，这个文件是不可以直接编辑的，只可以用crontab -e　来编辑。cron启动后每过一份钟读一次这个文件，检查是否要执行里面的命令。因此此文件修改后不需要重新启动cron服务。

查看crontab 执行的日志，可以在/var/log/cron* 查看，或者 0 3 * * * /root/mysqlbak.sh >/var/log/mysqlbak.log 2>&1 把日志定向出来查看。