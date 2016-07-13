#CSS常用功能实现
[github文件下载地址](https://github.com/JulieLee77/units)
##子元素float后父元素有高度的方法
1. 设置父元素float
2. 设置父元素高度
3. 设置父元素

	```
	div:after
	  	  clear both
	```
4. 设置父元素

	```
	div
  	  overflow hidden
	```
	
##水平／垂直居中	
###水平居中
1. inline/inline-block元素
	设置其父元素
	
	```
	div
  	  text-align center
	```
	**inline元素应注意元素间的空格／回车**
2. block元素

	```
	div
      width 960px
      margin 0 auto
	```
	
###垂直居中
1. 单行

	```
	p
      line-height 44px;
	```
2. 多行

	（1）父元素定高
	借助inline-block元素的 `vertical-align middle`

	（2）父元素不定高
	
	可参照以下方法
	
**不定高元素内容水平垂直居中（如弹出框）**

CSS实现方法（1）

```
.align-center
  text-align center
  &:after
    content ''
    width 0
    height 100%
  >div, &:after
    display inline-block
    vertical-align middle	
```
实现方法（2）－－flex方案
此方法会有兼容性问题，比如MX2不支持。

```
.align-center
  -webkit-align-items center
  -ms-flex-align center
  align-items center
  display -webkit-flex
  display flex
```
[未知尺寸元素水平垂直居中](http://demo.doyoe.com/css/alignment/)

##文字省略后加省略号

[多行文本溢出显示省略号(…)全攻略](http://www.css88.com/archives/5206) 

###单行文本省略
```
p
  text-overflow ellipsis
  overflow hidden
  white-space nowrap
``` 

###多行文本省略
-webkit内核浏览器（移动端）

```
p
  overflow hidden
  text-overflow ellipsis
  display -webkit-box
  -webkit-line-clamp 2
  -webkit-box-orient vertical
```
