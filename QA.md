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

####37、什么是surfaceView？

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














####36、

	
 
####手写单例

####手写快速排序算法













		