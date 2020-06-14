Rxjava
###观察者模式
* Subject: 被观察者
* ConcreteSubject: 具体被观察者
* Observer: 观察者
* ConcreteObserver: 具体观察者

<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava01.jpg" width = 700>
###Rxjava的观察者模式

#####一、简单demo

		 // 1. 创建一个被观察者
        Observable observable = Observable.create(new ObservableOnSubscribe<String>() {
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
        
		//3. 订阅
        observable.subscribe(observer);


#####二、源码分析
* 抽象被观察者：Observable
* 具体被观察者：ObservableCreate
* 抽象观察者：Observer
* 具体观察者：实现Observer的匿名内部类

* 一、创建一个被观察者Observable.create，由于Observable是一个抽象类，所以就new一个它的子类（ObservableCreate）返回
<img src="https://github.com/langsun/Learn/blob/master/Rxjava/image/rxjava02.jpg" width = 700>

* 二、创建观察者
* 三、订阅observable.subscribe(observer);
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

#####三、Hot Observable和Cold Observable

* Hot：发生了其他事，就不接收事之前的消息

		例如：被观察者发送10个整数1~10，每秒发送一个数字
		
		观察者A，一开始就订阅：所收到的数据为      1，2，3，4，5，6，7，8，9，10
		
		观察者B，休眠3秒后开始订阅：所收到的数据为          4，5，6，7，8，9，10

* Cold：发生了其他事，也阻止不了对消息的接收

		例如：被观察者发送10个整数1~10，每秒发送一个数字
		
		观察者A，一开始就订阅：所收到的数据为      1，2，3，4，5，6，7，8，9，10
		
		观察者B，休眠3秒后开始订阅：所收到的数据为          1，2，3，4，5，6，7