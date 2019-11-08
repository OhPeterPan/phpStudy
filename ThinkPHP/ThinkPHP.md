# 概要
## 学习目标
	1. 学会使用composer
	2. 学会使用TP5.1
	3. 说出tp5.1重要目录结构
	4. 理解tp5.1请求的生命周期
	5. 理解并使用路由注册功能
	6. 能够使用命令行创建分组和控制器
	7. 能够理解参数绑定和依赖注入
	8. 能够理解request类的基本使用
	9. 能够返回json格式
	10. 能够进行模板渲染
	11. 能够在模板中进行变量输出
	12. 能够在模板中使用foreach等内置标签

## compser
	1. 用来管理依赖关系的管家，是为php准备的一个软件管家
	2. 使用之前需要先安装Composer
	3. 安装完成之后使用 命令composer 查看是否安装composer
	
	
	2. 安装TP5.1命令 composer create-project --prefer-dist topthink/think=5.1.x-dev tp5(这是下载下来的文件名称)
		composer ---- 执行composer程序
		create-project ---- 通过composer创建应用
		topthink/think ----框架名字
		--prefer-dist ----- 优先下载压缩包，而不是直接从github下载源码

## TP操作
	1. 启动TP方式
		方案一：配置虚拟主机

	2. 命令行运行TP服务  (tp文件根目录下打开命令行，运行该命令)
		php think run 

### TP文件目录结构
	1. **application**   应用目录，程序所有的业务代码都是在这个目录
		1. common  公共模块目录，在application可以自定义自己的模块，但在common中定义的函数与模型都是公用的
		2. common.php  公共函数库文件
		
	
	2. **config **       项目配置目录
		1. app.php   应用主配置文件
		
	3. extend        扩展目录
	4. **public**        虚拟主机所指向的目录 		 
	5. **route**         路由 
	6. runtime       临时缓存路径，框架自动生成的目录(这个目录在Linux和mac下必须有读写权限)
	7. thinkphp
	8. vendor        第三方类库目录，该目录不用手动更改
	
### TP命名规范
	1. 目录使用小写+下划线
	2. 类的文件名均以，且命名空间路径与类文件所在路径一至
	3. 方法命名使用驼峰法
	4. 属性的命名使用驼峰法
	5. 常量以大写字母+下划线命名
	6. 配置参数以小写字母+下划线命名
	7. 环境变量定义使用大写字母+下划线命名
	8. 数据表和字段使用小写字母+下划线命名
		
### TP的生命周期
	（看图 -> TP生命周期流程）

## 路由
	将用户的请求按照事先规划好的方案提交给指定的控制器和方法进行处理
	
	TP提供两种路由规则： 二者只能存活一个，设置一个另外一个就会失效
		1. pathinfo模式：将用户的请求按照事先规划好的方案提交给指定的控制器和方法进行处理
		2. 自定义路由规则(推荐)
	
	TP5.1默认开启路由，如果没有定义路由，则使用pathinfo模式访问URL

	1. 隐藏index.php文件
		1. httpd.conf 中打开 LoadModule rewrite_module modules/mod_rewrite.so
		2. 虚拟主机中配置   AllowOverride All
		3. 修改.htaccess文件
		

	2. 路由相关配置
		1. config/app.php中配置 
			1. 强制路由  'url_route_must' => true
			2. 路由缓存  'route_check_cache' => true 对于路由特别多的地方使用此功能有效提升性能
			3. 完全匹配  'route_complete_match' => true 
		
		2. 定义路由
			1. Route::请求方式("路由表达式",匿名函数);
			2. Route::请求方式('路由表达式',"@模块名/控制器名/方法名"); 


		3. 请求类型
			1. Route::get("new/:id","News/read")    获取
			2. Route::put("new/:id","News/read")    修改
			3. Route::post("new/:id","News/read")   添加
			4. Route::delete("new/:id","News/read") 删除
			5. Route::any("new/:id","News/read") //所有请求都支持的路由规则，不分请求类型
			

	3. 路由参数
	4. 路由分组
		方便管理

### 使用命令行创建分组
	php think build --module 分组名称  创建模块

### 创建控制器
	1. 手动创建  application/模块目录/controller/xxx  new -> php.class
	2. 命令行创建 php think make:controller --plain 控制器名称(首字母大写)

### 查看已定义的路由列表
	php think route:list

## Request对象 -- think -> facade -> Request 类 
	门面方式获取数据
	1. 获取get的所有数据   Request::get()
	2. 排除不要的数据   Request::except(['id']) 
	3. 获取指定的数据   Request::only(['id','age'])
	4. 判断一个key是否存在  Request::has('id');
	5. 与$_SERVER 类似   Request::server();
	6. 获取环境变量$_ENV   Request::env();
	7. 获取路由          Request::route();


## Request对象  think -> Request类  上面那个类相当于门面  两个类方法一样
	依赖注入的方式获取数据[推荐]
	Request $request;
	$request -> xxx

## 助手函数获取数据 [推荐]
	1. 获取任意类型的请求 input("param.")
	2. 获取get数据  input("get.id");
	3. 获取post数据 input("post.id")
	4. 获取任意请求的key为name的值  input("name")  get和post一起时优先获取post数据
	5. 判断一个key的值是否存在  input('?name')

## 参数绑定
## 依赖注入

## 得到json
	json(....) 

# 视图
	1. 视图命名规则
		视图目录/控制器名/方法名+模板后缀

	2. 模板的渲染
		1. return $this -> fetch('[模板文件]','[模板变量]');
		2. return view('模块@控制器/控制器动作') 针对自定义路由

	3. 模板赋值
		1. $this -> assign("xx",变量) 
		2. return view('模块@控制器/控制器动作',['xx' => 变量,'xx1' => 变量])
		3. return view('模块@控制器/模板名字',compact('与变量同名不带$符号的名字','与变量同名不带$符号的名字1')) 推荐
		4. 全局赋值	
			use think\facade\View    View::share("webname",'网站名称');\

	4. 使用函数
		1. 需要对模板输出使用函数的时候 可以使用php提供的函数，也可以使用tp提供的，用户在tp中规定的文件中定义的函数
			语法:{:md5($password)}

	5. 原样输出
		1. 为了保持js不被tp的模板干扰  
			{literal} {/literal}

	6. 循环标签
		{foreach $list as $key =>$vo}  {/foreach}

	7. 条件判断
		{if ()OR()} {elseif 条件 /} {else /}

	8. 模板继承


# 连接数据库
	config/database.php中配置数据库参数

	配置数据库相关参数的时候一定要检查mysql和pdo是否开启

## 数据库增删改查
	1. 查询
		Db::query("sql语句",[占位数据])
		ps: 
		$sql = 'select * from article where id=:id';
		Db::query($sql,['id' => 24]);

	2. 插入、删除、更新
		Db::execute('sql语句',[占位数据])

	3. 添加数据Db
		$data = ['title' => '哈哈哈哈','content' => '文章吧']
		//table需要加上表前缀 配置文件里配置的那个前缀
		Db::table('tp_article')->insert($data);
		//不需要表前缀
		Db::name('article')->insert($data);
		//推荐写法，不需要前缀
		db('article')->insert($data);
		//获取插入数据的id值
		$resId = db('article')->insertGetId($data);

	4. 添加多条数据
		$data = [['title' => '哈哈哈哈','content' => '文章吧'],['title' => '哈哈哈哈','content' => '文章吧']]
		Db::name('article')->insertAll($data);
		db('article')->insertAll($data);

	5. 更新数据 一定要写where条件
		db('article')->where('id',1)->update(['title' => '哈哈哈哈']); //推荐写法
		db('article')->where('id',1)->data(['title' => '哈哈哈哈'])->update();

	6. 删除数据
		Db::table('tp_article')->delete(1); //根据id删除
		Db::table('tp_article')->delete([1,2,4]);//根据id组删除

		Db::table('tp_article')->where->('id',1) -> delete();//根据id组删除
		Db::table('tp_article')->where->('id','<',10) -> delete();//id <10的删除

		软删除 添加一个删除标记，并不是真正的删除

	7. 查询数据
		查询一条 
			Db::name("article")->where('id',2)->find();
			Db::name("article")->where('id',2)->findOrFail();

		查询多条
			Db::name("article")->where('id','>','2)->select();

		排序和分页
			db('article') ->order('id','desc') -> limit(0,2) -> select();

		获取指定字段的值
			db('article') -> where('id',3) ->value('title');

		获取指定的列
			db('article') -> column('title');
			db('article') -> column('title','id'); //以id为键

		获取器
			db('article') ->withAttr('title',function($val,$data){

			 		 return '你好世界--'.$val;//这里的val就是获取到的title的值 

				}) -> where('id','>','2') -> select(); 获取到指定数据后进行操作

## 模型
### 模型定义与设置
	1. 命令行创建model
		php think make:model 模块/模型(首字母大写)

### model的增删改查 .....还是看文档吧
	1. 查询单条记录
		User::where('name','thinkphp') -> find();

	2. 条件分组  where() or ()
		User::where(function($query){$query->where('id','<=',23)})->whereOr(function($query){$query->where('clcik','<=',23)})->select();
	
## TP5的分页功能
	{$data|raw}  直接出现分页

## TP5的验证码功能 重要
	1. 独立验证
		$validate=Validate::make(['name'=>'require|max:25|checkTitle:张三' ,'email' => 'email'],//定义规则

						//规则被触发的时候返回提示语句
						['name.require'=>'名称必须填写','name.max'=>'名称最多只能25个字','email.email'=>'邮箱不合法']);
	
		自定义规则
			Validate::extend('checkTitle',function($value,$rule){
				return strstr($value,$rule)?"不成功":true;
				})

	2. 验证器 TP5.1之后推荐使用
		1. 创建验证器
			php think make:validate 模块名/验证器名(首字母大写)

	3. csrf 验证  其实就是加上一个token 
		{:token()}

## composer 下载扩展包
	composer require topthink/think-captcha  tp验证码包

## TP5的cookie和session

### Cookie操作
	1. think\facade\Cookie  可以调用静态方法
	2. 获取 cookie('name')  设置cookie('name','value','3000(时间)');  删除cookie('name',null) 是否存在cookie('?name')

### Session操作
	1. think\facade\Session   调用静态方法
	2. 获取 session('name')  设置session('name','value');   删除session('name',null) 
		是否存在cookie('?name')      获取全部session('');
	   	清空所有session(null)

## 闪存
	Session::flash('name','value'); 定义好之后，下一次进行http请求的时候可以拿到，第二次就拿不到了

## 中间件
	用于拦截和过滤HTTP请求，并进行必要的业务处理

	1. 定义中间件  类似于OkHttp的拦截器?
		php think make:middleware  中间件名称

		 //pathInfo模式 注册中间件
		    protected $middleware=[
		        'CheckLogin:param'
		    ];

		//路由模式注册中间件
		#Route::get("login", "@index/Login/index")->name('index/Login/index')->middleware("CheckLogin:param");
		Route::group(["middleware" => 'CheckLogin:param'/*传值*/], function () {
		    Route::get("login", "@index/Login/index")->name('login');
		});

		全局配置中间件
		application目录中 创建middleware  return [\app\http\middleware\LoginCheck::class]; 支持多个中间件

### 中间件传参
	 return [[\app\http\middleware\LoginCheck::class,'param']];  
	 public function handle($request, \Closure $next,$param/*接受值*/)
    {

        echo '1111';
        return $next($request);
    }

		 
		
		
		
	
	
	
		
		 