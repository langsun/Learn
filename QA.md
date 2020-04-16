####1、android和java序列化的原理

**序列化 ：(Serialization)将对象转按一定的规则换为可以存储或传输的形式的过程。这是一个过程。**

	例如，将埃菲尔铁塔搬到中国，那么整体搬运是不现实的，那么我们把构成埃菲尔铁塔的每一根钢材我们都给他标上序号（序列化过程），然后把塔拆了运输，到中国后再把钢材按序号
	进行拼装（反序列化过程）。
		
**Serializable**
		
- java提供的，它是jdk中的一个空的接口，不需要你去实现它里面的方法，
- 那么要他有什么用呢？在java中用这样的空接口来做标记，如果一个类实现了这个接口，那么就对这个类打上了标记，打上标记后你就可以使用流把这个类的所有信息进行完整的打包，然后	就可以进行传输，当我们拿到这个打包的信息后，我们使用反序列化，序列化的一个逆过程，将这个信息恢复成一个对象。
- 在这个过程中，它的底层是使用大量IO，反射实现，获取类的名字，属性等，所以性能不太好

**Parcelable**		

- android提供的接口，这个接口就不是空的，需要你实现其中的一些方法writeToToParcel()、构建Creator对象，使用反序列化得到的 Parcel 构造对象等待
- 在序列化writeToToParcel()过程中，你只是把成员的值保存起来了，并没有把这个类保存起来，也就是只对成员序列化了，所以你没办法通过Parcelable去恢复一个完整的对象，所
  以要持久化的话，就不要样Parcelable
- 那它是怎么序列化的呢，他只是在android内存中序列化，你可以在android内存间去使用他，他产生的信息比较少，不需要借助IO，他们在一个内存中，效率比较高。
- 在序列化writeToToParcel()和使用反序列化得到的 Parcel 构造对象中写和读属性的顺序要一样

广义的来说，gson，json fastjson，都是序列化的一种，他们有自己的规则进行数据的续写

####2、什么是匿名内部类,它有什么特征

**匿名内部类**

- 匿名内部类也就是没有名字的内部类
- 正因为没有名字,所以匿名内部类只能使用一次,它通常用来简化代码编写
- 匿名内部类中是不能定义构造函数的（那怎么来初始化匿名内部类呢?使用构造代码块!利用构造代码块能够达到为匿名内部类创建一个构造器的效果）
- 定义了一个匿名内部类,并且希望它使用一个其外部定义的参数,那么编译器会要求该参数引用是final的。
（匿名内部类编译的时候也会编译成一个class文件，他和外部的class不同，所以访问外部类的变量时，变量要是final的，也就是当内部类改变外部类的变量时，外部类的变量如果不改变就显得很奇怪）

**内部类**

- 内部类是为了更好的封装,把内部类封装在外部类里,不允许同包其他类访问

####3、String中"=="与equals()的区别

- “==“比较的是内存中存放的位置- equals()比较的是字符序列
Java中默认的 equals方法实现如下：
	public boolean equals(Object obj) {
    	return (this == obj);
	}
	
String类则覆写了这个方法,直观的讲就是比较字符是不是都相同。



	public boolean equals(Object anObject) {
    	if (this == anObject) {
        	return true;
    	}
    	if (anObject instanceof String) {
        	String anotherString = (String)anObject;
        	int n = count;
        	if (n == anotherString.count) {
            	char v1[] = value;
            	char v2[] = anotherString.value;
            	int i = offset;
            	int j = anotherString.offset;
            	while (n-- != 0) {
                	if (v1[i++] != v2[j++])
                    	return false;
            	}
            	return true;
       		}
    	}
    	return false;
	}

####4、为什么说String是不可变的

- 在String类的源码中可以看到，直接将String定义为final
- 在String类的源码中可以看到，string的长度是不可变的，private final int count  
- 在String类的源码中可以看到，string的UID是不可变的，private final long serialVersionUID 
- 在String类的源码中可以看到，string的内容是不可变的，private final OutputStreamfield[] serialpersisentFields 

####5、什么是内存泄漏？java是怎么去处理他？

- 内存泄漏（Memory Leak）是指程序中己动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。
- 产生的原因：一个长生命周期的对象持有一个短生命周期对象的强引用
- 通俗讲就是该回收的对象，因为引用问题没有被回收，最终会产生OOM

java自身是不会处理的，这需要我们自己定位问题，自己处理

####6、Java回收机制？如何减少OOM的概率？

**算法**

- 复制算法
- 标记-清除	
- 标记-整理

**GC如何判断对象是否存活**

- 引用计数法（循环引用问题）
- 可达性分享法（从GC Roots开始）

**减少OOM的概率**

- 尽可能少的发生内存泄漏- 尽可能不在循环中申请内存- 尽可能不在调用次数多的函数中申请内存
####7、ArrayList
基于数组的数据结构
**顺序删除多个元素**
- 不能使用从前往后删除for循环，会报IndexOutOfBoundsException，但是可以从后往前删除
- 使用迭代器，在迭代器中会更新removeIndex

**ArrayList容量大小是如何自动增加的**

- ArrayList可以指定初始容量，也可以不知道，不指定默认是10，
- 当达到容量的时候，再添加元素，会进行扩容，扩容为原来的1.5倍
- 然后把原来的数组利用Arrays.copy（），将原数组copy到新数组

**什么时候使用ArrayList**

- 多查找，少添加、删除元素时，使用ArrayList

####7、LinkedList
基于链表的数据结构


ArrayList ：

	优点：存储空间连续，允许随机访问，尾删、尾插快
	缺点：插入和删除需要移动元素，效率低，长度固定
	
LinkedList：

	优点：随意进行增删改，插入和删除效率高，容量不固定
	缺点：内存不连续，查找困难


####8、HashMap的原理，HashMap和SparseArray的区别

####9、Java中的强引用，软引用，弱引用和虚引用

**强引用(StrongReference)**

	Object strongReference = new Object();
	当内存空间不足时，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足的问题。 如果强引用对象不使用时，需要
	弱化从而使GC能够回收，如 strongReference = null;

**软引用(SoftReference)**

	String str = new String("abc");
    SoftReference<String> softReference = new SoftReference<String>(str);
	如果一个对象只具有软引用，则内存空间充足时，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。
	
	注意：软引用对象是在jvm内存不够的时候才会被回收，我们调用System.gc()方法只是起通知作用，JVM什么时候扫描回收对象是JVM自己的状态决定的。就算扫描到软引用对象也不
	一定会回收它，只有内存不够的时候才会回收。
	
	当内存不足时，JVM首先将软引用中的对象引用置为null，然后通知垃圾回收器进行回收：
	
	if(JVM内存不足) {
        // 将软引用中的对象引用置为null
        str = null;
        // 通知垃圾回收器进行回收
        System.gc();
    }
    
**弱引用(WeakReference)**

	String str = new String("abc");
    WeakReference<String> weakReference = new WeakReference<>(str);
    str = null;
    
	弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足
	够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。


    注意：如果一个对象是偶尔(很少)的使用，并且希望在使用时随时就能获取到，但又不想影响此对象的垃圾收集，那么你应该用Weak Reference来记住此对象。
    
 **虚引用(PhantomReference)**

	虚引用顾名思义，就是形同虚设。与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回
	收器回收。
 
####10、依赖注入（Dagger2）

依赖导致原则：高层模块不应该依赖底层模块,二者都该依赖其抽象;抽象不应该依赖细节;细节应该依赖抽象;

	问题描述：类A直接依赖类B,假如要将类A改为依赖类C,则必须通 过修改类A的代码来达成。这种场景下,类A一般是高层模块,负责复 杂的业务逻辑;类B和类C是低层模块,负责基本的
	原子操作;假如修 改类A,会给程序带来不必要的风险。
	
	解决方案：将类A修改为依赖接口interface,类B和类C各自实现接 口interface,类A通过接口interface间接与类B或者类C发生联系,则 会大大降低修改类A的几率。

**构造函数注入方式**
	
	public class Person2 {	    private Driveable mDriveable;		//构造函数注入		public Person2(Driveable driveable){        	this.mDriveable = driveable;    	}	}
**setter 方式注入**
	public class Person2 {	    private Driveable mDriveable;		//setter 方式注入		public void setDriveable(Driveable mDriveable) {        	this.mDriveable = mDriveable;    	}	}	
	
**接口注入方式**	

	/**	* 接口方式注入	* 接口的存在,表明了一种依赖配置的能力。 	*/	public interface DepedencySetter {    	void set(Driveable driveable);	}
	
	public class Person2  implements DepedencySetter {	//接口方式注入		@Override		public void set(Driveable driveable) {        	this.mDriveable = mDriveable;    	}	}	
####11、synchronized作用

synchronized 关键字可以在多线程环境下用来作为线程安全的同步锁。悲观锁

三种用法：- 修饰代码块- 修饰成员方法- 修饰静态方法
 
 
####12、根据泛型T获取具体的对象的类型

        // 在类的外部这样获取
        Type type = ((ParameterizedType)foo.getClass().getGenericSuperclass()).getActualTypeArguments()[0];
        System.out.println(type);
        // 在类的内部这样获取
        System.out.println(foo.getTClass()); 
 
 
####13、注解的原理

**注解即标签**

	如果把代码想象成一个具有生命的个体,注解就是给这些代码的某些个体打标签
	
**元注解**

	元注解是可以注解到注解上的注解,或者说元注解是一种基本注解,但是它能够应用到其它的注解上面。
	
元标签有 @Retention、@Documented、@Target、@Inherited、 @Repeatable 5 种。

	@Retention   它解释说明了这个注解的的存活时间。
	
		a. RetentionPolicy.SOURCE 注解只在源码阶段保留,在编译 器进行编译时它将被丢弃忽视。		b. RetentionPolicy.CLASS 注解只被保留到编译进行的时候, 它并不会被加载到 JVM 中。		c. RetentionPolicy.RUNTIME 注解可以保留到程序运行的时 候,它会被加载进入到 JVM 中,所以在程序运行时可以获 取到它们		java - source被丢弃 -> class - class被丢弃 > jvm (runtime)
	@Target    指定了注解运用的地方 
		a. ElementType.ANNOTATION_TYPE 可以给一个注解进行注 解		b. ElementType.CONSTRUCTOR 可以给构造方法进行注解		c. ElementType.FIELD 可以给属性进行注解		d. ElementType.LOCAL_VARIABLE 可以给局部变量进行注解		e. ElementType.METHOD 可以给方法进行注解		f. ElementType.PACKAGE 可以给一个包进行注解		g. ElementType.PARAMETER 可以给一个方法内的参数进行注解
	@Documented   这个元注解肯定是和文档有关。它的作用是能够将注解中 的元素包含到 Javadoc 中去。ElementType.TYPE 可以给一个类型进 行注解,比如类、接口、枚举	@Inherited    Inherited 是继承的意思,但是它并不是说注解本身可以继承,而是说 如果一个超类被 @Inherited 注解过的注解进行注解的话,那么如果 它的子类没有被任何注  	解应用的话,那么这个子类就继承了超类的注 解。
	@Repeatable    Repeatable 自然是可重复的意思。@Repeatable 是 Java 1.8 才加进来的,所以算是一个新的特性。	
**注解的属性**

	注解的属性也叫做成员变量。注解只有成员变量,没有方法。 需要注意的是, 在注解中定义属性时它的类型必须是 8 种基本数据类型外加 类、接口、注解及 它们的数组 注解中属性
	可以有默认值,默认值需要用 default 关键值指定

	定义注解：
	
	@Target(ElementType.TYPE)	@Retention(RetentionPolicy.RUNTIME)	public @interface Test{		int id() default -1;		String msg() default "Hello";	}	
	使用注解
	￼@Test(id=1,msg="hello annotation")		public class TestAnnotation {	}
	
**注解的提取**

	注解通过反射获取。
	首先可以通过 Class 对象的 isAnnotationPresent() 方法判断它是否应用了某个注解
	然后通过 getAnnotation() /getAnnotations() 方法来获取 Annotation 对象。
	
**注解的使用场景**

- 提供信息给编译器：编译器可以利用注解来探测错误和警告信息（如 @override）
- 编译阶段时的处理: 软件工具可以用来利用注解信息来生成代码、 Html文档或者做其它相应处理。 （如ButterKnife，ARouter，GreenDao，Dagger2）
- 运行时的处理: 某些注解可以在程序运行的时候接受代码的提取 值得 注意的是,注解不是代码本身的一部分。（如 Glide，Retrofit）


####14、动态代理机制

**代理模式**

	给目标对象提供一个代理对象，由代理对象控制目标对象的引用
	代理对象起到中介作用，连接客户端与目标对象
	
**作用**
	
	保护目标对象
	
**为什么要使用动态代理**

	一个静态代理只能服务一种类型的目标对象，当服务的目标对象多的时候，就会出现，对象多，代码量大的问题
	
	缺点：效率低，因为使用反射
	
**Retrofit中动态代理的使用**

	public <T> T create(final Class<T> service) {
    	Utils.validateServiceInterface(service);
    	if (validateEagerly) {
      		eagerlyValidateMethods(service);
    	}
    	return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
        	new InvocationHandler() {
          		private final Platform platform = Platform.get();

          		@Override 
          		public Object invoke(Object proxy, Method method, @Nullable Object[] args)throws Throwable {
            		// If the method is a method from Object then defer to normal invocation.
            		if (method.getDeclaringClass() == Object.class) {
              			return method.invoke(this, args);
            		}
            		if (platform.isDefaultMethod(method)) {
              			return platform.invokeDefaultMethod(method, service, proxy, args);
            		}
            		ServiceMethod<Object, Object> serviceMethod =(ServiceMethod<Object, Object>) loadServiceMethod(method);
            		OkHttpCall<Object> okHttpCall = new OkHttpCall<>(serviceMethod, args);
            		return serviceMethod.adapt(okHttpCall);
          		}
        	});
	}

####15、Android ClassLoader原理

**双亲委托机制**

	某个类加载器在加载类的时候，首先将加载任务委托给父类加载器，依次递归，如果父类加载器可以完成加载任务，就成功返回；
	只有父类加载器无法完成加载任务或者没有父类加载器时才自己去加载

**BootClassLoader**

	加载Android Framework层class文件

**PathClassLoader**

	Android应用类加载器，可以加载指定的dex，以及jar、zip、apk中的classes.dex
	
**DexClassLoader**

	可以加载指定的dex，以及jar、zip、apk中的classes.dex（android应用中几乎没用到）
	
PathClassLoader和DexClassLoader没有什么区别，唯一的区别就是DexClassLoader需要传递一个路径，（这个路径是私有路径，不能是sdcard）去存储odex优化文件，而PathClassLoader不需要传递路径，使用默认路径（/data/davlic-cache）存储。

####15、Android 动画的种类及作用

**补间动画（视图动画）**

	平移动画
	缩放动画
	旋转动画
	透明度动画
	
	
	优点：简单方便，已封装好基础的动画效果
	缺点：仅控制整体实体效果，无法控制属性
	
**帧动画（视图动画）**

	将动画拆成帧的形式，且定义每一帧 = 每一张图片，然后以一定的速率切换
	
	
	优点：简单方便
	缺点：因使用大量的图片，容易引起OOM
	
**属性动画**

	在一定的时间间隔内，通过不断对值进行改变&不断将该值赋给对象的属性从而
	
	
	优点：作用对象进行了扩展，不只是view对象，甚至无对象也可以，动画效果丰富，不只是4种基本动画，还有其他动画
	
	
**插值器**

	定义：一个辅助动画实现的接口
	作用：设置属性值，从初始值过渡到结束值的变化规律（如匀速，加速，减速）
	应用场景：实现非线性运动的动画效果

**估值器**

	定义：一个辅助动画插值器的接口
	作用：设置属性值从初始值过渡到结束值的变化具体数值（插值器决定变化规律，如加速，加速匀速等，接下来的具体变化数值交给估值器）
	应用场景：协助插值器，实现非线性运动的动画效果
	
**总结**

	Q：内存溢出
	A：尽量避免使用帧动画，使用大量的图片
	
	
	Q：内存泄漏
	A：Activity退出的时候，要停止属性动画
	
	Q：兼容问题
	A：若在API11之前使用属性动画，则需要做好适配很兼容
	
	Q：屏幕适配
	A：实现动画的时候也要注意屏幕适配问题，避免使用px，否则会导致动画效果在不同的手机上不一样
	
	Q：与动画元素的交互
	A： 视图动画：无改变视图位置，即新位置无法触发点击事件，老位置才能触发点击事件
		属性动画：改变了视图位置，即新位置能触发点击事件，老位置不能触发点击事件

####16、Android中如何处理耗时操作
基本思想：将耗时操作放到非UI线程中执行

- AsyncTask
- Handler
- Thread
- Executors

处理思路

- 每次耗时操作启动一个线程，代价比较大
- 将所要的耗时操作放到一个队列，或者几个线程去执行，如HandlerThread和AsyncTask

####17、Handler源码解析

####18、两个Fragment之间怎么通信？

**传统方法**

- 一个Fragment持有另一个Fragment对象，然后直接调用其public方法
- 使用接口
- 使用广播

**谷歌推荐方法**

	从2018年谷歌IO大会开始，官方建议在Activity与Activity，Activity与Fragment，Fragment与Fragment
	之间使用LivedataBus进行数据的传递
	
[LiveDataBus使用简介](https://tech.meituan.com/2018/07/26/android-livedatabus.html)


####19、Activity的生命周期

####20、Fragment和Activity的关系

- Fragment是依赖于Activity的，不能独立存在，Activity是Fragment的一个容器。
- 一个Activity里可以有多个Fragment。
- 一个Fragment可以被多个Activity重用。
- Fragment有自己的生命周期，并能接收输入事件。
- 我们能在Activity运行时动态地添加或删除Fragment。

**Fragment的使用能解决以下问题：**

- 模块化（Modularity）：我们不必把所有代码全部写在Activity中，而是把代码写在各自的Fragment中，以方便不同业务的UI可以分离出来。
- 可重用（Reusability）：多个Activity可以重用一个Fragment。
- 可适配（Adaptability）：根据硬件的屏幕尺寸、屏幕方向，能够方便地实现不同的布局，这样用户体验更好。####21、为什么只使用默认的构造方法来创建Fragment
- 当系统内存不足的情况下，app处于后台条件下，内存会被系统回收，这时用户将app切换到前台，系统会重新将之前回收的内容实例化回来。
- 这个过程是Android系统通过两个方法来实现的:
- onSaveInstantceState是系统要回收该界面(Activity、Fragment)时调用的方法，用于保存该(Activity、Fragment)的实例变量到外存。
- onRestoreInstantceState是系统恢复该(Activity、Fragment)时调用的方法，用于恢复之前被回收的(Activity、Fragment)实例。####22、Bundle被用来传递数据，为什么不用HashMap代替- ArrayMap适合于小数据量操作，如果在数据量比较大的情况下，它的性能将退化。HashMap内部则是数组+链表结构，所以在数据量较少的时候，HashMap的Entry Array比ArrayMap占用更多的内存。而使用Bundle的场景大多数为小数据量。所以使用ArrayMap实现更合适。- Android中如果使用Intent来携带数据的话，需要数据是基本类型或者是可序列化类型，Bundle使用Parcelable进行序列化，而HashMap则是使用Serializable进行序列化。
####23、Activity，View，Window之间的关系####24、解释一下Android中的Intent(隐式&显式)**显示Intent是明确目标Activity的类名**
- 通过Intent(Context packageContext, Class<?> cls)构造方法- 通过Intent的setComponent()方法- 通过Intent的setClass/setClassName方法
**隐式Intent是没有明确目标Activity的类名**

- 通过设置Action、Data、Category，让系统来筛选出合适的Activity。筛选是根据所有的<intent-filter>来筛选。 
- 可以启动外部应用的Activity
- 当action相同时，他会弹出两个让你选择
####25、广播和EventBus的区别

- 广播是四大组件之一，EventBus是开源框架。- 广播不能直接执行耗时操作，如果超过10秒，会导致 ANR- 广播非常消耗资源，而EventBus非常轻量- 广播很容易获取Context和Intent- EventBus切换线程非常方便，只需要修改下注解就行了- 广播可以跨进程，而EventBus不可以####26、什么是 support libary？为什么要引入 support library ？
- support library引入主要是因为安卓版本有很多，新版本的更新也快，每个版本都会有一个开发版本号
- 在进行安卓开发的时候，我们通常需要考虑，应该选择什么样的 API 级别进行开发？
- 谷歌为此也给出了解决方案，我们在开发过程中可以给出三个设置项：minSdkVersion <= targetSdkVersion <= compileSdkVersion
- 当我们targetSdkVersion为 24 时，需要使用 API 28 的新增功能时，这时需要向后兼容，使用低版本的 API 兼容高版本的 API，而支持库就是这个作用，它会跟着每个新发布的 API 级别同步发布，所以这里支持库我们选择与 compileSdkVersion 一样的版本即可。support libary 只能保证我们针对24开发的APP在28上能正常运行，但运行效果不一定相同
####27、什么是ANR？怎么去避免ANR？
####28、Bitmap如何优化？三级缓存的思想与逻辑？
####29、Glide架构
####30、Dalvik和ART的区别

	Dalvik是Google公司自己设计的用于Android平台的java虚拟机，支持已转换为.dex格式的java应用程序的运行
	.dex格式是专为Dalvik应用设计的一种压缩格式，适合内存和处理器速度有限的系统运行。
	
	ART是Android Runtime，Android4.4引入的一个开发者选项，也是Android5.0及更高版本的默认模式。应用在
	安装的时候（Ahead-of-Time）AOT预编译字节码到机器语言，应用程序安装的时候回变慢，但是执行将更有效率，启动更快- Dalvik环境中，应用每次运行时，字节码都需要通过即时编译器 ( Just In Time，JIT) 转换为机器码。ART 环境中，应用会在安装的时候，就将字节码 预编译(Ahead Of Time，AOT) 成机器码，使其成为真正的本地应用。
- ART 占用的空间比 Dalvik 大，就是用 空间换时间。- ART 不用每次运行时都重复编译，减少了CPU 的使用频率，降低了能耗。
		Q:为什么不直接打包成机器码？
		A:因为Android 设备的CPU类型有的是Arm 有的是x86，机器码的效率很高，但是不能跨平台的，就想你写的c，c++程序，你在Windows上编译的，就不能拿到linux上用
ART能够兼容Dalvik，在ART上也能运行.dex文件

.dex---->AOT---->.oat---->ART


Dexopt：对dex文件进行验证和优化，优化后为.odex文件，优化是为了提高执行效率（Dalvik下）

DexAot：在安装时对dex文件执行dexopt优化之后，再将odex进行AOT提前编译操作，编译为OAT可执行文件（机器码）（ART下）


####31、内存溢出和内存泄漏

####32、AAPT

	AAPT（Android 资源打包工具，Android Asset Packaging Tool）是一种编译工具，供Android studio和
	Android Gradle Plugin用于编译和打包资源。而AAPT2是在AAPT上做了优化。Android Gradle Plugin
	3.0.0或者更高版本默认开启AAPT2。AAPT2会解析资源，为资源编索引并将资源编译为针对Android平台进行过优化
	的二进制文件
	
	
	
	AAPT2支持通过启用增量编译实现更快的资源编译，分为两步
	1.编译：将资源文件编译为二进制格式（.flat）
	2.链接：合并所有已编译的文件并将它们打包到一个软件包中
	
	这种拆分方法有助于提高增量分析的性能。（如：如果一个文件发生了改变，那么只需要重新编译这一个文件）####33、如何排除应用崩溃的原因

**开发过程中**
- Debug调试
- 看Logcat日志（搜索 fatal ，  exception ， Error等）
- 收集崩溃信息，保存本地后进行分析
- 找复现路径


**上线后**

- 友盟，Bugly，网易云捕
- 分析日志（搜索 fatal ，  exception ， Error等）####34、JNI说明与编译
####35、什么是NDK？它有什么用？

**JNI介绍**

- 定义：Java Native Interface，即 Java本地接口
- 作用：使得Java与本地其他类型语言（如C，C++）交互
- JNI是Java调用Native语言的一种特性
- JNI是属于Java的，与Android没有直接关系
- 实际中的驱动都是C/C++开发的，通过JNI，Java可以调用C/C++实现的驱动，从而扩展Java虚拟机的能力。另外，在高效的数学运算、游戏的实时渲染、音视频的编码和解码等方面，一般都是用C/C++开发的

**为什么使用JNI**

- 实际使用中，Java需要与本地代码进行交互
- 因为Java具备跨平台的特点，所以Java与本地代码的交互能力非常弱
- 采用JNI特性，增强Java与本地代码的交互能力

**实现步骤**

- 在Java中声明Native方法（即需要调用的本地方法）
- 编译上述Java源文件javac（得到.class文件）
- 通过javah命令导出JNI的头文件（.h文件）
- 使用Java需要交互的本地代码实现java中声明的Native方法
- 编译.so库文件
- 通过Java命令执行Java程序，最终实现Java调用本地代码

**NDK**

- 定义：Native Development Kit，是Android的一个工具包
- 和SDK性质一样，SDK用于java的开发，NDK用于C和C++的开发
- 作用：快速开发C,C++的动态库，并自动将so和应用一起打包生成apk
- 在Android的场景下使用JNI
- 提供了把.os和.apk打包的工具
- NDK提供的库有限，仅拥有算法效率和敏感问题
- 提供了交叉编译器，用于生成特定的CPU平台动态库

**特点**

- 运行效率高
- 代码安全性高
- 功能拓展性好
- 易于代码的复用和移植

**NDK和JNI的关系**

- JNI是实现Java调用C/C++的途径，NDK是Android中实现JNI的工具

####36、什么是surfaceView？

	Q: 当view中显示一些需要长时间绘制的画面怎么办？
	A: 普通的view会卡顿，这时用surfaceView

**定义**

	surfaceView是View的子类，是一个专门用于做频繁绘制的view的子类，他的绘制是在子线程中执行，所以频繁绘制不会阻塞主线程，使用他去做一下需要频繁绘制和长时间绘制效果会高很多

**特点**

1. 子线程绘制

		1.surfaceView是在一个新启的单独线程中可以重新绘制画面，而view必须在UI主线程中更新画面
		
		2.但这也带来一个问题，就是事件同步。比如你点击一下屏幕，你需要surfaceView中thread处理，一般就需要有一个Event queue的设计来保存touch Event，这会稍微复杂一点，因为涉及到线程同步
		
2. 双缓冲模式

		1.通俗来讲就是两个缓冲区，一个后台缓冲区和一个前台缓冲区，每次后台缓冲区接受数据，当填充完整后交换给前台缓冲区，这样就保证前台缓冲区里的数据是完整的
		
		2.在后台线程执行繁重的绘图任务，把所有绘制的东西缓存起来，绘制完成后，在回到UI主线程中，一次性的把绘制的东西渲染到屏幕上（本质：就是后台线程绘制，UI主线程渲染）	

####37、如何理解上下文Context？

- Application中的Context 
- Activity中的Context 
- Service中的Context


**Context内存泄漏问题**
     静态资源导致的内存泄漏     单例模式导致内存泄漏

**总结**

- Context是什么?Context是”运行上下文环境“,从代码角度看 Application,Service,Activity都是Context。
- 所有Context都是在应用的主线程ActivityThread中创建的,由于 Application,Service,Activity的祖先都是Context抽象类,所以在创 建它们的同时也会为每一个类创建一个ContextImpl类,ContextImpl 是Context的子类,真正实现Context功能方法的类。因此 Application,Service,Activity都关联着一个ContextImpl对象。
- 尽量少用Context对象去获取静态变量,静态方法,以及单例对象。 以免导致内存泄漏。
- 在创建与UI相关的地方,比如创建一个Dialog,或者在代码中创建一 个TextView,都用Activity的Context去创建。然而在引用静态资源, 创建静态方法,单例模式等情况下,使用生命周期更长的Application 的Context才不会导致内存泄漏。


####38、Android中你可以使用什么进行后台操作？
现有的后台操作机制

- Service（前台服务，后台服务）
- 线程池
- WorkManager（JobScheduler，Firebase JobDispatcher，自定义AlarmManager）

**Service和Thread的区别**

	
	Service 是android的一种机制，它是有生命周期的，当它运行的时候如果是Local Service，那么对应的 	Service 是运行在主进程的 main 线程上的。如果是Remote Service，那么对应的 Service 则是运行在
	独立进程的 main 线程上。Service中做耗时操作一样会导致ANR，要想在Service中做耗时操作，那要在
	Service中开启一个子线程

	
	Thread 是程序执行的最小单元，可以用 Thread 来执行一些异步的操作。

	
	Service的后台概念是，Service不依赖UI界面，就是有界面没界面都可以执行，只要进程在，Service就可以在，
	界面上显示的东西可以和Service运行的没有关系，它的后台是针对UI界面来说的（如心跳服务）
	
	Thread的后台概念是，开启一个子线程，执行任务，可以在这个子线程做耗时操作，而不影响UI线程的展示，它的后台
	是针对UI线程来说的
	
		
	他俩没有半毛钱关系，之所以把他俩联系在一起，是因为Service有一个后台的概念，当我们要播放一个音乐的时候
	我们会想到开启一个Service，然后Thread就是做耗时操作的，然后就把耗时操作和Service联系起来了。但是在
	Service中做耗时操作一样会导致ANR，
	
**Service中创建子线程和Activity中创建子线程的区别**

	1.Thread 的运行是独立的，也就是说当一个 Activity 被 finish 之后，如果没有主动停止 Thread 或者 	  Thread 里的 run 方法没有执行完毕的话，Thread 也会一直执行。因此这里会出现一个问题：当 Activity 	  被 finish 之后，不再持有该 Thread 的引用，也就是不能再控制该Thread
	
	2.在一个Activity中创建的子线程，其他Activity无法对其进行操作。
	
	3.Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使	  Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，	  使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。
	
**如何进行选择？**

- 此项工作是否可以延时或者需要在预定时间内完成？如点击提交按钮，完成网络请求提交 （用Thread）
- 此项工作是否需要采取保活措施长存于系统中？如地图定位，音乐播放（用Service）
- 此项工作是否是为了响应系统某种机制？如网络状态，电池状态（用WorkManager）

	
####39、onTrimMemory() 方法是什么？

能让activity得到内存情况的通知

	@Override  	public void onTrimMemory(int level) {  		super.onTrimMemory(level);  		switch (level) {  		case TRIM_MEMORY_UI_HIDDEN:  			// 进行资源释放操作  			break;  		}  	} 	

**七种状态，处于前台三种，后台四种**

**前台内存状态**

- TRIM_MEMORY_RUNNING_MODERATE ：表示应用程序正常运行，并且不会被杀掉。但是目前手机的内存已经有点低了- TRIM_MEMORY_RUNNING_LOW ：表示应用程序正常运行，并且不会被杀掉。但是目前手机的内存已经非常低了，我们应该去释放掉一些不必要的资源以提升系统的性能。- TRIM_MEMORY_RUNNING_CRITICAL ：表示应用程序仍然正常运行，但是系统已经根据LRU缓存规则杀掉了大部分缓存的进程了。

**后台内存状态**

- TRIM_MEMORY_UI_HIDDEN：UI组件全部不可见的时候才会触发，一旦触发了之后就说明用户已经离开了我们的程序- TRIM_MEMORY_BACKGROUND：表示手机目前内存已经很低了，系统准备开始根据LRU缓存来清理进程。这个时候我们的程序在LRU缓存列表的最近位置，是不太可能被清理掉的。- TRIM_MEMORY_MODERATE：表示手机目前内存已经很低了，并且我们的程序处于LRU缓存列表的中间位置。- TRIM_MEMORY_COMPLETE：表示手机目前内存已经很低了，并且我们的程序处于LRU缓存列表的最边缘位置。

####40、LayoutInflater.inflate函数意义与参数说明

所有xml文件的解析都是由LayoutInflater.inflate函数来解析的

	public View inflate (int resource, ViewGroup root)
	public View inflate (int resource, ViewGroup root, boolean attachToRoot)
	
	两个参数的inflate会调用三个参数的inflate，最终都调用
	public View inflate(XmlPullParser parser, ViewGroup root, boolean attachToRoot) {
   
	}	

可以分这四种情况分析

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    	android:layout_width="150dp"    //注释1     
    	android:layout_height="150dp"   //注释2  
    	android:background="#ff0000">

    	<Button
        	android:layout_width="wrap_content"
        	android:layout_height="wrap_content"
        	android:text="开开心心每一天" />

	</LinearLayout>

	inflater.inflate(R.layout.item, null, true);
	inflater.inflate(R.layout.item, null, false);
	inflater.inflate(R.layout.item, parent, true);
	inflater.inflate(R.layout.item, parent, false);
    
- 第一个参数：想要添加的布局

- 第二个参数：想要添加到哪个布局上面</br>
         （null和!null的区别 null时第一个参数中最外层的布局大小无效，也就是注释1注释2无效，!null时候最外层的布局大小有效，也就是注释1注释2有效）
         
- 第三个参数：是否直接添加到第二个参数布局上面</br>
         （true代表layout文件填充的View会被直接添加进parent,有可能报错，而传入false则代表创建的View会以其他方式被添加进parent）


####40、OKHttp和Retrofit 的区别与联系

- OKHttp 就是利用socket来建立Tcp/Ip连接，在socket的基础之上去完成Http报文的收发，网络功能库

- Retrofit是借助于OKHttp进行网络的访问，利用反射和代理对OkHttp的进一步封装，应用封装库，侧重于框架


####41、插件化、组件化与热修复的区别

**插件化  在程序运行阶段，利用java动态编程，也就是反射**

	插件化技术最初源于免安装运行Apk的想法，这个免安装的apk可以理解为插件化。支持插件化的App可以在运行时加载
	和运行插件，这样可以讲app中一些不常用的功能模块做成插件，一方面减小了安装包的大小，另一方面可以实现app功
	能的动态扩展
	
**热修复  在程序运行阶段，利用java动态编程，也就是反射**

	让应用能够在无需重新安装的情况实现更新，帮助应用快速建立动态修复能力
	
	热补丁节省Android大量应用市场的发布时间，同时用户也无需重新安装，只要上线就能无感知的更新。热补丁技术
	当前依然存在局限性
	1. 补丁只能针对单一客户端版本，随着版本差异变大，补丁的体积也会增大
	2. 补丁不支持所以的修改，例如AndroidManifest
	3. 补丁无论对代码还是资源的更新成功率无法达到100%

**组件化 在编译阶段，来完成项目的配置，可以说是开发阶段的插件化**

	组件化就是将一个项目拆分成若干个组件，分而治之。比如一个汽车的生产，也就是轮子，灯，座椅等等作为单独的组件
	由各自的工厂去生产维护，生产轮子就专门的做轮子，生产座椅就专门做座椅，等各个组件都做好了再拿到组装厂统一
	组装使用。
	组件化的目的主要是为了业务解耦，每个模块需要不同的功能
####42、APK的打包流程
我们打包成的APK其实就是一个压缩文件，我们将后缀改为.zip，然后就可以解压了，解压后得到以下文件
- assets文件夹：存放资源文件
- lib文件夹：放so库
- META-INF文件夹：放签名文件
- r文件夹：放R文件
- AndroidManifest.xml：清单文件
- classes.dex：java文件编译的
- resources.arsc：
[APK的打包流程](https://juejin.im/entry/58b78d1b61ff4b006cd47e5b)
####43、WMS
###### Activity与Window相关概念
- Activity只负责生命周期和事件处理
- Window只控制视图
- 一个Activity包含一个Window，如果Activity没有Window，那就相当于Service
- AMS统一调度所有应用程序的Activity
- WMS控制所有Window的显示与隐藏以及要显示的位置

###### Window
“Window”表明它是和窗口相关的，“窗口”是一个抽象的概念，从用户的角度来讲，它是一个“界面”；从SurfaceFlinger的角度来看，它是一个Layer，承载着和界面有关的数据和属性；从WMS角度来看，它是一个WIndowState，用于管理和界面有关的状态。

- 表示一个窗口的概念，是所有View的直接管理者，任何视图都通过Window呈现(点击事件由Window->DecorView->View; Activity的setContentView底层通过Window完成)
- Window是一个抽象类，具体实现是PhoneWindow
- 创建Window需要通过WindowManager创建
- WindowManager是外界访问Window的入口
- Window具体实现位于WindowManagerService中
- WindowManager和WindowManagerService的交互是通过IPC完成
- 定义窗口样式和行为的抽象基类，用于作为顶层的view加到WindowManager中，其实现类是PhoneWindow。
- 每个Window都需要指定一个Type（应用窗口、子窗口、系统窗口）。Activity对应的窗口是应用窗口；PopupWindow，ContextMenu，OptionMenu是常用的子窗口；像Toast和系统警告提示框（如ANR）就是系窗口，还有很多应用的悬浮框也属于系统窗口类型。

###### WindowManager
用来在应用与window之间的管理接口，管理窗口顺序，消息等。

###### WindowManagerService
简称Wms，WindowManagerService管理窗口的创建、更新和删除，显示顺序等，是WindowManager这个管理接品的真正的实现类。它运行在System_server进程，作为服务端，客户端（应用程序）通过IPC调用和它进行交互。

###### Token
这里提到的Token主是指窗口令牌（Window Token），是一种特殊的Binder令牌，Wms用它唯一标识系统中的一个窗口。



###### Window的type
- 应用窗口：层级范围是1~99
- 子窗口：层级范围是1000~1999
- 系统窗口：层级范围是2000~2999
- 各级别type值在WindowManager中的定义分别为：

- 应用窗口（1~99）


		//第一个应用窗口
		public static final int FIRST_APPLICATION_WINDOW = 1;
		//所有程序窗口的base窗口，其他应用程序窗口都显示在它上面
		public static final int TYPE_BASE_APPLICATION   = 1;
		//所有Activity的窗口，只能配合Activity在当前APP使用
		public static final int TYPE_APPLICATION        = 2;
		//目标应用窗口未启动之前的那个窗口
		public static final int TYPE_APPLICATION_STARTING = 3;
		//最后一个应用窗口
		public static final int LAST_APPLICATION_WINDOW = 99;

- 子窗口（1000~1999）


		//第一个子窗口
		public static final int FIRST_SUB_WINDOW        = 1000;
		// 面板窗口，显示于宿主窗口的上层,只能配合Activity在当前APP使用
		public static final int TYPE_APPLICATION_PANEL  = FIRST_SUB_WINDOW;
		// 媒体窗口（例如视频），显示于宿主窗口下层
		public static final int TYPE_APPLICATION_MEDIA  = FIRST_SUB_WINDOW+1;
		// 应用程序窗口的子面板，只能配合Activity在当前APP使用(PopupWindow默认就是这个Type)
		public static final int TYPE_APPLICATION_SUB_PANEL = FIRST_SUB_WINDOW+2;
		//对话框窗口,只能配合Activity在当前APP使用
		public static final int TYPE_APPLICATION_ATTACHED_DIALOG = FIRST_SUB_WINDOW+3;
		//
		public static final int TYPE_APPLICATION_MEDIA_OVERLAY  = FIRST_SUB_WINDOW+4;
		//最后一个子窗口
		public static final int LAST_SUB_WINDOW         = 1999;

- 系统窗口（2000~2999）

        //系统窗口，非应用程序创建
        public static final int FIRST_SYSTEM_WINDOW     = 2000;
        //状态栏，只能有一个状态栏，位于屏幕顶端，其他窗口都位于它下方
        public static final int TYPE_STATUS_BAR         = FIRST_SYSTEM_WINDOW;
        //搜索栏，只能有一个搜索栏，位于屏幕上方
        public static final int TYPE_SEARCH_BAR         = FIRST_SYSTEM_WINDOW+1; 
        //
        //电话窗口，它用于电话交互（特别是呼入），置于所有应用程序之上，状态栏之下,属于悬浮窗(并且给一个Activity的话按下HOME键会出现看不到桌面上的图标异常情况)
        public static final int TYPE_PHONE              = FIRST_SYSTEM_WINDOW+2;
        //
        //系统警告提示窗口，出现在应用程序窗口之上,属于悬浮窗, 但是会被禁止
        public static final int TYPE_SYSTEM_ALERT       = FIRST_SYSTEM_WINDOW+3;
        //
        //信息窗口，用于显示Toast, 不属于悬浮窗, 但有悬浮窗的功能, 缺点是在Android2.3上无法接收点击事件
        public static final int TYPE_TOAST              = FIRST_SYSTEM_WINDOW+5;
        //
        public static final int TYPE_KEYGUARD           = FIRST_SYSTEM_WINDOW+4;
        //锁屏窗口
        public static final int TYPE_KEYGUARD           = FIRST_SYSTEM_WINDOW+4;
        //系统顶层窗口，显示在其他一切内容之上，此窗口不能获得输入焦点，否则影响锁屏
        public static final int TYPE_SYSTEM_OVERLAY     = FIRST_SYSTEM_WINDOW+6;
        //电话优先，当锁屏时显示，此窗口不能获得输入焦点，否则影响锁屏
        public static final int TYPE_PRIORITY_PHONE     = FIRST_SYSTEM_WINDOW+7;
        //系统对话框窗口
        public static final int TYPE_SYSTEM_DIALOG      = FIRST_SYSTEM_WINDOW+8;
        //锁屏时显示的对话框
        public static final int TYPE_KEYGUARD_DIALOG    = FIRST_SYSTEM_WINDOW+9;
        //系统内部错误提示，显示在任何窗口之上
        public static final int TYPE_SYSTEM_ERROR       = FIRST_SYSTEM_WINDOW+10;
        //内部输入法窗口，显示于普通UI之上，应用程序可重新布局以免被此窗口覆盖
        public static final int TYPE_INPUT_METHOD       = FIRST_SYSTEM_WINDOW+11;
        //内部输入法对话框，显示于当前输入法窗口之上
        public static final int TYPE_INPUT_METHOD_DIALOG= FIRST_SYSTEM_WINDOW+12;
        //墙纸窗口
        public static final int TYPE_WALLPAPER          = FIRST_SYSTEM_WINDOW+13;
        //状态栏的滑动面板
        public static final int TYPE_STATUS_BAR_PANEL   = FIRST_SYSTEM_WINDOW+14;
        //安全系统覆盖窗口，这些窗户必须不带输入焦点，否则会干扰键盘
        public static final int TYPE_SECURE_SYSTEM_OVERLAY = FIRST_SYSTEM_WINDOW+15;
        //最后一个系统窗口
        public static final int LAST_SYSTEM_WINDOW      = 2999;

2.窗口flags显示属性在WindowManager中也有定义：

        //窗口特征标记
        public int flags;
        //当该window对用户可见的时候，允许锁屏
        public static final int FLAG_ALLOW_LOCK_WHILE_SCREEN_ON     = 0x00000001;
        //窗口后面的所有内容都变暗
        public static final int FLAG_DIM_BEHIND        = 0x00000002;
        //Flag：窗口后面的所有内容都变模糊
        public static final int FLAG_BLUR_BEHIND        = 0x00000004;
        //窗口不能获得焦点
        public static final int FLAG_NOT_FOCUSABLE      = 0x00000008;
        //窗口不接受触摸屏事件
        public static final int FLAG_NOT_TOUCHABLE      = 0x00000010;
        //即使在该window在可获得焦点情况下，允许该窗口之外的点击事件传递到当前窗口后面的的窗口去
        public static final int FLAG_NOT_TOUCH_MODAL    = 0x00000020;
        //当手机处于睡眠状态时，如果屏幕被按下，那么该window将第一个收到触摸事件
        public static final int FLAG_TOUCHABLE_WHEN_WAKING = 0x00000040;
        //当该window对用户可见时，屏幕出于常亮状态
        public static final int FLAG_KEEP_SCREEN_ON     = 0x00000080;
        //：让window占满整个手机屏幕，不留任何边界
        public static final int FLAG_LAYOUT_IN_SCREEN   = 0x00000100;
        //允许窗口超出整个手机屏幕
        public static final int FLAG_LAYOUT_NO_LIMITS   = 0x00000200;
        //window全屏显示
        public static final int FLAG_FULLSCREEN      = 0x00000400;
        //恢复window非全屏显示
        public static final int FLAG_FORCE_NOT_FULLSCREEN   = 0x00000800;
        //开启窗口抖动
        public static final int FLAG_DITHER             = 0x00001000;
        //安全内容窗口，该窗口显示时不允许截屏
        public static final int FLAG_SECURE             = 0x00002000;
        //锁屏时显示该窗口
        public static final int FLAG_SHOW_WHEN_LOCKED = 0x00080000;
        //系统的墙纸显示在该窗口之后
        public static final int FLAG_SHOW_WALLPAPER = 0x00100000;
        //当window被显示的时候，系统将把它当做一个用户活动事件，以点亮手机屏幕
        public static final int FLAG_TURN_SCREEN_ON = 0x00200000;
        //该窗口显示，消失键盘
        public static final int FLAG_DISMISS_KEYGUARD = 0x00400000;
        //当该window在可以接受触摸屏情况下，让因在该window之外，而发送到后面的window的触摸屏可以支持split touch
        public static final int FLAG_SPLIT_TOUCH = 0x00800000;
        //对该window进行硬件加速，该flag必须在Activity或Dialog的Content View之前进行设置
        public static final int FLAG_HARDWARE_ACCELERATED = 0x01000000;
        //让window占满整个手机屏幕，不留任何边界
        public static final int FLAG_LAYOUT_IN_OVERSCAN = 0x02000000;
        //透明状态栏
        public static final int FLAG_TRANSLUCENT_STATUS = 0x04000000;
        //透明导航栏
        public static final int FLAG_TRANSLUCENT_NAVIGATION = 0x08000000;




####44、dialog不能application的原因

dialog 的构造方法中 

	mWindowManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
 
 如果这里的context是Activity，那么我们来看看Activity的getSystemService
 
	@Override
	public Object getSystemService(@ServiceName @NonNull String name) {
		if (WINDOW_SERVICE.equals(name)) {//返回mWindowManager
			return mWindowManager;
			} 
			return super.getSystemService(name);
	}
    
而这个mWindowManager 是在attach的过程中 调用Window.setWindowManager 里面的

	mWindowManager = ((WindowManagerImpl)wm).createLocalWindowManager(this);
	WindowManagerImpl.createLocalWindowManager(Window parentWindow)
	
这里把Activity的attach中创建的phonewindow作为参数传递进来了parentWindow不为空
所以 它创建的WindowManagerImpl 是调用的
WindowManagerImpl(Context context, Window parentWindow) 
里面的mParentWindow是有值的
所以在WindowManagerGlobal.addView方法里面
//在activity创建则不为空

        if (parentWindow != null) {
            parentWindow.adjustLayoutParamsForSubWindow(wparams);
            
这adjustLayoutParamsForSubWindow 会给dialog的token赋值

	wp.token = mContainer == null ? mAppToken : mContainer.mAppToken;	
 
然而如果context 不是Activity
context.getSystemService最终的实现是在
SystemServiceRegistry.getSystemService的

	registerService(Context.WINDOW_SERVICE, WindowManager.class,
                new CachedServiceFetcher<WindowManager>() {
            @Override
            public WindowManager createService(ContextImpl ctx) {
                return new WindowManagerImpl(ctx);//在这里创建的WindowManagerImpl
            }});
            
调用的是WindowManagerImpl

	public WindowManagerImpl(Context context) {
        this(context, null);
    }
    
导致mParentWindow 为空
这样WindowManagerGlobal.addView的时候不能给token赋值
因为token赋值是在parentWindow不为空的情况下 在adjustLayoutParamsForSubWindow里面赋值的

	if (parentWindow != null) {
            parentWindow.adjustLayoutParamsForSubWindow(wparams);
            
####45、PKMS

**职责**

- 负责Android系统中Package的安装、升级、卸载
- 对外提供统一的信息查询功能，其中包括查询系统中匹配某Intent的Activities、BroadCastReceivers或Services等          

**apk安装的方式**

- 安装系统APK和预置的APK(第一次开机时安装,没有安装界)

		PackageManagerService的构造中会扫描对应目录下的apk,完成安装
		
- 网络下载应用安装――通过market应用完成，没有安装界面

		调用PackageManager的installPackage方法执行安装

- ADB工具安装,没有安装界面

		/repo/system/core/adb/commandline.cpp中install_app方法,该方法调用pm_command通过send_shell_command方法将数据发送到手机端的adbd守护进程中,adbd在收到PC中Console发来的数据之后启动一个Shell,然后执行pm脚本(pm位于/system/bin目录下).pm脚本通过app_process执行pm.jar包的main函数(\system\framework\pm.jar) 对应源码: /repo/frameworks/base/cmds/pm/src/com/android/commands/pm/Pm.java

- 第三方应用安装,有安装界面

**apk 安装流程**

- 将app复制到data/app目录下
- 扫描并解压安装包,并把dex文件(DVM字节码)保存到dalvik-cache目录,
- 在data/data目录下创建对应的应用数据目录.
- 解析apk的AndroidManifest.xml文件
- 显示快捷方式

**APK卸载流程**

- PKMS.deletePackage
- 从PMS的内部结构上删除Activity，Service，Provider等信息
- 更新Setting中的package信息
- 删除code，resource信息
- 删除.dex文件

####46、AMS

**系统启动流程**

- Loader           -------》   BootLoader
- Kernel           -------》   Linux Kernel
- Native           -------》   Init
- JNI              -------》   Zygote
- Java Framework


####47、系统服务的注册

- ServiceManager的addService()
- SystemServiceManager的startService()

**总结**

方式1. ServiceManager.addService():

	功能：向ServiceManager注册该服务.
	特点：服务往往直接或间接继承于Binder服务；
	举例：input, window, package；
	
方式2. SystemServiceManager.startService:

	功能：
		创建服务对象；
		执行该服务的onStart()方法；该方法会执行上面的SM.addService()；
		根据启动到不同的阶段会回调onBootPhase()方法；
		另外，还有多用户模式下用户状态的改变也会有回调方法；例如onStartUser();
	特点：服务往往自身或内部类继承于SystemService；
	举例：power, activity；
**两种方式真正注册服务的过程都会调用到ServiceManager.addService()方法. 对于方式2多了一个服务对象创建以及 根据不同启动阶段采用不同的动作的过程。可以理解为方式2比方式1的功能更丰富。**







 
####手写单例

####手写快速排序算法













		