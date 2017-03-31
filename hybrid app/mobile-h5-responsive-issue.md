# 移动端h5适配问题
参考文章  

[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)

[移动端高清、多屏适配方案](http://div.io/topic/1092?utm_source=tuicool&utm_medium=referral)

补充一些用于**css media query**概念：

1. **dpr（device-pixel-ratio，设备像素比）**

	*dpr=物理像素/设备独立像素*
	
	因为retina高清屏幕的出现，才有了dpr的概念。比如，css像素为*1px × 1px*的块，在*dpr=2*的屏幕中物理像素为*2px × 2px*的块。

2. **max-device-width与max-width**

	max-device-width指设备屏幕的宽度，max-width指浏览器的宽度。两者有区别的情形：（1）在浏览器中打开h5页面（2）当设备横屏时

以下将讨论h5页面适配中的几种尺寸的一般方案，具体元素的适配可能需要根据效果具体调整。

首先，加载一段求出dpr，并添加为html的 `data-dpr`属性

```
//放在head里，需要在其它资源之前加载
    document.documentElement.setAttribute('data-dpr',window.devicePixelRatio|1);
```
这样，之后就可以用html[data-dpr=*]写一些css hack了。
# 图片
应该适应使用高清多倍图片，最好不同的dpr适应不同的图片文件。
假设以*750X1334*（iphone 6，*dpr=2*）做设计基准，有一个*400X600*的图，那么css只需写

```
img{
  width:200px;
  height:300px;
}
```
而在不同设备中加载不同图片，就显示相应尺寸的图片。

**参考做法**：先切最大倍的图，然后用图片处理器等比缩放并命名，最后得到*3X* /*2X* /*1X*的图。
（这里建议命名方式以 *imageName_1X.png*这种方式）
# 字体/间隙
为了在大屏下显示更多的内容，字体和有些间隙不适合用rem适配，可以写成固定或设定几个适配的px值。

**参考做法**：写一个.mixin函数，然后设置css media query的时候调用。形如：

```
.px2px(font-size, 32);
```

以下为参考的less的.mixin函数（可能需要适当修改）

```
.px2px(@name, @px){
    @{name}: round(@px / 2) * 1px;
    [data-dpr="2"] & {
        @{name}: @px * 1px;
    }
    // for mx3
    [data-dpr="2.5"] & {
        @{name}: round(@px * 2.5 / 2) * 1px;
    }
    // for 小米note
    [data-dpr="2.75"] & {
        @{name}: round(@px * 2.75 / 2) * 1px;
    }
    [data-dpr="3"] & {
        @{name}: round(@px / 2 * 3) * 1px
    }
    // for 三星note4
    [data-dpr="4"] & {
        @{name}: @px * 2px;
    }
}
```
# 其它控件
推荐用%、vw、vh或rem（font-size of the root element）单位。设置html的font-size（基准值），不同元素就可以根据设置的rem值自适应了。

*元素的尺寸=px/基准值*

*基准值=clientWidth×dpr/10*  （推荐用此算法）

比如，设计基准是iphone 6。*基准值=375×2/10=75px*，加入一个元素在设计图中是100px，那么尺寸为*100/75=1.3333rem*。

这时候，就产生了两种设置基准值用于适配的思路：

1. 用js给每个设备设置基准值

	用js获取*clientWidth*并求出基准值后，给html元素加*style:font-size=基准值*。（可与设置data-dpr属性同时）
2. 用css media query写一些适配

	例如：
	
	```
	@media screen and (max-device-width: 320px){
	      html[data-dpr="1"]{
	        font-size:32px;
	      }
	      html[data-depr="2"]{
	        font-size:64px;
	      }
	    }
	    @media screen and (max-device-width: 360px){
	      html[data-dpr="1"]{
	        font-size:36px;
	      }
	      html[data-depr="2"]{
	        font-size:72px;
	      }
	      html[data-dpr="3"]{
	        font-size:108px;
	      }
	    }
	```