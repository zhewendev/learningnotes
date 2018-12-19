# 参考资料：

1. [姚智胜博客](https://blog.csdn.net/a243981326/article/details/73556892)
2. [小鄧子简书博客](https://www.jianshu.com/p/7567ed0d1853)
3. [蠢萌的技术宅简书博客](https://www.jianshu.com/p/4b754ea48a40)
4. [JesseWuuu博客](http://www.jcodecraeer.com/a/anzhuokaifa/2017/1020/8625.html)



# 1. MVP架构概念

## 1.1 什么是MVP

MVP代表Model，View和Presenter

- **View**层：

  负责处理用户事件和视图部分的展示，即**显示数据的地方**。得到数据后，数据传递到view层，显示数据。同时view层的点击事件等处理也在这里出现，真正的数据处理在model层中处理。在Android中，它可能是Activity或者Fragment类。

- **Model**层：

  负责访问数据，即单纯的处理数据，不接手其他任务。数据可以是远端的Server API，本地数据库或者SharedPreference等。

- **Presenter**层：

  是连接（或适配）View和Model的桥梁。M层获得数据后交给P层，P层再交给View层。同理，View层的点击事件等处理通过P层去通知M层，让其进行数据处理。

示例：

​	下图是基于MVP架构的模式之一。View是UI线程。Presenter是View与Model之间的适配器。UseCase或者Domain在Model层中，负责从实体获取或载入数据。依赖规则如下：

![](https://upload-images.jianshu.io/upload_images/268450-901fc7b43f05763e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800/format/webp)

关键是，高层接口不知道底层接口的细节，或者更准确地说，**高层接口不能，不应该，并且必须不了解底层接口的细节，是（面向）抽象的，并且是细节隐藏的**。

![](https://upload-images.jianshu.io/upload_images/268450-dd987ab16a97e4cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800/format/webp)

> 同心圆将软件划分为不同的区域，一般的，随着层级的深入，软件的等级也就越高。外圆是实现机制，内圆是核心策略。

**Entities：**

- 可以是一个持有方法函数的对象
- 可以是一组数据结构或方法函数
- 它并不重要，能在项目中被不同应用程序使用即可

**Use Cases**

- 包含特定于应用程序的业务规则
- 精心编排流入Entity或从Entity流出的数据
- 指挥Entity直接使用项目范围内的业务规则，从而实现Use Case的目标

**Presenters,Controllers**

- 将Use Case和Entity中的数据转换成格式最方便的数据
- 外部系统，如数据库或网页能够方便的使用这些数据
- 完全包含GUI的MVC架构

**External Interfaces, UI, DB**

- 所有的细节所在
- 如数据库细节，Web框架细节，等等

## 1.2 MVP关系再介绍

![](https://upload-images.jianshu.io/upload_images/1858247-57db1c3f3fa9380d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/822/format/webp)

MVP分离了view和model层，Presenter层充当了桥梁的角色，View层只负责更新界面即可，这里的View我们要明白只是一个viewinterface，它是视图的接口，这样我们在做单元测试的时候可以非常方便编写Presenter层代码。

MVP 模式将Activity 中的业务逻辑全部分离出来，让Activity 只做 UI 逻辑的处理，所有跟Android API无关的业务逻辑由 Presenter 层来完成。

![](https://upload-images.jianshu.io/upload_images/1858247-2d77d0d965a54aa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/941/format/webp)



# 2. MVP架构设计思路

mvp架构的中心思想是面向接口编程，调用层使用接口对象，去调用接口方法，实现层去实现接口方法，并在调用层实例化。

![](https://img-blog.csdn.net/20170621214243411?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTI0Mzk4MTMyNg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

片段示例：

- 假设要做一个加载图片的抽象方法：

  ```java
  public interface ImageLoaderInterface<T extends View> extends Serializable {
  
      void displayImage(Context context, String path, T imageView);
  
      void displayImage(Context context, @DrawableRes Integer resId, T imageView);
  
      T createImageView(Context context);
  }
  ```

- 我们使用的时候就是直接去使用它的方法，比如我们要在adapter中使用他， 那么我们不需要在adapter中去实例化他，而是直接使用这个接口对象，在使用的地方调用接口的方法

  ```java
   public ImageLoaderInterface imageLoaderInterface;
  
      public void showPic(ViewHolder holder) {
  
              imageLoaderInterface.displayImage(context, addPicRes, holder.iv_pic);
  
          } 
  ```

- 然后在其他类中去写实现类，在adapter的初始化中把这个实现类去传递给他

  ```java
  public abstract class ImageLoader implements ImageLoaderInterface<ImageView> {
  
      @Override
      public ImageView createImageView(Context context) {
          return new ImageView(context);
      }
  
  }
  
  public class Loader extends ImageLoader {
  
      @Override
      public void displayImage(Context context, String path, ImageView imageView) {
          Glide.with(context).load(path).into(imageView);
  
      }
  
      @Override
      public void displayImage(Context context, @DrawableRes Integer resId, ImageView imageView) {
          imageView.setImageResource(resId);
      }
  
  }
  
  
  adapter中写set方法，让外部调用
  
  adapter.setImageLoaderInterface(new Loader());
  ```

mvp架构中，我们要把m层的接口方法抽象出来，p层的接口方法抽象出来，v层的接口方法抽象出来，同时分别写3个接口的实现类，（**v层一般是activity或者fragment继承view层的接口**），**<font color = red>v层持有p的接口对象，p层持有v层和m层的接口对象，m层为p层的提供数据</font>**，这时也就形成了mvp架构，三者之间通过p层相互连接



# 3. MVP架构基本设计，使用

## 3.1 MVP使用案例一

**功能实现**：通过在Activity中的一个按钮的点击事件，获取数据

- 首先定义出MVP的所有接口

  ```java
  public interface TestContract {
  
      interface View {
          //显示数据
          void showData(String str);
  
      }
  
      interface Presenter  {
           //通知model要获取数据并把model返回的数据交给view层
          void getData();
  
      }
  
      interface Model {
           //获取数据
         String doData();
  
      }
  }
  ```

- 实现P接口

  ```java
  public class TestPresenter implements TestContract.Presenter {
  
      private TestContract.Model model;
      private TestContract.View view;
  
      public TestPresenter (TestContract.Model model, TestContract.View view) {
          this.model = model;
          this.view = view;
      }
  
  
      @Override
      public void getData() {
        view.showData(model.doData());    
      }
  
  
  }	
  ```

- 实现M接口

  ```java
  public class TestModel implements TestContract.Model {
      private TestModel model;
  
      public static TestModel getInstance() {
          if (model== null) {
              model= new TestModel ();
          }
          return model;
      }
  
      @Override
      public  String doData() {
  
          return "mvp架构";
      }
  
  
  }
  ```

- 让Activity继承V接口，初始化P，让P持有M和V

  ```java
  public class TestActivity extends Activity implements TestContract.View,View.OnClickListener {
  private TestContract.Presenter  presenter;
  
   presenter = new TestPresenter (TestModel.getInstance(), this);
  
  ...
      @Override
      public void onClick(View view) {
          switch (view.getId()) {
              case R.id.tv_test: // 回到顶部按钮
                  presenter.getData();
                  break;
                 }
      }
  
      @Override
      protected void showData(String str) {
          textVeiw.setText(str)
      }
  }
  ```



## 3.2 MVP使用案例二

下面是项目的架构，一个Activity，一个Fragment，Data层主要负责获取App已安装的应用列表，AppListPresenter负责业务逻辑处理

![](https://upload-images.jianshu.io/upload_images/1858247-880d79faaa1e27b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/480/format/webp)

- 查看一下presenter，Viewinterface的结构

![](https://upload-images.jianshu.io/upload_images/1858247-6db3d94abc02b9dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/526/format/webp)

![](https://upload-images.jianshu.io/upload_images/1858247-eef3002b1318feac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/411/format/webp)

- AppListFragment代码

  ```java
  public class AppListFragment extends Fragment implements AppViewInterface {
  
          private Presenter presenter;
  
          private List<PackageInfo> packageInfoList = new ArrayList<>();
          private RecyclerView recyclerView;
          private MyAppListRecyclerViewAdapter myAppListRecyclerViewAdapter;
  
          @Override
          public void showAppList(List<PackageInfo> packageInfos) {
              if (packageInfos.isEmpty())
                  return;
              packageInfoList.clear();
              packageInfoList.addAll(packageInfos);
              myAppListRecyclerViewAdapter.notifyDataSetChanged();
          }
  
          @Override
          public void setPresenter(Presenter presenter) {
              this.presenter = presenter;
          }
      }
  ```

  AppListFragment实现了AppViewInterface接口，需要在Activity中把AppListPresenter和AppViewInterface双向绑定

- AppListPresenter关键代码

  ```java
  public class AppListPresenter implements Presenter, LoaderManager.LoaderCallbacks<List<PackageInfo>>{
  
          private AppViewInterface viewInterface;
          private AppClassLoader appClassLoader;
          private LoaderManager loaderManager;
  
          private final int id = 0;
          public AppListPresenter(AppViewInterface viewInterface, AppClassLoader appClassLoader, 
                          LoaderManager loaderManager) {
              this.viewInterface = viewInterface;
              this.appClassLoader = appClassLoader;
              this.loaderManager = loaderManager;
              viewInterface.setPresenter(this);
          }
  
          @Override
          public void loadInstallApps() {
              //通过loadmanager提供的方法获取安装的应用列表
              loaderManager.initLoader(id, null, this);
          }
  
          @Override
          public void destory() {
              loaderManager.destroyLoader(id);
          }
  
          @Override
          public void onLoadFinished(Loader<List<PackageInfo>> loader, List<PackageInfo> data) {
              //获取到已安装的应用列表，调用AppViewInterface的showAppList方法
              viewInterface.showAppList(data);
          }
  
          @Override
          public void launchApp(PackageInfo packageInfo) {
              Intent intent = appClassLoader.queryLaunchIntent(packageInfo);
              if (intent != null)
                  appClassLoader.getContext().startActivity(intent);
              else
                  Toast.makeText(appClassLoader.getContext(), "Can not start the app", Toast.LENGTH_SHORT).show();
          }
      }
  ```

  关键方法是loadInstallApps，这个方法在MainActivity的onCreate中调用

  ```java
  private Presenter appListPresenter;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
  
          setContentView(R.layout.activity_main);
  
          Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
          setSupportActionBar(toolbar);
  
          FragmentTransaction fragmentTransaction = getSupportFragmentManager().beginTransaction();
          AppListFragment appListFragment = AppListFragment.newInstance();
          fragmentTransaction.add(R.id.fm, appListFragment);
          fragmentTransaction.commit();
  
          appListPresenter = new AppListPresenter(appListFragment, new AppClassLoader(getApplicationContext()), 
                                              getSupportLoaderManager());
          //调用loadInstallApps
          appListPresenter.loadInstallApps();
      }
  ```

  首先，我们获取一个AppListFragment的实例，在AppListPresenter构造函数里面我们传入AppViewInterface，同时在AppPresenter的构造函数中又将presenter注入到了AppViewInerface里面，这样就实现了Presenter和ViewInerface双向绑定，之后调用AppPresenter的loadInstallApps方法，在onLoadFinished回调里面又调用了AppViewInterface的showApps方法，这样数据就显示在界面。

## 3.3 MVP使用案例三（<font color=red>重要</font>）

**核心问题**：

- 关于如何在Activity中高效的复用Presenter和View；
- Mode层定义到什么程度才算是比较理想的解耦；
- Model层与Presenter层如何比较优雅的相互通信。



根据前述的讲解，MVP架构主要分为三层：

- Activity 和Fragment 视为View层，负责处理 UI。
- Presenter 为业务处理层，既能调用UI逻辑，又能请求数据，该层为纯Java类，不涉及任何Android API。
- Model 层中包含着具体的数据请求，数据源

三层之间的调用顺序为view->presenter->model，**不可反向调用！不可跨级调用！**

model层如何反馈给Presenter层？Presenter如何操控View层？如下图所示

![](http://www.jcodecraeer.com/uploads/userup/13953/1G020140036-F40-0.png)

底层不会直接给上一层做反馈，而是通过View，Callback为上级做出了反馈，这样就解决了请求数据与更新界面的异步操作。图中View和Callback都是以接口的形式存在。

View中定义了Activity的具体操作，主要将请求到的数据在界面中更新之类

Callback中定义了请求数据是反馈的各种状态：成功，失败，异常等

### 3.3.1 简易模拟请求网络

- 目录结构

![1544536071198](C:\Users\11085273\AppData\Roaming\Typora\typora-user-images\1544536071198.png)

- Callback接口

  该接口是Model层给Presenter层反馈请求信息的传递载体，在CallBack中定义数据请求的各种反馈状态：

  ```java
  public interface MvpCallback {
     /**
       * 数据请求成功
       * @param data 请求到的数据
       */
      void onSuccess(String data);
      /**
       *  使用网络API接口请求方式时，虽然已经请求成功但是由
       *  于{@code msg}的原因无法正常返回数据。
       */
      void onFailure(String msg);
       /**
       * 请求数据失败，指在请求网络API接口请求方式时，出现无法联网、
       * 缺少权限，内存泄露等原因导致无法连接到请求数据源。
       */
      void onError();
      /**
       * 当请求数据结束时，无论请求结果是成功，失败或是抛出异常都会执行此方法给用户做处理，通常做网络
       * 请求时可以在此处隐藏“正在加载”的等待控件
       */
      void onComplete();
  }
  ```

- Model类

  在Model类中定义了具体的网络请求操作。可以通过postDelayed方法模拟耗时操作，通过判断请求参数反馈不同的请求状态：

  ```java
  public class MvpModel {
      /**
       * 获取网络接口数据
       * @param param 请求参数
       * @param callback 数据回调接口
       */
      public static void getNetData(final String param, final MvpCallback callback){
          // 利用postDelayed方法模拟网络请求数据的耗时操作
          new Handler().postDelayed(new Runnable() {
              @Override
              public void run() {
                  switch (param){
                      case "normal":
                          callback.onSuccess("根据参数"+param+"的请求网络数据成功");
                          break;
                      case "failure":
                          callback.onFailure("请求失败：参数有误");
                          break;
                      case "error":
                          callback.onError();
                          break;
                  }
                  callback.onComplete();
              }
          },2000);
      }
  }
  ```

- View接口

  View接口是Activity与Presenter层的中间层，它的作用是根据具体业务需求，为Presenter提供调用Activity中具体UI逻辑操作的方法

  ```java
  public interface MvpView {
      /**
       * 显示正在加载进度框
       */
      void showLoading();
      /**
       * 隐藏正在加载进度框
       */
      void hideLoading();
      /**
       * 当数据请求成功后，调用此接口显示数据
       * @param data 数据源
       */
      void showData(String data);
      /**
       * 当数据请求失败后，调用此接口提示
       * @param msg 失败原因
       */
      void showFailureMessage(String msg);
      /**
       * 当数据请求异常，调用此接口提示
       */
      void showErrorMessage();
  }
  ```

- Presenter类

  Presenter类是具体的逻辑业务处理类，该类为纯java类，不包含任何Android API，负责请求数据，并对数据请求的反馈进行处理。

  Presenter类的构造方法中有一个View接口参数，为了通过View结构通知Activiy进行更新界面等操作

  ```java
  public class MvpPresenter {
      // View接口
      private MvpView mView;
      public MvpPresenter(MvpView view){
          this.mView = view;
      }
      /**
       * 获取网络数据
       * @param params 参数
       */
      public void getData(String params){
          //显示正在加载进度条
          mView.showLoading();
          // 调用Model请求数据
          MvpModel.getNetData(params, new MvpCallback() {
              @Override
              public void onSuccess(String data) {
                  //调用view接口显示数据
                  mView.showData(data);
              }
              @Override
              public void onFailure(String msg) {
                  //调用view接口提示失败信息
                  mView.showFailureMessage(msg);
              }
              @Override
              public void onError() {
                  //调用view接口提示请求异常
                  mView.showErrorMessage();
              }
              @Override
              public void onComplete() {
                  // 隐藏正在加载进度条
                  mView.hideLoading();
              }
          });
      }
  }
  ```

- xml布局文件

  ```java
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:padding="16dp"
      android:orientation="vertical"
      tools:context="com.jessewu.mvpdemo.MainActivity">
      <TextView
          android:id="@+id/text"
          android:layout_width="match_parent"
          android:layout_height="0dp"
          android:layout_weight="1"
          android:text="点击按钮获取网络数据"/>
      <Button
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="获取数据【成功】"
          android:onClick="getData"
          />
      <Button
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="获取数据【失败】"
          android:onClick="getDataForFailure"
          />
      <Button
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="获取数据【异常】"
          android:onClick="getDataForError"
          />
  </LinearLayout>
  ```

- Activity

  在Activity代码中需要强调的是如果想要调用Presenter就要先实现Presenter需要的对应的View接口。

  ```java
  public class MainActivity extends AppCompatActivity implements MvpView  {
      //进度条
      ProgressDialog progressDialog;
      TextView text;
      MvpPresenter presenter;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          text = (TextView)findViewById(R.id.text);
          // 初始化进度条
          progressDialog = new ProgressDialog(this);
          progressDialog.setCancelable(false);
          progressDialog.setMessage("正在加载数据");
          //初始化Presenter
          presenter = new MvpPresenter(this);
      }
      // button 点击事件调用方法
      public void getData(View view){
          presenter.getData("normal");
      }
      // button 点击事件调用方法
      public void getDataForFailure(View view){
          presenter.getData("failure");
      }
      // button 点击事件调用方法
      public void getDataForError(View view){
          presenter.getData("error");
      }
      @Override
      public void showLoading() {
          if (!progressDialog.isShowing()) {
              progressDialog.show();
          }
      }
      @Override
      public void hideLoading() {
          if (progressDialog.isShowing()) {
              progressDialog.dismiss();
          }
      }
      @Override
      public void showData(String data) {
          text.setText(data);
      }
      @Override
      public void showFailureMessage(String msg) {
          Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
          text.setText(msg);
      }
      @Override
      public void showErrorMessage() {
          Toast.makeText(this, "网络请求数据出现异常", Toast.LENGTH_SHORT).show();
          text.setText("网络请求数据出现异常");
      }
  }
  ```

  （这里与案例一有点不同的地方主要是model类的静态方法导致）

### 3.3.2 MVP代码复用

前述的Demo只实现了一个Activiy请求操作，而每个Activity并不是需要与它对应的一套MVP。

不需要数据请求的Activiy不需要MVP辅助，与其他Activity中存在相同逻辑的Activity，不需要重复生成对应的MVP。

![](https://ask.qcloudimg.com/http-save/yehe-1277822/pkoqm9j9s9.jpeg?imageView2/2/w/1620)

- 场景1：业务逻辑完全相同

​	逻辑相同的，Activiy C可以直接使用Activiy A 写好的MVP无需再做处理

- 场景2,3：包含部分相同逻辑

  场景2、3逻辑类似，都属于一个业务逻辑中包含另一个可以单独存在的业务逻辑，这种情况可以采用继承方法：

  ![](http://www.jcodecraeer.com/uploads/userup/13953/1G020140036-54A-6.png)

- 场景4：

  Activity C 想要同时调用独立于服务与ActiviyA和ActivityB的业务逻辑，需将两个业务逻辑对应的Presenter分别实例化并调用业务方法：

  ```java
  private PresenterA presenterA;
  private PresenterB presenterB;
  ...
  ...
  private void getData(){
      presenterA.getData();
      presenterB.getData();
  }
  ```

  同时实现两个Presenter对应的View

  ```java
  public class ActivityC extends Activity implements ViewA,ViewB{
    ...
  }
  ```

- 场景5：

  场景5是场景3与4的结合体，需要将A和B的业务逻辑拆分开，然后同时调用

  。。。。



**注意事项**：

- MVP结构过于繁重，避免重复代码，开发前设计好逻辑调用图

### 3.3.3 MVP架构优化——base层顶级父类

前述MVP架构存在的问题：

- 构架存在漏洞
- 代码冗余量大
- 通用性差



**核心问题点**：调用View可能引发空指针异常

前述案例中应用请求网络数据时需要等待后台反馈数据后更新界面，但是在请求过程中当前**Activity突然因为某种原因被销毁**，Presenter收到后台反馈并调用View接口处理UI逻辑时由于Activity已经被销毁，就会引发空指针异常。

要避免这种情况的发生就需要**每次调用View前都知道宿主Activity的生命状态**。

前述案例是在Presenter的构造方法中得到View接口的引用，现在需要修改Presenter引用View接口的方式，让View接口与宿主Activity共存亡。

```java
public class MvpPresenter {
    // View接口
    private MvpView mView;
    public MvpPresenter(){
        //构造方法中不再需要View参数
    }
  　/**
     * 绑定view，一般在初始化中调用该方法
     */
    public void attachView(MvpView  mvpView) {
        this.mView= mvpView;
    }
    /**
     * 断开view，一般在onDestroy中调用
     */
    public void detachView() {
        this.mView= null;
    }
    /**
     * 是否与View建立连接
     * 每次调用业务请求的时候都要出先调用方法检查是否与View建立连接
     */
    public boolean isViewAttached(){
        return mView!= null;
    }
    /**
     * 获取网络数据
     * @param params 参数
     */
    public void getData(String params){
        //显示正在加载进度条
        mView.showLoading();
        // 调用Model请求数据
        MvpModel.getNetData(params, new MvpCallback() {
            @Override
            public void onSuccess(String data) {
                //调用view接口显示数据
                if(isViewAttached()){
                    mView.showData(data);
                }
            }
            @Override
            public void onFailure(String msg) {
                //调用view接口提示失败信息
                if(isViewAttached()){
                     mView.showFailureMessage(msg);
                }          
            }
            @Override
            public void onError() {
                //调用view接口提示请求异常
                if(isViewAttached()){
                    mView.showErrorMessage();
                } 
            }
            @Override
            public void onComplete() {
                // 隐藏正在加载进度条
                if(isViewAttached()){
                    mView.hideLoading();
                } 
            }
        });
    }
}
```

上述的Presenter较前述代码增加了三个方法：

- `attachView()`：绑定View引用
- `detachView()`：断开View引用
- `isViewAttached()`：判断View引用是否存在

其中前两个方法是为Activity准备`，isViewAttached()`作用是Presenter内部每次调用View接口中的方法判断View的引用是否存在。

将绑定View的方法写到Activity的生命周期中：

```java
public class MainActivity extends Activity implements MvpView{
  MvpPresenter presenter;
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //初始化Presenter
        presenter = new MvpPresenter();
        // 绑定View引用
        presenter.attachView(this);
    }
    @Override
    protected void onDestroy() {
        super.onDestroy();
        // 断开View引用
        presenter.detachView();
    }
}
```

------

**<font color = red>结合Activity构建base层</font>**

MVP模式代码量巨大，冗余代码多，可以为MVP中所有单元都设计一个顶级父类减少重复的冗余代码。同理为Activity设计一个父类方便与MVP架构更完美的结合，最后将所有父类单独分到一个base包中供外界继承调用。

**Callback**

建议模拟请求网络中Callback接口中`onSuccess()`方法需要根据请求的数据类型的不同设置为不同类型的参数，每当有新的数据类型都需要新建一个Callback，可以通过引泛型的概念，用调用者去定义具体想要接收的数据类型。

```java
public interface Callback<T> {
   /**
     * 数据请求成功
     * @param data 请求到的数据
     */
    void onSuccess(T data);
    /**
     *  使用网络API接口请求方式时，虽然已经请求成功但是由
     *  于{@code msg}的原因无法正常返回数据。
     */
    void onFailure(String msg);
     /**
     * 请求数据失败，指在请求网络API接口请求方式时，出现无法联网、
     * 缺少权限，内存泄露等原因导致无法连接到请求数据源。
     */
    void onError();
    /**
     * 当请求数据结束时，无论请求结果是成功，失败或是抛出异常都会执行此方法给用户做处理，通常做网络
     * 请求时可以在此处隐藏“正在加载”的等待控件
     */
    void onComplete();
}
```

**BaseView**

View接口定义了Activity的UI逻辑。将几乎在每个Activity中都会用到的方法变成通用的：

```java
public interface BaseView {
     /**
     * 显示正在加载view
     */
    void showLoading();
    /**
     * 关闭正在加载view
     */
    void hideLoading();
    /**
     * 显示提示
     * @param msg
     */
    void showToast(String msg);
    /**
     * 显示请求错误提示
     */
    void showErr();
    /**
     * 获取上下文
     * @return 上下文
     */
    Context getContext();
}
```

**BasePresenter**

Presenter中可共用的代码就是对View引用的方法，前述已定义了BaseView，希望Presenter中持有的View都是BaseView的子类，需要泛型约束：

```java
public class BasePresenter<V extends IBaseView> {
    /**
     * 绑定的view
     */
    private V mvpView;
    /**
     * 绑定view，一般在初始化中调用该方法
     */
    @Override
    public void attachView(V mvpView) {
        this.mvpView = mvpView;
    }
    /**
     * 断开view，一般在onDestroy中调用
     */
    @Override
    public void detachView() {
        this.mvpView = null;
    }
    /**
     * 是否与View建立连接
     * 每次调用业务请求的时候都要出先调用方法检查是否与View建立连接
     */
    public boolean isViewAttached(){
        return mvpView != null;
    }
    /**
     * 获取连接的view
     */
    public V getView(){
        return mvpView;
    }
```

**BaseActivity**

BaseActivity主要是负责实现BaseView中通用的UI逻辑方法

```java
public abstract class BaseActivity extends Activity implements IBaseView {
    private ProgressDialog mProgressDialog;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mProgressDialog = new ProgressDialog(this);
        mProgressDialog.setCancelable(false);
    }
    @Override
    public void showLoading() {
        if (!mProgressDialog.isShowing()) {
            mProgressDialog.show();
        }
    }
    @Override
    public void hideLoading() {
        if (mProgressDialog.isShowing()) {
            mProgressDialog.dismiss();
        }
    }
    @Override
    public void showToast(String msg) {
        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }
    @Override
    public void showErr() {
        showToast(getResources().getString(R.string.api_error_msg));
    }
    @Override
    public Context getContext() {
        return BaseActivity.this;
    }
```

封装完base层，再实现一下之前MVP的应用

**Model**

```java
public class MvpModel {
    /**
     * 获取网络接口数据
     * @param param 请求参数
     * @param callback 数据回调接口
     */
    public static void getNetData(final String param, final MvpCallback<String> callback){
        // 利用postDelayed方法模拟网络请求数据的耗时操作
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                switch (param){
                    case "normal":
                        callback.onSuccess("根据参数"+param+"的请求网络数据成功");
                        break;
                    case "failure":
                        callback.onFailure("请求失败：参数有误");
                        break;
                    case "error":
                        callback.onError();
                        break;
                }
                callback.onComplete();
            }
        },2000);
    }
}
```

**View接口**

```java
public interface MvpView extends BaseView{
    /**
     * 当数据请求成功后，调用此接口显示数据
     * @param data 数据源
     */
    void showData(String data);
}
```

**Presenter类**

```java
public class MvpPresenter extends BasePresenter<MvpView > {
    /**
     * 获取网络数据
     * @param params 参数
     */
    public void getData(String params){
        if (!isViewAttached()){
            //如果没有View引用就不加载数据
            return;
        }
        //显示正在加载进度条
        getView().showLoading();
        // 调用Model请求数据
        MvpModel.getNetData(params, new MvpCallback()<String> {
            @Override
            public void onSuccess(String data) {
                //调用view接口显示数据
                if(isViewAttached()){
                    getView().showData(data);
                }
            }
            @Override
            public void onFailure(String msg) {
                //调用view接口提示失败信息
                if(isViewAttached()){
                     getView().showToast(msg);
                }          
            }
            @Override
            public void onError() {
                //调用view接口提示请求异常
                if(isViewAttached()){
                    getView().showErr();
                } 
            }
            @Override
            public void onComplete() {
                // 隐藏正在加载进度条
                if(isViewAttached()){
                    getView().hideLoading();
                } 
            }
        });
    }
}
```

**Activity**

```java
public class MainActivity extends BaseActivity implements MvpView  {
    TextView text;
    MvpPresenter presenter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        text = (TextView)findViewById(R.id.text);
        //初始化Presenter
        presenter = new MvpPresenter();
        presenter.attachView(this);
    }
    @Override
    protected void onDestroy() {
        super.onDestroy();
        //断开View引用
        presenter.detachView();
    }
    @Override
    public void showData(String data) {
        text.setText(data);
    }
    // button 点击事件调用方法
    public void getData(View view){
        presenter.getData("normal");
    }
    // button 点击事件调用方法
    public void getDataForFailure(View view){
        presenter.getData("failure");
    }
    // button 点击事件调用方法
    public void getDataForError(View view){
        presenter.getData("error");
    }  
}
```

------

**Fragment**

日常开发中不是所有的UI处理都在Activity中进行，Fragment也是重要的一员，实现BaseFragment做法与BaseActivity类似，注意Fragment与宿主Activity的链接情况。

```java
public abstract class BaseFragment extends Fragment implements BaseView {
    public abstract int getContentViewId();
    protected abstract void initAllMembersView(Bundle savedInstanceState);
    protected Context mContext;
    protected View mRootView;
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        mRootView = inflater.inflate(getContentViewId(), container, false);
        this.mContext = getActivity();
        initAllMembersView(savedInstanceState);
        return mRootView;
    }
    @Override
    public void showLoading() {
        checkActivityAttached();
        ((BaseFragmentActivity) mContext).showLoading();
    }
    @Override
    public void showLoading(String msg) {
        checkActivityAttached();
        ((BaseFragmentActivity) mContext).showLoading(msg);
    }
    @Override
    public void hideLoading() {
        checkActivityAttached();
        ((BaseFragmentActivity) mContext).hideLoading();
    }
    @Override
    public void showToast(String msg) {
        checkActivityAttached();
        ((BaseFragmentActivity) mContext).showToast(msg);
    }
    @Override
    public void showErr() {
        checkActivityAttached();
        ((BaseFragmentActivity) mContext).showErr();
    }
    protected boolean isAttachedContext(){
        return getActivity() != null;
    }
    /**
     * 检查activity连接情况
     */
    public void checkActivityAttached() {
        if (getActivity() == null) {
            throw new ActivityNotAttachedException();
        }
    }
    public static class ActivityNotAttachedException extends RuntimeException {
        public ActivityNotAttachedException() {
            super("Fragment has disconnected from Activity ! - -.");
        }
    }
}
```

### 3.3.4 MVP架构再优化——model层单独优化

model层相对其他单元来说比较特殊，更像一个整体，只是单纯帮上层那数据，再就是MVP理念让业务逻辑互相独立，导致每个网络请求被独立成单个Model，不光没必要而且找起来麻烦。

存在的问题：

- 无法对所有Model统一管理。
- 每个Model对外提供的获取数据方法不一样，上层请求数据没有规范。
- 代码冗余高，网络数据请求除URL和参数外其他大概都一样的。
- 对已存在的Model管理困难，不能直观的统计已存在的Model。

这里将Model层整体封装成庞大且独立单一模块

![](http://www.jcodecraeer.com/uploads/userup/13953/1G020140036-5C1-7.jpeg)

**优势**：

- 数据请求单独编写，无需配合上层界面测试。
- 统一管理，修改方便
- 实现不同数据源(NetAPI,cache,database)的无缝切换

![](http://www.jcodecraeer.com/uploads/userup/13953/1G024140A1-52C-0.jpeg)

**Presenter 请求数据不再直接调用具体的Model对象，统一以 `DataModel` 类作为数据请求层的入口，以常量类 `Token` 区别具体请求。** DataModel会根据Token的不同拉取底层对应的具体Model。这样可以利用Token类直观的统计出已存在的请求接口。

这里需要设计几个类：

- `DataModel`： 数据层顶级入口，项目中所有数据都由该类流入和流出，负责分发所有的请求数据。
- `Token`：数据请求标识类，定义了项目中所有的数据请求。
- `BaseModel`：所有Model的顶级父类，负责对外提供数据请求标准，对内为所有Model提供请求的底层支持



**BaseModel**

BaseModel中定义了对外的请求数据规则，包括设置参数的方法和设置Callback的方法，还可以定义一些通用的数据请求方法，比如说网络请求的Get和Post方法。

```java
public abstract class BaseModel<T>  {
    //数据请求参数
    protected String[] mParams;
    /**
     * 设置数据请求参数
     * @param args 参数数组
     */
    public  BaseModel params(String... args){
        mParams = args;
        return this;
    }
    // 添加Callback并执行数据请求
    // 具体的数据请求由子类实现
    public abstract void execute(Callback<T> callback);
    // 执行Get网络请求，此类看需求由自己选择写与不写
    protected void requestGetAPI(String url,Callback<T> callback){
        //这里写具体的网络请求
    }
    // 执行Post网络请求，此类看需求由自己选择写与不写
    protected void requestPostAPI(String url, Map params,Callback<T> callback){
        //这里写具体的网络请求
    }
}
```

具体的Model：

```java
public class UserDataModel extends BaseModel<String> {
    @Override
    public void execute(final Callback<String> callback) {
        // 模拟网络请求耗时操作
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                // mParams 是从父类得到的请求参数
                switch (mParams[0]){
                    case "normal":
                        callback.onSuccess("根据参数"+mParams[0]+"的请求网络数据成功");
                        break;
                    case "failure":
                        callback.onFailure("请求失败：参数有误");
                        break;
                    case "error":
                        callback.onError();
                        break;
                }
                callback.onComplete();
            }
        },2000);
    }
}
```

实现具体Model请求必须要重写BaseModel的抽象方法`execute`。

**DataModel**

DataModel负责数据请求的分发，可以做成一个简单工厂模式，通过`switch（token）`语句判断要调用的model，但是这样设计每添加一个数据请求接口，不光需要新建对应的Model和Token，还需要在DataModel类的`switch(token)`语句中新增加对应的判断。

可以利用反射机制，请求数据时以具体Model的包名+类型作为Token，利用反射机制直接找到对应的Model。

```java
public class DataModel {
    public static BaseModel request(String token){
        // 声明一个空的BaseModel
        BaseModel model = null;
        try {
            //利用反射机制获得对应Model对象的引用
            model = (BaseModel)Class.forName(token).newInstance();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        return model;
    }
}
```

**Token**

由于上节中DataModel使用反射机制获取对应Model的引用，所以Token中存的就应该是对应Model的包名+类名：

```java
public class Token {
    // 包名
    private static final String PACKAGE_NAME = "com.jesse.mvp.data.model.";
    // 具体Model
    public static final String API_USER_DATA = PACKAGE_NAME + "UserDataModel";
}
```

**使用步骤**：

完成了Model层之后再去Presenter调用数据

```java
DataModel
    // 设置请求标识token
    .request(Token.API_USER_DATA)
    // 添加请求参数，没有则不添加
    .params(userId)
    // 注册监听回调
    .execute(new Callback<String>() {
           @Override
           public void onSuccess(String data) {
               //调用view接口显示数据
               mView.showData(data);
           }
           @Override
           public void onFailure(String msg) {
               //调用view接口提示失败信息
               mView.showFailureMessage(msg);
           }
           @Override
           public void onError() {
               //调用view接口提示请求异常
               mView.showErrorMessage();
           }
           @Override
           public void onComplete() {
               // 隐藏正在加载进度条
               mView.hideLoading();
           }
 });
```

**添加Model的步骤**

1. 新建一个Model并继承BaseModel，完成具体的数据请求。
2. 在Token中添加对用的Model包名+类名。注意写好注释，方便以后查阅。



# 4. MVP结构理解

