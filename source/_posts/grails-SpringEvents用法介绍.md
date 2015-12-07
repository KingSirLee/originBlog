title: grails SpringEvents用法介绍
date: 2015-10-13 21:00:32
tags: grails
---
又一次用到grails SpringEvents插件，以前都是用到了就去看看以前是怎么写的，然后copy一下，今天决定总结一下。
#### 1.具体的代码
<!-- more -->
<pre><code>class UpdateTradeCountEvent extends ApplicationEvent {
    Trade trade
    UpdateTradeCountEvent(trade) {
        super(trade)
        this.trade = trade // 注意这一行代码不能少
    }
}
</code></pre>
<pre><code>class UpdateTradeCountListenerService implements ApplicationListener {
    @Override
    void onApplicationEvent(UpdateTradeCountEvent event) {
        // 这里面写相应的逻辑
        xxx
    }
}
</code></pre>
#### 2.调用方法
<pre><code>def event = new UpdateTradeCountEvent(trade)
publishEvent(event)
</code></pre>
这里是异步手动去调用，当然也可以写到一个domain中，这样子当该domain更新时候也会通知依赖者更新
#### 3.约定大于配置
一个xxxEvent必然有一个对应的xxxUpdateTradeCount,例如：UpdateTradeCountEvent和UpdateTradeCountUpdateTradeCount

### 相关技术文档
[http://grails.org/plugin/spring-events](http://grails.org/plugin/spring-events)
***
#### Observer模式
<p>简单了解一下Oberver设计模式，也就是我们常说的观察者模式。简单说来，就是有依赖关系的对象，当有一个对象发生改变时，其他依赖它的对象被通知并自动更新。在该设计模式中两个角色：观察者、被观察者。</p>
应用场景<br>
* 1、对一个对象状态的更新，需要其他对象同步更新，而且其他对象的数量动态可变
* 2、对象仅需要将自己的更新通知给其他对象而不需要知道其他对象的细节
----
相关资料
[http://flyaway2.iteye.com/blog/1935587](http://flyaway2.iteye.com/blog/1935587)

