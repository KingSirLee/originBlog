title: JS处理时差的问题
date: 2015-11-30 13:49:50
tags: js
---
&#160; &#160; &#160; &#160;今天发现一个代码上的坑,在订单去支付的时候会有半个小时倒计时的交互,但是一个客户反映:一下单就到截止时间了.后来发现,我们计算倒计时的时候取的是客户端的时间(new Date()),所以存在一个时差的问题.
<!-- more -->
有问题的代码:
```
var endTime = $("input[name=countTime]").val();
var end_time = new Date(endTime).getTime(),
                sys_second = (end_time-new Date().getTime())/1000;
var timer = setInterval(function(){
            if (sys_second > 1) {
                sys_second -= 1;
                var hour = Math.floor(sys_second / 3600);
                var minute = Math.floor((sys_second / 60) % 60);
                var second = Math.floor(sys_second % 60);
                hour = hour<10?"0"+hour:hour; //计算小时
                minute = minute<10?"0"+minute:minute; //计算分钟
                second = second<10?"0"+second:second; //计算秒杀
                var text = hour + ":" + minute + ":" + second;
                //....
            } else {
                clearInterval(timer);
            }
        }, 1000);
```
<code>new Date()</code>取的是当前客户端的时间,很明显,如果不在同一个时差会有问题的.<br>
 如果在不改变接口的情况下,仅仅通过js来判断的话加上一句代码就OK.
```
var chinaTime =  new Date(+new Date()+8*3600*1000).toISOString().replace(/T/g,' ').replace(/\.[\d]{3}Z/,'');
sys_second = (end_time-new Date(chinaTime).getTime())/1000;
```
这样子计算时间的标准也就一样了.
ps:**当然这不是最佳实践方式,起止时间都应该从服务端获取,这样子才可以!**

另外,在JS里面可以写一个方法,代替<code>new Date()</code>来获取当前时间.
```
function getNow(){
    var chinaTime =  new Date(+new Date()+8*3600*1000).toISOString().replace(/T/g,' ').replace(/\.[\d]{3}Z/,'');
    return new Date(chinaTime)
}
```