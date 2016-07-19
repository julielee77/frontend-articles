#javascript特殊属性总结
##element.classList
支持IE10+，Android 2.3（不包括）+

API `length`, `item`, `add`, `remove`, `contains`, `toggle`
##element.scrollTop=XX
使DOM元素滚动到某处，无px单位
##阻止事件默认行为
- event.preventDefault()

	firefox/chrome／IE9+
- event.returnValue=false

	IE/chrome
- return false
	查找到的此方法，但经测试均不支持

移动端对前两种方法均支持。示例参见[移动端h5问题探索(1)支付密码输入框](https://github.com/JulieLee77/frontend-articles/blob/master/hybrid%20app/mobile-h5-issue(1)pay-password.md)
##document.activeElement.blur()
`document.activeElement`可获取当前聚焦元素。

`element.blur()`基本不会用，因为当需要改变聚焦元素直接调用 `element.focus()`即可，对于不支持直接 `focus()`的环境先调用 `blur()`也解决不了（如ios）。
##window.reload()
刷新页面