# 1. WebView基本用法

**参考文献**：

参考文献一：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-webview.html)

参考文献二：[API](https://developer.android.com/reference/android/webkit/WebView)

参考文献三：[Carson_Ho简书](https://www.jianshu.com/p/3c94ae673e2a)

参考文献四：第一行代码

参考文献五：[WebChromeClient](http://androiddoc.qiniudn.com/reference/android/webkit/WebChromeClient.html)

参考文献六：[WebSettings](http://androiddoc.qiniudn.com/reference/android/webkit/WebSettings.html)

参考文献七：[WebViewClient](https://developer.android.com/reference/android/webkit/WebViewClient)

------

![](https://upload-images.jianshu.io/upload_images/944365-e8e5a75c56f059a8.jpg)

**什么是WebView**？

Android内置**webkit**内核的高性能浏览器,而WebView则是在这个基础上进行封装后的一个 控件,WebView直译网页视图,我们可以简单的看作一个可以嵌套到界面上的一个浏览器控件！

自Android4.4系统开始，Chromium内核取代了webkit内核，

**作用**？

- 显示和渲染**Web**页面
- 直接使用**html**文件（网络上或本地assets中）作布局
- 可和**JavaScript**交互调用

> WebView控件功能强大，除了具有一般View的属性和设置外，还可以对url请求、页面加载、渲染、页面交互进行强大的处理。

## 基本使用

- 新建项目，修改acitivity_main.xml布局文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent" >
  
      <WebView
          android:id="@+id/web_view"
          android:layout_width="match_parent"
          android:layout_height="match_parent" />
  
  </LinearLayout>
  ```

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          WebView webView = (WebView) findViewById(R.id.web_view);
          webView.getSettings().setJavaScriptEnabled(true);
          webView.setWebViewClient(new WebViewClient());
          webView.loadUrl("https://www.baidu.com");
      }
  
  }
  ```

- 配置权限

  ```xml
  <uses-permission android:name="android.permission.INTERNET" />
  ```

## WebView常用方法

```java
mWebView.loadUrl("http://www.jianshu.com/u/fa272f63280a");// 加载url，也可以执行js函数
    mWebView.setWebViewClient(new SafeWebViewClient());// 设置 WebViewClient 
    mWebView.setWebChromeClient(new SafeWebChromeClient());// 设置 WebChromeClient
    mWebView.onResume();// 生命周期onResume
    mWebView.resumeTimers();//生命周期resumeTimers
    mWebView.onPause();//生命周期onPause
    mWebView.pauseTimers();//生命周期pauseTimers (上数四个方法都是成对出现)
    mWebView.stopLoading();// 停止当前加载
    mWebView.clearMatches();// 清除网页查找的高亮匹配字符。
    mWebView.clearHistory();// 清除当前 WebView 访问的历史记录
    mWebView.clearSslPreferences();//清除ssl信息
    mWebView.clearCache(true);//清空网页访问留下的缓存数据。需要注意的时，由于缓存是全局的，所以只要是WebView用到的缓存都会被清空，即便其他地方也会使用到。该方法接受一个参数，从命名即可看出作用。若设为false，则只清空内存里的资源缓存，而不清空磁盘里的。
    mWebView.loadUrl("about:blank");// 清空当前加载
    mWebView.removeAllViews();// 清空子 View
    if (Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN_MR2) {
        mWebView.removeJavascriptInterface("AndroidNative");// 向 Web端注入 java 对象
    }
    mWebView.destroy();// 生命周期销毁
	//...
```

### 加载Url

加载方式根据资源分为三种：

```java
//方式1. 加载一个网页：
  webView.loadUrl("https://www.google.com/");

  //方式2：加载apk包中的html页面
  webView.loadUrl("file:///android_asset/test.html");

  //方式3：加载手机本地的html页面
   webView.loadUrl("content://com.android.htmlfileprovider/sdcard/test.html");

   // 方式4： 加载 HTML 页面的一小段内容
  WebView.loadData(String data, String mimeType, String encoding)
// 参数说明：
// 参数1：需要截取展示的内容
// 内容里不能出现 ’#’, ‘%’, ‘\’ , ‘?’ 这四个字符，若出现了需用 %23, %25, %27, %3f 对应来替代，否则会出现异常
// 参数2：展示内容的类型
// 参数3：字节码
```

### WebView状态

```java
//激活WebView为活跃状态，能正常执行网页的响应
webView.onResume() ；

//当页面被失去焦点被切换到后台不可见状态，需要执行onPause
//通过onPause动作通知内核暂停所有的动作，比如DOM的解析、plugin的执行、JavaScript执行。
webView.onPause()；

//当应用程序(存在webview)被切换到后台时，这个方法不仅仅针对当前的webview而是全局的全应用程序的webview
//它会暂停所有webview的layout，parsing，javascripttimer。降低CPU功耗。
webView.pauseTimers()
//恢复pauseTimers状态
webView.resumeTimers()；

//销毁Webview
//在关闭了Activity时，如果Webview的音乐或视频，还在播放。就必须销毁Webview
//但是注意：webview调用destory时,webview仍绑定在Activity上
//这是由于自定义webview构建时传入了该Activity的context对象
//因此需要先从父容器中移除webview,然后再销毁webview:
rootLayout.removeView(webView); 
webView.destroy();
```

### 前进/后退网页

```java
//是否可以后退
Webview.canGoBack() 
//后退网页
Webview.goBack()

//是否可以前进                     
Webview.canGoForward()
//前进网页
Webview.goForward()

//以当前的index为起始点前进或者后退到历史记录中指定的steps
//如果steps为负数则为后退，正数则为前进
Webview.goBackOrForward(intsteps)
```

**常见用法**：Back键控制网页后退

- **问题**：在不做任何处理前提下 ，浏览网页时点击系统的“Back”键,整个 Browser 会调用 finish()而结束自身
- **目标**：点击返回后，是网页回退而不是推出浏览器
- **解决方案**：在当前Activity中处理并消费掉该 Back 事件

```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
    if ((keyCode == KEYCODE_BACK) && mWebView.canGoBack()) { 
        mWebView.goBack();
        return true;
    }
    return super.onKeyDown(keyCode, event);
}
```

或者：

```java
@Override
public viod onBackPressed() {
    super.onBackPressed();
    //处理网页后退事件
}
```

### 清除缓存数据

```java
//清除网页访问留下的缓存
//由于内核缓存是全局的因此这个方法不仅仅针对webview而是针对整个应用程序.
Webview.clearCache(true);

//清除当前webview访问的历史记录
//只会webview访问历史记录里的所有记录除了当前访问记录
Webview.clearHistory()；

//这个api仅仅清除自动完成填充的表单数据，并不会清除WebView存储到本地的数据
Webview.clearFormData()；
```

------

# 2. WebView设置

## WebSetting类

- 作用：对WebView进行配置和管理

### 添加网络访问权限

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

### 生成WebView组件

```java
//方式1：直接在在Activity中生成
WebView webView = new WebView(this)

//方法2：在Activity的layout文件里添加webview控件：
WebView webview = (WebView) findViewById(R.id.webView1);
```

### 利用WebSettings子类进行配置

```java
//声明WebSettings子类
WebSettings webSettings = webView.getSettings();

//如果访问的页面中要与Javascript交互，则webview必须设置支持Javascript
webSettings.setJavaScriptEnabled(true);  
// 若加载的 html 里有JS 在执行动画等操作，会造成资源浪费（CPU、电量）
// 在 onStop 和 onResume 里分别把 setJavaScriptEnabled() 给设置成 false 和 true 即可

//支持插件
webSettings.setPluginsEnabled(true); 

//设置自适应屏幕，两者合用
webSettings.setUseWideViewPort(true); //将图片调整到适合webview的大小 
webSettings.setLoadWithOverviewMode(true); // 缩放至屏幕的大小

//缩放操作
webSettings.setSupportZoom(true); //支持缩放，默认为true。是下面那个的前提。
webSettings.setBuiltInZoomControls(true); //设置内置的缩放控件。若为false，则该WebView不可缩放
webSettings.setDisplayZoomControls(false); //隐藏原生的缩放控件

//其他细节操作
webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK); //关闭webview中缓存 
webSettings.setAllowFileAccess(true); //设置可以访问文件 
webSettings.setJavaScriptCanOpenWindowsAutomatically(true); //支持通过JS打开新窗口 
webSettings.setLoadsImagesAutomatically(true); //支持自动加载图片
webSettings.setDefaultTextEncodingName("utf-8");//设置编码格式
```

### 设置WebView缓存

- 当加载 **html** 页面时，WebView会在**/data/data/**包名目录下生成 **database** 与 **cache** 两个文件夹

- 请求的 URL记录保存在 WebViewCache.db，而 URL的内容是保存在 **WebViewCache** 文件夹下

  ```java
  //优先使用缓存: 
      WebView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK); 
          //缓存模式如下：
          //LOAD_CACHE_ONLY: 不使用网络，只读取本地缓存数据
          //LOAD_DEFAULT: （默认）根据cache-control决定是否从网络上取数据。
          //LOAD_NO_CACHE: 不使用缓存，只从网络获取数据.
          //LOAD_CACHE_ELSE_NETWORK，只要本地有，无论是否过期，或者no-cache，都使用缓存中的数据。
  
      //不使用缓存: 
      WebView.getSettings().setCacheMode(WebSettings.LOAD_NO_CACHE);
  ```

- 结合使用

  ```java
  if (NetStatusUtil.isConnected(getApplicationContext())) {
      webSettings.setCacheMode(WebSettings.LOAD_DEFAULT);//根据cache-control决定是否从网络上取数据。
  } else {
      webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);//没网，则从本地获取，即离线加载
  }
  
  webSettings.setDomStorageEnabled(true); // 开启 DOM storage API 功能
  webSettings.setDatabaseEnabled(true);   //开启 database storage API 功能
  webSettings.setAppCacheEnabled(true);//开启 Application Caches 功能
  
  String cacheDirPath = getFilesDir().getAbsolutePath() + APP_CACAHE_DIRNAME;
  webSettings.setAppCachePath(cacheDirPath); //设置  Application Caches 缓存目录
  ```

**注意：** 每个 Application 只调用一次 `WebSettings.setAppCachePath()`，`WebSettings.setAppCacheMaxSize()`



## WebViewClient类

- 作用：处理各种通知&请求

### 常见方法

- `shouldOverrideUrlLoading()`

  打开网页时不调用系统浏览器，而在本WebView中显示；网页中所有的加载都经过这个方法

  ```java
  //步骤1. 定义Webview组件
  Webview webview = (WebView) findViewById(R.id.webView1);
  
  //步骤2. 选择加载方式
    //方式1. 加载一个网页：
    webView.loadUrl("http://www.google.com/");
  
    //方式2：加载apk包中的html页面
    webView.loadUrl("file:///android_asset/test.html");
  
    //方式3：加载手机本地的html页面
     webView.loadUrl("content://com.android.htmlfileprovider/sdcard/test.html");
  
  //步骤3. 复写shouldOverrideUrlLoading()方法，使得打开网页时不调用系统浏览器， 而是在本WebView中显示
      webView.setWebViewClient(new WebViewClient(){
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            view.loadUrl(url);
        return true;
        }
    });。
  ```

- `onPageStarted()`

  开始载入页面调用的，可以设定一个**loading**页面，告诉用户程序在等待网络响应

  ```java
  webView.setWebViewClient(new WebViewClient(){
       @Override
       public void  onPageStarted(WebView view, String url, Bitmap favicon) {
          //设定加载开始的操作
       }
   });
  ```

- `onPageFinished()`

  页面加载结束时调用，可以关闭loading条，切换程序动作

  ```java
  webView.setWebViewClient(new WebViewClient(){
        @Override
        public void onPageFinished(WebView view, String url) {
           //设定加载结束的操作
        }
    });
  ```

- `onLoadResouce()`

  加载页面资源时会调用，每一个资源的加载会调用一次

  ```java
  webView.setWebViewClient(new WebViewClient(){
        @Override
        public boolean onLoadResource(WebView view, String url) {
           //设定加载资源的操作
        }
    });
  ```

- `onReceivedError()`

  加载页面的服务器出现错误时调用

  > App里面使用webview控件的时候遇到了诸如404这类的错误的时候，若也显示浏览器里面的那种错误提示页面就显得很丑陋了，那么这个时候我们的app就需要加载一个本地的错误提示页面，即webview如何加载一个本地的页面

  ```java
  //步骤1：写一个html文件（error_handle.html），用于出错时展示给用户看的提示页面
  //步骤2：将该html文件放置到代码根目录的assets文件夹下
  
  //步骤3：复写WebViewClient的onRecievedError方法
  //该方法传回了错误码，根据错误类型可以进行不同的错误分类处理
      webView.setWebViewClient(new WebViewClient(){
        @Override
        public void onReceivedError(WebView view, int errorCode, String description, String failingUrl){
  switch(errorCode)
                  {
                  case HttpStatus.SC_NOT_FOUND:
                      view.loadUrl("file:///android_assets/error_handle.html");
                      break;
                  }
              }
          });
  ```

- `OnReceivedSslError()`

  处理https请求

  > webView默认是不处理https请求的，页面显示空白，需要进行设置

  ```java
  webView.setWebViewClient(new WebViewClient() {    
          @Override    
          public void onReceivedSslError(WebView view, SslErrorHandler handler, SslError error) {    
              handler.proceed();    //表示等待证书响应
          // handler.cancel();      //表示挂起连接，为默认方式
          // handler.handleMessage(null);    //可做其他处理
          }    
      });  
  
  // 特别注意：5.1以上默认禁止了https和http混用，以下方式是开启
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
  mWebView.getSettings().setMixedContentMode(WebSettings.MIXED_CONTENT_ALWAYS_ALLOW);
  }
  ```

------

##  WebChromeClient类

**作用**:辅助WebView处理Javascript的对话框，网站图标，网站标题

### 常用方法

- `onProgressChanged()`

  获取网页的加载进度并显示

  ```java
  webview.setWebChromeClient(new WebChromeClient(){
  
        @Override
        public void onProgressChanged(WebView view, int newProgress) {
            if (newProgress < 100) {
                String progress = newProgress + "%";
                progress.setText(progress);
              } else {
          }
      });
  ```

- `onReceivedTitle()`

  获取Web页中的标题

  > 每个网页页面都有一个标题，百度标题为“百度一下，你就知道”，如何知道当前webview正在加载的页面的title并进行设置

  ```java
  webview.setWebChromeClient(new WebChromeClient(){
  
      @Override
      public void onReceivedTitle(WebView view, String title) {
         titleview.setText(title)；
      }
  ```

- `onJsAlert()`

  支持**javascript**的警告框

  ```java
  webview.setWebChromeClient(new WebChromeClient() {
              
              @Override
              public boolean onJsAlert(WebView view, String url, String message, final JsResult result)  {
      new AlertDialog.Builder(MainActivity.this)
              .setTitle("JsAlert")
              .setMessage(message)
              .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialog, int which) {
                      result.confirm();
                  }
              })
              .setCancelable(false)
              .show();
      return true;
              }
  ```

- `onJsConfirm()`

  支持**javascript**确认框

  ```java
  webview.setWebChromeClient(new WebChromeClient() {
          
              @Override
  public boolean onJsConfirm(WebView view, String url, String message, final JsResult result) {
      new AlertDialog.Builder(MainActivity.this)
              .setTitle("JsConfirm")
              .setMessage(message)
              .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialog, int which) {
                      result.confirm();
                  }
              })
              .setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialog, int which) {
                      result.cancel();
                  }
              })
              .setCancelable(false)
              .show();
  // 返回布尔值：判断点击时确认还是取消
  // true表示点击了确认；false表示点击了取消；
      return true;
  }
  ```

- `onJsPrompt()`

  支持**javascript**输入框

  ```java
  webview.setWebChromeClient(new WebChromeClient() {
              @Override
              public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, final JsPromptResult result) {
      final EditText et = new EditText(MainActivity.this);
      et.setText(defaultValue);
      new AlertDialog.Builder(MainActivity.this)
              .setTitle(message)
              .setView(et)
              .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialog, int which) {
                      result.confirm(et.getText().toString());
                  }
              })
              .setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                  @Override
                  public void onClick(DialogInterface dialog, int which) {
                      result.cancel();
                  }
              })
              .setCancelable(false)
              .show();
  
      return true;
  }
  ```



```java
boolean shouldOverrideUrlLoading(WebView view, String url){}//已废弃
```

# 3. 常见需求实例

## 根据URL加载页面

```java
public class MainActivity extends AppCompatActivity {

    private WebView webView;
    private long exitTime;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        webView = new WebView(this);
        webView.setWebViewClient(new WebViewClient() {
            //设置webview点击打开新网页在当前界面显示，而不跳转到浏览器中
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                //
                if (url == null) return false;

                try{
                    if(!url.startsWith("http://") && !url.startsWith("https://")){
                        Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
                        startActivity(intent);
                        return true;
                    }
                }catch (Exception e){//防止crash (如果手机上没有安装处理某个scheme开头的url的APP, 会导致crash)
                    return true;//没有安装该app时，返回true，表示拦截自定义链接，但不跳转，避免弹出上面的错误页面
                }
                view.loadUrl(url);
                return true;
            }
        });
        webView.getSettings().setJavaScriptEnabled(true);   // 设置WebView属性，运行执行js脚本
        webView.loadUrl("https://www.baidu.com");           //调用loadUrl方法为WebView加入链接
        setContentView(webView);                            //调用Activity提供的setContentView将webView显示出来
    }

    //我们需要重写回退按钮的时间,当用户点击回退按钮：
    //1.webView.canGoBack()判断网页是否能后退,可以则goback()
    //2.如果不可以连续点击两次退出App,否则弹出提示Toast
    @Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            if ((System.currentTimeMillis() - exitTime) > 2000) {
                Toast.makeText(getApplicationContext(),"再按一次退出程序",Toast.LENGTH_SHORT).show();
                exitTime = System.currentTimeMillis();
            } else {
                super.onBackPressed();
            }
        }
    }
}
```

## 新闻门户网站页面

- 修改activity_main.xml布局文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <LinearLayout
          android:orientation="horizontal"
          android:layout_width="match_parent"
          android:layout_height="wrap_content">
          <Button
              android:id="@+id/btn_back"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="BACK"/>
          <TextView
              android:id="@+id/text_title"
              android:layout_width="0dp"
              android:layout_height="wrap_content"
              android:layout_weight="1"
              android:gravity="center"
              android:maxLines="1"/>
          <Button
              android:id="@+id/btn_refresh"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="刷新"/>
          <Button
              android:id="@+id/btn_top"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="TOP"/>
      </LinearLayout>
      <WebView
          android:id="@+id/wView"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
  
  </LinearLayout>
  ```

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      private Button btn_back;
      private TextView text_title;
      private Button btn_top;
      private Button btn_refresh;
      private WebView webView;
      private long exitTime = 0;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          bindViews();
      }
  
      private void bindViews() {
          btn_back = (Button) findViewById(R.id.btn_back);
          btn_top = (Button) findViewById(R.id.btn_top);
          btn_refresh = (Button) findViewById(R.id.btn_refresh);
          text_title = (TextView) findViewById(R.id.text_title);
          webView = (WebView) findViewById(R.id.wView);
  
          btn_back.setOnClickListener(this);
          btn_top.setOnClickListener(this);
          btn_refresh.setOnClickListener(this);
  
          webView.setWebChromeClient(new WebChromeClient(){
  
              //这里设置获取到的网站title
              @Override
              public void onReceivedTitle(WebView view, String title) {
                  super.onReceivedTitle(view, title);
                  text_title.setText(title);
              }
          });
          webView.setWebViewClient(new WebViewClient() {
              //设置webview点击打开新网页在当前界面显示，而不跳转到浏览器中
              @Override
              public boolean shouldOverrideUrlLoading(WebView view, String url) {
                  if (url == null) return false;
  
                  try{
                      if(!url.startsWith("http://") && !url.startsWith("https://")){
                          Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
                          startActivity(intent);
                          return true;
                      }
                  }catch (Exception e){//防止crash (如果手机上没有安装处理某个scheme开头的url的APP, 会导致crash)
                      return true;//没有安装该app时，返回true，表示拦截自定义链接，但不跳转，避免弹出上面的错误页面
                  }
                  view.loadUrl(url);
                  return true;
              }
          });
          webView.getSettings().setJavaScriptEnabled(true);   // 设置WebView属性，运行执行js脚本
          webView.loadUrl("https://www.baidu.com");           //调用loadUrl方法为WebView加入链接
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.btn_back:
                  finish();   //关闭当前Activity
                  break;
              case R.id.btn_refresh:
                  webView.reload();//刷新当前页面
                  break;
              case R.id.btn_top:
                  webView.setScrollY(0);//滚动回到顶部
                  break;
          }
      }
  
      //我们需要重写回退按钮的时间,当用户点击回退按钮：
      //1.webView.canGoBack()判断网页是否能后退,可以则goback()
      //2.如果不可以连续点击两次退出App,否则弹出提示Toast
      @Override
      public void onBackPressed() {
          if (webView.canGoBack()) {
              webView.goBack();
          } else {
              if ((System.currentTimeMillis() - exitTime) > 2000) {
                  Toast.makeText(getApplicationContext(),"再按一次退出程序",Toast.LENGTH_SHORT).show();
                  exitTime = System.currentTimeMillis();
              } else {
                  super.onBackPressed();
              }
          }
      }
  }
  ```

## WebView滚动事件监听

- 修改**activity_main.xml**布局文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <Button
          android:id="@+id/btn_icon"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_alignParentRight="true"
          android:layout_centerVertical="true"
          android:text="回到顶部"/>
      <com.vivo.a11085273.webviewtest.MyWebView
          android:id="@+id/wView"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
  
  </RelativeLayout>
  ```

- 新建MyWebView.java，自定义WebView

  ```java
  public class MyWebView extends WebView {
  
      private OnScrollChangedCallback mOnScrollChangedCallback;
  
      public MyWebView(Context context) {
          super(context);
      }
  
      public MyWebView(Context context, AttributeSet attrs) {
          super(context, attrs);
      }
  
      public MyWebView(Context context, AttributeSet attrs, int defStyleAttr) {
          super(context, attrs, defStyleAttr);
      }
  
      @Override
      protected void onScrollChanged(int l, int t, int oldl, int oldt) {
          super.onScrollChanged(l, t, oldl, oldt);
          if (mOnScrollChangedCallback != null) {
              mOnScrollChangedCallback.onScroll(1 - oldl, t - oldt);
          }
      }
  
      public OnScrollChangedCallback getOnScrollChangedCallback() {
          return mOnScrollChangedCallback;
      }
  
      public void setOnScrollChangedCallback(final OnScrollChangedCallback onScrollChangedCallback) {
          mOnScrollChangedCallback = onScrollChangedCallback;
      }
  
      public interface OnScrollChangedCallback {
          //这里的dx和dy代表的是x轴和y轴上的偏移量，你也可以自己把l, t, oldl, oldt四个参数暴露出来
          void onScroll(int dx, int dy);
      }
  }
  ```

- 修改MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private Button btn_icon;
      private MyWebView webView;
      private long exitTime = 0;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          btn_icon = (Button) findViewById(R.id.btn_icon);
          webView = (MyWebView) findViewById(R.id.wView);
          webView.setWebViewClient(new WebViewClient() {
              //设置webview点击打开新网页在当前界面显示，而不跳转到浏览器中
              @Override
              public boolean shouldOverrideUrlLoading(WebView view, String url) {
                  if (url == null) return false;
  
                  try{
                      if(!url.startsWith("http://") && !url.startsWith("https://")){
                          Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
                          startActivity(intent);
                          return true;
                      }
                  }catch (Exception e){//防止crash (如果手机上没有安装处理某个scheme开头的url的APP, 会导致crash)
                      return true;//没有安装该app时，返回true，表示拦截自定义链接，但不跳转，避免弹出上面的错误页面
                  }
                  view.loadUrl(url);
                  return true;
              }
          });
          webView.getSettings().setJavaScriptEnabled(true);   // 设置WebView属性，运行执行js脚本
          webView.loadUrl("https://www.baidu.com");           //调用loadUrl方法为WebView加入链接
  
          //比如这里做一个简单的判断，当页面发生滚动，显示那个Button
          webView.setOnScrollChangedCallback(new MyWebView.OnScrollChangedCallback() {
              @Override
              public void onScroll(int dx, int dy) {
                  if (dy > 0) {
                      btn_icon.setVisibility(View.VISIBLE);
                  } else {
                      btn_icon.setVisibility(View.GONE);
                  }
              }
          });
  
          btn_icon.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  webView.setScrollY(0);
                  btn_icon.setVisibility(View.GONE);
              }
          });
      }
  
      //我们需要重写回退按钮的时间,当用户点击回退按钮：
      //1.webView.canGoBack()判断网页是否能后退,可以则goback()
      //2.如果不可以连续点击两次退出App,否则弹出提示Toast
      @Override
      public void onBackPressed() {
          if (webView.canGoBack()) {
              webView.goBack();
          } else {
              if ((System.currentTimeMillis() - exitTime) > 2000) {
                  Toast.makeText(getApplicationContext(),"再按一次退出程序",Toast.LENGTH_SHORT).show();
                  exitTime = System.currentTimeMillis();
              } else {
                  super.onBackPressed();
              }
          }
      }
  }
  ```

------

# 4. WebView与JS交互方式

- 现在很多App里都内置了Web网页（Hybrid App），比如说很多电商平台，淘宝、京东、聚划算等等，其功能是由Android的WebView实现的，涉及到Android客户端与Web网页交互的实现。

![](https://upload-images.jianshu.io/upload_images/944365-29c6a46c81304f4f.png)

**对于Android调用JS代码的方法有2种：**

1. 通过`WebView`的`loadUrl（）` 
2. 通过`WebView`的`evaluateJavascript（）` 

**对于JS调用Android代码的方法有3种：**

1. 通过`WebView`的`addJavascriptInterface（）`进行对象映射
2. 通过 `WebViewClient` 的`shouldOverrideUrlLoading ()`方法回调拦截 url
3. 通过 `WebChromeClient` 的`onJsAlert()`、`onJsConfirm()`、`onJsPrompt（）`方法回调拦截JS对话框`alert()`、`confirm()`、`prompt（）` 消息

## Android通过WebView调用JS代码

**对于Android调用JS代码的方法有2种：**

1. 通过`WebView`的`loadUrl（）` 
2. 通过`WebView`的`evaluateJavascript（）` 

### 通过WebView的loadUrl()

点击Android按钮，即调用WebView JS（文本名为`javascript`）中`callJS()`

- **将需要调用的JS代码以.html格式放到src/main/assets文件夹里**

  > 1. 为了方便展示，本文是采用Andorid调用本地JS代码说明；
  > 2. 实际情况时，Android更多的是调用远程JS代码，即将加载的JS代码路径改成url即可

  ```html
  <!DOCTYPE html>
  <html>
  
     <head>
        <meta charset="utf-8">
        <title>Carson_Ho</title>
       <script>
  // Android需要调用的方法
     function callJS(){
        alert("Android调用了JS的callJS方法");
     }
  </script>
  
     </head>
  
  </html>
  ```

- 修改**activity_main.xml**布局文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <Button
          android:id="@+id/button"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="调用JS代码"/>
      <WebView
          android:id="@+id/wView"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
  
  </LinearLayout>
  ```

- 在MainActivity.java中通过WebView设置调用JS代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      WebView mWebView;
      Button button;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          mWebView =(WebView) findViewById(R.id.wView);
  
          WebSettings webSettings = mWebView.getSettings();
  
          // 设置与Js交互的权限
          webSettings.setJavaScriptEnabled(true);
          // 设置允许JS弹窗
          webSettings.setJavaScriptCanOpenWindowsAutomatically(true);
  
          // 先载入JS代码
          // 格式规定为:file:///android_asset/文件名.html
          mWebView.loadUrl("file:///android_asset/javascript.html");
  
          button = (Button) findViewById(R.id.button);
  
  
          button.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  // 通过Handler发送消息
                  mWebView.post(new Runnable() {
                      @Override
                      public void run() {
  
                          // 注意调用的JS方法名要对应上
                          // 调用javascript的callJS()方法
                          mWebView.loadUrl("javascript:callJS()");
                      }
                  });
  
              }
          });
  
          // 由于设置了弹窗检验调用结果,所以需要支持js对话框
          // webview只是载体，内容的渲染需要使用webviewChromClient类去实现
          // 通过设置WebChromeClient对象处理JavaScript的对话框
          //设置响应js 的Alert()函数
          mWebView.setWebChromeClient(new WebChromeClient() {
              @Override
              public boolean onJsAlert(WebView view, String url, String message, final JsResult result) {
                  AlertDialog.Builder b = new AlertDialog.Builder(MainActivity.this);
                  b.setTitle("Alert");
                  b.setMessage(message);
                  b.setPositiveButton(android.R.string.ok, new DialogInterface.OnClickListener() {
                      @Override
                      public void onClick(DialogInterface dialog, int which) {
                          result.confirm();
                      }
                  });
                  b.setCancelable(false);
                  b.create().show();
                  return true;
              }
  
          });
  
  
      }
  }
  
  ```

**注**：**JS代码调用一定要在 onPageFinished（） 回调之后才能调用，否则不会调用**



### 通过WebView的evaluatejavascript()

- 优点：该方法比第一种方法效率更高、使用更简洁。

> 1. 因为该方法的执行不会使页面刷新，而第一种方法（loadUrl ）的执行则会。
> 2. Android 4.4 后才可使用

```java
// 只需要将第一种方法的loadUrl()换成下面该方法即可
    mWebView.evaluateJavascript（"javascript:callJS()", new ValueCallback<String>() {
        @Override
        public void onReceiveValue(String value) {
            //此处为 js 返回的结果
        }
    });
//API level 19 以上
```

​	

## JS通过WebView调用Android代码

对于JS调用Android代码的方法有3种：

1. 通过`WebView`的`addJavascriptInterface()`进行对象映射
2. 通过 `WebViewClient` 的`shouldOverrideUrlLoading()`方法回调拦截 url
3. 通过 `WebChromeClient` 的`onJsAlert()`、`onJsConfirm()`、`onJsPrompt()`方法回调拦截JS对话框`alert()`、`confirm()`、`prompt()` 消息

### 通过WebView的addJavascriptInterface()进行对象映射

- 定义一个与JS对象映射关系的Android类：AndroidtoJs

  ```java
  public class AndroidtoJs extends Object {
  
      // 定义JS需要调用的方法
      // 被JS调用的方法必须加入@JavascriptInterface注解
      @JavascriptInterface
      public void hello(String msg) {
          System.out.println("JS调用了Android的hello方法");
      }
  }
  ```

- **将需要调用的JS代码以.html格式放到src/main/assets文件夹里**

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>Carson</title>
      <script>
  
  
           function callAndroid(){
          // 由于对象映射，所以调用test对象等于调用Android映射的对象
              test.hello("js");
           }
        </script>
  </head>
  <body>
  <!--//点击按钮则调用callAndroid函数-->
  <button type="button" id="button1" value="点击调用Android代码" onclick="callAndroid()"></button>
  </body>
  </html>
  ```

- **在Android里通过WebView设置Android类与JS代码的映射**

  ```java
  public class MainActivity extends AppCompatActivity {
  
      WebView mWebView;
  //    Button button;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          mWebView = (WebView) findViewById(R.id.wView);
          WebSettings webSettings = mWebView.getSettings();
  
          // 设置与Js交互的权限
          webSettings.setJavaScriptEnabled(true);
  
          // 通过addJavascriptInterface()将Java对象映射到JS对象
          //参数1：Javascript对象名
          //参数2：Java对象名
          mWebView.addJavascriptInterface(new AndroidtoJs(), "test");//AndroidtoJS类对象映射到js的test对象
  
          // 加载JS代码
          // 格式规定为:file:///android_asset/文件名.html
          mWebView.loadUrl("file:///android_asset/javascript.html");
      }
  }
  ```

优点：使用简单

缺点：存在严重漏洞，[参考文献](https://www.jianshu.com/p/3a345d27cd42)

------

### 通过WebView的shouldOverrideUrlLoading()方法拦截url

- **具体原理**：

1. Android通过 `WebViewClient` 的回调方法`shouldOverrideUrlLoading ()`拦截 url
2. 解析该 url 的协议
3. 如果检测到是预先约定好的协议，就调用相应方法



- 在JS约定所需要的Url协议：

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>Carson</title>
      <script>
  
  
           function callAndroid(){
          /*约定的url协议为：js://webview?arg1=111&arg2=222*/
              document.location = "js://webview?arg1=111&arg2=222";
           }
        </script>
  </head>
  <body>
  <!--//点击按钮则调用callAndroid函数-->
  <button type="button" id="button1" onclick="callAndroid()">点击调用Android代码</button>
  </body>
  </html>
  ```

  当该JS通过Android的`mWebView.loadUrl("file:///android_asset/javascript.html")`加载后，就会回调`shouldOverrideUrlLoading （）`

- 在Android通过WebViewClient复写`shouldOverrideUrlLoading （）`

  ```java
  public class MainActivity extends AppCompatActivity {
  
      WebView mWebView;
  //    Button button;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          mWebView = (WebView) findViewById(R.id.wView);
          WebSettings webSettings = mWebView.getSettings();
  
          // 设置与Js交互的权限
          webSettings.setJavaScriptEnabled(true);
          // 设置允许JS弹窗
          webSettings.setJavaScriptCanOpenWindowsAutomatically(true);
  
          // 步骤1：加载JS代码
          // 格式规定为:file:///android_asset/文件名.html
          mWebView.loadUrl("file:///android_asset/javascript.html");
  
  
  // 复写WebViewClient类的shouldOverrideUrlLoading方法
          mWebView.setWebViewClient(new WebViewClient() {
              @Override
              public boolean shouldOverrideUrlLoading(WebView view, String url) {
  
                // 步骤2：根据协议的参数，判断是否是所需要的url
                // 一般根据scheme（协议格式） & authority（协议名）判断（前两个参数）
                //假定传入进来的 url = "js://webview?arg1=111&arg2=222"（同时也是约定好的需要拦截的）
  
                Uri uri = Uri.parse(url);
                // 如果url的协议 = 预先约定的 js 协议
                // 就解析往下解析参数
                  if ( uri.getScheme().equals("js")) {
  
                    // 如果 authority  = 预先约定协议里的 webview，即代表都符合约定的协议
                    // 所以拦截url,下面JS开始调用Android需要的方法
                    if (uri.getAuthority().equals("webview")) {
  
                        //  步骤3：
                        // 执行JS所需要调用的逻辑
                        System.out.println("js调用了Android的方法");
                        // 可以在协议上带有参数并传递到Android上
                        HashMap<String, String> params = new HashMap<>();
                        Set<String> collection = uri.getQueryParameterNames();
  
                    }
  
                    return true;
                  }
                  return super.shouldOverrideUrlLoading(view, url);
              }
          });
      }
  ｝
  ```
  - 优点：不存在方式1的漏洞；
  - 缺点：JS获取Android方法的返回值复杂。

- 如果JS想要得到Android方法的返回值，只能通过 WebView 的 `loadUrl （）`去执行 JS 方法把返回值传递回去，相关的代码如下：

  ```java
  // Android：MainActivity.java
  mWebView.loadUrl("javascript:returnResult(" + result + ")");
  
  // JS：javascript.html
  function returnResult(result){
      alert("result is" + result);
  }
  ```

------

### 通过WebChromeClient的onJsAlert()、onJsConfirm()、onJsPrompt()、方法回调拦截JS对话框alert()、confirm()、prompt()消息

在JS中、有三个常用的对话框方法：

![](https://upload-images.jianshu.io/upload_images/944365-1385f748618af886.png)

**原理**：

Android通过 `WebChromeClient` 的`onJsAlert()`、`onJsConfirm()`、`onJsPrompt（）`方法回调分别拦截JS对话框
（即上述三个方法），得到他们的消息内容，然后解析即可。

示例：使用拦截JS输入框说明：

> 1. 常用的拦截是：拦截 JS的输入框（即`prompt（）`方法）
> 2. 因为只有`prompt（）`可以返回任意类型的值，操作最全面方便、更加灵活；而alert（）对话框没有返回值；confirm（）对话框只能返回两种状态（确定 / 取消）两个值

- 加载JS代码：

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>Carson_Ho</title>
      <script>
  
  
           function clickprompt(){
              //调用prompt()
              var result=prompt("js://demo?arg1=111&arg2=222");
              alert("demo " + result);
           }
        </script>
  </head>
  <body>
  <!--//点击按钮则调用callAndroid函数-->
  <button type="button" id="button1" onclick="clickprompt()">点击调用Android代码</button>
  </body>
  </html>
  ```

  当使用`mWebView.loadUrl("file:///android_asset/javascript.html")`加载了上述JS代码后，就会触发回调`onJsPrompt（）`

  > 1. 如果是拦截警告框（即`alert()`），则触发回调`onJsAlert（）`；
  > 2. 如果是拦截确认框（即`confirm()`），则触发回调`onJsConfirm（）`；

- 在AndroidWebChromeClient复写`onJsPrompt()`（**<font color = red>程序正确性待验证</font>**）

  ```java
  public class MainActivity extends AppCompatActivity {
  
      WebView mWebView;
  //    Button button;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
  
          mWebView = (WebView) findViewById(R.id.wView);
  
          WebSettings webSettings = mWebView.getSettings();
  
          // 设置与Js交互的权限
          webSettings.setJavaScriptEnabled(true);
          // 设置允许JS弹窗
          webSettings.setJavaScriptCanOpenWindowsAutomatically(true);
  
          // 先加载JS代码
          // 格式规定为:file:///android_asset/文件名.html
          mWebView.loadUrl("file:///android_asset/javascript.html");
  
  
          mWebView.setWebChromeClient(new WebChromeClient() {
              // 拦截输入框(原理同方式2)
              // 参数message:代表promt（）的内容（不是url）
              // 参数result:代表输入框的返回值
              @Override
              public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {
                  // 根据协议的参数，判断是否是所需要的url(原理同方式2)
                  // 一般根据scheme（协议格式） & authority（协议名）判断（前两个参数）
                  //假定传入进来的 url = "js://webview?arg1=111&arg2=222"（同时也是约定好的需要拦截的）
  
                  Uri uri = Uri.parse(message);
                  // 如果url的协议 = 预先约定的 js 协议
                  // 就解析往下解析参数
                  if ( uri.getScheme().equals("js")) {
  
                      // 如果 authority  = 预先约定协议里的 webview，即代表都符合约定的协议
                      // 所以拦截url,下面JS开始调用Android需要的方法
                      if (uri.getAuthority().equals("webview")) {
  
                          //
                          // 执行JS所需要调用的逻辑
                          System.out.println("js调用了Android的方法");
                          // 可以在协议上带有参数并传递到Android上
                          HashMap<String, String> params = new HashMap<>();
                          Set<String> collection = uri.getQueryParameterNames();
  
                          //参数result:代表消息框的返回值(输入值)
                          result.confirm("js调用了Android的方法成功啦");
                      }
                      return true;
                  }
                  return super.onJsPrompt(view, url, message, defaultValue, result);
              }
  
  // 通过alert()和confirm()拦截的原理相同，此处不作过多讲述
  
              // 拦截JS的警告框
              @Override
              public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
                  return super.onJsAlert(view, url, message, result);
              }
  
              // 拦截JS的确认框
              @Override
              public boolean onJsConfirm(WebView view, String url, String message, JsResult result) {
                  return super.onJsConfirm(view, url, message, result);
              }
          });
      }
  }
  ```



![](https://upload-images.jianshu.io/upload_images/944365-613b57c93dff2eb8.png)



# 5. 注意事项

## 多线程

如果你在子线程中调用WebView的相关方法，而不在UI线程，则可能会出现无法预料的错误。 所以，当你的程序中需要用到多线程时候，也请使用`runOnUiThread()`方法来保证你关于 WebView的操作是在UI线程中进行的

```java
runOnUiThread(newRunnable(){
@Override
publicvoid run(){
   // Code for WebView goes here
   }
});
```

## 线程阻塞

永远不要阻塞UI线程，这是开发Android程序的一个真理。虽然是真理，我们却往往不自觉的 犯一些错误违背它，**一个开发中常犯的错误就是：在UI线程中去等待JavaScript 的回调**。

```java
// This code is BAD and will block the UI thread
webView.loadUrl("javascript:fn()"); 
while(result ==null) {  
    Thread.sleep(100); 
}
```

`evaluateJavascript()` 就是专门来异步执行JavaScript代码

## evaluateJavascript() 方法

专门用于异步调用JavaScript方法，并且能够得到一个回调结果。

```java
mWebView.evaluateJavascript(script, new ValueCallback<String>() {
 @Override
 public void onReceiveValue(String value) {
      //TODO
 }
});
```

## 处理WebView中url的跳转

> 新版WebView对于自定义scheme的url跳转，新增了更为严格的限制条件。 当你实现了 shouldOverrideUrlLoading() 或 shouldInterceptRequest() 回调，WebView 也只会在跳转url是合法Url时才会跳转。 例如，如果你使用这样一个url ：

```html
<a href="showProfile">Show Profile</a>
```

shouldOverrideUrlLoading() 将不会被调用。

正确的使用方式是：

```html
<a href="example-app:showProfile">Show Profile</a>
```

对应的检测Url跳转的方式：

```java
// The URL scheme should be non-hierarchical (no trailing slashes)
 privatestaticfinalString APP_SCHEME ="example-app:";
 @Override 
 publicboolean shouldOverrideUrlLoading(WebView view,String url){
     if(url.startsWith(APP_SCHEME)){
         urlData =URLDecoder.decode(url.substring(APP_SCHEME.length()),"UTF-8");
         respondToData(urlData);
         returntrue;
     }
     returnfalse;
}
```

当然，也可以这样使用：

```java
webView.loadDataWithBaseURL("example-app://example.co.uk/", HTML_DATA,null,"UTF-8",null);
```

## UserAgent

> 如果你的App对应的服务端程序，会根据客户端传来的UserAgent来做不同的事情，那么你需要注意 的是，新版本的WebView中，UserAgent有了些微妙的改变：

```kotlin
Mozilla/5.0 (Linux; Android 4.4; Nexus 4 Build/KRT16H)
AppleWebKit/537.36(KHTML, like Gecko) Version/4.0 Chrome/30.0.0.0
Mobile Safari/537.36
```

使用**getDefaultUserAgent()**方法可以获取默认的UserAgent，也可以通过：

```java
mWebView.getSettings().setUserAgentString(ua);
mWebView.getSettings().getUserAgentString();
```

来设置和获取自定义的UserAgent。

## 使用addJavascriptInterface()的注意事项

> 从Android4.2开始。 只有添加 @JavascriptInterface 声明的Java方法才可以被JavaScript调用

```java
class JsObject {
    @JavascriptInterface
    public String toString() { return "injectedObject"; }
}

webView.addJavascriptInterface(new JsObject(), "injectedObject");
webView.loadData("", "text/html", null);
webView.loadUrl("javascript:alert(injectedObject.toString())");
```

## Remote Debugging

使用Chrome来调试你运行在WebView中的程序 