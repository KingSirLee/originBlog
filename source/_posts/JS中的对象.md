title: JS中的对象
date: 2015-12-14 20:14:59
tags: [js,前端]
---
今天被自己大意坑了一下.
本来打算声明一个对象,存一下键值对,类似于java中的map,于是:
<!-- more -->
```
$scope.itemMap = [];
for(var i=0; i<= list.length; i++){
    $scope.itemMap.push(list[i], true);
}
```
接着就屁颠屁颠的在页面上拿来用,很显然,并拿不到.
于是检查代码,并没有什么报错,后来console.log一下$scope.itemMap
```
[12,true,34,ture]
```
呵呵,并不是自己想要的数据结构.后来检查发现是自己声明对象的时候错了,应该是:
```
$scope.itemMap = [];
for(var i=0; i<= list.length; i++){
    $scope.itemMap[list[i]] = true;
}
```

这样的数据结构是:
```
Object {12: true, 34: true}
```
这才是我想要的.
好吧,基础知识有必要巩固一下了.

**复习**:在JS中，[]表示数组，{}表示对象.
例如:
```
var json={
    "eles":["aaa","bbb","ccc","ddd"]
};
```
表示对象json的eles属性的值为一个四个元素的数组,可以通过json.eles[0]、json.eles[1]...来获取这些值；

然而:
```
var arr = new Array();
```
在js中其实可以等价于

```
var arr = [];
```
表示一个数组.