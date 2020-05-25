##Retrofit流程

#####一、Retrofit.Builder---->Retrofit对象

		//通过建造者模式创建Retrofit对象
		Retrofit retrofit = new Retrofit.Builder()//建造者模式 
				.baseUrl("https://www.wanandroid.com/")                .addConverterFactory(GsonConverterFactory.create())                .build();
		
#####二、retrofit.create(WanAndroidApi.class)----> Interface实例

		//通过动态代理创建出接口实例：Proxy.newProxyInstance
		WanAndroidApi wanAndroidApi = retrofit.create(WanAndroidApi.class);
		
<img src = "https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit01.jpg" width = "700">

<img src = "https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit02.jpg" width = "700">		


**下面看一下对这四个对象的创建**

<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit03.jpg" width = 700>
<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit04.jpg" width = 700>


**再回过头来看**

<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit05.jpg" width = 700>
<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit06.jpg" width = 700>

#####三、OkHttpcall进行真正的网络请求和处理返回结果

<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit07.jpg" width = 700>
<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit08.jpg" width = 700>
	
		
##Retrofit中的设计模式

**1、建造者模式**

	Retrofit对象的创建，将复杂对象的创建和表示分离,调用 者不需要知道复杂的创建过程,使用Build的相关方法进行配置创建对象。

**2、外观模式**	

	Retrofit对外提供了统一的调度,屏蔽了内部的实现,使得使用该网络库简单便捷。	门面模式: 提供一个统一的接口去访问多个子系统的多个不同的接口,它为子系统中的一组接口提供一 个统一的高层接口。使用子系统更容易使用

**3、动态代理模式**

	通过动态代理的方式,当调用Retrofit的create()方法时,会进行动态代理监听。当执行具体的接口方法时,会回调InvocationHandler。通过反射解析method的标注及参数,生成ServiceMethod对象。
	
**4、静态代理模式**

	Android平台默认的适配器ExecutorCallbackCall,采用静态代理的模式。具体的实现delegate为 OkHttpCall。
	
**5、工厂模式**

	Converter及CallAdapter的创建都采用了工厂模式进行创建。
	**6、适配器模式**
	CallAdapter的adapt采用了适配器模式,使得interface的返回对象可以动态扩展,增强了灵活性	
#####四、实战
**1、网络请求在子线程中进行，请求结果返回到主线程？**


		//在Platform类中有一个这样的内部类，MainThreadExecutor拿到主线程的Looper，然后
		//通过Handler将信息发送到主线程

<img src="https://github.com/langsun/Learn/blob/master/Retrofit/image/retrofit09.jpg" width = 700>
    
**2、比如我们在接收返回值时我们这个去接收{ errorCode:200;  ErrorMeg:"";data:{} }，但是某天服务端返回data字段为null或数组的时候，那么解析就报错了，我们该怎么办？**

	自定义Converter.Factory
	自定义Converter<ResponseBody, T>
	通过自定义之后，对返回值类型进行处理
	
**3、Retrofit 如何将定义的interface转换成网络请求? **
		略

**4、 Retrofit的Converter机制是如何实现?**

		Converter就是将返回的response转化为对应的实体类
	**5、 Retrofit的CallAdapter机制是如何实现?**
**6、在第一张图中，methodHandlerCache.put(method, handler);这里为什么要使用缓存？为什么用到反射的时候就要使用缓存？**

	用到反射的时候就需要大量的IO操作，IO操作就耗时，所以使用缓存提高效率	