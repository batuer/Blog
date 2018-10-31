##### 策略模式

###### 场景

- 一类型问题，多种处理方式。

###### 特点

- 分离、封装算法，相互替换。
- OCP原则，单一原则。
- 算法独立于客户端独立变化。
- 策略由Context决定。

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

