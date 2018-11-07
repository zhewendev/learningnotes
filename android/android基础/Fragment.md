# 1 基本概述

- **Fragment**是一种可以嵌入在活动中的UI片段，能够让程序更加合理和充分地利用大屏幕的空间，出现的初衷是为了适应大屏幕的平板电脑，可以将其看成一个小型Activity，又称作**Activity片段**。
- 使用Fragment可以把屏幕划分成几块，然后进行分组，进行一个模块化管理。Fragment不能够单独使用，需要嵌套在Activity中使用，其生命周期也受到宿主Activity的生命周期的影响。

一个Fragment分别对应手机与平板间不同情况的处理图：

![](http://www.runoob.com/wp-content/uploads/2015/08/41442282.jpg)

## Fragment生命周期

Fragment生命周期会经历：运行、暂停、停止、销毁。

- **运行状态**：碎片可见时，关联活动处于运行状态，其也为运行状态
- **暂停状态**：活动进入暂停状态，相关联可见碎片就会进入暂停状态
- **停止状态**：活动进入停止状态，相关联碎片就会进入停止状态，或者通过FragmentTransaction的`remove()`、`replace()`方法将碎片从从活动中移除，但如果在事务提交之前调用`addToBackStack()`方法，这时的碎片也会进入到停止状态。
- **销毁状态**：当活动被销毁，相关联碎片进入销毁状态。或者调用FragmentTransaction的`remove()`、`replace()`方法将碎片从活动中移除，但在事务提交之前并没有调用`addToBackStack()`方法，碎片也会进入到销毁状态。
- **相关重点回调方法**：
  - `onAttach()`：碎片和活动建立关联时候调用
  - `onCreateView()`:为碎片创建视图（加载布局）时调用
  - onActivityCreated()：确保与碎片相关联的活动一定已经创建完毕的时候调用
  - onDestroyView()：当与碎片关联的视图被移除的时候调用
  - onDetach()：当碎片和活动接触关联的时候调用

![](http://www.runoob.com/wp-content/uploads/2015/08/31722863.jpg)

### Fragment几个子类

- 对话框:**DialogFragment**
- 列表:**ListFragment**
- 选项设置:**PreferenceFragment**
- WebView界面:**WebViewFragment**



**注**：开发Fragment不建议使用android.app下的Fragment而应是android:support.v4.app

## 创建一个Fragment

### 静态加载Fragment

其实现流程如图所示：

![](http://www.runoob.com/wp-content/uploads/2015/08/14443487.jpg)

示例：

- 定义Fragment的布局，即fragment显示内容：新建left_fragment.xml和right_fragment.xml文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <Button
          android:id="@+id/button"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_gravity="center_horizontal"
          android:text="Button" />
  </LinearLayout>
  ```

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:background="#00ff00"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_gravity="center_horizontal"
          android:textSize="20sp"
          android:text="this is Fragment" />
  
  </LinearLayout>
  ```

- 自定义Fragment类，继承Fragment或者其子类，重写`onCreateView()`方法，在方法中调用`inflater.inflate()`方法加载Fragment布局文件，接着返回加载的view对象

  ```java
  public class LeftFragment extends Fragment {
  
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container,
                               Bundle savedInstanceState) {
          View view = inflater.inflate(R.layout.left_fragment, container,false);
          return view;
      }
  
  }
  ```

  ```java
  public class RigthFragment extends Fragment {
  
      @Nullable
      @Override
      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
          View view = inflater.inflate(R.layout.right_fragment, container, false);
          return view;
      }
  }
  ```

- 在需要加载Fragment的Activity对应的布局文件中添加fragment标签，name属性是全限定类名，包含包名

  ```xml
  <fragment
      android:id="@+id/left_fragment"
      android:name="com.vivo.a11085273.secondfragmenttest.LeftFragment"
      android:layout_width="0dp"
      android:layout_height="match_parent"
      android:layout_weight="1" />
  <fragment
      android:id="@+id/right_fragment"
      android:name="com.vivo.a11085273.secondfragmenttest.RigthFragment"
      android:layout_width="0dp"
      android:layout_height="match_parent"
      android:layout_weight="1"
      />
  ```

- Activity在`onCreate()`方法中调用`setContentView()`加载布局文件即可

### 动态加载Fragment

实现流程：

![](http://www.runoob.com/wp-content/uploads/2015/08/29546434.jpg)

上图中第一步方法改为`getSupportFragmentManager()`：

- 新建another_right_fragment.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:background="#ffff00"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:textSize="20sp"
          android:text="This is the another right fragment"/>
  
  </LinearLayout>
  ```

- 新建AnotherRightFragment

  ```java
  public class AnotherRightFragment extends Fragment {
  
      @Nullable
      @Override
      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
          View view = inflater.inflate(R.layout.another_right_fragment, container, false);
          return view;
      }
  }
  ```

- 修改activity_main.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="horizontal"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <fragment
          android:id="@+id/left_fragment"
          android:name="com.vivo.a11085273.secondfragmenttest.LeftFragment"
          android:layout_width="0dp"
          android:layout_height="match_parent"
          android:layout_weight="1" />
      <FrameLayout
          android:id="@+id/right_layout"
          android:layout_width="0dp"
          android:layout_height="match_parent"
          android:layout_weight="1"
          />
  </LinearLayout>
  ```

- 在代码中向FrameLayout里添加内容，修改MainActivity中代码

  ```java
  public class MainActivity extends AppCompatActivity implements View.OnClickListener {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button button = (Button) findViewById(R.id.button);
          button.setOnClickListener(this);
          replaceFragment(new RigthFragment());
      }
  
      @Override
      public void onClick(View v) {
          switch (v.getId()) {
              case R.id.button:
                  replaceFragment(new AnotherRightFragment());
                  break;
              default:
                  break;
          }
      }
  
      private void replaceFragment(Fragment fragment) {
          FragmentManager fragmentManager = getSupportFragmentManager();
          FragmentTransaction transaction = fragmentManager.beginTransaction();
          transaction.replace(R.id.right_layout, fragment);
          transaction.commit();
      }
  }
  ```

## Fragment管理与Fragment事务

![](http://www.runoob.com/wp-content/uploads/2015/08/97188171.jpg)

想要模拟类似返回栈的效果，back键返回上一个碎片，则可以在**FragmentTransaction**中添加`addToBackStack()`方法：

```java
private void replaceFragment(Fragment fragment) {
    FragmentManager fragmentManager = getSupportFragmentManager();
    FragmentTransaction transaction = fragmentManager.beginTransaction();
    transaction.replace(R.id.right_layout, fragment);
    transaction.addToBackStack(null);
    transaction.commit();
}
```



## Fragment与Activity的交互

![](http://www.runoob.com/wp-content/uploads/2015/08/45971686.jpg)

- **组件获取**：

  - Fragment获得Activity中的组件: **getActivity().findViewById(R.id.list)；**
  - Activity获得Fragment中的组件(根据id和tag都可以)：**getSupportFragmentManager.findFragmentById(R.id.right_fragment);**

- **数据传递**：

  - **Activity传递数据给Fragment**：在Activity中创建**Bundle数据包**,调用Fragment实例的**setArguments(bundle)** 从而将Bundle数据包传给Fragment,然后Fragment中调用getArguments获得 Bundle对象,然后进行解析就可以了

    ```java
    //创建Fragment对象，并通过Bundle对象传递值（在onCreate方法中）
    MyFragment fragment = new MyFragment();
    Bundle bundle = new Bundle();
    bundle.putString("key", values);
    fragment.setArguments(bundle);
    ```

    ```java
    //（在Fragment类中的onCreateView方法中）
    Bundle bundle = this.getArguments();
        if (bundle != null)
        {
            String str = bundle.getString("key");
        }
        TextView textView = new TextView(getActivity());
        textView.setText("上上下下的享受");//是电梯，别误会
    ```

  - **Fragment传递数据给Activity**：在Fragment中定义一个内部回调接口,再让包含该Fragment的Activity实现该回调接口, Fragment就可以通过回调接口传数据了。

    - 首先在Fragment中定义一个回调接口：（接口中定义抽象方法，要传什么类型数据参数设为何种类型）

      ```java
      /*接口*/  
      public interface Mylistener{
              public void thanks(String code);
          } 
      ```

    - 定义该接口（在Fragment类中）

      ```java
      private Mylistener listener;
      ```

    - 在onAttach方法中，将定义的该接口强转为activity类型便可与Activity有交际

      ```java
      @Override
          public void onAttach(Activity activity) {
              // TODO Auto-generated method stub
              listener=(Mylistener) activity;
              super.onAttach(activity);
          }
      ```

    - 通过使用Listener接口里面的方法，在Activity中可以获得Fragment传入的code（在onCreateView方法中）

      ```java
       listener.thanks(code);
      ```

    - 在Acvitity中只需要实现该接口并重写里面的方法即可。

      ```java
      @Override
          public void thanks(String code) {
              // TODO Auto-generated method stub
              Toast.makeText(this, "已收到Fragment的消息：--"+code+"--,客气了", Toast.LENGTH_SHORT).show();;
          }
      ```

  - **Fragment与Fragment之间数据互传**：

    [Fragment间参数传递](https://blog.csdn.net/harvic880925/article/details/44966913)

# 2 动态加载布局技巧

程序可以根据设备的分辨率或屏幕大小在运行时来决定加载哪一个布局。



## 使用限定符

平板应用采用双页模式，手机是单页显示。如何在运行时判断程序是使用双叶模式还是单页模式？可以借助限定符（**Qualifiers**）来实现。

- 将前述的activity_main.xml文件改为：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/left_fragment"
        android:name="com.vivo.a11085273.secondfragmenttest.LeftFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

- 在res下新建layout-large文件夹，在这个文件夹下新建一个布局，也叫做activity_main.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="horizontal"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <fragment
          android:id="@+id/left_fragment"
          android:name="com.vivo.a11085273.secondfragmenttest.LeftFragment"
          android:layout_height="match_parent"
          android:layout_width="0dp"
          android:layout_weight="1" />
      <fragment
          android:id="@+id/right_fragment"
          android:name="com.vivo.a11085273.secondfragmenttest.RigthFragment"
          android:layout_width="0dp"
          android:layout_height="match_parent"
          android:layout_weight="3"/>
  
  </LinearLayout>
  ```

  large是一个限定符，屏幕被认为是large的设备就会自动加载layout-large文件夹下的布局，而小屏幕的设备还是会加载layout文件夹下布局。

- 将MainActivity中`replaceFragment()`方法里的代码注释掉





Android中常见的限定符：

![](E:\document\photo\first-code\限定符表.jpg)

![](E:\document\photo\first-code\限定符表2.jpg)



## 使用最小宽度限定符

如果希望可以更加灵活地为不同设备加载布局，不管它们是不是被系统认定为large，就可以使用**最小宽度限定符**（Smallest-width-Qualifier）

**最小宽度限定符允许对屏幕宽度指定一个最小值**（dp为单位），然后以这个最小值为临界点，屏幕大于这个值的设备就加载一个布局，屏幕宽度小鱼这个值的设备就加载另一个布局。

在res目录下新建layout-sw600dp文件夹，然后再这个文件夹下新建activity_main.xml布局，**则当程序运行在屏幕宽度大于600dp设备上时，会加载该布局，当程序运行在屏幕宽度小于600dp的设备上时，仍然加载默认的layout/activity_main布局。**