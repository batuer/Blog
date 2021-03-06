title:  Java内存
date: 2018年4月13日08:26:26
categories: Java
tags: 

	 - Java
cover_picture: /images/java.png
---

#### Java内存区域

![](https://upload-images.jianshu.io/upload_images/2088926-e9844014cb5433ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 程序计数器：
  - 较小的内存空间。
  - 当前线程锁执行的字节码行号指示器。
  - 下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程回复等基本功能都依赖于计数器。
- 虚拟机栈：
  - 线程私有，**生命周期与线程相同**。
  - 每个方法执行的时候都会同时创建一个栈帧用于存储局部变量表、操作站、动态链接、方法出口等信息。
  - 每个方法调用到执行完成，就想对应着一个栈帧在虚拟机中从入栈到出栈的过程。
  - 如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError。
  - 如果虚拟机栈扩展无法申请足够内存时会抛出OutOfMemoryError。
- 本地方法栈：
  - 与虚拟机栈作用相似。
  - 区别在于虚拟机栈为虚拟机执行Java方法服务，而本地方法栈为Native方法服务。
- 堆：
  - Java虚拟机中所管理内存中最大的一块。
  - 所有线程共享，虚拟机启动时创建。
  - 存放对象实例。
  - Java堆有划分成好几个区域。
  - 内存不够时会抛出OutOfMemoryError。
- 方法区：
  - 线程共享。
  - 包含所有class和static变量。
  - 运行时常量池都分配在Java虚拟机的方法区之中。

#### Java内存模型(Java Memory Model,JMM)

- 一种抽象概念，并不真实存在，描述的是一组规范。定义程序中各个变量的访问方式。
- JMM可以屏蔽各种硬件和操作系统的访问差异，实现Java程序在各种平台下能达到一致的内存访问效果。
- 由于JVM(Java Virtual Machine)运行程序的实体是线程，每个线程创建时JVM都会为其创建一个栈空间，用于存储线程私有数据。
- Java内存模型规定所有变量都存储于在主内存中，主内存是共享区域，线程对变量的操作必须在工作内存中进行。
  - 主内存：所有线程创建的实例对象都存放在主内存中，不论是成员变量还是局部变量，由于是共享区域，所以涉及线程安全问题。
  - 工作内存：存储当前方法的所有变量信息（主内存中的副本），线程间不可互相访问工作内存。
  - 主内存的实例对象可以被多线程共享，拷贝副本到自己线程进行操作，执行操作完成刷新到主内存中。
- JMM定义了一组规则，通过这组规则来决定一个线程对共享变量的写入何时对另一个线程可见，这组规则也称为Java内存模型(即JMM)，JMM是围绕着程序执行的原子性。有序性、可见性展开的。
  - 原子性：一个操作不可终端，即使在多线程环境下，一个操作不会受其他线程影响。
  - 有序性(指令重排)：为了提高性能编译器和处理器常常会指令做重排。
    - 编译器优化重排：重新安排语句的执行顺序并不严格按照代码顺序执行。
    - 处理器重排：内存系统的重排，缓存的读写同步。
  - 可见性：当某一线程修改了某个共享变量的值，其他线程能否马上得知这个修改的值。
- JM们的解决方案：
  - 基本数据类型读写操作具有原子性。
  - Synchronized关键字和重入锁(ReentrantLock)保证**程序执行**的原子性。
  - volatile关键字
    - 精致重拍优化。
    - 某一线程修改了volatile修饰的关键字，其他线程会马上得知修改值。
- JMM中的happens-before原则：
  - 程序顺序原则：一个线程内，代码顺序执行。
  - 传递性。
  - 锁规则：解锁操作必然在后续的同一个锁的加锁之前，有对应加锁解锁。
  - volatile规则：volatile修改前必然先读主内存。
  - 线程启动规则：线程的start()方法先于它的每一个动作。
  - 线程终端规则：对线程 interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测线程是否中断。
  - 线程终止规则：线程的所有操作先于线程的终结。
  - 对象终结规则：对象的构造函数执行，结束先于finalize()方法。
- JMM就是一组规则，这组规则意在解决在并发编程可能出现的线程安全问题，并提供了内置解决方案（happen-before原则）及其外部可使用的同步手段(synchronized/volatile等)，确保了程序执行在多线程环境中的应有的原子性，可视性及其有序性。