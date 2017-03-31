# h5开发实用功能／属性
一些常用的功能，可根据需求应用。
## meta属性
- apple-mobile-web-app-capable

	```
<meta name="apple-mobile-web-app-capable" content="yes">
	```	
	yes删除apple默认的工具栏和菜单
- apple-mobile-web-app-status-bar-style

	```
	<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
	```
	default/black/black-translucent 设置apple状态栏样式
	另会配合apple-touch-fullscreen
	
	```
	<meta name="apple-touch-fullscreen" content="yes">
	```
- formata-detection
	
	```
	<meta name="format-detection" content="telephone=no">
	```
	telephone=no/email=no,adress=no 取消apple的默认格式识别
	
		
## -webkit-min-device-pixel-ratio
```
@media and screen(-webkit-device-pixel-ratio:2){
	...
}
```	
`-webkit-max(min)-device-pixel-ratio` 设备最大／最小像素密度

`deviePixelRatio`指的是 `window.devicePixelRatio`，被所有主流浏览器支持。

`devicePixelRatio=物理像素／dip`

dip或dp（**device independent pixel**）与屏幕密度有关。可以用来辅助区分视网膜设备和非视网膜设备。
## autocompelete="off"
去掉输入框的历史记录
## -webkit-tap-highlight-color

```
a,img,button,input,textarea{
	-webkit-tap-highlight-color:rgba(0,0,0,0);
}
```
取消移动端tap后的高亮显示
## 去掉input，button点击时蓝色边框

```
input[type=button],button{
	outline:none;
}
```
(input一般只用得上type=button)

## -webkit-appearance:none

```
textarea,input{
	-webkit-appearance:none;
	appearance:none;
}
```
不显示浏览器的默认样式。

对于`textrea`, `input`去掉ios的圆角及内阴影。

对于 `radio`，`checkbox`可扩展选取范围，但已选样式需要图标。
## user-select:none
禁止长按选取／复制

```
body{
	-webkit-user-select:none;
	user-select:none;
}
```
## pointer-events:none;
决定是否可以穿越绝对定位元素，触发遮挡元素的某些行为。
## -webkit-touch-callourt:none;
当触摸链接时禁用系统默认菜单。
设置ios的webview可设置body此属性。
## -webkit-text-security	
`input[type=password]` 默认值disc

对其它type的input设置此属性时，移动设备会有较长延迟（即输入时先明码显示，之后一会儿才转换为disc）

详细示例及说明参见 [移动端h5问题探索(1)支付密码输入框](https://github.com/JulieLee77/frontend-articles/blob/master/hybrid%20app/mobile-h5-issue1-pay-password.md)
## -webkit-text-size-adjust:100%;
禁止横竖屏时webkit内核浏览器网页文字缩放，解决font-size<12px不能显示的问题（现代浏览器基本已无此问题）。

chrome27已不再支持此属性。

若设置全局的`-webkit-text-size-adjust:none;`会导致放大网页的时候字号不能改变。

