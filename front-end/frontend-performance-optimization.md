#前端性能优化
##一. 优化总则
###（一）缓存
缓存无疑是非常重要的，在一个项目中一般以做好各级的缓存，而对于前端来说需要关注浏览器缓存以及localStorage/sessionStorage能做到的一些事情（如缓存常用js文件／图片等）。

推荐阅读
强烈推荐[AlloyTeam的web缓存机制系列](http://www.alloyteam.com/2012/03/web-cache-1-web-cache-overview/)的六篇文章

[浅谈Web缓存](http://www.alloyteam.com/2016/03/discussion-on-web-caching/?utm_source=tuicool&utm_medium=referral) 比较系统地介绍缓存机制

[浅谈浏览器http的缓存机制](http://www.360doc.com/content/16/0405/10/30136251_547971176.shtml)

[关于Web静态资源缓存自动更新的思考与实践](http://web.jobbole.com/82838/)
###（二）节省字节
1. css和js文件压缩
2. 一些css属性可简写
3. css或javascript可用单引号。

##二. HTML优化
###（一）节省字节
1. 非同源url或协议相同可省略，写作`//:`

##三. CSS优化
###（一）节省字节
1. 0px等省略单位
2. 纯小数可省略0

	```
	.mask
	  opacity .6
	```
3. 十六进制颜色使用小写缩写形式
4. 
	可设置编辑器使用小写颜色编写，如webstorm
		
	![webstorm-hex colors](images/webstorm-hex colors.tiff)
4. 引号
5. 
	可以省略的引号则省略
	
	```
	input[type=button]
	  ...
	```
	不可省略的可使用单引号
	
	```
	div:after
	  content ''
	```

###（二）属性书写顺序
按照规范的css书写顺序(最初由Mozilla提出)，便于浏览器渲染，同时提高了代码的阅读体验。

Andy Ford是HTML和CSS方面的专家，最近写了一篇文章：[Order of the Day: CSS Properties](http://aloestudios.com/2009/02/order-of-the-day-css-properties/) . 文章推荐的CSS书写顺序为：

1. Display & Flow
2. Positioning
3. Dimensions
4. Margins, Padding, Borders, Outline
5. Typographic Styles
6. Backgrounds
7. Opacity, Cursors, Generated Content

Sublime Text编辑器可安装 [CSSComb](http://csscomb.com/docs)

###（三）css lint
1. 加载性能  不要用import、压缩等，主要是减少文件体积、减少阻塞加载、提高并发方面
2. 选择器性能 （非重点）
3. 渲染性能 （重点）
4. 可维护性、健壮性

##四. javascript优化
待续－－

