

# 1 常用控件的使用方法

**常见单位介绍**：

- **dp(dip):** device independent pixels(设备独立像素). 不同设备有不同的显示效果,这个和设备硬件有关，一般我们为了支持WVGA、HVGA和QVGA 推荐使用这个，不依赖像素。
-  **px: pixels(像素)**. 不同设备显示效果相同，一般我们HVGA代表320x480像素，这个用的比较多。
- **pt: point**，是一个标准的长度单位，1pt＝1/72英寸，用于印刷业，非常简单易用
-  **sp: scaled pixels(放大像素)**. 主要用于字体显示**best for textsize**。

## TextView

该控件主要用于在界面上显示一段文本信息

- Android中所有的控件都具有`layout_width`和`layout_height`这两个属性，可选值有3种：match_parent、wrap_content、fill_parent。TextView默认是居左上角对齐。
- 可以使用**android:gravity**来指定文字的对齐方式，可选值有：top、bottom、left、right、center等。可以用**|**来同时指定多个值
- 可以对TextView中文字的大小，颜色进行修改：通过**android:textSize**可以指定文字的大小；通过**android:textColor**可以指定文字的颜色，字体的大小使用sp作为单位。

### 实际中常见的开发案例：

### 带阴影的TextView

- **android:shadowColor:**设置阴影颜色,需要与shadowRadius一起使用
- **android:shadowRadius:**设置阴影的模糊程度,设为0.1就变成字体颜色了,建议使用3.0
- **android:shadowDx:**设置阴影在水平方向的偏移,就是水平方向阴影开始的横坐标位置
- **android:shadowDy:**设置阴影在竖直方向的偏移,就是竖直方向阴影开始的纵坐标位置

### 带边框的TextView

为TextView加边框。须要在**drawable建xml文件**，里面设置shape来设置文本框的特殊效果，然后将blackground设置为这个drawable资源。

- <**solid** android:color = "xxx"> 这个是设置背景颜色的
- <**stroke** android:width = "xdp" android:color="xxx"> 这个是设置边框的粗细,以及边框颜色的
- <**padding** androidLbottom = "xdp"...> 这个是设置边距的
- <**corners** android:topLeftRadius="10px"...> 这个是设置圆角的
- <**gradient**> 这个是设置渐变色的,可选属性有: **startColor**:起始颜色 **endColor**:结束颜色 **centerColor**:中间颜色 **angle**:方向角度,等于0时,从左到右,然后逆时针方向转,当angle = 90度时从下往上 **type**:设置渐变的类型

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- 设置透明背景色 -->
    <solid android:color="#87CEEB" />

    <!-- 设置一个黑色边框 -->
    <stroke
        android:width="2px"
        android:color="#000000" />
    <!-- 设置四个圆角的半径 -->
    <corners
        android:bottomLeftRadius="10px"
        android:bottomRightRadius="10px"
        android:topLeftRadius="10px"
        android:topRightRadius="10px" />
    <!-- 设置一下边距,让空间大一点 -->
    <padding
        android:bottom="5dp"
        android:left="5dp"
        android:right="5dp"
        android:top="5dp" />
</shape>
```

### 带图片（drawableXxx）的Textview

![](http://www.runoob.com/wp-content/uploads/2015/07/68693829.jpg)

- 要实现上面这种功能，可以使用**drawableXxx**可以省略上面过程，可以设置四个方向的图片**drawableTop**(上), **drawableButtom**(下), **drawableLeft**(左),  **drawableRight**(右) 。另外,你也可以使用**drawablePadding**来设置图片与文字间的间距。
- 在XML中是无法直接设置drawable大小，需要在Java代码中进行修改。

```java
public class MainActivity extends AppCompatActivity {  
    private TextView txtZQD;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        txtZQD = (TextView) findViewById(R.id.txtZQD);  
        Drawable[] drawable = txtZQD.getCompoundDrawables();  
        // 数组下表0~3,依次是:左上右下  
        drawable[1].setBounds(100, 0, 200, 200);  
        txtZQD.setCompoundDrawables(drawable[0], drawable[1], drawable[2],  
                drawable[3]);  
    }  
```

- ①**Drawable[] drawable = txtZQD.getCompoundDrawables( );** 获得四个不同方向上的图片资源,数组元素依次是:左上右下的图片
- ②**drawable[1].setBounds(100, 0, 200, 200);** 接着获得资源后,可以调用setBounds设置左上右下坐标点,比如这里设置了代表的是: 长是:从离文字最左边开始100dp处到200dp处 宽是:从文字上方0dp处往上延伸200dp!
- ③**txtZQD.setCompoundDrawables(drawable[0], drawable[1], drawable[2], drawable[3]);**为TextView重新设置drawable数组!没有图片可以用null代替哦! PS：另外，从上面看出我们也可以直接在Java代码中调用setCompoundDrawables为 TextView设置图片！

### TextView玩转HTML

### SpannableString&SpannableStringBuilder定制文本

### 跑马灯效果TextView

```xml
android:singleLine="true"
        android:ellipsize="marquee"
        android:marqueeRepeatLimit="marquee_forever"
        android:focusable="true"
        android:focusableInTouchMode="true"
```

在XML定义了上述属性，还需要在java代码中进行程序控制。

```java
TextView mTextView = (TextView) findViewById(R.id.myTextView);

        // 开始走马灯效果
        mTextView.setSelected(true);

        // 停止走马灯效果
        // mTextView.setSelected(false);
```

### 设置TextView字间距与行间距

**字间距：**

```xml
android:textScaleX：控制字体水平方向的缩放，默认值1.0f，值是float
Java中setScaleX(2.0f); 
垂直方向间距为android:textScaleY
```

**行间距：** Android系统中TextView默认显示中文时会比较紧凑，为了让每行保持的行间距

> **android:lineSpacingExtra：**设置行间距，如"3dp" **android:lineSpacingMultiplier：**设置行间距的倍数，如"1.2"

Java代码中可以通过: **setLineSpacing**方法来设置



### 使用autoLink属性识别链接类型

当文字中出现了**URL，E-Mail，电话号码，地图**的时候，我们可以通过设置autoLink属性；当我们点击 文字中对应部分的文字，即可跳转至某默认APP，比如一串号码，点击后跳转至拨号界面！



相关属性使用时查阅文档或者[TextView其他功能实践](http://www.runoob.com/w3cnote/android-tutorial-textview.html)

## Button

Button是程序用于和用户交互的重要控件，可配置的属性与TextView相似。

- 系统会对Button中的所有英文字母自动进行大写转换，可以使用如下配置禁止这一默认特性：

  ```xml
  android:textAllCaps="false"
  ```

- 之后可以为Button注册一个监听器，每当点击按钮就会执行监听器的onClick()方法。前述文档都是采用匿名类的方式注册监听器，也可以使用接口的方式。

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button button = (Button) findViewById(R.id.button);
          button.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.button:
                  break;
              default:
                  break;
          }
      }
  }
  ```

### StateListDrawable简介

StateListDrawable是Drawable资源的一种，可以根据不同的状态，设置不同的图片效果，关键节点 **< selector >**，我们只需要将Button的background属性设置为该drawable资源即可轻松实现，按下 按钮时不同的按钮颜色或背景！

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true" android:drawable="@drawable/ic_course_bg_fen"/>
    <item android:state_enabled="false" android:drawable="@drawable/ic_course_bg_pressed"/>
    <item android:drawable="@drawable/ic_course_bg_cheng"/>
</selector>
```



其他属性查阅文档或[相关实践](http://www.runoob.com/w3cnote/android-tutorial-button-imagebutton.html)

## EditText

改控件允许用户在控件里输入和编辑内容，并可以在程序中对这些内容进行处理。例如：短信、微信、QQ等操作。

如前述控件所示，在xml文件中定义该控件。

- **Android中的控件规律**：给控件定义一个id，再指定控件的宽度和高度，再适当加入一些控件特有的属性。

- 输入框中如果想显示一些提示性的文字，可以使用**android:hint**来指定提示的内容

- 如果输入的内容过多会导致EditText不断拉长，界面变丑，可以通过使用**android:maxLines**属性指定最大行数，输入内容超过两行，文本向上滚动。

- EditText可以和Button相结合完成一些功能

  示例：

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      private EditText editText;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button button = (Button) findViewById(R.id.button);
          editText = (EditText) findViewById(R.id.edit_text);
          button.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.button:
                  String inputText = editText.getText().toString();
                  Toast.makeText(MainActivity.this, inputText, Toast.LENGTH_SHORT).show();
                  break;
              default:
                  break;
          }
      }
  }
  ```

### 全选文本与限制输入类型

- 点击输入框，获得焦点，获得输入框全部本文内容使用`android:selectAllOnFocus`：

```xml
android:selectAllOnFocus="true"
```

- 限制输入类型可以通过inputType属性来实现

  ```
  android:inputType="phone"
  ```

- **android:capitalize**:提供英文字母大写类型

  - **sentences：**仅第一个字母大写
  - **words：**每一个单词首字母大小，用空格区分单词
  - **characters:**每一个英文字母都大写

其他功能查阅API或者[EditText其他实践](http://www.runoob.com/w3cnote/android-tutorial-edittext.html)



## ImageView

ImageView是用于在界面上展示图片的一个控件。图片通常都放在以drawable开头的目录下。

```xml
<ImageView
        android:id="@+id/image_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/img_1" />
```

其中使用**android:src**属性给ImageView指定了一张图片，由于宽高未知，最好选择wrap_content。

------

在API文档中我们发现ImageView有两个可以设置图片的属性，分别是：src和background

**常识：**

①background通常指的都是**背景**,而src指的是**内容**!!

②当使用**src**填入图片时,是按照图片大小**直接填充**,并**不会进行拉伸**

而使用background填入图片,则是会根据ImageView给定的宽度来进行**拉伸**

------

可以在程序中通过代码动态地更改ImageView中的图片。

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private EditText editText;
    private ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = (Button) findViewById(R.id.button);
        editText = (EditText) findViewById(R.id.edit_text);
        imageView = (ImageView) findViewById(R.id.image_view);
        button.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button:
                imageView.setImageResource(R.drawable.img_2);
                break;
            default:
                break;
        }
    }
}

```

- `android:scaleType`：用于设置显示的图片如何缩放或者移动适应ImageView的大小，Java代码中可以通过`imageView.setScaleType(ImageView.ScaleType.CENTER);`设置。



## ProgressBar

用于在界面上显示进度条。

- 所有的Android控件都有可见属性，可以通过**android:visibility**进行指定，可选值有3种：**visible**、**invisible**和**gone**。

  - visible：可见默认状态，不指定时，控件都是可见的
  - invisible：控件不可见，它仍然占据着原来的位置和大小，只是变为透明了。
  - gone：控件不仅不可见，还不占用任何屏幕空间。

- 还可以通过代码来设置控件可见性，通过`setVisibility()`，可以传入**View.VISIBLE**、**View.INVISIBLE**和**View.GONE**这3个值。

- 可以通过`getVisibility()`方法来判断ProgressBar是否可见

- 可以给ProgressBar指定不同的样式。默认是圆形进度条，通过style属性可以将它指定成水平进度条。

  ```xml
   style="?android:attr/progressBarStyleHorizontal"
          android:max="100"
  ```

  max属性给进度条设置了一个最大值

- 动态更改进度条进度：

  ```java
   @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.button:
                  imageView.setImageResource(R.drawable.img_2);
                  int progress = progressBar.getProgress();
                  progress = progress + 10;
                  progressBar.setProgress(progress);
                  break;
              default:
                  break;
          }
      }
  ```


其他样式可查阅相关文档或者[相关实践](http://www.runoob.com/w3cnote/android-tutorial-progressbar.html)



## AlertDialog

该控件可以在当前界面弹出一个对话框，这个对话框置顶于所有界面元素之上，屏蔽掉其他控件的交互能力。一般用于提示一些非常重要的内容或警告信息。

```java
 @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button:
                AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
                dialog.setTitle("This is Dialog");
                dialog.setMessage("Something important.");
                dialog.setCancelable(false);
                dialog.setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {

                    }
                });
                dialog.setNegativeButton("Cancle", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {

                    }
                });
                dialog.show();
                break;
            default:
                break;
        }
    }
```

通过AlterDialog.Builder创建一个实例，为这个对话框设置标题、内容、可否取消等属性，接下来调用`setPositiveButton()`方法为对话框设置确定按钮的点击事件，调用`setNegativeButton()`方法为对话框设置取消按钮点击事件，最后调用`show()`方法将对话框显示出来。



## ProgressDialog

与前者类似，但是其会在对话框中显示一个进度条，用于表示当前操作比较耗时，让用户耐心等待。（最新android版本中该控件**已废弃**）

## Adapter

![](http://www.runoob.com/wp-content/uploads/2015/09/11147289.jpg)

**图解**：

- **Model**：通常可以理解为数据,负责执行程序的核心运算与判断逻辑,,通过view获得用户 输入的数据,然后根据从数据库查询相关的信息,最后进行运算和判断,再将得到的结果交给view来显示
- **view**:用户的操作接口,说白了就是GUI,应该使用哪种接口组件,组件间的排列位置与顺序都需要设计
- **Controller**:控制器,作为model与view之间的枢纽,负责控制程序的执行流程以及对象之间的一个互动

**Adapter则是中间的这个Controller的部分**： **Model**(数据) ---> **Controller**(以什么方式显示到)---> **View**(用户界面) 这就是简单MVC组件的简单理解！

![](http://www.runoob.com/wp-content/uploads/2015/09/77919389.jpg)

- **BaseAdapter**：抽象类，实际开发中我们会继承这个类并且重写相关方法，用得最多的一个Adapter
- **ArrayAdapter**：支持泛型操作，最简单的一个Adapter，**只能展现一行文字~**
- **SimpleAdapter**：同样具有良好扩展性的一个Adapter，可以**自定义**多种效果
- **SimpleCursorAdapter**：用于显示简单文本类型的**listView**，一般在数据库那里会用到，不过有点过时， **不推荐使用**！

（Adapter一般需要结合ListView，GridView等控件）

### ArrayAdatper

代码示例：

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //要显示的数据
        String[] strs = {"基神","B神","翔神","曹神","J神"};
        //创建ArrayAdapter
        ArrayAdapter<String> adapter = new ArrayAdapter<String>
                (this,android.R.layout.simple_expandable_list_item_1,strs);
        //获取ListView对象，通过调用setAdapter方法为ListView设置Adapter设置适配器
        ListView list_test = (ListView) findViewById(R.id.list_test);
        list_test.setAdapter(adapter);
    }
}
```

除了上述用java代码创建，还可以写到一个数组资源文件中：

- 在res/valuse下创建一个数组资源的xml文件，例如**arrays.xml**

```xml
<resources>
    <string-array name="myarray">
        <item>语文</item>
        <item>数学</item>
        <item>英语</item>
        <item>体育</item>
    </string-array>
</resources>
```

接着布局的ListView设置：

```xml
<ListView
    android:id="@+id/list_test"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:entries="@array/myarray" />
```

或不在布局的LIstView中设置，可以在java代码中添加：

```java
ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,
        R.array.myarray,android.R.layout.simple_expandable_list_item_1);
ListView list_test = (ListView) findViewById(R.id.list_test);
list_test.setAdapter(adapter);
```

- 最开始的java代码写法也可以改为：

```java
List<String> data = new ArrayList<String>();
data.add("基神");
data.add("B神")；
ArrayAdapter<String> adapter = new ArrayAdapter<String>
                (this,android.R.layout.simple_expandable_list_item_1,data);
```

- 前述实例化ArrayAdapter的第二个参数：android.R.layout.simple_expandable_list_item_1等都为系统给定的模版
  - **simple_list_item_1** : 单独一行的文本框 
  - **simple_list_item_2** : 两个文本框组成
  - **simple_list_item_checked** : 每项都是由一个已选中的列表项 
  - **simple_list_item_multiple_choice** : 都带有一个复选框 
  -  **simple_list_item_single_choice** : 都带有一个单选钮 



### SimpleAdapter

- 首先编写一个列表项目每一项的布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <!-- 定义一个用于显示头像的ImageView -->
    <ImageView
        android:id="@+id/imgtou"
        android:layout_width="64dp"
        android:layout_height="64dp"
        android:baselineAlignBottom="true"
        android:paddingLeft="8dp" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:id="@+id/name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingLeft="8dp"
            android:textColor="#1D1D1C"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/says"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingLeft="8px"
            android:textColor="#B4B4B9"
            android:textSize="14sp" />

    </LinearLayout>
</LinearLayout>
```

- 接着修改MainActivity.java

```java
public class MainActivity extends AppCompatActivity {

    private String[] names = new String[]{"B神", "基神", "曹神"};
    private String[] says = new String[]{"无形被黑，最为致命", "大神好厉害~", "我将带头日狗~"};
    private int[] imgIds = new int[]{R.mipmap.ic_launcher, R.mipmap.ic_launcher, R.mipmap.ic_launcher};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
         List<Map<String, Object>> listitem = new ArrayList<Map<String, Object>>();
        for (int i = 0; i < names.length; i++) {
            Map<String, Object> showitem = new HashMap<String, Object>();
            showitem.put("touxiang", imgIds[i]);
            showitem.put("name", names[i]);
            showitem.put("says", says[i]);
            listitem.add(showitem);
        }

        //创建一个simpleAdapter
        SimpleAdapter myAdapter = new SimpleAdapter(getApplicationContext(), listitem, R.layout.list_item, new String[]{"touxiang", "name", "says"}, new int[]{R.id.imgtou, R.id.name, R.id.says});
        ListView listView = (ListView) findViewById(R.id.list_test);
        listView.setAdapter(myAdapter);
    }
}
```

SimpleAdapter只有一个构造函数，签名为：

```java
public SimpleAdapter (Context context, List<? extends Map<String, ?>> data, int resource, String[] from, int[] to)
```

- data表示的是List数据源，其中List中的元素都是Map类型，并且Map的key是String类型，Map的value可以是任意类型，我们一般使用`HashMap<String, Object>`作为List中的数据项。
- resource表示数据项UI所对应的layout文件，在本例中即R.layout.item。在本例中，每条数据项都要包含图片、名称、描述三条信息，所以我们在item.xml中定义了一个ImageView表示图片，两个TextView分别表示名称和描述，并且都设置了ID值。
- 每个数据项对应一个Map，from表示的是Map中key的数组。
- 数据项Map中的每个key都在layout中有对应的View，to表示数据项对应的View的ID数组。

### SimpleCursorAdapter

相关内容查阅API文档或者[相关实践](http://www.runoob.com/w3cnote/android-tutorial-adapter.html)与[详解](https://blog.csdn.net/iispring/article/details/50793455)













# 2 详解4种布局

![](E:\document\photo\first-code\布局与控件的关系图.jpg)

## 线性布局（LinearLayout）

**LinearLayout**又称线性布局，这个布局会将它所包含的控件在线性方向上向上一次排列。

- 通过**android:orientation**属性指定了排列方向。如果不指定，默认的排列方向是**horizontal**

- 如果排列方向是horizontal，内部的控件就不能将宽度指定为match_parent。同理，如果排列方向是vertical，内部控件就不能将高度指定为match_parent。

- **android:layout_gravity**用于指定控件在布局中的对齐方式。排列方向是horizontal时，只有垂直方向上的对齐方式才会生效，排列方向为vertical时同理。

- **android:layout_weight**该属性允许我们使用比例的方式来指定控件的大小

  ```xml
  <EditText
          android:id="@+id/input_message"
          android:layout_width="0dp"
          android:layout_height="wrap_content"
          android:layout_weight="1"
          android:hint="type something" />
  
  
      <Button
          android:id="@+id/send"
          android:layout_height="wrap_content"
          android:layout_width="0dp"
          android:layout_weight="1"
          android:text="send" />
  ```

  由于使用了**android:layout_weight**，控件的宽度不再由**android:layout_width**决定，指定为0dp只是一种规范的写法。**dp是Android中用于指定控件大小，间距等属性的单位。**

  这里属性指定为1，表示平分屏幕宽度。系统会将所有控件的该值相加，然后每个控件所占的比例就是该控件的值除以总值。

- 如果没有指定**android:layout_weight**，则控件大小仍按规定计算，剩余的空间给指定了该属性的控件分配。

如果想要为LinearLayout设置分割线，可以直接在布局中添加一个view

```xml
<View  
    android:layout_width="match_parent"  
    android:layout_height="1px"  
    android:background="#000000" /> 
```



## 相对布局

**RelativeLayout**又称相对布局。可以通过相对定位的方式让控件出现在布局的任何位置。

![](http://www.runoob.com/wp-content/uploads/2015/07/797932661-1.png)

![](http://www.runoob.com/wp-content/uploads/2015/07/44967125.jpg)

```xml
 <Button
        android:id="@+id/button_1"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:text="button 1" />

    <Button
        android:id="@+id/button_2"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_centerInParent="true"
        android:text="button 2" />
```

上述例子是相对于父布局进行定位的，控件也可以相对于控件进行定位的：

```xml
<Button
        android:id="@+id/button_1"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_centerInParent="true"
        android:text="button 1" />

    <Button
        android:id="@+id/button_2"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_above="@id/button_1"
        android:layout_toLeftOf="@id/button_1"
        android:text="button 2" />
```

其他属性可查阅文档



**注意**：

**margin**代表的是偏移,比如marginleft = "5dp"表示组件离容器左边缘偏移5dp; 而**padding**代表的则是填充,而填充的对象针对的是组件中的元素,比如TextView中的文字 比如为TextView设置paddingleft = "5dp",则是在组件里的元素的左边填充5dp的空间！ **<font color=red>margin针对的是容器中的组件，而padding针对的是组件中的元素</font>>**



## 帧布局

FrameLayout又称作帧布局。**所有的控件都会默认摆放在布局的左上角**。

除了默认的效果之外，还可以使用**android:layout_gravity**属性指定控件在布局的对齐方向。

```xml
<Button
        android:id="@+id/button_2"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:layout_gravity="bottom"
        android:text="button 2" />
```

- **android:foreground**:设置帧布局容器的前景图像
- **android:foregroundGravity**：设置帧布局容器前景图像的显示位置

（**前景图像**即永远处于帧布局最上面，不会被覆盖的图片。）

## 百分比布局（在API_26中已废弃）

前述三种版本只有**LinearLayout**支持用**layout_weight**事项按比例指定控件大小的功能。Android引入了全行的布局方式——百分比布局，可以直接指定控件在布局中所占的百分比。

- 百分比布局为FrameLayout和RelativeLayout进行功能扩展，提供了**PercentFramelayout**和**PercentRelativeLayout**两种全新布局。



# 3 自定义控件

![常用控件和布局的继承结构](E:\document\photo\first-code\常用控件和布局的继承结构.jpg)

控件都是直接或间接继承自View的，所用的布局都是直接或间接继承自ViewGroup。**View是Android中最基本的一种UI组件，可以在屏幕上绘制一块矩形区域，并响应这块区域的各种事件**。**ViewGroup**是一种特殊的View，可以包含很多子View和子ViewGroup，是一个用于放置控件和布局的容器。当自带控件无法满足需求，可以自定义控件。

## 引入布局

如果程序中可能有多个活动需要一样的标题啥的，可以使用引入布局的方式来解决这个问题。

示例：新建一个布局`title.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/title_bg">

    <Button
        android:id="@+id/title_back"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:background="@drawable/back_bg"
        android:text="Back"
        android:textColor="#fff" />

    <TextView
        android:id="@+id/title_text"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Title Text"
        android:textColor="#fff"
        android:textSize="24sp" />

    <Button
        android:id="@+id/title_edit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:background="@drawable/edit_bg"
        android:text="Edit"
        android:textColor="#fff" />

</LinearLayout>
```

- **android:backgroud**：用于为布局或控件指定一个背景，可以使用颜色或图片进行填充。
- **android:layout_margin**：可以指定控件在上下左右方向上偏移的距离
- 可以使用**android:layout_marginleft**或**android:layout_marginTop**等属性单独指定控件在某个方向上偏移的距离。

修改activity_main.xml中的代码：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

   <include layout="@layout/title" />

</LinearLayout>
```

通过一行include语句将标题栏布局引入进来。

在**MainActivity**中将系统自带的标题栏隐藏掉

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ActionBar actionbar = getSupportActionBar();
        if (actionbar != null) {
            actionbar.hide();
        }
    }
```



## 创建自定义控件

如果布局中有一些控件要求能够响应事件，前述方法还是需要在每个活动中为这些控件单独编写一次事件注册的代码。如果功能相同，最好使用自定义控件的方式解决。

新建TitleLayout继承LinearLayout，让其成为自定义的标题栏控件：

```java
public class TitleLayout extends LinearLayout {

    public TitleLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        LayoutInflater.from(context).inflate(R.layout.title, this);
    }
}
```

在构造函数中对标题栏布局进行动态加载，借助**LayoutInflater**实现，其`from()`方法可以构建出一个LayoutInflater对象，调用其`inflat()`方法可以动态加载一个布局文件，，其接收两个参数，一个要加载布局文件的Id，另一个是给加载好的布局再添加一个父布局。

然后在布局文件中添加这个自定义控件

```xml
<com.vivo.a11085273.uicustomviews.TitleLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

添加自定义控件需要指明控件的完整类名。

修改TitleLayout中代码：

```java
public class TitleLayout extends LinearLayout {

    public TitleLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        LayoutInflater.from(context).inflate(R.layout.title, this);
        Button titleBack = (Button) findViewById(R.id.title_back);
        Button titleEdit = (Button) findViewById(R.id.title_edit);
        titleBack.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                ((Activity) getContext()).finish();
            }
        });
        titleEdit.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(getContext(), "You Clicked Edit button", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

# 4 ListView

当程序中有大量的数据需要展示时，可以借助ListView实现。

## 简单用法

```xml
<ListView
    android:id="@+id/list_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

```java
public class MainActivity extends AppCompatActivity {

    private String[] data = { "Apple", "Banana", "Orange", "Watermelon", "Pear", "Grape",
    "Pineapple", "Strawberry", "Cherry", "Mango", "Apple", "Banana", "Orange", "Watermelon",
            "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango",};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(
                MainActivity.this, android.R.layout.simple_list_item_1,data);
        ListView listView = (ListView) findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }
}
```

**数组中的数据无法直接传递给ListView，需要借助适配器**。例如**ArrayAdapter**。可以通过泛型指定要适配的数据类型，在构造器中把要适配的数据传入。然后依次传入当前上下文，ListView子项布局的id，要适配的数据。**android.R.layout.simple_list_item_1**是Android内置的布局文件，里面只有一个TextView，用于简单的显示一段文本。

最后调用ListView的`setAdapter()`方法，将构建好的适配器对象传递进去。



## 定制ListView界面

准备号一组图片，之后定义一个实体类，作为ListView适配器的适配类型。新建类Fruit

```java
public class Fruit {

    private String name;
    private int imageId;

    public Fruit(String name, int imageId) {
        this.name = name;
        this.imageId = imageId;
    }

    public String getName() {
        return name;
    }

    public int getImageId() {
        return imageId;
    }
}
```

为ListView子项指定一个自定义布局，新建fruit_item.xml

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <ImageView
        android:id="@+id/fruit_image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <TextView
        android:id="@+id/fruit_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:layout_marginLeft="10dp" />

</LinearLayout>
```

创建一个自定义适配器，继承ArrayAdapter。新建类FruitAdapter，

```java
public class FruitAdapter extends ArrayAdapter<Fruit> {

    private int resourceId;

    public FruitAdapter(Context context, int textViewResourceId, List<Fruit> objects) {
        super(context, textViewResourceId, objects);
        resourceId = textViewResourceId;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Fruit fruit = getItem(position);
        View view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
        TextView fruitName = (TextView) view.findViewById(R.id.fruit_name);
        fruitImage.setImageResource(fruit.getImageId());
        fruitName.setText(fruit.getName());
        return view;
    }
}
```

或者自定义适配器，继承BaseAdapter。新建类FruitBaseAdapter，

```java
public class FruitBaseAdapter extends BaseAdapter {

//    private Context context;
    private List<Fruit> list;//数据源
    private LayoutInflater inflater; //初始化ListView每个item的布局文件

    public FruitBaseAdapter(Context context, List<Fruit> objects) {
        this.list = objects;
        this.inflater = LayoutInflater.from(context);
    }
	//得到总的数量
    @Override
    public int getCount() {
        return this.list.size();
    }
	
    //根据ListView位置返回View
    @Override
    public Object getItem(int position) {
        return this.list.get(position);
    }
	//根据ListView位置得到List中的ID
    @Override
    public long getItemId(int position) {
        return position;
    }
	//根据位置得到View对象
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Fruit fruit =(Fruit) getItem(position);
        View view;
        ViewHolder viewHolder;
        if (convertView == null) {
            //解析布局
            convertView = inflater.inflate(R.layout.fruit_item, null);
            //创建ViewHolder持有类
            viewHolder = new ViewHolder();
            //将每个控件的对象都保存到持有类中
            viewHolder.fruitImage = (ImageView) convertView.findViewById(R.id.fruit_image);
            viewHolder.fruitName = (TextView) convertView.findViewById(R.id.fruit_name);
            //将每个convertView对象中设置这个持有类对象
            convertView.setTag(viewHolder);
        }
        //需要使用时拿到这个持有类
        viewHolder = (ViewHolder) convertView.getTag();
        //直接使用这个类中的控件
        viewHolder.fruitImage.setImageResource(fruit.getImageId());
        viewHolder.fruitName.setText(fruit.getName());
        return convertView;

    }

    //保存当前所有控件
     private class ViewHolder {
        ImageView fruitImage;
        TextView fruitName;
    }
}
```

`getView()`方法里在每个子项被滚动到屏幕内都会被调用。首先通过`getItem()`方法得到当前项的Fruit实例，然后使用LayoutInflater为这个子项加载我们传入的布局。



不能为View添加父局，否则不能再添加到ListView中。调用View的`findViewById()`方法获取ImageView和TextView示例，调用其方法`setImageResource()`和`setText()`设置显示的图片和文字。最后将布局返回。

最后修改MainActivity代码：

```java
public class MainActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits(); // 初始化水果数据
        FruitAdapter adapter = new FruitAdapter(MainActivity.this, R.layout.fruit_item, fruitList);
        //FruitBaseAdapter adapter = new FruitBaseAdapter(MainActivity.this,  fruitList);
        ListView listView = (ListView) findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }

    private void initFruits() {
        for (int i = 0; i < 2; i++) {
            Fruit apple = new Fruit("Apple", R.drawable.apple_pic);
            fruitList.add(apple);
            Fruit banana = new Fruit("Banana", R.drawable.banana_pic);
            fruitList.add(banana);
            Fruit orange = new Fruit("Orange", R.drawable.orange_pic);
            fruitList.add(orange);
            Fruit watermelon = new Fruit("Watermelon", R.drawable.watermelon_pic);
            fruitList.add(watermelon);
            Fruit pear = new Fruit("Pear", R.drawable.pear_pic);
            fruitList.add(pear);
            Fruit grape = new Fruit("Grape", R.drawable.grape_pic);
            fruitList.add(grape);
            Fruit pineapple = new Fruit("Pineapple", R.drawable.pineapple_pic);
            fruitList.add(pineapple);
            Fruit strawberry = new Fruit("Strawberry", R.drawable.strawberry_pic);
            fruitList.add(strawberry);
            Fruit cherry = new Fruit("Cherry", R.drawable.cherry_pic);
            fruitList.add(cherry);
            Fruit mango = new Fruit("Mango", R.drawable.mango_pic);
            fruitList.add(mango);
        }
    }

}
```

如果需要定制更加复杂的界面，只需要修改fruit_item.xml中的内容。

## 提升ListView运行效率

前述的FruitAdapter的`getView()`方法，每次都将布局重新加载一遍，如果ListView快速滚动，则会成为性能瓶颈。

`getView()`中还有一个convertView参数，用于将之前加载好的布局进行缓存，之后可以进行重用。修改FruitAdapter中代码：

```java
@Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Fruit fruit = getItem(position);
        View view;
        if (convertView == null) {
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        } else {
            view = convertView;
        }
        ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
        TextView fruitName = (TextView) view.findViewById(R.id.fruit_name);
        fruitImage.setImageResource(fruit.getImageId());
        fruitName.setText(fruit.getName());
        return view;
    }
```

前述代码中每次在`getView()`方法中还是会调用View的`findViewById()`方法来获取一次控件的实例。可以借助ViewHolder进行优化。

```java
@Override
    public View getView(int position, View convertView, ViewGroup parent) {
        Fruit fruit = getItem(position);
        View view;
        ViewHolder viewHolder;
        if (convertView == null) {
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
            viewHolder.fruitName = (TextView) view.findViewById(R.id.fruit_name);
            view.setTag(viewHolder);
        } else {
            view = convertView;
            viewHolder = (ViewHolder) view.getTag();
        }
        viewHolder.fruitImage.setImageResource(fruit.getImageId());
        viewHolder.fruitName.setText(fruit.getName());
        return view;
    }

    class ViewHolder {
        ImageView fruitImage;
        TextView fruitName;
    }
```

新增内部类ViewHolder对控件实例进行缓存。当convertView == null，创建该对象，存放控件的实例，然后调用View的`setTag()`方法，将ViewHolder对象存储在View中。当convertView不为null，调用`getTag()`方法，把ViewHoler重新取出来。



## ListView点击事件

修改MainActivity中代码：

```java
listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view,
                                    int position, long id) {
                Fruit fruit = fruitList.get(position);
                Toast.makeText(MainActivity.this, fruit.getName(), Toast.LENGTH_SHORT).show();
            }
        });
```

使用`setOnItemClickListener()`方法为ListView注册了一个监听器，当用户点击了ListView中任何一个子项，就会回调`onItemClick()`方法。该方法中可以通过position参数判断出用户点击的是哪一个子项。

## 自定义BaseAdapter，绑定ListView

- 编写界面的XML文件，item_list_animal.xml文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/img_icon"
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:paddingLeft="8dp" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <TextView
            android:id="@+id/txt_aName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingLeft="8dp"
            android:textColor="#1D1D1C"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/txt_aSpeak"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:paddingLeft="8px"
            android:textColor="#B4B4B9"
            android:textSize="14sp" />

    </LinearLayout>
</LinearLayout>
```

- 实现**Animal.java**代码：

```java
package com.vivo.a11085273.secondlistviewtest;

public class Animal {

    private String aName;
    private String aSpeak;
    private int aIcon;

    public Animal() {
    }

    public Animal(String aName, String aSpeak, int aIcon) {
        this.aName = aName;
        this.aSpeak = aSpeak;
        this.aIcon = aIcon;
    }

    public String getaName() {
        return aName;
    }

    public String getaSpeak() {
        return aSpeak;
    }

    public int getaIcon() {
        return aIcon;
    }

    public void setaName(String aName) {
        this.aName = aName;
    }

    public void setaSpeak(String aSpeak) {
        this.aSpeak = aSpeak;
    }

    public void setaIcon(int aIcon) {
        this.aIcon = aIcon;
    }

}
```

- 添加**AnimalAdapter.java**代码，自定义BaseAdapter

```java
public class AnimalAdapter extends BaseAdapter {

    private LinkedList<Animal> mData;
    private Context mContext;

    public AnimalAdapter(LinkedList<Animal> mData, Context mContext) {
        this.mData = mData;
        this.mContext = mContext;
    }

    @Override
    public int getCount() {
        return mData.size();
    }

    @Override
    public Object getItem(int position) {
        return null;
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        convertView = LayoutInflater.from(mContext).inflate(R.layout.item_list_animal,parent,false);
        ImageView img_icon = (ImageView) convertView.findViewById(R.id.img_icon);
        TextView txt_aName = (TextView) convertView.findViewById(R.id.txt_aName);
        TextView txt_aSpeak = (TextView) convertView.findViewById(R.id.txt_aSpeak);
        img_icon.setBackgroundResource(mData.get(position).getaIcon());
        txt_aName.setText(mData.get(position).getaName());
        txt_aSpeak.setText(mData.get(position).getaSpeak());
        return convertView;
    }


}
```

- 最后修改**MainActivity.java**

```java
public class MainActivity extends AppCompatActivity {

    private List<Animal> mData = null;
    private Context mContext;
    private AnimalAdapter mAdapter = null;
    private ListView list_animal;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mContext = MainActivity.this;
        list_animal = (ListView) findViewById(R.id.list_animal);
        mData = new LinkedList<Animal>();
        mData.add(new Animal("狗说", "你是狗么?", R.mipmap.ic_launcher));
        mData.add(new Animal("牛说", "你是牛么?", R.mipmap.ic_launcher));
        mData.add(new Animal("鸭说", "你是鸭么?", R.mipmap.ic_launcher));
        mData.add(new Animal("鱼说", "你是鱼么?", R.mipmap.ic_launcher));
        mData.add(new Animal("马说", "你是马么?", R.mipmap.ic_launcher));
        mAdapter = new AnimalAdapter((LinkedList<Animal>) mData, mContext);
        list_animal.setAdapter(mAdapter);
    }
}
```

[自定义通用适配器封装](https://blog.csdn.net/qq_27630169/article/details/52200885)

### 表头表尾分割线设置

ListView可以自己设置表头，表尾以及分割线。

- **footerDividersEnabled**：是否在footerView(表尾)前绘制一个分隔条,默认为true
- **headerDividersEnabled**:是否在headerView(表头)前绘制一个分隔条,默认为true
- **divider**:设置分隔条,可以用颜色分割,也可以用drawable资源分割
- **dividerHeight**:设置分隔条的高度

翻遍了了API发现并没有可以直接设置ListView表头或者表尾的属性，只能在Java中写代码 进行设置了，可供我们调用的方法如下：

- **addHeaderView(View v)**：添加headView(表头),括号中的参数是一个View对象
- **addFooterView(View v)**：添加footerView(表尾)，括号中的参数是一个View对象
- **addHeaderView(headView, null, false)**：和前面的区别：设置Header是否可以被选中
- **addFooterView(View,view,false)**：同上

对了，**使用这个addHeaderView方法必须放在listview.setAdapter前面**，否则会报错。

**示例**：

- 首先编写表头与表尾的布局：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:layout_height="48dp"
        android:layout_width="match_parent"
        android:textSize="18sp"
        android:text="表头"
        android:gravity="center"
        android:background="#43BBEB"
        android:textColor="#FFFFFF"/>

</LinearLayout>
```

表尾view_footer与其类似。

- 修改MainActivity代码：

```java
public class MainActivity extends AppCompatActivity implements AdapterView.OnItemClickListener{

    private List<Animal> mData = null;
    private Context mContext;
    private AnimalAdapter mAdapter = null;
    private ListView list_animal;
    private LinearLayout ly_content;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mContext = MainActivity.this;
        list_animal = (ListView) findViewById(R.id.list_animal);
        //动态加载顶部View和底部View
        final LayoutInflater inflater = LayoutInflater.from(this);
        View headView = inflater.inflate(R.layout.view_header, null, false);
        View footView = inflater.inflate(R.layout.view_footer, null, false);

        mData = new LinkedList<Animal>();
        mData.add(new Animal("狗说", "你是狗么?", R.mipmap.ic_icon_dog));
        mData.add(new Animal("牛说", "你是牛么?", R.mipmap.ic_icon_cow));
        mData.add(new Animal("鸭说", "你是鸭么?", R.mipmap.ic_icon_duck));
        mData.add(new Animal("鱼说", "你是鱼么?", R.mipmap.ic_icon_fish));
        mData.add(new Animal("马说", "你是马么?", R.mipmap.ic_icon_horse));
        mAdapter = new AnimalAdapter((LinkedList<Animal>) mData, mContext);
        //添加表头和表尾需要写在setAdapter方法调用之前！！！
        list_animal.addHeaderView(headView);
        list_animal.addFooterView(footView);

        list_animal.setAdapter(mAdapter);
        list_animal.setOnItemClickListener(this);
    }

    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        Toast.makeText(mContext,"你点击了第" + position + "项",Toast.LENGTH_SHORT).show();
    }
}
```

如果想要列表显示列表的最下面，可以使用：`android:stackFromBttom`属性设置为true即可。



**ListView焦点问题**：

- 只需为抢占了ListView Item焦点的控件设置**android:focusable="false"**即可解决这个问题 或者在代码中获得控件后调用：**setFocusable(false)** !!另外，EditText却不行，
- 在Item布局的根节点添加上述属性，**android:descendantFocusability="blocksDescendants"** 即可，另外该属性有三个可供选择的值：
  - **beforeDescendants**：viewgroup会优先其子类控件而获取到焦点
  - **afterDescendants**：viewgroup只有当其子类控件不需要获取焦点时才获取焦点
  - **blocksDescendants**：viewgroup会覆盖子类控件而直接获得焦点







## 自定义通用BaseAdapter适配器











# 5 RecyclerView

ListView只能实现数据纵向滚动效果，扩展性较差。而RecyclerView是一个功能更为强大的滚动控件。

## 基本用法

RecyclerView定义在support库当中，想要使用该控件，需要在项目的**build.gradle**中添加相应的依赖库。在**app/build.gradle**文件，在dependencies闭包中添加：

```java
 implementation 'com.android.support:recyclerview-v7:28.0.0'
```

添加完之后点击Sync Now进行同步。修改activity_main.xml中的代码：

```xml
<android.support.v7.widget.RecyclerView
    android:id="@+id/recycler_view"
    android:scrollbars="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

**由于RecyclerView并不是内置在系统SDK中，需要把完整的包路径写出来**。将前述的图片，Fruit类和fruit_item.xml复制过来，新建FruitAdapter类，继承自RecyclerView.Adapter。

```java
public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {

    private List<Fruit> mFruitList;

    static class ViewHolder extends RecyclerView.ViewHolder {
        ImageView fruitImage;
        TextView fruitName;

        public ViewHolder(View view) {
            super(view);
            fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
            fruitName = (TextView) view.findViewById(R.id.fruit_name);
        }
    }

    public FruitAdapter(List<Fruit> fruitList) {
        mFruitList = fruitList;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
        ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        Fruit fruit = mFruitList.get(position);
        holder.fruitImage.setImageResource(fruit.getImageId());
        holder.fruitName.setText(fruit.getName());
    }

    @Override
    public int getItemCount() {
        return mFruitList.size();
    }
}
```

修改MainActivity代码：

```java
public class MainActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();
        RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new
                LinearLayoutManager(this);
        recyclerView.setLayoutManager(layoutManager);
        FruitAdapter adapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(adapter);
    }

    private void initFruits() {
        for (int i = 0; i < 2; i++) {
            Fruit apple = new Fruit("Apple", R.drawable.apple_pic);
            fruitList.add(apple);
            Fruit banana = new Fruit("Banana", R.drawable.banana_pic);
            fruitList.add(banana);
            Fruit orange = new Fruit("Orange", R.drawable.orange_pic);
            fruitList.add(orange);
            Fruit watermelon = new Fruit("Watermelon", R.drawable.watermelon_pic);
            fruitList.add(watermelon);
            Fruit pear = new Fruit("Pear", R.drawable.pear_pic);
            fruitList.add(pear);
            Fruit grape = new Fruit("Grape", R.drawable.grape_pic);
            fruitList.add(grape);
            Fruit pineapple = new Fruit("Pineapple", R.drawable.pineapple_pic);
            fruitList.add(pineapple);
            Fruit strawberry = new Fruit("Strawberry", R.drawable.strawberry_pic);
            fruitList.add(strawberry);
            Fruit cherry = new Fruit("Cherry", R.drawable.cherry_pic);
            fruitList.add(cherry);
            Fruit mango = new Fruit("Mango", R.drawable.mango_pic);
            fruitList.add(mango);
        }
    }
}
```

除了LinearLayoutManager之外，还提供了**GridLayoutManager**和**StaggeredGridLayoutManager**这两种内置的布局排列方式。前者实现网格布局，后者可以用于实现瀑布流布局。





# 6 Nine-Patch图片

- 什么是**.9**图片

  图片后缀名前有.9的图片，例如pic.9.png这样的图片

- 9图的作用

  图片拉伸的时候特定区域不会发生图片失真，不失真的区域可以由我们自己绘制。

- 制作9图的工具

  - **Android SDK自带：draw9patch.bat**，在sdk目录下的tools文件夹中
  - **NinePatchEditor**，做了一些优化，支持批量操作。

Android Studio集成了draw9patch.bat，制作Nine-patch，首先选取图片，后缀名改为**.png**,然后右键选择`Create nine-patch pticure`

![](http://www.runoob.com/wp-content/uploads/2015/06/85496981.jpg)

之后就可以在斑马线上进行操作（黑色那条线是一条条点出来的，如果想消除可以按住shift键+鼠标左键即可）



