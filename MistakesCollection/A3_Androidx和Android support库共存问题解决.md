###No resource identifier found for attribute 'appComponentFactory' in package 'android'的解决方案

####报错信息如下

	app/build/intermediates/manifests/full/debug/AndroidManifest.xml

		No resource identifier found for attribute 'appComponentFactory' in package 'android'

	string 'permission_denied' has no default translation.

	java.util.concurrent.ExecutionException: com.android.ide.common.process.ProcessException: Error while executing process /Users/sun/Development/android-sdk-macosx/build-tools/26.0.2/aapt with arguments {package -f --no-crunch -I /Users/sun/Development/android-sdk-macosx/platforms/android-27/android.jar -M /Users/sun/XuanFeng/chats/chat/app/build/intermediates/manifests/full/debug/AndroidManifest.xml -S /Users/sun/XuanFeng/chats/chat/app/build/intermediates/res/merged/debug -m -J /Users/sun/XuanFeng/chats/chat/app/build/generated/source/r/debug -F /Users/sun/XuanFeng/chats/chat/app/build/intermediates/res/debug/resources-debug.ap_ --custom-package com.xf.chat -0 apk --preferred-density xxhdpi --output-text-symbols /Users/sun/XuanFeng/chats/chat/app/build/intermediates/symbols/debug --no-version-vectors}

	com.android.ide.common.process.ProcessException: Error while executing process /Users/sun/Development/android-sdk-macosx/build-tools/26.0.2/aapt with arguments {package -f --no-crunch -I /Users/sun/Development/android-sdk-macosx/platforms/android-27/android.jar -M /Users/sun/XuanFeng/chats/chat/app/build/intermediates/manifests/full/debug/AndroidManifest.xml -S /Users/sun/XuanFeng/chats/chat/app/build/intermediates/res/merged/debug -m -J /Users/sun/XuanFeng/chats/chat/app/build/generated/source/r/debug -F /Users/sun/XuanFeng/chats/chat/app/build/intermediates/res/debug/resources-debug.ap_ --custom-package com.xf.chat -0 apk --preferred-density xxhdpi --output-text-symbols /Users/sun/XuanFeng/chats/chat/app/build/intermediates/symbols/debug --no-version-vectors}

	org.gradle.process.internal.ExecException: Process 'command '/Users/sun/Development/android-sdk-macosx/build-tools/26.0.2/aapt'' finished with non-zero exit value 1

	Process 'command '/Users/sun/Development/android-sdk-macosx/build-tools/26.0.2/aapt'' finished with non-zero exit value 1
	
	
####经查找发现是Androidx和Android support库共存问题，既然发现问题了，那么就解决问题

######一. 修改文件

	第一步，修改app的build.gradle
	compileSdkVersion=28， targetSdkVersion = 28    
	implementation 'com.android.support:appcompat-v7:28.0.0'
	
	第二步，修改项目的build.gradle中的build:gradle版本到3.2.0以上
	classpath 'com.android.tools.build:gradle:3.2.0'
	
	第三步，修改gradle-wrapper.properties，和上面的build.gradle版本对应
	distributionUrl=https\://services.gradle.org/distributions/gradle-4.6-all.zip
	
######二. 选择项目右键→Refactor→Migrate to Androidx...
######三. 选中所有需要重命名的目录，执行Do Refactor

感谢参考文章：</br>
https://www.jianshu.com/p/f7a7a8765294</br>
https://stackoverflow.com/questions/50266035/no-resource-identifier-found-for-attribute-appcomponentfactory-in-package-and
 

