##关于vmware虚拟机 device eth0 does not seem to present 的问题

一、克隆之后的虚拟机centos6.x会有这种情况，解决方法：

1、删掉网卡配置文件的HWADDR和UUID这两行，将：

ONBOOT=yes

NM_CONTROLLED=no

2、将/etc/udev/rules.d/70-persistent-net.rules文件移动到别的地方或者rm掉

3、重启系统，ok。

4、重启之后70文件如果没有自动生成，可以在别的机器上拷贝一份过来，

5、执行 ifconfig ethx,查看现在的MAC地址，然后将网卡配置文件添加上现在的MAC地址，并修改70文件种的Mac地址。