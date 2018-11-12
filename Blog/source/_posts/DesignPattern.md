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

![](https://upload-images.jianshu.io/upload_images/2088926-aa1a0fbbd376a078.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

![](https://upload-images.jianshu.io/upload_images/2088926-750ea9795e10abc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

![](https://upload-images.jianshu.io/upload_images/2088926-1edb211471ed96ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

###### 场景

###### 优点

###### 缺点

###### UML

###### 代码