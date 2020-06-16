Rxjava
###观察者模式
* Subject: 被观察者
* ConcreteSubject: 具体被观察者
* Observer: 观察者
* ConcreteObserver: 具体观察者

<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava01.jpg" width = 700>
###Rxjava的观察者模式

#####一、简单demo

		 //这里用非链式结构写，要用一个Observable来接收每次返回的值
		 Observable observable;
		 
		 // 1. 创建一个被观察者
        observable = Observable.create(new ObservableOnSubscribe<String>() {
            @Override
            public void subscribe(ObservableEmitter<String> emitter) throws Exception {
                emitter.onNext("test");
                emitter.onComplete();
            }
        });
        
        //2. 创建一个观察者
        Observer observer = new Observer<String>() {
            @Override
            public void onSubscribe(Disposable d) {
                Log.i("sun", "onSubscribe: " +d);
            }

            @Override
            public void onNext(String str) {
                Log.i("sun", "onNext: " + str);
            }

            @Override
            public void onError(Throwable e) {
                Log.i("sun", "onError: " + e.getMessage());
            }

            @Override
            public void onComplete() {
                Log.i("sun", "onComplete: ");
            }
        };
        
        //3. map转换
        observable = observable.map(new Function<String,String>(){
            @Override
            public String apply(String s) throws Exception {
                return s + "map";
            }
        });
        
        //4. 线程切换
        observable = observable.subscribeOn(Schedulers.io());
        observable = observable.observeOn(AndroidSchedulers.mainThread());
        
		//5. 订阅
        observable.subscribe(observer);


#####二、源码分析
* 抽象被观察者：Observable
* 具体被观察者：ObservableCreate
* 抽象观察者：Observer
* 具体观察者：实现Observer的匿名内部类

* 一、创建一个被观察者Observable.create，由于Observable是一个抽象类，所以就new一个它的子类（ObservableCreate）返回

	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava02.jpg" width = 700>

* 二、创建观察者
* 三、转换符：以map为例，发送的是test，接收到的是testmap

	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava12.jpg" width = 700>
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava13.jpg" width = 700>
	
* 四、线程切换  observable.subscribeOn(Schedulers.io());

	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava09.jpg" width = 700>
	
  observable.observeOn(AndroidSchedulers.mainThread());
  
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava10.jpg" width = 700>
	
  线程切换表现为洋葱结构模型。其实也就是装饰者模式
  
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava11.jpg" width = 700>
	
   线程切换源码分析实现原理<br/>
   以observable.observeOn(AndroidSchedulers.mainThread());中的
   AndroidSchedulers.mainThread()来分析，AndroidSchedulers.mainThread()最终创建一个HandlerScheduler对象
   
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava14.jpg" width = 700>

   在HandlerScheduler中通过createWorker方法创建一个HandlerWorker对象
	
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava15.jpg" width = 700>
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava16.jpg" width = 700>
   
   那在什么地方调用HandlerWorker中的schedule（）方法呢？
   在我们调用observable.observeOn()时，
   
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava17.jpg" width = 700>
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava18.jpg" width = 700>
   
   [RxJava中subscribeOn到底是不是只有第一次才有效？](https://www.jianshu.com/p/4e78d447394e)
   <img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava19.jpg" width = 700>
  
* 五、订阅observable.subscribe(observer);

	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava03.jpg" width = 700>
	
	在Observable中subscribeActual是一个抽象方法，要看subscribeActual在Observable的子类ObservableCreate中的实现
	
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava04.jpg" width = 700>
	
	ObservableCreate中的subscribeActual实现
	
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava05.jpg" width = 700>
	
	在发射器CreateEmitter中实现消息的发送
	
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava06.jpg" width = 700>
	<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava07.jpg" width = 700>

#####三、五种观察者模式

<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava08.jpg" width = 700>

#####四、Hot Observable和Cold Observable

* Hot：发生了其他事，就不接收事之前的消息；无论有没有观察者订阅，数据都一直发送着，多个观察者订阅的时候，信息是共享的

		例如：被观察者发送10个整数1~10，每秒发送一个数字
		
		观察者A，一开始就订阅：所收到的数据为      1，2，3，4，5，6，7，8，9，10
		
		观察者B，休眠3秒后开始订阅：所收到的数据为          4，5，6，7，8，9，10

* Cold：发生了其他事，也阻止不了对消息的接收；观察者订阅了才开始发送数据，Observable和Observer是一对一的关系

		例如：被观察者发送10个整数1~10，每秒发送一个数字
		
		观察者A，一开始就订阅：所收到的数据为      1，2，3，4，5，6，7，8，9，10
		
		观察者B，休眠3秒后开始订阅：所收到的数据为          1，2，3，4，5，6，7
		
#####五、背压
**背压是指在异步场景中，被观察者发送事件速度远快于观察者的处理速度的情况下，一种告诉上游的被观察者降低发送速度的策略**

简而言之，**背压是流速控制的一种策略**。

需要强调两点：

- 背压策略的一个前提是**异步环境**，也就是说，被观察者和观察者处在不同的线程环境中。
- 背压（Backpressure）并不是一个像flatMap一样可以在程序中直接使用的操作符，他只是一种控制事件流速的策略。

#####响应式拉取（reactive pull）

* 在RxJava的观察者模型中，**被观察者是主动的推送数据给观察者，观察者是被动接收的**。
* 而响应式拉取则反过来，**观察者主动从被观察者那里去拉取数据，而被观察者变成被动的等待通知再发送数据**。


观察者可以根据自身实际情况按需拉取数据，而不是被动接收（也就相当于告诉上游观察者把速度慢下来），最终实现了上游被观察者发送事件的速度的控制，实现了背压的策略。

五种背压策略

		MISSING:没有任何缓存和丢弃，如果流的速度无法保持同步，可能会抛出异常
		ERROR:缓冲区大概在128，下游的处理速度无法跟上上游的发射速度时报错
		BUFFER:缓冲区大概1000左右，上游不断发出onNext请求，直到下游处理完
		DROP:下游的处理速度无法跟上上游的发射速度时把onNext的值丢弃
		LATEST:会一直保留最新的onNext的值，直到被下游消费掉
		
使用Flowable
		
		Flowable<Integer> upstream = Flowable.create(new FlowableOnSubscribe<Integer>() {
            @Override
            public void subscribe(FlowableEmitter<Integer> emitter) throws Exception {
                Log.d(TAG, "emit 1");
                emitter.onNext(1);
                Log.d(TAG, "emit 2");
                emitter.onNext(2);
                Log.d(TAG, "emit 3");
                emitter.onNext(3);
                Log.d(TAG, "emit complete");
                emitter.onComplete();
            }
        }, BackpressureStrategy.ERROR); //增加了一个参数

        Subscriber<Integer> downstream = new Subscriber<Integer>() {

            @Override
            public void onSubscribe(Subscription s) {
                Log.d(TAG, "onSubscribe");
                s.request(Long.MAX_VALUE);  //注意这句代码
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "onNext: " + integer);

            }

            @Override
            public void onError(Throwable t) {
                 Log.w(TAG, "onError: ", t);
            }

            @Override
            public void onComplete() {
                Log.d(TAG, "onComplete");
            }
        };

        upstream.subscribe(downstream);
        
我们注意到这次和`Observable`有些不同. 首先是创建`Flowable`的时候增加了一个参数, 这个参数是用来选择背压,也就是出现上下游流速不均衡的时候应该怎么处理的办法, 这里我们直接用`BackpressureStrategy.ERROR`这种方式, 这种方式会在出现上下游流速不均衡的时候直接抛出一个异常,这个异常就是著名的`MissingBackpressureException`. 其余的策略后面再来讲解.

另外的一个区别是在下游的`onSubscribe`方法中传给我们的不再是`Disposable`了, 而是`Subscription`, 它俩有什么区别呢, 首先它们都是上下游中间的一个开关, 之前我们说调用`Disposable.dispose()`方法可以切断水管, 同样的调用`Subscription.cancel()`也可以切断水管, 不同的地方在于`Subscription`增加了一个`void request(long n)`方法, 这个方法有什么用呢, 在上面的代码中也有这么一句代码:

```
 s.request(Long.MAX_VALUE);
```

这是因为`Flowable`在设计的时候采用了一种新的思路也就是`响应式拉取`的方式来更好的解决上下游流速不均衡的问题, 与我们之前所讲的`控制数量`和`控制速度`不太一样, 这种方式用通俗易懂的话来说就好比是`叶问打鬼子`, 我们把`上游`看成`小日本`, 把`下游`当作`叶问`, 当调用`Subscription.request(1)`时, `叶问`就说`我要打一个!` 然后`小日本`就拿出`一个鬼子`给叶问, 让他打, 等叶问打死这个鬼子之后, 再次调用`request(10)`, 叶问就又说`我要打十个!` 然后小日本又派出`十个鬼子`给叶问, 然后就在边上看热闹, 看叶问能不能打死十个鬼子, 等叶问打死十个鬼子后再继续要鬼子接着打...

所以我们把request当做是一种能力, 当成`下游处理事件`的能力, 下游能处理几个就告诉上游我要几个, 这样只要上游根据下游的处理能力来决定发送多少事件, 就不会造成一窝蜂的发出一堆事件来, 从而导致OOM. 这也就完美的解决之前我们所学到的两种方式的缺陷, 过滤事件会导致事件丢失, 减速又可能导致性能损失. 而这种方式既解决了事件丢失的问题, 又解决了速度的问题, 完美 !

#####六、Rxjava实例开发应用

* 网络请求处理(轮询，嵌套，出错重连)
* 功能防抖
* 从多级缓存获取数据
* 合并数据源
* 联合判断
* 与 Retrofit,RxBinding,EventBus结合使用

#####七、Rxjava原理

* Scheduler线程切换工作原理
* 数据的发送与接收(观察者模式)
* lift的工作原理
* map的工作原理
* flatMap的工作原理
* merge的工作原理
* concat的工作原理

