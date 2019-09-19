# php操作数据库
	1. 连接数据库系统
		语法：mysqli_connect([host],[username],[pwd],[dbname],[port]); 得到一个连接对象
	2. 屏蔽系统错误 @
		如果能从某处获取值，就在前面加上@运算符
		可以把它放在 变量、函数和include 调用 之前，不能放在函数或者类定义之前，也不能用于条件结构之前

	3. exit()或者die() 终止当前的执行
		输出一个信息并且退出当前的脚本

	4. mysqli_connect_error()  无参数有返回值
		返回上一个MySql连接产生的文本错误信息

	5. mysqli_close(mysqli $link); 
		关闭先前打开的数据库连接

	6. 选择一个数据库 mysqli_select_db(连接对象,dbname);
	
	7. 设定字符集 mysqli_set_charset(连接对象,字符集);

	8. 执行查询语句 mysqli_query(连接对象,查询语句);
		仅对select、show、describe语句返回一个结果集对象(mysqli_result结果集)，其他的成功返回true，失败返回false
	
	9. 释放结果集对象(其实就是释放内存)
		mysqli_free_result(mysqli_result $result) 手动销毁变量		
		内存中的变量何时消失：
			1. 网页执行完成后，所有与本页面有关的对象将被销毁
			2. 手动销毁内存中的变量
		
	10. 从结果集里面取出一行数据 取出一个枚举数组(下标是整型的是枚举数组，下标是字符的属于关联数组)
		mysqli_fetch_row(mysqli_result $result) 没调用一次函数就取出一行，指针会自动下移

	11. 从结果集中取出一行数据作为关联数组
		musqli_fetch_assoc(mysqli_result $result);  返回的字段对大小写敏感
			
	12. 从结果集中取出一行数据作为关联数组、或枚举数组、或者二者兼有
		mysqli_fetch_array(mysqli_result $result,[int $tag=MYSQLI_BOTH(默认)|MYSQLI_ASSOC|MYSQLI_NUM]);	
		根据tag变量取出对应的数组格式，两者都有、关联数组、枚举数组(又称为数字索引数组)
		该函数返回字段区分大小写

	13. 从结果集中获取多行数据 获取的是二维数组
		mysqli_fetch_all(mysqli_result $result,[int $tag=MYSQLI_BOTH(默认)|MYSQLI_ASSOC|MYSQLI_NUM]);

	14. 获取查询的记录数  此命令仅对select语句有效
		mysqli_num_rows(mysqli_result $result); 获取结果集中的行数
		
	15. 获取前一次mysql操作所影响的记录行数
		mysqli_affected_rows(mysqli $link); 如果最近查询失败返回-1


# php操作目录与文件
## php操作目录
	目录管理：复制，移动，创建，重命名...
	文件管理：创建，复制，移动，重命名，读取，打开
	文件上传：单文件上传，多文件上传，上传文件夹
	
	1. 创建新目录 
		mkdir(string $pathname,[int $mode=0777,[bool $recursive=false]])
		0777代表最大可能的访问权限  $mode是八进制 windows下被忽略
		$recursive 如果指定的上级目录不存在，则也会递归创建	

	2. 判断是否是一个目录 可以用来判断是文件还是目录
		is_dir(string $pathname);

	3. 判断目录或者文件是否存在
		file_exists(string $filename);

	4. 删除目录
		rmdir(string $pathname);  目录必须是空的，而且要有权限
	
	5. 更改目录的访问权限 该函数不能用于远程文件 windows设置没啥用 linux专用
		chmod(string $pathname,int $mode)  mode是八进制数据
		1表示文件可执行    2表示可写   4表示可读  竟然是相加得来的权限
		window文件只读权限0444   目录的是0555

	6. 获取文件权限值
		fileperms(string $filename);   以十进制数据返回

	7. 目录名更改或者移动
		更改目录或者文件名称，原目录必须存在
		如果原目录与新目录在同一个父目录下，认为是改名
		如果原目录与新目录不在同一个父目录中，则认为是移动
		rename(string $oldname,string $newname);
		
	8. 打开目录
		resource opendir(string $pathname); 成功返回目录句柄的resource(相当于一个中介，php与操作系统操作的中介) 失败返回false
	
	9. 读取目录的条目 从中介资源里面取出文件数据
		string readdir([resource $dir_handle]);	返回目录中下一个文件的文件名，是以文件系统的排序返回

	10. 字符集转换
		string iconv(string $in_charset,string $out_charset,string $str);返回转换后的字符串
		$in_charset 输入字符集
		$out_charset 输出字符集
		$str 原始字符集

	11. 关闭目录句柄 
		closedir([resource $handler]); 关闭目录句柄

	12. 打开文件
		resource fopen(string $filename,string $mode);
		为了移植考虑，打开文件时总用'b'标识，
		根据不同模式打开一个文件
		$mode: 带'+'的都是读写 全部加上b 比如'rb'
			1. 'r' 只读   指针放在内容的开头
			2. 'r+' 读写方式打开文件 指针放在内容的开头
			3. 'w' 可写方式打开 文件指针指向开头并将文件大写截为0，文件不存在，尝试创建
			4. 'w+' 可读写方式打开文件  文件指针指向开头并将文件大写截为0，文件不存在，尝试创建
			5. 'a' 写入方式打开文件，文件指针放到末尾，文件不存在尝试去创建
			6. 'a+' 读写方式打开文件 ，文件指针放在末尾，文件不存在尝试去创建
			7. 'x' 只写，并创建文件，如果文件已经存在,fopen调用失败并返回false
			8. 'x+' 可读写，并创建文件，如果文件已经存在,fopen调用失败并返回false
	
	13. 关闭文件
		fclose(resource $resource); 

	14. 读取指定大小的文件内容（可安全地用于二进制文件）
		string fread(resource $res,int $length);
		$length 最多读取的字节数 

	15. 获取文件大小，以字节为单位返回
		filesize(string $filename);

	16. 一行一行读取 碰到换行符和文件结束符都会停止读取数据
		fgets(resource $res,[int $length]); 一次读一行，适用于读取记事本信息

	17. 读取文件内容到数组不需要打开或者关闭文件
		array file(string $filename,[int $flags]);
		$flags:
			FILE_USE_INCLUDE_PATH(1);在include_path中查找文件
			FILE_IGNORE_NEW_LINES(2);在数组的每个元素末尾不添加换行符
			FILE_SKIP_EMPTY_LINES(4);跳过空行

	18. 读取文件内容到字符串  一次读一整个文件
		string	file_get_contents(string $pathname);

	19. 将一个数据写入到文件 文件不存在会自动创建;文件过大，一次处理不了
		file_put_contents(string $filename,mixed $data[,int $flags]);
		$filename:要写入的文件名
		$data:写入的数据 可以是string、一维数组
		$flags 附加选项

	20. 写入文件  可以写入字节(图像、视频.....)
		int fwriter(resource $res,string $string);//返回写入的字节长度

	21. 复制文件
		copy(string $souce,string $dest);如果目标文件已经存在，则会覆盖

	22. 删除文件
		unlink(string $filename); 删除的文件不会进入回收站

	header()函数：告诉浏览器应该怎么做
# 数据库分页与HTTP原理
## 数据分页
	公式 limit (n-1)*pageSize,pageSize;

## HTTP协议相关
	特点：简单快速、灵活(HTTP允许传输任意类型的数据对象)、无状态、无连接
	1. 请求
		请求行，请求头，请求空行，请求体
		HTTP1.0 短连接(请求完成后立即关闭连接)  HTTP1.1 长连接(请求完成后会有一个等待时间，到时间后断开连接)
	<img><script><link><frame>会自动向服务器发送请求
	
	2. 响应
		状态行、响应头、响应空行、响应体
		响应结果码： 
			1xx：已经接收到数据，继续处理
			200 成功
			3xx：重定向-完成请求需要进一步的操作 302-临时重定向，不需要配置服务器，php代码中修改就行
			 	304-不从服务器访问数据，使用缓存
				301-域名完全更改，需要配置服务器，
			4xx：客户端错误-比如404-资源未找到 400-当前请求无法被服务器理解 403-连接却被拒绝访问
			5xx：服务端错误-服务端未能实现合法的请求 500-服务器发生不可预知的错误 503-服务器当前不能处理请求，稍后可能恢复

## 下载文件
	header('Content-Type:application/octet-stream');//告诉浏览器内容类型为8位二进制数据流
	header('Content-disposition:attachment;filename="文件名字;"'); //告诉浏览器处置方式，以附件的形式

	
			
			
			