# 1. LayoutInflate初始

参考文献：

参考文献一：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-layoutinflater.html)

参考文献二：[guolin博客](https://blog.csdn.net/guolin_blog/article/details/12921889)

------

**Layout是什么**？

> 答：一个用于加载布局的系统服务，就是实例化与Layout XML文件对应的View对象，不能直接使用， 需要通过**getLayoutInflater**( )方法或**getSystemService**( )方法来获得与当前Context绑定的 LayoutInflater实例!

## LayoutInflate简单用法

1. **获取LayoutInflater实例的三种方法**：

```java
LayoutInflater inflater1 = LayoutInflater.from(this);  
LayoutInflater inflater2 = getLayoutInflater();  
LayoutInflater inflater3 = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE); 
```

> 后面两个底层与第一种方法一样

2. **加载布局的方法**：

`public View inflate (int resource, ViewGroup root, boolean attachToRoot)` 该方法的三个参数依次为：

- **resource**：要加载的布局对应的资源ID
- **root**：为该布局的外部在嵌套一层父布局，如不需要，填写null即可
- **attachToRoot**：是否为加载的布局文件的最外层嵌套一层root布局。**root不为null，默认为true**，如果为false，则root失去作用，如果root为null，attachToRoot无作用。

3. 通过**LayoutInflate.LayoutParams**设置相关属性



## 纯Java代码加载布局

> 特定情况下，需要使用Java代码王布局中动态添加组件或者布局

**纯Java代码加载布局流程**：

1. **创建容器**：

   ```java
   LinearLayout ly = new LinearILayout(this);
   ```

2. **创建组件**：

   ```java
   Button btnOne = new Button(this);
   ```

3. **为容器或组件设置属性**：

   如**LinearLayout**，可以设置组件排列方向：`ly.setOrientation(LinearLayout.VERTICAL)`，等等

4. **将组件或容器添加到容器中**：

   这里可能需要设置组件的添加位置或者大小，需要用到一个类：**LayoutParams**，将其看作是布局容器的一个信息包，封装位置与大小等信息的一个类，例如设置大小：

   ```java
   LinearLayout.LayoutParams lp1 = new LinearLayout.LayoutParams( 
           LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT); 
   ```

   设置位置的话，通常考虑只是**RelativeLayout**，用到`addRule()`方法，可添加多个。

   ```java
   RelativeLayout rly = new RelativeLayout(this);  
   RelativeLayout.LayoutParams lp2 = new RelativeLayout.LayoutParams(  
                   LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);  
   lp2.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);  
   Button btnOne = new Button(this);  
   rly.addView(btnOne, lp2);
   ```

   **参照其他组件对齐**：（参考组件手动设置id）

   ```java
   public class MainActivity extends Activity {  
       @Override  
       protected void onCreate(Bundle savedInstanceState) {  
           super.onCreate(savedInstanceState);  
           RelativeLayout rly = new RelativeLayout(this);  
           Button btnOne = new Button(this);  
           btnOne.setText("按钮1");  
           Button btnTwo = new Button(this);  
           btnTwo.setText("按钮2");  
           // 为按钮1设置一个id值  
           btnOne.setId(123);  
           // 设置按钮1的位置,在父容器中居中  
           RelativeLayout.LayoutParams rlp1 = new RelativeLayout.LayoutParams(  
                   LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);  
           rlp1.addRule(RelativeLayout.CENTER_IN_PARENT);  
           // 设置按钮2的位置,在按钮1的下方,并且对齐父容器右面  
           RelativeLayout.LayoutParams rlp2 = new RelativeLayout.LayoutParams(  
                   LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);  
           rlp2.addRule(RelativeLayout.BELOW, 123);  
           rlp2.addRule(RelativeLayout.ALIGN_PARENT_RIGHT);  
           // 将组件添加到外部容器中  
           rly.addView(btnTwo, rlp2);  
           rly.addView(btnOne, rlp1);  
           // 设置当前视图加载的View即rly  
           setContentView(rly);  
       }  
   }  
   ```

   **注**：前述的`setId()`方法会报错

   **方案一**：通过调用`View.generateViewId()`作为setId的参数，但此方案不是最佳方案，因为View.generateViewId()方法必须为SDK版本17及以上才行，否则报错。（但也有可以通过自写一个Utils.generateViewId()解决，不过既然有方案二更好的方法，就不过多赘述此方法了）

   ```java
   `my_view.setId(View.generateViewId());`
   ```

   **方案二**：在res/values/下添加id.xml(名字可随意)文件，代码如下：

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <resources>
       <item name="my_view" type="id" />
   </resources>
   ```

    　　然后在代码中做如下设置即可：

   ```java
   my_view.setId(R.id.my_view);
   ```

5. 调用`setContentView()`方法加载布局对象，移除组件可以调用`容器.removeView(要移除的组件)`

## Java动态添加控件或xml布局

实际中更多的时候是动态地添加View控件以及动态地加载XML布局！

### 动态增加View

动态添加组件的写法有两种，**区别在于是否需要先setContentView(R.layout.activity_main)**

假设布局文件为：

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:id="@+id/RelativeLayout1"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent" >  
  
    <TextView   
        android:id="@+id/txtTitle"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="我是xml文件加载的布局"/>  
      
</RelativeLayout>  
```

- 第一种写法：最后`setContentView()`加载布局

  ```java
  public class MainActivity extends Activity {  
      @Override  
      protected void onCreate(Bundle savedInstanceState) {  
          super.onCreate(savedInstanceState);  
          Button btnOne = new Button(this);  
          btnOne.setText("我是动态添加的按钮");  
          RelativeLayout.LayoutParams lp2 = new RelativeLayout.LayoutParams(    
                  LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);    
          lp2.addRule(RelativeLayout.CENTER_IN_PARENT);    
          LayoutInflater inflater = LayoutInflater.from(this);  
          RelativeLayout rly = (RelativeLayout) inflater.inflate(  
                  R.layout.activity_main, null)  
                  .findViewById(R.id.RelativeLayout1);  
          rly.addView(btnOne,lp2);  
          setContentView(rly);  
      }  
  } 
  ```

- 第二种写法：先`setContentView()`加载布局

  ```java
  public class MainActivity extends Activity {  
      @Override  
      protected void onCreate(Bundle savedInstanceState) {  
          super.onCreate(savedInstanceState);  
          setContentView(R.layout.activity_main);  
          Button btnOne = new Button(this);  
          btnOne.setText("我是动态添加的按钮");  
          RelativeLayout.LayoutParams lp2 = new RelativeLayout.LayoutParams(    
                  LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);    
          lp2.addRule(RelativeLayout.CENTER_IN_PARENT);    
          RelativeLayout rly = (RelativeLayout) findViewById(R.id.RelativeLayout1);  
          rly.addView(btnOne,lp2);  
      }  
  } 
  ```

代码分析：

- 创建按钮组件，又创建了一个LayoutParams对象，设置Button大小，通过`addRule()`方法设置Button位置
- **第一种方法**:通过LayoutInflate的`inflate()`方法加载了activity_main布局，获得了外层容器， 接着addView添加按钮进容器,最后setContentView();
- **第二种方法**：因为我们已经通过setContetView()方法加载了布局，此时我们就可以通过 findViewById找到这个外层容器,接着addView,最后setContentView()即可!

### 动态加载xml布局

- 先写下主布局文件和动态加载的布局文件

  **activity_main.xml**

  ```xml
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:id="@+id/RelativeLayout1"
      android:layout_width="match_parent"
      android:layout_height="match_parent" >
      <Button
          android:id="@+id/btnLoad"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="动态加载布局"/>
  </RelativeLayout>  
  ```

  **inflate.xml**

  ```xml
  <?xml version="1.0" encoding="utf-8"?>  
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
      android:layout_width="match_parent"  
      android:layout_height="match_parent"  
      android:gravity="center"  
      android:orientation="vertical"  
      android:id="@+id/ly_inflate" >  
    
      <TextView  
          android:layout_width="wrap_content"  
          android:layout_height="wrap_content"  
          android:text="我是Java代码加载的布局" />  
    
      <Button  
          android:layout_width="wrap_content"  
          android:layout_height="wrap_content"  
          android:text="我是布局里的一个小按钮" />  
    
  </LinearLayout> 
  ```

- 在MainActivity动态加载xml布局：

  ```java
  public class MainActivity extends Activity {  
      @Override  
      protected void onCreate(Bundle savedInstanceState) {  
          super.onCreate(savedInstanceState);  
          setContentView(R.layout.activity_main);  
          //获得LayoutInflater对象;  
          final LayoutInflater inflater = LayoutInflater.from(this);    
          //获得外部容器对象  
          final RelativeLayout rly = (RelativeLayout) findViewById(R.id.RelativeLayout1);  
          Button btnLoad = (Button) findViewById(R.id.btnLoad);  
          btnLoad.setOnClickListener(new OnClickListener() {  
              @Override  
              public void onClick(View v) {  
                  //加载要添加的布局对象  
                  LinearLayout ly = (LinearLayout) inflater.inflate(  
                          R.layout.inflate, null, false).findViewById(  
                          R.id.ly_inflate);  
                  //设置加载布局的大小与位置  
                  RelativeLayout.LayoutParams lp = new RelativeLayout.LayoutParams(    
                          LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);    
                  lp.addRule(RelativeLayout.CENTER_IN_PARENT);    
                  rly.addView(ly,lp);  
              }  
          });  
      }  
  }
  ```

**①获取容器对象**:

```java
final RelativeLayout rly = (RelativeLayout) findViewById(R.id.RelativeLayout1);
```

**②获得Inflater对象,同时加载被添加的布局的xml,通过findViewById找到最外层的根节点**

```java
final LayoutInflater inflater = LayoutInflater.from(this);
LinearLayout ly = (LinearLayout) inflater.inflate(R.layout.inflate, null, false)
   .findViewById(R.id.ly_inflate);
```

**③为这个容器设置大小与位置信息:**

```java
RelativeLayout.LayoutParams lp = new RelativeLayout.LayoutParams(  
               LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT);  
       lp.addRule(RelativeLayout.CENTER_IN_PARENT); 
```

**④添加到外层容器中:**

```java
rly.addView(ly,lp);
```



