title:  Android面试
date: 22018年2月19日22:21:30
categories: Android
tags: 
	 - Android面试
cover_picture: /images/AndroidInterivew.jpg
---

## 一、Java

####  Java基础

1. 对封装、抽象、继承、多态的理解

   > - 封装：面向对象重要原则，把过程和数据包围起来，数据的访问通过自定义接口，隐藏内部实现细节。增加安全性。
   > - 抽象：同一事物共有属性和方法的集合，多态的基础。
   > - 继承：代码复用的重要手段。单继承特点，只有一个父类，继承父类非私有属性和方法。根据自身需求扩展。
   > - 多态：同一种行为具有不同的表现形态或形式的能力。程序中定义的引用变量在编译时期不能确定具体类型，在运行中才能知道具体类型。

2. 泛型的作用及使用场景

   > 作用：编译阶段完成类型转换，避免运行时期转换异常。类型安全。
   >
   > 场景：
   >
   > - 泛型类和接口
   > - 泛型方法
   > - 泛型构造器
   > - 类型通配符，上限和下限通配符。

3. 枚举的特点及使用场景

   > 特点：
   >
   > - 枚举的直接父类是java.lang.Enum，但是不能显示的继承Enum。
   > - 枚举就相当于一个类，可以定义构造方法、成员变量、普通方法和抽象方法。
   > - 每个实例分别用于一个全局常量表示，枚举类型的对象是固定的，实例个数有限，不能使用new关键字。 
   > - 枚举实例后有花括号时，该实例是枚举的**匿名内部类对象**。
   >
   > 场景：
   >
   > - 普通常量。
   > - 枚举中添加变量、构造函数，灵活获取指定值。
   > - 添加自己特定的方法，实现自己的需求。根据code获取相应的对应值。（常见于原因Code获取具体原因描述）

4. 线程sleep和wait的区别

   > - sleep（）属于Thread，wait（）属于Object.。
   > - sleep（）仅仅是睡眠，不涉及到锁的释放问题，让出CPU，睡眠时间结束自动竞争CPU执行。 wait（）绑定了某个对象的锁，等待该对象的notify（），notifyAll（）来唤醒自己，等待的时间是未知的，甚至出现死锁。
   > - 无论怎用sleep都会释放cpu，但是在线程池中会占用位置。（CPU和线程区别）。

5. JAVA反射机制

   Java反射机制是在**运行**状态中，对于任意一个类，都能够知道这个类中的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种**动态获取的信息以及动态调用对象的方法**的功能称为java语言的反射机制。

   > - 在运行时判断任意一个对象所属的类。
   > - 在运行时构造任意一个类的对象。
   > - 在运行时判断任意一个类所具有的成员变量和方法。
   > - 在运行时调用任意一个对象的方法。
   > - 生成**动态代理**。

6. weak/soft/strong引用的区别

   > - JAVA虚拟机通过**可达性**（Reachability)来判断对象是否存活，基本思想：以"GC Roots"的对象作为起始点向下搜索，搜索形成的路径称为引用链，当一个对象到GC Roots没有任何引用链相连（即不可达的），则该对象被判定为可以被回收的对象，反之不能被回收。
   > - Strong：普通的java引用，我们通常new的对象就是： `StringBuffer buffer = new StringBuffer();` 如果一个对象通过一串强引用链可达，那么它就不会被垃圾回收。
   > - Soft：当内存不足的时候才回收它。
   > - Weak：一旦gc发现对象是weakReference可达，就会把它放到ReferenceQueue中，下次gc时回收它。
   > - Phantom：和soft，weak Reference区别较大，它的get()方法总是返回null。

7. Object的hashCode()与equals()的区别和作用
   > - equals() 的作用是 用来判断两个对象是否相等。
   > - hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个int整数。这个哈希码的作用是确定该对象在哈希表中的**索引位置**。
   > - hashCode是为了提高在散列结构存储中查找的效率，在线性表中没有作用。

8. interface和abstract class区别

   > - Java抽象类定义的两种机制，这两张机制赋予了Java强大的面向对象能力。两者有很大相似性，也可以相互替换，但两者之间也有很大区别。
   > - 抽象类：在代码中使用abstract修饰的class即为抽象类，**类对象的抽象集合。**具体的使用中主要用来进行类型隐藏，我们可以构造出一组固定的行为，这组行为却能够有任意个可能的具体实现，这个抽象描述就是**抽象类**，这一组任意个可能的具体实现则表现为泽类。这样模块可以操作一个抽象提，由于模块依赖于一个固定的抽象提，一次它可以使不允许修改的，但是允许扩展，这就是面向对象设计的一个核心原则OCP，抽象是关键所在。
   > - 接口：比abstract class更加抽象，是一种特殊的abstract class。用Interface关键字修饰，**类方法的抽象集合。**为了把程序模块进行固话契约，降低耦合。

   ![Markdown](http://i2.bvimg.com/635133/f502ff864834f9d1.png)


#### 集合类

1. JAVA常用集合类功能、区别和性能

   - Java的集合类主要由两个接口派生而出：Collection和Map,Collection和Map是Java集合框架的根接口。

     ![](http://upload-images.jianshu.io/upload_images/3985563-e7febf364d8d8235.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

     ​

     图中，ArrayList,HashSet,LinkedList,TreeSet是我们经常会有用到的已实现的集合类。

     1. Collection: 最基本的集合类型，实现**Iterable**接口。
        1. List: **有序，可重复**。
           1. LinkedList: 双向链表实现，可被用作堆栈、队列、双向队列，set和get函数以O(n/2)的性能获取一个节点。
           2. ArrayList: 数组实现，自动扩容。
           3. Vector: 数组实现，自动扩容，同步存取。
           4. Stack: 继承Vector,实现后进先出堆栈，提供5个额外方法使得Vector当堆栈使用。
        2. Set: **不可重复**。
           1. HashSet: 代用对象hashCode，计算存放位置,通过hashCode 和equals判断重复。(HashMap的Key的判断,无序)
           2. TreeSet: 排序。

   - Map实现类用于保存具有映射关系的数据。Map保存的每项数据都是key-value对，也就是由key和value两个值组成。Map里的key是不可重复的，key用户标识集合里的每项数据。

     ![](http://upload-images.jianshu.io/upload_images/3985563-06052107849a7603.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

     图中，HashMap和TreeMap经常用到。

     1. Map: Key和Value 的映射，不包含相同的Key。
        1. HashTable: 同步，存取非空对象。
        2. HashMap: 不同步，允许空对象，不保证有序，存储Entrty。
        3. Treemap: 类似HashMap，实现排序。
        4. LinkedHashMap: Hash表和链表的实现，类似HashMap，保证双向链表节点的顺序。
        5. WeakHashMap: key弱应用。
        6. ArrayMap: 没用Entrty，由两数组维护。速度慢于HashMap，有排序，二分法查找，数组收缩功能，时间换空间。

2. 并发相关的集合类 

3. 部分常用集合类的内部实现方式

#### 多线程相关

1. Thread、Runnable、Callable、Futrue类关系与区别
2. JDK中默认提供了哪些线程池，有何区别
3. 线程同步有几种方式，分别阐述在项目中的用法
4. 在理解默认线程池的前提下，自己实现线程池

#### 字符

1. String的不可变性
2. StringBuilder和StringBuffer的区别
3. 字符集的理解：Unicode、UTF-8、GB2312等 
4. 正则表达式相关问题

#### 注解

1. 注解的使用 
2. 注解的级别及意义  
3. 如何自定义注解


## 二、Android技术

#### Android基础

1. 四大组件的意义及使用，生命周期回调及意义
2. AsyncTask、Handler的使用
3. Android系统层次框架结构
4. AsyncTask的实现方式
5. AsyncTask使用的时候应该注意什么
6. Android常见的存储方式
7. Looper、Handler和MessageQueue的关系  
8. Activity的启动流程（考察对Framwork的熟悉程度）
9. 多进程开发的注意事项(Application类区分进程，进程间内存不可见、进程间通讯方式)