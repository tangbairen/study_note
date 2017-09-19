##有记录就加一，没有就添加数据
	insert into `aaaaa` (`id`,`name`,`title`,`num`) VALUES ('传过来的值','asdf','1111',1) 
	on DUPLICATE key UPDATE `num`=`num`+1;	

##mysql分页性能优化：提高mysql大数据量下分页查询速度
	关于分页的优化。
	我们知道，在MySQL中分页很简单，直接LIMIT page_no,page_total 就可以了。
	可是当记录数慢慢增大时，她就不那么好使了。
	这里我们创建摘要表来记录页码和原表之间的关联。

原表：

	CREATE TABLE `t_group` (
	   `id` int(11) NOT NULL auto_increment,
	   `money` decimal(10,2) NOT NULL,
	   `user_name` varchar(20) NOT NULL,
	   `create_time` timestamp NOT NULL default CURRENT_TIMESTAMP on update   CURRENT_TIMESTAMP,
	   PRIMARY KEY  (`id`),
	   KEY `idx_combination1` (`user_name`,`money`)
	 ) ENGINE=MyISAM DEFAULT CHARSET=utf8;

分页表：

	CREATE TABLE `t_group_ids` (
	   `id` int(11) NOT NULL,
	   `group_id` int(11) NOT NULL,
	   PRIMARY KEY  (`id`,`group_id`),
	   KEY `idx_id` (`id`),
	   KEY `idx_group_id` (`group_id`)
	 ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


插入分页表数据。当然这里如果你的表主键不是ID,那你得自己想办法搞这个分页表的数据了。这个好实现，就不说了。

	mysql> insert into t_group_ids select ceil(id/20),id from t_group;

用普通LIMIT来实现分页。

	mysql> select * from t_group where 1 limit 20;

用分页表来实现分页：其中b.id = 1就是分页表示第一页。

mysql> select a.* from t_group as a inner join t_group_ids as b where a.id = b.group_id and b.id =


总结：我们看到，当表记录数增加时，LIMIT的性能随着线性增长。而当我们存放了页码与主键的关联后，性能大增。

但是，这个方法你需要在删除数据时，及时维护分页表，也就是：

	mysql> insert into t_group_ids select ceil(id/20),id from t_group;
	 Query OK, 10485760 rows affected (2 min 56.19 sec)
	 Records: 10485760  Duplicates: 0  Warnings: 0
	维护耗时较长。

##DCL 语句
	主要是DBA用来管理系统中的对象权限时使用，一般开发人员很少使用。

###创建一个数据库用户zl, 具有对sakila 数据库中所有表的 select / insert 权限

	grant select,insert on sakila.* to 'zl'@'localhost' identified by '123'

###由于权限变更，需要将zl的权限变更，收回insert，只能对数据进行select 操作
	
	revoke insert on sakila.* from 'zl'@'localhost'


##MySQL 流程函数

流程函数也是很常用的一类函数，用户可以使用这类函数在一个SQL语句中实现条件选择，这样做能够提高语句的效率

	Mysql中的流程函数
	
	if(value,t,f) 	如果value是真的， 返回 t，否则返回 f

	ifnull(value1 , value2)		如果value1不为空，返回value1,否则返回value2
	
	case when [value] then [result] else [default] end  如果value是真，返回result,否则返回result

	case [expr] when [value] then [result] end  如果expr 等于 value 返回result

##Mysql 其它函数
	
	database()				返回当前数据库名
	version()				返回当前数据库版本
	user()					返回当前登录用户名
	inet_aton(IP)			返回IP地址的数字表示
	inet_ntoa(num)          返回数字代表的IP地址
	password(str)			返回字符串str的加密办法
	md5()					返回字符串str的MD5值
	



