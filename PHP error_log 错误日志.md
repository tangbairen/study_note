##PHP error_log 错误日志处理


	一、
	<?php  
	// 如果无法连接到数据库，发送通知到服务器日志  
	if (!Ora_Logon($username, $password)) {  
	    error_log("Oracle database not available!", 0);  
	}  
	  
	// 如果用尽了 FOO，通过邮件通知管理员  
	if (!($foo = allocate_new_foo())) {  
	    error_log("Big trouble, we're all out of FOOs!", 1,  
	               "operator@example.com");  
	}  
	  
	// 调用 error_log() 的另一种方式:  
	error_log("You messed up!", 3, "/var/tmp/my-errors.log");  

两个自定义记录出错日志实例:
	
	<?php  
    //文件1：【error.class.php】  
    function exceptionHandler(){  
        error_reporting(E_ALL ^ E_NOTICE);  
        date_default_timezone_set('Etc/GMT-8');    //设置时区  
     
        ini_set('display_errors',0);    //将错误记录到日志  
        ini_set('error_log','D:\'.date('Y-m-d').'_weblog.txt');  
        ini_set('log_errors',1);    //开启错误日志记录  
        ini_set('ignore_repeated_errors',1);    //不重复记录出现在同一个文件中的同一行代码上的错误信息。  
  
        $user_defined_err = error_get_last();  
        if($user_defined_err['type'] > 0)  
        {  
            switch($user_defined_err['type']){  
                case 1:  
                    $user_defined_errType = '致命的运行时错误(E_ERROR)';  
                    break;  
                case 2:  
                    $user_defined_errType = '非致命的运行时错误(E_WARNING)';  
                    break;  
                case 4:  
                    $user_defined_errType = '编译时语法解析错误(E_PARSE)';  
                    break;  
                case 8:  
                    $user_defined_errType = '运行时提示(E_NOTICE)';  
                    break;  
                case 16:  
                    $user_defined_errType = 'PHP内部错误(E_CORE_ERROR)';  
                    break;  
                case 32:  
                    $user_defined_errType = 'PHP内部警告(E_CORE_WARNING)';  
                    break;  
                case 64:  
                    $user_defined_errType = 'Zend脚本引擎内部错误(E_COMPILE_ERROR)';  
                    break;  
                case 128:  
                    $user_defined_errType = 'Zend脚本引擎内部警告(E_COMPILE_WARNING)';  
                    break;  
                case 256:  
                    $user_defined_errType = '用户自定义错误(E_USER_ERROR)';  
                    break;  
                case 512:  
                    $user_defined_errType = '用户自定义警告(E_USER_WARNING)';  
                    break;  
                case 1024:  
                    $user_defined_errType = '用户自定义提示(E_USER_NOTICE)';  
                    break;  
                case 2048:  
                    $user_defined_errType = '代码提示(E_STRICT)';  
                    break;  
                case 4096:  
                    $user_defined_errType = '可以捕获的致命错误(E_RECOVERABLE_ERROR)';  
                    break;  
                case 8191:  
                    $user_defined_errType = '所有错误警告(E_ALL)';  
                    break;  
                default:  
                    $user_defined_errType = '未知类型';  
                    break;  
                }  
            $msg = sprintf('%s %s %s %s %s',date("Y-m-d H:i:s"),$user_defined_errType,$user_defined_err['message'],$user_defined_err['file'],$user_defined_err['line']);  
            error_log($msg,0);  
        }  
    }  
  
    register_shutdown_function('exceptionHandler');  
	?>  
	  
	调用方法  
	  
	<meta charset="utf-8">  
	<?php  
	    //文件2：【test.php】  
	    include('error.class.php');  
	    echo $_COOKIE['aaaaadfa'];    //此cookie不存在就会产生一个错误，用来做测试用  
	    echo $_SESSION['aaaaadfa'];    //此session不存在就会产生一个错误，用来做测试用  
	?>

第二个实例：
	
	<?php  
	/**********************************************************  
	 * File name: LogsClass.class.php  
	 * Class name: 日志记录类  
	 * Create date: 2008/05/14  
	 * Update date: 2008/09/28  
	 * Author: blue  
	 * Description: 日志记录类  
	 * Example: //设定路径和文件名  
	 * $dir="a/b/".date("Y/m",time());  
	 * $filename=date("d",time()).".log";  
	 * $logs=new Logs($dir,$filename);  
	 * $logs->setLog("test".time());  
	 * //使用默认  
	 * $logs=new Logs();  
	 * $logs->setLog("test".time());  
	 * //记录信息数组  
	 * $logs=new Logs();  
	 * $arr=array(  
	 * 'type'=>'info',  
	 * 'info'=>'test',  
	 * 'time'=>date("Y-m-d H:i:s",time())  
	 * );  
	 * $logs->setLog($arr);  
	 **********************************************************/  
	class Logs {  
    private $_filepath; //文件路径  
    private $_filename; //日志文件名  
    private $_filehandle; //文件句柄  
     
  
    /**  
     *作用:初始化记录类  
     *输入:文件的路径,要写入的文件名  
     *输出:无  
     */  
    public function Logs($dir = null, $filename = null) {  
        //默认路径为当前路径  
        $this->_filepath = empty ( $dir ) ? '' : $dir;  
         
        //默认为以时间＋.log的文件文件  
        $this->_filename = empty ( $filename ) ? date ( 'Y-m-d', time () ) . '.log' : $filename;  
         
        //生成路径字串  
        $path = $this->_createPath ( $this->_filepath, $this->_filename );  
        //判断是否存在该文件  
        if (! $this->_isExist ( $path )) { //不存在  
            //没有路径的话，默认为当前目录  
            if (! empty ( $this->_filepath )) {  
                //创建目录  
                if (! $this->_createDir ( $this->_filepath )) { //创建目录不成功的处理  
                    die ( "创建目录失败!" );  
                }  
            }  
            //创建文件  
            if (! $this->_createLogFile ( $path )) { //创建文件不成功的处理  
                die ( "创建文件失败!" );  
            }  
        }  
         
        //生成路径字串  
        $path = $this->_createPath ( $this->_filepath, $this->_filename );  
        //打开文件  
        $this->_filehandle = fopen ( $path, "a+" );  
    }  
     
    /**  
     *作用:写入记录  
     *输入:要写入的记录  
     *输出:无  
     */  
    public function setLog($log) {  
        //传入的数组记录  
        $str = "";  
        if (is_array ( $log )) {  
            foreach ( $log as $k => $v ) {  
                $str .= $k . " : " . $v . "n";  
            }  
        } else {  
            $str = $log . "n";  
        }  
         
        //写日志  
        if (! fwrite ( $this->_filehandle, $str )) { //写日志失败  
            die ( "写入日志失败" );  
        }  
    }  
     
    /**  
     *作用:判断文件是否存在  
     *输入:文件的路径,要写入的文件名  
     *输出:true | false  
     */  
    private function _isExist($path) {  
        return file_exists ( $path );  
    }  
     
    /**  
     *作用:创建目录(引用别人超强的代码-_-;;)  
     *输入:要创建的目录  
     *输出:true | false  
     */  
    private function _createDir($dir) {  
        return is_dir ( $dir ) or ($this->_createDir ( dirname ( $dir ) ) and mkdir ( $dir, 0777 ));  
    }  
     
    /**  
     *作用:创建日志文件  
     *输入:要创建的目录  
     *输出:true | false  
     */  
    private function _createLogFile($path) {  
        $handle = fopen ( $path, "w" ); //创建文件  
        fclose ( $handle );  
        return $this->_isExist ( $path );  
    }  
     
    /**  
     *作用:构建路径  
     *输入:文件的路径,要写入的文件名  
     *输出:构建好的路径字串  
     */  
    private function _createPath($dir, $filename) {  
        if (empty ( $dir )) {  
            return $filename;  
        } else {  
            return $dir . "/" . $filename;  
        }  
    }  
     
    /**  
     *功能: 析构函数，释放文件句柄  
     *输入: 无  
     *输出: 无  
     */  
    function __destruct() {  
        //关闭文件  
        fclose ( $this->_filehandle );  
    }  
	}  
	?> 