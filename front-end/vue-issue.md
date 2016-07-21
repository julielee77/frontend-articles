#Vue实践总结
###click延迟
###for渲染对象顺序
ios和android顺序不一样，应避免使用。

此方法类似`for attr in object`再判断`hasOwnproperty`，故遍历结果顺序不确定。