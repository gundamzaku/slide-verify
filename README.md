## 云天河 - 滑动验证码

目前只兼容webkit内核浏览器

很遗憾，目前不支持移动端！

演示地址 [//mall.hlzblog.top/test/slide_verify](//mall.hlzblog.top/test/slide_verify)




##### 滑动验证码的嵌入

> 实例化视图
>> 将验证码图片赋值给视图模板
>>>	输出模板

	$v = new View; 
	$v->captchar = \Mine\Slide::instance();
	echo $>v->fetch('User/login');

示例模板文件，如下

	<body>
	    <h1>云天河 滑动验证码</h1>
	    <!-- 模板赋值区域，开始 -->
	    <div id="yth_captchar">{$captchar}</div>
	    <!-- 模板赋值区域，结束 -->
	<script type="text/javascript" src="http://libs.baidu.com/jquery/1.9.0/jquery.js"></script>
	<script src='./static/plugins/layer/js/layer.js'></script>
	<script src='./static/plugins/verify/js/drag.js'></script>
	<script>
	$(document).ready(function() {
		$(this).yth_drag({
			"verify_url": api("Slide_Verify", "check"),
			"source_url": api("Slide_Verify", "captchar")
		});
	});
	</script>
	</body>



##### 验证用户拖动的滑动验证码的接口，如下

	<?php
	/*
	$msg['status'] = false; // 是否需要重新请求验证码，默认否
	$msg['Err'] = 1004;     // 错误编码
	$msg['out'] ;           // 编码对应输出
	*/
	use Mine\Slide; 	// 引入文件
	class Slide_Verify {
	    /**
	    * 获取验证码的html
	    * @param GET   : (source)
	    * @echo Html | String
	    */
	    public function captchar(){
	        echo Slide::instance(1);
	    }
	    /**
	    * 验证码，校验
	    * @param POST  : (int)x_value 横坐标，用户滑动结果
	    * @echo String : {"Err":"","out":""}
	    */
	    public function check(){
	        // 参数过滤
	        Slide::instance(2);
	    }
	}

##### 通过滑动验证码后的验证

示例如下

	<?php
	use Mine\Slide; 	// 引入文件
	class index{
		/**
		* 普通登陆
		* @param POST  : loginName,loginPwd 
		* @echo String : 状态
		*/
		public function login(){
			// 判断验证码已否通过验证
			Slide::instance(3); 
			/* 
				验证码通过验证？
				  true
					自动输出 JSon字符串 并 终止程序，示例如下
						{"status":false,"Err":错误码,"out":"对应的错误信息"}
				  false
				  	继续执行程序
						程序有错误出现？
						  true
						  	输出 JSon 字符串 终止程序，示例如下
						  	{"out":"用户名不正确"}
						  false
						  	输出 JSon 字符串 ，请包含`status`等于true，示例如下
						  	{"status":true}
			*/
		}
	}