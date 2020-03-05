###Glide缓存
Glide又将它分成了两个模块，一个是内存缓存，一个是硬盘缓存。
#####缓存Key
既然是缓存功能，就必然会有用于进行缓存的Key。那么Glide的缓存Key是怎么生成的呢？我不得不说，Glide的缓存Key生成规则非常繁琐，决定缓存Key的参数竟然有10个之多。

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache01.jpg" width = "480" height = "222"> 

调用了fetcher.getId()方法获得了一个id字符串，这个字符串也就是我们要加载的图片的唯一标识，比如说如果是一张网络上的图片的话，那么这个id就是这张图片的url地址。

将这个id连同着signature、width、height等等10个参数一起传入到EngineKeyFactory的buildKey()方法当中，从而构建出了一个EngineKey对象，这个EngineKey也就是Glide中的缓存Key了。

可见，决定缓存Key的条件非常多，即使你用override()方法改变了一下图片的width或者height，也会生成一个完全不同的缓存Key。

EngineKey类的源码大家有兴趣可以自己去看一下，其实主要就是重写了equals()和hashCode()方法，保证只有传入EngineKey的所有参数都相同的情况下才认为是同一个EngineKey对象


#####一、内存缓存
**1)、LruCache算法**</br>
**2)、使用的就是弱引用**

	Glide.with(this)
     .load(url)
     .skipMemoryCache(true)   //skipMemoryCache()方法并传入true，就表示禁用掉Glide的内存缓存功能。
     .into(imageView);
     

LruCache算法（Least Recently Used），也叫近期最少使用算法。它的主要算法原理就是把最近使用的对象用强引用存储在LinkedHashMap中，并且把最近最少使用的对象在缓存值达到预设定值之前从内存中移除。</br>

在load()方法中，我们当时分析到了在loadGeneric()方法中会调用Glide.buildStreamModelLoader()方法来获取一个ModelLoader对象。

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache02.jpg" width = "480" height = "390"> 

在第11行去构建ModelLoader对象的时候，先调用了一个Glide.get()方法，而这个方法就是关键。我们可以看到，get()方法中实现的是一个单例功能，而创建Glide对象则是在第24行调用GlideBuilder的createGlide()方法来创建的

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache03.jpg" width = "480" height = "390"> 

这里也就是构建Glide对象的地方了。那么观察第22行，你会发现这里new出了一个LruResourceCache，并把它赋值到了memoryCache这个对象上面。你没有猜错，这个就是Glide实现内存缓存所使用的LruCache对象了。

刚才在Engine的load()方法中我们已经看到了生成缓存Key的代码，而内存缓存的代码其实也是在这里实现的，那么我们重新来看一下Engine类load()方法的完整源码

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache04.jpg" width = "480" height = "400">

可以看到，这里在第17行调用了loadFromCache()方法来获取缓存图片，如果获取到就直接调用cb.onResourceReady()方法进行回调。如果没有获取到，则会在第26行调用loadFromActiveResources()方法来获取缓存图片，获取到的话也直接进行回调。只有在两个方法都没有获取到缓存的情况下，才会继续向下执行，从而开启线程来加载图片。

也就是说，Glide的图片加载过程中会调用两个方法来获取内存缓存，loadFromCache()和loadFromActiveResources()。这两个方法中一个使用的就是LruCache算法，另一个使用的就是弱引用。我们来看一下它们的源码：

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache05.jpg" width = "480" height = "400">
<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache06.jpg" width = "480" height = "220">



#####二、硬盘缓存

	Glide.with(this)
     .load(url)
     .diskCacheStrategy(DiskCacheStrategy.NONE)    //传入DiskCacheStrategy.NONE，就可以禁用掉Glide的硬盘缓存功能了
     .into(imageView);

这个diskCacheStrategy()方法基本上就是Glide硬盘缓存功能的一切，它可以接收四种参数：

1. DiskCacheStrategy.NONE： 表示不缓存任何内容。
2. DiskCacheStrategy.SOURCE： 表示只缓存原始图片。
3. DiskCacheStrategy.RESULT： 表示只缓存转换过后的图片（默认选项）。
4. DiskCacheStrategy.ALL ： 表示既缓存原始图片，也缓存转换过后的图片。


#####三、高级用法

虽说Glide将缓存功能高度封装之后，使得用法变得非常简单，但同时也带来了一些问题。

比如之前有一位群里的朋友就跟我说过，他们项目的图片资源都是存放在七牛云上面的，而七牛云为了对图片资源进行保护，会在图片url地址的基础之上再加上一个token参数。也就是说，一张图片的url地址可能会是如下格式：

	http://url.com/image.jpg?token=d9caa6e02c990b0a
	
而使用Glide加载这张图片的话，也就会使用这个url地址来组成缓存Key。

但是接下来问题就来了，token作为一个验证身份的参数并不是一成不变的，很有可能时时刻刻都在变化。而如果token变了，那么图片的url也就跟着变了，图片url变了，缓存Key也就跟着变了。结果就造成了，明明是同一张图片，就因为token不断在改变，导致Glide的缓存功能完全失效了。

这其实是个挺棘手的问题，而且我相信绝对不仅仅是七牛云这一个个例，大家在使用Glide的时候很有可能都会遇到这个问题。

那么该如何解决这个问题呢？我们还是从源码的层面进行分析，首先再来看一下Glide生成缓存Key这部分的代码：

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache07.jpg" width = "480" height = "250">

来看一下第11行，刚才已经说过了，这个id其实就是图片的url地址。那么，这里是通过调用fetcher.getId()方法来获取的图片url地址，而我们在上一篇文章中已经知道了，fetcher就是HttpUrlFetcher的实例，我们就来看一下它的getId()方法的源码吧，如下所示：

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache08.jpg" width = "480" height = "235">

可以看到，getId()方法中又调用了GlideUrl的getCacheKey()方法。那么这个GlideUrl对象是从哪里来的呢？其实就是我们在load()方法中传入的图片url地址，然后Glide在内部把这个url地址包装成了一个GlideUrl对象。

很明显，接下来我们就要看一下GlideUrl的getCacheKey()方法的源码了，如下所示

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache09.jpg" width = "480" height = "350">

这里我将代码稍微进行了一点简化，这样看上去更加简单明了。GlideUrl类的构造函数接收两种类型的参数，一种是url字符串，一种是URL对象。然后getCacheKey()方法中的判断逻辑非常简单，如果传入的是url字符串，那么就直接返回这个字符串本身，如果传入的是URL对象，那么就返回这个对象toString()后的结果。

其实看到这里，我相信大家已经猜到解决方案了，因为getCacheKey()方法中的逻辑太直白了，直接就是将图片的url地址进行返回来作为缓存Key的。那么其实我们只需要重写这个getCacheKey()方法，加入一些自己的逻辑判断，就能轻松解决掉刚才的问题了。

创建一个MyGlideUrl继承自GlideUrl，代码如下所示：

<img src = "https://github.com/langsun/Learn/blob/master/LoadImage/image/glideCache09.jpg" width = "480" height = "320">
可以看到，这里我们重写了getCacheKey()方法，在里面加入了一段逻辑用于将图片url地址中token参数的这一部分移除掉。这样getCacheKey()方法得到的就是一个没有token参数的url地址，从而不管token怎么变化，最终Glide的缓存Key都是固定不变的了。

当然，定义好了MyGlideUrl，我们还得使用它才行，将加载图片的代码改成如下方式即可：

	Glide.with(this)
     .load(new MyGlideUrl(url))
     .into(imageView);

也就是说，我们需要在load()方法中传入这个自定义的MyGlideUrl对象，而不能再像之前那样直接传入url字符串了。不然的话Glide在内部还是会使用原始的GlideUrl类，而不是我们自定义的MyGlideUrl类。















