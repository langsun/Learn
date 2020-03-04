###Glide

#####Glide
#####.with(this)
以Context重载方法如下：


	public static RequestManager with(Context context) {
        RequestManagerRetriever retriever = RequestManagerRetriever.get();
        return retriever.get(context);
    }

with()方法是Glide类中的一组静态方法，先调用RequestManagerRetriever的静态get()方法得到一个RequestManagerRetriever对象，然后再调用RequestManagerRetriever的实例get()方法，去获取RequestManager对象。它有好几个方法重载(Context、Activity、FragmentActivity、android.app.Fragment、Fragment)，实际上只有两种情况而已，即传入Application类型的参数，和传入非Application类型的参数。</br>

**Application类型的参数：**如果在Glide.with()方法中传入的是一个Application对象，那么这里就会调用带有Context参数的get()方法重载，然后会调用getApplicationManager()方法来获取一个RequestManager对象。其实这是最简单的一种情况，因为Application对象的生命周期即应用程序的生命周期，因此Glide并不需要做什么特殊的处理，它自动就是和应用程序的生命周期是同步的，如果应用程序关闭的话，Glide的加载也会同时终止。</br>

**非Application参数的情况：**不管你在Glide.with()方法中传入的是Activity、FragmentActivity、v4包下的Fragment、还是app包下的Fragment，最终的流程都是一样的，那就是会向当前的Activity当中添加一个隐藏的Fragment。那么这里为什么要添加一个隐藏的Fragment呢？因为Glide需要知道加载的生命周期。很简单的一个道理，如果你在某个Activity上正在加载着一张图片，结果图片还没加载出来，Activity就被用户关掉了，那么图片还应该继续加载吗？当然不应该。可是Glide并没有办法知道Activity的生命周期，于是Glide就使用了添加隐藏Fragment的这种小技巧，因为Fragment的生命周期和Activity是同步的，如果Activity被销毁了，Fragment是可以监听到的，这样Glide就可以捕获这个事件并停止图片加载了。

如果我们是在非主线程当中使用的Glide，那么不管你是传入的Activity还是Fragment，都会被强制当成Application来处理。


#####.load(url)
load()方法是在RequestManager类中，Glide是支持图片URL字符串、图片本地路径等等加载形式的，因此RequestManager中也有很多个load()方法的重载（String，File，Integer，Uri，Url，byte[],T）等等。</br>
以load(String)为例：

	public DrawableTypeRequest<String> load(String string) {
        return (DrawableTypeRequest<String>) fromString().load(string);
    }
    public DrawableTypeRequest<String> fromString() {
        return loadGeneric(String.class);
    }
    private <T> DrawableTypeRequest<T> loadGeneric(Class<T> modelClass) {
        ModelLoader<T, InputStream> streamModelLoader = Glide.buildStreamModelLoader(modelClass, context);
        ModelLoader<T, ParcelFileDescriptor> fileDescriptorModelLoader =
                Glide.buildFileDescriptorModelLoader(modelClass, context);
        if (modelClass != null && streamModelLoader == null && fileDescriptorModelLoader == null) {
            throw new IllegalArgumentException("Unknown type " + modelClass + ". You must provide a Model of a type for"
                    + " which there is a registered ModelLoader, if you are using a custom model, you must first call"
                    + " Glide#register with a ModelLoaderFactory for your custom model class");
        }
        return optionsApplier.apply(
                new DrawableTypeRequest<T>(modelClass, streamModelLoader, fileDescriptorModelLoader, context,
                        glide, requestTracker, lifecycle, optionsApplier));
    }

先调用了fromString()方法，在fromString()方法中就直接调用了loadGeneric()方法，并且指定参数为String.class.</br>

其实loadGeneric()方法也没几行代码，这里分别调用了Glide.buildStreamModelLoader()方法和Glide.buildFileDescriptorModelLoader()方法来获得ModelLoader对象。ModelLoader对象是用于加载图片的，而我们给load()方法传入不同类型的参数，这里也会得到不同的ModelLoader对象。</br>

最后我们可以看到，loadGeneric()方法是要返回一个DrawableTypeRequest对象的，因此在loadGeneric()方法的最后又去new了一个DrawableTypeRequest对象，然后把刚才获得的ModelLoader对象，还有一大堆杂七杂八的东西都传了进去。</br>

DrawableTypeRequest的父类是DrawableRequestBuilder，DrawableRequestBuilder中有很多个方法，这些方法其实就是Glide绝大多数的API了。里面有不少我们在上篇文章中已经用过了，比如说placeholder()方法、error()方法、diskCacheStrategy()方法、override()方法等。当然还有很多暂时还没用到的API，我们会在后面的文章当中学习。</br>

到这里，第二步load()方法也就分析结束了。为什么呢？因为你会发现DrawableRequestBuilder类中有一个into()方法（上述代码第220行），也就是说，最终load()方法返回的其实就是一个DrawableTypeRequest对象。那么接下来我们就要进行第三步了，分析into()方法中的逻辑。

因为into()方法也是整个Glide图片加载流程中逻辑最复杂的地方。

不过从刚才的代码来看，into()方法中并没有任何逻辑，只有一句super.into(view)。那么很显然，into()方法的具体逻辑都是在DrawableRequestBuilder的父类当中了。

DrawableRequestBuilder的父类是GenericRequestBuilder


#####.into(imageView);

into()方法也是整个Glide图片加载流程中逻辑最复杂的地方。不过从刚才的代码来看，into()方法中并没有任何逻辑，只有一句super.into(view)。那么很显然，into()方法的具体逻辑都是在DrawableRequestBuilder的父类当中了。DrawableRequestBuilder的父类是GenericRequestBuilder。</br>
GenericRequestBuilder类中的into()

	public Target<TranscodeType> into(ImageView view) {
    	Util.assertMainThread();
    	if (view == null) {
        	throw new IllegalArgumentException("You must pass in a non null View");
    	}
    	if (!isTransformationSet && view.getScaleType() != null) {
        	switch (view.getScaleType()) {
            	case CENTER_CROP:
                	applyCenterCrop();
                	break;
            	case FIT_CENTER:
            	case FIT_START:
            	case FIT_END:
               		applyFitCenter();
                	break;
            	//$CASES-OMITTED$
            	default:
                	// Do nothing.
        	}
    	}
    	return into(glide.buildImageViewTarget(view, transcodeClass));
	}

最后一行代码先是调用了glide.buildImageViewTarget()方法，这个方法会构建出一个Target对象，Target对象则是用来最终展示图片用的















