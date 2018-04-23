title:  Android性能优化
date: 2018年4月23日08:09:57
categories: Android
tags: 

	 - Android
	 - 性能优化
cover_picture: /images/OptimizingPerformance.jpg
---

### 性能优化

- 流畅(卡顿优化)
- 内存优化
- 耗电优化
- 安装包大小优化

#### 卡顿优化

![](https://upload-images.jianshu.io/upload_images/2088926-be2fac616c7b93b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 原因
  - 绘制任务太重，绘制一帧内容耗时太长。
  - 主线程太忙，根据系统传递过来的 VSYNC 信号来时还没准备好数据导致丢帧。
- 优化
  - 界面绘制卡顿
    - 布局优化：布局嵌套过深、使用合适布局。列表控件缓存复用、include、merge、ViewStub、移除Activity默认背景。
  - 数据处理阻塞UI线程。
    - 获取数去耗时，占用UI线程。
    - 处理数据占用CPU高，UI线程拿不到时间片。
  - 内存占用过大
    - 具体见内存优化。
  - 启动优化
    - 绘制优化。
    - 启动加载逻辑优化。
    - 可延迟初始化项，延迟初始化。
  - 动画优化
    - 可以使用硬件加速，提高流畅度。
- 工具
  - System Trace（收集和检测时间信息，CPU的消耗）

  - Hierarchy Viewer（布局层级、绘制）

  - TraceView（定位代码的执行时间）

  - AndroidStudio3.0 自带的 [Profiler](https://developer.android.google.cn/studio/releases/index.html#3-0-0)
  
#### 内存优化

- ​

