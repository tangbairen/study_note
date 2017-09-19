##php 获取指定月第一天和最后一天

借助于date和strtotime函数，可以轻松的获取本月、下月以及上月的第一天和最后一天，下面分别给出其实现。其中函数的参数date格式为yyyy-MM-dd。

1、给定一个日期，获取其本月的第一天和最后一天


	function getCurMonthFirstDay($date) {
	    return date('Y-m-01', strtotime($date));
	}
	function getCurMonthLastDay($date) {
	    return date('Y-m-d', strtotime(date('Y-m-01', strtotime($date)) . ' +1 month -1 day'));
	}

2、给定一个日期，获取其下月的第一天和最后一天

	function getNextMonthFirstDay($date) {
    	return date('Y-m-d', strtotime(date('Y-m-01', strtotime($date)) . ' +1 month'));
	}
	function getNextMonthLastDay($date) {
	    return date('Y-m-d', strtotime(date('Y-m-01', strtotime($date)) . ' +2 month -1 day'));
	}

3、给定一个日期，获取其下月的第一天和最后一天

	function getPrevMonthFirstDay($date) {
	    return date('Y-m-d', strtotime(date('Y-m-01', strtotime($date)) . ' -1 month'));
	}
	function getPrevMonthLastDay($date) {
	    return date('Y-m-d', strtotime(date('Y-m-01', strtotime($date)) . ' -1 day'));
	}


php计算两个日期相隔多少年,多少月,多少日的函数
	
	/*
	*function：计算两个日期相隔多少年，多少月，多少天
	*param string $date1[格式如：2011-11-5]
	*param string $date2[格式如：2012-12-01]
	*return array array('年','月','日');
	*/
	function diffDate($date1,$date2){
		if(strtotime($date1)>strtotime($date2)){
			$tmp=$date2;
			$date2=$date1;
			$date1=$tmp;
		}
		list($Y1,$m1,$d1)=explode('-',$date1);
		list($Y2,$m2,$d2)=explode('-',$date2);
		$Y=$Y2-$Y1;
		$m=$m2-$m1;
		$d=$d2-$d1;
		if($d<0){
			$d+=(int)date('t',strtotime("-1 month $date2"));
			$m--;
		}
		if($m<0){
			$m+=12;
			$y--;
		}
		return array('year'=>$Y,'month'=>$m,'day'=>$d);
	}