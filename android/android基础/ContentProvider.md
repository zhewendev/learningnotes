# 1. ContentProvider基础

**参考文献**：

- 参考文献一：[Android开发指南](https://developer.android.com/guide/topics/providers/content-provider-basics?hl=zh-cn)
- 参考文献二：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-contentprovider.html)
- 参考文献三：第一行代码

------



**ContentProvider使用场景**：

- 想在自己的应用中访问别的应用，或者说一些ContentProvider暴露给我们的一些数据， 比如手机联系人，短信等！我们想对这些数据进行读取或者修改，这就需要用到ContentProvider了
- 我们自己的应用，想把自己的一些数据暴露出来，给其他的应用进行读取或操作，我们也可以用 到ContentProvider，另外我们可以选择要暴露的数据，就避免了我们隐私数据的的泄露！

![](http://www.runoob.com/wp-content/uploads/2015/08/58811327.jpg)

## ContentProvider概述

内容提供器主要用于不同应用程序之间竖线数据共享的功能，，提供一套完整的机制，允许一个程序访问另一个程序中的数据，保证被访问数据的安全性。

不同于文件存储和SharedPreferences存储中两种全局刻度写操作模式，内容提供器只对那一部分数据进行共享。

## 运行时权限

Android 6.0中加入了运行时权限功能，用户不需要在安装软件时一次性授权所有申请的权限。Android将所有权限归成两类，一类是**普通权限**，一类是**危险权限**，确切还有第三类**特殊权限**。

Android中所有危险权限一共是**9组24个权限**：

![](E:\document\photo\first-code\android危险权限.jpg)

非属于危险权限的，只需要在AndroidManifest.xml文件中添加权限声明即可。

进行运行时权限处理时使用的是**权限名**，一旦用户同意授权，改权限对应的权限组中所有的权限也会同时被授权。

- **Android6.0**之前，若要拨打电话，修改**activity_main.xml**文件

  ```xml
  <Button
      android:id="@+id/make_call"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Make Call" />
  ```

- 在MainActivity.java中构建一个隐式Intent，其action指向Intent。ACTION_CALL，系统内置打电话动作。

  ```java
  public class MainActivity extends AppCompatActivity {
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          Button makeCall = (Button) findViewById(R.id.make_call);
          makeCall.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                   try {
              		Intent intent = new Intent(Intent.ACTION_CALL);
              		intent.setData(Uri.parse("tel:10086"));
                      startActivity(intent);
                      } catch (SecurityException e) {
                      e.printStackTrace();
          		}
              });
     }
  }
  ```

- 在**AndroidManifest.xml**文件中声明权限：

  ```xml
  <uses-permission android:name="android.permission.CALL_PHONE"/>
  ```



在Android6.0及以上系统，使用危险权限都必须进行运行时权限处理：

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button makeCall = (Button) findViewById(R.id.make_call);
        makeCall.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (ContextCompat.checkSelfPermission(MainActivity.this, Manifest.permission.CALL_PHONE)
                != PackageManager.PERMISSION_GRANTED) {
                    ActivityCompat.requestPermissions(MainActivity.this, new String[]{Manifest.permission.CALL_PHONE}, 1);;
                } else {
                    call();
                }
            }
        });
    }

    private void call() {
        try {
            Intent intent = new Intent(Intent.ACTION_CALL);
            intent.setData(Uri.parse("tel:10086"));
            startActivity(intent);
        } catch (SecurityException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
            switch (requestCode) {
                case 1:
                    if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                        call();
                    } else {
                        Toast.makeText(this, "You denied the permission", Toast.LENGTH_SHORT).show();
                    }
                    break;
                default:
            }
    }
}
```

运行时权限处理流程步骤：

- 判断用户是否已经授权，借助`ContextCompat.checkSelfPermission()`方法，第一个参数是Context，第二个参数是具体权限名。用该方法的返回值与`PackageManager.PERMISSION_GRANTED`做比较，相等说明用户已授权，不等未授权
- 已授权则直接执行逻辑操作，未授权则需要调用`ActivityCompat.requestPermission()`方法向用户申请授权，接收三个参数，第一参数Acitivity实例，第二参数是String数组，存放权限名，第三是请求码。
- 调用该申请方法，弹出权限对话框，最终回调到`onRequestPermissionResult()`方法中，授权结果封装在`grantResults`参数中。这里判断一下授权结果来进行逻辑操作。



------

# 2. 访问其他程序数据

内容提供器用法一般有两种，

- 一是使用现有的内容提供器读取和操作相应程序中的数据
- 另一种是创建自己的内容提供器给我们程序中的数据提供外部访问接口

如果一个应用程序通过内容提供器对其数据提供了外部访问接口，那么任何其他应用程序就都可以对这部分数据进行访问。

## ContentResolver基本用法

若需访问内容提供器中共享数据，一定要借助**ContentResolver**类，通过**Context**中`getContentResolver()`方法获取到该类的实例。

**ContentResolver**类提供了一系列的方法用于对数据进行CRUD操作。不同于SQLiteDatabase，ContentResolver中的增删改查方法**都不接受表名参数**，使用一个**Uri**参数代替，这个参数称为内容URI。

内容URI给内容提供器数据建立了唯一标识符，主要有两部分组成：authority和path。

- **authority**用于对不同的应用程序做区分的，避免冲突，都会采用程序包名方式进行命名，程序包名为com.example.app,那么程序对应的authority既可以命名为com.example.app.provider。
- **path**用于对同一个程序中不同的表做区分，通常添加到authority之后。数据库中存放了两张表，table1和table2，就可以将path分别命名为/table1和table2。

二者组合，内容URI就变成了：**com.example.app.provider/table1**和**com.example.app.provider/table2**

最好还需要在字符串头部加上协议声明：

```java
content://com.example.app.provider/table1
content://com.example.app.provider/table2
```

得到内容URI字符串，还需将其解析为Uri对象才可以作为参数传入，解析方法为：

```java
Uri uri = Uri.parse("content://com.example.app.provider/table1")
```

即调用`Uri.parse()`方法。

**示例**：

```java
Cursor cursor = get ContentResolver().query(
uri,
projection,
selection,
selectionArgs,
sortOrder);
```

![](E:\document\photo\first-code\contentproviderQuery.jpg)

查询完成后返回仍然是一个Cursor对象，将数据从这个对象中取出来。通过移动游标的位置来遍历Cursor所有行，然后在取出每一行中相列的数据。

```java
if (cursor != null) {
    while (cursor.moveToNext()) {
        String column1 = cursor.getString(cursor.getColumnIndex("column1"));
        int column2 = cursor.getInt(cursor.getColumnIndex("column2"));
    }
    cursor.close();
}
```

- **添加数据**：

```java
ContentValues.values = new ContentValues();
values.put("column1", "text");
values.put("column2", 1);
getContentResolver().insert(uri, values);
```

- **更新数据**

```java
ContentValues values = new ContentValues();
values.put("column1", "");
getContentResolver().update(uri, values, "column1 = ? and column2 = ?", new String[]{"text", "1"});
```

- **删除数据**

```java
getContentResolver().delete(uri, "column2 = ?", new String[] {"1"});
```

------

## 读取联系人

- 新建ContactsTest项目，修改`activity_main.xml`

  ```xml
  <Button
      android:id="@+id/make_call"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Make Call" />
  ```

- 修改**MainActivity.java**代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      ArrayAdapter<String> adapter;
      List<String> contactsList = new ArrayList<>();
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          ListView contactsView = (ListView) findViewById(R.id.contacts_view);
          adapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, contactsList);
          contactsView.setAdapter(adapter);
          if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS)
                  != PackageManager.PERMISSION_GRANTED) {
              ActivityCompat.requestPermissions(this, new String[]
                      { Manifest.permission.READ_CONTACTS }, 1);
          } else {
              readContacts();
          }
      }
  
      private void readContacts() {
          Cursor cursor = null;
          try {
              //查询联系人数据
              cursor = getContentResolver().query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
                      null, null, null, null);
              if (cursor != null) {
                  while (cursor.moveToNext()) {
                      //获取联系人姓名
                      String displayName = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
                      //获取联系人手机号
                      String number = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
                      contactsList.add(displayName + "\n" + number);
                  }
                  adapter.notifyDataSetChanged();
              }
          } catch (Exception e) {
              e.printStackTrace();
          } finally {
              if (cursor != null) {
                  cursor.close();
              }
          }
      }
  
      @Override
      public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
          switch (requestCode) {
              case 1:
                  if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                      readContacts();
                  } else {
                      Toast.makeText(this, "You denied the permission", Toast.LENGTH_SHORT).show();
                  }
                  break;
              default:
          }
      }
  }
  ```

- 修改**AndroidManifest.xml**文件添加权限：

  ```xml
  <uses-permission android:name="android.permission.READ_CONTACTS"/>
  ```

**注**：

- **ContactsContract.CommonDataKinds.Phone**类做了封装，提供了一个**CONTENT_RUI**常量，这个常量就是使用`Uri.parse()`方法解析出来的结果。
- 联系人对应的常量是**ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME**
- 手机号对应常量是：**ContactsContract.CommonDataKinds.Phone.NUMBER**

## 创建内容提供器

![](http://www.runoob.com/wp-content/uploads/2015/08/40787698.jpg)

实现跨程序共享数据，使用内容提供器，可以通过新建一个类去继承ContentProvider的方式来创建，该类有6个抽象方法需要重写：

示例：

```java
public class MyProvider extends ContentProvider {

     @Override
    public boolean onCreate() {
    return false;
    }
    
    @Override
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
        return null;
    }
    
    @Override
    public Uri insert(Uri uri, ContentValues) {
        return null;
    }
    
    @Override
    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
        return 0;
    }
    
    @Override
    public int delete(Uri uri, String selection, String[] selectionArgs) {
        return 0;
    }
    
    @Override
    public String getType(Uri uri) {
        return null;
    }
}
```

- `onCreate()`：初始内容提供器时调用，完成数据库创建与升级等操作。只有当存在ContentResolver尝试访问我们程序中的数据时，内容提供器才会被初始化
- `query()`：从内容提供器中查询数据
- `insert()`：向内容提供器添加数据。添加完成返回一个用于表示这条新纪录的URI
- `update()`：更新内容提供器已有的数据。返回受影响的行数
- `delete()`：从内容提供器删除数据。被删除的行数作为返回值返回
- `getType()`根据传入的内容URI来返回相应的MIME类型

可以在内容URI后面加一个id：

```java
content://com.example.app.provider/table1/1
```

表示调用方期望访问的是com.example.app这个应用的table1表中id为1的数据

内容URI格式主要有：以路径结尾表示期望访问该表中所有的数据，以id结尾表示期望访问该表中拥有相应的id数据。可以使用通配符方式匹配这两种格式的内容URI：

- *：表示匹配任意长度的任意字符

  ```java
  content://com.example.app.provider/*
  ```

  表示一个能够匹配任意表的内容URI格式

- #：表示匹配任意长度的数字

  ```java
  content://com.example.app.provider/table1/#
  ```

  表示能够匹配table1表中任意一行数据的内容URI格式



**UriMatcher**类可以实现匹配内容URI功能。**UriMatcher**类中提供了一个`addURI()`方法，接收三个参数：authority、path和一个自定义代码。调用**UriMatcher**的`match()`方法，可以将一个Uri对象传入，返回值是某个能够匹配这个Uri对象所对应的自定义码。利用这个码可以判断出调用方期望访问是哪种表中的数据。

**示例**：

```java
public class DatabaseProvider extends ContentProvider {

    public static final int BOOK_DIR = 0;

    public static final int BOOK_ITEM = 1;

    public static final int CATEGORY_DIR = 2;

    public static final int CATEGORY_ITEM = 3;

    public static final String AUTHORITY = "com.example.databasetest.provider";

    private static UriMatcher uriMatcher;

    private MyDatabaseHelper dbHelper;

    static {
        uriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
        uriMatcher.addURI(AUTHORITY, "book", BOOK_DIR);
        uriMatcher.addURI(AUTHORITY, "book/#", BOOK_ITEM);
        uriMatcher.addURI(AUTHORITY, "category", CATEGORY_DIR);
        uriMatcher.addURI(AUTHORITY, "category/#", CATEGORY_ITEM);
    }
    @Override
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
        // 查询数据
        SQLiteDatabase db = dbHelper.getReadableDatabase();
        Cursor cursor = null;
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
                cursor = db.query("Book", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                cursor = db.query("Book", projection, "id = ?", new String[] { bookId }, null, null, sortOrder);
                break;
            case CATEGORY_DIR:
                cursor = db.query("Category", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                cursor = db.query("Category", projection, "id = ?", new String[] { categoryId }, null, null, sortOrder);
                break;
            default:
                break;
        }
        return cursor;
    }
｝
```



关于`getType()`方法。用于获取Uri对象所对应的**MIME**类型。一个内容URI所对应的MIME字符串主要有3部分组成：

- 必须以**vnd**开头
- 内容URI一路径结尾，后接**android.cursor.dir/**，如果内容URI以id结尾，后接**android.cursor.item/**
- 最后接上vnd.<authority>.<path>

示例：**content://com.example.app.provider/table1**对应的MIME类型可以写成：

```java
vnd.android.cursor.dir/vnd.content://com.example.app.provider/table1
```

关于`getType()`方法中逻辑，代码示例：

```java
@Override
public String getType(Uri uri) {
    switch (uriMatcher.match(uri)) {
        case BOOK_DIR:
            return "vnd.android.cursor.dir/vnd.com.example.databasetest. provider.book";
        case BOOK_ITEM:
            return "vnd.android.cursor.item/vnd.com.example.databasetest. provider.book";
        case CATEGORY_DIR:
            return "vnd.android.cursor.dir/vnd.com.example.databasetest. provider.category";
        case CATEGORY_ITEM:
            return "vnd.android.cursor.item/vnd.com.example.databasetest. provider.category";
    }
    return null;
}
```

------

## 实现跨程序共享

修改**DatabaseProvider**代码

```java
public class DatabaseProvider extends ContentProvider {

    public static final int BOOK_DIR = 0;

    public static final int BOOK_ITEM = 1;

    public static final int CATEGORY_DIR = 2;

    public static final int CATEGORY_ITEM = 3;

    public static final String AUTHORITY = "com.example.databasetest.provider";

    private static UriMatcher uriMatcher;

    private MyDatabaseHelper dbHelper;

    static {
        uriMatcher = new UriMatcher(UriMatcher.NO_MATCH);
        uriMatcher.addURI(AUTHORITY, "book", BOOK_DIR);
        uriMatcher.addURI(AUTHORITY, "book/#", BOOK_ITEM);
        uriMatcher.addURI(AUTHORITY, "category", CATEGORY_DIR);
        uriMatcher.addURI(AUTHORITY, "category/#", CATEGORY_ITEM);
    }

    @Override
    public boolean onCreate() {
        dbHelper = new MyDatabaseHelper(getContext(), "BookStore.db", null, 2);
        return true;
    }

    @Override
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
        // 查询数据
        SQLiteDatabase db = dbHelper.getReadableDatabase();
        Cursor cursor = null;
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
                cursor = db.query("Book", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                cursor = db.query("Book", projection, "id = ?", new String[] { bookId }, null, null, sortOrder);
                break;
            case CATEGORY_DIR:
                cursor = db.query("Category", projection, selection, selectionArgs, null, null, sortOrder);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                cursor = db.query("Category", projection, "id = ?", new String[] { categoryId }, null, null, sortOrder);
                break;
            default:
                break;
        }
        return cursor;
    }

    @Override
    public Uri insert(Uri uri, ContentValues values) {
        // 添加数据
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        Uri uriReturn = null;
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
            case BOOK_ITEM:
                long newBookId = db.insert("Book", null, values);
                uriReturn = Uri.parse("content://" + AUTHORITY + "/book/" + newBookId);
                break;
            case CATEGORY_DIR:
            case CATEGORY_ITEM:
                long newCategoryId = db.insert("Category", null, values);
                uriReturn = Uri.parse("content://" + AUTHORITY + "/category/" + newCategoryId);
                break;
            default:
                break;
        }
        return uriReturn;
    }

    @Override
    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
        // 更新数据
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        int updatedRows = 0;
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
                updatedRows = db.update("Book", values, selection, selectionArgs);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                updatedRows = db.update("Book", values, "id = ?", new String[] { bookId });
                break;
            case CATEGORY_DIR:
                updatedRows = db.update("Category", values, selection, selectionArgs);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                updatedRows = db.update("Category", values, "id = ?", new String[] { categoryId });
                break;
            default:
                break;
        }
        return updatedRows;
    }

    @Override
    public int delete(Uri uri, String selection, String[] selectionArgs) {
        // 删除数据
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        int deletedRows = 0;
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
                deletedRows = db.delete("Book", selection, selectionArgs);
                break;
            case BOOK_ITEM:
                String bookId = uri.getPathSegments().get(1);
                deletedRows = db.delete("Book", "id = ?", new String[] { bookId });
                break;
            case CATEGORY_DIR:
                deletedRows = db.delete("Category", selection, selectionArgs);
                break;
            case CATEGORY_ITEM:
                String categoryId = uri.getPathSegments().get(1);
                deletedRows = db.delete("Category", "id = ?", new String[] { categoryId });
                break;
            default:
                break;
        }
        return deletedRows;
    }

    @Override
    public String getType(Uri uri) {
        switch (uriMatcher.match(uri)) {
            case BOOK_DIR:
                return "vnd.android.cursor.dir/vnd.com.example.databasetest. provider.book";
            case BOOK_ITEM:
                return "vnd.android.cursor.item/vnd.com.example.databasetest. provider.book";
            case CATEGORY_DIR:
                return "vnd.android.cursor.dir/vnd.com.example.databasetest. provider.category";
            case CATEGORY_ITEM:
                return "vnd.android.cursor.item/vnd.com.example.databasetest. provider.category";
        }
        return null;
    }

}
```

- 修改**activity_main.xml**

  ```xml
  <Button
      android:id="@+id/create_database"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Create database"
      />
  
  <Button
      android:id="@+id/add_data"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Add data"
      />
  
  <Button
      android:id="@+id/update_data"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Update data"
      />
  
  <Button
      android:id="@+id/delete_data"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Delete data"
      />
  
  <Button
      android:id="@+id/query_data"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="Query data"
      />
  ```

- 修改**MainActivity.java**代码

  ```java
  public class MainActivity extends AppCompatActivity {
  
      private MyDatabaseHelper dbHelper;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          dbHelper = new MyDatabaseHelper(this, "BookStore.db", null, 2);
          Button createDatabase = (Button) findViewById(R.id.create_database);
          createDatabase.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  dbHelper.getWritableDatabase();
              }
          });
          Button addData = (Button) findViewById(R.id.add_data);
          addData.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  SQLiteDatabase db = dbHelper.getWritableDatabase();
                  ContentValues values = new ContentValues();
                  // 开始组装第一条数据
                  values.put("name", "The Da Vinci Code");
                  values.put("author", "Dan Brown");
                  values.put("pages", 454);
                  values.put("price", 16.96);
                  db.insert("Book", null, values); // 插入第一条数据
                  values.clear();
                  // 开始组装第二条数据
                  values.put("name", "The Lost Symbol");
                  values.put("author", "Dan Brown");
                  values.put("pages", 510);
                  values.put("price", 19.95);
                  db.insert("Book", null, values); // 插入第二条数据
              }
          });
          Button updateData = (Button) findViewById(R.id.update_data);
          updateData.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  SQLiteDatabase db = dbHelper.getWritableDatabase();
                  ContentValues values = new ContentValues();
                  values.put("price", 10.99);
                  db.update("Book", values, "name = ?", new String[] { "The Da Vinci Code" });
              }
          });
          Button deleteButton = (Button) findViewById(R.id.delete_data);
          deleteButton.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  SQLiteDatabase db = dbHelper.getWritableDatabase();
                  db.delete("Book", "pages > ?", new String[] { "500" });
              }
          });
          Button queryButton = (Button) findViewById(R.id.query_data);
          queryButton.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  SQLiteDatabase db = dbHelper.getWritableDatabase();
                  // 查询Book表中所有的数据
                  Cursor cursor = db.query("Book", null, null, null, null, null, null);
                  if (cursor.moveToFirst()) {
                      do {
                          // 遍历Cursor对象，取出数据并打印
                          String name = cursor.getString(cursor.getColumnIndex("name"));
                          String author = cursor.getString(cursor.getColumnIndex("author"));
                          int pages = cursor.getInt(cursor.getColumnIndex("pages"));
                          double price = cursor.getDouble(cursor.getColumnIndex("price"));
                          Log.d("MainActivity", "book name is " + name);
                          Log.d("MainActivity", "book author is " + author);
                          Log.d("MainActivity", "book pages is " + pages);
                          Log.d("MainActivity", "book price is " + price);
                      } while (cursor.moveToNext());
                  }
                  cursor.close();
              }
          });
      }
  
  }
  ```
