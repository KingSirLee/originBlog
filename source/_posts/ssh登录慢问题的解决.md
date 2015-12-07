title: ssh登录慢问题的解决
date: 2015-12-02 14:10:14
tags: linux
---
在pull/push代码的时候往往会卡顿一下,有时候会大概停留十秒左右,甚至更久,这样子很影响效率.google得知ssh登录的问题,解决方案如下:
<!-- more -->
**关闭ssh的gssapi认证**
修改本机ssh客户端配置ssh_config(注意不是sshd_conf)
vi /etc/ssh/ssh_config，设置GSSAPIAuthentication no 并重启sshd
如图所示:
<img src="http://77g54t.com1.z0.glb.clouddn.com/201512021438.jpg" alt="示例图片">
配置过后明显速度快了很多.

另外了解到:
用ssh -v user@server 可以看到登录时有如下信息：
debug1: Next authentication method: gssapi-with-mic
debug1: Unspecified GSS failure. Minor code may provide more information
注：ssh -vvv user@server 可以看到更细的debug信息

相关连接:
[http://www.linuxidc.com/Linux/2012-12/77144.htm](http://www.linuxidc.com/Linux/2012-12/77144.htm)