##jQuery tmpl 模板引擎
php高级技术，php页面静态化，php缓存机制，memcached分布式对象缓存系统，数据库设计优化，mysql优化

学习网站：

	http://ju.outofmemory.cn/entry/115070
	http://www.cnblogs.com/zhuzhiyuan/p/3510175.html
	http://www.lanhusoft.com/Article/93.html

由此我们可以见得使用{{html}}来输出模版里面的内容是带有一定的风险的（XSS攻击），所以在非确定数据的安全性下最好还是使用${}来输出内容既简单又简洁
	


简单代码：

	<!DOCTYPE html>
	<html>
	<head>
	<title></title>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
		<table cellspacing="0" cellpadding="3" border="1">
		<tbody id="rows">
		</tbody>
		</table>
	
		<div id="div"></div>
	</body>
	<script src="./js/jquery-1.9.1.min.js" type="text/javascript"></script>
	<script src="./js/jquery.tmpl.min.js" type="text/javascript"></script>
	<script id="myTemplate" type="text/x-jquery-tmpl">
		<tr><td>${$data.url}</td><td>${$data.name}</td><td>${$item.getTags(';')}</td></tr>
	</script>
	<script id="mydiv" type="text/x-jquery-tmpl">
		<h1>${name}</h1>
		<p>${$item.getAge()}</p>
		<h2>
		${$item.getSex()}
		</h2>
	</script>
	<script>
		$(function () {
			var website = [{ url: 'www.phpddt.com', name: 'PHP点点通', tags: ['web教程', '博客'] }, { url: 'www.baidu.com', name: '百度', tags: ['搜索引擎', '中文搜索']}];
			//$('#myTemplate').tmpl(website).appendTo('#rows');
			$('#myTemplate').tmpl(website, {
			getTags: function (separator) {
			return this.data.tags.join(separator);
			}
			}).appendTo('#rows');
	
			var data={
				name:'hello world!',
				age:['打篮球','打羽毛球'],
				sex:['女','男']
			};
			$('#mydiv').tmpl(data,{
				getAge:function(str){
					return  this.data.age.join(str);
				},
				getSex:function(res){
					return this.data.sex.join(res);
				}
			}).appendTo('#div');
		});
	
	</script>
	</html>
