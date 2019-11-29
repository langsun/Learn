###一、简介

EventBus是一个Android端优化的publish/subscribe消息总线，简化了应用程序内各组件间，组件与后台线程间的通信。比如请求网络，等待网络返回时通过Handler或者Broadcast通知主线程更新UI，两个Fragment之间需要通过Listener通信，这些需求都可以通过EventBus实现。

###二、下载地址

[EventBus下载地址](https://github.com/greenrobot/EventBus)

###三、使用步骤

**1.注册**

	EventBus.getDefault().register(this);
		
**2.发送**	

	EventBus.getDefault().post(new PersonEvent("张三");
	
**3.接收**	

	@Subscribe(threadMode = ThreadMode.MAIN)
    public void getEvent(PersonEvent event) {
        if (event != null) {
          String name =  event.getName();        
        } 
    }
    
**4.解除注册**

	EventBus.getDefault().unregister(this);	
	
**接收消息的四种线程**

名称                    | 注释
---------------------- | ------------- 
ThreadMode.MAIN        | 表示该方法在主线程中执行  
ThreadMode.BACKGROUND  | 表示该方法在后台执行，不能并发处理
ThreadMode.ASYNC       | 表示该方法在后台执行，可以异步并发处理
ThreadMode.POSTING     | 表示该方法和消息发送方在同一个线程中执行

###四、粘性事件

**之前说的使用方法都是需要先注册事件，然后在post发送事件，最后才能接收事件。如果你使用粘性事件postSticky发送事件，那么可以先不注册，也能接收到事件**

	EventBus.getDefault().postSticky(new PersonEvent("张三");

###五、源码分析

**注册**

1. 通过反射或注解获取所有的订阅方法
2. **将当前订阅者添加到EventBus总的事件订阅集合subscriptionsByEventType中（用于发送消息）**      subscriptionsByEventType（EventType，Subscriptions）；<br>
Subscription  subscription = new Subscription（subscriber，subscriberMethod）<br>
EventType就是注册的Event类型，如XXXEvent<br>
subscriber就是传入的当前类，如XXXActivity<br>
subscriberMethod就是传入当前类XXXActivity中用于接收Event消息的方法<br>
当发送消息时，拿到subscriptionsByEventType，中的XXXEvent，在拿到XXXEvent对应的Subscriptions，就知道该给哪个类发送消息
3. **将当前订阅者所有订阅的事件类型添加到typesBySubscriber中（用于解注册）**<br>
  typesBySubscriber(subscriber, EventTypes)<br>
  subscriber就是传入的当前类，如XXXActivity<br>
  EventTypes就是XXXActivity中所有要接收的Event类型<br>
  当解注册的时候，拿到XXXActivity，然后遍历这个类中所有的Event，一一解除注册。

**发送**

1. 得到要发送事件类型
2. 根据事件类型获取所有订阅者，并循环向每个订阅者发送消息

**解注册**

1. 通过typesBySubscriber获取当前订阅者所有的事件类型
2. 循环遍历每一个事件类型，并删除当前订阅者的订阅方法

###六、回顾

[EventBus源码解析](http://www.cnblogs.com/all88/archive/2016/03/30/5338412.html)

[EventBus源码解析](http://www.2cto.com/kf/201607/524998.html)