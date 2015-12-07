title: JS中new Date()和ios的兼容问题
date: 2015-12-01 14:13:03
tags: js
---
移动端,做关于时间的一个需求,很自然的想到<code>new date()</code>这个方法.
代码如下:
```
var time = new Date('2015-11-11 11:11:11');
console.log(time);
```
<!-- more  -->
这段代码在安卓,pc上都没有问题,但是在IOS下会有问题,显示<code>Invalid Date</code>,明显存在兼容性问题.
后来google得知在ios下面Date解析的格式应该是"**yyyy/MM/dd HH:mm:ss**",也就是代码应该这样子:
```
var time = new Date('2015/11/11 11:11:11');
console.log(time);
```
经测试,完美支持其他浏览器(chrome,firefox,android).据说IE下也存在这样的问题也必须是"**yyyy/MM/dd HH:mm:ss**"(未测试).

另外,如果拿到的数据数"2015-11-11 11:11:11"的话,js <code>replace()</code>方法也能帮你搞定.
```
var str = '2015-11-11 11:11:11';
str = str.replace(/-/g, '/');
```
参考连接:[http://www.w3school.com.cn/jsref/jsref_replace.asp](http://www.w3school.com.cn/jsref/jsref_replace.asp)
