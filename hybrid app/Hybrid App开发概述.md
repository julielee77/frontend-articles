#Hybrid App开发概述
移动端网页开发主要考虑**webkit内核的浏览器**（hybird App主要）和chrome，uc,qq,小米浏览器---测试的重点

##前言
移动端网页开分为三种模式：

1.	移动端web app
	如新浪微博网页版，一般特点是
	
	```
	<meta name="apple-mobile-web-app-capable" content="yes">
	```
2.	移动端网页

	如新浪网
3.	Hybrid App

	Hybrid APP包括Andriod或ios开发的原生部分和在其webview中打开的html5页面（如微信网页）

##特点
1.  资源大部分以file开头，本地、网络资源使用js异步接口和native获取，再和js的接口交互
2.  js调用native功能

	(1)	让native监控一系列特殊的url请求，再在native中调用相应的功能或者提供数据接口，数据的提供方式类似JSONP
	
	(2)	类似Apache cordova（驱动phonegap的核心引擎，从phonegap抽出的核心代码贡献给apache）	提供设备相关API、js类库以及这些类库相关的设备原生后台代码。

##优势
1.  可以指定什么版本的webview，用什么内核，拥有什么等级的安全权限等等 
2.  因为资源本地化，可以使用重级的框架，如angular,react （最终都是通过和native代码捆绑发布）
3.  native的静态前端部分的更新，可以使用远程下载静态资源包的方式实现，不发布大版本而修改webview中的逻辑请求（这点也是大部分公司选择一般native，一半h5开发原因。都知道ios审核发版很慢）

	移动端优化的很大一个瓶颈就是网络加载速度不一致。代码量在移动端开发是很大的一个考察点（这点我目前没体会到）

附参考：

[移动前端开发和 Web 前端开发的区别是什么？推荐小爵的回答](https://www.zhihu.com/question/20269059)

[What is a Hybrid Mobile App?](http://developer.telerik.com/featured/what-is-a-hybrid-mobile-app/)

[Android开发学习笔记：浅谈WebView](http://liangruijun.blog.51cto.com/3061169/647456/)