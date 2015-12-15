title: angularJS全选
date: 2015-12-15 10:20:38
tags: angularJS
---
做了一个功能,购物车内lineItem全选和反选功能,记录一下主要代码块.
首先声明一个对象,储存动态生成的键值对,默认是全选的.
```
$scope.selectLineItemList = {};
$scope.select_all = true;
// 生出数据
for(var i= 0; i<$scope.sortLineItems.length; i++){
    var key = $scope.sortLineItems[i].lineItemId;
    $scope.selectLineItemList[key] = true;
}
```
<!-- more -->

页面上的代码:
```
<div class="sortLineItemList" ng-repeat="lineItem in sortLineItems">
    <div class="cart-content" ng-click="selectLineItem(lineItem.lineItemId)">
        <div class="detail box">
            <i class="icon-on" ng-show="selectLineItemList[lineItem.lineItemId]"></i>
            <i class="icon-off ng-hide" ng-show="!selectLineItemList[lineItem.lineItemId]"></i>
        </div>
    </div>
</div>

<div ng-click="selectAll(select_all)">
    <i class="icon-on" ng-show="select_all"></i>
    <i class="icon-off ng-hide" ng-show="!select_all"></i>
</div>
```

JS:
```
// 全选
$scope.selectAll = function(selected){
    $scope.select_all = !selected;
    angular.forEach($scope.selectLineItemList, function(value,key){
        $scope.selectLineItemList[key] = !selected;
    });
}
// 选择/取消limeItem
$scope.selectLineItem = function(lineItemId){
    var value = $scope.selectLineItemList[lineItemId];
    $scope.selectLineItemList[lineItemId] = !value;
    // 判断是否全选
    var flag = false;
    angular.forEach($scope.selectLineItemList, function(value,key){
        if(!$scope.selectLineItemList[key]){
            flag = true;
        }
    });
    if (flag){
        $scope.select_all = false;
    } else {
        $scope.select_all = true;
    }
}
```


