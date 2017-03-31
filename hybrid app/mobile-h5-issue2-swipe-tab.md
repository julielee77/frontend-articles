# 移动端h5问题探索(2)左右滑切换tab
移动端h5开发经常会用到滑动效果，但浏览器并没有提供swipe事件，可以用touch事件模拟。注意必须采用javascript的addEventListener绑定监听事件。

参考[HTML5的javascript touch事件](http://hedgehogking.com/?p=556)

## 主要涉及知识点
1. 移动端touch事件
2. 阻止左右滑默认事件

## touch事件

1. ### touchstart
	触摸开始
	
2. ### touchmove
	触摸拖动
	
	可在此阻止页面滚动 `event.preventDefault();`
	
3. ### touchend
	触摸结束	
	
4. ### touchcancel
	触摸取消（官方无详细说明，作为不支持touchend时的替代方案）

注：touchend部分手机不支持，如小米4，可用touchcanel代替（iphone SE不支持，但支持touchend）。
## 每个touch事件都包含了3个touch对象列表

1. ### touches
	当前屏幕上所有触摸点的列表
	
2. ### targetTouches
	targetTouches当前对象上所有触摸点的列表
	
3. ### changedTouches
	涉及当前事件的触摸点的列表
	
## 每个touch对象有如下属性
1. target

	触摸目标的DOM节点坐标 
2. identifier
 
	触摸的唯一ID，一般为从0开始的流水号	
3. clientX/clientY

	触摸目标在视口中的x/y坐标
4. pageX/pageY

	触摸目标在页面中的x/y坐标
5. screenX/screenY

 	触摸目标在屏幕中的x坐标
 	
6. radiusX/radiusY/rotationAngle
	
	手指触摸区域椭圆形的半径和旋转角度（低版本浏览器不支持）	

## touch示例
[demo](https://julielee77.github.io/demo/0003.html) 当触摸屏幕时，tip消失；触摸结束，tip显示。

## 实现方法
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

