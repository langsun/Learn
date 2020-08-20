###记一次由 AndroidManifest.xml 添加 FileProvider 引起的 Manifest merger failed with multiple errors, see logs

**故事较长，看完，不仅可以帮你解决问题，还会让你学到不少东西**

事情的起因是这样的，由于功能需要，需要在项目中添加FileProvider，然后就兴致冲冲开始撸

#####一、将 FileProvider 正确的添加到 AndroidManifest.xml 
将一下代码添加至AndroidManifest.xml中

	    <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="com.xf.demo.fileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_path" />
        </provider>
        
然后发现不对，我的项目已经迁移到AndroidX了，这里    	

	android:name="android.support.v4.content.FileProvider"
就直接报错，然后我把它改为AndroidX的FileProvider

	  <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="com.xf.demo.fileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_path" />
      </provider>
      
然后心里就开始有疑问：

	android:name="android.support.v4.content.FileProvider"
	改为
	android:name="androidx.core.content.FileProvider"
	
	
	那么
	android:name="android.support.FILE_PROVIDER_PATHS"
	需要改吗
	
	答案是不需要的
官方给出的AndroidX配置FileProvider[设置文件共享](https://developer.android.com/training/secure-file-sharing/setup-sharing)

	 <provider
          android:name="androidx.core.content.FileProvider"
          android:authorities="com.example.myapp.fileprovider"
          android:grantUriPermissions="true"
          android:exported="false">
          <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/filepaths" />
      </provider>
	
	
#####二、报错：error running 'app': Default Activity not found

首先发现一个报错，android studio 的app项目上出现一个红叉，如图
<img src = "https://github.com/langsun/Learn/blob/master/MistakesCollection/image/a1_1.jpg" width = "300" height = "50">

并且点击Run App 时，报错 `error running 'app': Default Activity not found`，有报错就好说，然后一顿的开始google，然后也找到的[答案](https://juejin.im/post/5b6586b8e51d45191e0d3225)

原因一 没有配置启动Activity

	    <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>


	
原因二 本来项目就没有Activity，进入设置页面，将launch设置为Nothing

但是我的都不是，我项目有配置启动activity，这为什么报	`error running 'app': Default Activity not found`呢

#####三、报错：Manifest merger failed with multiple errors, see logs
然后我Clear Project --> Rebuild Project，最终出现一个错误，`Manifest merger failed with multiple errors, see logs`，我cao，这还没详细的信息，去哪看logs呢，

	1. 通过Terminal窗口，运行 gradlew processDebugManifest --stacktrace，就得到日志

	2. 通过Android studio中的 Help---》Show Log in Finder就打开了idea.log文件所在文件夹，然后打开idea.log，就能看的日志了

发现原因：`由于多个lib库中定义类相同所以冲突，重写FileProvider类即可。`

然后就重写FileProvider类

	
	public class MyFileProvider extends FileProvider {
 
	}
	
在AndroidManifest.xml中配置如下

	  <provider
            android:name=".pro.MyProvider"
            android:authorities="com.xf.demo.fileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_path" />
       </provider>

 然后我Clear Project --> Rebuild Project --> Run ,就ok了

	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	