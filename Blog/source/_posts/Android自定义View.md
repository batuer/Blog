title:  自定义View
date: 2018年5月3日08:34:08
categories: Activity
tags: 

	 - Android
	 - View基础
	 - View Measure
	 - View Layout
	 - View Draw
cover_picture: /images/common.png
---

[学习链接](https://www.jianshu.com/p/146e5cec4863)

#### View基础

##### View分类

1. View，不包含子View，所有View的最基类。
2. ViewGroup，包含子View，也是View的子类。

##### View构造函数

```java
  //代码new
  public MyView(Context context) {
    super(context);
  }

  /**
   * xml inflate
   *
   * @param attrs 属性集，自定义属性也从这里来
   */
  public MyView(Context context, @Nullable AttributeSet attrs) {
    super(context, attrs);
  }

  /**
   * 不会主动调用
   *
   * @param defStyleAttr style属性
   */
  /*
    1.首先获取给定的AttributeSet中的属性值
    2.如果找不到，则去AttributeSet中style（你在写布局文件时定义的style="@style/xxxx"）指定的资源获取
    3.如果找不到，则去defStyleAttr以及defStyleRes中的默认style中获取。
    4.最后去找的是当前theme下的基础值。
   */
  public MyView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
  }

  /**
   * Api >=21
   * @param defStyleRes View有style属性时
   */
  @TargetApi(Build.VERSION_CODES.LOLLIPOP)
  public MyView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
    super(context, attrs, defStyleAttr, defStyleRes);
  }
```

##### View位置(坐标)

![](https://upload-images.jianshu.io/upload_images/2088926-35c44aad5ee72007.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### View  Measure



