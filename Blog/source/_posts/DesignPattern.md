##### 策略模式

###### 定义

- 分离、封装算法，相互替换。
- OCP原则，单一原则。
- 算法独立于客户端独立变化。
- 策略由客户端决定。

###### 场景

- 一类型问题，多种处理方式。

###### 优点

- 结构清晰，简单直观。
- 耦合度低，可扩展性好。

###### 缺点

- 策略增加，子类相应增多。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-0265e6be332b6305.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```Java
//Context
public class Context {
    private IStragety mIStragety;

    public void setIStragety(IStragety IStragety) {
        mIStragety = IStragety;
    }

    public void doSth() {
    }
}
//Stragety
public interface IStragety {
    void doSth();
}
//Stragety impl
public class AStragety implements IStragety {
    @Override
    public void doSth() {
        System.out.println("A doSth");
    }
}
```

###### 策略模式与工厂模式的却别

|            策略模式            |          工厂模式          |
| :----------------------------: | :------------------------: |
|         创建型设计模式         |       行为型设计模式       |
|          关注对象创建          |        关注行为选择        |
| 黑盒子（不知道具体的实现过程） | 白盒子（知道具体实现过程） |

##### 状态模式

###### 定义

- 对象的内在状态改变时允许改变其行为。

###### 场景

- 对象的行为取决于**对象的状态**。
- 不同状态对同一行为不同的响应。

###### 优点

- 特定状态相关的行为放入同一个状态对象中。
- 结构清晰，可扩展性、可维护性高。

###### 缺点

- 增加系统类和对象的个数。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-508f9cd9d5c47eb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```Java
public interface UserState {
    /**
     * 转发
     */
    void forward(Context context);

    /**
     * 评论
     * @param context
     */
    void comment(Context context);
}

public class LoginState implements UserState {
    @Override
    public void forward(Context context) {
        Toast.makeText(context, "转发微博", Toast.LENGTH_SHORT)
                .show();
    }

    @Override
    public void comment(Context context) {
        Toast.makeText(context, "评论微博", Toast.LENGTH_SHORT)
                .show();
    }
}

public class LogoutState implements UserState {
    @Override
    public void forward(Context context) {
        gotoLogin(context);
    }

    @Override
    public void comment(Context context) {
        gotoLogin(context);
    }

    private void gotoLogin(Context context) {
        context.startActivity(new Intent(context, LoginActivity.class));
    }
}

public class LoginContext {
    public static LoginContext getInstance() {
        return LoginContextHolder.loginContext;
    }

    private static final class LoginContextHolder {
        private static final LoginContext loginContext = new LoginContext();
    }

    private LoginContext() {

    }

    UserState mState = new LogoutState();

    public void setState(UserState state) {
        mState = state;
    }

    public void forward(Context context) {
        mState.forward(context);
    }

    public void comment(Context context) {
        mState.comment(context);
    }
}

```

###### 策略模式、状态模式、模板模式对比

|      |                        策略模式                        |                           状态模式                           |                           模板模式                           |
| :--: | :----------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 定义 | 定义封装算法，可以相互替换，算法独立于客户端独立变化。 | 对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类 。 | 定义算法框架，一些步骤延迟到子类中，使得算法结构不变化可以重定义算法。 |
| 场景 |                一类型问题，多种处理方式                |             不同的状态对不同的行为有不同的响应。             |        一次性实现算法框架不变的部分，子类可改变行为。        |
| 动机 |                        算法分离                        |                         状态改变行为                         |                         多种输出模板                         |

- 驱动行为的改变
  - 状态模式：状态的变化是由Context或State自己管理。
  - 策略模式：由客户端提供策略。
  - 模板模式：具体子类自己决定是否变化。

###### 模板模式

###### 定义

- 一个算法框架，一些步骤延迟到子类中，使得子类可以不改变算法的结构重实现特定步骤。
- 关键步骤、执行顺序、具体步骤实现会随环境变化。

###### 场景

- 多个子类有公有的方法并且逻辑基本相同，有一定的执行顺序，具体步骤实现会随环境变化。

###### 优点

- 封装不变部分，扩展可变部分。
- 提取公共部分，便于维护。

###### 缺点

- 阅读理解难度

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-f0904b58ed87d8dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```Java
public abstract class AbsComputer {
    protected void powerOn() {
        System.out.println("开机");
    }

    protected void checkHardWare() {
        System.out.println("检查硬件");
    }

    protected void loadOs() {
        System.out.println("载入系统");
    }

    protected void login() {
        System.out.println("登录");
    }

    public final void startUp() {
        System.out.println("开机 Start");
        powerOn();
        checkHardWare();
        loadOs();
        login();
        System.out.println("开机 End");
    }

}

public class NormalComputer extends AbsComputer {
    @Override
    protected void login() {
        super.login();
        System.out.println("普通电脑账户密码校验");
    }
}

public class MilitaryComputer extends AbsComputer {
    @Override
    protected void checkHardWare() {
        super.checkHardWare();
        System.out.println("特殊检查防火墙");
    }

    @Override
    protected void login() {
        super.login();
        System.out.println("指纹人脸特殊验证");
    }
}

```

##### 单例模式

###### 定义

- 一个类只有一个实例（多线程下、反序列化不重建）。
- 构造函数private。

###### 场景

- new对象消耗过多的资源。

###### 优点

- 只有一个实例，减少内存开支。
- 频繁创建销毁时，减少性能开销。
- 优化共享资源的访问。

###### 缺点

- 没有接口，扩展困难。
- 内存泄漏隐患。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-f61be7c9c87a720c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```java
public class Singleton1 {
    //volatile 顺序
    private static volatile Singleton1 sSingleton1;

    private Singleton1() {
        //no instance
    }

    public static Singleton1 getInstance() {
        if (sSingleton1 == null) {
            synchronized (Singleton1.class) {
                if (sSingleton1 == null) {
                    sSingleton1 = new Singleton1();
                }
            }
        }
        return sSingleton1;
    }
    
}
//推荐 第一次调用加载
public class Singleton2 {
    private Singleton2() {
        //no instance
    }

    public static Singleton2 getInstance() {
        return Holder.single;
    }

    private static final class Holder {
        private static final Singleton2 single = new Singleton2();
    }
}

public enum Singleton3 {
    //线程安全
    SINGLETON_3;

    public void doSth() {
    }
}


 //反序列化
    private Object readResolve() throws ObjectStreamException {
        return Holder.single;
    }
```

##### Builder模式

###### 定义

- 复杂对象的构建与它的表示分离。

###### 场景

- 相同的方法，不同的执行顺序，产生不同的事件结果。
- 复杂对象的构建，参数多，且参数可以有默认值。

###### 优点

- 良好的封装性，客户端不必知道内部组成的细节。
- 建造者独立，扩展性强。

###### 缺点

- 多余的Builder对象以及Director对象，消耗内存。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-a5adf8172956079b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```java
public class Computer {
    private String mBoard;
    private String mDisplay;
    private String mOS;

    private Computer(Builder builder) {
        mBoard = builder.mBoard;
        mDisplay = builder.mDisplay;
        mOS = builder.mOS;
    }

    public static final class Builder {
        private String mBoard;
        private String mDisplay;
        private String mOS;

        public Builder() {
        }

        public Builder mBoard(String val) {
            mBoard = val;
            return this;
        }

        public Builder mDisplay(String val) {
            mDisplay = val;
            return this;
        }

        public Builder mOS(String val) {
            mOS = val;
            return this;
        }

        public Computer build() {
            return new Computer(this);
        }
    }
}
```

##### 原型模式

###### 定义

- Clone创建新的对象。

###### 场景

- new对象消耗资源。
- new对象需要繁琐的准备或访问权限。

###### 优点

- 内存中二进制流的拷贝，比new对象性能好。

###### 缺点

- 内存中拷贝，**不执行构造函数**。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-584799dd2d9e0c85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```java
public class User implements Cloneable {
    public int age;
    public String name;
    public String phoneNum;
    public Address address;//深拷贝（浅拷贝数据修改对应同一堆内存地址）

    @Override
    protected User clone() {
        User clone = null;
        try {
            clone = (User) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}

public class Address {
    public String city;
    public String district;
    public String street;

    @Override
    protected Address clone() {
        Address clone = null;
        try {
            clone = (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }
}
```

##### 工厂方法模式

###### 定义

- 定义一个用于创建对象的接口，子类决定实例化那个类。

###### 场景

- 任何需要生成复杂对象的地方。
- new就可以创建的对象无需使用工厂模式。

###### 优点

- 生成复杂对象方便快捷。

###### 缺点

- 添加新产品时繁琐，复杂化。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-cbe18d7b56a2df3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```java
//产品类
public abstract class IOHandler {
  /**
   * add
   */
  public abstract void add(String id, String name);

  /**
   * remove
   */
  public abstract void remove(String id, String name);

  /**
   * update
   */
  public abstract void update(String id, String name);

  /**
   * query
   */
  public abstract void query(String id, String name);
}  

public class IOFactory {
    public static <T extends IOHandler> T getIOHandler(Class<T> clz) {
        IOHandler handler = null;
        try {
            handler = (IOHandler) Class.forName(clz.getName())
                    .newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return (T) handler;
    }
}  

public class DBHandler extends IOHandler {
    @Override
    public void add(String id, String name) { }

    @Override
    public void remove(String id, String name) { }

    @Override
    public void update(String id, String name) { }

    @Override
    public void query(String id, String name) { }
}
```

######  抽象工厂模式

###### 定义

- 创建一组相关或是相互依赖的对象提供一个接口，不需要指定具体的类。（产品是抽象的）

###### 场景

- 不同抽象产品在不同环境下同一表现行为。

###### 优点

- 分离接口与实现。
- 客户端不关系具体产品的实现（面向接口）。

###### 缺点

- 类文件增加。
- 难以扩展新产品。

###### UML

![](https://upload-images.jianshu.io/upload_images/2088926-261503ef95c23c14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 代码

```java
/**
 * 抽象工厂
 */
public abstract class CarFactory {
    public abstract ITire createTire();
}

/**
 * 具体工厂
 */
public class Q3Car extends CarFactory {
    @Override
    public ITire createTire() {
        return new NormalTire();
    }
}

/**
 * 抽象产品
 */
public interface ITire {
  void tire();
}  

/**
 * 具体产品
 */
public class NormalTire implements ITire {
    @Override
    public void tire() {
        System.out.println("normal createTire");
    }
}
```

######  责任链模式

###### 定义

###### 场景

###### 优点

###### 缺点

###### UML

###### 代码