title:  Activity启动流程
date: 2018年3月27日08:02:11
categories: Activity
tags: 

```
 - Android
 - Activity
```

## cover_picture: /images/common.png

> https://www.jianshu.com/p/9ecea420eb52

![Activity启动流程](https://upload-images.jianshu.io/upload_images/1869462-882b8e0470adf85a.jpg)

### ActivityThread.java

Android中，一个应用程序的开始从ActivityThread.java中的main()开始。

1. 配置程序运行环境UserEnvironment,日志等。

2. 准备当前线程的Looper为程序的MainLooper。

3. 初始化ActivityThread实例

4. ActivityThread实例调用attach()

   发送一条创建Application的消息——H.BIND_APPLICATION。

   1. 判断是否系统程序，不同的初始化流程(分析非系统)

   2. 获得ActivityManager实例，ActivityManagerNative.getDefault()获得代理类**ActivityManagerProxy**，通过ServiceManager获得IBinder实例。获取IBInder目的既是为了通过这个IBinder和ActivityManager进行通讯。

      ```java
      IBinder b = ServiceManager.getService("activity");
      if (false) {
          Log.v("ActivityManager", "default service binder = " + b);
      }
      IActivityManager am = asInterface(b);
      ```

      ```java
      static public IActivityManager asInterface(IBinder obj) {
              if (obj == null) {
                  return null;
              }
              IActivityManager in =
                  (IActivityManager)obj.queryLocalInterface(descriptor);
              if (in != null) {
                  return in;
              }

              return new ActivityManagerProxy(obj);
          }
      ```

   3. ActivityManagerProxy.attachApplication(mAppThread)

      ```java
       Parcel data = Parcel.obtain();
              Parcel reply = Parcel.obtain();
              data.writeInterfaceToken(IActivityManager.descriptor);
              data.writeStrongBinder(app.asBinder());
              mRemote.transact(ATTACH_APPLICATION_TRANSACTION, data,reply,0);
              reply.readException();
              data.recycle();
              reply.recycle();
      ```

      - 调用IBInder实例的tansact()方法，把参数app放到data中，最终传递给ActivityManager。
      - IActvitymanager的实现类ActivityManagerProxy。

   4. ApplicationThread  mAppThread ？

      ```java
      private class ApplicationThread extends ApplicationThreadNative {
      ```

      ```java
      public abstract class ApplicationThreadNative extends Binder
              implements IApplicationThread {
      ```

      ```java
      public interface IApplicationThread extends IInterface {
          String descriptor = "android.app.IApplicationThread";
      ```

      ​

      - ActivityThread 中的常量，不希望中途被修改。
      - ApplicationThread是ActivityThread内部类。
      - ApplicationThreadNative继承Binder实现IApplicationThread。
      - ApplicationThread作为IApplicationThread实例承担了最后发送Activity生命周期、及其它一些任务。ApplicationThread传到ActivityManager中为了让系统根据情况控制。

5. 获取MainThreadHandler

6. MainLooper开始loop(),接收发送消息.