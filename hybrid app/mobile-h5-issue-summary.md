# 移动端h5问题总结
以下问题提到的机型特指原系统自带的浏览器。

- [fixed定位](#fixed定位)
- [button类型input的disabled](#button类型input的disabled)
- [1px问题](#1px问题)
- [border-radius](#border-radius)
- [禁止滚动](#禁止滚动)
- [background](#background)

## fixed定位
1. ### ios4-, Android 2.1-不支持fixed
	position一律为static

2. ### 当设置position:fixed，若滚屏会出现错位。

	解决办法：用absolute模拟fixed定位。[demo](https://julielee77.github.io/demo/0001.html)

	**HTML部分**

	```
	<!DOCTYPE html>
	<html lang="en">
	<head>
	  <meta charset="UTF-8">
	  <meta name="viewport" content="width=device-	width,initial-scale=1,maximum-scale=1,user-	scalable=0">
	  <title>absolute stimulates fixed</title>
	  <link rel="stylesheet" href="../css/demo/0001.css">
	</head>
	<body>
	  <header>
	    header fixed
	  </header>
	  <section class="main-wrapper">
	    <p>...超过一屏的文字</p>
	  </section>
	  <footer>
	    footer fixed
	  </footer>
	</body>
	</html>
	```
	**css部分（.styl）**
	
	```
	header,footer
	  position absolute
	  left 0
	  right 0
	  background #43a08d
	  line-height 44px
	  z-index 1
	  text-align center
	  color #fff
	  font-size 20px
	header
	  top 0
	footer
	  bottom 0
	.main-wrapper
	  position absolute
	  top 0
	  bottom 0
	  padding 44px 0
	  overflow-y auto
	  -webkit-overflow-scrolling touch //ios原生滚动回调效果
	  p
	    line-height 24px
	```
目前，我们团队移动端Andriod为4.0＋，因此可不考虑此bug，但ios若将弹出框等设置为fixed，使其show时会出现页面未渲染出来但DOM文档中可找到（其实是存在的，只是页面看不到，比如弹出框的按钮还是可点击的）的情况，这种情况建议还是使用absolute。

### 移动端fixed受transform影响
//TODO
此外，position:fixed还有其它一些坑，比如其中不能有input/textarea输入框，这样调出键盘后会会出现错位。而且，Android比ios表现更好。[更多fixed坑](https://github.com/maxzhang/maxzhang.github.com/issues/2)
## button类型input的disabled
```input[type=button][disabled]
  opacity 1```
重设ios渲染的默认opacity=0.4
## 1px问题
原生1px等于物理1px，但web的1px则依 `devicePixelRatio`而定。因此，在Retina屏中的1px往往是物理1px的数倍，参照一般 `devicePixelRatio=2`可实现近似1px（当然也可以根据不同 `devicePixelRatio`更精细地适配）
```gray-4 = #e7e7e7 //间隔线vendor(prop, args...)
  -webkit-{prop} args
  {prop} args
.scale
  position relative @unless @position
  &:after
    content ''
    position absolute
    bottom 0 @unless @bottom
    left 0 @unless @left
    right 0 @unless @right
    border-bottom 1px solid gray-4
    vendor transform scaleY(.5)
    vendor transform-origin 0 0  ```
在ios8中支持`border:0.5px`
## border-radius
### Android2.3不支持％
会将50%写成一个较大的值，如：
```
div
  border-radius 999px```
### Android及safari低版本img圆角问题
当img有 `border`时设置 `border-radius`会导致圆角变形，需要在img外嵌套一层元素，并设置 `border`和 `border-radius`
### Android4.2.x背景溢出如Galary S4，红米，小米3。在Android4.2.x中同时设置 `border-radius`和 `background`时，背景色会溢出到圆角以外，需要使用 `background-clip:padding-box`来修复，但是如果border-color为半透明时，北京直角部分依然会露出来。
### Android4.2.x不支持border-radius简写
如Galary S4，红米，小米3。解决方法：使用 `border-radius`四个扩展属性，缩写属性放在最后。
```
border-radius(args)
  if(length(args)==1)
	border-top-left-radius args
	border-top-right-radius args
	border-bottom-right-radius args
	border-bottom-left-radius args
	border-radius args
  else
    border-radius args			```
### 其它问题
IE9中fieldset不支持 `border-radius`
IE9中带有背景渐变（ `gradient`）的时候背景溢出。

更多`border-radius`问题参考 [border-radius 移动之伤](https://github.com/yisibl/blog/issues/2)
## 禁止滚动
移动端设置

```
body{overflow-x:hidden;}
```
不会生效，因为移动端是基于touch，可将内容包裹再设置

```
.wrapper
  position fixed  
  overflow-x hidden
```
## background
1. ### background渐变背景
	部分Android对backgorund渐变不支持，而需要使用更明确的	background-image属性。 
	
	```
	background-image: -webkit-linear-gradient(left, 	#f8f8f8 0%, #9edef9 50%, #f8f8f8 100%);
	``` 
	此外，to right写法QQ浏览器／手机safari浏览器不支持。
2. ### background-size简写兼容性
	MX2不支持background-size简写

	补充background简写

	```
	background: color image position/size repeat 	attachment clip origin;
	```
	
## -webkit-backface-visibility:none;
可解决CSS3动画闪烁问题，但也会导致其它一些奇葩问题，慎用。
## transform:translate3d()
特别是Z轴的移位会导致一些问题，慎用。
## max-device-width/max-width
MX2不支持 `max-device-width`
## input与line-height
注：input元素设置line-height控制高度时，mx2的光标高度等于line-height，可改为

```
input[type=tel]{
	height:44px;
	vertical-align:middle;
}
```
## ios不支持focus()
ios中调用focus()方法，document.activeElement会改变为设置的元素，但并不会因此调出键盘。解决方法为**在click方法中立即调用focus()， 不能有在timeout／异步请求等延时之后**。

详细示例参见[移动端h5问题探索(1)支付密码输入框](https://github.com/JulieLee77/frontend-articles/blob/master/hybrid%20app/mobile-h5-issue1-pay-password.md)
## 获取设备可用尺寸
`screen.avialHeight` 包含了webview渲染的标题栏，应使用 `window.innerHeight`获取文档显示区的高度。

  
