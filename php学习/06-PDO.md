# 静态延时绑定
	语法：static::成员方法 静态成员 静态方法 类常量

	1. 只有一个类：self和static 值代表当前类
	2. 继承范围内：self永远代表当前类，static代表真正执行的类

# PDO与命名空间

## 命名空间
	用来解决编写类库或者应用程序的名称冲突问题
	
	只有类、函数、常量(const定义的常量)受命名空间影响

	定义单个命名空间：namespace 空间名  必须是第一行语句
		语法格式：namespace XXX;   
		使用的时候   调用目标空间下的数据  只有类、函数、常量受命名空间的影响，其它代码不受影响
		1. 类 new xxx\类名();   一定是反斜线
		2. 方法  XXX\函数
		3. 常量   XXX\常量

	子命名空间 使用的时候加上很多子空间目录    谨记：需要使用反斜线
		定义:namespace a\b\c\d....
	a\b\c\XXXX

	访问空间中元素的方式
		全限定名称(\开头，从根目录开始寻找)  
		非限定名称(不带任何前缀)
		限定名称(带个别的前缀)
					

	namespace关键字的另外含义：代表当前命名空间

	命名空间支持导入空间的元素并起别名
		1. 空间名导入
		2. 类导入
	使用'use'导入命名空间路径   as变成另外的别名   使用方式：use XXXXX as xxx 

	spl_autoload_register(function($className){  //有命名空间的地方返回的是带命名空间的类地址 });

## PDO
	PHP DATA OBJECT php数据对象
	作用：封装对各种服务器数据操作的类，使用功能后不再需要"mysqli_*"函数了.

	1. 创建PDO对象 PDO::__construct(string $dsn,[string $username,string $pwd]);
		$dsn示例格式: $dsn="mysql:host=127.0.0.1;port=3306;dbname=db;charset=utf8";

### PDO常用方法
	1. int  exec(string $sql);  执行一条语句，获取受影响的行数 用来执行update、insert、delete..操作
	2. PDOStatement query(string $sql);  执行语句，返回一个结果集对象 //用来执行获取操作
	3. string lastInsertId();  返回最后插入行的ID或者序列值
	4. setAttribute(int $attr,mixed $value);   设置数据库句柄属性,白话：给常量(因为PDO没有属性)赋值
	5. mixed PDOStatement::fetch([int $fetch_style]);参数：获取返回值的类型(关联数组(assoc)或者枚举数组或者全部返回)
		返回一行数据，并把指针移动到下一行,获取失败返回false
	
	6. array PDOStatement::fetchAll([int $fetch_style]);返回所有的数据列表
	7. int PDOStatement::rowCount();//返回受上一条语句影响的行数

### PDO错误处理
	3种处理方式：
		1. 静默模式；//错误发生后，不会主动报错
		2. 警告模式；//有错误时，按照PHP的错误发生警告
		3. 异常模式；//错误发生后，抛出异常，我们需要捕获并处理异常
	使用PDO::setAttribute()更改错误模式

#### 静默模式
	PDO::errorCode()和PDO::errorInfo();查看错误信息
 
#### 警告模式  不推荐使用
	设置：PDO::setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_WARNING);
	设置之后有错误直接网页显示，但是会显示出错误的文件以及行号，不安全

#### 异常模式
	设置：PDO::setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_EXCEPTION);
	try{}catch(PDOException $e){}

### PDO预处理
	1. 提取相同的部分，不同的部分去掉 也可以使用占位符(:value  ?) 查看PDO::prepare(string $statement)示例
	2. 预编译相同的部分，将编译结果储存
	3. 将不同的数据部分进行替换
	4. 执行即可  execute
	
	占位符：  :value  ?
    PDOStatement PDO::prepare
	给占位符绑定值：PDOStatement::bindValue(mixed $paramter,mixed $value);  冒号开头的 bindValue(':value',值)；
				  问号的$paramter 使用索引代替，从1开始
	执行绑定数据 PDOStatement::execute();