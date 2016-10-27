#常用算法的javascript实现
##排序算法
###冒泡排序
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
###插入排序
插入排序分为直接插入排序和折半插入排序，这里讨论直接插入排序
```
function insertSortion(arr) {
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
