#移动端h5问题探索(2)左右滑切换tab
移动端h5开发经常会用到滑动效果，但浏览器并没有提供swipe事件，可以用touch模拟。
##主要涉及知识点
1. 移动端touch方法
2. 阻止ios左右滑默认事件
##touch相关方法
###touchstart
触摸开始
###touchmove
触摸拖动
###touchend
触摸结束
###touchcancel
触摸取消
##touch相关属性数组
###touches
当前屏幕上所有触摸点的列表
###targetTouches
targetTouches当前对象上所有触摸点的列表

###changedTouches
涉及当前事件的触摸点的列表
##touch示例
[demo](https://julielee77.github.io/demo/0003.html) 当触摸屏幕时，tip消失；触摸结束，tip显示。

##实现方法
[实现demo](https://julielee77.github.io/demo/0004.html)

**js关键部分**

```
//slide left & right, switch the tab
var xx, yy, XX, YY, swipeX, swipeY;
window.addEventListener('touchstart', function (event) {
  xx = event.targetTouches[0].screenX;
  yy = event.targetTouches[0].screenY;
  swipeX = true;
  swipeY = true;
}, false);
window.addEventListener('touchend', function (event) {
  XX = event.changedTouches[0].screenX;
  YY = event.changedTouches[0].screenY;
  if (swipeX && Math.abs(XX - xx) - Math.abs(YY - yy) > 0) {//左右滑动
    event.stopPropagation();
    event.preventDefault();
    swipeY = false;
    if (XX > xx && vm.tabIndex > 0) {
      vm.tabIndex--;
    } else if (XX < xx && vm.tabIndex < vm.tabNames.length - 1) {
      vm.tabIndex++;
    }
  } else if (swipeY && Math.abs(XX - xx) - Math.abs(YY - yy) < 0) {//上下滑动
    swipeX = false;
  }
});
```

