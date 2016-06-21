#css特殊元素／属性总结
##inline元素
###高度依浏览器而异
inline元素设置line-height后，与其高度不想等，不同浏览器渲染有差异。因此，设置inline元素float或子元素相对于其定位时，均会出现问题。可将元素设置为inline-block以解决此类问题。
##margin属性
##垂直外边距合并
1. 两个垂直外边距相遇时，将形成一个外边距，值等于两者中的较大者。
2. 子元素的margin-top会赋给最近一层有效border-top或padding-top的父级（IE的haslayout渲染则无此问题）

