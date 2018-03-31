title:  Activity启动流程
date: 2018年3月27日08:02:11
categories: Activity
tags: 
	 - Android
	 - Activity
cover_picture: /images/common.png
---

> https://www.jianshu.com/p/9ecea420eb52

![Activity启动流程](https://upload-images.jianshu.io/upload_images/1869462-882b8e0470adf85a.jpg)

### 一切从main()开始

Android中，一个应用程序的开始从ActivityThread.java中的main()开始。

1. 配置程序运行环境UserEnvironment,日志等。
2. 准备当前线程的Looper为程序的MainLooper。
3. 初始化ActivityThread实例
4. ActivityThread实例调用attach(false)，创建Application
5. 获取MainThreadHandler
6. MainLooper开始loop(),接收发送消息。程序开始运行。

### 创建Application的消息

ActivityThread实例调用attach(false)，创建Application

1. 判断是否系统程序，不同的初始化流程(分析非系统)

2. 获得ActivityManager实例——ActivityManagerService，ActivityManagerNative.getDefault()获得代理类**ActivityManagerProxy**，通过ServiceManager获得IBinder实例。获取IBInder目的既是为了通过这个IBinder和ActivityManager进行通讯。

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
   }
   ```

   ```java
   public abstract class ApplicationThreadNative extends Binder
           implements IApplicationThread {
           }
   ```

   ```java
   public interface IApplicationThread extends IInterface {
       String descriptor = "android.app.IApplicationThread";
   }
   ```

   ​

   - ActivityThread 中的常量，不希望中途被修改。

   - ApplicationThread是ActivityThread内部类。

   - ApplicationThreadNative继承Binder实现IApplicationThread。

   - ApplicationThread作为IApplicationThread实例承担了最后发送Activity生命周期、及其它一些任务。ApplicationThread传到ActivityManager中为了让系统根据情况控制。

     ​

   ### ActivityManagerService调度发送初始化消息

   ```java
   public abstract class ActivityManagerNative extends Binder implements IActivityManager{
   }
   ```

   ```java
   public final class ActivityManagerService extends ActivityManagerNative
           implements Watchdog.Monitor, BatteryStatsImpl.BatteryCallback {
   }
   ```

   - 获得ActivityManager实例——ActivityManagerService

     ```java
     final IActivityManager mgr = ActivityManagerNative.getDefault();
                 try {
                     mgr.attachApplication(mAppThread);
                 } catch (RemoteException ex) {
                     throw ex.rethrowFromSystemServer();
                 }
     ```

     实现类ActivityManagerService

     ```java
     @Override
      public final void attachApplication(IApplicationThread thread) {
            synchronized (this) {
                 int callingPid = Binder.getCallingPid();
                 final long origId = Binder.clearCallingIdentity();
                 attachApplicationLocked(thread, callingPid);
                 Binder.restoreCallingIdentity(origId);
             }
         }
     ```

     attachApplicationLocked()

     ```java
     private final boolean attachApplicationLocked(IApplicationThread   		thread,int pid) {
         // 以前存在 pid
         // If the app is being launched for restore or full backup, set it up specially
         ...
          //实现类ApplicationThread 执行bindApplication初始化Application
          thread.bindApplication(...);
       	...
           return true;
        }
     ```

     ```java
      public final void bindApplication(...) {
                 if (services != null) {
                     // Setup the service cache in the ServiceManager
                     ServiceManager.initServiceCache(services);
                 }
                 setCoreSettings(coreSettings);
                 AppBindData data = new AppBindData();
                 ...
                 sendMessage(H.BIND_APPLICATION, data);
             }
     ```

     ```java
     case BIND_APPLICATION:
          AppBindData data = (AppBindData)msg.obj;
     	//重要
          handleBindApplication(data);
          Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
     break;
     ```

     ```java
     private void handleBindApplication(AppBindData data) {
         ...
        //通过反射初始化一个Instrumentation仪表。
         mInstrumentation = (Instrumentation)
             cl.loadClass(data.instrumentationName.getClassName())
             .newInstance();
         ...
         //通过LoadedApp命令创建Application实例
         Application app = data.info.makeApplication(data.restrictedBackupMode, null);
         mInitialApplication = app;
         ...
         mInstrumentation.callApplicationOnCreate(app);
         //让仪器调用Application的onCreate()方法
         ...
     }
     ```


### Instrumentation ?

- Instrumentation会在应用程序的任何代码运行之前被实例化，它能够允许你监视应用程序和系统的所有交互。

- 收集AndroidManifest.xml标签信息

- Apllication的创建，Activity的创建，以及生命周期都会经过这个对象去执行。简单点说，就是把这些操作包装了一层。通过操作Instrumentation进而实现上述的功能。

- ```java
  public void callApplicationOnCreate(Application app) {
          app.onCreate();
      }
  ```

  ​

