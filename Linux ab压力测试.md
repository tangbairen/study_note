##Linux ab压力测试
	语法：
		ab -n 1000 -c 500 -t 60 http://www.baidu.com/
	-n 连接数    -c 并发数   -t 时间（秒） url地址
##返回的信息
	Concurrency Level:      所进行的并发请求总数
	Time taken for tests:   运行所花费的总时间
	Complete requests:		模拟的请求总数中已完成的请求总数
	Failed requests:		模拟的请求总数中失败的请求数
	Total transferred:		模拟的响应中传输的总数据
	HTML transferred:		整个模拟传输的内容正文的总大小
	Requests per second:	每秒支持的请求总数		（平均值）
	Time per request: 		满足一个请求需要花费的总时间	（毫秒）
	
	Time per request:		满足所有并发请求中的一个请求需要花费总时间 （毫秒）
	Transfer rate: 			每秒收到的字节总数		（/Kb）
##目标
我们的目标是成功降低HTML transferred ,提高Requests per second 并且降低 Time per request 值


