# 1. HttpURLConnection使用

参考文献：

- 参考文献一：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-httpurlconnection.html)
- 参考文献二：第一行代码
- 参考文献三：[API](https://developer.android.com/reference/java/net/HttpURLConnection)

------

**HttPURLConnection使用步骤**：

- 首先，获取**HttpURLConnection**实例，需new一个**URL**对象，传入目标网络地址，然后调用一下`openConnection()`方法即可

  ```java
  URL url = new URL("http://www.baidu.com);
  HttpURLConnection connection = (HttpURLConnection) url.openConnection();
  ```

- 设置**HTTP**请求使用方法：一帮有两种：**GET**和**POST**。前者表示希望从服务器哪里获取数据，后者表示希望提交数据给服务器

  ```java
  connection.setRequestMethod("GET");
  ```

- **自由定制**，比如设置连接超时，读取超时的毫秒数，以及服务器希望得到的一些消息头等。

  ```java
  connection.setConnectionTimeout(8000);
  connection.setReadTimeout(8000);
  ```

- 调用`getInputStream()`方法获取到服务器返回的输入流，对输入流进行读取

  ```java
  InputStream in = connection.getInputStream();
  ```

- 最后，调用`disconnection()`方法将这个HTTP连接关闭掉

  ```java
  connection.disconnection();
  ```



**示例**：

- 新建NetworkTest项目，修改**activity_main.xml**文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <Button
          android:id="@+id/send_request"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="Send Request"/>
      <ScrollView
          android:layout_width="match_parent"
          android:layout_height="match_parent">
          <TextView
              android:id="@+id/response_text"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content" />
      </ScrollView>
  
  </LinearLayout>
  ```

- 修改MainActivity.java

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      TextView responseText;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button sendRequest = (Button) findViewById(R.id.send_request);
          responseText = (TextView) findViewById(R.id.response_text);
          sendRequest.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          if (v.getId() == R.id.send_request) {
              Log.e("MainActivity","first");
              sendRequestWithHttpURLConnection();
          }
      }
  
      private void sendRequestWithHttpURLConnection() {
          //开启线程发起网络请求
          new Thread(new Runnable() {
              @Override
              public void run() {
                  HttpURLConnection connection = null;
                  BufferedReader reader = null;
                  try {
                      URL url = new URL("https://www.baidu.com");
                      connection = (HttpURLConnection) url.openConnection();
                      connection.setRequestMethod("GET");
                      connection.setConnectTimeout(8000);
                      connection.setReadTimeout(8000);
                      int i = connection.getResponseCode();
                      InputStream in = connection.getInputStream();
                      //对输入流进行读取
                      reader = new BufferedReader(new InputStreamReader(in));
                      StringBuilder response = new StringBuilder();
                      String line;
                      while ((line = reader.readLine()) != null) {
                          response.append(line);
                      }
                      showResponse(response.toString());
                  } catch (Exception e) {
                      e.printStackTrace();
                  } finally {
                      if (reader != null) {
                          try {
                              reader.close();
                          } catch (IOException e) {
                              e.printStackTrace();
                          }
                      }
                      if (connection != null) {
                          connection.disconnect();
                      }
                  }
              }
          }).start();
      }
  
      private void showResponse( final String response) {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  //进行UI操作
                  responseText.setText(response);
              }
          });
      }
  }
  ```

- 在AndroidManifest.xml注册权限

  ```xml
  <uses-permission android:name="android.permission.INTERNET"/>
  ```

（Android 9.0不能明文连接，需用**https**）

------



## 使用HttpURLConnection下载图片

- 修改**activity_main.xml**文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <Button
          android:id="@+id/send_request"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="Send Request"/>
      <Button
          android:id="@+id/download_picture"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="Download_picture"/>
      <ImageView
          android:id="@+id/imagPic"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content" />
      <ScrollView
          android:layout_width="match_parent"
          android:layout_height="match_parent">
          <TextView
              android:id="@+id/response_text"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content" />
      </ScrollView>
  
  </LinearLayout>
  ```

- 新建**StreamTool.java**文件，读取流中文件

  ```java
  public class StreamTool {
      //从流中读取数据
      public static byte[] read(InputStream inStream) throws Exception{
          ByteArrayOutputStream outStream = new ByteArrayOutputStream();
          byte[] buffer = new byte[1024];
          int len = 0;
          while((len = inStream.read(buffer)) != -1)
          {
              outStream.write(buffer,0,len);
          }
          inStream.close();
          return outStream.toByteArray();
      }
  }
  ```

- 新建GetData.java文件,获取数据类

  ```java
  public class GetData {
      // 定义一个获取网络图片数据的方法:
      public static byte[] getImage(String path) throws Exception {
          URL url = new URL(path);
          HttpURLConnection conn = (HttpURLConnection) url.openConnection();
          // 设置连接超时为5秒
          conn.setConnectTimeout(5000);
          // 设置请求类型为Get类型
          conn.setRequestMethod("GET");
          // 判断请求Url是否成功
          if (conn.getResponseCode() != 200) {
              throw new RuntimeException("请求url失败");
          }
          InputStream inStream = conn.getInputStream();
          byte[] bt = StreamTool.read(inStream);
          inStream.close();
          return bt;
      }
  }
  ```

- 修改**MainActivity.java**

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      private TextView responseText;
      private String picture = "https://i.ytimg.com/vi/ck-2nnFC8oE/hqdefault.jpg";
      private Bitmap bitmap;
      private ImageView imgPic;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button sendRequest = (Button) findViewById(R.id.send_request);
          responseText = (TextView) findViewById(R.id.response_text);
          imgPic = (ImageView) findViewById(R.id.imagPic);
          Button downloadPicture = (Button) findViewById(R.id.download_picture);
          downloadPicture.setOnClickListener(this);
          sendRequest.setOnClickListener(this);
      }
  
      @Override
      public void onClick(View v) {
          if (v.getId() == R.id.send_request) {
              sendRequestWithHttpURLConnection();
          } else if (v.getId() == R.id.download_picture) {
              new Thread(new Runnable() {
                  @Override
                  public void run() {
                      try {
                          byte[] data = GetData.getImage(picture);
                          bitmap = BitmapFactory.decodeByteArray(data, 0, data.length);
                          showBitmap();
                      } catch (Exception e) {
                          e.printStackTrace();
                      }
                  }
              }).start();
  
          }
      }
  
      private void sendRequestWithHttpURLConnection() {
          //开启线程发起网络请求
          new Thread(new Runnable() {
              @Override
              public void run() {
                  HttpURLConnection connection = null;
                  BufferedReader reader = null;
                  try {
                      URL url = new URL("https://www.baidu.com");
                      connection = (HttpURLConnection) url.openConnection();
                      connection.setRequestMethod("GET");
                      connection.setConnectTimeout(8000);
                      connection.setReadTimeout(8000);
                      int i = connection.getResponseCode();
                      InputStream in = connection.getInputStream();
                      //对输入流进行读取
                      reader = new BufferedReader(new InputStreamReader(in));
                      StringBuilder response = new StringBuilder();
                      String line;
                      while ((line = reader.readLine()) != null) {
                          response.append(line);
                      }
                      showResponse(response.toString());
                  } catch (Exception e) {
                      e.printStackTrace();
                  } finally {
                      if (reader != null) {
                          try {
                              reader.close();
                          } catch (IOException e) {
                              e.printStackTrace();
                          }
                      }
                      if (connection != null) {
                          connection.disconnect();
                      }
                  }
              }
          }).start();
      }
  
      private void showResponse( final String response) {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  //进行UI操作
                  responseText.setText(response);
              }
          });
      }
  
      private void showBitmap() {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  imgPic.setImageBitmap(bitmap);
              }
          });
      }
  }
  ```



# 2. OkHttp使用

使用开源项目OkHttp，需要在项目中添加OkHttp库依赖。

**file——》project structure——》Dependecies——》+   ——》okhttp**



- 使用OkHttp，首先创建一个OkHttpClient的实例：

  ```java
  OkHttpClient client = new OkHttpClient();
  ```

- 若需发起Http请求，需要创建一个Request对象：

  ```java
  Request request = new Request.Builder().build();
  ```

- 前述表达创建是空Request对象，可以在最终build()方法之前连缀其他方法丰富这个Request对象。

  ```java
  Request request = new Request.Builder()
  .url("https://www.baidu.com")
  .build();
  ```

- 之后调用OkHttpClient的`newCall()`方法来创建一个Call对象，并调用它的`execute()`方法来发送请求并获取服务器返回的数据：

  ```java
  Response response = client.newCall(request).execute();
  ```

  其中Response对象即服务器返回的数据

  ```java
  String responseData = response.body().string();
  ```

  - 若发起的是POST请求，，需要先构建一个RequestBody对象存放待提交的参数：

    ```java
    RequestBody requestBody = new FormBody.Builder()
    .add("username", "admin")
    .add("password", "123456")
    .build();
    ```

  - 在Request.Builder中调用一下post()方法，将RequestBody对象传入：

    ```java
    Request request = new Request.Builder()
    .url("https://www.baidu.com")
    .post(requestBody)
    .build();
    ```

  - 之后操作与GET请求一致



**示例**：将**HttpURLConnection**改为**OkHttp**实现

- 简单修改**MainActivity.java**代码

  ```java
  @Override
      public void onClick(View v) {
          if (v.getId() == R.id.send_request) {
  //            sendRequestWithHttpURLConnection();
              sendRequestWithOkHttp();
              }
              }
  public void sendRequestWithOkHttp() {
          new Thread(new Runnable() {
              @Override
              public void run() {
                  try {
                      OkHttpClient client = new OkHttpClient();
                      Request request = new Request.Builder()
                              .url("https://www.baidu.com")
                              .build();
                      Response response = client.newCall(request).execute();
                      String responseData = response.body().string();
                      showResponse(responseData);
                  } catch (Exception e) {
                      e.printStackTrace();
                  }
              }
          }).start();
      }
  ```

