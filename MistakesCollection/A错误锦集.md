###错误锦集
1. [ERROR: Failed to resolve: com.github.chrisbanes:PhotoView:2.3.0 ](https://github.com/Lichenwei-Dev/ImagePicker/issues/1)
2. [The option 'android.enableAapt2' is deprecated and should not be used anymore](https://blog.csdn.net/L_201607/article/details/80679814)
3. [Android binary layout inflate exception : ClassNotFoundException: Didn't find class “android.view.layout” on path:](https://stackoverflow.com/questions/41485577/android-binary-layout-inflate-exception-classnotfoundexception-didnt-find-cl)
4. [Android Studio 不显示LogCat 日志 的几种解决办法](https://blog.csdn.net/z1web/article/details/84726720)
5. [解决 Android 输出Logcat时报错[ read: unexpected EOF! ]](https://www.jianshu.com/p/4679f7bfac3a)
6. [Android高版本联网失败报错:Cleartext HTTP traffic to xxx not permitted解决方法](https://blog.csdn.net/gengkui9897/article/details/82863966)
7. [Android Studio编译失败：Caused by: java.lang.RuntimeException](https://blog.csdn.net/yinxing2008/article/details/82865568) 
3. 依赖冲突 ：Program type already present: com.netease.nimlib.chatroom.a.b
    
		一、引入网易云信IM文档中的依赖配置

		//    网易云信IM
    	// 基础功能 (必需)
		implementation 'com.netease.nimlib:basesdk:7.8.0'
    	// 聊天室需要
		implementation 'com.netease.nimlib:chatroom:7.8.0'
    	// 通过云信来集成小米等厂商推送需要
		implementation 'com.netease.nimlib:push:7.8.0'
    	// 超大群需要
		implementation 'com.netease.nimlib:superteam:7.8.0'
    	// 全文检索插件
		implementation 'com.netease.nimlib:lucene:7.8.0'
		
	
		二、下面是网易云信提供的高级版api，也就是对之前的网易云信IM文档中api进行了封装实现，功能便于用户使用其SDK。
		
		//引入uikit
    	implementation project(path: ':uikit')
    	
		项目把这两部分都添加在了dependencies{}模块中，很显然uikit已经引进了基本的配置，才成为高级版，所以这里就出现了依赖冲突，将第一部分的依赖配置注释掉就可以了
 
 7. ScrollView 和 RecyclerView 显示不全问题

		ScrollView改为NestedScrollView
 
 