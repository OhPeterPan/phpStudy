# PHP文件中php代码优先执行！！！！

#COOKIE
	1. 概述
		服务器暂时存放在电脑中数据(.txt格式的文本)，可以让服务器辨认你的计算机，通常经过加密
		Cookie由服务器端生成，传递给浏览器让浏览器保存起来，浏览器浏览同一个网站时把Cookie发送给服务器
		Cookie的名称和值可以由服务器开发者定义
		每种浏览器都可以储存cookie，位置可能不同，各个浏览器的cookie不能共享

		通过响应头发送给浏览器
	2. 操作
		添加数据：bool setcookie($name,$value,int $expire,string $path,string $domain,bool $secure,bool $key);
		$name,$value 键值对
		$expire cookie的有效期  默认是浏览器关闭就失效了；
		$path Cookie的服务器路径
		$domain Cookie的域名
		$secure 规定是否通过安全的Https来连接传输Cookie
		$key  是否只能服务器使用该Cookie

		值只能是标量数据类型，不能是类、数组、对象...

		读取数据：$_COOKIE  超全局变量

	3. 删除
		1. 设置持久化事件为过去的一个时间戳 就可以删除该Cookie
		2. 设置值为false或者空字符串
		3. 浏览器清除Cookie	
	
	4. 配置
		就是操作setCookie除了键值外的其它几个参数
		$expire(秒)为一个时间戳 cookie的有效期  默认是浏览器关闭就失效了；一般设置就是time()+N秒，设置后就与浏览器是否关闭就无关了(持久化Cookie),将该时间设置为前面的时间戳就可以把Cookie删除了

		$path 有效路径	默认是"/"，网站根目录，在当前目录以及所有子目录的地方使用，每次连接服务器的时候，会携带大量的Cookie数据，所以需要划分路径，为对应的路径传递合适的Cookie

		$domain 域名有效性，默认只在当前域名有效,  分为主域名 一级域名  二级域名  三级域名  主域名设置Cookie，所有子域名就可以使用，子域名设置Cookie只能在子域名中使用

		$secure 设置是否只能使用安全连接发送Cookie

		Cookie不仅能使用Http发送给服务端使用，js也可以读取使用

	5. 缺点
		1. 存储在客户端，相对不安全
		2. 存储的数据类型只有字符串
		3. 存储的数据容量有限，大概4kb
		4. 浏览器可以禁用缓存就可以禁用Cookie，则Cookie就会失效

# SESSION
	1. 概述
		1. 存储特定用户的对话数据
		2. 将会话数据存储在服务器中
		3. 基于Cookie技术，没有Cookie就没有Session
		4. 在整个用户会话中，一直存在下去
		5. 一个用户会话时效：登陆开始到用户登录结束
		6. 存储数据量比Cookie大
		7. 存储的数据类型不限于字符串
		8. 数据存储在服务器，更安全可靠		

	2. 操作
		开启：bool session_start(void)  开启SESSION支持，开启之后$_SESSION才能使用 只能开启一次，所有需要session的地方都需要开启
	
	3. 删除
	  删除内容：
		unset($_SESSION[key]);删除$_SESSION的数据，但是删除后文件依然存在；直接删除$_SESSION无效
		$_SESSION=array();删除$_SESSION的数据，把整个文件内容全部清除

	  删除文件
		seesion_destroy();//删除文件

	4. 配置
		SESSION对应的COOKIE的配置(php.ini)	

		修改SESSION对应COOKIE过期时间
			session.cookie_lifetime

		修改SESSION对应COOKIE的有效路径
			session.cookie_path

		修改SESSION对应COOKIE的有效域名
			session.cookie_domain

		修改SESSION对应COOKIE是否开启安全连接
			session.cookie_secure

		修改SESSION对应COOKIE是否仅限服务器使用
			session.cookie_httponly

	5. 垃圾回收
		服务器会自动删除过期的session文件

		php_ini文件：
			1. 垃圾回收周期 session.gc_maxlifetime 默认24分钟清理一次
			2. 垃圾回收概率  session.gc_divisor  分子/分母 得到一个百分比  达到100%就会清理一次

		原理 以24分钟  1/1000 (即人数是否达到1000)
			时间达到24分钟，人数是否达到1000，如果达到1000就清理，达不到就不清理

# 示例 我的相册
	查找伪装文件类型打开fileinfo扩展 php.ini里面开启  严格检查文件类型