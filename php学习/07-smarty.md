# smarty 模板引擎

## 实现Html与php分离
	1. {$name} 代替 <?php echo name; ?> 然后使用str_replace替换
	2. 在php控制文件中使用 include "视图文件路径"，包含html视图文件; 运行的是php文件

## 常用的php模板引擎
	smarty 所有模板引擎的鼻祖
	模板引擎的作用：实现前端与控制器分离

	smarty并不会改变原有文件的代码，会生成一个编译过后的php缓存文件，加载的时候加载的就是这个缓存文件

## smarty配置
	1. 左右定界符
		因为设置样式的时候也是使用 '{ xxxx }'，所以会导致冲突，添加左右定界符
		$smarty -> left_delimiter ="string";   $smarty -> right_delimiter = "string"; html文件中使用自定义的左右定界符

	2. 修改视图目录
		a. 设置模板目录  $smarty -> setTemplateDir(); 获取  $smarty -> getTemplateDir();
		b. 设置配置目录  $smarty -> setConfigDir();   获取  $smarty -> getConfigDir();
		c. 设置编译目录  $smarty -> setCompileDir();  获取  $smarty -> getCompileDir();
		d. 设置缓存目录  $smarty -> setCacheDir();    获取  $smarty -> getCacheDir();
		e. 设置插件目录  $smarty -> setPluginsDir();  获取  $smarty -> getPluginsDir();

## smarty 模板变量 下面使用{}只是设置左右定界符是这个而已，如果是自己设置的，则需要使用自己设置的
	1. 普通变量 
		$smarty -> assign("变量名",mixed $value);
		$smarty -> display("视图文件目录");  包含视图文件

	2. 保留变量 
		{$smarty -> get} 视图中直接访问$_GET数组
		{$smarty -> post} 访问$_POST数组
		{$smarty -> request} 访问$_REQUEST数组
		{$smarty -> session} 访问$_SESSION数组
		{$smarty -> cookie} 访问$_COOKIE数组
		{$smarty -> server} 访问$_SERVER数组
		{$smarty -> files} 访问$_FILES数组

	3. 预定义常量 通过$smarty.const访问php的预定义常量
		语法:{$smarty.const.预定义常量}

	4. 时间戳保留变量
		语法:{$smarty.now}

	5. 配置文件变量
		自定义配置文件：.ini、.conf、.txt结尾均可以  内部数据  键 = 值方式  值不需要双引号
		示例：
				a = 公司名称
				b = 公司简介
				c = 联系我们
		'#' 注释
		使用$smarty -> setConfigDir("配置文件路径");

		视图中使用配置文件的值：
		{#键#} 或者  {$smarty.config.配置变量}
		
	6. 配置文件分组
		自定义配置文件：.ini、.conf、.txt结尾均可以  内部数据示例：
		使用 [] 实现 
			示例：
				[cn]
					a = 公司名称
					b = 公司简介
					c = 联系我们
				[taiwan] #使用繁体
					a = 公司名称
					b = 公司简介
					c = 联系我们

		'#' 注释
		使用$smarty -> setConfigDir("配置文件路径");

		视图文件中加载配置文件
		{config_load file="配置文件名" section="taiwan"}

## smarty for..each 语法格式
	语法格式1：{foreach $arr as $key => $value}{/foreach}
	语法格式2：{foreach from=$myarr key="mykey"  item="myitem"}{/foreach}  //不能加空格

	示例：左右定界符是 <{}>
			<?php
			require_once("./Smart/libs/Smarty.class.php");
			$array=array(
			'dahost'=>'localhost',
			'dbname' => 'test'
			);
			$smarty = new Smarty();
			
			$smarty->left_delimiter = "<{";
			$smarty->right_delimiter = "}>";
			
			$smarty->assign("arr",$array);
			$smarty->display("./foreach.html");
			
			?>
			html文件中:
			<{foreach $arr as $key=>$value}>

			$arr[<{$key}>]=<{$value}> <br>
			
			<{/foreach}>
			
			<{foreach from=$arr key="key" item="value"}>
			
			$arr[<{$key}>]=<{$value}> <br>
			
			<{/foreach}>

### foreach常用属性
	1. value@key 取出当前值的索引,可能是整型索引，也可能是字符索引
	2. value@index 当前值得索引，从0开始计算
	3. value@iteration 当前循环的次数，从1开始计算
	4. value@first   首次循环时，值为true
	5. value@last   最后一次循环时，值为true
	6. value@total  整个循环的次数，可以在foreach内部或者外部使用

## smarty的for循环  ---section循环
	语法格式
		{section name="" loop=数组变量 start=开始 step=步长 max=最大 show=""}
				//输出数组内容
					{sectionelse} //数组为空，执行
		{/section}

## smarty if判断
	语法格式:{if 条件} xxx {else if 条件} xxxx {else} xxx {/if}	

## smarty变量调节器
	变量修饰器可以用来格式化变量
	语法格式：
		{变量|调节器1|调节器2|调节器3...}
		{变量|调节器1:参数1：参数2|调节器2:参数1：参数2}
		示例：{$smaerty.now|date_formt:"%Y-%m-%d %H:%M:%S"} 格式化日期

### 常用的变量调节器
	1. {$title|upper} 转成全大写
	2. {$title|lower} 转成全小写
	3. {$title|capitalize}首字母大写
	4. {$title|nl2br}将\n转成html的<br/>
	5. {$title|replace:'a':'b'}替换
	6. {$title|date_formt:"%Y-%m-%d %H:%M:%S"}格式化日期
	7. {$title|truncate:长度:截取字符最后边的显示}  截取字符串到指定长度