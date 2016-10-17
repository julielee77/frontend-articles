#css特殊元素／属性总结
##选择器
选择器权值：（1）通用* 0.01（2）继承 0.1（3）标签 1（4）类 10（5）id 100（6）!important最高

选择器分类：（1）基本选择器（2）层次选择器（3）过滤选择器

- 后代选择器、子代选择器`>`、兄弟选择器`+`、通配选择器`*`
- 链接静态伪类： `:link`, `:visited`
- 动态伪类：  `:hover`, `:active`, `:focus`
- 结构伪类： `:first-child`, `:last-child`
- 其它伪类： `:left`, `:right`(仅用于@page规则，与装订线相关),     `:lang`
- 伪元素： `::first-letter`, `::first-line`, `::before`, `::after`
- CSS3选择器 通用兄弟选择器`~`、各种伪类选择器

<table>
<caption>属性选择器匹配示例</caption>
  <tr>
    <td>p[class~="myClass"]</td>
    <td>附带class属性的p元素，且class值列表包含myClass</td>
  </tr>
  <tr>
    <td>p[class^="b"]</td>
    <td>class属性值以“b”开头</td>
  </tr>
  <tr>
    <td>p[class*="b"]</td>
    <td>class属性值包含b</td>
  </tr>
  <tr>
    <td>p[class$="b"]</td>
    <td>class属性值以“b”结尾</td>
  </tr>
  <tr>
    <td>p[class｜="en"]</td>
    <td>class属性值为“en”或以"en-"开头</td>
  </tr>
</table>

<table>
<caption>伪类选择器</caption>
  <tr>
    <td rowspan="9">结构性伪类</td>
    <td>:root</td>
    <td>文档根元素html</td>
  </tr>
  <tr>
    <td>:nth-child(n)</td>
    <td>第n个子元素</td>
  </tr>
  <tr>
    <td>:nth-last-child(n)</td>
    <td>倒数第n个子元素</td>
  </tr>
  <tr>
    <td>:nth-of-type(n)</td>
    <td>同类型中第n个同级兄弟元素</td>
  </tr>
  <tr>
    <td>:first-child/ :last-child</td>
    <td></td>
  </tr>
  <tr>
    <td>:first-of-type/ :last-of-type</td>
    <td>同级兄弟元素中第一个元素</td>
  </tr>
  <tr>
    <td>:only-child</td>
    <td>唯一的子元素</td>
  </tr>
  <tr>
    <td>:only-of-type</td>
    <td>同类型中唯一兄弟元素</td>
  </tr>
  <tr>
    <td>:empty</td>
    <td>没有任何子元素（包括text节点）的元素</td>
  </tr>
  <tr>
    <td>目标伪类</td>
    <td>:target</td>
    <td>相关url指向的元素</td>
  </tr>
  <tr>
    <td rowspan="4">UI元素状态伪类</td>
    <td>:enabled</td>
    <td>表单处于可用状态的元素</td>
  </tr>
  <tr>
    <td>:disabled</td>
    <td>表单处于不可用状态的元素</td>
  </tr>
  <tr>
    <td>::selection</td>
    <td>元素被选中或出于高亮状态</td>
  </tr>
  <tr>
    <td>:checked</td>
    <td>表单处于选中状态的元素</td>
  </tr>
  <tr>
    <td>否定伪类</td>
    <td>:not(selector)</td>
    <td>--</td>
  </tr>
</table>
###:before,:after
:before,:after插入的内容默认是一个行内元素，并且在HTML源代码中无法看到，这就是为什么被称之为伪元素的理由，所以也就无法通过DOM对其进行操作（可通过改变住元素的class）。
##css继承
- 所有元素可继承： `visibility`和 `cursor`
- 内联元素和块元素可继承： `letter-spacing`、`word-spacing`、`white-space`、`line-height`、`color`、`font`、 `font-family`、`font-size`、`font-style`、`font-variant`、`font-weight`、`text- decoration`、`text-transform`、`direction`
- 块状元素可继承： `text-indent`和`text-align`
- 列表元素可继承： `list-style`、`list-style-type`、`list-style-position`、`list-style-image`
- 表格元素可继承： `border-collapse`
- 不可继承的： `display`、`margin`、`border`、`padding`、`background`、`height`、`min-height`、`max-height`、`width`、`min-width`、`max-width`、`overflow`、`position`、`left`、`right`、`top`、 `bottom`、`z-index`、`float`、`clear`、`table-layout`、`vertical-align`、`page-break-after`、 `page-bread-before`和`unicode-bidi`

总结：文本／列表相关属性可继承
##box-model
- ###inline元素
	**高度依浏览器而异**
	inline元素设置 `line-height`后，与其高度不相等，不同浏览器渲染有差异。因此，设置inline元素 `float`或子元素相对于其定位时，均会出现问题。
	
	可将元素设置为 `inline-block`以解决此类问题。若需垂直居中，可再添加 `vertical-align:middle`。
	
	注：**元素设置position:absolute ／ float后将变为inline-block元素。**
- ###line-height
	- `line-height: 1.5`
	- `line-height: 150%`
	- `line-height: 1.5em`
	- `line-height: 36px`

	建议设置 `line-height`为数值，它的子元素将根据其 `font-size`重新设置 `line-height`；其它情形则由像素值或计算出的像素值直接继承。
- ###margin
//TODO 待补充
	**block元素垂直margin 重叠**
	1. 两个垂直外边距相遇时，将形成一个外边距，值等于两者中的较大者。
	2. 子元素的 `margin-top`会赋给最近一层有效 `border-top`或 `padding-top`的父级（IE的 `haslayout`渲染则无此问题）
	
		解决方法：（1）父元素设置透明 `border-top`（2）改用 `padding-top`(3)bootstrap方法 `:before{content:"",display:table;}`

	**inline/inline-block元素间的margin**
	inline/inline-block元素间的空格／回车会形成一定的默认margin。
	（回车可使用 `<!-- -->`消除）
- ###border
	`border:none` 不渲染，但有兼容性问题（IE6/7），可同时设置`background:none`。
	
	`border:0` 会渲染，因此会占用内存，但无兼容问题。
- ###outline
	位于边框边缘的外围，不占据空间。
	IE／Firefox/chrome默认点击 `a`/ `button`会出现一个轮廓框。 (现代浏览器已解决)
	 `outline`的none值与0值表现基本同 `border`，但考虑到用户体验PC端不建议去除。
	
	```
	a
	  overflow hidden
	```
	可有效解决轮廓被拉长问题（旧版的Opera有bug）
	
	[页面可用性之outline轮廓外框的一些研究－－张鑫旭](http://www.zhangxinxu.com/wordpress/2010/01/页面可用性之outline轮廓外框的一些研究/)
	
	`outline-radius`只有firefox支持。
##positioning
- ###z-index
	默认值为auto。建议：非弹窗元素的z-index不大于2，避免层级混乱。
	
	 更多详细内容参见[张鑫旭z-index－慕课视频](http://www.imooc.com/learn/643)
- ###overflow
	overflow规定当内容溢出元素时发生的事情。值visible（默认）/hidden/scroll/auto/inherit（IE8-不支持）。
	
	`overflow-x`/`overlow-y`
	
	如果 `overflow`被设置为 `hidden`，元素的内容会在元素的边界处剪裁，不过不会提供滚动接口使用户访问到超出剪裁区域的内容。
	
##font/text
- ###文字换行
	- word-wrap
	
		`normal` 只在允许的断字点换行（默认值）
		
		`break-word` 在长单词或URL内部换行
	- word-break
	
		`normal` 使用浏览器默认的换行规则
		
		`break-all` 允许在单词内换行
		
		`keep-all` 只能在半角空格或连字符处换行
	- white-space
	
		`normal` 空白会被浏览器忽略（默认）
		
		`pre` 空白会被浏览器保留。其行为方式类似HTML中的pre标签
		
		`nowrap` 文本不会换行，文本会在同一行上继续，知道遇到br标签为止
		
		`pre-wrap` 保留空白符序列，但是正常地进行换行
		
		`pre-line` 合并空白符序列，但是保留换行符
		
		`inherit` 继承父级 
		
##颜色相关
###background
css3 增加了image相关的属性。

css2的background简写：

```
background: background-color || background-image || background-repeat || background-attachment || background-position;
```

css3的background简写：

```
background: background-image || background-position/background-size || background-repeat || background-attachment || background-clip || background-origin || background-color
```
此外，CSS3支持多背景，其主要作用就是给同一个元素设置多个背景图像，并可以给多个背景图像设置相同或不相同的background-(position||repeat||clip||size||origin||attachment)。
###渐变
线性渐变

```
.container {
  background: -webkit-linear-gradient(left, #69f 20%, #396 50%, red 80%);
  background: -o-linear-gradient(left, #69f 20%, #396 50%, red 80%);
  background: linear-gradient(to right, #69f 20%, #396 50%, red 80%);
}
```

径向渐变  `background: radial-gradient(ellipse at center, #0ff 0%, rgba(0, 0, 255, 0) 50%, #00f 95%);`

或使用 `gradient(type,...)`

**注：**web app开发时，有些低版本机型不支持background渐变，可改写为background-image渐变。		
##不常用css属性
###-webkit-font-smoothing:anialiased;
设置字体的抗锯齿或平滑度	
###-webkit-backface-visibility:hidden;
当元素不面向屏幕时是否可见
###transform-style:perserve-3d;
存在兼容性问题	

