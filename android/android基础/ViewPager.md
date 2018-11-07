# 1 ViewPager简单使用

参考文献：

1. [参考文献一](http://www.runoob.com/w3cnote/android-tutorial-viewpager.html)
2. [参考文献二](https://juejin.im/post/5a4c2f496fb9a044fd122631)
3. [参考文献三](https://www.jianshu.com/p/e5abbda4a71c)
4. [参考文献四](https://www.jianshu.com/p/6b1008fcc082)

**ViewPager**主要包括以下内容：

- ViewPager 基本使用（简介、适配器）
- ViewPager + TabLayout + Fragment 的使用
- ViewPager 轮播图的使用（指示器、标题、自动轮播、首尾循环）
- ViewPager 的切换效果（PageTransformer）
- ViewPager 切换效果进阶

## ViewPager基础介绍

ViewPager就是一个简单的页面切换组件，我们可以往里面填充多个View，然后我们可以左 右滑动，从而切换不同的View。

> 这里简单归结如下：
>
> - ViewPager 是 v4 包中的一个类。
> - ViewPager 类直接继承了 ViewGroup 类，它是一个容器类，可以在其中添加其他的 view 。
> - 类似于 ListView，也有自己的适配器，用来填充数据页面



ViewPager常用的可以动态设置方法有：

- `setAdapter(PagerAdapter adapter)` 设置适配器
- `setOffscreenPageLimit(int limit)` 设置缓存的页面个数,默认是 1
- `setCurrentItem(int item)` 跳转到特定的页面
- `setOnPageChangeListener(..)` 设置页面滑动时的监听器（现在API中建议使用 `addOnPageChangeListener(..)`）
- `setPageTransformer(..PageTransformer)` 设置页面切换时的动画效果
- `setPageMargin(int marginPixels)` 设置不同页面之间的间隔
- `setPageMarginDrawable(..)` 设置不同页面间隔之间的装饰图也就是 divide ，要想显示设置的图片，需要同时设置 `setPageMargin()`

前面学的ListView，GridView一样，我们也需要一个Adapter (适配器)将我们的View和ViewPager进行绑定，而ViewPager则有一个特定的Adapter—— **PagerAdapter**！

Google官方是建议我们使用Fragment来填充ViewPager的，这样 可以更加方便的生成每个Page，以及管理每个Page的生命周期！给我们提供了两个Fragment 专用的Adapter：**FragmentPagerAdapter**和**FragmentStatePagerAdapter** 我们简要的来分析下这两个Adapter的区别：

- **FragmentPagerAdapter**：和PagerAdapter一样，只会缓存当前的Fragment以及左边一个，右边 一个，即总共会缓存3个Fragment而已，假如有1，2，3，4四个页面：
  处于1页面：缓存1，2
  处于2页面：缓存1，2，3
  处于3页面：销毁1页面，缓存2，3，4
  处于4页面：销毁2页面，缓存3，4
  更多页面的情况，依次类推~
- **FragmentStatePagerAdapter**：当Fragment对用户不见得时候，整个Fragment会被销毁， 只会保存Fragment的状态！而在页面需要重新显示的时候，会生成新的页面！

综上，**FragmentPagerAdapter适合固定的页面较少的场合；而FragmentStatePagerAdapter则适合 于页面较多或者页面内容非常复杂**(需占用大量内存)的情况！



## PagerAdapter的使用

PagerAdapter 是抽象的类，所以使用时只能使用它的子类，即**FragmentPagerAdapter**和**FragmentStatePagerAdapter** 。

**实现子类必须实现以下四个方法**（一般重写`getCount()`与`isViewFromObject()`即可）：

- `getCount();` 是获取当前窗体界面数，也就是数据的个数。即Viewpager中有多少个view
- `destroyItem(ViewGroup container, int position, Object object);` 如果页面不是当前显示的页面也不是要缓存的页面，会调用这个方法，将页面销毁。确保在`finishUpdate(viewGroup)`返回时视图能够被移除。
- `instantiateItem(View container, int position);` 要显示的页面或需要缓存的页面，会调用这个方法进行布局的初始化。
- `isViewFromObject(View view, Object object);` 这个方法用于判断是否由对象生成界面，官方建议直接返回 `return view == object;`。

**ViewPager的常用方法有**：

- **setAdapter(PagerAdapter adapter)**
  建立与适配器的联系
  adapter:与ViewPager配合使用的适配器

- **ViewPager.getCurrentItem()**

  返回当前页的索引，也就是返回当前显示的页码；

  adapter:与ViewPager配合使用的适配器

- **ViewPager.setCurrentItem(int position)**

  设置当前页，position:被设置的当前页的索引；

### PagerAdapter最简单用法

**<font color = red>示例</font>**：

- 编写每个View的布局，（view_one.xml）

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:background="#FFBA55"
      android:gravity="center"
      android:orientation="vertical">
  
      <TextView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="第一个Page"
          android:textColor="#000000"
          android:textSize="18sp"
          android:textStyle="bold" />
  
  </LinearLayout>
  ```

- 编写activity_main.xml布局

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="vertical"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <android.support.v4.view.ViewPager
          android:id="@+id/vpager_one"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
  </LinearLayout>
  ```

- 编写自定义的PagerAdapter

  ```java
  public class MyPagerAdapter extends PagerAdapter {
  
      private ArrayList<View> viewLists;
  
      public MyPagerAdapter() {
  
      }
  
      public MyPagerAdapter(ArrayList<View> viewLists) {
          super();
          this.viewLists = viewLists;
      }
  
      @Override
      public int getCount() {
          return viewLists.size();
      }
  
      @Override
      public boolean isViewFromObject(@NonNull View view, @NonNull Object o) {
          return view == o;
      }
  
      @NonNull
      @Override
      public Object instantiateItem(@NonNull ViewGroup container, int position) {
          container.addView(viewLists.get(position));
          return viewLists.get(position);
      }
  
      @Override
      public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
          container.removeView(viewLists.get(position));
      }
  }
  ```

- 编写MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ViewPager vpager_one;
      private ArrayList<View> aList;
      private MyPagerAdapter mAdapter;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          vpager_one = (ViewPager) findViewById(R.id.vpager_one);
  
          aList = new ArrayList<View>();
          LayoutInflater li = getLayoutInflater();
          aList.add(li.inflate(R.layout.view_one,null,false));
          aList.add(li.inflate(R.layout.view_two,null,false));
          aList.add(li.inflate(R.layout.view_three,null,false));
          aList.add(li.inflate(R.layout.view_one,null,false));
          mAdapter = new MyPagerAdapter(aList);
          vpager_one.setAdapter(mAdapter);
      }
  }
  ```

### 标题栏——PagerTitleStrip与PagerTabStrip

标题栏，即随着ViewPager滑动而滑动的标题。**前一个是普通文字，后一个是带有下划线以及可以点击文字切换页面**。

二者的代码区别：**仅仅是布局不同**

- PagerTitleStrip所在Activity布局：activity_two.xml

  ```xml
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <TextView
          android:layout_width="match_parent"
          android:layout_height="48dp"
          android:background="#CCFF99"
          android:gravity="center"
          android:text="PagerTitleStrip效果演示"
          android:textColor="#000000"
          android:textSize="18sp" />
  
      <android.support.v4.view.ViewPager
          android:id="@+id/vpager_two"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_gravity="center">
  
          <android.support.v4.view.PagerTitleStrip
              android:id="@+id/pagertitle"
              android:layout_width="wrap_content"
              android:layout_height="40dp"
              android:layout_gravity="top"
              android:textColor="#000000" />
      </android.support.v4.view.ViewPager>
  
  </LinearLayout>
  ```

- PagerTabStrip所在布局：activity_three.xml:

  ```xml
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical">
  
      <TextView
          android:layout_width="match_parent"
          android:layout_height="35dp"
          android:background="#C0C080"
          android:gravity="center"
          android:text="PagerTabStrip效果演示"
          android:textSize="18sp" />
  
      <android.support.v4.view.ViewPager
          android:id="@+id/vpager_three"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_gravity="center">
  
          <android.support.v4.view.PagerTabStrip
              android:id="@+id/pagertitle"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:layout_gravity="top" />
      </android.support.v4.view.ViewPager>
  </LinearLayout>
  ```

- 编写自定义的PagerAdapter，需要额外重写一个方法：getPageTitle()，用来设置标题

  ```java
  public class MyPagerAdapter extends PagerAdapter {
  
      private ArrayList<View> viewLists;
      private ArrayList<String> titleLists;
  
      public MyPagerAdapter() {
  
      }
  
      public MyPagerAdapter(ArrayList<View> viewLists, ArrayList<String> titleLists) {
          super();
          this.viewLists = viewLists;
          this.titleLists = titleLists;
      }
  
      @Override
      public int getCount() {
          return viewLists.size();
      }
  
      @Override
      public boolean isViewFromObject(@NonNull View view, @NonNull Object o) {
          return view == o;
      }
  
      @NonNull
      @Override
      public Object instantiateItem(@NonNull ViewGroup container, int position) {
          container.addView(viewLists.get(position));
          return viewLists.get(position);
      }
  
      @Override
      public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
          container.removeView(viewLists.get(position));
      }
  
      @Nullable
      @Override
      public CharSequence getPageTitle(int position) {
          return titleLists.get(position);
      }
  }
  ```

- 最后编写MainActivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private ViewPager vpager_two;
      private ArrayList<View> aList;
      private ArrayList<String> sList;
      private MyPagerAdapter mAdapter;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_three);
          vpager_two = (ViewPager) findViewById(R.id.vpager_three);
  
          aList = new ArrayList<View>();
          LayoutInflater li = getLayoutInflater();
          aList.add(li.inflate(R.layout.view_one,null,false));
          aList.add(li.inflate(R.layout.view_two,null,false));
          aList.add(li.inflate(R.layout.view_three,null,false));
          sList = new ArrayList<String >();
          sList.add("橘黄");
          sList.add("淡黄");
          sList.add("浅棕");
          mAdapter = new MyPagerAdapter(aList, sList);
          vpager_two.setAdapter(mAdapter);
      }
  }
  ```

# 2. ViewPager循环实现

实现ViewPager无限循环有2种方法：

- 重写PagerAdapter中的getCount()方法
- 重写OnPagerChangeListener接口中的onPageSelected()方法

## 循环原理

![](https://img-blog.csdn.net/20160502164043990)

如果旧数据结构是1、2、3，拼接后的数据格式是3、1、2、3、1。

初始化数据，让ViewPager指向position=1这个位置，如果向左滑，当前显示List<0>这个页面，这会儿让ViewPager重新指向红色3的位置，即`List<length-2>`。

如果当前位置在红色3页面，如果向右滑，当前显示的`List<length-1>`这个页面，重新setCurrentItem指定到红色1位置

示例：

- 修改activity_main.xml布局文件，

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <android.support.v4.view.ViewPager
          android:id="@+id/view_pager"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
  
  </LinearLayout>
  ```

- 新建viewpager_item1.xml文件，设置ViewPager页面

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <ImageView
          android:id="@+id/image_view"
          android:layout_width="match_parent"
          android:layout_height="match_parent" />
      <TextView
          android:id="@+id/text_view"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_gravity="bottom|end"
          android:layout_margin="10dp"
          android:textColor="#000000"/>
  
  </FrameLayout>
  ```

- 自定义适配器MyPagerAdapter，继承PagerAdapter类

  ```java
  public class MyPagerAdapter extends PagerAdapter {
  
      private List<View> viewList;
      private List<Integer> drawableList;
  
      public MyPagerAdapter() {
  
      }
      public MyPagerAdapter(List<View> viewList, List<Integer> drawableList) {
          this.viewList = viewList;
          this.drawableList = drawableList;
      }
  
      @Override
      public int getCount() {
          return viewList.size();
      }
  
      @Override
      public boolean isViewFromObject(@NonNull View view, @NonNull Object o) {
          return view == o;
      }
  
      @NonNull
      @Override
      public Object instantiateItem(@NonNull ViewGroup container, int position) {
          View view = viewList.get(position);
          ImageView imageView = (ImageView) view.findViewById(R.id.image_view);
          imageView.setImageResource(drawableList.get(position));
          TextView textView = (TextView) view.findViewById(R.id.text_view);
          textView.setText(String.valueOf(position));
          container.addView(view);
          return view;
      }
  
      @Override
      public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
          container.removeView(viewList.get(position));
      }
  }
  ```

- 设置滑动监听器

  ```java
  public class CycleScrollOnPageChangeListener implements ViewPager.OnPageChangeListener {
  
      private List<View> viewList;
      private ViewPager viewPager;
  
      public CycleScrollOnPageChangeListener() {
  
      }
  
      public CycleScrollOnPageChangeListener(List<View> viewList, ViewPager viewPager) {
          this.viewList = viewList;
          this.viewPager = viewPager;
      }
  
      @Override
      public void onPageSelected(int i) {
  
      }
  
      @Override
      public void onPageScrolled(int i, float v, int i1) {
          if (v == 0) {
              if (i == 0) {
                  viewPager.setCurrentItem(viewList.size() - 2, false);
              } else if(i == (viewList.size() - 1)) {
                  viewPager.setCurrentItem(1,false);
              }
          }
      }
  
      @Override
      public void onPageScrollStateChanged(int i) {
  
      }
  }
  ```

- 修改Mainactivity.java代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private int[] drawableIds = new int[] {R.drawable.a, R.drawable.b, R.drawable.c,
              R.drawable.d, R.drawable.e};
      private List<View> viewList = new ArrayList<>();
      private ViewPager viewPager;
      private List<Integer> drawableList;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          viewPager = (ViewPager) findViewById(R.id.view_pager);
          initData();
          createPageItems();
          viewPager.setAdapter(new MyPagerAdapter(viewList, drawableList));
          viewPager.addOnPageChangeListener(new CycleScrollOnPageChangeListener(viewList, viewPager));
          viewPager.setCurrentItem(viewList.size() - 2, false);
          viewPager.setVisibility(View.INVISIBLE);
          viewPager.postDelayed(new Runnable() {
  
              @Override
              public void run() {
                  viewPager.setVisibility(View.VISIBLE);
                  // 设置初始 position
                  viewPager.setCurrentItem(1, false);
              }
          }, 100);
  
  
  
      }
  
      private void initData() {
          drawableList = new ArrayList<Integer>();
          drawableList.add(drawableIds[drawableIds.length - 1]);
          for (int id : drawableIds) {
              drawableList.add(id);
          }
          drawableList.add(drawableIds[0]);
      }
  
      public void createPageItems() {
          LayoutInflater inflater = LayoutInflater.from(this);
          for (int i = 0; i < drawableList.size(); i++) {
              View view = inflater.inflate(R.layout.viewpager_item1, null);
              viewList.add(view);
          }
      }
  }
  ```



