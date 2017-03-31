# IE兼容性总结
（注：文中所涉及IE版本均包含自身。文中也会适当提及其它浏览器的兼容性。）
##概况
IE9+基本已和chrome兼容性差不多，而IE7-市场占有率基本可不考虑，因此一般考虑的是IE8的兼容性。

目前最新的windows10默认浏览器已换成spartan，而IE11将不做重大修改，这预示着微软已自动放弃IE浏览器。

补充：jquery框架1.X版本支持IE7-，解决了很多IE兼容性问题，且便于操作DOM。其2.X版本支持IE8+。
## HTML/CSS兼容情况
IE8-一般不支持HTML5和CSS3，无特殊情况以下将不讨论。
### HTML
### CSS
1. 外部样式

	若外部样式文件中有中文注释，IE6-会导致其无法生效。可在css文件开头使用 `@charset "utf-8";`
2. 选择器
	- 类／子代／相邻兄弟选择器 IE6-不支持
	
		IE6-不支持多类选择器（对应不支持HTML多类写法）
	
		```
		/*将匹配最后一个类*/
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

	 IE8-不支持，可使用respond.js解决。
	 
	 ```
	 <!--[if lt IE 9]><script src = "http://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script><![endif]--> 
	 ```
4. 盒模型

	IE6-不支持自动padding。
	
	IE7-不支持inline-block/inline-table。 
	
	IE6/7容易出现层级覆盖，因为z-index:auto会创建层叠上下文。
	
	利用拉伸实现全屏效果（IE7+）
	```
	div{
	  position:absolute;top:0;right:0;bottom:0;left:0;
	}
	```
	

## javascript兼容情况
### 核心JS
**IE8-一般不支持ES5＋。**

1. ES5支持字符串索引访问

	```	
	var str='abc';
	str[0]//IE7-不支持 =>str.charAt(0)
	```

### DOM
IE5开始支持DOM 1级，直到IE5.5才完全支持。IE8开始修复DOM的bug。

1. 节点树和元素树
	
	IE不支持元素树。对于节点树，IE与其它浏览器的解析不同，其它浏览器会包含Text节点。IE8-无内置Node对象。
	
	获取节点时，Chrome、Firefox、IE9+等现代浏览器下，获取到的是ElementNode和TextNode，它会将空格符、回车符、换行符也看做文本节点，而IE8-则会无视空格、回车符。
	
	<table>
    <caption>DOM操作相关属性／方法</caption>
    <tr>
      <th>功能</th>
      <th>属性／方法</th> 
      <th>说明</th>
    </tr>
    <tr>
      <td>获取子节点</td>
      <td> `childNodes`标准属性
      
      `children` 非标准属性</td>
      <td>`children`不包含TextNode。但IE6会包含CommentNode。  </td>
    </tr>
    <tr>
      <td>获取兄弟节点</td>
      <td>nextSibling/previousSibling</td>
      <td></td>
    </tr>
    <tr>
      <td>获取兄弟元素</td>
      <td>nextElementSibling/previousElementSibling</td>
      <td></td>
    </tr>
    <tr>
      <td>获取某位置节点</td>
      <td>firstChild/lastChild</td>
      <td></td>
    </tr>
    <tr>
      <td>获取某位置元素</td>
      <td>firstElementChild/lastElementChild</td>
      <td></td>
    </tr>
    <tr>
      <td>拷贝节点</td>
      <td>cloneNode()</td>
      <td></td>
    </tr>
    <tr>
      <td>获取／设置/创建属性</td>
      <td>getAttribute()/setAttribute()/createAttribute()/attributes[index]</td>
      <td>IE7-只能使用element.href获取／设置</td>
    </tr>
    <tr>
      <td>获取css样式</td>
      <td>currentStyle IE8-
      
      getComputedStyle()</td>
      <td>获取非內联样式，设置时使用element.style.cssAtrr</td>
    </tr>
    <tr>
      <td>获取／设置文本内容</td>
      <td>innerText IE8-textContent</td>
      <td></td>
    </tr>
  </table>
	 
	**查找元素**

	`querySeletor()`/ `querySeletorAll()` IE7~10只支持CSS2选择器，IE6-不支持此方法。
	
	`getElementsByClassName` IE10-不支持。	
	
2. DOM事件
	- 事件模型
	
		IE使用冒泡事件模型，其它现代浏览器使用捕获冒泡事件模型。
		```
		function XXX(event){
			event= event||window.event;//IE中使用全局Event对象
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
		    <td>event.preventDefault()</td>
		    <td>event.returnValue=false</td>
		  </tr>
		  <tr>
		    <td>阻止事件传播</td>
		    <td>event.stopPropagation()</td>
		    <td>event.cancelBubble=true</td>
		  </tr>
		  <tr>
		    <td>事件源</td>
		    <td>event.target  ff/chorme</td>
		    <td>event.srcElement  IE8-/chrome</td>
		  </tr>
		  <tr>
		    <td>按键码</td>
		    <td>event.keyCode/Event.which  ff/chrome</td>
		    <td>event.charCode IE8-</td>
		  </tr>
		</table> 
		
		setCapture/releaseCapture IE8-专用
	- 事件绑定
	
	    DOM 2级事件
		```
		myBtn.attachEvent('onclick',myFunc);  //IE8-
		myBtn.addEventListener('click',myFunc,?isCapture); //现代浏览器		
		```	
	- DOMContentLoaded  IE8- 不支持
	
		window的load事件则在全部资源加载完毕时才触发，DOM 3级标准推出了	DOMContentLoaded优化了这个过程。当文档加载解析完毕且所有延迟脚本都执行完毕时会触发DOMContentLoaded事件，此时图片和异步资源可能依旧在加载，但文档已经为操作准备就绪了。
		
		 DOM的兼容ready实现（大概同 jQuery）
		 
		```
		 function domReady(fn) {
		  //现代浏览器
		  if (document.addEventListener) {
		    document.addEventListener('DOMContentLoaded', fn, false);
		  } else {
		    IEContentLoaded(fn);
		  }
		  //IE模拟DOMContentLoaded
		  function IEContentLoaded(fn) {
		    var d = document,
		      done = false;
		    //执行一次用户回调函数init()
		    function init() {
		      if (!done) {
		        done = true;
		        fn();
		      }
		    }
		    //立即执行hack
		    (function() {
		      try {
		        d.documentElement.doScroll('left');
		      } catch (e) {
		        //延迟再试一次
		        setTimeout(arguments.callee, 50);
		        return;
		      }
		      //直到没有错误
		      init();
		    })();
		    //监听document的加载状态
		    d.onreadystatechange = function() {
		      //如果用户是在domReady之后绑定的函数，就立马执行
		      if (d.readyState == 'complete') {
		        d.onreadystatechange = null;
		        init();
		      }
		    };
		  }
		}
		 ```
3. ajax

	IE6-不兼容Ajax的XMLHttpRequest对象，使用ActiveXobject。
4. window.close()

	IE下提示是否需要关闭，ff无法执行并报错（不能关闭非脚本打开的页面）		

