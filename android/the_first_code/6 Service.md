# 1 引言概述

参考文献：

1. [API](https://developer.android.com/reference/android/app/Service)
2.  [参考文献一](http://www.runoob.com/w3cnote/android-tutorial-service-1.html)



------

**Service**是android中实现程序后台运行的解决方案，适合去执行那些不需要和用户交互且长期运行的任务。服务的运行不依赖任何用户界面。

Service并不是运行在一个独立的进程当中，依赖于创建服务时所在的应用程序进程。某个应用程序进程被杀掉时，所有依赖于该进程的服务也会停止。

## Android多线程编程

**<font color =red>相关概念</font>**：

- **程序**：为了完成特定任务，用某种语言编写的一组指令集合(一组**静态代码**)
- **进程**：**运行中的程序**，系统调度与资源分配的一个**独立单位**，操作系统会 为每个进程分配一段内存空间！程序的依次动态执行，经历代码的加载，执行， 执行完毕的完整过程！
- **线程**：比进程更小的执行单元，每个进程可能有多条线程，**线程**需要放在一个 **进程**中才能执行，**线程由程序**负责管理，而**进程则由系统**进行调度！
- **多线程的理解**：**并行**执行多个条指令，将**CPU时间片**按照调度算法分配给各个 线程，实际上是**分时**执行的，只是这个切换的时间很短，用户感觉到"同时"而已！

![](http://www.runoob.com/wp-content/uploads/2015/08/21242330.jpg)

### 线程基本用法

创建线程有三种方式：

1. **继承Thread类**：新建一个类，然后重写父类的run()方法

   ```java
   class MyThread extends Thread {
       @override
       public void run() {
           
       }
   }
   ......
       new MyThread().start();//启动线程
   ```

2. **实现Runnable接口**：这种方法可以降低耦合性。

   ```java
   class Mythread implements Runnable {
       @override
       public void run() {
           
       }
   }
   ......//启动线程
       MyThread mythread = new Mythread();
   new Thread(mythread).start();
   ```

   如果需要采用匿名类的方法：

   ```java
   new Thread(new Runnable() {
       public void run() {
           
       }
   }).start();
   ```

3. **实现Callable接口**：与方式2类似



## 在子线程中更新UI（Handler）

Android中UI是线程不安全的，想要更新应用程序中的UI元素，必须在主线程中进行，否则会出现异常。

Android提供了一套异步消息处理机制，完美解决了在子线程中进行UI操作的问题。

**示例**：修改**activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/change_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Change Text"/>
    <TextView
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Hello World!"
        android:textSize="20sp"/>

</RelativeLayout>
```

修改**MainActivity.java**

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener{

    public static final int UPDATE_TEXT = 1;

    private TextView text;

    private Handler handler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case UPDATE_TEXT:
                    text.setText("Nice to meet you");
                    break;
                default:
                    break;
            }
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        text = (TextView) findViewById(R.id.text);
        Button changeText = (Button) findViewById(R.id.change_text);
        changeText.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.change_text:
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        Message message = Message.obtain();
                        message.what = UPDATE_TEXT;
                        handler.sendMessage(message);
                    }
                }).start();
                break;
            default:
                break;
        }
    }
}
```

定义了一个整型常量UPDATA_TEXT，表示更新TextView这个动作，然后新增一个Handler对象，重写父类的`handleMessage()`方法，对Message进行处理。发现Message的what字段值等于UPDATE_TEXT,就将TextView显示的内容改成。。

在子线程中创建了一个Message(android.os.Message)对象，并将其what字段指定为UPDATE_TEXT,然后调用Handler的`sendMessage()`方法将这条Message发送出去。Handler接收到这条Message，在`handlerMessage()`方法中中对它进行处理。HandleMessage()方法就是在主线程当中运行。对Message携带的what字段值进行判断，如果等于UPDATE_TEXT，修改显示内容。

### 异步消息处理机制

异步消息处理主要有4部分组成：Message、Handler、MessageQueue、Looper。

- **Message**：

  用于在线程之中传递消息，可在内部携带少量信息，用于在不同线程之间交换数据。除了what字段，还可以使用arg1和arg2字段携带一些整型数据，使用obj字段携带一个Object对象。

- **Handler**：

  用于发送和处理消息，发送消息一般是使用Handler的sendMessage()方法，发出消息最宗会传递到Handler的`hanleMessage()`方法中

- **MessageQueue**：

  用于存放所有通过Handler发送的消息。这部分消息会一直存在于消息队列中，等待被处理。每个线程只会有一个MessageQueue对象。

- **Looper**:

  是每个线程中的MessageQueue管家，调用Looper的`loop()`,就会进入一个无限循环当中，每当发现MessageQueue中存在一条消息，就将它取出，并传递到Handler的`handleMessage()`方法中。每个线程也只会有一个Looper对象。

**异步消息的整个流程**如下：

​	首先在主线程当中创建一个Handler对象，并重写`handleMessage()`方法。然后子线程当中需要进行UI操作时，创建一个Message对象，并通过Handler将这条消息发出去。之后这条消息会被添加到MessageQueue队列中等待被处理，而Looper则会已知尝试从MessageQueue中取出待处理消息，最后分发回Handler的`handleMessage()`方法中。由于Handle是在主线程中创建的，所以`handleMessage()`方法代码也在主线程中运行。

![](E:\document\photo\first-code\异步消息处理机制流程图.jpg)



### 使用AsyncTask

AsyncTask可以很方便在子线程中对UI进行操作，也是一个基于异步消息处理机制的工具，Android做了很好的封装。

由于AsyncTask是一个**抽象类**，必须要创建一个子类去继承它。在继承时可以为AsyncTask类指定3各泛型参数：

- **Params**：执行AsyncTask时需要传入的参数，用于后台任务中使用
- **Progress**：后台任务执行时，需要在界面显示当前的进度，则使用这里的指定的泛型作为进度单位
- **Result**：任务执行完毕，需要对结果进行返回，则使用指定的泛型作为返回值类型。

示例：

```java
class DownloadTask extends AsyncTack<Void, Integer, Boolean>{
    
}
```

前述例子将第一个泛型参数指定为Void，无需传入参数；第二个泛型参数指定为Integer，表示使用整型数据作为进度显示单位；第三个泛型参数指定为Boolean，使用布尔型数据来反馈执行结果。

AsyncTack经常需要**重写的方法有4个**：

- **onPreExecute()**：

  这个方法在后台任务开始执行之前调用，用于进行一些界面的初始化操作，比如显示进度条对话框之类

- **doInBackgroud(Params...)**：

  这个方法中所有代码都会在子线程中运行，在这里处理所有耗时任务。任务完成就可以通过return语句将任务执行结果返回。如果AsyncTask第三个泛型参数指定的是Void，就可以不返回任务执行结果。（这个方法中不可以进行UI操作，如需UI操作，例如反馈当前任务的执行进度，可以调用`publishProgress(Progress...)`方法完成）

- **onProgressUpdate(Progress...)**

  后台任务中调用了`publishProgress(Progress...)`方法后，这个方法很快就会被调用，携带的参数是后台任务中传递过来的。这个方法中可以对UI进行操作。

- **onPostExecute(Result)**：

  后台执行完毕并通过return语句进行返回时，这个方法很快就会被调用。返回的数据会作为参数传递到此方法中，可以利用返回的数据进行UI操作，例如提醒任务执行结果，关闭进度条对话框等。

简单的一个自定义示例：

```java
public class DownloadTask extends AsyncTask {

    @Override
    protected void onPreExecute() {
        progressDialog.show(); //显示进度对话框
    }

    @Override
    protected Object doInBackground(Object[] objects) {
        try {
            while (true) {
                int downloadPercent = doDownload();
                publishProgress(downloadPercent);
                if (downloadPercent >= 100) {
                    break;
                }
            }
        } catch (Exception e) {
            return false;
        }
        return true;
    }

    @Override
    protected void onProgressUpdate(Object[] values) {
        //更新下载进度
        progressDialog.setMessage("Download " + values[0] + "%");
    }

    @Override
    protected void onPostExecute(Object o) {
        progressDialog.dismiss();//关闭进度对话框
        //这里提示下载结果
        if (o) {
            Toast.makeText(context, "Download succeeded", Toast.LENGTH_SHORT).show();
        } else {
            Toast.makeText(context, "Download failed", Toast.LENGTH_SHORT).show();
        }
    }
}
```

**使用AsyncTask的技巧**就是：`doInBackgroud()`方法中执行具体的耗时任务，在`onProgressUpdate()`方法中进行UI操作，在`onPostExecute()`方法中执行一些任务的收尾工作。

 若要启动该任务，则需编写：

```java
new DownloadTask().execute();
```

实例：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    tools:context=".MainActivity">

    <Button
        android:layout_centerInParent="true"
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="点我加载"/>

    <TextView
        android:id="@+id/text"
        android:layout_below="@+id/button"
        android:layout_centerInParent="true"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="还没开始加载!" />

    <ProgressBar
        android:layout_below="@+id/text"
        android:id="@+id/progress_bar"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:progress="0"
        android:max="100"
        style="?android:attr/progressBarStyleHorizontal"/>

    <Button
        android:layout_below="@+id/progress_bar"
        android:layout_centerInParent="true"
        android:id="@+id/cancel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="cancel"/>
</RelativeLayout>
```

```java
public class MainActivity extends AppCompatActivity {

    // 线程变量
    MyTask mTask;

    // 主布局中的UI组件
    Button button,cancel; // 加载、取消按钮
    TextView text; // 更新的UI组件
    ProgressBar progressBar; // 进度条

    /**
     * 步骤1：创建AsyncTask子类
     * 注：
     *   a. 继承AsyncTask类
     *   b. 为3个泛型参数指定类型；若不使用，可用java.lang.Void类型代替
     *      此处指定为：输入参数 = String类型、执行进度 = Integer类型、执行结果 = String类型
     *   c. 根据需求，在AsyncTask子类内实现核心方法
     */
    private class MyTask extends AsyncTask<String, Integer, String> {

        // 方法1：onPreExecute（）
        // 作用：执行 线程任务前的操作
        @Override
        protected void onPreExecute() {
            text.setText("加载中");
            // 执行前显示提示
        }


        // 方法2：doInBackground（）
        // 作用：接收输入参数、执行任务中的耗时操作、返回 线程任务执行的结果
        // 此处通过计算从而模拟“加载进度”的情况
        @Override
        protected String doInBackground(String... params) {

            try {
                int count = 0;
                int length = 1;
                while (count<99) {

                    count += length;
                    // 可调用publishProgress（）显示进度, 之后将执行onProgressUpdate（）
                    publishProgress(count);
                    // 模拟耗时任务
                    Thread.sleep(50);
                }
            }catch (InterruptedException e) {
                e.printStackTrace();
            }

            return null;
        }

        // 方法3：onProgressUpdate（）
        // 作用：在主线程 显示线程任务执行的进度
        @Override
        protected void onProgressUpdate(Integer... progresses) {

            progressBar.setProgress(progresses[0]);
            text.setText("loading..." + progresses[0] + "%");

        }

        // 方法4：onPostExecute（）
        // 作用：接收线程任务执行结果、将执行结果显示到UI组件
        @Override
        protected void onPostExecute(String result) {
            // 执行完毕后，则更新UI
            text.setText("加载完毕");
        }

        // 方法5：onCancelled()
        // 作用：将异步任务设置为：取消状态
        @Override
        protected void onCancelled() {

            text.setText("已取消");
            progressBar.setProgress(0);

        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 绑定UI组件
        setContentView(R.layout.activity_main);

        button = (Button) findViewById(R.id.button);
        cancel = (Button) findViewById(R.id.cancel);
        text = (TextView) findViewById(R.id.text);
        progressBar = (ProgressBar) findViewById(R.id.progress_bar);

        /**
         * 步骤2：创建AsyncTask子类的实例对象（即 任务实例）
         * 注：AsyncTask子类的实例必须在UI线程中创建
         */
        mTask = new MyTask();

        // 加载按钮按按下时，则启动AsyncTask
        // 任务完成后更新TextView的文本
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                /**
                 * 步骤3：手动调用execute(Params... params) 从而执行异步线程任务
                 * 注：
                 *    a. 必须在UI线程中调用
                 *    b. 同一个AsyncTask实例对象只能执行1次，若执行第2次将会抛出异常
                 *    c. 执行任务中，系统会自动调用AsyncTask的一系列方法：onPreExecute() 、doInBackground()、onProgressUpdate() 、onPostExecute()
                 *    d. 不能手动调用上述方法
                 */
                mTask.execute();
            }
        });

        cancel = (Button) findViewById(R.id.cancel);
        cancel.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 取消一个正在执行的任务,onCancelled方法将会被调用
                mTask.cancel(true);
            }
        });

    }
}
```

# 2 Service基本用法

![](http://www.runoob.com/wp-content/uploads/2015/08/11165797.jpg)

从上图可以看出，**Android中使用Service的方式有两种**：

- `StartService()`启动Service
- `BindService()`启动Service
- 额外其实还有一种，就是启动Service后，绑定Service

**前述相关方法介绍**：

- **onCreate()**:当Service第一次被创建后立即回调该方法，该方法在整个生命周期 中只会调用一次！
- **onDestory()**：当Service被关闭时会回调该方法，该方法只会回调一次！
- **onStartCommand(intent,flag,startId)**：早期版本是onStart(intent,startId), 当客户端调用startService(Intent)方法时会回调，**<font color = red>可多次调用StartService方法</font>**， 但不会再创建新的Service对象，而是继续复用前面产生的Service对象，但会继续回调 onStartCommand()方法！
- **IBinder onBind(intent)**：该方法是Service都必须实现的方法，该方法会返回一个 IBinder对象，app通过该对象与Service组件进行通信！
- **onUnbind(intent)**：当该Service上绑定的所有客户端都断开时会回调该方法！

## 生命周期验证

### StartService启动Service

（自定义Service，重写相关方法，在log上打印验证）

- 修改布局文件（activity_main.xml）

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <Button
          android:id="@+id/start_service"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="start service" />
      <Button
         android:id="@+id/stop_service"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="Stop Service"/>
  
  </LinearLayout>
  ```

- 自定义一个Service

  ```java
  public class MyService extends Service {
      public MyService() {
      }
  
      @Override
      public IBinder onBind(Intent intent) {
          // TODO: Return the communication channel to the service.
          throw new UnsupportedOperationException("Not yet implemented");
      }
  
      @Override
      public void onCreate() {
          super.onCreate();
          Log.d("MyService", "onCreate executed");
      }
  
      @Override
      public int onStartCommand(Intent intent, int flags, int startId) {
          Log.d("MyService", "onStartCommand executed");
          return super.onStartCommand(intent, flags, startId);
      }
  
      @Override
      public void onDestroy() {
          super.onDestroy();
          Log.d("MyService", "onDestroy executed");
      }
  }
  ```

- 修改**MainActivity.java**

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button startService = (Button) findViewById(R.id.start_service);
          Button stopService = (Button) findViewById(R.id.stop_service);
          startService.setOnClickListener(this);
          stopService.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.start_service:
                  Intent startIntent = new Intent(this, MyService.class);
                  startService(startIntent);
                  break;
              case R.id.stop_service:
                  Intent stopIntent = new Intent(this, MyService.class);
                  stopService(stopIntent);
                  break;
              default:
                  break;
  
          }
      }
  }
  ```

- 在AndroidManifest.xml文件中进行注册（四大组件都需要注册）

  ```xml
  <service
      android:name=".MyService"
      android:enabled="true"
      android:exported="true"></service>
  ```

**注**：

- **exported**属性表示是否允许除了当前程序之外的其他程序访问这个服务
- **enabled**属性表示是否启用这个服务。
- 服务的启动和停止的方法，主要借助了Intent实现。
- `startService()`和`stopService()`方法都是定义在Context类中，可以直接调用这两个方法来启动和停止服务。
- 如要服务自行停止，在MyService的任何一个位置调用`stopSelf()`方法就可以停止服务

### BindService启动Service

前述的方式启动服务之后，活动与服务基本就没有关系了。而用Binder方法可以让活动和服务的关系更加紧密。

**基本知识介绍**：

- **ServiceConnection对象**:监听访问者与Service间的连接情况,如果成功连接,回调 `onServiceConnected()`,如果异常终止或者其他原因终止导致Service与访问者断开 连接则回调`onServiceDisconnected()`方法,调用`unBindService()`不会调用该方法!

- `onServiceConnected()`方法中有一个IBinder对象,**该对象即可实现与被绑定Service 之间的通信**!我们在开发Service类时,默认需要实现`IBinder onBind()`方法,**该方法返回的 IBinder对象会传到ServiceConnection对象中的onServiceConnected的参数**,我们就可以 在这里通过这个IBinder与Service进行通信!

**<font color = red>步骤流程</font>**：

- 在自定义的Service中继承Binder，实现自己的IBinder对象
- 通过`onBind()`方法返回自己的IBinder对象
- 绑定该Service的类中定义一个ServiceConnection对象，重写两个方法，`onServiceConnected()`和`onServiceDisconnected()`,然后直接读取IBinder传递过来的参数即可。

示例（在MyService里提供一个下载功能，活动中决定何时下载，查看下载进度）：

- 创建一个专门的Binder对象对下载功能进行管理，修改MyService中代码

  ```java
  public class MyService extends Service {
  
      private DownloadBinder mBinder = new DownloadBinder();
  
      class DownloadBinder extends Binder {
  
          public void startDownload() {
              Log.d("MyService", "startDownload");
          }
  
          public int getProgress() {
              Log.d("MyService", "getProgress executed");
              return 0;
          }
      }
  
      public MyService() {
      }
  
      @Override
      public IBinder onBind(Intent intent) {
          // TODO: Return the communication channel to the service.
          return mBinder;
      }
      ...
   }
  ```

- 在布局文件中新增两个按钮：

  ```xml
  <Button
      android:id="@+id/bind_service"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Bind Service"/>
  <Button
      android:id="@+id/unbind_service"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Unbind Service"/>
  ```

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      private MyService.DownloadBinder downloadBinder;
      private ServiceConnection connection = new ServiceConnection() {
          @Override
          public void onServiceConnected(ComponentName name, IBinder service) {
              downloadBinder = (MyService.DownloadBinder) service;
              downloadBinder.startDownload();
              downloadBinder.getProgress();
          }
  
          @Override
          public void onServiceDisconnected(ComponentName name) {
          }
      };
      
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button startService = (Button) findViewById(R.id.start_service);
          Button stopService = (Button) findViewById(R.id.stop_service);
          startService.setOnClickListener(this);
          stopService.setOnClickListener(this);
          Button bindService = (Button) findViewById(R.id.bind_service);
          Button unbindService = (Button) findViewById(R.id.unbind_service);
          bindService.setOnClickListener(this);
          unbindService.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.start_service:
                  Intent startIntent = new Intent(this, MyService.class);
                  startService(startIntent);
                  break;
              case R.id.stop_service:
                  Intent stopIntent = new Intent(this, MyService.class);
                  stopService(stopIntent);
                  break;
              case R.id.bind_service:
                  Intent bindIntent = new Intent(this, MyService.class);
                  bindService(bindIntent, connection, BIND_AUTO_CREATE);
                  break;
              case R.id.unbind_service:
                  unbindService(connection);
                  break;
              default:
                  break;
  
          }
      }
  }
  
  ```

**注**：

- **bindService()**方法接收3个参数：第一个参数就是刚构建的Intent对象；第二个参数就是前面创建出的ServiceConnection的实例；第三个参数则是一个标志位
  - BIND_AUTO_CREATE表示活动和服务进行绑定后自动创建服务。这使得MyService中`onCreate()`方法得到执行。
- 绑定多客户端情况需要解除所有 的绑定才会调用onDestoryed方法进行销毁哦！



# 3 IntentService的使用

如果直接把耗时线程放到Service中的`onStart()`方法中，容易引起**ANR(Application Not Responding)**异常.Service不是一个单独的进程，与应用程序同在一个进程中；Service不是一个线程，避免在Service中进行耗时操作。

我们可以用到之前的多线程技术，在服务的每个具体方法里开启一个子线程，在这里去处理耗时的逻辑，则一个标准的逻辑可以写成：

```java
public class MyService extends Service {
    ...
    @override
        public int onStartCommand(Intent intent, int flags, int startId) {
        new Thread(new Runnable() {
            @override
            public void run() {
                //处理具体逻辑
                stopself();//执行完毕自动停止服务
            }
        }).start();
        return super.onStartCommand(intent, flags, startId);
    }
}
```

前述方法容易忘记开启线程，或者忘记调用stopSelf()方法。

IntentService继承于Service并处理异步请求的一个类，在IntentService中有一个工作线程来处理耗时操作，请求的Intent记录会加入队列。

**工作流程**：

客户端通过startService(Intent)来启动IntentService，不需要手动地区控制IntentService,当任务执行完后,IntentService会自动停止; 可以启动IntentService多次,每个耗时操作会以工作队列的方式在IntentService的 **onHandleIntent**回调方法中执行,并且每次只会执行一个工作线程,执行完一，再到二这样!



**示例**：

- 新建一个MyIntentService类继承IntentService

  ```java
  public class MyIntentService extends IntentService {
  
      public MyIntentService() {
          super("MyIntentService");
      }
  
      @Override
      protected void onHandleIntent(Intent intent) {
          Log.d("MyIntentService", "Thread id is" + Thread.currentThread().getId());
      }
  
      @Override
      public void onDestroy() {
          super.onDestroy();
          Log.d("MyIntentService", "onDestory ececuted");
      }
  }
  ```

- 修改activity_main.xim，加入一个用于启动MyIntentService这个服务的按钮

  ```xml
  <Button
      android:id="@+id/start_intent_service"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Start IntentService" />
  ```

- 最后修改MainActivity中代码：

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
      //...
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          //...
          Button startIntentService = (Button) findViewById(R.id.start_intent_service);
          startIntentService.setOnClickListener(this);
      }
      
       @Override
      public void onClick(View v) {
          switch (v.getId()) {
                  //...
                  case R.id.start_intent_service:
                  Log.d("MainActivity", "Thread id is" + Thread.currentThread().getId());
                  Intent intentService = new Intent(this, MyIntentService.class);
                  startService(intentService);
                  break;
                   default:
                  break;
  
          }
      }
  }
  ```

- 最后，在AndroidManifest.xml中注册服务

  ```xml
  <service android:name=".MyIntentService" />
  ```

**注**：

- 自定义的IntentService类需要先提供一个无参构造器，并且必须在其内部调用父类的有参构造函数。
- 要在子类中实现`onHandleIntent()`这个抽象方法，在这个方法中处理一些具体的逻辑
- 这个服务运行结束后自动停止，重写了`onDestroy()`方法。



# 4 前台服务(待研究)

Service一般都是运行在后台的，但是Service的系统优先级 还是比较低的，当系统内存不足的时候，就有可能回收正在后台运行的Service， 对于这种情况我们可以使用前台服务，从而让Service稍微没那么容易被系统杀死。

前台服务与普通服务之间**最大的区别在于**：它会有一个正在运行的图标在系统的状态栏显示，下拉状态栏可以看到更加详细的信息。

在**<font color=red>Android 8.0</font>**之前未对后台服务做过多限制，创建一个前台服务并不复杂，修改前述的MyService中代码：

```java
public class MyService extends Service {
    //.......
    @Override
    public void onCreate() {
        super.onCreate();
        Log.d("MyService", "onCreate executed");
        Intent intent = new Intent(this, MainActivity.class);
        PendingIntent pi = PendingIntent.getActivity(this, 0, intent, 0);
        Notification notification = new NotificationCompat.Builder(this)
                .setContentTitle("this is the content title")
                .setContentText("this is the content text")
                .setWhen(System.currentTimeMillis())
                .setSmallIcon(R.mipmap.ic_launcher)
                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                .setContentIntent(pi)
                .build();
        startForeground(1, notification);
    }
    //......
}
```





# 5 AIDL

**参考文献**：

- 参考文献一：[Android开发指南](https://developer.android.com/guide/components/aidl#PassingObjects)
- 参考文献二：[Qiujuer博客](https://blog.csdn.net/qiujuer/article/details/46885987)
- 参考文献三：[掘金博客](https://juejin.im/entry/59c33643f265da06624e5670)

------

**AIDL** (Android Interface Definition Language) 是一种**IDL** 语言，用于生成可以在Android设备上两个进程之间进行进程间通信(interprocess communication, **IPC**)的代码。在 Android 上，一个进程通常无法访问另一个进程的内存。如果在一个进程中（例如Activity）要调用另一个进程中（例如Service）对象的操作，就可以使用AIDL生成可序列化的参数。

**注**：

只有允许不同应用的客户端用 IPC 方式访问服务，并且想要在服务中处理多线程时，才有必要使用 AIDL。 如果您不需要执行跨越不同应用的并发 IPC，就应该通过[实现一个 Binder](https://developer.android.com/guide/components/bound-services.html?hl=zh-cn#Binder) 创建接口；或者，如果您想执行 IPC，但根本不需要处理多线程，则[使用 Messenger 类](https://developer.android.com/guide/components/bound-services.html?hl=zh-cn#Messenger)来实现接口。

**传值方式**：

AIDL允许跨进程传递值，一般来说有三种：

- **广播**：传递小数据
- **文件**：保存到文件中，然后读取，传递大数据
- **Service Bind模式**：居中模式，效率较高，但是较为麻烦



**传递类型**：

在AIDL中使用Bind进行传递会比较麻烦是因为：在跨进程情况下只允许如下类型数据：

- String
- CharSequence
- android.os.Parcelable
- java.util.List
- java.util.Map
- 基本数据类型：**byte,char,int,long,float,double,boolean**



**简单的流程**：

![](E:\document\photo\first-code\AIDL简单流程.jpg)

其调用方式依然为：**绑定服务->得到目标服务的Binder->调用对应方法**。 
跨进程传递数据麻烦就在于打包/解包Parcelable的操作。

## AIDL使用

### 创建aidl

- 首先创建一个android工程，充当跨进程通信的服务端。同时创建一个aidl文件。

  ![](E:\document\photo\first-code\aidl文件夹.jpg)

- 修改aidl文件：

  ```java
  // IMyAidlInterface.aidl
  package com.vivo.a11085273.othertest;
  
  // Declare any non-default types here with import statements
  
  interface IMyAidlInterface {
      /**
       * 自己添加的方法
        */
      int add(int value1, int value2);
  }
  ```

- 完成上述操作后手动编译程序，生成aidl对应的Java代码，以便代码能够链接到生成的类

### 实现接口，向客户端开放接口

```java
public class MyAidlService extends Service {
    public MyAidlService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        return iBinder;
    }

    private IBinder iBinder = new IMyAidlInterface.Stub(){
        @Override
        public int add(int value1, int value2) throws RemoteException {
            return value1 + value2;
        }
    };
}
```

们创建了一个service，并在service内部声明了一个IBinder对象，它是一个匿名实现的IMyAidlInterface.Stub的实例，同时我们在发现**IMyAidlInterface.Stub**实例实现了add方法，这个方法正是我们在aidl中声明的供客户端调用的方法。

### 客户端调用aidl

客户端还必须具有对 interface 类的访问权限，因此如果客户端和服务在不同的应用内，则客户端的应用 `src/` 目录内必须包含 `.aidl` 文件（它生成 `android.os.Binder` 接口 — 为客户端提供对 AIDL 方法的访问权限）的副本。

当客户端在 `onServiceConnected()` 回调中收到 `IBinder` 时，它必须调用`*YourServiceInterface*.Stub.asInterface(service)` 以将返回的参数转换成`*YourServiceInterface*` 类型。

- 将服务端的aidl拷贝到客户端，拷贝完编译工程，生成对应的java文件

- 在Activity的onCreate中绑定服务

  ```java
  public class MainActivity extends AppCompatActivity {
  
      IMyAidlInterface aidl;
  
      private ServiceConnection connection = new ServiceConnection() {
          @Override
          public void onServiceConnected(ComponentName name, IBinder service) {
              //绑定服务成功回调
              aidl = IMyAidlInterface.Stub.asInterface(service);
          }
  
          @Override
          public void onServiceDisconnected(ComponentName name) {
              //服务断开时回调
              aidl = null;
          }
      };
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          bindService();
          Button button = (Button) findViewById(R.id.button);
          button.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  try {
                      int result = aidl.add(12, 12);
                      Log.e("MainActivity", "远程回调结果:" + result);
                  } catch (RemoteException e) {
                      e.printStackTrace();
                  }
              }
          });
      }
  
      private void bindService(){
          Intent intent = new Intent();
          //Android 5.0开始，启动服务必须使用显示的，不能用隐式的
          intent.setComponent(new ComponentName("com.vivo.a11085273.othertest", "com.vivo.a11085273.othertest.MyAidlService"));
          startService(intent);
          bindService(intent, connection, Context.BIND_AUTO_CREATE);
      }
  }
  ```

（在布局文件中设置了一个按钮，点击按钮进行远程调用，可以从logcat打印结果）



## 通过IPC传递对象

urvey. [Start survey](https://goo.gl/KsxGKm)

App Basics[Introduction](https://developer.android.com/guide/)Build your first app[App fundamentals](https://developer.android.com/guide/components/fundamentals)App resourcesApp manifest fileApp permissionsDevicesDevice compatibilityWearAndroid TVAndroid AutoAndroid ThingsChrome OS devicesCore topicsActivitiesArchitecture ComponentsIntents and intent filtersUser interface & navigationAnimations & transitionsImages & graphicsAudio & videoBackground tasks[Overview](https://developer.android.com/training/best-background)[Guide to background processing](https://developer.android.com/guide/background/index)Sending operations to multiple threads[Schedule jobs intelligently](https://developer.android.com/topic/performance/scheduling)Services[Overview](https://developer.android.com/guide/components/services)[Create a background service](https://developer.android.com/training/run-background-service/create-service)[Send work requests to the background service](https://developer.android.com/training/run-background-service/send-request)[Report work status](https://developer.android.com/training/run-background-service/report-status)[Bound services](https://developer.android.com/guide/components/bound-services)[AIDL overview](https://developer.android.com/guide/components/aidl)[Background optimizations](https://developer.android.com/topic/performance/background-optimization)[Broadcasts overview](https://developer.android.com/guide/components/broadcasts)[Implicit Broadcast Exceptions](https://developer.android.com/guide/components/broadcast-exceptions)Manage device awake stateApp data & filesUser data & identityUser locationTouch & inputCameraSensorsConnectivityRenderscriptWeb-based contentAndroid App BundlesGoogle Play Instant[App Actions](https://developer.android.com/guide/actions/index)SlicesBest practicesTestingPerformanceAccessibilitySecurityBuild for BillionsBuild for EnterpriseGoogle Play

[目录](https://developer.android.com/guide/components/aidl#top_of_page)[定义 AIDL 接口](https://developer.android.com/guide/components/aidl#Defining)[1. 创建 .aidl 文件](https://developer.android.com/guide/components/aidl#Create)[2. 实现接口](https://developer.android.com/guide/components/aidl#Implement)[3. 向客户端公开该接口](https://developer.android.com/guide/components/aidl#Expose)[通过 IPC 传递对象](https://developer.android.com/guide/components/aidl#PassingObjects)[调用 IPC 方法](https://developer.android.com/guide/components/aidl#Calling)















# Android 接口定义语言 (AIDL)

AIDL（Android 接口定义语言）与您可能使用过的其他 IDL 类似。 您可以利用它定义客户端与服务使用进程间通信 (IPC) 进行相互通信时都认可的编程接口。 在 Android 上，一个进程通常无法访问另一个进程的内存。 尽管如此，进程需要将其对象分解成操作系统能够识别的原语，并将对象编组成跨越边界的对象。 编写执行这一编组操作的代码是一项繁琐的工作，因此 Android 会使用 AIDL 来处理。

**注**：只有允许不同应用的客户端用 IPC 方式访问服务，并且想要在服务中处理多线程时，才有必要使用 AIDL。 如果您不需要执行跨越不同应用的并发 IPC，就应该通过[实现一个 Binder](https://developer.android.com/guide/components/bound-services.html#Binder) 创建接口；或者，如果您想执行 IPC，但根本不需要处理多线程，则[使用 Messenger 类](https://developer.android.com/guide/components/bound-services.html#Messenger)来实现接口。无论如何，在实现 AIDL 之前，请您务必理解[绑定服务](https://developer.android.com/guide/components/bound-services.html)。

在您开始设计 AIDL 接口之前，要注意 AIDL 接口的调用是直接函数调用。 您不应该假设发生调用的线程。 视调用来自本地进程还是远程进程中的线程，实际情况会有所差异。 具体而言：

- 来自本地进程的调用在发起调用的同一线程内执行。如果该线程是您的主 UI 线程，则该线程继续在 AIDL 接口中执行。 如果该线程是其他线程，则其便是在服务中执行您的代码的线程。 因此，只有在本地线程访问服务时，您才能完全控制哪些线程在服务中执行（但如果真是这种情况，您根本不应该使用 AIDL，而是应该通过[实现 Binder 类](https://developer.android.com/guide/components/bound-services.html#Binder)创建接口）。
- 来自远程进程的调用分派自平台在您的自有进程内部维护的线程池。 您必须为来自未知线程的多次并发传入调用做好准备。 换言之，AIDL 接口的实现必须是完全线程安全实现。
- `oneway` 关键字用于修改远程调用的行为。使用该关键字时，远程调用不会阻塞；它只是发送事务数据并立即返回。接口的实现最终接收此调用时，是以正常远程调用形式将其作为来自 `Binder` 线程池的常规调用进行接收。 如果 `oneway` 用于本地调用，则不会有任何影响，调用仍是同步调用。

## 定义 AIDL 接口

您必须使用 Java 编程语言语法在 `.aidl` 文件中定义 AIDL 接口，然后将它保存在托管服务的应用以及任何其他绑定到服务的应用的源代码（`src/` 目录）内。

您开发每个包含 `.aidl` 文件的应用时，Android SDK 工具都会生成一个基于该 `.aidl` 文件的 `IBinder` 接口，并将其保存在项目的 `gen/` 目录中。服务必须视情况实现 `IBinder` 接口。然后客户端应用便可绑定到该服务，并调用 `IBinder` 中的方法来执行 IPC。

如需使用 AIDL 创建绑定服务，请执行以下步骤：

1. 创建 .aidl 文件

   此文件定义带有方法签名的编程接口。

2. 实现接口

   Android SDK 工具基于您的 `.aidl` 文件，使用 Java 编程语言生成一个接口。此接口具有一个名为 `Stub` 的内部抽象类，用于扩展 `Binder` 类并实现 AIDL 接口中的方法。您必须扩展 `Stub` 类并实现方法。

3. 向客户端公开该接口

   实现 `Service` 并重写 `onBind()` 以返回 `Stub` 类的实现。

**注意**：在 AIDL 接口首次发布后对其进行的任何更改都必须保持向后兼容性，以避免中断其他应用对您的服务的使用。 也就是说，因为必须将您的 `.aidl` 文件复制到其他应用，才能让这些应用访问您的服务的接口，因此您必须保留对原始接口的支持。

### 1. 创建 .aidl 文件

AIDL 使用简单语法，使您能通过可带参数和返回值的一个或多个方法来声明接口。 参数和返回值可以是任意类型，甚至可以是其他 AIDL 生成的接口。

您必须使用 Java 编程语言构建 `.aidl` 文件。每个 `.aidl` 文件都必须定义单个接口，并且只需包含接口声明和方法签名。

默认情况下，AIDL 支持下列数据类型：

- Java 编程语言中的所有原语类型（如 `int`、`long`、`char`、`boolean` 等等）

- `String`

- `CharSequence`

- List

  `List` 中的所有元素都必须是以上列表中支持的数据类型、其他 AIDL 生成的接口或您声明的可打包类型。 可选择将 `List` 用作“通用”类（例如，`List<String>`）。另一端实际接收的具体类始终是 `ArrayList`，但生成的方法使用的是 `List` 接口。

- Map

  `Map` 中的所有元素都必须是以上列表中支持的数据类型、其他 AIDL 生成的接口或您声明的可打包类型。 不支持通用 Map（如 `Map<String,Integer>` 形式的 Map）。 另一端实际接收的具体类始终是 `HashMap`，但生成的方法使用的是 `Map` 接口。

您必须为以上未列出的每个附加类型加入一个 `import` 语句，即使这些类型是在与您的接口相同的软件包中定义。

定义服务接口时，请注意：

- 方法可带零个或多个参数，返回值或空值。

- 所有非原语参数都需要指示数据走向的方向标记。可以是

   

  in

  、

  out

   

  或

   

  inout

  （见以下示例）。

  原语默认为 `in`，不能是其他方向。

  **注意**：您应该将方向限定为真正需要的方向，因为编组参数的开销极大。

- `.aidl` 文件中包括的所有代码注释都包含在生成的 `IBinder` 接口中（import 和 package 语句之前的注释除外）

- 只支持方法；您不能公开 AIDL 中的静态字段。

以下是一个 `.aidl` 文件示例：

```
// IRemoteService.aidl
package com.example.android;

// Declare any non-default types here with import statements

/** Example service interface */
interface IRemoteService {
    /** Request the process ID of this service, to do evil things with it. */
    int getPid();

    /** Demonstrates some basic types that you can use as parameters
     * and return values in AIDL.
     */
    void basicTypes(int anInt, long aLong, boolean aBoolean, float aFloat,
            double aDouble, String aString);
}
```

只需将您的 `.aidl` 文件保存在项目的 `src/` 目录内，当您开发应用时，SDK 工具会在项目的`gen/` 目录中生成 `IBinder` 接口文件。生成的文件名与 `.aidl` 文件名一致，只是使用了`.java` 扩展名（例如，`IRemoteService.aidl` 生成的文件名是 `IRemoteService.java`）。

如果您使用 Android Studio，增量编译几乎会立即生成 Binder 类。 如果您不使用 Android Studio，则 Gradle 工具会在您下一次开发应用时生成 Binder 类 — 您应该在编写完 `.aidl` 文件后立即用 `gradle assembleDebug` （或 `gradle assembleRelease`）编译项目，以便您的代码能够链接到生成的类。

### 2. 实现接口

当您开发应用时，Android SDK 工具会生成一个以 `.aidl` 文件命名的 `.java` 接口文件。生成的接口包括一个名为 `Stub` 的子类，这个子类是其父接口（例如，`YourInterface.Stub`）的抽象实现，用于声明 `.aidl` 文件中的所有方法。

**注：**`Stub` 还定义了几个帮助程序方法，其中最引人关注的是 `asInterface()`，该方法带 `IBinder`（通常便是传递给客户端 `onServiceConnected()` 回调方法的参数）并返回存根接口实例。 如需了解如何进行这种转换的更多详细信息，请参见[调用 IPC 方法](https://developer.android.com/guide/components/aidl#Calling)一节。

如需实现 `.aidl` 生成的接口，请扩展生成的 `Binder` 接口（例如，`YourInterface.Stub`）并实现从 `.aidl` 文件继承的方法。

以下是一个使用匿名实例实现名为 `IRemoteService` 的接口（由以上 `IRemoteService.aidl`示例定义）的示例：

```
private final IRemoteService.Stub mBinder = new IRemoteService.Stub() {
    public int getPid(){
        return Process.myPid();
    }
    public void basicTypes(int anInt, long aLong, boolean aBoolean,
        float aFloat, double aDouble, String aString) {
        // Does nothing
    }
};
```

现在，`mBinder` 是 `Stub` 类的一个实例（一个 `Binder`），用于定义服务的 RPC 接口。 在下一步中，将向客户端公开该实例，以便客户端能与服务进行交互。

在实现 AIDL 接口时应注意遵守以下这几个规则：

- 由于不能保证在主线程上执行传入调用，因此您一开始就需要做好多线程处理准备，并将您的服务正确地编译为线程安全服务。
- 默认情况下，RPC 调用是同步调用。如果您明知服务完成请求的时间不止几毫秒，就不应该从 Activity 的主线程调用服务，因为这样做可能会使应用挂起（Android 可能会显示“Application is Not Responding”对话框）— 您通常应该从客户端内的单独线程调用服务。
- 您引发的任何异常都不会回传给调用方。

### 3. 向客户端公开该接口

您为服务实现该接口后，就需要向客户端公开该接口，以便客户端进行绑定。 要为您的服务公开该接口，请扩展 `Service` 并实现 `onBind()`，以返回一个类实例，这个类实现了生成的`Stub`（见前文所述）。以下是一个向客户端公开 `IRemoteService` 示例接口的服务示例。

```
public class RemoteService extends Service {
    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public IBinder onBind(Intent intent) {
        // Return the interface
        return mBinder;
    }

    private final IRemoteService.Stub mBinder = new IRemoteService.Stub() {
        public int getPid(){
            return Process.myPid();
        }
        public void basicTypes(int anInt, long aLong, boolean aBoolean,
            float aFloat, double aDouble, String aString) {
            // Does nothing
        }
    };
}
```

现在，当客户端（如 Activity）调用 `bindService()` 以连接此服务时，客户端的 `onServiceConnected()` 回调会接收服务的 `onBind()` 方法返回的 `mBinder` 实例。

客户端还必须具有对 interface 类的访问权限，因此如果客户端和服务在不同的应用内，则客户端的应用 `src/` 目录内必须包含 `.aidl` 文件（它生成 `android.os.Binder` 接口 — 为客户端提供对 AIDL 方法的访问权限）的副本。

当客户端在 `onServiceConnected()` 回调中收到 `IBinder` 时，它必须调用`*YourServiceInterface*.Stub.asInterface(service)` 以将返回的参数转换成`*YourServiceInterface*` 类型。例如：

```
IRemoteService mIRemoteService;
private ServiceConnection mConnection = new ServiceConnection() {
    // Called when the connection with the service is established
    public void onServiceConnected(ComponentName className, IBinder service) {
        // Following the example above for an AIDL interface,
        // this gets an instance of the IRemoteInterface, which we can use to call on the service
        mIRemoteService = IRemoteService.Stub.asInterface(service);
    }

    // Called when the connection with the service disconnects unexpectedly
    public void onServiceDisconnected(ComponentName className) {
        Log.e(TAG, "Service has unexpectedly disconnected");
        mIRemoteService = null;
    }
};
```

如需查看更多示例代码，请参见 [ApiDemos](https://developer.android.com/resources/samples/ApiDemos/index.html) 中的 [`RemoteService.java`](https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/app/RemoteService.html) 类。

## 通过 IPC 传递对象

通过 IPC 接口把某个类从一个进程发送到另一个进程是可以实现的。 不过，您必须确保该类的代码对 IPC 通道的另一端可用，并且**该类必须支持 `Parcelable` 接口**。

Parcelable是类似于Java中的Serializable，Android中定义了Parcelable，用于进程间数据传递，对传输数据进行分解，编组的工作，相对于Serializable，他对于进程间通信更加高效。

**操作流程**：

1. 让您的类实现 `Parcelable` 接口。

2. 实现 `writeToParcel`，它会获取对象的当前状态并将其写入 `Parcel`。

3. 为您的类添加一个名为 `CREATOR` 的静态字段，这个字段是一个实现`Parcelable.Creator` 接口的对象。

4. 最后，创建一个声明可打包类的.aidl 文件。

   如果您使用的是自定义编译进程，切勿在您的编译中添加 `.aidl` 文件。 此 `.aidl` 文件与 C 语言中的头文件类似，并未编译。

**示例**：

- 定义一个User类，实现了**Parcelable**接口（Parcelable对数据进行分解/编组的时候必须使用相同的顺序，字段以什么顺序分解的，编组时就以什么顺序读取数据）

  ```java
  public class MyAidlService extends Service {
  
      private ArrayList users;
  
      public MyAidlService() {
      }
  
      @Override
      public IBinder onBind(Intent intent) {
          users = new ArrayList<User>();
          return iBinder;
      }
  
      private IBinder iBinder = new IMyAidlInterface.Stub(){
          @Override
          public int add(int value1, int value2) throws RemoteException {
              return value1 + value2;
          }
  
          @Override
          public List<User> addUser(User user) throws RemoteException {
              users.add(user);
              return users;
          }
      };
  }
  ```

  ```java
  public class User implements Parcelable {
  
      private int id;
      private String name;
  
      public User() {
      }
  	
      public User(int id, String name) {
          this.id = id;
          this.name = name;
      }
  	//从Parcel中读出之前写入的数据
      public User(Parcel in){
          //注意顺序！！！注意顺序！！！注意顺序！！！
          this.id = in.readInt();
          this.name = in.readString();
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  	//实现Parce需重写的方法
      @Override
      public int describeContents() {
          return 0;
      }
  	//把对象的每个字段写入到Parcle中
      @Override
      public void writeToParcel(Parcel dest, int flags) {
          //注意顺序！！！注意顺序！！！注意顺序！！！
          dest.writeInt(id);
          dest.writeString(name);
      }
  	
      public static final  Parcelable.Creator<User> CREATOR = new Parcelable.Creator<User>(){
  		//反序列化，把我们通过writeToParce方法写进的数据再读出来
          @Override
          public User createFromParcel(Parcel source) {
              return new User(source);
          }
  
          @Override
          public User[] newArray(int size) {
              return new User[size];
          }
      };
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  '}';
      }
  }
  ```

- 创建一个aidl文件，定义User，否则User在aidl中将无法识别：

  ```java
  package com.vivo.a11085273.othertest;
  
  parcelable User;
  ```

- 在前述的服务器端的aidl中新增方法。

  ```java
  // IMyAidlInterface.aidl
  package com.vivo.a11085273.othertest;
  import com.vivo.a11085273.othertest.User;
  
  // Declare any non-default types here with import statements
  interface IMyAidlInterface {
      /**
       * 自己添加的方法
        */
      int add(int value1, int value2);
      List<User> addUser(in User user);
  }
  ```

- 在Service中实现新增的addUser方法

  ```java
  public class MyAidlService extends Service {
  
      private ArrayList users;
  
      public MyAidlService() {
      }
  
      @Override
      public IBinder onBind(Intent intent) {
          users = new ArrayList<User>();
          return iBinder;
      }
  
      private IBinder iBinder = new IMyAidlInterface.Stub(){
          @Override
          public int add(int value1, int value2) throws RemoteException {
              return value1 + value2;
          }
  
          @Override
          public List<User> addUser(User user) throws RemoteException {
              users.add(user);
              return users;
          }
      };
  }
  ```

  此时服务端的结构目录为：

  ![](E:\document\photo\first-code\parceable服务端结构.jpg)

- 将服务端的两个aidl文件复制到客户端中，包结构一致，aidl重新编译

- 将User类也复制到客户端，包结构一致

- 在客户端添加addUser操作

  ```java
  button.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View v) {
          try {
              ArrayList<User> users = (ArrayList<User>) aidl.addUser(new User(12, "demaxiya"));
              Log.d("MainActivity", "远程回调结果:" + users.toString());
          } catch (RemoteException e) {
              e.printStackTrace();
          }
      }
  });
  ```

  此时客户端的目录结构为：

  ![](E:\document\photo\first-code\parcelable客户端目录结构.jpg)

- 运行服务端和客户端，点击addUser，输出日志，查看调用

## AIDL原理

 要了解aidl原理，我们需要看一下根据aidl生成的对应的java代码了，

```
public interface IMyAidlInterface extends android.os.IInterface{
    public static abstract class Stub extends android.os.Binder implements com.yunzhou.aidlserver.IMyAidlInterface{...}
    public int add(int value1, int value2) throws android.os.RemoteException;
    public java.util.List<com.yunzhou.aidlserver.User> addUser(com.yunzhou.aidlserver.User user) throws android.os.RemoteException;
}
```

 我们可以看到，生成的代码结构很简单，一个静态抽象类Stub，以及aidl中定义的方法，其中Stub肯定时核心，我们深入阅读

![](https://user-gold-cdn.xitu.io/2017/9/21/ab6587947b90423015cd91727efc9156?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Stub.png

 Stub的目录结构也不复杂，一个构造函数，一个asInterface方法，一个asBinder方法，一个onTransact方法，一个Proxy代理类，这边Proxy与Stub同时实现了我们定义的aidl，且Proxy中实现了我们在aidl中定义的add/addUser方法。下面两个int为告诉系统的方法Id

 在客户端，我们绑定服务的时候通过Stub.asInterface()回去aidl对象，查看asInterface源码，我们不难发现，客户端获取到的其实时Stub.Proxy,一个远程服务的代理。

```
//客户端获取aidl
aidl = IMyAidlInterface.Stub.asInterface(service);

//Stub的asInterface
public static com.yunzhou.aidlserver.IMyAidlInterface asInterface(android.os.IBinder obj) {
  if ((obj==null)) {
    return null;
  }
  android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
  if (((iin!=null)&&(iin instanceof com.yunzhou.aidlserver.IMyAidlInterface))) {
    return ((com.yunzhou.aidlserver.IMyAidlInterface)iin);
  }
  return new com.yunzhou.aidlserver.IMyAidlInterface.Stub.Proxy(obj);
}
```

 所以客户端调用add/addUser方法其实调用的时Stub.Proxy中实现的add/addUser,在Stub.Proxy中我们可以看到一句核心代码

```
//add
mRemote.transact(Stub.TRANSACTION_add, _data, _reply, 0);

//addUser
mRemote.transact(Stub.TRANSACTION_addUser, _data, _reply, 0);
```

 追溯源头，mRemote其实就是IMyAidlInterface.Stub，随意mRemote.transact传递到了IMyAidlInterface.Stub.OnTransact, onTransact中执行了add/addUser,回调到了我们在服务端定义Service中声明IBidner时重写的add/addUser,这就是AIDL的整个流程。

```
private IBinder iBinder = new IMyAidlInterface.Stub(){
  @Override
  public int add(int value1, int value2) throws RemoteException {
    return value1 + value2;
  }

  @Override
  public List<User> addUser(User user) throws RemoteException {
    users.add(user);
    return users;
  }
};
```

![](https://user-gold-cdn.xitu.io/2017/9/21/a81ccab1b5fac20ec2bd5683a1bf0d3d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





