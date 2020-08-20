
###Matisse 引起的java.lang.NoSuchMethodError: No virtual method override(II)Lcom/bumptech/glide/request/BaseRequestOptions

#####一、具体错误如下：

	java.lang.NoSuchMethodError: No virtual method override(II)Lcom/bumptech/glide/request/BaseRequestOptions; in class Lcom/bumptech/glide/request/RequestOptions; or its super classes (declaration of 'com.bumptech.glide.request.RequestOptions' appears in /data/app/com.sun.basedemo-w6WBqV4rUaaVzMliuHo2sg==/base.apk)
	at com.zhihu.matisse.engine.impl.GlideEngine.loadThumbnail(GlideEngine.java:40)
	at com.zhihu.matisse.internal.ui.widget.MediaGrid.setImage(MediaGrid.java:117)
	at com.zhihu.matisse.internal.ui.widget.MediaGrid.bindMedia(MediaGrid.java:84)
	at com.zhihu.matisse.internal.ui.adapter.AlbumMediaAdapter.onBindViewHolder(AlbumMediaAdapter.java:122)
	at com.zhihu.matisse.internal.ui.adapter.RecyclerViewCursorAdapter.onBindViewHolder(RecyclerViewCursorAdapter.java:45)
	at androidx.recyclerview.widget.RecyclerView$Adapter.onBindViewHolder(RecyclerView.java:6781)
	at androidx.recyclerview.widget.RecyclerView$Adapter.bindViewHolder(RecyclerView.java:6823)
	at androidx.recyclerview.widget.RecyclerView$Recycler.tryBindViewHolderByDeadline(RecyclerView.java:5752)
	at androidx.recyclerview.widget.RecyclerView$Recycler.tryGetViewHolderForPositionByDeadline(RecyclerView.java:6019)
	at androidx.recyclerview.widget.RecyclerView$Recycler.getViewForPosition(RecyclerView.java:5858)
	at androidx.recyclerview.widget.RecyclerView$Recycler.getViewForPosition(RecyclerView.java:5854)
	at androidx.recyclerview.widget.LinearLayoutManager$LayoutState.next(LinearLayoutManager.java:2230)
	at androidx.recyclerview.widget.GridLayoutManager.layoutChunk(GridLayoutManager.java:557)
	at androidx.recyclerview.widget.LinearLayoutManager.fill(LinearLayoutManager.java:1517)
	at androidx.recyclerview.widget.LinearLayoutManager.onLayoutChildren(LinearLayoutManager.java:612)
	at androidx.recyclerview.widget.GridLayoutManager.onLayoutChildren(GridLayoutManager.java:171)
	at androidx.recyclerview.widget.RecyclerView.dispatchLayoutStep2(RecyclerView.java:3924)
	at androidx.recyclerview.widget.RecyclerView.dispatchLayout(RecyclerView.java:3641)
	at androidx.recyclerview.widget.RecyclerView.onLayout(RecyclerView.java:4194)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.FrameLayout.layoutChildren(FrameLayout.java:323)
	at android.widget.FrameLayout.onLayout(FrameLayout.java:261)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.FrameLayout.layoutChildren(FrameLayout.java:323)
	at android.widget.FrameLayout.onLayout(FrameLayout.java:261)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.RelativeLayout.onLayout(RelativeLayout.java:1083)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.FrameLayout.layoutChildren(FrameLayout.java:323)
	at android.widget.FrameLayout.onLayout(FrameLayout.java:261)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.LinearLayout.setChildFrame(LinearLayout.java:1791)
	at android.widget.LinearLayout.layoutVertical(LinearLayout.java:1635)
	at android.widget.LinearLayout.onLayout(LinearLayout.java:1544)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.FrameLayout.layoutChildren(FrameLayout.java:323)
	at android.widget.FrameLayout.onLayout(FrameLayout.java:261)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.LinearLayout.setChildFrame(LinearLayout.java:1791)
	at android.widget.LinearLayout.layoutVertical(LinearLayout.java:1635)
	at android.widget.LinearLayout.onLayout(LinearLayout.java:1544)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.widget.FrameLayout.layoutChildren(FrameLayout.java:323)
	at android.widget.FrameLayout.onLayout(FrameLayout.java:261)
	at com.android.internal.policy.DecorView.onLayout(DecorView.java:947)
	at android.view.View.layout(View.java:19811)
	at android.view.ViewGroup.layout(ViewGroup.java:6240)
	at android.view.ViewRootImpl.performLayout(ViewRootImpl.java:2584)
	at android.view.ViewRootImpl.performTraversals(ViewRootImpl.java:2300)
	at android.view.ViewRootImpl.doTraversal(ViewRootImpl.java:1456)
	at android.view.ViewRootImpl$TraversalRunnable.run(ViewRootImpl.java:6964)
	at android.view.Choreographer$CallbackRecord.run(Choreographer.java:932)
	at android.view.Choreographer.doCallbacks(Choreographer.java:744)
	at android.view.Choreographer.doFrame(Choreographer.java:676)
	at android.view.Choreographer$FrameDisplayEventReceiver.run(Choreographer.java:918)
	at android.os.Handler.handleCallback(Handler.java:790)
	at android.os.Handler.dispatchMessage(Handler.java:99)
	at android.os.Looper.loop(Looper.java:192)
	at android.app.ActivityThread.main(ActivityThread.java:6738)
	at java.lang.reflect.Method.invoke(Native Method)
	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:549)
	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:825)
	
######二、错误原因：由于Matisse框架中也有使用Glide框架，我的项目中也有使用Glide框架，但是两个框架的版本不一致，导致了这个错误，我项目中使用的Matisse版本为0.5.3-beta3：`implementation 'com.zhihu.android:matisse:0.5.3-beta3'`

	    在matisse:0.5.3-beta3'中使用的Glide版本如下：
	    implementation 'com.github.bumptech.glide:glide:4.9.0'
	    
	    我项目中使用的版本如下：
	    implementation 'com.github.bumptech.glide:glide:3.7.0'
	    
#####三、解决方案：怎么改还用我说吗？是的，将项目中的版本改为和Matisse中Glide的版本一致即可。

		项目中使用的版本改为：
	    implementation 'com.github.bumptech.glide:glide:4.9.0'
	    
###希望对你有用，谢谢