
# CSS常用功能实现

- [水平垂直居中](#水平垂直居中)
- [清除浮动并使父元素有高度](#清除浮动并使父元素有高度)
- [文本省略](#文本省略)
- [禁止浏览器自动填充表单](#禁止浏览器自动填充表单)

[github文件下载地址](https://github.com/JulieLee77/units)

## 水平垂直居中
### 水平居中
1. inline/inline-block元素
	设置其父元素
	
	```
	div{text-align: center;}
	```
	**inline元素应注意元素间的空格／回车**
2. block元素

	```
	div{
      width: 960px;
      margin: 0 auto;
    }  
	```
3. 使用position属性，先left:50%，再right:50%	
	
### 垂直居中
1. 单行

	```
	p{line-height: 44px;}
	```
2. 多行

	（1）父元素定高
	借助inline-block元素的 `vertical-align middle`

	（2）父元素不定高
	
	可参照以下方法
	
	**不定高元素内容水平垂直居中（如弹出框）**
	
	1）利用inline-block元素
	
	```
	/*stylus*/
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
	2）flexbox方案
	此方法会有兼容性问题，比如MX2不支持。
	
	```
	.align-center{
	  align-items: center;
	  display: flex;
	}  
	```
	[未知尺寸元素水平垂直居中](http://demo.doyoe.com/css/alignment/)
	
	3）display:table-cell
	
	支持IE8+/chrome/firefox，利用td的vertical-align:middle
	4）设置固定上下padding
	5）利用定位元素（IE8+）
	
	```
	.box {
	  background: olive;
	  height: 300px;
	  position: relative;
	}
	
	.box>div {
	  background: #69f;
	  position: absolute;
	  top: 0;
	  right: 0;
	  bottom: 0;
	  left: 0;
	  width: 200px;
	  height: 200px;
	  margin: auto;
	}
	```

## 清除浮动并使父元素有高度
1. 加空元素

	```
	.clear{
	  clear:both;
	  height:0;
	  line-height:0;
	  font-size:0;
	}
	```
2. 父级overflow:auto

	```
	.parent{
	  overflow:auto;
	  zoom:1;/*解决IE6/7兼容问题*/
	}
	```
3. 结合:after（bootStrap）

	bootstrap v4.0.0的实现
	
	```
	.clearfix:after{
	  content:"";
	  display:table;
	  clear:both;
	}    
	```
	之前的bootstrap实现
	
	```
	/*stylus*/
	.clearfix2
	  zoom 1
	  &:before,&:after
	    content ""
	    display table
	    line-height 0
	  &:after
	    clear both     
	 ```
## 文本省略

[多行文本溢出显示省略号(…)全攻略](http://www.css88.com/archives/5206) 

### 单行文本省略
```
p{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}  
``` 

### 多行文本省略
-webkit内核浏览器（移动端）

```
p{
  overflow: hidden;
  text-overflow: ellipsis;
  display:flex;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}  
```

注意：设置以上样式后设置其:first-line的样式不生效。
## 禁止浏览器自动填充表单
pc或手机的chrome浏览器中浏览器会根据表单的name自动填充域名下的表单项。解决方法：

1. h5的 `autocomplete="off"`  
	无效（但可以加上，也许有浏览器支持）
2. 清除输入框内容  
	无效（填充是发生在页面完全加载完）	
3. 将type=password改为text
	不推荐
4. 让浏览器填充隐藏的输入框

```
<input type="tel" name="phone" style="display:none;">
<input type="tel" name="phone" maxlength="11" autocomplete="off"> 
<input type="password" name="password" style="display:none;">
<input type="password" name="password" maxlength="11" autocomplete="off">
```
此方法可解决pc端chrome浏览器问题，效果是浏览器会弹出保存密码modal，但只会填充到隐藏的输入框里。

但mobile的chrome浏览器仍然会填充到显示出的输入框中，因此只能将from去掉以结局。

## 页脚自适应居底
使用flexbox方法
```
body
  display flex
  min-height 100vh
  flex-direction column
main
  flex 1
```


