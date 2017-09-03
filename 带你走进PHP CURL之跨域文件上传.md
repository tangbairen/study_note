##带你走进PHP CURL之跨域文件上传

	CURL描述:
		CURL是一个非常强大的开源库，支持很多协议，包括HTTP、FTP、TELNET等，我们使用它来发送HTTP请求。
		它给我 们带来的好处是可以通过灵活的选项设置不同的HTTP协议参数，并且支持HTTPS。CURL可以根据URL
		前缀是“HTTP” 还是“HTTPS”自动选择是否加密发送内容

使用CURL的PHP扩展完成一个HTTP请求的发送一般有以下几个步骤：

	1.初始化连接句柄；
	2.设置CURL选项；
	3.执行并获取结果；
	4.释放CURL连接句柄。


获取CURL请求的输出信息

	在curl_exec()函数执行之后，可以使用curl_getinfo()函数获取CURL请求输出的相关信息，示例代码如下：
		curl_exec($ch);
		$info = curl_getinfo($sh);
		echo ' 获取 '.$info['url'].'耗时'.$info['total_time'].'秒';


使用CURL发送GET请求

	$uid=$_GET['id'];
    $ch = curl_init();
    $url = 'http://localhost/advanced/frontend/web/index.php?r=friends/friends/basicinformation&uid='.$uid;
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    // 执行HTTP请求
    curl_setopt($ch , CURLOPT_URL , $url);
    $res = curl_exec($ch);
    var_dump(json_decode($res));


使用CURL发送POST请求

	可以使用CURL提供的选项CURLOPT_POSTFIELDS，设置该选项为POST字符串数据就可以把请求放在正文中
	代码如下：
		$url = "http://localhost/advanced/frontend/web/index.php?r=friends/friends/updatephone";
	// 填写你的登陆信息，
		$data = array ("uid" => '1',"newphone" => "15278364755");
		$ch = curl_init();
		curl_setopt($ch, CURLOPT_URL, $url);
		curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
		curl_setopt($ch, CURLOPT_POST, 1);
		curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
		$output = curl_exec($ch);
		curl_close($ch);
	
		$res=json_decode($output,true);
		print_r($res);



使用CURL 进行跨域上传文件

	/*
	*   限制图片上传的格式
	*   假设提交表单的名称为：imageFile
	*/
	$array=['image/png','image/jpg','image/jpeg','image/gif'];
	if(!in_array($_FILES['imageFile']['type'], $array)){
		echo "<script>alert('请上传正确格式的图片');window.history.go(-1);</script>";
		exit;
	}
	
	foreach ($_FILES as $key => $value) {
	 	$tmpname = $_FILES[$key]['name'];
	    $tmpfile = $_FILES[$key]['tmp_name'];
	    $tmpType = $_FILES[$key]['type'];
	    $curlfiles[$key] = new CURLFile(realpath($tmpfile), $tmpType, $tmpname);
	}

	$data=array();
	$dataTotal = array_merge($data, $curlfiles);
	$dataTotal['name']='imageFile';
	$dataTotal['uid']=1;

	$url='http://localhost/advanced/frontend/web/index.php?r=service/service/servicesave';
  	$ch = curl_init();
    curl_setopt($ch, CURLOPT_USERPWD, 'joe:secret');
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $dataTotal);
    // curl_setopt($ch, CURLOPT_HEADER, false);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $return_data = curl_exec($ch);
    curl_close($ch);
    // echo $return_data;
    var_dump($return_data);
    echo '<pre>';
		print_r(json_decode($return_data,true));
	echo '</pre>';



解决CURL PHP版本兼容性问题

	// 解决兼容性问题
	if(version_compare(phpversion(),'5.5.0') >= 0 && class_exists('CURLFile')){
		$data['uid']=2;
		$data['name']='imageFile'; //表单名
	    $data['imageFile'] = new CURLFile(realpath($name));//图片的路径
	}else{
		$id=1;
		$data['name']='imageFile';//表单名
	    $data['imageFile'] = '@'.$name;
	}
    $ch = curl_init(); 

    curl_setopt($ch, CURLOPT_URL, "http://localhost/advanced/frontend/web/index.php?r=service/service/servicesave"); 
    curl_setopt($ch, CURLOPT_POST, 1);//post请求
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 60);//请求时间 
    curl_setopt($ch, CURLOPT_POSTFIELDS , $data);//传输的数据
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 
    $output = curl_exec($ch); 
    $aStatus = curl_getinfo($ch);
    curl_close($ch);

   	var_dump($output);

##PHP CURL GET请求

	/**
     * 使用cURL模拟get请求
     * @param 请求的地址
     * @param 请求头(将apikey设置)
     * @return [json] 返回的结果
     */
    function getcurl($url , $data = []){
        // 1.初始化cURL
        $ch = curl_init( $url );
        // 2.设置请求头
        curl_setopt($ch,CURLOPT_HTTPHEADER , $data );
        // 3.获取的信息以字符串返回，而不是直接输出
        curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
        // 4.发起请求
        return curl_exec($ch);
    }

##PHP CURL POST请求

	/**
     * 使用cURL模拟post请求
     * @param [string] $url [请求的地址(必须是绝对路径)]
     * @param [array]  $data [提交的键值对信息]
     * @return [json] 返回的结果
     */
    function postcurl($url , $data = []){
        // 1.初始化cURL
        $ch = curl_init( $url );
        // 2.获取的信息以字符串返回，而不是直接输出
        curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
        // 3.声明为POST请求
        curl_setopt($ch , CURLOPT_POST , 1);
        // 4.设置请求体
        curl_setopt($ch , CURLOPT_POSTFIELDS , $data );
        // 5.发起请求
        return curl_exec($ch);
    }