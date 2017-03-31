# tap事件
 由于移动端click事件有300ms的延迟，推荐用tap事件代替。
 
## 定义 
当touchstart/touchend发生时，如果手指为同一位置且没有touchmove时，即认为一个tap事件。
## tap“穿透”
现象：当使用tap事件让一个蒙层消失时，会触发蒙层覆盖部分的click事件。

解释：因为tap后蒙层消失，但300ms后click事件会被触发，就被覆盖部分的元素接受了。
 
解决方法：
1. 使用缓动动画，过渡300ms的延迟
2. 中间层DOM元素，让其接受此“穿透”
3. 全部都是click事件（但避免不了a标签这种原生的click）
4. 改用Fastclick的库（zepto.js已修复）


 

