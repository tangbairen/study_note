##yum 源 ：DATABASE DISK IMAGE IS MALFORMED错误的解决办法

昨天更新一个新装的CentOS系统时候，出现了 database disk image is malformed 的错误，Google后发现是yum的数据缓存出问题了，解决办法如下
估计是由于yum的原数据损坏导致的，与rpm的数据库损坏类似，前者会导致更新不能正常执行，后者会导致安装失败并出现乱码，前者的解决参见yum更新和rpm安装包问题(rpmdb: PANIC: Invalid argument)，
后者的错误可以通过一下方法解决：
终端，依次输入：

Shell

	[root@localhost /]#  yum clean metadata
	[root@localhost /]#  yum clean dbcache
	[root@localhost /]#yum makecache

	[root@localhost /]#  yum clean metadata
	[root@localhost /]#  yum clean dbcache
	[root@localhost /]#yum makecache
	即先删除原数据和数据库缓存，然后重建之，问题即可解决。
