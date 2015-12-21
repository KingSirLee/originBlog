title: angularJS中ng-class的用法
date: 2015-12-21 19:04:59
tags: angularJS
---
在开发中我们通常会遇到一种需求：一个元素在不同的状态需要展现不同的样子。有一种做法是都写在页面上,通过ng-show来进行显示,另外一种就是通过ng-class来控制,通过改变class的值实现动态改变.
<!-- more -->

这里有三种实现方式:

**第一种:通过数据的双向绑定（不推荐）**
实现方式:
```
function changeClass(){
  $scope.className = "change2";
}
 
<div class="{{className}}"></div>
```
这种方式完全没错，是angular提供的一种改变class的方式，但是在controller涉及了classname在我看来是乎总是那么诡异，我希望的是controller是一个干净的纯javascript意义的object。

**第二种:通过字符串数组的形式来改变**
实现方式:
```
function changeClass(){
  $scope.isActive = true;
}
  
<div ng-class="{true:'isActive',false:'noActive'}[isActive]"></div>
```
当isActive=true的时候,class为isActive;反之为noActive.虽然说只能让一个元素拥有两个状态,但是一般需求来说足够了,我一般会用到这个.

**第三种:通过key/value的方式**
实现方式:
```
function changeClass(){
  $scope.choiceClass = true;
}
  
<div ng-class="{'selectClass':select,'choiceClass':choice,'makeCalss':make}"></div>
```
当choiceClass为true的时候，class则为choice，以此类推.这样的写法choiceClass可以弥补第二种方式的点点遗憾~