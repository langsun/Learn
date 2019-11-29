###一、为什么要学习Handler

#####阿里面试题：点击页面上的按钮更新Textview的内容，要注意些什么？

问题          |原因 
------------ | ------------- 
Only the original thread that created a view </br> hierarchy can touth its view | 不能在子线程更新UI
OOM|Handler使用不当可能引起内存泄漏 
Handler obtainMessage()|Message的优化
Can‘t create Handlerinside thread that has not <br> called Looper.prepare()|在子线程中创建Handler，需要为这个Handler</br>准备Looper
java.lang.NullPointerException|在Handler把消息处理完之后，但是页面销毁了，</br>这个时候可能Handler会更新UI，但是TextView、</br>ImageView之类的资源引用不见了，就会报空指</br>针异常

###二、Handler能做什么
* **处理延时任务：**推送未来某个时间点将要执行的Message或者Runnable到消息队列；
* **线程间通信：**在子线程把需要在另一个线程执行的操作添加到消息队列中去；

###三、Handler模型

<img src = "https://github.com/langsun/Learn/blob/master/zimage/handler/handler1.jpg" width = "600" height = "350"> 

###四、源码分析
#####发送消息

**Handler主要函数**

* Handler.XXXMessage最终都会调用Handler.enqueueMessage，最后都会调用MessageQueue.enqueueMessage

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler2.jpg" width = "600" height = "350">

* 在Handler中enqueueMessage中，msg.tagate = this;就是把当前Handler打包到msg中，确保了handler的唯一性

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler9.jpg" width = "600" height = "350">


**MessageQueue主要函数**

* MessageQueue消息的发送与接收，利用内存实现线程间通信

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler8.jpg" width = "600" height = "300">

* MessageQueue.enqueueMessage

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler4.jpg" width = "600" height = "300">

* 示意图如下

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler3.jpg" width = "600" height = "230">

#####接收消息

* ActivityThread ---> Looper.prepareMainLooper();----->  Looper.loop(); --->MessageQueue.next()


* 在ActivityThread中Looper.prepareMainLooper();对Looper进行初始化

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler6.jpg" width = "600" height = "350">


<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler10.jpg" width = "600" height = "350">

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler11.jpg" width = "600" height = "150">

* 介绍一下ThreadLocal，线程隔离工具，里面以map存储模式<key,value>   key 为当前线程，Value 为泛型T，在当前Looper中  Value就是Looper

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler7.jpg" width = "600" height = "600">

* Looper准备完毕后，启动Looper.loop()方法，在loop()中调用MessageQueue.next()

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler16.jpg" width = "600" height = "350">

* MessageQueue.next()

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler5.jpg" width = "600" height = "350">

* handler.handleMessage()

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler17.jpg" width = "600" height = "300">


#####消息的阻塞

<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler12.jpg" width = "600" height = "350">
#####消息的唤醒
<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler13.jpg" width = "600" height = "350">

###Q&A
<img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler14.jpg" width = "600" height = "250">

  
    Q: ThreadLocal底层实现是ThreadLocalMap,也是一个Map，但为什么不直接使用HashMap呢，非要设计一个ThreadLocal呢？
    A: 1.HashMap太大太臃肿了，ThreadLocal唯一的可以就是线程，不需要考虑int，String等等类型
       2.ThreadLocal是线程隔离，线程在应用程序中是唯一的，那么就用一个唯一的ThreadLocal来管理更方便
       3.ThreadLocal是利用了HashMap，又拓展了HashMap，


    Q: 享元模式有用到吗？
    A: Message的设计就使用了享元模式


    Q: 子线程真的不能更新UI吗？
    A: 能，surfaceView可以在子线程中更新UI，王者荣耀就是在surfaceView可以在子线程中更新UI
    
 下面的代码也能在子线程中更新UI，因为**在onCreate()方法执行的时候，onResume()方法还没有执行，系统不会检测在什么线程中更新UI，详情请看ActivityManagerService源码**
 <img src ="https://github.com/langsun/Learn/blob/master/zimage/handler/handler15.jpg" width = "600" height = "350">
 
 
###五、回顾
 
 [Handler用法及解析](https://blog.csdn.net/qq_37321098/article/details/81535449)
