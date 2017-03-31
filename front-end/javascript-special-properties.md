# javascript特殊属性总结
## DOM
### 尺寸相关
1. window 窗口

	`innerWidth` ／`innerHeight`  （ IE9＋）
	
	`outerWidth` ／`outerHeight`（IE9+）
	
	`screenLeft` ／`screenTop`
	
	`screenX` ／`screenY`
2. screen 设备
	
	`width` ／`height`
	`availWidth` ／`availHeight`
3.  其它尺寸//TODO 待详细分析
	- client
		
		＝ content－滚动距离-滚动条（如果占据尺寸）+padding 无兼容问题
		
		`clientWidth` /`clientHeight` 
		
		`clientLeft` /`clientTop`
	- offset
		
		 ＝content－滚动距离+padding+border --chrome/firefox/safari
		 
		`offsetWidth` /`offsetHeight`  
		 
	 	`offsetLeft` /`offsetTop`

	- scroll
	
		＝ content+padding --chrome/firefox/safari

		`scrollWidth` /`scrollHeight` 
		    
		`scrollLeft` /`scrollTop`（可读写）
		
由于firefox等对body高度的解析不同，故应注意：

```
var clientWidth=(document.documentElement||document.body).clientWidth
```		

scrollTop/scrollLeft可设置（无px单位），使DOM元素滚动到某处。		
### element.classList
使用className属性操作class时，需要考虑元素原有class。而classList属性返回所有class的数组，有`add()`、 `remove()` 、`toggle()`、 `contains()`、 `item(index)`、 `length`等方法和属性。且每次只能添加／删除一个类。

支持IE10+，Android 3+（4.4+完全支持）。

### 阻止事件默认行为
- `event.preventDefault()`

	firefox/chrome／IE9+
- `event.returnValue=false`

	IE/chrome
- `return false`
	查找到的此方法，但经测试均不支持

移动端对前两种方法均支持。示例参见[移动端h5问题探索(1)支付密码输入框](https://github.com/JulieLee77/frontend-articles/blob/master/hybrid%20app/mobile-h5-issue1-pay-password.md)
### document.activeElement.blur()
`document.activeElement`可获取当前聚焦元素。

`element.blur()`基本不会用，因为当需要改变聚焦元素直接调用 `element.focus()`即可，对于不支持直接 `focus()`的环境先调用 `blur()`也解决不了（如ios）。
### window.reload()
刷新页面
###getComputedStyle()
支持IE9+/chrome/firefox，IE8-对应的方法为 `element.currentStyle`。

`getComputedStyle(element,null).height` 获取的是原始值，包括auto/%。 可用 `clientHeight`、`offsetHeight`获取计算后的px。值。
