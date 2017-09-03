##Thinkphp 5 数据库的连接操作
config()助手函数，可以打印配置信息

	config('database');


连接数据库操作
	
	Db::connect();
	它并没有真正的连接，是惰性加载，等执行对应的方法时，才回进行对应的连接！

还可以动态配置
![](http://i.imgur.com/cARJJ69.jpg)

还可以使用DNS 方式进行配置
![](http://i.imgur.com/jIaifC7.jpg)

也可以在config文件，再定义一个数据库配置信息的数组
![](http://i.imgur.com/DSjt25I.jpg)

可以不用Config::get();直接对应的参数就可以了



##Thinkphp5 数据库查询操作

原生SQL操作

![](http://i.imgur.com/paRcI9j.jpg)

Db操作类：

![](http://i.imgur.com/JniRQoH.jpg)

查询一条数据；

![](http://i.imgur.com/244IvoP.jpg)

查询某个字段的一条记录值

![](http://i.imgur.com/yOJwY46.jpg)

![](http://i.imgur.com/w4s7Av6.jpg)

![](http://i.imgur.com/lFCtWHB.jpg)	

Tp5中还提供了db助手函数

	db('user')->select();//使用了表前缀 、、这样写，每次调用都会实例化数据库Db类
	db('user',[],false)->select();  //这样就使用了单例模式，


##Thinkphp5 数据库添加
![](http://i.imgur.com/PMgB1hN.jpg)

$db->insert([]); //返回受影响行数

添加返回自增ID的方法

	$db->insertGetId(['name'=>'111']);

批量添加方法
	$db->insertAll($data); // $data是一个二维数组，返回总共的受影响行数

##Thinkphp5 数据更新 update

![](http://i.imgur.com/zqoInyk.jpg)

更新某一个字段的值 ： setField();  返回受影响行数

	$db->where([])->setField('name','bairen');

对字段 修改 自增 数   setInc()

![](http://i.imgur.com/72UqAI7.jpg)

对字段 修改 自减数  setDec()

![](http://i.imgur.com/iSpp00L.jpg)


##Thinkphp5 数据删除

![](http://i.imgur.com/nndYjS3.jpg)

如果是主键，自增id ，删除方法

	$db->delete(1);



##Thinkphp5 条件构造器

返回查询语句的方法：  buildSql();

![](http://i.imgur.com/01W06wj.jpg)

between and 

![](http://i.imgur.com/vaj2PID.jpg)

多种where 语句

![](http://i.imgur.com/TdTDd3j.jpg)

使用表达式：

![](http://i.imgur.com/g7ddqYX.jpg)

如果有多个条件，可以写 多个where（） 方法

or 条件的操作

![](http://i.imgur.com/Pr5U7j3.jpg)

![](http://i.imgur.com/T2xbIB7.jpg)


##Thinkphp5 链式操作

![](http://i.imgur.com/njgo766.jpg)

分页 page（） 方法

![](http://i.imgur.com/IUI5T5X.jpg)


##Thinkphp5 模型 Model部分

模型的定义：
![](http://i.imgur.com/nxJ8Rm7.jpg)

在控制器中使用模型 model

![](http://i.imgur.com/SVdN92j.jpg)

使用model() 助手函数 操作

![](http://i.imgur.com/n2EqlIg.jpg)


##Thinkphp5 使用模型查询数据

![](http://i.imgur.com/KboArNr.jpg)

![](http://i.imgur.com/XRP7oYU.jpg)

![](http://i.imgur.com/v9b8HIf.jpg)

返回字段的所有数据，指定它的下标

![](http://i.imgur.com/v0NE7QF.jpg)

获取多条数据

![](http://i.imgur.com/HrKscLc.jpg)


##Thinkphp 5 模型添加数据

create()方法，返回插入对象的值，
![](http://i.imgur.com/KOYI5qF.jpg)

添加不存在的字段，默认会报错，如果要过滤不存在的字段，在create（）方法，传递第二个参数值：true

![](http://i.imgur.com/D4pFpAM.jpg)

只允许添加的字段

![](http://i.imgur.com/JuyLiXx.jpg)

使用save()方法，添加数据

![](http://i.imgur.com/x1UNgZE.jpg)

save（）方法的另一种方式，allowField() 方法为true,过滤不存在的值

![](http://i.imgur.com/jELYxGS.jpg)

添加多条数据  二维数组

![](http://i.imgur.com/S4DrUnS.jpg)


##Thinkphp5 使用模型更新数据

![](http://i.imgur.com/ZQjwb0g.jpg)

![](http://i.imgur.com/0CA65W2.jpg)

使用闭包函数

![](http://i.imgur.com/LVesQpx.jpg)

判断是否被更新 

![](http://i.imgur.com/bSY2Whe.jpg)

使用save 方式更新数据， get()方法里面的是id, 也是可以判断是否被更新

![](http://i.imgur.com/hZr4Otr.jpg)

还可以使用对象的方式

![](http://i.imgur.com/RhhqOpM.jpg)

![](http://i.imgur.com/xjj9j7N.jpg)

##Thinkphp 5 使用模型删除数据

![](http://i.imgur.com/bywbcEE.jpg)

![](http://i.imgur.com/FL6SMXX.jpg)

![](http://i.imgur.com/relnUMf.jpg)

![](http://i.imgur.com/i4LlGv8.jpg)


##Thinkphp5 模型聚合操作

![](http://i.imgur.com/ErzTu7X.jpg)


##Thinkphp 5 模型获取器

例如把性别转换成 中文（男，女），toAarray()转换成数组，getData():获取原始数据

![](http://i.imgur.com/PWoPYcX.jpg)

模型示例：

![](http://i.imgur.com/8KW5lHq.jpg)


##Thinkphp5 模型修改器 + 自动完成

