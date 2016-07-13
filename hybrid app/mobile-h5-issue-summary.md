#移动端h5问题总结
以下问题提到的机型特指原系统自带的浏览器。
##position:fixed
1. ###ios4-, Android 2.1-不支持fixed
	position一律为static

2. ###当设置position:fixed，若滚屏会出现错位。

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


###移动端fixed受transform影响
//TODO



  opacity 1




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
    vendor transform-origin 0 0  




div
  border-radius 999px






border-radius(args)
  if(length(args)==1)
	border-top-left-radius args
	border-top-right-radius args
	border-bottom-right-radius args
	border-bottom-left-radius args
	border-radius args
  else
    border-radius args			




更多`border-radius`问题参考 [border-radius 移动之伤](https://github.com/yisibl/blog/issues/2)
##禁止滚动
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
##background
1. ###background渐变背景
	部分Android对backgorund渐变不支持，而需要使用更明确的	background-image属性。 
	
	```
	background-image: -webkit-linear-gradient(left, 	#f8f8f8 0%, #9edef9 50%, #f8f8f8 100%);
	``` 
	此外，to right写法QQ浏览器／手机safari浏览器不支持。
2. ###background-size简写兼容性
	MX2不支持background-size简写

	补充background简写

	```
	background: color image position/size repeat 	attachment clip origin;
	```
	
##-webkit-backface-visibility:none;
可解决CSS3动画闪烁问题，但也会导致其它一些奇葩问题，慎用。
##transform:translate3d()
特别是Z轴的移位会导致一些问题，慎用。
##max-device-width/max-width
MX2不支持 `max-device-width`
  
