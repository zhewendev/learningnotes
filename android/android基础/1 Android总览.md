# 1 android系统架构

**参考文献**：

- [API](https://developer.android.com/guide/components/fundamentals)
- 参考文献一：**第一行代码**

------



- **android大致可以分为四层架构**：Linux内核层，系统运行库层、应用框架层和应用层

  - **Linux内核层**

    android系统是基于Linux内核，这一层为android设备的各种硬件提供了底层驱动。

  - **系统运行库层**

    通过一些C/C++库来为android提供了主要的特性支持。还有android运行时库，主要提供一些库，允许开发者使用Java语言来编写android应用。

  - **应用结构层**

    提供了构建应用程序时可能用到的各种API。

  - **应用层**

    手机上的应用程序

![](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/The-Android-software-stack.png/800px-The-Android-software-stack.png)

# 2 应用基础知识

Android 应用采用 Java 编程语言编写。Android SDK 工具将您的代码 — 连同任何数据和资源文件 — 编译到一个 **APK**：*Android 软件包*，即带有 `.apk` 后缀的存档文件中。一个 APK 文件包含 Android 应用的所有内容，它是基于 Android 系统的设备用来安装应用的文件。

安装到设备后，**每个 Android 应用都运行在自己的安全沙箱内**：

- Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户；
- 默认情况下，系统会为**每个应用分配一个唯一的 Linux 用户 ID**（该 ID 仅由系统使用，应用并不知晓）。系统为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；
- 每个进程都具有自己的虚拟机 (VM)，因此应用代码是在与其他应用隔离的环境中运行；
- 默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 会在需要执行任何应用组件时启动该进程，然后在不再需要该进程或系统必须为其他应用恢复内存时关闭该进程

Android 系统可以通过这种方式实现*最小权限原则*。不过，应用仍可以通过一些途径来与其他应用共享数据及访问系统服务。

## 应用组件

应用组件是 Android 应用的基本构建基块。共有四种不同的应用组件类型。每种类型都服务于不同的目的，并且具有定义组件的创建和销毁方式的不同生命周期。

- **Activity**（活动）

  *Activity* 表示具有用户界面的单一屏幕。凡是应用中看得到的都是放在活动中

- **Service**（服务）

  *服务*是一种在后台运行的组件，用于执行长时间运行的操作或为远程进程执行作业。 服务不提供用户界面。 

- **Broadcast Receiver**（广播接收器）

  *广播接收器*是一种用于响应系统范围广播通知的组件。例如电话，短信等

- **Content Provider**（内容提供器）

  *内容提供程序*管理一组共享的应用数据，为应用程序之间数据共享提供了可能。

Android 系统设计的独特之处在于，**任何应用都可以启动其他应用的组件**。与大多数其他系统上的应用不同，Android 应用并没有单一入口点（例如，没有 `main()` 函数）。

## 启动组件

四种组件类型中的三种 — **Activity、服务和广播接收器** — 通过名为 *Intent* 的异步消息进行启动。

每种类型的组件有不同的启动方法：

- 您可以通过将 `Intent` 传递到 `startActivity()` 或 `startActivityForResult()`（当您想让 Activity 返回结果时）来启动 Activity（或为其安排新任务）。
- 您可以通过将　`Intent` 传递到 `startService()` 来启动服务（或对执行中的服务下达新指令）。 或者，您也可以通过将 `Intent` 传递到 `bindService()` 来绑定到该服务。
- 您可以通过将 `Intent` 传递到 `sendBroadcast()`、`sendOrderedBroadcast()` 或 `sendStickyBroadcast()` 等方法来发起广播；
- 您可以通过在 `ContentResolver` 上调用 `query()` 来对内容提供程序执行查询。

## 清单文件

在 Android 系统启动应用组件之前，系统必须通过读取应用的 `AndroidManifest.xml` 文件（“清单”文件）**确认组件存在**。 您的应用必须在此文件中声明其所有组件，该文件**必须位于应用项目目录的根目录中**。

除了声明应用的组件外，清单文件还有许多其他作用，如：

- 确定应用需要的任何用户权限，如互联网访问权限或对用户联系人的读取权限
- 根据应用使用的 API，声明应用所需的最低 [API 级别](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- 声明应用使用或需要的硬件和软件功能，如相机、蓝牙服务或多点触摸屏幕
- 应用需要链接的 API 库（Android 框架 API 除外），如 [Google 地图库](http://code.google.com/android/add-ons/google-apis/maps-overview.html)
- 其他功能

------

- **声明组件**

  清单文件的主要任务是告知系统有关应用组件的信息。

- **声明组件功能**

  当您在应用的清单文件中声明 Activity 时，可以选择性地加入声明 Activity 功能的 Intent 过滤器，以便响应来自其他应用的 Intent。

- **声明应用要求**

  您必须通过在清单文件中声明设备和软件要求，为您的应用支持的设备类型明确定义一个配置文件。

## 应用资源

Android 应用并非只包含代码 — 它还需要与源代码分离的资源，如图像、音频文件以及任何与应用的视觉呈现有关的内容。提供了丰富的系统控件爱你，SQLite数据库，强大的多媒体，地理位置定位等。



# 3 分析android程序

![](E:\document\photo\first-code\1.JPG)

项目结构模式切换为Project：

![](E:\document\photo\first-code\2.JPG)

## 项目结构模式

- `.gradle`与`.ideal`

  放置android studio自动生成的文件

- `app`

  存放项目中的代码，资源等内容

- `.gitignore`

  用来将指定的目录或文件排除在版本控制之外

- `build.gradle`

  项目全局gradle构建脚本

- `.gradel.properties`

  全局的gradle配置文件，其属性会影响到项目中所有的gradle编译脚本

- `.gradlew`和`gradle.bat`

  用在命令行中执行gradle命令。前者linux或mac，后者windows

- `HelloWorld.iml`

  标识这是一个IntelliJ IDEA项目，无需修改

- `local.properties`

  指定本机中Android SDK路径。

- `settings.gradle`

  用于指定项目中所有引入的模块

## app 目录下结构模式

- build：与外层的build相似，包含一些在编译时生成的文件
- libs：存放第三方jar包
- androidTest：**编写Android测试用例**
- java：放置所有java代码的地方
- res：项目中使用到的所有图片，布局，字符串等资源。
- AndroidMainfest.xml：整个项目的配置文件，ID你定义的四大组件都需要在这个文件里注册。
- test：**编写Unit test测试用例**
- .gitignore:将app模块内的指定的目录或文件排除在版本控制之外。
- build.gradle：指定很多项目构建相关的脚本
- proguard-rules.pro：指定项目代码的混淆规则，让破解者难以阅读

## 项目中的资源

详解res中的资源类型

- 所有以**drawble**开头的文件夹都是存放**图片**的
- 以**mipmap**开头的文件都是来放应用**图标**的
- 以**values**开头的都是用来放**字符串，样式，颜色**等配置。
- **layout**文件夹用来放**布局**文件

注意：这么多**mipmap**文件夹是为了让程序更好的兼容各种设备，**drawable**也是，需要自己创建。

# 日志工具的使用

日志工具中提供了5各方法来供打印日志：

- Log.v()：打印最为琐碎，意义最小的日志信息。对应级别verbose，级别最低
- Log.d()：打印调试信息，对应级别为debug，比verbose高一级。
- Log.i()：打印重要数据，分析用户行为数据，对应级别info，比debug高一级
- Log.w()：打印一些警告信息，对应级别为warn，比info高一级
- Log.e()：打印程序中错误信息，对应级别为error，最高级别





