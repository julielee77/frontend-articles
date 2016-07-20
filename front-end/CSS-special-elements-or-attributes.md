#css特殊元素／属性总结
##inline元素
###高度依浏览器而异
inline元素设置 `line-height`后，与其高度不相等，不同浏览器渲染有差异。因此，设置inline元素 `float`或子元素相对于其定位时，均会出现问题。

可将元素设置为 `inline-block`以解决此类问题。若需垂直居中，可再添加 `vertical-align:middle`。

注：**元素设置position:absolute ／ float后将变为inline-block元素。**
##margin属性
###垂直外边距合并
1. 两个垂直外边距相遇时，将形成一个外边距，值等于两者中的较大者。
2. 子元素的 `margin-top`会赋给最近一层有效 `border-top`或 `padding-top`的父级（IE的 `haslayout`渲染则无此问题）

###inline/inline-block元素间的margin
inline/inline-block元素间的空格／回车会形成一定的默认margin。
（回车可使用 `<!-- -->`消除）

##border属性
`border:none` 不渲染，但有兼容性问题（IE6/7），可同时设置`background:none`。
`border:0` 会渲染，因此会占用内存，但无兼容问题。
##outline属性
位于边框边缘的外围，不占据空间。
IE／Firefox/chrome默认点击 `a`/ `button`会出现一个轮廓框。 
 `outline`的none值与0值表现基本同 `border`，但考虑到用户体验PC端不建议去除。

```
a
  overflow hidden
```
可有效解决轮廓被拉长问题（旧版的Opera有bug）

[页面可用性之outline轮廓外框的一些研究－－张鑫旭](http://www.zhangxinxu.com/wordpress/2010/01/页面可用性之outline轮廓外框的一些研究/)

`outline-radius`只有firefox支持。

##:before,:after
:before,:after插入的内容默认是一个行内元素，并且在HTML源代码中无法看到，这就是为什么被称之为伪元素的理由，所以也就无法通过DOM对其进行操作（可通过改变住元素的class）。
##line-height
1. `line-height: 1.5`
2. `line-height: 150%`
3. `line-height: 1.5em`
4. `line-height: 36px`

建议设置 `line-height`为数值，它的子元素将根据其 `font-size`重新设置 `line-height`；其它情形则由像素值或计算出的像素值直接继承。
##overflow:hidden
如果 `overflow`被设置为 `hidden`，元素的内容会在元素的边界处剪裁，不过不会提供滚动接口使用户访问到超出剪裁区域的内容。
##css继承
- 所有元素可继承： `visibility`和 `cursor`
- 内联元素和块元素可继承： `letter-spacing`、`word-spacing`、`white-space`、`line-height`、`color`、`font`、 `font-family`、`font-size`、`font-style`、`font-variant`、`font-weight`、`text- decoration`、`text-transform`、`direction`
- 块状元素可继承： `text-indent`和`text-align`
- 列表元素可继承： `list-style`、`list-style-type`、`list-style-position`、`list-style-image`
- 表格元素可继承： `border-collapse`
- 不可继承的： `display`、`margin`、`border`、`padding`、`background`、`height`、`min-height`、`max-height`、`width`、`min-width`、`max-width`、`overflow`、`position`、`left`、`right`、`top`、 `bottom`、`z-index`、`float`、`clear`、`table-layout`、`vertical-align`、`page-break-after`、 `page-bread-before`和`unicode-bidi`

##css动画
改变layout相关属性会触发重新布局，动画中应避免。如：
 `width`, `height`, `margin`, `padding`, `border` , `top`, `right`, `bottom`, `left`, `position`, `display`, `float`, `overflow`等

不会触发重新布局的属性： `transform`, `color`, `background`等
##文字换行
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
	
##z-index
默认值为auto。建议：非弹窗元素的z-index不大于2，避免层级混乱。

 更多详细内容参见[张鑫旭z-index－慕课视频](http://www.imooc.com/learn/643)
	



