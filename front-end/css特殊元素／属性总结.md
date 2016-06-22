#css特殊元素／属性总结
##inline元素
###高度依浏览器而异
inline元素设置line-height后，与其高度不想等，不同浏览器渲染有差异。因此，设置inline元素float或子元素相对于其定位时，均会出现问题。可将元素设置为inline-block以解决此类问题。
##margin属性
###垂直外边距合并
1. 两个垂直外边距相遇时，将形成一个外边距，值等于两者中的较大者。
2. 子元素的margin-top会赋给最近一层有效border-top或padding-top的父级（IE的haslayout渲染则无此问题）
##border属性
border:none 不渲染，但有兼容性问题（IE6/7），可同时设置background:none。
border:0 会渲染，因此会占用内存，但无兼容问题。
##outline属性
位于边框边缘的外围，不占据空间。
IE／Firefox/chrome默认点击a/button会出现一个轮廓框。 
outline的none值与0值表现基本同border，但考虑到用户体验PC端不建议去除。

```
a
  overflow hidden
```
可有效解决轮廓被拉长问题（旧版的Opera有bug）
[页面可用性之outline轮廓外框的一些研究－－张鑫旭](http://www.zhangxinxu.com/wordpress/2010/01/页面可用性之outline轮廓外框的一些研究/)

##:before,:after
:before,:after插入的内容默认是一个行内元素，并且在HTML源代码中无法看到，这就是为什么被称之为伪元素的理由，所以也就无法通过DOM对其进行操作（可通过改变住元素的class）。
##line-height
1. line-height: 1.5
2. line-height: 150%
3. line-height: 1.5em
4. line-height: 36px
建议设置line-height为数值，它的子元素将根据其font-size重新设置line-height；其它情形则由像素值或计算出的像素值直接继承。