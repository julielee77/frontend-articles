# Hybrid App开发概述
移动端网页开发主要考虑**webkit内核的浏览器**（hybird App主要）和chrome，uc,qq,小米浏览器---测试的重点

- [前言](#前言)
- [webview](#webview)
- [特点](#特点)
- [优势](#优势)

## 前言
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
	
	以下讨论Hybrid App。
	
## webview
webview做为承载网页的载体控件，可以将其视为客户端系统内置的浏览器，它使用了webkit渲染引擎加载显示网页。

WebView在网页显示的过程中会产生一些事件，并回调给应用程序，以便在网页加载过程中做应用程序想处理的事情。

WebView提供两个事件回调类给应用层——

- WebViewClient 提供网页加载各个阶段的通知
- WebChromeClient 提供网页加载过程中提供的数据内容

**webview与js交互：**

1. webview调用js
	
	```
	/*andriod */
	mWebview.loadUrl("javascript:do()");
	```
	
	webview调用js中do方法。
2. js调用webview

	android或ios开放某些特定接口供javascript调用，即UIViewController中的代码（Native）。
		
	js是不能直接调用native的method，所以需要借助uiwebview的代理方法
	
	```
	/*andriod */
	(BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
	```
	每次在重新定向URL的时候，这个方法就会被触发。webviewJavascriptBridge 也是利用这个方法实现的。

	假设以下本地类给js调用
		
	```
	/*andriod */
	package com.test.webview;
	class DemoJavaScriptInterface {
	  DemoJavaScriptInterface() {
	  }
	  /**
	   * This is not called on the UI thread. Post a runnable to invoke
	   * loadUrl on the UI thread.
	   */
	  public void clickOnAndroid() {
	      mHandler.post(new Runnable() {
	    public void run() {
	        //TODO
	    }
	      });
	  }
	    }
	```
	首先给webview设置：
	
	```
	/*andriod */
	mWebview.setJavaScriptEnabled(true);
	```
	
	接着将本地的类映射出去：
	
	```
	/*andriod */
	mWebView.addJavascriptInterface(new DemoJavaScriptInterface(), "demo");
	```
	
	以上实现demo类公布出去供js调用。
	
## 特点
1.  资源大部分以file开头，本地、网络资源使用js异步接口和native获取，再和js的接口交互
2.  js调用native功能

	- 让native监控一系列特殊的url请求，再在native中调用相应的功能或者提供数据接口，数据的提供方式类似JSONP
	- 类似Apache cordova（驱动phonegap的核心引擎，从phonegap抽出的核心代码贡献给apache）
		
		提供设备相关API、js类库以及这些类库相关的设备原生后台代码。

## 优势
1.  可以指定什么版本的webview，用什么内核，拥有什么等级的安全权限等等 
2.  因为资源本地化，可以使用重级的框架，如angular,react （最终都是通过和native代码捆绑发布）
3.  native的静态前端部分的更新，可以使用远程下载静态资源包的方式实现，不发布大版本而修改webview中的逻辑请求（这点也是大部分公司选择一般native，一半h5开发原因。都知道ios审核发版很慢）

移动端优化的很大一个瓶颈就是网络环境不一致，如wifi、4G 、3G、2G，以及其对应的网络状态良好、较差等情况，特别是对于大流量资源如大量图片。

附参考：

[移动前端开发和 Web 前端开发的区别是什么？推荐小爵的回答](https://www.zhihu.com/question/20269059)

[What is a Hybrid Mobile App?](http://developer.telerik.com/featured/what-is-a-hybrid-mobile-app/)

[Android开发学习笔记：浅谈WebView](http://liangruijun.blog.51cto.com/3061169/647456/)