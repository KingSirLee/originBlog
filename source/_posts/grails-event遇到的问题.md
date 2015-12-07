title: grails event遇到的问题
date: 2015-11-24 19:40:46
tags: grails
---
今天在做一个需求，就是订单创建成功后新建快照。做的时候遇到一个问题，时不时报错,错误如下所示：
<pre><code>org.hibernate.exception.GenericJDBCException: could not load an entity
</code></pre>
看字面意思，问题很简单，就是没有加载到相应的对象。
<!--more-->
### 出错原因
因为在当前event中，参数是以对象进行传输的，该对象的属性中存在懒加载关系，在之前的线程中，该对象的属性都正常的被加载。新打开的event相当于又开了一个线程，当要用到该属性值的时候，如果没有加载到就会出现问题。
### 解决方案
1.在domain中该属性lazy改为false，不懒加载。
2.参数传输不以对象方式，以Id进行传输,在要用到哪些值的时候再去查询。

<p>好了这个问题解决了，然后后来发现偶尔会出现另外一个错误，nullpoint，但是看代码bing没有什么逻辑错误。后来想了下可能是这样子的，在主线程中订单保存成功，实际上数据库没有执行完毕，然而这时候在event中就开始去用这个trade，这个时候就有问题。后来采用了一个不是好办法的办法，在event中sleep一秒，再执行新建快照的逻辑，在运行代码就没有什么问题了。好吧，这也算解决问题了。。</p>
<br><br>
相关文献
[http://stackoverflow.com/questions/10040430/genericjdbcexception-could-not-load-an-entity]()