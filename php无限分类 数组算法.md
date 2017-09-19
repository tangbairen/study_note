##PHP无限分类 数组算法

	<?php
		header("Content-type: text/html; charset=utf-8");  
		$items = array(
		    1 => array('id' => 1, 'pid' => 0, 'name' => '水果'),
		    2 => array('id' => 2, 'pid' => 0, 'name' => '数码'),
		    3 => array('id' => 3, 'pid' => 1, 'name' => '书籍'),
		    4 => array('id' => 4, 'pid' => 3, 'name' => '酒水'),
		    5 => array('id' => 5, 'pid' => 1, 'name' => '手机'),
		);
	
	
		function getArray($array,$pid){
	
			if(empty($array)){
				return null;
			}
			$arr=[];
			foreach($array as $key=>$val){
				if($val['pid'] == $pid){ //查找顶级父类
					$val['children']=getArray($array,$val['id']);
	
					if($val['children'] == null){
						unset($val['children']);
					}
	
					$arr[]=$val;
				}
	
			}
	
			return $arr;
		}
	
		function demo($array){
			
		}
	
	
	
		$data=getArray($items,0)
		
	
	?>

##
	<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>Document</title>
	</head>
	<body>
		<form action="" style="margin-bottom: 500px;">
	
			<select name="" id="" style="width: 200px;">
				<?php  foreach($data as $key=>$val): ?>
					<option value=""><?php echo $val['name'];?></option>
					<?php foreach($val['children'] as  $k=>$v):?>
					<option value="">&nbsp;&nbsp;|<?php echo str_repeat(' - ', 5).$v['name'];?></option>
					<?php endforeach;?>
				<?php endforeach;?>
			</select>
	
		</form>
	</body>
	</html>