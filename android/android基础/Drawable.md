# 1. Drawable概述

参考文献：

参考文献一：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-drawable1.html)

参考文献二：[API](https://developer.android.com/reference/android/graphics/drawable/Drawable)

------

![](http://www.runoob.com/wp-content/uploads/2015/10/57840489.jpg)

- Drawable分为两种： 一种是我们**普通的图片资源**，在Android Studio中我们一般放到**res/mipmap**目录下， 和以前的Eclipse不一样哦！另外我们**如果把工程切换成Android项目模式，我们直接 往mipmap目录下丢图片即可**，AS会自动分hdpi，xhdpi...！ 另一种是我们编写的**XML形式的Drawable资源**，我们一般把他们放到**res/drawable**目录 下，比如最常见的按钮点击背景切换的Selctor！

- 在XML我们直接通过@mipmap或者@drawable设置Drawable即可 比如: `android:background = "@mipmap/iv_icon_zhu"` / "`@drawable/btn_back_selctor`" 而在Java代码中我们可以通过Resource的`getDrawable(R.mipmap.xxx)`可以获得drawable资源 如果是为某个控件设置背景，比如ImageView，我们可以直接调用控件`.getDrawale()`同样 可以获得drawable对象

- Android中drawable中的资源名称有约束，必须是：**[a-z0-9_.]**（即：只能是字母数字及*和.）， 而且**不能以数字开头**，否则编译会报错： Invalid file name: must contain only [a-z0-9*.]！ **小写**啊
- Android中给我们提供的13种Drawable

------

# 2. Drawable详解

## ColorDrawable

最简单的一种Drawable，当我们将ColorDrawable绘制到Canvas(画布)上的时候, 会使用一种固定的颜色来填充Paint,然后在画布上绘制出一片单色区域！

1)**.Java中定义ColorDrawable**:

```
ColorDrawable drawable = new ColorDrawable(0xffff2200);  
txtShow.setBackground(drawable);  
```

2)**.在xml中定义ColorDrawable**:

```
<?xml version="1.0" encoding="utf-8"?>  
<color  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    android:color="#FF0000"/>  
```

当然上面这些用法,其实用得不多,更多的时候我们是在res/values目录下创建一个color.xml 文件,然后把要用到的颜色值写到里面,需要的时候通过@color获得相应的值，比如：

- 建立一个**color.xml**文件

```xml
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <color name="material_grey_100">#fff5f5f5</color>
    <color name="material_grey_300">#ffe0e0e0</color>
    <color name="material_grey_50">#fffafafa</color>
    <color name="material_grey_600">#ff757575</color>
    <color name="material_grey_800">#ff424242</color>
    <color name="material_grey_850">#ff303030</color>
    <color name="material_grey_900">#ff212121</color>
</resources>
```

然后如果是在xml文件中话我们可以通过`@color/xxx`获得对应的color值 如果是在Java中:

```java
int mycolor = getResources().getColor(R.color.mycolor);    
btn.setBackgroundColor(mycolor);  
```

如果我们在Java中直接定义颜色值的话,要加上0x,而且不能把透明度漏掉:

```java
int mycolor = 0xff123456;    
btn.setBackgroundColor(mycolor); 
```

- **使用系统定义好的color**:

比如:BLACK(黑色),BLUE(蓝色),CYAN(青色),GRAY(灰色),GREEN(绿色),RED(红色),WRITE(白色),YELLOW(黄色)! 用法： **btn.setBackgroundColor(Color.BLUE);** 也可以获得系统颜色再设置：

```java
int getcolor = Resources.getSystem().getColor(android.R.color.holo_green_light);  
btn.setBackgroundColor(getcolor);  
```

xml中使用:**android:background="@android:color/black"**

- **利用静态方法argb来设置颜色**:

> Android使用一个int类型的数据表示颜色值,通常是十六进制,即**0x开头**， 颜色值的定义是由透明度alpha和RGB(红绿蓝)三原色来定义的,以"#"开始,后面依次为: 
> **透明度-红-绿-蓝**;eg:#RGB #ARGB #RRGGBB #AARRGGBB 
> 每个要素都由一个字节(8 bit)来表示,所以取值范围为0~255,在xml中设置颜色可以忽略透明度, 但是**如果你是在Java代码中的话就需要明确指出透明度的值**了,省略的话表示完全透明,这个时候 就没有效果了哦~比如:0xFF0000虽然表示红色,但是如果直接这样写,什么的没有,而应该这样写: 0xFFFF0000,记Java代码设置颜色值,需要在前面添加上透明度~ 示例:(参数依次为:透明度,红色值,绿色值,蓝色值) **txtShow.setBackgroundColor(Color.argb(0xff, 0x00, 0x00, 0x00));**

## NiewPatchDrawable

即**.9图**，实现图片拉伸的自适应

**注意事项**：

- 点9图不能放在mipmap目录下，而需要放在drawable目录下！
- AS中的.9图，必须要有黑线，不然编译都不会通过

**xml定义NinePatchDrawable**:

```xml
<!--pic9.xml-->  
<!--参数依次为:引用的.9图片,是否对位图进行抖动处理-->  
<?xml version="1.0" encoding="utf-8"?>  
<nine-patch  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    android:src="@drawable/dule_pic"  
    android:dither="true"/>  
```

**使用Bitmap包装.9图片**:

```xml
<!--pic9.xml-->  
<!--参数依次为:引用的.9图片,是否对位图进行抖动处理-->  
<?xml version="1.0" encoding="utf-8"?>  
<bitmap  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    android:src="@drawable/dule_pic"  
    android:dither="true"/>
```

## ShapeDrawable

形状的Drawable咯,定义基本的几何图形,如(矩形,圆形,线条等),根元素是<shape../> 

> - ① <**shape**>:
> - ~ **visible**:设置是否可见
> - ~ **shape**:形状,可选:rectangle(矩形,包括正方形),oval(椭圆,包括圆),line(线段),ring(环形)
> - ~ **innerRadiusRatio**:当shape为ring才有效,表示环内半径所占半径的比率,如果设置了innerRadius, 他会被忽略
> - ~ **innerRadius**:当shape为ring才有效,表示环的内半径的尺寸
> - ~ **thicknessRatio**:当shape为ring才有效,表环厚度占半径的比率
> - ~ **thickness**:当shape为ring才有效,表示环的厚度,即外半径与内半径的差
> - ~ **useLevel**:当shape为ring才有效,表示是否允许根据level来显示环的一部分
> - ②<**size**>:
> - ~ **width**:图形形状宽度
> - ~ **height**:图形形状高度
> - ③<**gradient**>：后面GradientDrawable再讲~
> - ④<**solid**>
> - ~ **color**:背景填充色,设置solid后会覆盖gradient设置的所有效果!!!!!!
> - ⑤<**stroke**>
> - ~ **width**:边框的宽度
> - ~ **color**:边框的颜色
> - ~ **dashWidth**:边框虚线段的长度
> - ~ **dashGap**:边框的虚线段的间距
> - ⑥<**conner**>
> - ~ **radius**:圆角半径,适用于上下左右四个角
> - ~ **topLeftRadius**,**topRightRadius**,**BottomLeftRadius**,**tBottomRightRadius**: 依次是左上,右上,左下,右下的圆角值,按自己需要设置!
> - ⑦<**padding**>
> - left,top,right,bottm:依次是左上右下方向上的边距!

## GradientDrawable

一个具有渐变区域的Drawable，可以实现线性渐变,发散渐变和平铺渐变效果 核心节点：<**gradient**/>

> - **startColor**:渐变的起始颜色
> - **centerColor**:渐变的中间颜色
> - **endColor**:渐变的结束颜色
> - **type**:渐变类型,可选(**linear**,**radial**,**sweep**), **线性渐变**(可设置渐变角度),发散渐变(中间向四周发散),平铺渐变
> - **centerX**:渐变中间颜色的x坐标,取值范围为:0~1
> - **centerY**:渐变中间颜色的Y坐标,取值范围为:0~1
> - **angle**:只有linear类型的渐变才有效,表示渐变角度,**必须为45的倍数**哦
> - **gradientRadius**:只有radial和sweep类型的渐变才有效,radial必须设置,表示渐变效果的半径
> - **useLevel**:判断是否根据level绘制渐变效果

示例：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(new SampleView(this));
    }

    private static class SampleView extends View {
        private ShapeDrawable[] mDrawables;

        private static Shader makeSweep() {
            return new SweepGradient(150, 25,
                    new int[] { 0xFFFF0000, 0xFF00FF00, 0xFF0000FF, 0xFFFF0000 },
                    null);
        }

        private static Shader makeLinear() {
            return new LinearGradient(0, 0, 50, 50,
                    new int[] { 0xFFFF0000, 0xFF00FF00, 0xFF0000FF },
                    null, Shader.TileMode.MIRROR);
        }

        private static Shader makeTiling() {
            int[] pixels = new int[] { 0xFFFF0000, 0xFF00FF00, 0xFF0000FF, 0};
            Bitmap bm = Bitmap.createBitmap(pixels, 2, 2,
                    Bitmap.Config.ARGB_8888);

            return new BitmapShader(bm, Shader.TileMode.REPEAT,
                    Shader.TileMode.REPEAT);
        }

        private static class MyShapeDrawable extends ShapeDrawable {
            private Paint mStrokePaint = new Paint(Paint.ANTI_ALIAS_FLAG);

            public MyShapeDrawable(Shape s) {
                super(s);
                mStrokePaint.setStyle(Paint.Style.STROKE);
            }

            public Paint getStrokePaint() {
                return mStrokePaint;
            }

            @Override protected void onDraw(Shape s, Canvas c, Paint p) {
                s.draw(c, p);
                s.draw(c, mStrokePaint);
            }
        }

        public SampleView(Context context) {
            super(context);
            setFocusable(true);

            float[] outerR = new float[] { 12, 12, 12, 12, 0, 0, 0, 0 };
            RectF inset = new RectF(6, 6, 6, 6);
            float[] innerR = new float[] { 12, 12, 0, 0, 12, 12, 0, 0 };

            Path path = new Path();
            path.moveTo(50, 0);
            path.lineTo(0, 50);
            path.lineTo(50, 100);
            path.lineTo(100, 50);
            path.close();

            mDrawables = new ShapeDrawable[7];
            mDrawables[0] = new ShapeDrawable(new RectShape());
            mDrawables[1] = new ShapeDrawable(new OvalShape());
            mDrawables[2] = new ShapeDrawable(new RoundRectShape(outerR, null,
                    null));
            mDrawables[3] = new ShapeDrawable(new RoundRectShape(outerR, inset,
                    null));
            mDrawables[4] = new ShapeDrawable(new RoundRectShape(outerR, inset,
                    innerR));
            mDrawables[5] = new ShapeDrawable(new PathShape(path, 100, 100));
            mDrawables[6] = new MyShapeDrawable(new ArcShape(45, -270));

            mDrawables[0].getPaint().setColor(0xFFFF0000);
            mDrawables[1].getPaint().setColor(0xFF00FF00);
            mDrawables[2].getPaint().setColor(0xFF0000FF);
            mDrawables[3].getPaint().setShader(makeSweep());
            mDrawables[4].getPaint().setShader(makeLinear());
            mDrawables[5].getPaint().setShader(makeTiling());
            mDrawables[6].getPaint().setColor(0x88FF8844);

            PathEffect pe = new DiscretePathEffect(10, 4);
            PathEffect pe2 = new CornerPathEffect(4);
            mDrawables[3].getPaint().setPathEffect(
                    new ComposePathEffect(pe2, pe));

            MyShapeDrawable msd = (MyShapeDrawable)mDrawables[6];
            msd.getStrokePaint().setStrokeWidth(4);
        }

        @Override protected void onDraw(Canvas canvas) {

            int x = 10;
            int y = 10;
            int width = 400;
            int height = 100;

            for (Drawable dr : mDrawables) {
                dr.setBounds(x, y, x + width, y + height);
                dr.draw(canvas);
                y += height + 5;
            }
        }
    }
}
```

## BitmapDrawable

对**Bitmap**的一种封装,可以设置它包装的bitmap在BitmapDrawable区域中的绘制方式,有: 平铺填充,拉伸填或保持图片原始大小!以<**bitmap**>为根节点! 

> - **src**:图片资源~
> - **antialias**:是否支持抗锯齿
> - **filter**:是否支持位图过滤,支持的话可以是图批判显示时比较光滑
> - **dither**:是否对位图进行抖动处理
> - **gravity**:若位图比容器小,可以设置位图在容器中的相对位置
> - **tileMode**:指定图片平铺填充容器的模式,设置这个的话,gravity属性会被忽略,有以下可选值: **disabled**(整个图案拉伸平铺),**clamp**(原图大小), **repeat**(平铺),**mirror**(镜像平铺)

在res/drawable中xml定义**BitmapDrawable**

```xml
<?xml version="1.0" encoding="utf-8"?>  
<bitmap xmlns:android="http://schemas.android.com/apk/res/android"  
    android:dither="true"  
    android:src="@drawable/ic_launcher"  
    android:tileMode="mirror" />
```

**②实现相同效果的Java代码**:

```java
BitmapDrawable bitDrawable = new BitmapDrawable(bitmap);  
bitDrawable.setDither(true);  
bitDrawable.setTileModeXY(TileMode.MIRROR,TileMode.MIRROR);  
```

## InsetDrawable

> 表示把一个Drawable嵌入到另外一个Drawable的内部，并且在内部留一些间距, 类似与Drawable的padding属性,但**padding**表示的是**Drawable的内容与Drawable本身的边距**! 而**InsetDrawable**表示的是**两个Drawable与容器之间的边距**,当控件需要的**背景比实际的边框 小的时候**,比较适合使用InsetDrawable,比如使用这个可以解决我们自定义Dialog与屏幕之间 的一个间距问题,相信做过的朋友都知道,即使我们设置了layout_margin的话也是没用的,这个 时候就可以用到这个InsetDrawable了!只需为InsetDrawable设置一个insetXxx设置不同 方向的边距,然后为设置为Dialog的背景即可！



**相关属性**：

- 1.**drawable**:引用的Drawable,如果为空,必须有一个Drawable类型的子节点!
- 2.**visible**:设置Drawable是否额空间
- 3.**insetLeft**,**insetRight**,**insetTop**,**insetBottm**:设置左右上下的边距

**XML中使用**:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<inset xmlns:android="http://schemas.android.com/apk/res/android"  
    android:drawable="@drawable/test1"  
    android:insetBottom="10dp"  
    android:insetLeft="10dp"  
    android:insetRight="10dp"  
    android:insetTop="10dp" /> 
```

**在Java代码中使用**：

```java
InsetDrawable insetDrawable = new InsetDrawable(getResources()  
        .getDrawable(R.drawable.test1), 10, 10, 10, 10); 
```

## ClipDrawable

> 可以把ClipDrawable理解为从位图上剪下一个部分; Android中的进度条就是使用ClipDrawable来实现的,他根据设置level的值来决定剪切 区域的大小,根节点是<**clip**>

**相关属性设置**：

- **clipOrietntion**:设置剪切的方向,可以设置水平和竖直2个方向
- **gravity**:从那个位置开始裁剪
- **drawable**:引用的drawable资源,为空的话需要有一个Drawable类型的子节点 ps:这个Drawable类型的子节点:就是在<clip里>加上这样的语句: 这样...

**核心**：通过代码修改ClipDrawable的level的值！**Level的值是0~10000**！

**示例**：

- 定义一个**ClipDrawable**资源的xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <clip xmlns:android="http://schemas.android.com/apk/res/android"
      android:clipOrientation="horizontal"
      android:drawable="@drawable/apple_pic"
      android:gravity="left" />
  ```

- 修改**activity_main.xml**布局文件，将src设置为**clipDrawable**。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <ImageView
          android:id="@+id/img_show"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:src="@drawable/clip" />
  
  
  </LinearLayout>
  ```

- **MainActivity.java通过setLevel设置截取区域大小**:

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ImageView img_show;
      private ClipDrawable cd;
      private Handler handler = new Handler() {
          @Override
          public void handleMessage(Message msg) {
              if (msg.what == 0x123) {
                  cd.setLevel(cd.getLevel() + 500);
              }
          }
      };
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          img_show = (ImageView) findViewById(R.id.img_show);
          // 核心实现代码
          cd = (ClipDrawable) img_show.getDrawable();
          final Timer timer = new Timer();
          timer.schedule(new TimerTask() {
              @Override
              public void run() {
                  handler.sendEmptyMessage(0x123);
                  if (cd.getLevel() >= 10000) {
                      timer.cancel();
                  }
              }
          }, 0, 300);
      }
  }
  ```

## RotateDrawable

用来对Drawable进行旋转，通过**setLevel**控制旋转，最大值也是10000；

**相关属性**：

> - **fromDegrees**:起始的角度,,对应最低的level值,默认为0
> - **toDegrees**:结束角度,对应最高的level值,默认360
> - **pivotX**:设置参照点的x坐标,取值为0~1,默认是50%,即0.5
> - **pivotY**:设置参照点的Y坐标,取值为0~1,默认是50%,即0.5 ps:如果出现旋转图片显示不完全的话可以修改上述两个值解决!
> - **drawable**:设置位图资源
> - **visible**:设置drawable是否可见!

![](http://www.runoob.com/wp-content/uploads/2015/10/70168328.jpg)

**示例**：

- 定义一个rotateDrawable资源文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <rotate xmlns:android="http://schemas.android.com/apk/res/android"
      android:drawable="@mipmap/ic_launcher"
      android:fromDegrees="-180"
      android:pivotX="50%"
      android:pivotY="50%" />
  ```

- 修改activity_main.xml布局文件，修改前述的src指向即可

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ImageView img_show;
      private RotateDrawable cd;
      private Handler handler = new Handler() {
          @Override
          public void handleMessage(Message msg) {
              if (msg.what == 0x123) {
                  cd.setLevel(cd.getLevel() + 400);
              }
          }
      };
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          img_show = (ImageView) findViewById(R.id.img_show);
          // 核心实现代码
          cd = (RotateDrawable) img_show.getDrawable();
          final Timer timer = new Timer();
          timer.schedule(new TimerTask() {
              @Override
              public void run() {
                  handler.sendEmptyMessage(0x123);
                  if (cd.getLevel() >= 10000) {
                      timer.cancel();
                  }
              }
          }, 0, 100);
      }
  }
  ```

## AnimationDrawable

> AnimationDrawable是用来实现Android中帧动画的,就是把一系列的 Drawable，按照一定得顺序一帧帧地播放；Android中动画比较丰富,有传统补间动画,平移, 缩放等等效果,使用<animation-list>作为根节点

**相关属性**：

**oneshot**:设置是否循环播放,false为循环播放!!! **duration**:帧间隔时间,通常我们会设置为300毫秒 我们获得AniamtionDrawable实例后，需要调用它的`start()`方法播放动画，另外要注意 在OnCreate()方法中调用的话,是没有任何效果的,因为View还没完成初始化,我们可以 用简单的handler来延迟播放动画!

①**先定义一个AnimationDrawable的xml资源文件**:

```java
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false">

    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading01"
        android:duration="100" />

    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading02"
        android:duration="100" />
    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading03"
        android:duration="100" />
    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading04"
        android:duration="100" />
    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading05"
        android:duration="100" />
    <item
        android:drawable="@mipmap/ic_pull_to_refresh_loading06"
        android:duration="100" />

</animation-list> 
```

**②activity_main.xml设置下src,然后MainActivity中**:

```java
public class MainActivity extends AppCompatActivity {

    private ImageView img_show;
    private AnimationDrawable ad;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        img_show = (ImageView) findViewById(R.id.img_show);
        // 核心实现代码
        ad = (AnimationDrawable) img_show.getDrawable();
        Handler handler = new Handler();
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                ad.start();
            }
        }, 300);
    }
}
```

## LayerDrawable

层图形对象，包含一个Drawable数组，然后按照数组对应的顺序来绘制他们，索引 值最大的Drawable会被绘制在最上层！虽然这些Drawable会有交叉或者重叠的区域，但 他们位于不同的层，所以并不会相互影响，以<**layer-list**>作为根节点！

- **drawable**:引用的位图资源,如果为空有一个Drawable类型的子节点
- **left**:层相对于容器的左边距
- **right**:层相对于容器的右边距
- **top**:层相对于容器的上边距
- **bottom**:层相对于容器的下边距
- **id**:层的id

**示例**：

- 新建layerlist_one.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item android:id="@android:id/background">
          <shape android:shape="rectangle">
              <solid android:color="#C2C2C1" />
              <corners android:radius="50dp" />
          </shape>
      </item>
      <item android:id="@android:id/progress">
          <clip>
              <shape android:shape="rectangle">
                  <solid android:color="#BCDA73" />
                  <corners android:radius="50dp" />
              </shape>
          </clip>
      </item>
  </layer-list>
  ```

- 修改activity_main.xml布局文件

  ```xml
  <SeekBar
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:indeterminateDrawable="@android:drawable/progress_indeterminate_horizontal"
      android:indeterminateOnly="false"
      android:maxHeight="10dp"
      android:minHeight="5dp"
      android:progressDrawable="@drawable/layerlist_one"
      android:thumb="@drawable/apple_pic" />
  ```

示例二：

- 新建layerlist_two.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <item>
          <bitmap
              android:gravity="center"
              android:src="@mipmap/ic_bg_ciwei" />
      </item>
      <item
          android:left="25dp"
          android:top="25dp">
          <bitmap
              android:gravity="center"
              android:src="@mipmap/ic_bg_ciwei" />
      </item>
      <item
          android:left="50dp"
          android:top="50dp">
          <bitmap
              android:gravity="center"
              android:src="@mipmap/ic_bg_ciwei" />
      </item>
  </layer-list> 
  ```

- 修改activity_main.xml布局文件：

  ```xml
  <ImageView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:src="@drawable/layerlist_two"/>
  ```

## TransitionDrawable

LayerDrawable的一个子类，TransitionDrawable只管理**两层的Drawable**！两层！两层！ 并且**提供了透明度变化**的动画,可以控制一层Drawable过度到另一层Drawable的动画效果。 根节点为<**transition**>，记住只有**两个Item**，多了也没用，属性和LayerDrawable差不多， 我们需要调用**startTransition**方法才能启动两层间的切换动画； 也可以调用**reverseTransition()**方法反过来播放：

示例：

- 新建transitiondrawable.xml文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <transition xmlns:android="http://schemas.android.com/apk/res/android" >
      <item android:drawable="@mipmap/ic_bg_meizi1"/>
      <item android:drawable="@mipmap/ic_bg_meizi2"/>
  </transition>
  ```

- 在activity_main.xml布局文件中添加ImageView，将src设为上面的drawable，修改MainActivity.java

  ```java
  public class MainActivity extends AppCompatActivity {
      private ImageView img_show;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          img_show = (ImageView) findViewById(R.id.img_show);
          TransitionDrawable td = (TransitionDrawable) img_show.getDrawable();
          td.startTransition(3000);
          //你可以可以反过来播放，使用reverseTransition即可~
          //td.reverseTransition(3000);
      }
  }
  ```



## LevleListDrawable

用来管理一组Drawable的,我们可以为里面的drawable设置不同的level， 当他们绘制的时候，会根据**level属性值获取对应的drawable绘制到画布**上，根节点 为:<**level-list**>他并没有可以设置的属性，我们能做的只是设置每个<**item**> 的属性！

**item属性**：

- **drawable**:引用的位图资源,如果为空有一个Drawable类型的子节点
- **minlevel:level**对应的最小值
- **maxlevel:level**对应的最大值

**示例**：

- **shape_cir1.xml**

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <shape xmlns:android="http://schemas.android.com/apk/res/android"
      android:shape="oval">
      <solid android:color="#2C96ED"/>
      <size android:height="20dp" android:width="20dp"/>
  </shape>
  ```

- **level_cir.xml**

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <level-list xmlns:android="http://schemas.android.com/apk/res/android" >
      <item android:drawable="@drawable/shape_cir1" android:maxLevel="2000"/>
      <item android:drawable="@drawable/shape_cir2" android:maxLevel="4000"/>
      <item android:drawable="@drawable/shape_cir3" android:maxLevel="6000"/>
      <item android:drawable="@drawable/shape_cir4" android:maxLevel="8000"/>
      <item android:drawable="@drawable/shape_cir5" android:maxLevel="10000"/>
  ```

- **MainActivity.java**

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ImageView img_show;
  
      private LevelListDrawable ld;
      private Handler handler = new Handler() {
          public void handleMessage(Message msg) {
              if (msg.what == 0x123) {
                  if (ld.getLevel() > 10000) ld.setLevel(0);
                  img_show.setImageLevel(ld.getLevel() + 2000);
              }
          }
      };
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          img_show = (ImageView) findViewById(R.id.img_show);
          ld = (LevelListDrawable) img_show.getDrawable();
          img_show.setImageLevel(0);
          new Timer().schedule(new TimerTask() {
              @Override
              public void run() {
                  handler.sendEmptyMessage(0x123);
              }
          }, 0, 100);
      }
  }
  ```

## StateListDrawable

**相关属性**：

> - **drawable**:引用的Drawable位图,我们可以把他放到最前面,就表示组件的正常状态~
> - **state_focused**:是否获得焦点
> - **state_window_focused**:是否获得窗口焦点
> - **state_enabled**:控件是否可用
> - **state_checkable**:控件可否被勾选,eg:checkbox
> - **state_checked**:控件是否被勾选
> - **state_selected**:控件是否被选择,针对有滚轮的情况
> - **state_pressed**:控件是否被按下
> - **state_active**:控件是否处于活动状态,eg:slidingTab
> - **state_single**:控件包含多个子控件时,确定是否只显示一个子控件
> - **state_first**:控件包含多个子控件时,确定第一个子控件是否处于显示状态
> - **state_middle**:控件包含多个子控件时,确定中间一个子控件是否处于显示状态
> - **state_last**:控件包含多个子控件时,确定最后一个子控件是否处于显示状态

示例：(简单的圆角按钮)

- **shape_btn_normal.xml**

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <shape xmlns:android="http://schemas.android.com/apk/res/android"
      android:shape="rectangle">
      <solid android:color="#DD788A"/>
      <corners android:radius="5dp"/>
      <padding android:top="2dp" android:bottom="2dp"/>
  </shape>
  ```

- **selector_btn.xml**

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <selector xmlns:android="http://schemas.android.com/apk/res/android">
      <item android:state_pressed="true" android:drawable="@drawable/shape_btn_pressed"/>
      <item android:drawable="@drawable/shape_btn_normal"/>
  </selector>
  ```

然后按钮设置android:background="@drawable/selctor_btn"就可以了~ 你可以根据自己需求改成矩形或者椭圆，圆形等

# 3. Bitmap解析

> - **Drawable**：通用的图形对象，用于装载常用格式的图像，既可以是PNG，JPG这样的图像， 也是前面学的那13种Drawable类型的可视化对象！我们可以理解成一个用来放画的——**画框**！
> - **Bitmap(位图)**：我们可以把他看作一个**画架**，我们先把画放到上面，然后我们可以 进行一些处理，比如获取图像文件信息，做旋转切割，放大缩小等操作！
> - **Canvas(画布)**：如其名，**画布**，我们可以在上面作画(绘制)，你既可以用**Paint(画笔)**， 来画各种形状或者写字，又可以用**Path(路径)**来绘制多个点，然后连接成各种图形！
> - **Matrix(矩阵)**：用于图形特效处理的，颜色矩阵(ColorMatrix)，还有使用Matrix进行图像的 平移，缩放，旋转，倾斜等！
>
> 而上述的这些都是Android中的底层图形类：**android.graphics**给我们提供的接口

## Bitmap、BitmapFactory、BitmapFactory.Options

Bitmap的构造方法是私有的，外面不能实例化，只能通过JNI实例化！ 当然，肯定也会给我们提供一个接口给我们来创建Bitmap的，而这个接口类就是：**BitmapFactory**！

![](http://www.runoob.com/wp-content/uploads/2015/10/47964812.jpg)

每一种方法，都会有一个Options类型的参数，点进去看看： 于是乎我们发现了这货是一个静态内部类:**BitmapFacotry.Options**! 而他是用来设置decode时的选项的！

![](http://www.runoob.com/wp-content/uploads/2015/10/85259228.jpg)

## Bitmap常用方法

> **普通方法**
>
> - public boolean **compress** (Bitmap.CompressFormat format, int quality, OutputStream stream) 将位图的压缩到指定的OutputStream，可以理解成将Bitmap保存到文件中！ **format**：格式，PNG，JPG等； **quality**：压缩质量，0-100，0表示最低画质压缩，100最大质量(PNG无损，会忽略品质设定) **stream**：输出流 返回值代表是否成功压缩到指定流！
> - void **recycle**()：回收位图占用的内存空间，把位图标记为Dead
> - boolean **isRecycled**()：判断位图内存是否已释放
> - int **getWidth**()：获取位图的宽度
> - int **getHeight**()：获取位图的高度
> - boolean **isMutable**()：图片是否可修改
> - int **getScaledWidth**(Canvas canvas)：获取指定密度转换后的图像的宽度
> - int **getScaledHeight**(Canvas canvas)：获取指定密度转换后的图像的高度
>
> **静态方法**：
>
> - Bitmap **createBitmap**(Bitmap src)：以src为原图生成不可变得新图像
> - Bitmap **createScaledBitmap**(Bitmap src, int dstWidth,int dstHeight, boolean filter)：以src为原图，创建新的图像，指定新图像的高宽以及是否变。
> - Bitmap **createBitmap**(int width, int height, Config config)：创建指定格式、大小的位图
> - Bitmap **createBitmap**(Bitmap source, int x, int y, int width, int height)以source为原图，创建新的图片，指定起始坐标以及新图像的高宽。
> - public static Bitmap **createBitmap**(Bitmap source, int x, int y, int width, int height, Matrix m, boolean filter)

**BitmapFactory.Option**可设置参数：

> - boolean **inJustDecodeBounds**——如果设置为true，不获取图片，不分配内存，但会返回图片的高宽度信息。
> - int **inSampleSize**——图片缩放的倍数。如果设为4，则宽和高都为原来的1/4，则图是原来的1/16。
> - int **outWidth**——获取图片的宽度值
> - int **outHeight**——获取图片的高度值
> - int **inDensity**——用于位图的像素压缩比
> - int **inTargetDensity**——用于目标位图的像素压缩比（要生成的位图）
> - boolean **inScaled**——设置为true时进行图片压缩，从inDensity到inTargetDensity。

## 获取Bitmap

获取位图的方式有两种：通过BitmapDrawable或者BitmapFactory。

### BitmapDrawable

- 创建一个构造BitmapDrawable对象：

  ```java
  BitmapDrawable bmpMeizi = new BitmapDrawable(getAssets().open("pic_meizi.jpg"));
  Bitmap mBitmap = bmpMeizi.getBitmap();
  img_bg.setImageBitmap(mBitmap);
  ```

### BitmapFactory

都是静态方法，可以通过资源ID，路径，文件，数据流等方式获取位图

```java
//通过资源ID
private Bitmap getBitmapFromResource(Resources res, int resId) {
      return BitmapFactory.decodeResource(res, resId);
}

//文件
private Bitmap getBitmapFromFile(String pathName) {
      return BitmapFactory.decodeFile(pathName);
}

//字节数组
public Bitmap Bytes2Bimap(byte[] b) {
    if (b.length != 0) {
        return BitmapFactory.decodeByteArray(b, 0, b.length);
    } else {
        return null;
    }
}

//输入流
private Bitmap getBitmapFromStream(InputStream inputStream) {
      return BitmapFactory.decodeStream(inputStream);
}
```



关于Bitmap相关信息，可以通过调用相关方法获取对应的参数，

## 抠图、缩放、截屏

- 可以直接通过Bitmap的`createBitmap()`扣下来，参数依次为：处理的bitmap对象，起始x，y坐标，以及截取的宽高

```java
Bitmap bitmap1 = BitmapFactory.decodeResource(getResources(), R.mipmap.pic_meizi);
Bitmap bitmap2 = Bitmap.createBitmap(bitmap1,100,100,200,200);
img_bg = (ImageView) findViewById(R.id.img_bg);
img_bg.setImageBitmap(bitmap2);
```

- 可以直接使用Bitmap提供的**createScaledBitmap**来实现，参数依次为：处理的bitmap对象，缩放后的宽高

- 截屏：

  ```java
  public class MainActivity extends AppCompatActivity {
      static ByteArrayOutputStream byteOut = null;
      private Bitmap bitmap = null;
      private Button btn_cut;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          btn_cut = (Button) findViewById(R.id.btn_cut);
          btn_cut.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  captureScreen();
              }
          });
      }
  
      public void captureScreen() {
          Runnable action = new Runnable() {
              @Override
              public void run() {
                  final View contentView = getWindow().getDecorView();
                  try{
                      Log.e("HEHE",contentView.getHeight()+":"+contentView.getWidth());
                      bitmap = Bitmap.createBitmap(contentView.getWidth(),
                              contentView.getHeight(), Bitmap.Config.ARGB_4444);
                      contentView.draw(new Canvas(bitmap));
                      ByteArrayOutputStream byteOut = new ByteArrayOutputStream();
                      bitmap.compress(Bitmap.CompressFormat.JPEG, 100, byteOut);
                      savePic(bitmap, "sdcard/short.png");
                  }catch (Exception e){e.printStackTrace();}
                  finally {
                      try{
                          if (null != byteOut)
                              byteOut.close();
                          if (null != bitmap && !bitmap.isRecycled()) {
  //                            bitmap.recycle();
                              bitmap = null;
                          }
                      }catch (IOException e){e.printStackTrace();}
  
                  }
              }
          };
          try {
              action.run();
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  
      private void savePic(Bitmap b, String strFileName) {
          FileOutputStream fos = null;
          try {
              fos = new FileOutputStream(strFileName);
              if (null != fos) {
                  boolean success= b.compress(Bitmap.CompressFormat.PNG, 100, fos);
                  fos.flush();
                  fos.close();
                  if(success)
                      Toast.makeText(MainActivity.this, "截屏成功", Toast.LENGTH_SHORT).show();
              }
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  }
  ```

# 4. Bitmap引起的OOM问题

**OOM**即（Out Of Memory）内存溢出。Android系统会为每个APP分配一个独立的工作空间， 或者说分配一个单独的Dalvik虚拟机，这样每个APP都可以独立运行而不相互影响！而**Android对于每个 Dalvik虚拟机都会有一个最大内存限制，如果当前占用的内存加上我们申请的内存资源超过了这个限制 ，系统就会抛出OOM错误**！另外，这里别和RAM混淆了，即时当前RAM中剩余的内存有1G多，但是OOM还是会发生！别把RAM(物理内存)和OOM扯到一起！另外RAM不足的话，就是杀应用了，而不是仅仅是OOM了！ 而这个Dalvik中的最大内存标准，不同的机型是不一样的，可以调用：

```java
ActivityManager activityManager = (ActivityManager)context.getSystemService(Context.ACTIVITY_SERVICE);
Log.e("HEHE","最大内存：" + activityManager.getMemoryClass());
```

获得正常的最大内存标准，又或者直接在命令行键入：

```kotlin
adb shell getprop | grep dalvik.vm.heapgrowthlimit
```

你也可以打开系统源码/system/build.prop文件，看下文件中这一部分的信息得出：

```kotlin
dalvik.vm.heapstartsize=8m
dalvik.vm.heapgrowthlimit=192m
dalvik.vm.heapsize=512m
dalvik.vm.heaptargetutilization=0.75
dalvik.vm.heapminfree=2m
dalvik.vm.heapmaxfree=8m
```

**heapstartsize**堆内存的初始大小，**heapgrowthlimit**标准的应用的最大堆 内存大小，**heapsize**则是设置了使用android:largeHeap的应用的最大堆内存大小！

## 避免Bitmap引起的OOM技巧小结

### 采用低内存占用量的编码方式

上一节说了**BitmapFactory.Options**这个类，我们可以设置下其中的inPreferredConfig属性， 默认是**Bitmap.Config.ARGB_8888**，我们可以修改成**Bitmap.Config.ARGB_4444**
Bitmap.Config ARGB_4444：每个像素占四位，即A=4，R=4，G=4，B=4，那么一个像素点占4+4+4+4=16位
Bitmap.Config ARGB_8888：每个像素占八位，即A=8，R=8，G=8，B=8，那么一个像素点占8+8+8+8=32位
默认使用ARGB_8888，即一个像素占4个字节！

### 图片压缩

同样是BitmapFactory.Options，我们通过**inSampleSize**设置缩放倍数，比如写2，即长宽变为原来的1/2，图片就是原来的1/4，如果不进行缩放的话设置为1即可！但是不能一味的压缩，毕竟这个值太小 的话，图片会很模糊，而且要避免图片的拉伸变形，所以需要我们在程序中动态的计算，这个 inSampleSize的合适值，而Options中又有这样一个方法：**inJustDecodeBounds**，将该参数设置为 true后，decodeFiel并不会分配内存空间，但是可以计算出原始图片的长宽，调用 options.**outWidth**/**outHeight**获取出图片的宽高，然后通过一定的算法，即可得到适合的 inSampleSize，这里感谢**街神**提供的代码——摘自鸿洋blog！

```java
public static int caculateInSampleSize(BitmapFactory.Options options, int reqWidth, int reqHeight) {
    int width = options.outWidth;
    int height = options.outHeight;
    int inSampleSize = 1;
    if (width > reqWidth || height > reqHeight) {
        int widthRadio = Math.round(width * 1.0f / reqWidth);
        int heightRadio = Math.round(height * 1.0f / reqHeight);
        inSampleSize = Math.max(widthRadio, heightRadio);
    }
    return inSampleSize;
}
```

然后使用下上述的方法即可：

```java
BitmapFactory.Options options = new BitmapFactory.Options();
options.inJustDecodeBounds = true; // 设置了此属性一定要记得将值设置为false
Bitmap bitmap = null;
bitmap = BitmapFactory.decodeFile(url, options);
options.inSampleSize = computeSampleSize(options,128,128);
options.inPreferredConfig = Bitmap.Config.ARGB_4444;
/* 下面两个字段需要组合使用 */  
options.inPurgeable = true;
options.inInputShareable = true;
options.inJustDecodeBounds = false;
try {
    bitmap = BitmapFactory.decodeFile(url, options);
} catch (OutOfMemoryError e) {
        Log.e(TAG, "OutOfMemoryError");
}
```

### 及时回收图像

如果引用了大量的Bitmap对象，而应用又不需要同时显示所有图片。可以将暂时不用到的Bitmap对象 及时回收掉。对于一些明确知道图片使用情况的场景可以主动recycle回收，比如引导页的图片，使用 完就recycle，帧动画，加载一张，画一张，释放一张！使用时加载，不显示时直接置null或recycle！ 比如：imageView.setImageResource(0); 不过某些情况下会出现特定图片反复加载，释放，再加载等，低效率的事情...

### 其他方法

下面这些方法，我并没有用过，大家可以自行查阅相关资料：

#### 简单通过SoftReference引用方式管理图片资源

建个SoftReference的hashmap 使用图片时先查询这个hashmap是否有softreference， softreference里的图片是否为空， 如果为空就加载图片到softreference并加入hashmap。 无需再代码里显式的处理图片的回收与释放，gc会自动处理资源的释放。 这种方式处理起来简单实用，能一定程度上避免前一种方法反复加载释放的低效率。但还不够优化。

**示例代码：**

```java
private Map<String, SoftReference<Bitmap>> imageMap 
                                           = new HashMap<String, SoftReference<Bitmap>>();

public Bitmap loadBitmap(final String imageUrl,final ImageCallBack imageCallBack) {
    SoftReference<Bitmap> reference = imageMap.get(imageUrl);
    if(reference != null) {
        if(reference.get() != null) {
            return reference.get();
        }
    }
    final Handler handler = new Handler() {
        public void handleMessage(final android.os.Message msg) {
            //加入到缓存中
            Bitmap bitmap = (Bitmap)msg.obj;
            imageMap.put(imageUrl, new SoftReference<Bitmap>(bitmap));
            if(imageCallBack != null) {
                imageCallBack.getBitmap(bitmap);
            }
        }
    };
    new Thread(){
        public void run() {
            Message message = handler.obtainMessage();
            message.obj = downloadBitmap(imageUrl);
            handler.sendMessage(message);
        }
    }.start();
    return null ;
}

// 从网上下载图片
private Bitmap downloadBitmap (String imageUrl) {
    Bitmap bitmap = null;
    try {
        bitmap = BitmapFactory.decodeStream(new URL(imageUrl).openStream());
        return bitmap ;
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    } 
}
public interface ImageCallBack{
    void getBitmap(Bitmap bitmap);
}
```

#### LruCache + sd的缓存方式

> Android 3.1版本起，官方还提供了LruCache来进行cache处理，当存储Image的大小大于LruCache 设定的值，那么近期使用次数最少的图片就会被回收掉，系统会自动释放内存！

**使用示例**：

步骤：

1）要先设置缓存图片的内存大小，我这里设置为手机内存的1/8, 手机内存的获取方式：int MAXMEMONRY = (int) (Runtime.getRuntime() .maxMemory() / 1024);

2）LruCache里面的键值对分别是URL和对应的图片

3）重写了一个叫做sizeOf的方法，返回的是图片数量。

```java
private LruCache<String, Bitmap> mMemoryCache;
private LruCacheUtils() {
    if (mMemoryCache == null)
        mMemoryCache = new LruCache<String, Bitmap>(
                MAXMEMONRY / 8) {
            @Override
            protected int sizeOf(String key, Bitmap bitmap) {
                // 重写此方法来衡量每张图片的大小，默认返回图片数量。
                return bitmap.getRowBytes() * bitmap.getHeight() / 1024;
            }

            @Override
            protected void entryRemoved(boolean evicted, String key,
                    Bitmap oldValue, Bitmap newValue) {
                Log.v("tag", "hard cache is full , push to soft cache");
               
            }
        };
}
```

4）下面的方法分别是清空缓存、添加图片到缓存、从缓存中取得图片、从缓存中移除。

移除和清除缓存是必须要做的事，因为图片缓存处理不当就会报内存溢出，所以一定要引起注意。

```java
public void clearCache() {
    if (mMemoryCache != null) {
        if (mMemoryCache.size() > 0) {
            Log.d("CacheUtils",
                    "mMemoryCache.size() " + mMemoryCache.size());
            mMemoryCache.evictAll();
            Log.d("CacheUtils", "mMemoryCache.size()" + mMemoryCache.size());
        }
        mMemoryCache = null;
    }
}

public synchronized void addBitmapToMemoryCache(String key, Bitmap bitmap) {
    if (mMemoryCache.get(key) == null) {
        if (key != null && bitmap != null)
            mMemoryCache.put(key, bitmap);
    } else
        Log.w(TAG, "the res is aready exits");
}

public synchronized Bitmap getBitmapFromMemCache(String key) {
    Bitmap bm = mMemoryCache.get(key);
    if (key != null) {
        return bm;
    }
    return null;
}

/**
 * 移除缓存
 * 
 * @param key
 */
public synchronized void removeImageCache(String key) {
    if (key != null) {
        if (mMemoryCache != null) {
            Bitmap bm = mMemoryCache.remove(key);
            if (bm != null)
                bm.recycle();
        }
    }
}
```