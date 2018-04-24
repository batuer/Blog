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


#### Binder跨进程通信机制、模型

##### 模型原理

Binder跨进程通信机制基于Client -Server 模式

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
     1. Client进程通过系统调用copy_from_user()发送数据到内核空间中的缓存区；（当前线程被挂起）
     2. 由于内核缓存







[^1]: 通过文件共享、Socket、Messenger（AIDL一种）、ContentProvider、AIDL。