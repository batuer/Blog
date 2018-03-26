title:  Android FrameWork
date: 2018年3月21日08:42:08
categories: FrameWork
tags: 

	 - Android
	 - FrameWork
cover_picture: /images/AndroidFramWork.jpg
---

### Android 的四个层次

#### Android应用层

- 用Java、Kotlin或其它语言编写的运行在虚拟机上的程序。eg：Google原生的短信、浏览器或自己开发的程序。

#### FrameWork（框架层）

- Google核心应用是所使用的API框架，开发人员也可以使用用于开发自己的程序，但必须遵守框架的开发原则。
- 包含：丰富而又可扩展的视图层各种View
  - ContentProvider：多进程间数据哭数据共享
  - ResourceManager：提供资源的访问，包括字符串、图形、布局文件等。
  - NotifyManager：消息通知
  - ActivityManager：应用程序生命周期管理
  - WindowManager：窗口管理
  - ...

#### 系统运行层

- 这一层通过一些c/c++库位Android系统提供了支持。eg：Sqlite数据库、OpenGl|ES绘图的支持、Webkit浏览器内核的支持。
- 这一层还有Android运行核心库。每一个Android应用程序都在自己的进程中进行，拥有独立Dalvik虚拟机。Dalvik虚拟一依赖于LInux内核的一些功能，如底层内存管理机制和线程机制。

#### LInux内核

- 驱动层
- 进程通信的Binder
- 电源管理