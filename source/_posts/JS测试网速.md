title: JS测试网速
date: 2015-12-21 12:44:29
tags: js
---
js测试当前网络状况,为了不响应正常逻辑,在页面底部加入一段JS代码.
当然不是每次访问都要去测试,所以有个标识来记录,这里我打算把这个标识放在localStorage里面.
<!-- more -->
```
构建一个对象,包含有path,time,ip,area等这些属性.
页面JS代码
<script>
   if(window.localStorage){
        var speed = window.localStorage.getItem('speed');
        if (!speed || speed.indexOf('detail') < 0){
            window.localStorage.setItem('speed', speed+'detail')
    
            var ip = $("input[name=ip]").val();
            var st = new Date();
            var img = new Image();
            img.src = 'http://img.shijieyou.cn/upload/logo.jpg';
    
            img.onload = function(){
                var et = new Date();
                var time = et - st;
                $.ajax({
                    url: '/xxx/speedTest',
                    data: {'ip':ip,'path':'detail','time':time},
                    success: function(){
                    }
                });
            }
        }
    }
</script>
```
关于根据IP查找对应的地区,这里采用的是17mon提供的ip归属地查询.