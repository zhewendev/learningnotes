# 1 广播概述

参考文献：

1. [API](https://developer.android.com/reference/android/content/BroadcastReceiver)
2. [GUIDES](https://developer.android.com/guide/components/broadcasts)
3. [参考文献一](http://www.runoob.com/w3cnote/android-tutorial-broadcastreceiver.html)

------

Broadcast就是应用间的全局大喇叭，即通信的一个手段。Android中每个应用程序都可以对自己感兴趣的广播进行注册，这样该程序只会接收到自己关心的广播内容。这些广播可能来自系统，也可能来自其他应用程序。发送广播借助Intent，接收广播需要引入一个新的概念——广播接收器（Broadcast Receiver）。

## 广播类型

Android中广播主要分为两种类型：标准广播和有序广播。

![](http://www.runoob.com/wp-content/uploads/2015/08/72916726.jpg)

![](E:\document\photo\first-code\标准广播工作示意图.jpg)

![](E:\document\photo\first-code\有序广播工作示意图.jpg)

# 2 接收系统广播

Android 内置了很多系统级别的广播，可以在应用程序中通过监听这些波光来得到各种系统的状态信息。想要接收这些广播，需要使用广播接收器。注册的方法又分为两种：动态与静态

![](http://www.runoob.com/wp-content/uploads/2015/08/17322218.jpg)

![](http://www.runoob.com/wp-content/uploads/2015/08/93737904.jpg)

## 动态注册（监听网络变化）

创建一个广播接收器，只需要新建一个类，让它继承自BroadcastReceiver，并重写父类的`onReceive()`方法就可以。当有广播来，`onReceive()`方法就会得到执行，具体逻辑可以再这个方法中处理。

示例（**监听网络变化**）。

- 新建一个BroadcastTest项目，修改MainActivity中代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private IntentFilter intentFilter;
  
      private NetworkChangeReceiver networkChangeReceiver;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          intentFilter = new IntentFilter();
          intentFilter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
          networkChangeReceiver = new NetworkChangeReceiver();
          registerReceiver(networkChangeReceiver, intentFilter);
      }
  
      @Override
      protected void onDestroy() {
          super.onDestroy();
          unregisterReceiver(networkChangeReceiver);
      }
  
      class NetworkChangeReceiver extends BroadcastReceiver {
  
          @Override
          public void onReceive(Context context, Intent intent) {
              ConnectivityManager connectionManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
              NetworkInfo networkInfo = connectionManager.getActiveNetworkInfo();
              if (networkInfo != null && networkInfo.isConnected()) {
                  Toast.makeText(context, "network is Connected", Toast.LENGTH_SHORT).show();
              } else {
                  Toast.makeText(context, "network is unavailable", Toast.LENGTH_SHORT).show();
              }
  
  //            Toast.makeText(context,"network changes", Toast.LENGTH_SHORT).show();
          }
      }
  }
  ```

- 在配置文件`AndroidManifest.xml`文件中声明权限

  ```xml
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  ```

**注**：

- 在MainActivity中定义了一个内部类继承BroadcastReceiver，重写了父类的`onReceive()`方法。每当网络状态发生变化时，该方法就会得到执行。
- 在`onCreate()`方法，创建了一个IntentFilter的实例，并给它添加了一个值为**android.net.conn.CONNECTIVITY_CHANGE**的action。当网络状态发生变化时，系统发出的正是一条值为**android.net.conn.CONNETIVITY_CHANGE**的广播。
- 创建一个**NetWorkChangeReceiver**的实例，调用`registerReceiver()`方法进行注册，这样**NetworkChangeReceiver**就会收到所有值为**android.net.conn.CONNETIVITY_CHANGE**的广播，实现监听网络变化功能
- 动态注册的广播接收器一定要取消注册，在`onDestroy()`方法中调用`unregisterReceiver()`方法实现
- 在`onReceiver()`方法中，首先通过`getSystemService()`方法得到了**ConnectivityManager**实例，这是一个系统服务类，专门用于管理网络连接的。
- 调用`getActiveNetworkInfo()`方法可以得到NetworkInfo实例，接着调用**NetworkInfo**的`isConnected()`方法，就可以判断出当前是否有网络，最后通过Toast方式对用户进行提示。

## 静态注册（开机启动）

程序在未启动的情况下就能接收到广播，就需要静态注册的方式。

- 首先右击包新建一个Broadcast Receiver，勾选Exported属性与Enabled属性

  - Exported属性表示是否允许这个广播接收器接收本程序以外的广播
  - Enabled属性表示是否启用这个广播接收器

- 修改广播BootCompleteReceiver中代码：

  ```java
  public class BootCompleteReceiver extends BroadcastReceiver {
  
      @Override
      public void onReceive(Context context, Intent intent) {
          // TODO: This method is called when the BroadcastReceiver is receiving
          Toast.makeText(context, "Boot Complete", Toast.LENGTH_LONG).show();
      }
  }
  ```

- 在**AndroidManifest.xml**中注册

  ```xml
  <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
  ...
  <receiver
              android:name=".BootCompleteReceiver"
              android:enabled="true"
              android:exported="true">
              <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED"/>
              </intent-filter>
          </receiver>
  ```

**注**：

- Android系统启动完成会发出一条值为**android.intent.action.BOOT_COMPELTED**的广播，在`<intent-filter>`中添加相应的action
- 监听系统开机广播需要声明权限，使用`<uses-permission>`标签又加入一条**android.permission.RECEIVE_BOOT_COMPLETED**权限
- <font color = red>不能在`onReceive()`方法中添加过多的逻辑或者耗时操作</font>，广播接收器中不允许开启线程，当`onRecieve()`方法运行了较长时间没有结束，程序就会报错。

# 3 发送自定义广播

发送广播前，要先定义一个广播接收器。

- 自定义一个**BroadcastReceiver**，重写`onReceive()`方法，注册下
  - 标准广播：`sendBroadcast(intent)`
  - 有序广播:`sendOrderedBroadcast(intent, null)`
- 在有序广播的清单文件中的**Intent-filter**通过：**android:priority="100"**设置优先级，值越大优先级越高，越先收到广播，可以调用`abortBroadcast()`截断广播的继续传递优先级可选值：**-1000~1000**之间

## 标准广播

- 首先定义一个广播接收器接受此广播：

  ```java
  public class MyBroadcastReceiver extends BroadcastReceiver {
  
      @Override
      public void onReceive(Context context, Intent intent) {
          // TODO: This method is called when the BroadcastReceiver is receiving
          Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
      }
  }
  ```

- 在**AndroidManifest.xml**文件中进行注册修改

  ```xml
  <receiver
      android:name=".MyBroadcastReceiver"
      android:enabled="true"
      android:exported="true">
      <intent-filter>
          <action android:name="com.vivo.a11085273.broadcasttest.MY_BROADCAST"/>
      </intent-filter>
  </receiver>
  ```

- 在activity_main.xml文件中定义一个按钮，作为发送广播触发点

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <Button
          android:id="@+id/button"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="Send Broadcast"/>
  
  </LinearLayout>
  ```

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
      //...
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button button = (Button) findViewById(R.id.button);
          button.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  Intent intent = new Intent("com.vivo.a11085273.broadcasttest.MY_BROADCAST");
                  intent.setComponent(new ComponentName("com.vivo.a11085273.broadcasttest","com.vivo.a11085273.broadcasttest.MyBroadcastReceiver"));
                  sendBroadcast(intent);
              }
          });
          //...
      }
  }
  ```

**注**：

- 在**Android8.0**之后，对广播做了一些限制，除了有限的例外情况，应用无法使用清单注册隐式广播。 它们仍然可以在运行时注册这些广播，并且可以使用清单注册专门针对它们的显式广播。

- 上述使用静态注册的广播接收器接收自定义的广播，会发生接收不到自定义广播的问题，一种解决方法就是在Android7.0代码基础上在发送广播的基础上加上：

  ```java
  intent.setComponent(new ComponentName("com.vivo.a11085273.broadcasttest","com.vivo.a11085273.broadcasttest.MyBroadcastReceiver"));
  ```

  其中**ComponentName**第一个参数是自定义广播的包名，第二个参数是广播接收器的类



## 有序广播

由于Android8.0后限制了隐式广播，现在新建一个项目BroadcastTest2，自定义一个广播接收器AnotherBroadcastReceiver，动态注册，将前述的广播也改为动态注册，使得两个应用都可以接受到广播。

- 对于有序广播，在BroadcastTest项目中修改MainActivity中代码：

  ```java
  sendOrderedBroadcast(intent,null);
  ```

  将`sendBroadcast()`方法改成`sendOrderedBroadcast()`方法。该方法接收两个参数，第一个参数为Intent，第二个参数是一个权限相关字符。

- 如果想要设定广播接收器优先级，并将广播截断，可以使用`setPriority()`方法，在动态注册处设置优先级：

  ```java
  intentFilter2.setPriority(100);
  ```

  将MyBroadReceiver优先级设置为100，将项目BroadcastTest2中AnotherBroadcastReceiver优先级设为50，则MyBroadReceiver先收到广播

- 如果MyBroadcastReceiver想要截断广播

  ```java
  public class MyBroadcastReceiver extends BroadcastReceiver {
  
      @Override
      public void onReceive(Context context, Intent intent) {
          // TODO: This method is called when the BroadcastReceiver is receiving
          Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show();
          abortBroadcast();
      }
  }
  ```



# 4 本地广播

Android引入了一套本地广播机制，使用这个机制广播只能够在应用程序的内部进行传递，并且广播接收器也只能接收来自应用程序发出的广播。

本地广播主要使用了一个LocalBroadcastManager对广播进行管理，并提供发送广播和注册广播接收器的方法。

**核心方法（使用LocalBroadcastManager管理广播）**：

- 调用`LocalBraodcastManager.getInstance()`获得实例
- 调用`~.registerReceiver()`注册广播
- 调用`~.sendBroadcast()`发送广播
- 调用`~.unregisterReceiver()`取消注册
- ~表示**LocalBroadcastManager**的实例

**注**：本地广播无法通过静态注册方式来接收

**实例**：

- 修改**MainAcitivity.java**中的代码

```java
public class MainActivity extends AppCompatActivity {

    private IntentFilter intentFilter2;
    private LocalReceiver localReceiver;

    private LocalBroadcastManager localBroadcastManager;
    
     @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        localBroadcastManager = LocalBroadcastManager.getInstance(this);//获取实例

        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                 Intent intent = new Intent("com.vivo.a11085273.broadcasttest.LOCALBROADCAST");
                localBroadcastManager.sendBroadcast(intent);
            }
        });
         intentFilter2 = new IntentFilter();
        intentFilter2.addAction("com.vivo.a11085273.broadcasttest.LOCALBROADCAST");
        localReceiver = new LocalReceiver();
        localBroadcastManager.registerReceiver(localReceiver, intentFilter2);//注册监听广播
    }
    
    @Override
    protected void onDestroy() {
        super.onDestroy();
         localBroadcastManager.unregisterReceiver(localReceiver);
    }
    
     class LocalReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            Toast.makeText(context, "received local broadcast", Toast.LENGTH_SHORT).show();
        }
    }
}
```

**注**：

- 本地广播无法通过静态注册来接收，比全局系统广播更高效
- 在广播中启动Activity，需要为intent加入**FLAG_ACTIVITY_NEW_TASK**的标记，不然会报错，因为需要一个新的栈来存放新打开的Activity
- 广播中弹出的AlertDialog，需要设置对话框类型为**TYPE_SYSTEM_ALERT**，不然无法弹出

