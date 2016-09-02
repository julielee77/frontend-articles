#IE兼容性总结
（注：文中所涉及IE版本钧包含自身。文中也会适当提及其它浏览器的兼容性。）
##概况
IE9+基本已和chrome兼容性差不多，而IE7-市场占有率基本可不考虑，因此一般考虑的是IE8的兼容性。

目前最新的windows10默认浏览器已换成spartan，而IE11将不做重大修改，预示着微软已自动放弃IE浏览器。

jquery框架1.X版本支持IE7-，解决了很多IE兼容性问题，且便于操作DOM。其2.X版本支持IE8+。
##HTML/CSS兼容情况
IE8-一般不支持HTML5和CSS3
##javascript兼容情况
###核心JS
**IE8-一般不支持ES5以上。**

1. 字符串元素访问

	```	
	var str='abc';
	str[0]//IE7-不支持 =>str.charAt(0)
	```

##DOM
1. DOM元素
	- 节点树和元素树
	
		IE不支持元素树。对于节点树，IE与其它浏览器的解析不同，其它浏览器会包含Text节点。
		`elment.childNodes` IE6～8支持，IE9+和chrome不支持。
	- 查找元素
	
		`querySeletor`/ `querySeletorAll` IE7~10只支持CSS2选择器，IE6-不支持。
		`getElementsByClassName` IE-不支持	
	- 核心DOM操作
		setAtrribute IE7-不支持，操作属性多使用HTML DOM方法。
		<table>
		  <tr>
		    <th>其它浏览器</th>
		    <th>IE8-</th>
		  </tr>
		  <tr>
		    <td>currentStyle</td>
		    <td>getComputedStyle</td>
		  </tr>
		  <tr>
		    <td>innerText</td>
		    <td>textContent</td>
		  </tr>
		</table>	
2. DOM事件
	- 事件模型
	
		IE使用冒泡事件模型，其它现代浏览器使用捕获冒泡事件模型。
		```
		function XXX(event){
			event= event||window.event;//firefox在js事件中无event对象，需由参数传递而来
		}
		```
	- 事件及属性
		<table>
		  <tr>
			<th></th>
			<th>其它浏览器</th>
			<th>IE</th>
		  </tr>
		  <tr>
		    <td>阻止默认事件</td>
		    <td>event.preventDefault();</td>
		    <td>event.returnValue=false;</td>
		  </tr>
		  <tr>
		    <td>阻止事件冒泡</td>
		    <td>event.stopPropagation();</td>
		    <td>event.cancelBubble=true;</td>
		  </tr>
		  <tr>
		    <td>事件源</td>
		    <td>event.target  ff</td>
		    <td>event.srcElement  IE8-</td>
		  </tr>
		  <tr>
		    <td>按键码</td>
		    <td>event.keyCode
		    Event.which  ff</td>
		    <td>event.charCode IE8-</td>
		  </tr>
		</table> 
		setCapture/releaseCapture IE8-专用
3. ajax
	解决IE6-不兼容Ajax的XMLHttpRequest（其它浏览器），使用ActiveXobject	

