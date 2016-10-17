#IE兼容性总结
（注：文中所涉及IE版本均包含自身。文中也会适当提及其它浏览器的兼容性。）
##概况
IE9+基本已和chrome兼容性差不多，而IE7-市场占有率基本可不考虑，因此一般考虑的是IE8的兼容性。

目前最新的windows10默认浏览器已换成spartan，而IE11将不做重大修改，这预示着微软已自动放弃IE浏览器。

补充：jquery框架1.X版本支持IE7-，解决了很多IE兼容性问题，且便于操作DOM。其2.X版本支持IE8+。
##HTML/CSS兼容情况
IE8-一般不支持HTML5和CSS3，以下将不讨论。
###HTML
###CSS
1. 外部样式
	若外部样式文件中有中文注释，IE6-会导致其无法生效。可在css文件开头使用 `@charset "utf-8";`
2. 选择器
	- 类选择器／子代选择器／相邻兄弟选择器 IE6-不支持
	
		IE6-不支持多类选择器（对应不支持HTML多类写法）
	
		```
		.urgent.warning{font-weight:bold};
		```	
	- 属性选择器
	
		IE6-不支持属性选择器子串匹配
	
		```
		p[class^="en"]{text-decoration:underline;}
		```
	- 伪类选择器
	
		**动态伪类选择器（`:focus`, `:hover`, `:active`）**
		
		IE6-只支持a元素的动态伪类选择。IE7支持所有元素的 `:hover`选择。	
		
		`:first-child` `:last-child`
		
3. HTML5响应式布局
4. 
	 IE8-不支持，可使用respond.js解决。
	 
	 ```
	 <!--[if lt IE 9]><script src = "http://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script><![endif]--> 
	 ```


##javascript兼容情况
###核心JS
**IE8-一般不支持ES5以上。**

1. ES5支持字符串索引访问

	```	
	var str='abc';
	str[0]//IE7-不支持 =>str.charAt(0)
	```

###DOM
1. DOM元素
	- 节点树和元素树
	
		IE不支持元素树。对于节点树，IE与其它浏览器的解析不同，其它浏览器会包含Text节点。
		
		`elment.childNodes` IE6～8支持，IE9+和chrome不支持。
	- 查找元素
	
		`querySeletor`/ `querySeletorAll` IE7~10只支持CSS2选择器，IE6-不支持。
		
		`getElementsByClassName` IE10-不支持	
	- 核心DOM操作
		setAtrribute IE7-不支持，操作属性多使用HTML DOM方法。
		<table>
		  <tr>
		    <td></td>
		    <th>其它浏览器</th>
		    <th>IE8-</th>
		  </tr>
		  <tr>
		    <td>获取css style属性</td>
		    <td>currentStyle</td>
		    <td>getComputedStyle</td>
		  </tr>
		  <tr>
		    <td>获取／设置文本内容</td>
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
	- 事件绑定
	
	    DOM 2级事件
		```
		myBtn.attachEvent('onclick',myFunc);  //IE8-
		myBtn.addEventListener('click',myFunc,?capture); //其它
		```	
3. ajax

	解决IE6-不兼容Ajax的XMLHttpRequest（其它浏览器），使用ActiveXobject
4. window.close()

	IE下提示是否需要关闭，ff无法执行并报错（不能关闭非脚本打开的页面）		

