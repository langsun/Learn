###进程间通信
#####一、基本描述

<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC01.jpg" width = "600" height = "350">

1. 进程A访问进程B，肯定是访问进程B中的变量或者函数
2. 在安卓手机中，每一个进程都会单独的分配一块内存，所有的变量和方法都放在内存中
3. 不同的进程，他们的内存肯定是不相同的，那么即使是相同的类名，所new出来的同名字的对象，放在各自内存的堆里面，那么他们也是不同的，即使名字相同，他们也不能进行数据交互，因为他们的内存不同。
4. 实际上表示进程A和进程B不能直接进行通信
5. 那么Android是怎么做的呢？
6. Android会采用底层通信，因为Android系统的内核是Linux，我们经常看[Android四层结构图](https://www.cnblogs.com/yinrw/p/10694178.html)，在Linux层有一个Binder（IPC）Driver，这个Binder Driver里面是C++写的，进程A和进程B是java写的。Linux的Binder驱动会分享出一块公用内存，供java访问。java层利用JNI接口，通过NDK访问C代码
7. 在Binder驱动中，存在这各个对象的Binder引用。
8. 每一个被访问的对象，都会在Binder驱动中存储一个Binder引用（也就是被访问对象的一个映射），而这个Binder引用就存着被访问对象的aidl的描述（类名，变量，方法等），也就是被访问对象的一个标识
9. AIDL (Android Interface Definition Language) 是一种IDL 语言，用于生成可以在Android设备上两个进程之间进行进程间通信(interprocess communication, IPC)的代码。如果在一个进程中（例如Activity）要调用另一个进程中（例如Service）对象的操作，就可以使用AIDL生成可序列化的参数。[使用教程](https://zhuanlan.zhihu.com/p/31460556)


######总结
**进程A 通过java代码  调用 Binder驱动中的 C代码，Binder驱动 再通过C代码 调用 进程B中的java代码，这样就将进程A和进程B联系起来了，这就是进程间的通信**

#####二、AIDL是怎么和Binder驱动通信的

<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC02
.jpg" width = "600" height = "350">

1. 每个aidl文件都会在gen目录下生成对应的java文件，aidl包含两个部分：Stub（存根）、Proxy（代理）
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC08.jpg" width = "600" height = "350">

2. Stub：相当于服务端，用于被Binder驱动访问。
3. Proxy：相当于客户端，用于访问Binder驱动。
4. 进程A的Proxy（客户端）主动访问Binder驱动（服务端），将数据写到Binder驱动中，然后再由Binder驱动（客户端）来访问进程B的Stub（服务端），这就实现了进程A对进程B的访问。反之亦然。
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC04.jpg" width = "600" height = "350">
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC05.jpg" width = "600" height = "350">
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC06.jpg" width = "600" height = "350">
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC07.jpg" width = "600" height = "350">

#####三、IPC 的应用
1. 四大组件 的运行，生命周期都是由IPC进行实现的。比如启动一个应用，就是由一个进程打开另一个进程

2. 系统服务     系统服务和各个应用不在同一个进程中   this.getSystemService(Context.XXXService) 
<img src ="https://github.com/langsun/Learn/blob/master/zimage/ipc/IPC03.jpg" width = "600" height = "350">