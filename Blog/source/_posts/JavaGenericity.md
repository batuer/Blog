title:  Java泛型
date: 2018年2月14日09:13:56
categories: Java
tags: 
	 - Java基础
	 - Java泛型
cover_picture: /images/javaGenericity.jpg
---

### 一、泛型简介

##### 1.引入泛型的目的

了解引入泛型的动机，就先从语法糖开始了解。

**语法糖**

语法糖（Syntactic Sugar），也称糖衣语法，是由英国计算机学家Peter.J.Landin发明的一个术语，指在计算机语言中添加的某种语法，这种语法<u>对语言的功能并没有影响</u>，但是<u>更方便程序员使用</u>。Java中最常用的语法糖主要有泛型、变长参数、条件编译、自动拆装箱、内部类等。虚拟机并不支持这些语法，它们在<u>编译阶段就被还原回了简单的基础语法结构</u>，这个过程成为解语法糖。

**泛型的目的：** Java 泛型就是把一种语法糖，通过泛型使得在编译阶段完成一些类型转换的工作，避免在运行时强制类型转换而出现`ClassCastException`，即类型转换异常。

##### 2.泛型初探

JDK 1.5 时才增加了泛型，并在很大程度上都是方便集合的使用，使其能够记住其元素的数据类型。

##### 3.泛型的好处

①**类型安全**。类型错误现在在编译期间就被捕获到了，而不是在运行时当作java.lang.ClassCastException展示出来，将类型检查从运行时挪到编译时有助于开发者更容易找到错误，并提高程序的可靠性。

②消除了代码中许多的强制类型转换，增强了代码的**可读性**。

③为较大的优化带来了可能。

### 二、泛型的使用

#### 1.泛型类和泛型接口

- 这就是**泛型的实质：允许在定义接口、类时声明类型形参，类型形参在整个接口、类体内可当成类型使用，几乎所有可使用普通类型的地方都可以使用这种类型形参。**
- 在JDK 1.7 增加了泛型的“菱形”语法：**Java允许在构造器后不需要带完成的泛型信息，只要给出一对尖括号（<>）即可，Java可以推断尖括号里应该是什么泛型信息。**
- 当创建了带泛型声明的接口、父类之后，可以为该接口创建实现类，或者从该父类派生子类，需要注意：**使用这些接口、父类派生子类时不能再包含类型形参，需要传入具体的类型。**

#### 2.泛型的方法

- 泛型类和泛型接口中提到，可以在泛型类、泛型接口的方法中，把泛型中声明的类型形参当成普通类型使用。
- **所谓泛型方法，就是在声明方法时定义一个或多个类型形参。**

#### 3.泛型构造器

正如泛型方法允许在方法签名中声明类型形参一样，Java也允许在构造器签名中声明类型形参，这样就产生了所谓的泛型构造器。  
和使用普通泛型方法一样没区别，一种是显式指定泛型参数，另一种是隐式推断，如果是显式指定则以显式指定的类型参数为准，如果传入的参数的类型和指定的类型实参不符，将会编译报错。

### 三、类型通配符

顾名思义就是匹配任意类型的类型实参。

类型通配符是一个问号（？\)，将一个问号作为类型实参传给List集合，写作：`List<?>`（意思是元素类型未知的List）。这个问号（？）被成为通配符，它的元素类型可以匹配任何类型。

#### 带限通配符

简单来讲，使用通配符的目的是来限制泛型的类型参数的类型，使其满足某种条件，固定为某些类。

主要分为两类即：**上限通配符**和**下限通配符**。

##### 1.上限通配符

如果想限制使用泛型类别时，只能用某个特定类型或者是其子类型才能实例化该类型时，可以在定义类型时，**使用extends关键字指定这个类型必须是继承某个类，或者实现某个接口，也可以是这个类或接口本身。**

##### 2.下限通配符

如果想限制使用泛型类别时，只能用某个特定类型或者是其父类型才能实例化该类型时，可以在定义类型时，**使用super关键字指定这个类型必须是是某个类的父类，或者是某个接口的父接口，也可以是这个类或接口本身。**

### 四、类型擦除

- 不管为泛型的类型形参传入哪一种类型实参，对于Java来说，它们依然被当成同一类处理，在内存中也只占用一块内存空间。从Java泛型这一概念提出的目的来看，其只是作用于代码**编译**阶段，在编译过程中，对于正确检验泛型结果后，会将泛型的相关信息擦出，也就是说，成功编译过后的class文件中是不包含任何泛型信息的。泛型信息不会进入到运行时阶段。
- **在静态方法、静态初始化块或者静态变量的声明和初始化中不允许使用类型形参。由于系统中并不会真正生成泛型类，所以instanceof运算符后不能使用泛型类。**

