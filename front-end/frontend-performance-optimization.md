# 前端性能优化
为了优化网页加载速度，以及便于扩展或维护等，通常需要优化前端代码或配置。

- [优化总则](#优化总则)
- [html优化](#html优化)
- [css优化](#css优化)
- [javascript优化](#javascript优化)

## 优化总则
1. ###缓存
	缓存无疑是非常重要的，在一个项目中一般已做好各级的缓存（由客户端和后端），而对于前端来说需要关注浏览器缓存以及localStorage/sessionStorage能做到的一些事情（如缓存常用js文件／图片等）。
	
	**推荐阅读：**
	
	[AlloyTeam的web缓存机制系列](http://www.alloyteam.com/2012/03/web-cache-1-web-cache-overview/)的六篇文章（强烈推荐）
	
	[浅谈Web缓存](http://www.alloyteam.com/2016/03/discussion-on-web-caching/?utm_source=tuicool&utm_medium=referral) 比较系统地介绍缓存机制
	
	[浅谈浏览器http的缓存机制](http://www.360doc.com/content/16/0405/10/30136251_547971176.shtml)
	
	[关于Web静态资源缓存自动更新的思考与实践](http://web.jobbole.com/82838/)
	
2. ###尽量减少HTTP请求的个数
	- CSS sprite 合并多个背景图到一个单独图像（单张不高于200k）
	- 使用一些loader，合并js/css，并按需加载
3. ###DRY和可维护性
	- 压缩css和js文件 
	
		常用工具YUI Compressor, uglify-js,jsMin以及一些loader
	- **img** 不要在文档中缩放图像	
	- **HTML** 非同源url或协议相同可省略，写作`//`	
	- **css**
		- 简写属性
		
			此外，展开式属性并不能清空相关的其它属性。当然，在特定需求下，简写属性与展开式属性可合理配合。
		- 可省略引号或用单引号
		- 0px等省略单位
		- 纯小数可省略0
		- 十六进制颜色使用小写缩写形式 `#69f`
	- **js** 使用单引号	
	
	Sublime Text编辑器配置SublimeLinter对js/css/html进行代码检测。
	
## html优化
1. ###明确声明文档类型，避免混杂模式（quirk mode）
   `<!DOCTYPE html>`

2. ###`<html lang="">`

	方便语音合成工具或翻译工具
3. ###HTML属性顺序
	HTML属性应当按照以下顺序排列，确保代码的易读性
	
	`class`, `id`, `name`, `data-*`
	 
	`src`, `for`, `type`, `href` 
	
	`title`, `alt`
	 
	`aria-*`,`role`	

## css优化

1. ### 慎重选择高消耗的样式(Expensive styles)	
	- 合理安排selector
	- 高消耗属性（绘制前浏览器需要进行大量计算）
		`box-shadow` `border-radius` `transparency` `transform`
	
		css filters（性能杀手）
2. ### 避免过分重绘(reflow)		
	例如，改变layout相关属性会触发重新布局，动画中应避免。如：
	 `width`, `height`, `margin`, `padding`, `border` , `top`, `right`, `bottom`, `left`, `position`, `display`, `float`, `overflow`等
	
	不会触发重新布局的属性： `transform`, `color`, `background`等
3. ### css reset
	**reset.css** 重置全部效果，讲求浏览器的一致性
	
	**normalize.css** 注重通用方案，重置掉该重置的部分，保留有用的user Agent样式，同时修复一些bug
	
	**neat.css**  reset + normalize，此外还有以下特性
	- 人性化的细节处理
	- 考虑移动设备
	- 考虑响应式
	- 跨平台最佳font-family
	- 解决表单渲染问题
	- 面向未来
4. ### 属性书写顺序
	按照规范的css书写顺序(最初由Mozilla提出)，便于浏览器渲染，同时提高了代码的阅读体验。
	
	Andy Ford是HTML和CSS方面的专家，最近写了一篇文章：[Order of the Day: CSS Properties](http://aloestudios.com/2009/02/order-of-the-day-css-properties/) . 文章推荐的CSS书写顺序为：
	
	- Display & Flow `display` `float` `clear`
	- Positioning `position` `top/right/bottom/left`
	- Dimensions `z-index `
	- Margins, Padding, Borders, Outline `box-sizing` `margin/padding/border/outline`
	- Typographic Styles `font` `text-*` `color`
	- Backgrounds `background`
	- Opacity, Cursors, Generated Content `opacity` `coursor` `content` ^^^
	
	Sublime Text编辑器可安装 [CSSComb](http://csscomb.com/docs) 
	
	兼容前缀：（1）标准的属性放在后面（2）IE无需写，它使用的是标准属

5. ### css lint（补充）
	- 加载性能  不要用import、压缩等，主要是减少文件体积、减少阻塞加载、提高并发方面
	- 选择器性能 （非重点）
	- 渲染性能 （重点）
	- 可维护性、健壮性
	
推荐参考[CSS Guide](http://cssguidelin.es) By [Harry Roberts](http://csswizardry.com/work/)的规范。
## javascript优化
1. DOM的多个读操作，应放在一起。两个读操作间，不要插入写操作
2. 如果某个样式是通过重排得到的，那么最好缓存结果。
3. 不要一条条地改变样式，而要通过改变class或csstext属性，一次性改变样式。
4. 最好使用离线DOM，而不是真实的网面DOM来改变元素样式。 如docuementFragment对象
5. 先将元素设为display:none，然后对这个节点进行n次DOM操作，最后回复显示。
6. position:absolute或fixed的元素，重排开销会较小
7. 只在必要的时候，才将元素的display属性为可见
8. 使用虚拟DOM的脚本库，比如React等。
9. 使用window.requestAnimationFrame()、window.requestIdleCallback()调节重新渲染
requestAnimationFrame android 4.4.4以上支持  requestIdleCallback只有chrome支持

待续－－

