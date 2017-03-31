# 常用算法／功能的javascript实现
- [算法](#算法)
- [功能函数](#功能函数)
- [效果实现](#效果实现)
## 算法
### 排序算法
1. 冒泡排序

	基本思想：每轮循环找出最小值（或最大值），每次比较两个相邻的元素，若顺序错误则交换。时间复杂度为O(N^2)

	```
	function bubbleSort(arr) {
	  var length = arr.length;
	  for (var i = 0; i < length; i++) {
	    for (var j = i + 1; j < length; j++) {
	      if (arr[j] < arr[i]) {
	        //若比前一个元素小，则交换值
	        var temp = arr[j];
	        arr[j] = arr[i];
	        arr[i] = temp;
	      }
	    }
	  }
	  return arr;
	}
	```
2. 插入排序

	插入排序分为直接插入排序和折半插入排序，这里讨论直接插入排序。
	
	基本思想：每轮将当前元素及在前的元素排序，即当前元素从右至左依次与在前的元素比较，若顺序错误则插入。
	
	```
	function insertionSort(arr) {
	  var length = arr.length;
	  for (var i = 1; i < length; i++) {
	    if (arr[i - 1] > arr[i]) {
	      var v = arr[i];
	      for (var j = 0; j > 0 && arr[j - 1] > temp; j--) {
	        arr[j] = arr[j - 1]; //若比当前temp值大，则后移一位
	      }
	      arr[j] = v; //v代替arr[j]
	    }
	  }
	  return arr;
	}
	```

其它排序算法：桶排序、快速排序（每次将一个基准数归位）
## 功能函数
### 数据类型判断
1. typeof 

	=>string number boolean undefined function object 
	
	注：typeof检测正则表达式时，IE／Firefox返回`object`，Safari／chrome返回`function`。
2. object类型再进一步判断，可使用

	- instanceof
	- Obejct.prototype.toString().call =>[obejct array]…
	- constructor
	- Array.isArray() //ES5
	
```
// 判断数据类型的简单实现
function identifyType(val) {
  var type = typeof val;
  //非object类型 string,boolean,number,undefined,function
  if (type !== 'object') {
    return type;
  }
  //obejct类型使用prototype判断，如[object Array]
  return Object.prototype.toString.call(val).slice(8, -1); 
}
}
```	
### native js实现ajax
运用用XMLHttpRequest对象的方法和事件实现，但需注意IE6-的兼容性。

以下将其实现封装到一个构造函数中。

```
var XMLHttp = function() {
  var xhr;
  if (window.XMLHttPRequest) { //现代浏览器
    xhr = new XMLHttPRequest();
  } else if (window.ActiveXObject) { //IE6-
    xhr = new ActiveXObject();
  }
  if (xhr === undefined || xhr === null) { //其它不支持情况报错
    throw new Error('创建AMLHttpRequest对象失败');
  } else {
    return xhr;
  }
};
XMLHttp.prototype.send = function(url, method, async, data, fn) {
  var params = null;
  if (method.toUpperCase() === 'GET') {
    //GET方法需处理传递参数
    if (data !== undefined || data !== null) {
      url += '?' + parseData(data);
    }
  } else {
    params = JSON.stringify(data);
    //POST方法设置请求头
    this.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
  }
  //建立连接
  this.open(url, method, async);
  //发送请求及其数据
  this.send(params);
  //监听XMLHttpRequest对象的状态
  this.onreadystatechange = function() {
    //当请求完成，且状态为200 OK时，接收数据并处理
    if (this.readyState == 4 && this.status == 200) {
      fn(this.responseText);
    }
  };
  /*parse data
   ** data 可能为string或object
   */
  function parseData(data) {
    if (data === undefined || data === null) {
      return;
    }
    if (typeof data == 'string') {
      return data;
    }
    var str = '';
    for (var attr in data) {
      if (data.hasOwnProperty(attr)) {
        str += attr + '=' + data[attr] + '&';
      }
    }
    str = str.slice(0, -1);
    return str;
  }
};
```
## 效果实现
## 变速动画（js）
实现思路：对所有属性变化设置一个定时器，并设置一个标志符来标记是否停止。

```
/*ele--- DOMElement
 **json -- 动画属性数据，如{"width":"300px"}
 **fn -- callback function
 */
function changeAttr(ele, json, fn) {
  clearInterval(ele.timer);
  ele.timer = setInterval(function() {
    var bStop = true; //动画停止标志，当全部属性到达最终值时停止
    for (var attr in json) {
      var curAttr = getStyle(ele, attr);
      curAttr = attr == 'opacity' ? parseFloat(curAttr) : parseInt(curAttr);
      var value = json[attr];
      var speed = 0;
      if (attr == 'opacity') {
        if (Math.abs(value - curAttr) <= 0.05) {
          curAttr = value;
        } else {
          speed = parseInt(parseFloat(value - curAttr) * 100 / 10);
        }
        ele.style[attr] = curAttr + speed / 100;
        ele.style.filter = 'alpha(opacity=' + curAttr * 100 + speed + ')';
      } else {
        if (Math.abs(value - curAttr) <= 10) {
          curAttr = value;
        } else {
          speed = parseInt((value - curAttr) / 5);
        }
        ele.style[attr] = curAttr + speed + 'px';
      }
      if (value - curAttr) {
        bStop = false;
      }
    }
    if (bStop) {
      clearInterval(ele.timer);
      if (fn) {
        fn();
      }
    }
  }, 100);
}
```
并封装getStyle(obj,attr)获取DOMElement的属性值。其实现如下：

```
function getStyle(ele, attr) {
  if (ele.currentStyle) { //IE
    if (attr == 'opacity') {
      return ele.currentStyle.filter.match(/^d+/) / 100; //=>0.3 format
    }
    return ele.currentStyle[attr];
  } else { //现代浏览器
    return getComputedStyle(ele, null)[attr];
  }
}
```
	

