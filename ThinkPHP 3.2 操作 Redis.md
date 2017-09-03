##ThinkPHP 3.2 操作 Redis
	原本感觉 Redis 应该像是作为数据库的一种被拿来操作的，可是实际上 thinkphp 3.2 是把 redis 作为缓存的一种方式来进行解析的，
	从 redis 文件被存放的位置就可以看出来：
	\ThinkPHP \Library \Think \Cache \Driver
	是作为 Cache，缓存方式的一种被拿来使用的，可是经过我们前面的学习，我们发现 Redis 不光光能做这些。
	还有一个发现是这样的，假设我在 thinkphp 的控制器中执行如下代码：

代码：
	
	<?php  
  
	namespace Home\Controller;  
	  
	use Think\Controller;  
	class IndexController extends Controller {  
	    public function index() {  
	        // 配置 redis 缓存  
	        $set = array(  
	            'type' =>'redis' ,  
	            'host'=>'127.0.0.1' ,  
	            'port'=>6379,  
	            );  
	        // 实例化  
	        $redis=S($set);  
	        // 存储数据  
	        $redis->name="hello world again";  
	        $redis->id=1;  
	    }  
	} 

第二种：

	<?php  
	  
	namespace Home\Controller;  
	  
	use Think\Controller;  
	class IndexController extends Controller {  
	    public function index() {  
	        // 配置 redis 缓存  
	        $set = array(  
	            'type' =>'redis' ,  
	            'host'=>'127.0.0.1' ,  
	            'port'=>6379,  
	            );  
	        // 设置要存储的数据  
	        $message = array(  
	            'name' =>'yang' ,  
	            'id'=>1  
	             );  
	        // 缓存  
	        S('message',$message,$set);  
	    }  
	}  




## 使用ThinkPHP扩展，实现Redis的CURD操作
	
	本文章内容节选自《PHP MVC开发实战》一书第16.4.2章节。
	一、概述
	Redis是一个NoSQL数据库，由于其数据类型的差异，所以要在MVC框架中实现CURD操作，比较繁锁。事实上在ThinkPHP框架中，只能实现简单的缓存应用。而不像MongoDB那样能够实现常见数据库的CURD操作。本文章将通过扩展的方式，实现redis的CURD操作，这样我们就可以像操作普通的MySQL数据库那样实现Redis的编程了。
	
	二、实现过程
	接下为将以ThinkPHP作为MVC开发框架，详细介绍Redis的CURD操作。需要说明的是，在ThinkPHP中本身并不支持Redis开发环境，只支持使用Redis开发简单的数据缓存功能。所以我们必须要通过扩展功能，实现Redis的编程支持。为了方便读者学习，笔者临时开发了相应的模块扩展及数据库扩展。
	下载址为  http://beauty-soft.net/book/php_mvc/code/thinkphp_redis.html
	解压下载后的压缩包，将得到DbRedis.class.PHP文件及RedisModel.class.php文件。将DbRedis.class.php文件复制到ThinkPHP/Extend/Driver/Db目录；将RedisModel.class.php文件复制到ThinkPHP/Extend/Model目录。然后在项目配置文件中加入Redis数据库连接信息，如以下代码所示。


	'REDIS_HOST'=>'192.168.0.2',   
	'REDIS_PORT'=>6379,   
	'REDIS_AUTH'=>123456,   
	'REDIS_DB_PREFIX'=>'',

	读者可根据实际环境填写即可。通过前面步骤，至此就完成了在ThinkPHP中进行Redis开发的前期准备，
	接下来将结合示例代码，详细演示Redis的CURD操作。  


1、增加数据

这里的增加数据包括Redis五大数据类型的数据添加。由于篇幅所限，这里不再详细介绍操作的实现原理，将通过代码演示操作方式。如以下代码所示。

	<?php   
	/**   
	* redis添加数据   
	* Enter description here ...   
	* @author Administrator   
	*   
	*/   
	class AddAction extends Action{   
	    /**   
	     * list类型   
	     * Enter description here ...   
	     */   
	    public function lists(){   
	        $Redis=new RedisModel("list11");   
	        //一次只能推送一条         
	        echo $Redis->add("ceiba");   
	    }   
	     /**   
	     * 字符串类型   
	     * Enter description here ...   
	     */   
	    public function string(){   
	        $Redis=new RedisModel();   
	        $data=array(   
	            "str1"=>"ceiba", //一个key，对应一个值   
	            "str2"=>"李开湧",   
	            "str3"=>"李明",   
	        );   
	        echo $Redis->type("string")->add($data);   
	    }   
	    /**   
	     * HASH类型   
	     * Enter description here ...   
	     */   
	    public function hash(){   
	        $Redis=new RedisModel("user:1");   
	             $data=array(   
	               "field1"=>"ceiba", //一个key，对应一个值   
	               "field2"=>"李开湧",   
	               "field3"=>"李明",   
	             );   
	             //支持批量添加   
	             echo $Redis->type("hash")->add($data);          
	    }   
	     /**   
	     * 集合类型   
	     * Enter description here ...   
	     */   
	    public function sets(){   
	             $Redis=new RedisModel("sets:1");   
	        //一次只能推送一条         
	        echo $Redis->type("sets")->add("ceiba");   
	    }   
	      /**   
	     * 有序集合   
	     * Enter description here ...   
	     */   
	    public function zset(){    
	        $Redis=new RedisModel("zset:1");   
	        //支持批量添加   
	        $data=array(   
	            //排序=>值   
	            "10"=>"ceiba",   
	            "11"=>"李开湧",   
	            "12"=>"李明"   
	        );         
	        echo $Redis->type("zset")->add($data);   
	    }   
	}   
	?>      

2、查询数据
	
	<?php   
	// redis查询数据   
	class IndexAction extends Action {   
	    public function page(){   
	        $this->display();   
	    }   
	    /**   
	     * 列表类型，默认类型   
	     * Enter description here ...   
	     */   
	    public function lists(){   
	        //dump(C("REDIS_HOST"));    
	        $Redis=new RedisModel("list1");   
	        $field=array(   
	            "nmae","age","pro"   
	        );   
	        $data=$Redis->field($field)->select();   
	        dump($data);   
	        //获得队列中的记录总数   
	        $count=$Redis->count();   
	        dump($count);   
	    }   
	    /**   
	     * 字符串类型   
	     * Enter description here ...   
	     */   
	    public function string(){   
	            $Redis=new RedisModel();   
	            //field 表示每个key名称   
	            $rows=$Redis->type("string")->field(array("str1","str2"))->select();   
	            dump($rows);   
	    }   
	    /**   
	     * HASH类型   
	     * Enter description here ...   
	     */   
	    public function hash(){   
	            $Redis=new RedisModel("h9");   
	            //默认显示所有HASH字段，可以通过field连惯操作限制   
	            $rows=$Redis->type("hash")->field(array("field1"))->select();   
	            dump($rows);   
	            //统计总记录   
	            $count=$Redis->type("hash")->count();   
	            dump($count);          
	    }   
	    /**   
	     * 集合类型   
	     * Enter description here ...   
	     */   
	    public function sets(){   
	            $Redis=new RedisModel();   
	            $arr=array(   
	            "s3","s4"   
	            );   
	       $rows=$Redis->type("sets")->field($arr)->where("sinterstore")->select();//求交集   
	          dump($rows);   
	          $rows=$Redis->type("sets")->field($arr)->where("sunion")->select();//求并集   
	          dump($rows);   
	          $rows=$Redis->type("sets")->field($arr)->where("sdiff")->select();//求差集   
	          dump($rows);   
	          $Redis=new RedisModel("s3");   
	          $rows=$Redis->type("sets")->select(); //返回单个集合列表中的所有成员   
	          dump($rows);   
	          //统计记录   
	          $Redis=new RedisModel("s3");   
	          $count=$Redis->type("sets")->count();    
	          dump($count);        
	    }   
	    /**   
	     * 有序集合   
	     * Enter description here ...   
	     */   
	    public function zset(){    
	        $Redis=new RedisModel("z2");    
	        //默认显示0到20        
	        $data=$Redis->type("zset")->limit("0,-1")->select();   
	        dump($data);   
	        //使用zRevRange显示数据，数组第2个参数为true时显示排序号   
	         $data=$Redis->type("zset")->limit("0,-1")->order(array("zRevRange",true))->select();   
	        dump($data);   
	        //不设置limit时，将统计所有记录   
	        $count=$Redis->type("zset")->limit("0,1")->count();   
	        dump($count);   
	           
	    }   
	}

3、删除数据
	
	<?php   
	/**   
	* Redis删除数据   
	* Enter description here ...   
	* @author Administrator   
	*   
	*/   
	class DeleteAction extends Action{   
	    /**   
	     * list类型   
	     * Enter description here ...   
	     */   
	    public function lists(){   
	        $Redis=new RedisModel("mylist");   
	            //根据索引号，删除指定的list元素            
	        echo $Redis->where(3)->delete();   
	        //ltrim区间批量删除，保留4~5之间的记录   
	echo $Redis->type("list")->where(array("4","5"))->delete("ltrim");    
	        //lpop单条顺序弹出       
	echo $Redis->type("list")->delete("lpop");    
	           
	    }   
	     /**   
	     * 字符串类型   
	     * Enter description here ...   
	     */   
	    public function string(){   
	           $Redis=new RedisModel();   
	           //直接删除key，这各方式适用于所有数据类型   
	           echo $Redis->type("string")->field(array("str1","str2"))->delete();   
	    }   
	    /**   
	     * HASH类型   
	     * Enter description here ...   
	     */   
	    public function hash(){   
	        $Redis=new RedisModel("user:1");           
	             //删除指定hash中的指定字段(field),不支持批量删除   
	             echo $Redis->type("hash")->where("field1")->delete();    
	       
	    }   
	     /**   
	     * 集合类型   
	     * Enter description here ...   
	     */   
	    public function sets(){   
	             $Redis=new RedisModel("s1");   
	        //删除sets:1集合中名为age的value       
	        echo $Redis->type("sets")->where("age")->delete();   
	    }   
	    /**   
	     * 有序集合   
	     * Enter description here ...   
	     */   
	    public function zset(){    
	        $Redis=new RedisModel("z1");   
	        //根据集合元素value进行删除   
	        echo $Redis->type("zset")->where("two")->delete();    
	        //根据排序号进行区间批量删除，保留2~3之间的记录   
	        echo $Redis->type("zset")->where(array("1","4"))->delete("zremRangeByScore");    
	        //根据索引号进行区间批量删除，保留2~3之间的记录   
	        echo $Redis->type("zset")->where(array("1","3"))->delete("zRemRangeByRank");    
	    }   
	}   
	?>

在Redis中，更新数据与添加数据是可以相互转换的，所以这里不再介绍。更多的功能特性及使用方法，随着时间的推移，笔者会进行更新。本书读者可在配套网站中得到后续支持。
更多使用方式，请阅读《PHP MVC开发实战》。书中还介绍了怎样实现数据分页的全过程。        