## php如何上传超过8M的大文件
	如何上传超过8M的大文件？
	　　上传大文件主要涉及配置upload_max_filesize和post_max_size两个选项。
	　　PHP.ini配置文件中的默认文件上传大小为2M，php初学者容易犯的一个错误是在编写文件上传功能时通过设置上传文件最大大小的表单区 域，
		即允许上传文件的最大值，max_file_size（隐藏值域）的值来规定上传文件的大小，其实一般别人可以绕过这个值，所以安全起见，
		最好是在 php.ini配置文件中配置upload_max_filesize选项，设定文件上传的大小。

	　　默认upload_max_filesize = 2M，即文件上传的大小为2M，如果你想上传超过8M的文件，比如20M，你必须设定upload_max_filesize = 20M。

	　　但是光设置upload_max_filesize = 20M还是无法实现大文件的上传功能，你必须修改php.ini配置文件中的post_max_size选项，其代表允许POST的数
		据最大字节长度，默 认为8M。如果POST数据超出限制，那么$_POST和$_FILES将会为空。要上传大文件，你必须设定该选项值大于 upload_max_filesize指令的值，我一般设定upload_max_filesize和post_max_size值相等。另外如果启用 了内存限制，那么该值应当小于memory_limit选项的值。
	　　文件上传的其他注意事项
	　　在上传大文件时，你会有上传速度慢的感觉，当超过一定的时间，会报脚本执行超过30秒的错误，这是因为在php.ini配置文件中 max_execution_time配
		置选项在作怪，其表示每个脚本最大允许执行时间(秒)，0 表示没有限制。你可以适当调整max_execution_time的值，不推荐设定为0。
	　　至此，在php.ini配置文件中对文件上传选项进行配置的PHP教程就介绍完毕了，通过上面的步骤实践与学习，再结合PHP程序，文件上传功能就可以实现了。
