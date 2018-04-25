title:  Binder进程通信
date: 2018年4月12日10:17:30
categories: Android
tags: 

	 - Android
	 - Binder
cover_picture: /images/common.png
---

[链接](https://www.jianshu.com/p/4ee3fd07da14 )

#### Binder

- 从机制、模型的角度
  - 定义：Binder是Android中IPC[^1]的一种方式。
  - 作用：Android中跨进程通信。
- 模型的结构、组成
  - 定义：Binder是一种虚拟的物理设备驱动（Binder驱动)。
  - 作用：连接Service进程，Client进程和ServiceManager进程。
- Android代码的实现
  - 定义：Binder是一个类，实现IBinder接口。
  - 作用：Binder机制模型，代码的形式具体实现在Android中。


#### 进程空间

##### 划分

- 一个进程空间划分成用户空间与内核空间，进程内用户与内核隔离。
- 区别
  - 进程间，用户空间数据**不可共享。**
  - 进程间，内核空间数据**可共享。**
- **所有进程共用一个内核空间。**

##### 进程隔离

Linux机制，为了保证安全性与独立性，一个进程不能直接访问另一个进程。

##### IPC

进程间数据交互、通信。

#### Binder跨进程通信机制、模型

##### 模型原理

Binder跨进程通信机制基于Client -Server 模式

![](https://upload-images.jianshu.io/upload_images/2088926-d7e9a9466dec9452.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



##### 模型组成角色

- Client进程（Android客户端）：使用服务的进程。
- Server进程（服务器端）：提供服务的进程。
- ServiceManager进程（类似于路由器）：管理Service注册与查询（将字符形式的Binder名字转化成Client中对该Binder的引用）。
- Binder驱动（持有每个Server进程在内和空间的Binder实体，并提供Client进程Binder实体的引用）：
  - 虚拟设备驱动，是连接Servier、Client和ServiceManager的桥梁。
  - **传递服务进程消息。**
  - **传递进程间需要传递的数据：通过内存映射。**
  - 线程控制：采用Binder线程池，并有Binder驱动自身进行管理。

##### 原理步骤

- 注册服务：ServiceManager拥有Server进程的信息。
  1. Server进程向Binder驱动发起服务注册请求。
  2. Binder驱动将注册请求转发给ServiceManager。
  3. ServiceManager添加该Server进程。
- 获取服务：（Client进程与Server进程已建立连接）
  1. Client向Binder驱动传递要获取服务的名称，获取请求服务。
  2. Binder驱动将请求转发给ServiceManager进程。
  3. ServiceManager通过名称查找需要的Server信息。
  4. 通过Binder驱动将上述服务信息返回给Client进程。
- 使用服务
  1. Binder驱动为跨进程通信做准备——实现内存映射。
     1. Binder驱动创建一块接收缓存区。
     2. 根据ServiceManager进程里的Server信息找到对应的Server进程，实现<u>内核缓存区和Server进程用户空间地址同时映射到**同一个接收缓存区**中</u>。
  2. Client进程将参数数据发送到Server进程。
     1. Client进程通过系统调用copy_from_user()发送数据到内核空间中的缓存区（当前线程被挂起）。
     2. 由于内核缓存与接收进程的用户空间地址存在映射关系（同时映射Binder创建的接收缓存区中）相当于也发送到了Server进程的用户空间地址，即Binder驱动实现了跨进程通信。
     3. Binder驱动通知Server进程执行解包。
  3. Server进程将参数数据发送到Serve进程。
     1. 收到BInder驱动通知后，Server进程从线程池中取出线程，进行数据解包与调用目标方法。
     2. 将最终执行结果写入到自己的共享内存中。
  4. Server进程将目标方法的结果返回给Client进程。
     1. 将最宠执行结果写入映射的用户空间的内存区域中。
     2. 由于内核缓存区与接收进程的用户空间地址存在映射关系(同时映射Binder创建的接收缓存区中)，相当于也发送到了内核缓存区中。
     3. Binder通知Client进程获得返回结果(此时Client进程之前被挂起的线程被重新唤醒)。
     4. Client进程通过系统调用copy_to_user()从内核缓存区接收Server 进程返回的结果。
- 优点：
  - 传输效率高，每次单项通信数据拷贝次数一次，用户空间与内核空间可直接通过共享对象直交互。
  - 为接收进程分配了不确定大小的接收缓存区。


![Binder模型原理说明图](https://upload-images.jianshu.io/upload_images/2088926-86a9acfe520ed71f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 额外说明

1. Client、Server、ServiceManager进程之间的交互都必须通过Binder驱动，并非直接交互。
   1. Client、Server、ServiceManager进程数据进程空间的用户空间，不可直接交互。
   2. Binder驱动属于进程空间的内核空间。
2. Binder驱动与ServiceManager属于Android基础架构( 系统已经实现)，而Client进程和Server进程应用层，需开发者自己实现。
3. Binder请求的线程管理
   - Server进程可能会创建多个线程来处理Binder请求。
   - Binder模型线程管理采用Binder驱动的线程池，并由Binder驱动自身管理，非Server进程管理。
   - 一个进程Binder线程数默认为16。

#### Binder机制在Android具体实现原理

- Binder机制在Android中实现主要靠Binder类，其实现了IBinder接口。

##### 注册服务

- Server进程通过Binder驱动像ServiceManager进程注册服务。
- Server进程创建一个Binder对象。
  1. Binder实体是Server进程在Binder驱动中的存在形式。
  2. Binder实体保存Server和ServiceManager的信息(保存在内核空间中)。
  3. Binder驱动通过内核空间的Binder实体找到用户空间的Server对象。


[^1]: 通过文件共享、Socket、Messenger（AIDL一种）、ContentProvider、AIDL。