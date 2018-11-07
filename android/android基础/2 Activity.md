# 1 Activity 概念

**参考文献**：

- Android[指南](https://developer.android.com/guide/components/activities/intro-activities)
- 参考文献一：第一行代码
- 参考文献二：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-activity.html)

------

活动是一种**可以包含用户界面的组件，主要用于和用户进行交互**。

活动提供应用程序绘制其UI的窗口。此窗口通常填充屏幕，但可能小于屏幕并浮动在其他窗口的顶部。通常，**一个活动在应用程序中实现一个屏幕**。例如，应用程序的某个活动可能会实现*“首选项”*屏幕，而另一个活动会实现“ *选择照片”*屏幕。

**大多数应用程序包含多个屏幕，这意味着它们包含多个活动 通常，应用程序中的一个活动被指定为*主要活动*，这是用户启动应用程序时显示的第一个屏幕**。然后，每个活动可以启动另一个活动以执行不同的操作。

![](http://www.runoob.com/wp-content/uploads/2015/06/activity_main.jpg)

# 2 Activity基本用法

- **所有的活动都要在AndroidManifest.xml中进行注册才能生效**。活动的注册声明要放在`<application>`标签内，在`<activity>`标签中是晕了android：name来指定具体注册哪一个活动。

- **<font color=red>配置主活动的方法</font>**就是在`<application>`标签内加入`<intent-filter>`标签，并在这个标签内添加

  ```xml
  <action android:name="android.intent.action.MAIN" />
  
  <category android:name="android.intent.category.LAUNCHER" />
  ```

## 声明权限

You can use the manifest's [`<activity>`](https://developer.android.com/guide/topics/manifest/activity-element.html) tag to control which apps can start a particular activity. A parent activity cannot launch a child activity unless both activities have the same permissions in their manifest. If you declare a[`<uses-permission>`](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) element for a particular activity, the calling activity must have a matching [`<uses-permission>`](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) element.

```xml
<manifest>
<activity android:name="...."
  android:permission=”com.google.socialapp.permission.SHARE_POST”
/>
```

如果要允许调用SocialApp，您的应用必须与SocialApp清单中设置的权限相匹配

```xml
<manifest> <uses-permission android：name = “com.google.socialapp.permission.SHARE_POST” /> </ manifest>
```

## 销毁一个活动

销毁活动只要按back键就可以，而且Activity类提供一个`finish()`方法，活动中调用这个方法就可以销毁当前活动。将前述的按钮监听器代码加入该方法即可。

## 使用Intent

### 使用显式Intent

Intent是Android程序中各组件之间进行交互的一种重要方式，不仅可以指明当前组件想要执行的动作，还可以在不同组件之间进行传递数据。**被用于启动活动，启动服务以及发送广播等场景**。

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



### 使用隐式Intent

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

与此对应，还可以在<intent_filter>标签中再配置一个`<data>`标签，用于更精确地指定当前活动能够响应什么类型的数据。`<data>`标签中主要可以配置一下内容：

- **android:scheme**：用于指定数据的协议部分，如上例中的http
- **android:host**：用于指定数据的主机名部分，如上例中的www.baidu.com
- **android:port**：用于指定数据的端口部分，一般紧随在主机名之后
- **android:path**:用于指定主机名和端口之后的部分，如一段网址跟在主机名之后的内容
- **android:mimeType**：用于指定可以处理的数据类型，允许使用通配符的方式进行指定。

只有`<data>`标签中指定的内容和Intent中携带的Data完全一致，当前活动才能够相应该Intent。一般不会在`<data>`标签中指定过多内容。

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

# 3 Activity间数据传递

一个app一般都是由多个Activity构成，这就涉及到多个Acitivity间数据传递。

![](http://www.runoob.com/wp-content/uploads/2015/08/7185831.jpg)

### Intent向下一个活动传递数据

Intent中提供了一些列`putExtra()`方法的重载，可以吧想要传递的数据暂存在Intent中，启动了另一个活动后，把这些数据从Intent中取出来就可以。

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

# 3 活动的生命周期

## 返回栈

Android中的活动是可以层叠的。每启动一个新的活动，就会覆盖原来的活动之上，点击Back键就会销毁最上面的活动。

Android是使用任务（**Task**）来管理活动的，一个任务就是一组存放在栈里的活动的集合，这个栈也称作**返回栈**（Back Stack）。系统总是会显示处于栈顶活动给用户。

![](E:\document\photo\first-code\返回栈.jpg)

## 活动状态

每个活动在其生命周期中最多可能会有4中状态：

- 运行状态：

  活动位于返回栈的栈顶时，这时活动就处于运行状态。

- 暂停状态：

  活动不处于栈顶时，但是仍然可见，活动就进入暂停状态。

- 停止状态：

  活动不处于栈顶位置，且完全不可见，就进入停止状态。处于停止状态的活动有可能会被系统回收

- 销毁状态：

  活动从返回栈中移除后就变成了销毁状态。

## 活动的生命周期

Activity类中定义了7种回调方法，覆盖了活动生命周期的每一个环节：

- `onCreate()`：完成活动的初始化操作，比如加载布局，绑定事件
- `onStart()`:在活动由不可见变为可见的时候调用
- `onResume()`：在活动准备好和用户进行交互时调用。该活动一定处于返回栈栈顶，且处于运行状态
- `onPause()`：系统准备去启动或者恢复另一个活动的时候调用。该方法的执行速度一定要快，否则会影响到新的栈顶活动的使用
- `onStop`：在活动完全不可见时调用，与前者的区别时启动的新活动是一个对话框式的活动，那么onPause()方法会得到执行，而该方法不会
- `onDestory()`：在活动被销毁之前调用，之后活动变为销毁状态
- `onRestart()`：由停止状态变为运行时状态之前调用，活动被重新启动。

除了**onRestart()**方法之外，其他都是两两相对的，活动分为3中生存期：

- **完整生存期**：活动在`onCreate()`和`onDestrory()`之间经历的就是完整生存期
- **可见生存期**：活动在`onStart()`和`onStop`之间经历的就是可见生存期。活动对于用户总是可见的，即便有可能无法和用户进行交互。
- **前台生存期**：活动在`onResume()`和`onPause()`之间经历的就是前台生存期。活动总是处于运行状态，此时活动可以和用户进行交互。

![](https://developer.android.com/guide/components/images/activity_lifecycle.png)

onCreate()代码示例：

```java
TextView mTextView;

// some transient state for the activity instance
String mGameState;

@Override
public void onCreate(Bundle savedInstanceState) {
    // call the super class onCreate to complete the creation of activity like
    // the view hierarchy
    super.onCreate(savedInstanceState);

    // recovering the instance state
    if (savedInstanceState != null) {
        mGameState = savedInstanceState.getString(GAME_STATE_KEY);
    }

    // set the user interface layout for this activity
    // the layout file is defined in the project res/layout/main_activity.xml file
    setContentView(R.layout.main_activity);

    // initialize member TextView so we can manipulate it later
    mTextView = (TextView) findViewById(R.id.text_view);
}

// This callback is called only when there is a saved instance that is previously saved by using
// onSaveInstanceState(). We restore some state in onCreate(), while we can optionally restore
// other state here, possibly usable after onStart() has completed.
// The savedInstanceState Bundle is same as the one used in onCreate().
@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
    mTextView.setText(savedInstanceState.getString(TEXT_VIEW_KEY));
}

// invoked when the activity may be temporarily destroyed, save the instance state here
@Override
public void onSaveInstanceState(Bundle outState) {
    outState.putString(GAME_STATE_KEY, mGameState);
    outState.putString(TEXT_VIEW_KEY, mTextView.getText());

    // call superclass to save any view hierarchy
    super.onSaveInstanceState(outState);
}
```

## 活动被回收

如果一个活动进入停止状态，有可能被系统回收，其中的数据在回收时可能得不到保存。

Activity中提供了一个`onSaveInstanceState()`回调方法，这个方法可以保证活动被回收之前一定被调用，可以通过这个方法来保存数据。

```java
@Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        String tempData = "Something you just typed";
        outState.putString("data_key", tempData);
    }
```

数据的取值可以在onCreate()方法中取出：

```java
 if (savedInstanceState != null) {
            String tempData = savedInstanceState.getString("data_key");
            Log.d(TAG, tempData);
        }
```





# 4 横竖屏切换与状态保存

官方文档：[横竖屏切换](https://developer.android.com/reference/android/R.attr#screenOrientation)

参考文献一：[gdutxiaoxu博客](https://blog.csdn.net/gdutxiaoxu/article/details/62235974)

参考文献二：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-activity.html)

------

App横竖屏切换的时候会销毁当前的Activity然后重新创建一个，你可以自行在生命周期 的每个方法里都添加打印Log的语句，来进行判断，又或者设一个按钮一个TextView点击按钮后，修改TextView 文本，然后横竖屏切换，会神奇的发现TextView文本变回之前的内容了！ 横竖屏切换时Activity走下述生命周期：
**onPause-> onStop-> onDestory-> onCreate->onStart->onResume**

## 禁止屏幕横竖屏自动切换

如果要禁止屏幕横竖屏自动切换，在**AndroidManifest.xml**中为Activity添加一个属性：**android:screenOrientaiton**

。。。





# 5 启动模式

启动模式一共有4种，分别是：`standard`，`singleTop`，`singleTask`和`singleInstance`。可以再AndroidManifest.xml中通过`<activity>`标签指定**android:launchMode**属性来选择启动模式。

## standard

默认模式，每当启动一个新活动，就会在返回栈中入栈，并处于栈顶位置。该模式的活动无论这个活动是否在返回栈中存在，每次启动都会创建该活动的新实例。

![](E:\document\photo\first-code\standard模式示意图.jpg)

## singleTop

当活动指定为**singleTop**模式，启动活动时如果发现返回栈的栈顶已经是该活动，则认为可以直接使用它，不会再创建新的活动实例。

![](E:\document\photo\first-code\singleTop示意图.jpg)

## singleTask

当活动的启动模式指定为**singleTask**，每次启动该活动系统首先会在返回栈中检查是否存在该活动的实例，如果发现就直接使用，并把这个活动之上的所有活动统统出栈。如果没有发现就创建一个新的活动实例。

![](E:\document\photo\first-code\singleTask模式示意图.jpg)

## singleInstance

指定为**singleInstance**模式的活动会启用一个新的返回栈来管理这个活动（如果singleTask模式指定了不同的**taskAffinity**，也会启动一个新的返回栈）。这种模式解决了其他程序和我们的程序共享一个活动的实例。

![](E:\document\photo\first-code\singleInstance.jpg)



# 6 活动实践

## 确定当前活动

例如需要在某个界面上修改相关东西，快速定位该界面对应的活动

新建一个Java类：BaseActivity，继承AppCompatActivity,重写onCreate()方法。

```java
@Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("BaseActivity", getClass().getSimpleName());
    }
```

在`oncreate()`方法中获取了当前实例的类名，然后将项目中所有活动的父类都不再继承AppCompatActivity，而是继承BaseActivity，这样查看日志，，每当进入一个活动的界面，该活动的类名就被打印出来。

## 随时退出程序

按home键只是将程序挂起，并没有退出程序，如果程序需要一个注销或者退出的功能，只需要一个专门的集合类对所有的活动进行管理就可以。

新建一个**ActivityCollector**类作为活动管理器：

```java
public class ActivityCollector {

    public static List<Activity> activities = new ArrayList<>();

    public static void addActivity(Activity activity) {
        activities.add(activity);
    }

    public static void removeActivity(Activity activity) {
        activities.remove(activity);
    }

    public static void finishAll() {
        for (Activity activity : activities) {
            if (!activity.isFinishing()) {
                activity.finish();
            }
        }
    }
}
```

之后修改BaseActivity：

```java
public class BaseActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("BaseActivity", getClass().getSimpleName());
        ActivityCollector.addActivity(this);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        ActivityCollector.removeActivity(this);
    }
}
```

如果需要在某个地方退出程序，调用`ActivityCollector.finishAll()`方法就可以。

可以再销毁所有活动代码的后面加上杀掉当前进程，确保程序完全退出：

```java
android.os.Process.killProcess(android.os.Process.myPid());
```

其中`killProcess()`方法用于杀掉一个进程，接收一个进程参数，通过`myPid`()方法可以获得当前程序的进程id。

## 启动活动最佳写法

如果你的模块需要将一些重要数据传递给另一个模块，例如两个字符串参数，但是你不清楚启动这个活动需要传递哪些数据，只需在启动活动添加一个`actionStart()`方法。

```java
public static void actionStart(Context context, String data1, String data2) {
        Intent intent = new Intent(context, SecondActivity.class);
        intent.putExtra("param1", data1);
        intent.putExtra("param2", data2);
        context.startActivities(intent);
    }
```

这时候如果想要启动该活动，只需一行代码就可以启动SecondActivity。

```java
 button1.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View v) {
            	SecondActivity.actionStart(FirstActivity.this, "data1", "data2");
            }
 }
```





# 7 系统提供常见Activity

```java
//1.拨打电话
// 给移动客服10086拨打电话
Uri uri = Uri.parse("tel:10086");
Intent intent = new Intent(Intent.ACTION_DIAL, uri);
startActivity(intent);

//2.发送短信
// 给10086发送内容为“Hello”的短信
Uri uri = Uri.parse("smsto:10086");
Intent intent = new Intent(Intent.ACTION_SENDTO, uri);
intent.putExtra("sms_body", "Hello");
startActivity(intent);

//3.发送彩信（相当于发送带附件的短信）
Intent intent = new Intent(Intent.ACTION_SEND);
intent.putExtra("sms_body", "Hello");
Uri uri = Uri.parse("content://media/external/images/media/23");
intent.putExtra(Intent.EXTRA_STREAM, uri);
intent.setType("image/png");
startActivity(intent);

//4.打开浏览器:
// 打开Google主页
Uri uri = Uri.parse("http://www.baidu.com");
Intent intent  = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);

//5.发送电子邮件:(阉割了Google服务的没戏!!!!)
// 给someone@domain.com发邮件
Uri uri = Uri.parse("mailto:someone@domain.com");
Intent intent = new Intent(Intent.ACTION_SENDTO, uri);
startActivity(intent);
// 给someone@domain.com发邮件发送内容为“Hello”的邮件
Intent intent = new Intent(Intent.ACTION_SEND);
intent.putExtra(Intent.EXTRA_EMAIL, "someone@domain.com");
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject");
intent.putExtra(Intent.EXTRA_TEXT, "Hello");
intent.setType("text/plain");
startActivity(intent);
// 给多人发邮件
Intent intent=new Intent(Intent.ACTION_SEND);
String[] tos = {"1@abc.com", "2@abc.com"}; // 收件人
String[] ccs = {"3@abc.com", "4@abc.com"}; // 抄送
String[] bccs = {"5@abc.com", "6@abc.com"}; // 密送
intent.putExtra(Intent.EXTRA_EMAIL, tos);
intent.putExtra(Intent.EXTRA_CC, ccs);
intent.putExtra(Intent.EXTRA_BCC, bccs);
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject");
intent.putExtra(Intent.EXTRA_TEXT, "Hello");
intent.setType("message/rfc822");
startActivity(intent);

//6.显示地图:
// 打开Google地图中国北京位置（北纬39.9，东经116.3）
Uri uri = Uri.parse("geo:39.9,116.3");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);

//7.路径规划
// 路径规划：从北京某地（北纬39.9，东经116.3）到上海某地（北纬31.2，东经121.4）
Uri uri = Uri.parse("http://maps.google.com/maps?f=d&saddr=39.9 116.3&daddr=31.2 121.4");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);

//8.多媒体播放:
Intent intent = new Intent(Intent.ACTION_VIEW);
Uri uri = Uri.parse("file:///sdcard/foo.mp3");
intent.setDataAndType(uri, "audio/mp3");
startActivity(intent);

//获取SD卡下所有音频文件,然后播放第一首=-= 
Uri uri = Uri.withAppendedPath(MediaStore.Audio.Media.INTERNAL_CONTENT_URI, "1");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);

//9.打开摄像头拍照:
// 打开拍照程序
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE); 
startActivityForResult(intent, 0);
// 取出照片数据
Bundle extras = intent.getExtras(); 
Bitmap bitmap = (Bitmap) extras.get("data");

//另一种:
//调用系统相机应用程序，并存储拍下来的照片
Intent intent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE); 
time = Calendar.getInstance().getTimeInMillis();
intent.putExtra(MediaStore.EXTRA_OUTPUT, Uri.fromFile(new File(Environment
.getExternalStorageDirectory().getAbsolutePath()+"/tucue", time + ".jpg")));
startActivityForResult(intent, ACTIVITY_GET_CAMERA_IMAGE);

//10.获取并剪切图片
// 获取并剪切图片
Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
intent.setType("image/*");
intent.putExtra("crop", "true"); // 开启剪切
intent.putExtra("aspectX", 1); // 剪切的宽高比为1：2
intent.putExtra("aspectY", 2);
intent.putExtra("outputX", 20); // 保存图片的宽和高
intent.putExtra("outputY", 40); 
intent.putExtra("output", Uri.fromFile(new File("/mnt/sdcard/temp"))); // 保存路径
intent.putExtra("outputFormat", "JPEG");// 返回格式
startActivityForResult(intent, 0);
// 剪切特定图片
Intent intent = new Intent("com.android.camera.action.CROP"); 
intent.setClassName("com.android.camera", "com.android.camera.CropImage"); 
intent.setData(Uri.fromFile(new File("/mnt/sdcard/temp"))); 
intent.putExtra("outputX", 1); // 剪切的宽高比为1：2
intent.putExtra("outputY", 2);
intent.putExtra("aspectX", 20); // 保存图片的宽和高
intent.putExtra("aspectY", 40);
intent.putExtra("scale", true);
intent.putExtra("noFaceDetection", true); 
intent.putExtra("output", Uri.parse("file:///mnt/sdcard/temp")); 
startActivityForResult(intent, 0);

//11.打开Google Market 
// 打开Google Market直接进入该程序的详细页面
Uri uri = Uri.parse("market://details?id=" + "com.demo.app");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);

//12.进入手机设置界面:
// 进入无线网络设置界面（其它可以举一反三）  
Intent intent = new Intent(android.provider.Settings.ACTION_WIRELESS_SETTINGS);  
startActivityForResult(intent, 0);

//13.安装apk:
Uri installUri = Uri.fromParts("package", "xxx", null);   
returnIt = new Intent(Intent.ACTION_PACKAGE_ADDED, installUri);

//14.卸载apk:
Uri uri = Uri.fromParts("package", strPackageName, null);      
Intent it = new Intent(Intent.ACTION_DELETE, uri);      
startActivity(it); 

//15.发送附件:
Intent it = new Intent(Intent.ACTION_SEND);      
it.putExtra(Intent.EXTRA_SUBJECT, "The email subject text");      
it.putExtra(Intent.EXTRA_STREAM, "file:///sdcard/eoe.mp3");      
sendIntent.setType("audio/mp3");      
startActivity(Intent.createChooser(it, "Choose Email Client"));

//16.进入联系人页面:
Intent intent = new Intent();
intent.setAction(Intent.ACTION_VIEW);
intent.setData(People.CONTENT_URI);
startActivity(intent);

//17.查看指定联系人:
Uri personUri = ContentUris.withAppendedId(People.CONTENT_URI, info.id);//info.id联系人ID
Intent intent = new Intent();
intent.setAction(Intent.ACTION_VIEW);
intent.setData(personUri);
startActivity(intent);
```