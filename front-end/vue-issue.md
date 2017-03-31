# Vue实践总结
## Vue问题总结
### click延迟
页面初次加载立即触发click事件会无响应，需滑动页面再触发。

猜测是DOM还未渲染好或事件还未绑定。

//TODO 待解决
## v-for渲染对象顺序
ios和android顺序不一样，应避免使用。(vue 2.0已改为v-repeat)

此方法类似`for attr in object`再判断`hasOwnproperty`，故遍历结果顺序不确定。
## 插件
### vue-resource
实现ajax功能，并配置请求信息
### vue-touch
移动端手指事件组件库

该组件库使用hammer.js封装，但目前不支持vue 2.0。（2016.11.22）
