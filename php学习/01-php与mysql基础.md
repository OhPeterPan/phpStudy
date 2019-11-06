#php基础语法
	. 在php中属于拼接
	php是一门弱类型的语言

###php输出
	echo              粗糙的输出信息
	var_dump(变量名);  可以输出一个变量的完整信息
### php标记
	<?php ?> 推荐写法
	<script language="php"> php代码 </script>
	<? php代码  ?>   php.ini里面配置  short_open_tag=On;

### 变量
	1. 变量定义   $开头定义变量
	2. 判断变量是否有值  isset(变量名)  返回true或者false
	3. 删除/销毁变量         unset(变量名)  
	4. 变量操作  值传递和引用传递
		值传递    $v1=$v2;     
		引用传值  $v1=&$v2;   &是引用符号

### 预定义变量
	php中有几个预先定义的变量，可以直接拿来使用
	1. $_GET   接受get方法过来的数据  自动保存提交的数据  得到的是一个数组
	2. $_POST  接受post过来的数据
	3. $_REQUEST  获取get或者post过来的数据合集(既能得到get的数据，也能得到post的数据)   优先使用post和get的变量获取数据集
	4. $_SERVER  代表每次请求中，客户端或者服务端的一些基本信息  REMOTE_ADDR  客户端的IP地址  $_SERVER['REMOTE_ADDR']
	
#### 同时提交get和post数据(只有这一种方法)
		<form action="06-postandget.php?id=10&username=赵三" method="post">
		</form>

### 可变变量
	变量名又是一个变量的变量
	示例  $v1=10;   $v2="v1";    echo $$v2;   $$v2就是一个可变变量

###常量
	不会变化的值

	* 定义方式
		1. define('常量名' ,对应的常量值)   define("v3",34);  定义一个常量v3
		2. const 常量名=对应的常量值        const v3=34;  
		  
	* 取值方式
		1. echo 常量名;
		2. echo constant("常量名");  //此时常量名应该使用引号引起来
	
###预定义常量  php中已经设置好值的常量
	PHP_VERSION  获取php当前的版本号
	PHP_OS       获取当前php运行在那个系统中
	PHP_INT_MAX  当前php中最大的整数值
	M_PI         表示圆周率

###魔术常量  也是常量，不过是对比不同的文件值有所变化
	__DIR__  当前php文件所在的目录路径
	__FILE__ 当前php文件的路径  
	__LINE__ 这个常量所在的行号

###常量与变量之间的区别、以及常量的判断

	* 区别
		1. 常量值不能改变，变量的值可以改变
		2. 常量只能存储基本数据类型，变量可以存储任何数据类型
		3. 变量赋值可以是计算出的值，常量只能直接赋值   示例  $v1=10; $v2=3; $v3=$v1+$v2;   const v3=1+2;(错误写法)
		
	* 判断  defined('常量名')  返回布尔值

## php数据类型
	string、int、float、boolean、array、obj、NULL

	js数据类型：Number、string、boolean、undefined、null、array、function、obj

###整型
	php中进制定义  echo输出时 是以十进制的形式打印出来的
		1. 2进制 以0b开头		$v=10101010
		2. 8进制 以0开头		$v=0603
		3. 16进制  0x开头	$v=0X12A   
		
	进制转换  直接使用函数
		1. decbin()  10->2
		2. decoct()  10->8
		3. dechex()  10->16
		4. bindec()  2->10
		5. octdec()  8->10
		6. hecdec()  16->10
		
### 浮点型  
	就是小数     运算时考虑精度损失情况   不要直接做'=='比较运算
	先找出一个精度值，然后乘以对应精度的1*10^n  然后使用round()函数(四舍五入)   

	更高精度计算
		


### 布尔型
	true、false

	以下情况会被当成false  
		false、0、0.0、""、null、'0'、空数组
	
	js中会被当成false的情况
		undefined、null、false、0、NaN(但是 typeof NaN =="number")、""或者''(也就是空字符串)
	
### 字符串类型  使用 . 来拼接
	单引号或者双引号引起来的数据表示
		
	字符串双引号里面含有"$"时，当前变量使用,如果变量未定义，会报错;单引号没有这种；可以使用转义字符“\”
	
	模板代码  $v1=10;  echo "$v1";//输出10  谨记只对应双引号，单引号只表示一个字符
	
### 数组类型array  多个数据放在一起，有序序列
	数组定义
		1. $v=array('张三丰',10,"男");
		2. $v=array('name'=>'张三丰', 'age'=>18,"gender"=>"男");
		3. $v=['张三丰',10,"男"];
	
	js数组定义
		1. let array=[1,2,45,67,345]
		2. let array=new Array();
		3. let array=new Array(4);
		4. let array=new Array(3,4,56,234,67,6734);
		
### 类型判断
	gettype()  获取变量的类型名称
		$v1=1;  gettype($v1); ------>integer
		$v2='abc';  gettype($v2); ------>string
		$v3=1.23;  gettype($v3); ------>double

	settype()  设置变量的类型
		$v1=10;   settype($v1,'string');//设置成string类型

	var_dump() 获取变量的具体信息  包括：类型、长度、内容

	判断是否是某种类型
		1. is_int()
		2. is_float()
		3. is_bool()
		4. is_string()
		5. is_array()
		6. is_numeric()  //是否是数字类型(包括整型、浮点型、纯数字字符串)
		7. is_object()
		8. isset()  变量是否存在，或者是否有值
		9. isempty() 判断是否是空的   false、0、0.0、""、null、'0'、空数组都是空的

### 类型转换
	自动转换和强制转换

### 运算符
	1. 赋值运算符 =
	2. 算数运算符  + - * /  %(取余); 弱类型，所以做加减乘除必定有值  $v="a"+"b";//0  $v1=1+true;//2  true转为1,false转为0
	3. 自赋值运算符 += -= *= /= %= .=
	4. 连接运算符 .  连接字符串，连接的不是字符串的时候，自动转成字符串
	5. 比较运算符 > < <= <= == != === !==
	6. 逻辑运算符 && || !
	7. 条件运算符  数据1? 数据1：数据2; 三元运算符
	8. 位运算符  |(按位或)  &(按位与)  ^(异或 相同为0 不同为1)  ~(取反)  <<(按位左移)  >>(按位右移)
	9. 错误抑制符  @ 当一个表达式出现错误的时候，可以将错误隐藏起来，不让其输出错误；应用在我们未知的错误
		
###数据流程
####三大流程结构
	1. 顺序结构
	2. 分支结构 if elseif    switch
	3. 循环结构
	
###函数    跟js极其相似
php函数的定义

		function 函数名(形参1,形参2,形参3......){函数体语句块}
		function 函数名(&形参1.....)  引用传递只能使用变量传递，不能使用数据传递
		引用传递的形参，必须对应实参变量，不能直接传递数据

php不支持方法重载

函数执行原理

		程序总是运行在一个内存空间，程序开始执行位置所在的空间，可以称为“主运行空间”，实际上，通常函数外面所在的空间都属于主运行空间，那么，函数的运行，就相对独立---函数的每次调用，都运行在自己的空间中

	形参的本质是变量    实参的本质是数据

	形参有值传递和引用传递(&)两种形式

	形参可以使用默认值，使用默认值的形参，只能放在没有默认值形参的后边

函数返回值  return

匿名函数    

	示例 	$v=function (){         或者      (function (){ echo '可以输出吗？',"<br>"; })();
			echo '可以输出吗？';
				};
				$v();

#### 变量作用域
	1. 作用域有三种：局部作用域   全局作用域   超全局作用域
	2. 变量： 局部变量   全局变量   超全局变量

	static修饰的变量称为静态变量，作用在函数中时，不会因为函数的运行结束而被销毁
	
	全局变量：在函数外面定义的变量
	
	超全局变量：其实只有系统预定义的几个变量属于超全局变量  $_GET、$_POST......这些


	$GLOBALS 数组，里面存储了我们定义的所有的全局变量  $v1=10;$v2=20;$v3=30;  执行之后$GLOBALS 就会存在变量，值也是赋值的值

	global 关键字 用于局部变量，当修饰的局部变量与全局变量名字相同时，可以使用全局变量的值----处于引用关系
	
### 跟函数有关的函数
		1. function_exists("函数名");//判断一个函数是否存在
		2. func_get_arg(n); //在函数内部可用，用于获得第n个实参(n从0开始算起)  类似于数组的下标
		3. func_get_arg（）；//在函数内部可用，用于获取所有的函数实参，结果是一个数组
		4. func_num_args() ;//函数内部使用，获取实参的个数

### 常用时间函数
		1. time();//获取得秒数  1970.1.1 0:0:0 到目前的一个秒数
		2. microtime();//获取当前的秒数(可以是微秒)   970.1.1 0:0:0 到目前的一个秒数
			两种用法:	microtime(true); //获取秒数，精确到后四位
					  	microtime(false);//获取字符串秒数，因为精度太高，浮点数无法表达，就转成了字符串形式
		3. mktime();//创建一个时间数据，参数：时、分、秒、月、日、年
		4. date();//将一个时间转成字符串形式
		5. idate();//获取某个时间某个单项数据值 比如 idate("Y");取得年份
		6. strtotime();//字符串转为时间值
		7. date_default_timezone_set();//代码中设置时区
		8. date_default_timezone_get();//代码中获取时区

### Math函数
	1. max();//找出最大值  两个参数
	2. min();//找出最小值  两个参数
	3. round();//对浮点数四舍五入
	4. ceil();//获取大于参数的最小整数(包含自身)
	5. floor();//找出小于参数的最大整数(包含自身)
	6. abs();//绝对值
	7. rand();//获取随机数 参数属于两个可选
	
### 递归 (原来都有递归) 
	函数自调用，必须有一个结束条件，否则会崩溃

### 文件加载
	将一个文件加载到另一个文件中

	include "要加载的文件路径";//可以是相对路径，也可以使用绝对路径 可以是php文件，也可以是 html文件 可以直接使用include文件中的内容

#### 文件加载的四种方式
	include "";//每次都载入，有可能会重复载入，报错之后会继续执行
	include_once "";//只载入一次，有可能会重复载入，报错之后继续执行
	require "";    //每次都载入，可能会重复载入，如果报错就会停止执行程序
	require_once "";//只载入一次，有可能会重复载入，报错之后会停止执行程序

	如果载入的文件如果后边需要使用，最好使用require 	
	如果载入的文件只需要使用一次，使用XXXX_once就好	

	获取物理路径的方式
		__DIR__;//获取当前的文件所在的路径
		
		getcwd();//获取当前正访问的网页路径   这两个路径有时候不太一样  谨记，一个是当前文件所在的路径，一个是正访问的网页路径

### 错误处理
#### 错误分类
	1. 语法错误    
	2. 运行时错误  
	3. 逻辑错误    其他的没问题，但是得到的值是错的

#### 错误代号  都是系统常量  有一个值
	可以修改php.ini 中的 error_reporting 决定显示那些错误

	自定义触发错误  人为触发
	trigger_error("错误的信息",错误代号); //错误代号只只能为  E_USER_ERROR、E_USER_WARNING、E_USER_NOTICE（默认）
  

	错误显示控制
		1. php.ini 中设置 
		   display_errors=on/off 是否开启错误显示
		2. 代码中设置  ini_set("display_errors",0/1); 
	
	错误日志 开启
		1. php.ini 中设置
		   log_errors=On/Off

		2. 代码中设置 ini_set("log_errors",0/1);
		
	错误日志 记录位置
		1. php.ini 中设置 
		   error_log=error_log.txt

		2. 代码中设置 ini_set("error_log","错误文件名字");

#### 自定义错误处理
	步骤
		1. 设置发生错误时，由我们自己处理 --设定一个错误处理的函数名
		2. 定义该函数，在函数里详细设定错误的处理情况：显示什么、怎么显示、怎么记录记录什么
	示例
		set_error_handler("my_handler_error");//第一步：声明一个函数 set_error_handler("自己处理错误的函数名字")
			//第二步：定义该函数
			/**
			$errorCode 错误代码
			$errMsg 错误信息
			$errrFile 错误信息放置文件
			$errLine 错误行号
			
			形参顺序固定，由系统调用这个函数
			*/
			function my_handler_error($errorCode,$errMsg,$errrFile,$errLine){
		
				//可以记录到一个本地文件中
			}
	切记！！！只能处理非致命错误(就是不是“E_ERROR”的错误)！！！！！！

## 字符串	
### 定义方式(4种)
	1. 单引号  只支持两个转义字符  \\ 代表一个反斜杠    \' 单引号 一般没有特殊字符需要，推荐使用 单引号定义字符串
	
	2. 双引号  支持大多数转义字符 \\  \"   \r\n  \t(table)  \$(就是代表一个"$") "$v1"//代表读取一个变量，单引号没有
	
	   ""里面想要读取"$变量"时，尽量使用{$变量}包裹

	3. heredoc字符串 $v1=<<<"自定义标识符" 这里写内容 标识符; 特点跟双引号字符串相似  很奇特，标识符前面需要双引号，尾部不需要

	4. nowdoc字符串  $v1=<<<'标识符' 这里写内容 标识符; 无特点，最纯净的字符串，写什么就是什么，不需要特殊处理
	
### 字符串长度问题 一个英文字符一个字节，中文gbk 占两个字节  utf-8占三个字节 
	strlen(字符串);//获取字节长度 
	mb_strlen(字符串);//获取字符长度  使用需要开启php.ini中 extension=php_mbstring.dll

	header("content-type:text/html;charset=gbk");//设置php当前文件编码格式

### 字符串常用函数
	1. 输出
		echo       只能输出字符串  数组无法直接输出
		print      基本不用
		print_r    输出变量的基本信息
		var_dump():输出完整变量信息
	
	2. 字符串函数  查手册吧....
		trim ：消除字符串两端的空格符 不包括中间
		st_pad:讲一个字符串使用一个指定的字符填充到指定长度

	3. 特殊字符处理
		nl2br：将换行符转换为"<br/>"标签字符
		addslashes:将一个字符串中的以下几个字符使用反斜杠进行转义：\ ' "
		htmlspecialchars:将html中的特殊字符转换为html实体字符(& ' " < >)-->(&amp; &apos; &quot; &lt; $gt;)
		htmlspecialchars_decode:将html实体字符转化回原本的字符

## 数组
### 数组定义方式
	1. $v=[元素1,元素2,元素3,....];
	2. $v=array(元素1,元素2,元素3,....);
	3. $v=['a'=>1,'b'=>2,......]
	4. $v=array('a'=>1,'b'=>2,......);

### 数组遍历 foreach
	foreach(数组 as $value){};  foreach(数组 as $key=>$value){};  

### 数组的循环操作(非for) 有几个函数  数组指针默认在第一个
	1. current($array);//取出当前指针所在单元的值
	2. prev($array); //指针向前移动一位 取出所在单元的值
	3. next($array);//指针向后移动一位，取出值
	4. end($array);//指针跳到最后并取出当前位置的值
	5. reset($array);//指针跳到第一个并取出当前位置的值
	6. key($array);//取出当前的键 索引
	
### php数组算法
	1. 排序算法  冒泡排序、选择排序
	2. 查找算法  二分查找算法(条件：1、索引数组 2、针对已经排序好的数组)


# mysql基础   mysql内部竟然还有函数.....
	命令行: 开启数据库服务 net start 数据库名字    关闭数据库服务 net stop 数据库名字
	连接数据库： mysql -h 主机地址(数据库所在地址) -u 用户名 -p  cmd登录后立即使用“set names gbk;”来设置连接编码
	退出数据库：quit、exit、\q
	
	常用客户端：myadminphp

## 数据库操作语句  
	常用名词： DBMS 数据库管理系统  DB 数据库   table 表  column 列  row 行   filed 字段
	导入.sql文件语句: source .sql文件路径;

	1. show databases;  //展示所有数据库
	2. show charset;  //展示所有得字符集 
	3. show collation;//查看可用的校对规则 排序规则
	4. show create database 数据库名;//查看数据库创建信息
	5. create database 数据库名 charset 字符集名称;//创建数据库  字符集推荐utf8
	6. drop database 数据库名;//删除数据库
	7. alter database 数据库名 charset 新的字符集名称 collate 新的校对规则;//修改数据库的字符集以及校对规则
	8. use 数据库;//使用某个数据库

## 数据库表的操作  数据表才是存储数据的具体容器 []包括的代表可写可不写 
	1. create table 数据表名(字段1,字段2,..) [charset=字符集] [engine=表类型(InnoDB,MyIsam...默认InnoDB)];// 创建数据表 
		字段定义: 字段名 字段类型 [字段属性...]    

	2. show tables;//展示所有数据表
	3. desc 表名;//查看表结构
	4. show create table 表名;//查看创建表的语句
	5. drop table 表名;//删除数据表
	
## 修改表的语句
	1. alter table 表名 add 字段名 字段类型 [字段属性....] [after 某字段名 或者first];//添加字段
		after 在某个字段的后面添加新字段
		first 新加的字段放在第一位

	2. alter table 表名 change 旧字段名 新字段名 字段类型 [字段属性...];//修改字段
	3. alter table 表名 modify 要修改的字段  字段类型 [字段属性...];//仅仅修改字段其他的信息
	4. alter table 表名 drop 要删除的字段; //删除字段
	5. alter table 表名 rename 新的表名;//修改表名
	6. alter table 表名 charset=新的字符集;// 修改表的字符集

## 数据表的增删改查(CRUD)  增(insert)删(delete)改(update)查(select)
	1. insert into 表名 (字段名1,字段名2,....) values (数据1,数据2,.....);//插入数据
		1. 字段数量与数据要一一对应
		2. 日期和字符串需要使用单引号引起来
		3. 可以省略字段，但是需要数据数量与字段数量一一对应 insert into 表名 values (数据1,数据2,.....);
		
	2. select 字段1,字段2,..... from 表名 [where 条件]; //根据where条件查询数据
	3. delete from 表名 [where 条件]; //根据where条件删除数据，不写where删除所有数据
	4. update 表名 set 字段名 1=新值1,字段名2=新值2,..[where 条件];//更新字段的值

### select 高级查询 
	1. 一个完整的查询语句   下边即是查询顺序
		select语句 [from() 语句 ] [where(条件)语句] [group by(分组条件)语句] [having(条件筛选)语句] [order by(排序)语句] [limit(限制结果数量)语句];  as  设置别名
	
	2. select 计算  select 1+1;  select round(6.7);  可以计算
	3. distinct 消除查询结果重复行  两行以及两行数据完全一样，就是重复行  
		示例：select distinct 字段 from 表名  消除字段值重复的行

	4. as 设置别名   select 1+1 as f1,3+4*5 as f2;  字段名变为f1 f2
	
	5. where 子句； 
		where 条件  支持算数运算符、逻辑运算符(&&或and ||或or、！或not)、比较运算符；

	6. like 模糊查找运算符;//判断某个字符型字段的值是否包含给定的字符
		语句形式：XXX字段 like "%关键字%";  %:表示任意个数的任意字符  _:表示任意一个字符
		示例： select pro_name,pinpai from product where pro_name like '%智能%';

	7. between 范围限定运算符  
		XXX字段 between 值1 and 值2;等同于  XXX字段>=值1 and XXX字段 <=值2;
	8. in运算符；  形式：XXX字段 in(值1,值2,值3.....); 只要是其中的一个值就算满足条件
	9. is运算符; 判断一个字段中是否存在 (就是有没有)  
		形式： XXX字段 is null;或者 XXX字段 is not null; 只有这两种情况

	10. group by;对于所查询出来的数据根据某些字段进行分组,最后的结果是将结果分成若干组，每组作为一个整体成为一行数据
		注意：若干行数据分成若干组，一行数据代表一个组的概念吗，操作是也是对组进行操作；因此操作时一行中出现的也应该是组的改变而不是个体的信息
		支持函数：(聚合函数)
			count(字段) 一组的个数
			avg(字段)   一组中该字段的平均值
			max(字段)   一组中某个字段的最大值
			min(字段)   一组中某个字段的最小值
			sum(字段)   一组中某个字段值的总和
			
		语法形式：group by 字段1,字段2,字段3.....;
		示例：select pinpai,count(*) as 数量,avg(price) as 平均价 from product group by pinpai;
		
	11. having 子句;//与where类似 	形式：having 筛选条件  只用于group by之后的数据操作	
		示例：价格大于3000的品牌分组之后然后平均值大于5000的数据;
			select pinpai,avg(price) as 平均价 from product where price>3000 group by pinpai having 平均价>5000;

	12. order by;排序  
		语法格式:order by 字段1 [asc或desc],字段2 [asc或者desc] asc正序(默认值)   desc倒序

	13. limit ; 限制结果数量
		语法格式: limit 起始行号,行数; 从零开始  
		一般用于分页取数据 找出第n页数据: 公式 limit (n-1)*pageSize pageSize;  pageSize每页数据

### 高级插入语法
	1. 多行插入 insert into 表名(字段1，字段2，字段3...) values(值1，值2，值3......),((值1，值2，值3......)
	2. 插入查询的结果语句;//将查询过得结果插入到一个表中
		语法格式：insert into 表名(字段名1,字段名2,字段名3....) select xx1,xx2,xx3...字段需要一一对应 

	3. set语法插入数据
		语句格式：insert into 表名 set 字段1=值 ，字段2=值2.....; 

	4. 蠕虫复制
			就是将一个表中的数据，进行快速地复制并插入到所需要的表中，以期在段时间内具有大量数据，用于测试或者其他场合
			1.将一个表中的大量数据，复制到另一个表中
			2.将一个表中的大量数据，复制到本身表中以产生大量的数据

### 高级删除
	1. 带条件的删除语句  delete from 表名 where ... [order by xxx] [limit 数量n] ;排好序之后删除n行数据
		表会记录主键最大的id值，删除后再插入还是从最大值id开始递增  对比主键

	2. trancate 表名;  清空表 会清除主键的值，每次都会从1开始

### 高级更新
	update 表名 set 字段1=值1,字段2=值2.....[order by ] [limit 前几项];

### 主键冲突解决方法
	1. 忽略 终止插入，数据不改变
		示例：insert ignore into 表名(字段1，字段2，字段3....) values(值1，值2，值3....);

	2. 替换 删除原有纪录，插入新纪录
		示例：replace into 表名(字段1，字段2，字段3....) values(值1,值2,值3...);

	3. 更新 更新原有数据
		示例：insert into 表名 (字段....) values(值...) on duplicate key update XX字段=新的值;

## 数据库数据类型
	auto_increment 自增长     primary key;主键

### 整型
	1. tinyint 微整型	1byte   范围 0开始：0 ~ 2^8-1    负数开始 -((2^8-1)/2) ~ (2^8-1)/2
	2. smallint 小整型	2byte 
	3. mediumint 中整型	3byte 
	4. int 整型			4byte 
	5. bigint 大整形		8byte 

### 小数类型
	1. 浮点型小数
		1. float  单精度  4byte  约7位有效数字
		2. double 双精度	 8byte 	约17为有效数字

	2. 定点型小数  定义为精确地小数，突破有些小说无法使用二进制精确表示
		定义：decimal(M,D);  M表示该小数总的有效位数，D表示该小数小数点后的有效位数
	  
### 时间类型	 时间类型的字面值 通常需要用单引号引起来	 
	1. date  日期   格式：'0000-00-00'	
	2. time  时间   格式：'00：00：00'
	3. datetime 日期加时间：'0000-00-00 00：00：00'
	4. timestamp 时间戳(整数数据)  1970-01-01 0:0:0到目前时间的秒数 无需添加或者更新，自己会自动加入或者改变时间(会自动记录更新时间) insert或者update的时候会自动获取时间
	
	5. year  年数  '0000'

### 字符类型   1byte = 8bit  11111 1111  最大   存储的是全英文类型  中文 gbk/2   utf8/3 
	1. char 最多存储255个字符				1byte  可以设置长度           
	2. varchar 最多存储65532个字符		4byte  可以设置长度
	3. text   超大字符串   不能设置默认值，不能规定属性(比如长度) text类型的数据不存在行中

### enum 和 set类型   有给定值的可选字符
	1. enum 单选类型  设定形式： enum('选项值1','选项值2','选项值3'....) 选项值对应的都有索引值 ，从1开始
		示例：create table user3(id int not null auto_increment primary key,
				user_name char(20),
				user_pass char(32),
				edu enum('小学' ,'中学','高中'),
				hobby set('篮球','足球','排球','乒乓球')
				);

		//插入语句，请注意看单选和多选的写法
		insert into user3 (user_name,user_pass,edu,hobby) values('user1','123','小学','篮球,足球,乒乓球');
	2. set 多选类型 设定形式 ： set('选项值1','选项值2','选项值3'....) 有相应的索引值，从1(2^0)开始，但是他的递增方式是按2^n次方递增 1,2,4,8,16.....
		示例：insert into user3 (edu,hobby) values(2,3); //代表'中学'  '篮球,足球'(索引相加(1+2=3))

### 字段属性
	1. 列属性： 多个列属性用空格隔开  比如  auto_increment primary key
		1.  null,not null; 设定为空或者非空
		2.  default 默认值;  设置默认值
		3.  primary key;设置主键 ，就是设置一个关键值，可以根据该值获取一行数据；只能有一个主键；主键必须有值；主键不能重复	primary key(字段名1,字段名2..)
		4.  auto_increment;自增长；默认从1开始；一个表只有一个自增长的字段	
		5.  unique key;设置一个字段的值是唯一的；类似 primary key，但是值可以为空。
		6.  comment; 用于设定字段的说明性内容，类似注释，但是又不是注释(属于有效代码)  形式: comment '文字内容'；

### 实体与实体关系

#### 实体
	一个表中一行数据实际就是对某物的描述性数据，所以一行数据也是实体
	有时实体也指整个表

#### 实体间关系
	是指不同实体数据之间的关系，很多时候是表对表之间的关系
	分为几类：一对一  一对多   多对多

## mysql多表查询
### 联合查询
	将两个或者2个以上的字段数量相同的查询结果，“纵向堆叠”后合并成一个结果  union 同盟，联合，团结  字段相同
	连接的列数量以及字段类型必须相同

	示例：select f1 from 表名 where 条件 union[all/distinct] select f3 from 表名 where 条件 order by xx limit 行号 行数
	
	union可以带 all或者distinct  默认是distinct已经消除重复数据   
	语句1：m行  n列    语句2：i行  n列   合并之后就是  m+i行  n列
	查询的列类型应该一致

### 连接(join)查询
	是将两个查询的每一行，以“两两横向对接”得方式，所得到的所有行的结果；即一个表中的某行与另外一个表的某行进行横向连接
	语句1：m行 n列  语句2：i行 j列  连接之后的数据 m*i行   n+j列

	完全连接之后就是，表1的所有行与表2的每一行依次横向连接得到新的行，得到的新行依次排开

	格式：
		select ... from 表1 [连接方式] join 表2 [on连接条件] where ....;  
		表1和表2 join之后当成一个新表，然后执行where得到所需要的表
	连接查询只是作为from子句的数据源
	连接查询包括以下这些形式：
		1. 交叉连接
		2. 内连接
		3. 外连接//左外连接、右外连接

#### 交叉连接
	链接之后得到的数据有时候是不对的
		select ... from 表1 cross join 表2 [on连接条件] where ....;  

#### 内连接 找出连接后真正对的数据
	语法格式：
		select ... from 表1 [inner] join 表2 on 连接条件 where ....;  
		 from 表1 [inner] join 表2 on 连接条件 当成一个表来看待
	
	1. 在交叉连接的基础上，使用on 连接条件 而筛选出来的部分数据
	2. 关键字 inner可以省略，但是建议写上 
	3. 内连接是使用最广泛的一种连接查询，其本质是根据条件筛选出“有意义的数据”

	实例：select * from product inner join product_type on product.protype_id=product_type.protype_id where product_type.protype_id=1; 查询出的是有意义的数据
	简写：
	select * from product as p inner join product_type as t on p.protype_id=t.protype_id where t.protype_id=1;

#### 外连接
##### 左外链接(left join)
	from 表1 left [outer] join 表2 on 连接条件 ..
	其实在内连接的基础上，保证左边表的数据全部读取出来一种连接

##### 右外连接(right join)
	from 表1 right [outer] join 表2 on 连接条件 ..
	其实在内连接的基础上，保证右边表的数据全部读取出来一种连接

#### 自连接  自己跟自己连接
	语法格式：from 表名 as a [连接形式] join 表名 as b on a.字段 = b.字段p;
	
	一个表将两个表来用	
	适用于一个表中的字段的值 来源于 当前表的另外一个字段的情况

### 子查询
	指一个“正常查询语句”中的某个部分(select部分，from部分，where部分)又出现查询的一种查询方式
	数据格式：select * from 表名 where price>=(一个子查询语句); 子查询上面的叫做主查询

### 标量子查询
	指子查询的结果是“单个值(一行一列)”得查询
	常用在where字句中，作为主查询的一个条件判断数据
	本质上，标量子查询的结果，可以当成一个值来使用

### 列子查询
	查询出来的结果是“一列数据”
	
	用来代替in运算符

	数据格式：in(列子查询)

### 行子查询
	查询出来的结果是“一行数据”

	数据格式： 比如where price>100 and pinpai="北京";
		where (字段1，字段2...)=(行子查询);
		或
		where row(字段1，字段2...)=(行子查询);

### 表子查询
	查询出来的结果是“多行多列”

	我的理解可以作为一个表使用
	语句格式：
		from (select 表子查询语句)

### 子查询关键字
	in、any(用在比较操作符的后，示例where age = any(值1,值2,值3...)任意一个满足就行)、
	all(用在比较操作符的后，示例where age = all(值1,值2,值3...)所有条件必须满足才行)

### exists 子查询   exists 存在
	语法格式：
		where exists(任何子查询);

	exists的子查询语句不能单独执行
		
	该子查询如果有数据，则该exists()结果为真，即相当于where true
	该子查询如果没有数据，则该exists()结果为假，即相当于where false

	用处：
		子查询语句用到主查询语句中的字段作为查询条件

## 数据管理
### 数据库备份
	1. 备份整个数据库
		mysqldump.exe -h 主机名 -u用户名 -p密码 数据库名>备份文件名(含路径)

	2. 备份表
			mysqldump.exe -h 主机名 -u用户名 -p密码 数据库名 表名>备份文件名(含路径)

### 数据还原
	1. mysql.exe -h 主机名 -u用户名 -p 目录数据库名 < 想要恢复的备份文件名
	2. source 路径(目录隔开使用"/")

## 用户管理
### 账号管理  没有加 ''的地方不是忘了，而是不需要
	1. 创建用户
		create user '用户名'[@'允许登陆的地址'] identified by '密码';	

	2. 删除用户
		drop user 用户[@允许登录的地址];

	3. 修改/更改密码
		set password for 用户[@允许登陆的地址] = password('密码');

	4. 查看用户
		数据库系统中有个数据库“mysql”，里面有个user表，里面就是我们的用户信息

### 权限管理
	1. 授予用户权限
		grant 操作1,操作2,....on *.*或者数据库名字 或数据库.表名 to 用户[@'允许登陆的地址'];
		操作是特定词：update、select、delete、insert、create.....
		还可以使用all表示所有的权限
		
		*.* 可以操作所有数据库

	2. 取消用户授权
		revoke 操作1,操作2,....on *.*或者数据库名字 或数据库.表名 from 用户[@'允许登陆的地址'];
		

## ajax  异步的 js and xml
	在不更新整个界面的基础上实现局部更新网页的技术
