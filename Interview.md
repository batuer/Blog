#### Interview

##### 2018-05-07   陕西西建大聚慧

1. int、short、long字节

   | 类型   | 字节    |
   | ------ | ------- |
   | int    | 4个字节 |
   | char   | 2个字节 |
   | byte   | 1个字节 |
   | short  | 2个字节 |
   | long   | 8个字节 |
   | float  | 8个字节 |
   | double | 8个字节 |

2. 树、二叉树

3. Fragment初始化数据

4. service保活

5. 锁的使用

6. 线程

7. MVP、MVC优缺点

   - MVC(Mode-View-Controller)：
     - 优点：
       1. 耦合性不高，View层和Model层分离，减少模块之间的影响。
       2. 理解统一，开发维护成本低。
     - 缺点：
       1. View层和Model层相互可知(迪米特法则)。
       2. ​
   - MVP(Model-View-Presenter)：
     - 优点：
       1. 降低耦合度，实现Model和View真正的完全分离，可以修改View而不影响Model。
       2. 职责明显，层次清晰。
       3. 隐藏数据。
       4. Presenter可以复用。
       5. View组件化，提供给上层接口，可以复用View组件。
       6. 代码灵活性。
       7. 利于测试驱动开发。
     - 缺点
       1. Presenter通信协调View、Model，臃肿。
       2. 额外代码。

8. 列表View数据错乱

9. RecyclerView和ListView缓存

##### 2018-04-27   山东新北洋信息

1. 自定义View
2. 事件分发
3. ListView优化
4. 列表View数据错乱
5. 多层级控件View