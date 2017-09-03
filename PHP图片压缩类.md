##PHP图片压缩操作类 ：按缩放的比例来
	<?php  
	/** 
	 图片压缩操作类 
	 v1.0 
	*/  
   	class Image{  
         
       private $src;  
       private $imageinfo;  
       private $image;  
       public  $percent = 0.1;  
       public function __construct($src){  
           $this->src = $src;  
       }  
       /** 
       打开图片 
       */  
       public function openImage(){  
             
           list($width, $height, $type, $attr) = getimagesize($this->src);  
           $this->imageinfo = array(  
                  
                'width'=>$width,  
                'height'=>$height,  
                'type'=>image_type_to_extension($type,false),  
                'attr'=>$attr  
           );  
           $fun = "imagecreatefrom".$this->imageinfo['type'];  
           $this->image = $fun($this->src);  
       }  
       /** 
       操作图片 
       */  
       public function thumpImage(){  
             
            $new_width = $this->imageinfo['width'] * $this->percent;  
            $new_height = $this->imageinfo['height'] * $this->percent;  
            $image_thump = imagecreatetruecolor($new_width,$new_height);  
            //将原图复制带图片载体上面，并且按照一定比例压缩,极大的保持了清晰度  
            imagecopyresampled($image_thump,$this->image,0,0,0,0,$new_width,$new_height,$this->imageinfo['width'],$this->imageinfo['height']);  
            imagedestroy($this->image);    
            $this->image =   $image_thump;  
       }  
       /** 
       输出图片 
       */  
       public function showImage(){  
             
            header('Content-Type: image/'.$this->imageinfo['type']);  
            $funcs = "image".$this->imageinfo['type'];  
            $funcs($this->image);  
             
       }  
       /** 
       保存图片到硬盘 
       */  
       public function saveImage($name){  
             
            $funcs = "image".$this->imageinfo['type'];  
            $funcs($this->image,$name.'.'.$this->imageinfo['type']);  
             
       }  
       /** 
       销毁图片 
       */  
       public function __destruct(){  
             
           imagedestroy($this->image);  
       }  
         
   	}  
        $src = "9-1.jpg";  //图片路径
        $image = new Image($src);  //实例化一个类
        $image->percent = 0.5;      //按缩放的比例来
        $image->openImage();      
        $image->thumpImage();  
        $image->showImage();  //显示图片
        $image->saveImage(md5("aa123"));  //保存图片的名称
?>  



##PHP 处理压缩图片
	<?php
	function resizeImage($im,$maxwidth,$maxheight,$name,$filetype)
	{
	  $pic_width = imagesx($im);
	  $pic_height = imagesy($im);
	
	  if(($maxwidth && $pic_width > $maxwidth) || ($maxheight && $pic_height > $maxheight))
	  {
	      if($maxwidth && $pic_width>$maxwidth)
	  {
	    $widthratio = $maxwidth/$pic_width;
	    $resizewidth_tag = true;
	  }
	
	if($maxheight && $pic_height>$maxheight)
	{
	$heightratio = $maxheight/$pic_height;
	$resizeheight_tag = true;
	}
	
	if($resizewidth_tag && $resizeheight_tag)
	{
	if($widthratio<$heightratio)
	$ratio = $widthratio;
	else
	$ratio = $heightratio;
	}
	
	if($resizewidth_tag && !$resizeheight_tag)
	$ratio = $widthratio;
	if($resizeheight_tag && !$resizewidth_tag)
	$ratio = $heightratio;
	
	$newwidth = $pic_width * $ratio;
	$newheight = $pic_height * $ratio;
	
	if(function_exists("imagecopyresampled"))
	{
	$newim = imagecreatetruecolor($newwidth,$newheight);//PHP系统函数
	imagecopyresampled($newim,$im,0,0,0,0,$newwidth,$newheight,$pic_width,$pic_height);//PHP系统函数
	}
	else
	{
	$newim = imagecreate($newwidth,$newheight);
	imagecopyresized($newim,$im,0,0,0,0,$newwidth,$newheight,$pic_width,$pic_height);
	}
	
	$name = $name.$filetype;
	imagejpeg($newim,$name);
	imagedestroy($newim);
	}
	else
	{
	$name = $name.$filetype;
	imagejpeg($im,$name);
	}
	return $name;
	}
	//使用方法：
	// 获取图片信息
	$info = getimagesize('aaa.jpg');
	// 取出后缀
	$suffix = explode('/',$info['mime'])[1];
	// 根据不同的后缀拼接不同的变量函数
	$imagecreate = 'imagecreatefrom' . $suffix;
	$im=$imagecreate("aaa.jpg");//参数是图片的存方路径
	
	$maxwidth="480";//设置图片的最大宽度
	$maxheight="480";//设置图片的最大高度
	$name="3333333";//图片的名称，随便取吧
	$filetype=".jpg";//图片类型
	$name=resizeImage($im,$maxwidth,$maxheight,$name,$filetype);//调用上面的函数
	echo $name;


##PHP怎样按比例把图片压缩小一点？一个PHP图片压缩类

经常会用到把上传的大图片压缩，特别是体积，在微信等APP应用上，也默认都是有压缩的，

压缩通常是有按比例缩放，和指定宽度压缩的，效果很不错，一个数码相机拍的4M图片，压缩后保持了较高的清晰度和原图宽高值，只有700K。

下面是本站的一个PHP图片缩放类。如果需要指定宽度和高度值的缩放，则需要另一个thumb类，thumb类已取代本类。

	<?php
 
	/**
	 * 来源：http://www.vephp.com  维易学院，
	 * 分享请保持网址。尊重别人劳动成果。谢谢。
	 * 图片压缩类：通过缩放来压缩。如果要保持源图比例，把参数$percent保持为1即可。
	 * 即使原比例压缩，也可大幅度缩小。数码相机4M图片。也可以缩为700KB左右。如果缩小比例，则体积会更小。
	 * 结果：可保存、可直接显示。
	 */
	class imgcompress{
 
       private $src;
       private $image;
       private $imageinfo;
       private $percent = 0.5;
 
       /**
        * 图片压缩
        * @param $src 源图
        * @param float $percent  压缩比例
        */
       public function __construct($src, $percent=1)
       {
              $this->src = $src;
              $this->percent = $percent;
       }
 
 
       /** 高清压缩图片
        * @param string $saveName  提供图片名（可不带扩展名，用源图扩展名）用于保存。或不提供文件名直接显示
        */
       public function compressImg($saveName='')
       {
              $this->_openImage();
              if(!empty($saveName)) $this->_saveImage($saveName);  //保存
              else $this->_showImage();
       }
 
       /**
        * 内部：打开图片
        */
       private function _openImage()
       {
              list($width, $height, $type, $attr) = getimagesize($this->src);
              $this->imageinfo = array(
                     'width'=>$width,
                     'height'=>$height,
                     'type'=>image_type_to_extension($type,false),
                     'attr'=>$attr
              );
              $fun = "imagecreatefrom".$this->imageinfo['type'];
              $this->image = $fun($this->src);
              $this->_thumpImage();
       }
       /**
        * 内部：操作图片
        */
       private function _thumpImage()
       {
              $new_width = $this->imageinfo['width'] * $this->percent;
              $new_height = $this->imageinfo['height'] * $this->percent;
              $image_thump = imagecreatetruecolor($new_width,$new_height);
              //将原图复制带图片载体上面，并且按照一定比例压缩,极大的保持了清晰度
              imagecopyresampled($image_thump,$this->image,0,0,0,0,$new_width,$new_height,$this->imageinfo['width'],$this->imageinfo['height']);
              imagedestroy($this->image);
              $this->image = $image_thump;
       }
       /**
        * 输出图片:保存图片则用saveImage()
        */
       private function _showImage()
       {
              header('Content-Type: image/'.$this->imageinfo['type']);
              $funcs = "image".$this->imageinfo['type'];
              $funcs($this->image);
       }
       /**
        * 保存图片到硬盘：
        * @param  string $dstImgName  1、可指定字符串不带后缀的名称，使用源图扩展名 。2、直接指定目标图片名带扩展名。
        */
       private function _saveImage($dstImgName)
       {
              if(empty($dstImgName)) return false;
              $allowImgs = ['.jpg', '.jpeg', '.png', '.bmp', '.wbmp','.gif'];   //如果目标图片名有后缀就用目标图片扩展名 后缀，如果没有，则用源图的扩展名
              $dstExt =  strrchr($dstImgName ,".");
              $sourseExt = strrchr($this->src ,".");
              if(!empty($dstExt)) $dstExt =strtolower($dstExt);
              if(!empty($sourseExt)) $sourseExt =strtolower($sourseExt);
 
              //有指定目标名扩展名
              if(!empty($dstExt) && in_array($dstExt,$allowImgs)){
                     $dstName = $dstImgName;
              }elseif(!empty($sourseExt) && in_array($sourseExt,$allowImgs)){
                     $dstName = $dstImgName.$sourseExt;
              }else{
                     $dstName = $dstImgName.$this->imageinfo['type'];
              }
              $funcs = "image".$this->imageinfo['type'];
              $funcs($this->image,$dstName);
       }
 
       /**
        * 销毁图片
        */
       public function __destruct(){
              imagedestroy($this->image);
       }
 
	}


使用方法：
	
	$source = 'test.jpg';
	$dst_img = 'test_111.jpg';
	$percent = 1;  #原图压缩，不缩放
	$image = (new imgcompress($source,$percent))->compressImg($dst_img);