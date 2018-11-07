# 1.Intent使用

参考文献：

- 参考文献一：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-intent-pass-data.html)
- 参考文献二：第一行代码

## 使用显式Intent

Intent事Android程序中各组件之间进行交互的一种重要方式，不仅可以指明当前组件想要执行的动作，还可以在不同组件之间进行传递数据。**被用于启动活动，启动服务以及发送广播等场景**。

Activity类中提供一个startActivity()方法，专门用于启动活动，接受一个Intent参数，构建好的Intent传入该方法就可以启动目标活动了。创建一个新的Activity——SecondActivity。

- 修改FirstActivity中按钮点击事件代码：

```java
public void onClick(View v) {
                Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
                startActivity(intent);
            }
```

- 或通过**Intent**的**ComponentName**来启动

```java
ComponentName cn = new ComponentName("com.vivo.a11085273.activitytest","com.vivo.a11085273.activitytest.ThirdActivity");
Intent intent = new Intent();
intent.setComponent(cn);
startActivity(intent);
```

- 或通过初始化Intent时指定包名

```java
Intent intent = new Intent("android.intent.action.MAIN");
intent.setClassName("com.vivo.a11085273.activitytest", "com.vivo.a11085273.activitytest.ThirdActivity");
startActivity(intent);
```



## 使用隐式Intent

不明确指出我们想要启动哪一个活动，指定了一系列更为抽象的action和category等信息，交由系统分析这个Intent，找出合适的Intent去启动。通过Intent的**Intent-filter**实现。

```xml
<activity android:name=".SecondActivity">
            <intent-filter>
                <action android:name="com.vivo.a11085273.activitytest.ACTION_START" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
```

修改FirstActivity中按钮点击事件：

```java
@Override
            public void onClick(View v) {
                Intent intent = new Intent("com.vivo.a11085273.activitytest.ACTION_START");
                intent.addCategory("com.vivo.a11085273.activitytest.MY_CATEGORY");
                startActivity(intent);
            }
```

每个Intent只能指定一个action，但是却能指定多个category。可以调用Intent中的`addCategory()`方法来添加category。

### 隐式Intent其他功能

使用隐式Intent不仅可以启动程序内的活动，还可以启动其他程序的活动，使得Android多个应用程序之间的功能共享成为了可能。

示例:

```java
@Override
            public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_VIEW);
                intent.setData(Uri.parse("http://www.google.com.hk"));
                startActivity(intent);
            }
```

`Uri.parse`将一个网址字符串解析成一个Uri对象。

如果自己创建一个活动，也可以响应打开网页的Intent。

新建Activity，编辑布局文件，在AndroidManifest.xml中修改ThirdActivity注册信息：

```xml
<activity android:name=".ThirdActivity">
            <intent-filter tools:ignore="AppLinkUrlError">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <data android:scheme="http" />
            </intent-filter>
        </activity>
```

除了http协议外，可以指定很多其他协议，比如：`geo`表示显示地理位置，`tel`表示拨打电话。

# 2. Activity间数据传递

一个app一般都是由多个Activity构成，这就涉及到多个Acitivity间数据传递。

![](http://www.runoob.com/wp-content/uploads/2015/08/7185831.jpg)

### Intent向下一个活动传递数据

Intent中提供了一些列`putExtra()`方法的重载，可以把想要传递的数据暂存在Intent中，启动了另一个活动后，把这些数据从Intent中取出来就可以。

示例：在FirstActivity中修改点击按钮监听代码

```java
@Override
            public void onClick(View v) {
                String data = "Hello SecondActivity";
                Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
                intent.putExtra("extra_data", data);
                startActivity(intent);
            }
```

修改SecondActivity中代码：

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_activity);
        Intent intent = getIntent();
        String data = intent.getStringExtra("extra_data");
        Log.d("SecondActivity", data);
    }
```

### 返回数据给上一个活动

Activity中还有一个`startActivityForResult()`方法也适用于启动活动，但是该方法期望活动销毁的时候返回一个结果给上一个活动。

![](http://www.runoob.com/wp-content/uploads/2015/08/67124491.jpg)

该方法接收两个参数，一个是Intent，第二个参数是请求码，用于之后在回调中判断数据的来源。

首先修改FirstActivity中点击按钮监听事件代码：

```java
 @Override
            public void onClick(View v) {
                Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
                startActivityForResult(intent,1);
            }
```

其次修改SecondActivity中代码：

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.second_activity);
        Button button2 = (Button) findViewById(R.id.button_2);
        button2.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.putExtra("data_return","Hello FirstActivity");
                setResult(RRSULT_OK, intent);
                finish();
            }
        });
    }
```

最后在FirstActivity中重写这个方法来得到返回数据：

```java
 @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        switch(requestCode) {
            case 1:
                if (resultCode == RESULT_OK) {
                    String returnedData = data.getStringExtra("data_return");
                    Log.d("FritstActivity",returnedData);
                }
                break;
            default:
        }
    }
```

如果是通过back键返回，则可以通过在SecondActivity中重写`onBackPressed()`来解决：

```java
 @Override
    public void onBackPressed() {
        Intent intent = new Intent();
        intent.putExtra("data_return","Hello FirstActivity");
        setResult(RESULT_OK, intent);
        finish();
    }
```

# 

# 3. Intent传递复杂数据

## 传递简单数据

![](http://www.runoob.com/wp-content/uploads/2015/08/71858311.jpg)

直接通过调用Intent的`putExtra()`方法存入数据，然后在获得Intent后调用getXxxExtra获得 对应类型的数据；传递多个的话，可以使用Bundle对象作为容器，通过调用Bundle的putXxx先将数据 存储到Bundle中，然后调用Intent的`putExtras()`方法将Bundle存入Intent中，然后获得Intent以后， 调用`getExtras()`获得Bundle容器，然后调用其getXXX获取对应的数据！ 另外数据存储有点类似于Map的<键，值>！

## 传递数组

- **写入数组**：

  ```java
  bd.putStringArray("StringArray", new String[]{"呵呵","哈哈"});
  //可把StringArray换成其他数据类型,比如int,float等等...
  ```

- 读取数组

  ```java
  String[] str = bd.getStringArray("StringArray")
  ```

## 传递集合

### List<基本数据类型或String>

- **写入集合**：

  ```java
  intent.putStringArrayListExtra(name, value)
  intent.putIntegerArrayListExtra(name, value)
  ```

- **读取集合**：

  ```java
  intent.getStringArrayListExtra(name)
  intent.getIntegerArrayListExtra(name)
  ```

### `List<Object>`

将list强转成**Serializable**类型,然后传入(可用Bundle做媒介)

- **写入集合**：

  ```java
  public void onClick(View v) {
                  Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                  Bundle bd = new Bundle();
                  list = new ArrayList<Object>();
                  list.add("nice");
                  list.add("boy");
                  bd.putSerializable("list",list);
                  intent.putExtras(bd);
                  startActivity(intent);
              }
  ```

- **读取集合**：

  ```java
  Intent intent = getIntent();
          Bundle bd = intent.getExtras();
          List str = (List<Object>) bd.getSerializable("list");
          System.out.println(str.get(0));
  ```

**Object类需要实现Serializable接口**

### Map<String,Object>，或更复杂

**解决方法**：在外层套一个List

- **传递数据**：

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ArrayList bundleList;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button sendValue = (Button) findViewById(R.id.button_sendValue);
          sendValue.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  Map<String, Object> map1 = new HashMap<String, Object>();
                  map1.put("key1", "value1");
                  map1.put("key2", "value2");
                  List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
                  list.add(map1);
                  Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                  Bundle bd = new Bundle();
                  //须定义一个list用于在budnle中传递需要传递的ArrayList<Object>,这个是必须要的
                  bundleList = new ArrayList<>();
                  bundleList.add(list);
                  bd.putParcelableArrayList("list", bundleList);
                  intent.putExtras(bd);
                  startActivity(intent);
              }
          });
      }
  }
  ```

- **读取数据**：

  ```java
  public class SecondActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_second);
          Intent intent = getIntent();
          Bundle bd = intent.getExtras();
          ArrayList bundleList = bd.getParcelableArrayList("list");
          List<Map<String, Object>> list =(List) bundleList.get(0);
          Map<String, Object> map = list.get(0);
          System.out.println(map.containsKey("key1"));
      }
  }
  ```

## Intent传递对象

传递对象的方式有两种：

- 将对象转换为**Json**字符串
- 通过**Serializable**,**Parcelable**序列化 不建议使用Android内置的抠脚Json解析器，可使用fastjson或者Gson第三方库！

### 将对象转换为Json字符串

**Gson解析的例子：**

- **Model:**

  ```java
  public class Book{
      private int id;
      private String title;
      //...
  }
  
  public class Author{
      private int id;
      private String name;
      //...
  }
  ```

- **写入数据：**

  ```java
  Book book=new Book();
  book.setTitle("Java编程思想");
  Author author=new Author();
  author.setId(1);
  author.setName("Bruce Eckel");
  book.setAuthor(author);
  Intent intent=new Intent(this,SecondActivity.class);
  intent.putExtra("book",new Gson().toJson(book));
  startActivity(intent);
  ```

- **读取数据**：

  ```java
  String bookJson=getIntent().getStringExtra("book");
  Book book=new Gson().fromJson(bookJson,Book.class);
  Log.d(TAG,"book title->"+book.getTitle());
  Log.d(TAG,"book author name->"+book.getAuthor().getName());
  ```



### 使用Serializable，Parcelable序列化对象

**Serializable**实现：

- 业务Bean实现：Serializable接口,写上getter和setter方法
- Intent通过调用`putExtra(String name, Serializable value)`传入对象实例 当然对象有多个的话多个的话,我们也可以先`Bundle.putSerializable(x,x)`;
- 新Activity调用`getSerializableExtra()`方法获得对象实例: eg:Product pd = (Product) getIntent().getSerializableExtra("Product");
- 调用对象get方法获得相应参数

**Parcelable实现**：

- 业务Bean继承**Parcelable**接口,重写**writeToParcel**方法,将你的对象序列化为一个Parcel对象;
- 重写**describeContents**方法，内容接口描述，默认返回0就可以
- 实例化静态内部对象**CREATOR**实现接口**Parcelable.Creator**
- 同样式通过Intent的`putExtra()`方法传入对象实例,当然多个对象的话,我们可以先 放到Bundle里`Bundle.putParcelable(x,x)`，再`Intent.putExtras()`即可

**注**：

通过writeToParcel将你的对象映射成Parcel对象，再通过createFromParcel将Parcel对象映射 成你的对象。也可以将Parcel看成是一个流，通过writeToParcel把对象写到流里面，在通过createFromParcel从流里读取对象，只不过这个过程需要你来实现，因此**写的顺序和读的顺序必须一致**。

示例：实现Parcelable接口

```java
//Internal Description Interface,You do not need to manage  
@Override  
public int describeContents() {  
     return 0;  
}  
       
      
 
@Override  
public void writeToParcel(Parcel parcel, int flags){  
    parcel.writeString(bookName);  
    parcel.writeString(author);  
    parcel.writeInt(publishTime);  
}  
      

public static final Parcelable.Creator<Book> CREATOR = new Creator<Book>() {  
    @Override  
    public Book[] newArray(int size) {  
        return new Book[size];  
    }  
          
    @Override  
    public Book createFromParcel(Parcel source) {  
        Book mBook = new Book();    
        mBook.bookName = source.readString();   
        mBook.author = source.readString();    
        mBook.publishTime = source.readInt();   
        return mBook;  
    }  
};
```

**Android Studio生成Parcleable插件：**

Intellij/Andriod Studio插件android-parcelable-intellij-plugin 只要ALT+Insert，即可直接生成Parcleable接口代码。

另外：Android中大量用到Parcelable对象，实现Parcable接口又是非常繁琐的,可以用到 第三方的开源框架:**Parceler**,因为Maven的问题,暂时还没试。

**两种序列化方式的比较：**

两者的比较:

- 1）在使用内存的时候，Parcelable比Serializable性能高，所以推荐使用Parcelable。
- 2）Serializable在序列化的时候会产生大量的临时变量，从而引起频繁的GC。
- 3）**Parcelable不能使用在要将数据存储在磁盘上的情况**，因为Parcelable不能很好的保证数据的 持续性在外界有变化的情况下。尽管Serializable效率低点，但此时还是建议使用Serializable。

## Intent传递Bitmap

bitmap默认实现**Parcelable**接口,直接传递即可

```java
Bitmap bitmap = null;
Intent intent = new Intent();
Bundle bundle = new Bundle();
bundle.putParcelable("bitmap", bitmap);
intent.putExtra("bundle", bundle);
```

## 定义全局数据

> 如果是传递简单的数据，有这样的需求，Activity1 -> Activity2 -> Activity3 -> Activity4， 你想在Activity中传递某个数据到Activity4中，怎么破，一个个页面传么？
>
> 如果你想某个数据可以在任何地方都能获取到，你就可以考虑使用 **Application全局对象**了！
>
> Android系统在每个程序运行的时候创建一个Application对象，而且只会创建一个，所以Application 是单例(singleton)模式的一个类，而且Application对象的生命周期是整个程序中最长的，他的生命 周期等于这个程序的生命周期。如果想存储一些比静态的值(固定不改变的，也可以变)，如果你想使用 Application就需要自定义类实现Application类，并且告诉系统实例化的是我们自定义的Application 而非系统默认的，而这一步，就是在AndroidManifest.xml中卫我们的application标签添加:**name属性**！

- **自定义Application类**

  ```java
  class MyApp extends Application {
      private String myState;
      public String getState(){
          return myState;
      }
      public void setState(String s){
          myState = s;
      }
  }
  ```

- **AndroidManifest.xml中声明：**

  ```xml
  <application android:name=".MyApp" android:icon="@drawable/icon" 
    android:label="@string/app_name">
  ```

- **在需要的地方调用**

  ```java
  final MyApp appState = (MyApp)getApplicationContext();
  appState.setState("hello MainActivity");
  final String state = appState.getState();
  appState.setState("hello secondActivity");
  textView1.setText(state);
  sendValue.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View v) {
  
          Intent intent = new Intent(MainActivity.this, SecondActivity.class);
          startActivity(intent);
      }
  });
  ```

**注**：

> Application对象是存在于内存中的，也就有它可能会被系统杀死，比如这样的场景：
>
> 我们在Activity1中往application中存储了用户账号，然后在Activity2中获取到用户账号，并且显示！
>
> 如果我们点击home键，然后过了N久候，系统为了回收内存kill掉了我们的app。这个时候，我们重新 打开这个app，这个时候很神奇的，回到了Activity2的页面，但是如果这个时候你再去获取Application 里的用户账号，程序就会报NullPointerException，然后crash掉~
>
> 之所以会发生上述crash，是因为这个Application对象是全新创建的，可能你以为App是重新启动的， 其实并不是，仅仅是创建一个新的Application，然后启动上次用户离开时的Activity，从而创造App 并没有被杀死的假象！所以如果是比较重要的数据的话，建议你还是进行本地化，另外在使用数据的时候 要对变量的值进行非空检查！还有一点就是：不止是Application变量会这样，单例对象以及公共静态变量 也会这样~

## 单例模式传参

上面的Application就是基于单例的，单例模式的特点就是可以保证系统中一个类有且只有一个实例。 这样很容易就能实现，在A中设置参数，在B中直接访问了。这是几种方法中效率最高的。

示例：

```java
public class XclSingleton  
{  
    //单例模式实例  
    private static XclSingleton instance = null;  
      
    //synchronized 用于线程安全，防止多线程同时创建实例  
    public synchronized static XclSingleton getInstance(){  
        if(instance == null){  
            instance = new XclSingleton();  
        }     
        return instance;  
    }     
      
    final HashMap<String, Object> mMap;  
    private XclSingleton()  
    {  
        mMap = new HashMap<String,Object>();  
    }  
      
    public void put(String key,Object value){  
        mMap.put(key,value);  
    }  
      
    public Object get(String key)  
    {  
        return mMap.get(key);  
    }  
      
} 
```

设置参数：

```java
XclSingleton.getInstance().put("key1", "value1");  
XclSingleton.getInstance().put("key2", "value2");  
```