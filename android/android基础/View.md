# 1. View概述

参考文献：

- 参考文献一：[超超boy博客](https://www.cnblogs.com/jycboy/p/6219915.html)
- 参考文献二：[guolin博客](https://blog.csdn.net/guolin_blog/article/details/16330267)

![](https://upload-images.jianshu.io/upload_images/2397836-f1f6a200704884a2.png?imageMogr2/auto-orient/strip%7CimageView2/2)

**DecorView**是一个应用窗口的根容器，它本质上是一个**FrameLayout**。DecorView有唯一一个子View，它是一个垂直LinearLayout，包含两个子元素，一个是**TitleView**（ActionBar的容器），另一个是**ContentView**（窗口内容的容器）。关于ContentView，它是一个FrameLayout（android.R.id.content)，我们平常用的**setContentView**就是设置它的子View。上图还表达了每个Activity都与一个Window（具体来说是PhoneWindow）相关联，用户界面则由Window所承载。

## Window

Window即窗口，这个概念在Android Framework中的实现为android.view.Window这个抽象类，这个抽象类是对Android系统中的窗口的抽象。在介绍这个类之前，我们先来看看究竟什么是窗口呢？

实际上，窗口是一个宏观的思想，它是屏幕上用于绘制各种UI元素及响应用户输入事件的一个矩形区域。通常具备以下两个特点：

- 独立绘制，不与其它界面相互影响；
- 不会触发其它界面的输入事件；

在Android系统中，窗口是独占一个Surface实例的显示区域，每个窗口的Surface由WindowManagerService分配。我们可以把Surface看作一块画布，应用可以通过Canvas或OpenGL在其上面作画。画好之后，通过SurfaceFlinger将多块Surface按照特定的顺序（即Z-order）进行混合，而后输出到FrameBuffer中，这样用户界面就得以显示。

android.view.Window这个抽象类可以看做Android中对窗口这一宏观概念所做的约定，而PhoneWindow这个类是Framework为我们提供的Android窗口概念的具体实现。接下来我们先来介绍一下android.view.Window这个抽象类。

这个抽象类包含了三个核心组件：

- WindowManager.LayoutParams: 窗口的布局参数；
- Callback: 窗口的回调接口，通常由Activity实现；
- ViewTree: 窗口所承载的控件树。

下面我们来看一下Android中Window的具体实现（也是唯一实现）——PhoneWindow。

## PhoneWindow

前面我们提到了，PhoneWindow这个类是Framework为我们提供的Android窗口的具体实现。我们平时调用`setContentView()`方法设置Activity的用户界面时，实际上就完成了对所关联的PhoneWindow的ViewTree的设置。我们还可以通过Activity类的`requestWindowFeature()`方法来定制Activity关联PhoneWindow的外观，这个方法实际上做的是把我们所请求的窗口外观特性存储到了PhoneWindow的mFeatures成员中，在窗口绘制阶段生成外观模板时，会根据mFeatures的值绘制特定外观。

## ViewRoot

在介绍View的绘制前，首先我们需要知道是谁负责执行View绘制的整个流程。实际上，View的绘制是由ViewRoot来负责的。每个应用程序窗口的decorView都有一个与之关联的ViewRoot对象，这种关联关系是由WindowManager来维护的。

那么decorView与ViewRoot的关联关系是在什么时候建立的呢？答案是Activity启动时，`ActivityThread.handleResumeActivity()`方法中建立了它们两者的关联关系。

# 2. View绘制

## View绘制的起点

当建立好了decorView与ViewRoot的关联后，ViewRoot类的`requestLayout()`方法会被调用，以完成应用程序用户界面的初次布局。实际被调用的是**ViewRootImpl**类的`requestLayout()`方法，这个方法的源码如下：

```java
`@Override``public` `void` `requestLayout() {``  ``if` `(!mHandlingLayoutInLayoutRequest) {``    ``// 检查发起布局请求的线程是否为主线程 ``    ``checkThread();``    ``mLayoutRequested = ``true``;``    ``scheduleTraversals();``  ``}``}`
```

上面的方法中调用了`scheduleTraversals()`方法来调度一次完成的绘制流程，该方法会向主线程发送一个“遍历”消息，最终会导致ViewRootImpl的performTraversals()方法被调用。下面，我们以`performTraversals()`为起点，来分析View的整个绘制流程。

任。每一个视图的绘制过程都必须经历三个最主要的阶段，即`onMeasure()`、`onLayout()`和`onDraw()`

- **measure**: 判断是否需要重新计算View的大小，需要的话则计算；
- **layout**: 判断是否需要重新计算View的位置，需要的话则计算；
- **draw**: 判断是否需要重新绘制View，需要的话则重绘制。

![](https://upload-images.jianshu.io/upload_images/2397836-19c08de6439514a7.png?imageMogr2/auto-orient/strip%7CimageView2/2)

## onMeasure()

measure是测量的意思，那么`onMeasure()`方法顾名思义就是**用于测量视图的大小**的。View系统的绘制流程会从ViewRoot的`performTraversals()`方法中开始，在其内部调用View的`measure()`方法。

`measure()`方法接收两个参数：

- **widthMeasureSpec**：确定视图宽度的规格和大小
- **heightMeasureSpec**：确定视图高度的规格和大小

**MeasureSpec**的值由**specSize**和**specMode**共同组成的，其中specSize记录的是大小，specMode记录的是规格。

specMode一共有三种类型：

- **EXACTLY**

  表示父视图希望子视图的大小应该是由**specSize**的值来决定的，系统默认会按照这个规则来设置子视图的大小，开发人员当然也可以按照自己的意愿设置成任意的大小。

- **AT_MOST**

  表示子视图**最多只能是specSize中指定的大小**，开发人员应该尽可能小得去设置这个视图，并且保证不会超过specSize大小。系统默认会按照这个规则来设置子视图的大小，开发人员当然也可以按照自己的意愿设置成任意的大小。

- **UNSPECIFIED**

  开发人员可以将视图按照自己的意愿设置成任意的大小，没有任何限制。这

**widthMeasureSpec**和**heightMeasureSpec**这两个值都是由父视图经过计算后传递给子视图的，说明父视图会在一定程度上决定子视图的大小。但是最外层的根视图这两个值从何而来？分析ViewRoot中的源码了，观察`performTraversals()`方法：

```java
childWidthMeasureSpec = getRootMeasureSpec(desiredWindowWidth, lp.width);
childHeightMeasureSpec = getRootMeasureSpec(desiredWindowHeight, lp.height);

```

这里调用了`getRootMeasureSpec()`方法去获取widthMeasureSpec和heightMeasureSpec的值。注意方法中传入的参数，其中lp.width和lp.height在创建ViewGroup实例的时候就被赋值了，它们都等于**MATCH_PARENT**。然后看下`getRootMeasureSpec()`方法中的代码：

```java
private int getRootMeasureSpec(int windowSize, int rootDimension) {
    int measureSpec;
    switch (rootDimension) {
    case ViewGroup.LayoutParams.MATCH_PARENT:
        measureSpec = MeasureSpec.makeMeasureSpec(windowSize, MeasureSpec.EXACTLY);
        break;
    case ViewGroup.LayoutParams.WRAP_CONTENT:
        measureSpec = MeasureSpec.makeMeasureSpec(windowSize, MeasureSpec.AT_MOST);
        break;
    default:
        measureSpec = MeasureSpec.makeMeasureSpec(rootDimension, MeasureSpec.EXACTLY);
        break;
    }
    return measureSpec;
}

```

这里使用了`MeasureSpec.makeMeasureSpec()`方法来组装一个MeasureSpec，当rootDimension参数等于**MATCH_PARENT**的时候，MeasureSpec的specMode就等于**EXACTLY**，当rootDimension等于**WRAP_CONTENT**的时候，MeasureSpec的specMode就等于**AT_MOST**。并且MATCH_PARENT和WRAP_CONTENT时的specSize都是等于windowSize，意味着根视图总是会充满全屏

接下来我们看下View的`measure()`方法里面的代码

```java
public final void measure(int widthMeasureSpec, int heightMeasureSpec) {
    if ((mPrivateFlags & FORCE_LAYOUT) == FORCE_LAYOUT ||
            widthMeasureSpec != mOldWidthMeasureSpec ||
            heightMeasureSpec != mOldHeightMeasureSpec) {
        mPrivateFlags &= ~MEASURED_DIMENSION_SET;
        if (ViewDebug.TRACE_HIERARCHY) {
            ViewDebug.trace(this, ViewDebug.HierarchyTraceType.ON_MEASURE);
        }
        onMeasure(widthMeasureSpec, heightMeasureSpec);
        if ((mPrivateFlags & MEASURED_DIMENSION_SET) != MEASURED_DIMENSION_SET) {
            throw new IllegalStateException("onMeasure() did not set the"
                    + " measured dimension by calling"
                    + " setMeasuredDimension()");
        }
        mPrivateFlags |= LAYOUT_REQUIRED;
    }
    mOldWidthMeasureSpec = widthMeasureSpec;
    mOldHeightMeasureSpec = heightMeasureSpec;
}

```

`measure()`方法是final，无法在子类中重写这个方法，说明Android不允许我们改变View的measure框架。第9行调用了`onMeasure()`方法，即真正测量并设置View大小的地方，默认会调用`getDefaultSize()`方法获取视图的大小。

```java

public static int getDefaultSize(int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec.getMode(measureSpec);
    int specSize = MeasureSpec.getSize(measureSpec);
    switch (specMode) {
    case MeasureSpec.UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec.AT_MOST:
    case MeasureSpec.EXACTLY:
        result = specSize;
        break;
    }
    return result;
```

这里传入的measureSpec是一直从`measure()`方法中传递过来的。然后调用`MeasureSpec.getMode()`方法可以解析出specMode，调用`MeasureSpec.getSize()`方法可以解析出specSize。接下来进行判断，如果specMode等于AT_MOST或EXACTLY就返回specSize，这也是系统默认的行为。之后会在`onMeasure()`方法中调用`setMeasuredDimension()`方法来设定测量出的大小，这样一次measure过程就结束了

一个界面的展示可能会涉及到很多次的measure，因为一个布局中一般都会包含多个子视图，每个视图都需要经历一次measure过程。ViewGroup中定义了一个`measureChildren()`方法来去测量子视图的大小，

```java
protected void measureChildren(int widthMeasureSpec, int heightMeasureSpec) {
    final int size = mChildrenCount;
    final View[] children = mChildren;
    for (int i = 0; i < size; ++i) {
        final View child = children[i];
        if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
        }
    }
}
```

这里首先会去遍历当前布局下的所有子视图，然后逐个调用measureChild()方法来测量相应子视图的大小，

```java
protected void measureChild(View child, int parentWidthMeasureSpec,
        int parentHeightMeasureSpec) {
    final LayoutParams lp = child.getLayoutParams();
    final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight, lp.width);
    final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom, lp.height);
    child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}
```

可以看到，在第4行和第6行分别调用了getChildMeasureSpec()方法来去计算子视图的MeasureSpec，计算的依据就是布局文件中定义的MATCH_PARENT、WRAP_CONTENT等值。然后在第8行调用子视图的measure()方法，并把计算出的MeasureSpec传递进去，之后的流程就和前面所介绍的一样。

`onMeasure()`方法是可以重写的，也就是说，如果你不想使用系统默认的测量方式，可以按照自己的意愿进行定制。

```java
public class MyView extends View {
 
	......
	
	@Override
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		setMeasuredDimension(200, 200);
	}
 
}
```

这样的话就把View默认的测量流程覆盖掉了，不管在布局文件中定义MyView这个视图的大小是多少，最终在界面上显示的大小都将会是200*200。

在setMeasuredDimension()方法调用之后，我们才能使用getMeasuredWidth()和getMeasuredHeight()来获取视图测量出的宽高，以此之前调用这两个方法得到的值都会是0。

视图大小的控制是由**父视图、布局文件、以及视图本身**共同完成的，父视图会提供给子视图参考的大小，而开发人员可以在XML文件中指定视图的大小，然后视图本身会对最终的大小进行拍板。

## onLayout()

measure过程结束后，视图的大小就已经测量好了，接下来就是layout的过程了。这个方法是用于给视图进行布局的，也就是确定视图的位置。ViewRoot的`performTraversals()`方法会在measure结束后继续执行，调用View的`layout()`方法来执行此过程。

```java
host.layout(0, 0, host.mMeasuredWidth, host.mMeasuredHeight);
```

layout()方法接收四个参数，分别代表着左、上、右、下的坐标，当然这个坐标是相对于当前视图的父视图而言的。可以看到，这里还把刚才测量出的宽度和高度传到了layout()方法中。

```java
public void layout(int l, int t, int r, int b) {
    int oldL = mLeft;
    int oldT = mTop;
    int oldB = mBottom;
    int oldR = mRight;
    boolean changed = setFrame(l, t, r, b);
    if (changed || (mPrivateFlags & LAYOUT_REQUIRED) == LAYOUT_REQUIRED) {
        if (ViewDebug.TRACE_HIERARCHY) {
            ViewDebug.trace(this, ViewDebug.HierarchyTraceType.ON_LAYOUT);
        }
        onLayout(changed, l, t, r, b);
        mPrivateFlags &= ~LAYOUT_REQUIRED;
        if (mOnLayoutChangeListeners != null) {
            ArrayList<OnLayoutChangeListener> listenersCopy =
                    (ArrayList<OnLayoutChangeListener>) mOnLayoutChangeListeners.clone();
            int numListeners = listenersCopy.size();
            for (int i = 0; i < numListeners; ++i) {
                listenersCopy.get(i).onLayoutChange(this, l, t, r, b, oldL, oldT, oldR, oldB);
            }
        }
    }
    mPrivateFlags &= ~FORCE_LAYOUT;
}
```

在layout()方法中，首先会调用`setFrame()`方法来判断视图的大小是否发生过变化，以确定有没有必要对当前的视图进行重绘，同时还会在这里把传递过来的四个参数分别赋值给**mLeft、mTop、mRight和mBottom**这几个变量。接下来会在第11行调用`onLayout()`方法。

View中的onLayout()方法就是一个空方法，因为onLayout()过程是为了确定视图在布局中所在的位置，而这个操作应该是由布局来完成的，即父视图决定子视图的显示位置。我们来看下ViewGroup中的onLayout()方法是怎么写的吧，

```java
@Override
protected abstract void onLayout(boolean changed, int l, int t, int r, int b);
```

ViewGroup中的`onLayout()`方法竟然是一个抽象方法，这就意味着所有ViewGroup的子类都必须重写这个方法。像LinearLayout、RelativeLayout等布局，都是重写了这个方法，然后在内部按照各自的规则对子视图进行布局的。这里我们尝试自定义一个布局

```java
public class SimpleLayout extends ViewGroup {
 
	public SimpleLayout(Context context, AttributeSet attrs) {
		super(context, attrs);
	}
 
	@Override
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
		super.onMeasure(widthMeasureSpec, heightMeasureSpec);
		if (getChildCount() > 0) {
			View childView = getChildAt(0);
			measureChild(childView, widthMeasureSpec, heightMeasureSpec);
		}
	}
 
	@Override
	protected void onLayout(boolean changed, int l, int t, int r, int b) {
		if (getChildCount() > 0) {
			View childView = getChildAt(0);
			childView.layout(0, 0, childView.getMeasuredWidth(), childView.getMeasuredHeight());
		}
	}
 
}
```

你已经知道，onMeasure()方法会在onLayout()方法之前调用，因此这里在onMeasure()方法中判断SimpleLayout中是否有包含一个子视图，如果有的话就调用measureChild()方法来测量出子视图的大小。

接着在onLayout()方法中同样判断SimpleLayout是否有包含一个子视图，然后调用这个子视图的layout()方法来确定它在SimpleLayout布局中的位置,这里传入的四个参数依次是0、0、childView.getMeasuredWidth()和childView.getMeasuredHeight()，分别代表着子视图在SimpleLayout中左上右下四个点的坐标。其中，调用childView.getMeasuredWidth()和childView.getMeasuredHeight()方法得到的值就是在onMeasure()方法中测量出的宽和高。

```xml
<com.example.viewtest.SimpleLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
	
    <ImageView 
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_launcher"
        />
    
</com.example.viewtest.SimpleLayout>

```

我们能够像使用普通的布局文件一样使用SimpleLayout，只是注意它只能包含一个子视图，多余的子视图会被舍弃掉。这里SimpleLayout中包含了一个ImageView，并且ImageView的宽高都是wrap_content.

![](https://img-blog.csdn.net/20131223212259890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3VvbGluX2Jsb2c=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

如果你想改变ImageView显示的位置，只需要改变childView.layout()方法的四个参数就行了

在onLayout()过程结束后，我们就可以调用getWidth()方法和getHeight()方法来获取视图的宽高了.**getWidth()方法和getMeasureWidth()方法到底有什么区别呢**。

首先getMeasureWidth()方法在measure()过程结束后就可以获取到了，而getWidth()方法要在layout()过程结束后才能获取到。另外，getMeasureWidth()方法中的值是通过setMeasuredDimension()方法来进行设置的，而getWidth()方法中的值则是通过视图右边的坐标减去左边的坐标计算出来的。

观察SimpleLayout中onLayout()方法的代码，这里给子视图的layout()方法传入的四个参数分别是0、0、childView.getMeasuredWidth()和childView.getMeasuredHeight()，因此getWidth()方法得到的值就是childView.getMeasuredWidth() - 0 = childView.getMeasuredWidth() ，所以此时getWidth()方法和getMeasuredWidth() 得到的值就是相同的，但如果你将onLayout()方法中的代码进行如下修改：

```java
@Override
protected void onLayout(boolean changed, int l, int t, int r, int b) {
	if (getChildCount() > 0) {
		View childView = getChildAt(0);
		childView.layout(0, 0, 200, 200);
	}
}
```

这样getWidth()方法得到的值就是200 - 0 = 200，不会再和getMeasuredWidth()的值相同了。当然这种做法充分不尊重measure()过程计算出的结果，通常情况下是不推荐这么写的。getHeight()与getMeasureHeight()方法之间的关系同上，就不再重复分析了。

## onDraw()

ViewRoot中的代码会继续执行并创建出一个Canvas对象，然后调用View的draw()方法来执行具体的绘制工作。draw()方法内部的绘制过程总共可以分为六步，其中第二步和第五步在一般情况下很少用到，因此这里我们只分析简化后的绘制过程：

```java
public void draw(Canvas canvas) {
	if (ViewDebug.TRACE_HIERARCHY) {
	    ViewDebug.trace(this, ViewDebug.HierarchyTraceType.DRAW);
	}
	final int privateFlags = mPrivateFlags;
	final boolean dirtyOpaque = (privateFlags & DIRTY_MASK) == DIRTY_OPAQUE &&
	        (mAttachInfo == null || !mAttachInfo.mIgnoreDirtyState);
	mPrivateFlags = (privateFlags & ~DIRTY_MASK) | DRAWN;
	// Step 1, draw the background, if needed
	int saveCount;
	if (!dirtyOpaque) {
	    final Drawable background = mBGDrawable;
	    if (background != null) {
	        final int scrollX = mScrollX;
	        final int scrollY = mScrollY;
	        if (mBackgroundSizeChanged) {
	            background.setBounds(0, 0,  mRight - mLeft, mBottom - mTop);
	            mBackgroundSizeChanged = false;
	        }
	        if ((scrollX | scrollY) == 0) {
	            background.draw(canvas);
	        } else {
	            canvas.translate(scrollX, scrollY);
	            background.draw(canvas);
	            canvas.translate(-scrollX, -scrollY);
	        }
	    }
	}
	final int viewFlags = mViewFlags;
	boolean horizontalEdges = (viewFlags & FADING_EDGE_HORIZONTAL) != 0;
	boolean verticalEdges = (viewFlags & FADING_EDGE_VERTICAL) != 0;
	if (!verticalEdges && !horizontalEdges) {
	    // Step 3, draw the content
	    if (!dirtyOpaque) onDraw(canvas);
	    // Step 4, draw the children
	    dispatchDraw(canvas);
	    // Step 6, draw decorations (scrollbars)
	    onDrawScrollBars(canvas);
	    // we're done...
	    return;
	}
}
```

可以看到，第一步是从第9行代码开始的，这一步的作用是对视图的背景进行绘制。这里会先得到一个mBGDrawable对象，然后根据layout过程确定的视图位置来设置背景的绘制区域，之后再调用Drawable的`draw()`方法来完成背景的绘制工作。那么这个mBGDrawable对象是从哪里来的呢？其实就是在XML中通过android:background属性设置的图片或颜色。当然你也可以在代码中通过`setBackgroundColor()`、`setBackgroundResource()`等方法进行赋值。

接下来的第三步是在第34行执行的，这一步的作用是对视图的内容进行绘制。可以看到，这里去调用了一下onDraw()方法，那么onDraw()方法里又写了什么代码呢？进去一看你会发现，原来又是个空方法啊。其实也可以理解，因为每个视图的内容部分肯定都是各不相同的，这部分的功能交给子类来去实现也是理所当然的。

第三步完成之后紧接着会执行第四步，这一步的作用是对当前视图的所有子视图进行绘制。但如果当前的视图没有子视图，那么也就不需要进行绘制了。因此你会发现View中的`dispatchDraw()`方法又是一个空方法，而ViewGroup的`dispatchDraw()`方法中就会有具体的绘制代码。

以上都执行完后就会进入到第六步，也是最后一步，这一步的作用是对视图的滚动条进行绘制。那么你可能会奇怪，当前的视图又不一定是ListView或者ScrollView，为什么要绘制滚动条呢？其实不管是Button也好，TextView也好，任何一个视图都是有滚动条的，只是一般情况下我们都没有让它显示出来而已。绘制滚动条的代码逻辑也比较复杂，这里就不再贴出来了，因为我们的重点是第三步过程。

View是不会帮我们绘制内容部分的，因此需要每个视图根据想要展示的内容来自行绘制。如果你去观察TextView、ImageView等类的源码，你会发现它们都有重写onDraw()这个方法，，并且在里面执行了相当不少的绘制逻辑。绘制的方式主要是借助Canvas这个类，它会作为参数传入到onDraw()方法中，供给每个视图使用。Canvas这个类的用法非常丰富，基本可以把它当成一块画布，在上面绘制任意的东西.



# 3. 视图状态

使用View的时候都会发现它是有状态的，比如说有一个按钮，普通状态下是一种效果，但是当手指按下的时候就会变成另外一种效果，这样才会给人产生一种点击了按钮的感觉。它背后的实现原理应该是什么样？

视图状态的种类非常多，一共有十几种类型，不过多数情况下我们只会使用到其中的几种，因此这里我们也就只去分析最常用的几种视图状态。

## enabled

表示当前视图是否可用。可以调用`setEnable()`方法来改变视图的可用状态，传入true表示可用，传入false表示不可用。它们之间最大的区别在于，不可用的视图是无法响应onTouch事件的。

## focused

表示当前视图是否获得到焦点。通常情况下有两种方法可以让视图获得焦点，即通过键盘的上下左右键切换视图，以及调用`requestFocus()`方法。现在的Android手机几乎都没有键盘了，因此基本上只可以使用`requestFocus()`这个办法来让视图获得焦点了。而`requestFocus()`方法也**不能保证一定可以让视图获得焦点**，它会有一个布尔值的返回值，如果返回true说明获得焦点成功，返回false说明获得焦点失败。**一般只有视图在focusable和focusable in touch mode同时成立的情况下才能成功获取焦点**，比如说EditText。

## window_focused

表示当前视图是否处于正在交互的窗口中，这个值由系统自动决定，应用程序不能进行改变。

## selected

表示当前视图是否处于选中状态。一个界面当中可以有多个视图处于选中状态，调用`setSelected()`方法能够改变视图的选中状态，传入true表示选中，传入false表示未选中。

## pressed

表示当前视图是否处于按下状态。可以调用`setPressed()`方法来对这一状态进行改变，传入true表示按下，传入false表示未按下。通常情况下这个状态都是由系统自动赋值的，但开发者也可以自己调用这个方法来进行改变。

------



```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
 
    <item android:drawable="@drawable/compose_pressed" android:state_pressed="true"></item>
    <item android:drawable="@drawable/compose_pressed" android:state_focused="true"></item>
    <item android:drawable="@drawable/compose_normal"></item>
 
</selector>
```

这段代码就表示，当视图处于正常状态的时候就显示compose_normal这张背景图，当视图获得到焦点或者被按下的时候就显示compose_pressed这张背景图。

创建好了这个selector文件后，我们就可以在布局或代码中使用它了，比如将它设置为某个按钮的背景图

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    
	<Button 
	    android:id="@+id/compose"
	    android:layout_width="60dp"
	    android:layout_height="40dp"
	    android:layout_gravity="center_horizontal"
	    android:background="@drawable/compose_bg"
	    />
    
</LinearLayout>
```

当手指按在视图上的时候，视图的状态就已经发生了变化，此时视图的pressed状态是true。每当视图的状态有发生改变的时候，就会回调View的`drawableStateChanged()`方法，

```java
protected void drawableStateChanged() {
    Drawable d = mBGDrawable;
    if (d != null && d.isStateful()) {
        d.setState(getDrawableState());
    }
}
```

首先是将mBGDrawable赋值给一个Drawable对象，那么这个mBGDrawable是什么呢？观察`setBackgroundResource()`方法中的代码

```java
public void setBackgroundResource(int resid) {
    if (resid != 0 && resid == mBackgroundResource) {
        return;
    }
    Drawable d= null;
    if (resid != 0) {
        d = mResources.getDrawable(resid);
    }
    setBackgroundDrawable(d);
    mBackgroundResource = resid;
}
```

在第7行调用了Resource的`getDrawable()`方法将resid转换成了一个Drawable对象，然后调用了`setBackgroundDrawable()`方法并将这个Drawable对象传入，在`setBackgroundDrawable()`方法中会将传入的Drawable对象赋值给mBGDrawable。

我们在布局文件中通过**android:background**属性指定的selector文件，效果等同于调用`setBackgroundResource()`方法。也就是说`drawableStateChanged()`方法中的**mBGDrawable**对象其实就是我们指定的**selector**文件

接下来在`drawableStateChanged()`方法的第4行调用了`getDrawableState()`方法来获取视图状态：

```java
public final int[] getDrawableState() {
    if ((mDrawableState != null) && ((mPrivateFlags & DRAWABLE_STATE_DIRTY) == 0)) {
        return mDrawableState;
    } else {
        mDrawableState = onCreateDrawableState(0);
        mPrivateFlags &= ~DRAWABLE_STATE_DIRTY;
        return mDrawableState;
    }
}
```

这里首先会判断当前视图的状态是否发生了改变，如果没有改变就直接返回当前的视图状态，如果发生了改变就调用`onCreateDrawableState()`方法来获取最新的视图状态。视图的所有状态会以一个整型数组的形式返回。

得到了视图状态的数组之后，就会调用Drawable的`setState()`方法来对状态进行更新

```java
public boolean setState(final int[] stateSet) {
    if (!Arrays.equals(mStateSet, stateSet)) {
        mStateSet = stateSet;
        return onStateChange(stateSet);
    }
    return false;
}
```

这里会调用Arrays.equals()方法来判断视图状态的数组是否发生了变化，如果发生了变化则调用onStateChange()方法，否则就直接返回false。但你会发现，Drawable的onStateChange()方法中其实就只是简单返回了一个false，并没有任何的逻辑处理，这是为什么呢？这主要是因为mBGDrawable对象是通过一个selector文件创建出来的，而通过这种文件创建出来的Drawable对象其实都是一个**StateListDrawable**实例，因此这里调用的onStateChange()方法实际上调用的是StateListDrawable中的`onStateChange()`方法，代码如下：

```java
@Override
protected boolean onStateChange(int[] stateSet) {
    int idx = mStateListState.indexOfStateSet(stateSet);
    if (DEBUG) android.util.Log.i(TAG, "onStateChange " + this + " states "
            + Arrays.toString(stateSet) + " found " + idx);
    if (idx < 0) {
        idx = mStateListState.indexOfStateSet(StateSet.WILD_CARD);
    }
    if (selectDrawable(idx)) {
        return true;
    }
    return super.onStateChange(stateSet);
}
```

这里会先调用`indexOfStateSet()`方法来找到当前视图状态所对应的Drawable资源下标，然后在第9行调用`selectDrawable()`方法并将下标传入，在这个方法中就会将视图的背景图设置为当前视图状态所对应的那张图片了。

任何一个视图的显示都要经过非常科学的绘制流程的，很显然，背景图的绘制是在draw()方法中完成的，那么为什么selectDrawable()方法能够控制背景图的改变呢？这就要研究一下视图重绘的流程了。

# 4. 视图重绘

虽然视图会在Activity加载完成之后自动绘制到屏幕上，但是我们完全有理由在与Activity进行交互的时候要求动态更新视图，比如改变视图的状态、以及显示或隐藏某个控件等。那在这个时候，之前绘制出的视图其实就已经过期了，此时我们就应该对视图进行重绘。

调用视图的`setVisibility()`、`setEnabled()`、`setSelected()`等方法时都会导致视图重绘，而如果我们想要手动地强制让视图进行重绘，可以调用`invalidate()`方法来实现。

View的源码中会有数个invalidate()方法的重载和一个`invalidateDrawable()`方法，当然它们的原理都是相同的

```java
void invalidate(boolean invalidateCache) {
    if (ViewDebug.TRACE_HIERARCHY) {
        ViewDebug.trace(this, ViewDebug.HierarchyTraceType.INVALIDATE);
    }
    if (skipInvalidate()) {
        return;
    }
    if ((mPrivateFlags & (DRAWN | HAS_BOUNDS)) == (DRAWN | HAS_BOUNDS) ||
            (invalidateCache && (mPrivateFlags & DRAWING_CACHE_VALID) == DRAWING_CACHE_VALID) ||
            (mPrivateFlags & INVALIDATED) != INVALIDATED || isOpaque() != mLastIsOpaque) {
        mLastIsOpaque = isOpaque();
        mPrivateFlags &= ~DRAWN;
        mPrivateFlags |= DIRTY;
        if (invalidateCache) {
            mPrivateFlags |= INVALIDATED;
            mPrivateFlags &= ~DRAWING_CACHE_VALID;
        }
        final AttachInfo ai = mAttachInfo;
        final ViewParent p = mParent;
        if (!HardwareRenderer.RENDER_DIRTY_REGIONS) {
            if (p != null && ai != null && ai.mHardwareAccelerated) {
                p.invalidateChild(this, null);
                return;
            }
        }
        if (p != null && ai != null) {
            final Rect r = ai.mTmpInvalRect;
            r.set(0, 0, mRight - mLeft, mBottom - mTop);
            p.invalidateChild(this, r);
        }
    }
}
```

在这个方法中首先会调用`skipInvalidate()`方法来判断当前View是否需要重绘，判断的逻辑也比较简单，如果View是不可见的且没有执行任何动画，就认为不需要重绘了。之后会进行透明度的判断，并给View添加一些标记位，然后在第22和29行调用ViewParent的`invalidateChild()`方法，这里的ViewParent其实就是当前视图的父视图，因此会调用到ViewGroup的invalidateChild()方法中：

```java
public final void invalidateChild(View child, final Rect dirty) {
    ViewParent parent = this;
    final AttachInfo attachInfo = mAttachInfo;
    if (attachInfo != null) {
        final boolean drawAnimation = (child.mPrivateFlags & DRAW_ANIMATION) == DRAW_ANIMATION;
        if (dirty == null) {
            ......
        } else {
            ......
            do {
                View view = null;
                if (parent instanceof View) {
                    view = (View) parent;
                    if (view.mLayerType != LAYER_TYPE_NONE &&
                            view.getParent() instanceof View) {
                        final View grandParent = (View) view.getParent();
                        grandParent.mPrivateFlags |= INVALIDATED;
                        grandParent.mPrivateFlags &= ~DRAWING_CACHE_VALID;
                    }
                }
                if (drawAnimation) {
                    if (view != null) {
                        view.mPrivateFlags |= DRAW_ANIMATION;
                    } else if (parent instanceof ViewRootImpl) {
                        ((ViewRootImpl) parent).mIsAnimating = true;
                    }
                }
                if (view != null) {
                    if ((view.mViewFlags & FADING_EDGE_MASK) != 0 &&
                            view.getSolidColor() == 0) {
                        opaqueFlag = DIRTY;
                    }
                    if ((view.mPrivateFlags & DIRTY_MASK) != DIRTY) {
                        view.mPrivateFlags = (view.mPrivateFlags & ~DIRTY_MASK) | opaqueFlag;
                    }
                }
                parent = parent.invalidateChildInParent(location, dirty);
                if (view != null) {
                    Matrix m = view.getMatrix();
                    if (!m.isIdentity()) {
                        RectF boundingRect = attachInfo.mTmpTransformRect;
                        boundingRect.set(dirty);
                        m.mapRect(boundingRect);
                        dirty.set((int) boundingRect.left, (int) boundingRect.top,
                                (int) (boundingRect.right + 0.5f),
                                (int) (boundingRect.bottom + 0.5f));
                    }
                }
            } while (parent != null);
        }
    }
}
```

可以看到，这里在第10行进入了一个while循环，当ViewParent不等于空的时候就会一直循环下去。在这个while循环当中会不断地获取当前布局的父布局，并调用它的`invalidateChildInParent()`方法，在ViewGroup的`invalidateChildInParent()`方法中主要是来计算需要重绘的矩形区域，这里我们先不管它，当循环到最外层的根布局后，，就会调用ViewRoot的`invalidateChildInParent()`方法了。

```java
    public ViewParent invalidateChildInParent(final int[] location, final Rect dirty) {
        invalidateChild(null, dirty);
        return null;
    }
```

这里的代码非常简单，仅仅是去调用了invalidateChild()方法而已，那我们再跟进去瞧一瞧

```java
public void invalidateChild(View child, Rect dirty) {
    checkThread();
    if (LOCAL_LOGV) Log.v(TAG, "Invalidate child: " + dirty);
    mDirty.union(dirty);
    if (!mWillDrawSoon) {
        scheduleTraversals();
    }
}
```

这个方法也不长，它在第6行又调用了scheduleTraversals()这个方法，那么我们继续跟进

```java
public void scheduleTraversals() {
    if (!mTraversalScheduled) {
        mTraversalScheduled = true;
        sendEmptyMessage(DO_TRAVERSAL);
    }
}
```

可以看到，这里调用了sendEmptyMessage()方法，并传入了一个DO_TRAVERSAL参数。了解Android异步消息处理机制的朋友们都会知道，任何一个Handler都可以调用sendEmptyMessage()方法来发送消息，并且在handleMessage()方法中接收消息，而如果你看一下ViewRoot的类定义就会发现，它是继承自Handler的，也就是说这里调用sendEmptyMessage()方法出的消息，会在ViewRoot的handleMessage()方法中接收到。那么赶快看一下handleMessage()方法的代码吧

```java
public void handleMessage(Message msg) {
    switch (msg.what) {
    case DO_TRAVERSAL:
        if (mProfile) {
            Debug.startMethodTracing("ViewRoot");
        }
        performTraversals();
        if (mProfile) {
            Debug.stopMethodTracing();
            mProfile = false;
        }
        break;
    ......
}
```

这里在第7行调用了performTraversals()方法，可以确定的是，调用视图的invalidate()方法后确实会走到performTraversals()方法中，然后重新执行绘制流程。

了解了这些之后，我们再回过头来看看刚才的selectDrawable()方法中到底做了什么才能够控制背景图的改变

```java
public boolean selectDrawable(int idx) {
    if (idx == mCurIndex) {
        return false;
    }
    final long now = SystemClock.uptimeMillis();
    if (mDrawableContainerState.mExitFadeDuration > 0) {
        if (mLastDrawable != null) {
            mLastDrawable.setVisible(false, false);
        }
        if (mCurrDrawable != null) {
            mLastDrawable = mCurrDrawable;
            mExitAnimationEnd = now + mDrawableContainerState.mExitFadeDuration;
        } else {
            mLastDrawable = null;
            mExitAnimationEnd = 0;
        }
    } else if (mCurrDrawable != null) {
        mCurrDrawable.setVisible(false, false);
    }
    if (idx >= 0 && idx < mDrawableContainerState.mNumChildren) {
        Drawable d = mDrawableContainerState.mDrawables[idx];
        mCurrDrawable = d;
        mCurIndex = idx;
        if (d != null) {
            if (mDrawableContainerState.mEnterFadeDuration > 0) {
                mEnterAnimationEnd = now + mDrawableContainerState.mEnterFadeDuration;
            } else {
                d.setAlpha(mAlpha);
            }
            d.setVisible(isVisible(), true);
            d.setDither(mDrawableContainerState.mDither);
            d.setColorFilter(mColorFilter);
            d.setState(getState());
            d.setLevel(getLevel());
            d.setBounds(getBounds());
        }
    } else {
        mCurrDrawable = null;
        mCurIndex = -1;
    }
    if (mEnterAnimationEnd != 0 || mExitAnimationEnd != 0) {
        if (mAnimationRunnable == null) {
            mAnimationRunnable = new Runnable() {
                @Override public void run() {
                    animate(true);
                    invalidateSelf();
                }
            };
        } else {
            unscheduleSelf(mAnimationRunnable);
        }
        animate(true);
    }
    invalidateSelf();
    return true;
}
```

这里前面的代码我们可以都不管，关键是要看到在第54行一定会调用invalidateSelf()方法，这个方法中的代码：

```java
public void invalidateSelf() {
    final Callback callback = getCallback();
    if (callback != null) {
        callback.invalidateDrawable(this);
    }
}
```

可以看到，这里会先调用getCallback()方法获取Callback接口的回调实例，然后再去调用回调实例的invalidateDrawable()方法。那么这里的回调实例又是什么呢？观察一下View的类定义其实你就知道了，

```java
public class View implements Drawable.Callback, Drawable.Callback2, KeyEvent.Callback,
AccessibilityEventSource {
    ......
}
```

View类正是实现了Callback接口，所以刚才其实调用的就是View中的invalidateDrawable()方法，之后就会按照我们前面分析的流程执行重绘逻辑，所以视图的背景图才能够得到改变的。

另外需要注意的是，invalidate()方法虽然最终会调用到performTraversals()方法中，但这时measure和layout流程是不会重新执行的，因为视图没有强制重新测量的标志位，而且大小也没有发生过变化，所以这时只有draw流程可以得到执行。而如果你希望视图的绘制流程可以完完整整地重新走一遍，就不能使用invalidate()方法，而应该调用requestLayout()了。这个方法中的流程比invalidate()方法要简单一些，但中心思想是差不多的。