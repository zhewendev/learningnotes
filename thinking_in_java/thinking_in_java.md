

# 第1章 对象导论

## 1.1 抽象过程

所有编程语言都提供抽象机制。可以认为，人们所能够解决的问题的复杂性直接取决于抽象的**类型和质量**。所谓类型是指“所抽象得是什么”。面向对象方式将问题空间中的元素及其在空间中的表称为“对象”。这种思想的实质是：程序可以通过添加新类型的对象使自身适用于某个特定问题。

面向对象的语言大概有一下五个特性：

1. **万物皆为对象。**可以抽取待求解问题的任何概念化构件（狗，建筑等），将其表示为程序中的对象。
2. **程序是对象的集合，他们通过发送消息来告知彼此所要做的事。**想要请求一个对象，必须给该对象发送一条消息。，即特定对象的方法调用请求。
3. **每个对象都有自己的由其他对象所构成的存储。**可以通过创建包含现有对象的包的方式来创建新类型对象。
4. **每个对象都有其类型。**每个对象都是某个类（class）的实例（instance）
5. **某一特定类型的所有对象都可以接受同样的消息。**



## 1.2 每一个对象都有一个接口

在程序执行期间具有不同的状态而其他方面都相似的对象会被分到对象的类中，即关键字**class**由来。创建抽象数据类型（类）是面向对象程序设计个基本概念之一。抽象数据类型的运行方式与内置类型几乎一致：创建某一类型的变量（对象或实例），然后操作这些变量。其中每一个对象都属于定义了特性和行为的某个特定类。

因为类描述了具有相同特性（数据元素）和行为（功能）的对象集合，所以一个类实际上就是一个数据类型。每个对象都只能满足某些请求，这些请求由对象的接口（interface）定义，决定接口的便是类型。**接口定义了对某一特定对象所能发出的请求。**

## 1.3 每个对象都提供服务

开发或理解某个程序设计时，将对象想象为“服务提供者”。程序本身将向用户提供服务，通过调用其他对象的服务来实现这一目的。你的目标就是去创建能够提供理想的服务来解决问题的一系列对象。

这种思想的另一个附带好处就是：**有助于提高对象的内聚性**。高内聚是软件设计的基本质量要求之一：意味着软件构件的各个方面组合的很好。每个对象都可以很好的完成一项任务，但是它并不试图做更多的事。

## 1.4 被隐藏的具体实现

程序开发人员按照角色分为**类创建者**和**客户端程序员**。**类创建者的目标**是构建类，**这种类只向客户端程序员暴露必须的部分而隐藏其他部分**。如果加以隐藏，客户端程序员将不能够访问它，意味着类创建者可以任意修改被隐藏部分，不用担心对其他人造成任何影响。被隐藏的部分通常代表对象内部脆弱的部分。

**访问控制原因**：

1. 让客户端程序员无法触及他们不应该触及的部分
2. 允许库设计者改变类内部的工作方式而不用担心影响到客户端程序员

**三个关键字**：**public、private、protected及默认**。

## 1.5 复用具体实现

复用某个类的方式就是直接使用该类的一个对象，也可以将那个类的一个对象置于某个新的类中。新的类可以由任意数量，任一类型的其他对象以任意可以实现新的类中想要的功能方式所组成。这种概念称之为**组合**。如果组合是动态发生的，通常被称为**聚合**。视为<font color=red>"**has-a"**</font>关系。例如汽车与引擎。
新类的成员对象通常都被声明为**private**。**组合有极大的灵活性，在建立新类时，应该首先考虑组合，它更加简单灵活，再考虑继承。**

## 1.6 继承

**继承**，即你可以以现有的类为基础，复制它，然后通过添加和修改这个副本来创建新类。当源类（基类，超类或者父类）发生变动，被修改的副本（导出类，继承类或者子类）也会反映这些问题。

类型不仅仅只是描述了一个作用于一个对象集合上的约束条件，同时还有与其他类型之间的关系。两个类型可以有相同的特性和行为，但是其中一个类型可能比另一个含有更多的特性，并且可以处理更多的消息。继承使用基类型和导出类型的概念表示这种类型之间的相似性。**一个基类型包含其所有导出类型所共享的特性和行为。**例如基类是几何类，可以在此基础上，继承出具体的几何形状——圆形，正方形，三角形等。

**当继承现有类型时，也就创造了新的类型**。这个新的类型不仅包括现有类型的所有成员，并且**复制了基类的接口**。也就是说，**所有可以发送给基类对象的消息同时也可以发送给导出类对象**。由于通过发送给类的消息的类型可知类的类型，意味着导出类与基类具有相同的类型。“即圆形也是一个几何形”。

**有两种方法可以使基类与其导出类产生差异**。

1. 直接在导出类中添加新方法。这些新方法不是基类接口的一部分。
2. 改变现有基类方法的行为，称之为**覆盖（overriding）**。

只覆盖基类的方法，这样做二者具有相同的类型和相同的接口，可以用一个导出类对象完全替代一个基类对象。这可以视为纯粹替代，通常称之为**替代原则**。这种情况下基类与导出类的关系称之为<font color=red>**is-a**</font>关系，可以说一个圆形是一个几何形状。**判断是否继承，就是要确定是否可以用is-a关系描述类之间的关系。**
如果在导出类添加了新的接口元素，也就是**扩展了接口**，这种类型仍然可以替代基类，基类无法访问新的方法，这种情况描述为<font color =red>**is-like-a**</font>关系。

## 1.7 伴随多态的可互换对象

处理类型的层次结构时，经常想把一个对象不当做它所属的特定类型来对待，而是将其当做其基类的对象来对待。这使得人们可以编写出不依赖于特定类型的代码。在“几何形例子中，方法操作都是”泛化的形状，不关心它们是圆形，正方形，三角形还是其他什么尚未定义的形状。

但是试图将导出类型的对象当作其泛化基类型对象来看待时，仍然存在一个问题。例如让泛化的交通工具行驶，泛化的鸟类飞行，编译器在编译时不知道应该执行哪一段代码：发送这样的消息，程序员并不想知道哪一段代码将被执行；绘图方法可以被等同应用于圆形，正方形，三角形，对象会依据自身具体类型来执行恰当的代码。

为了解决这个问题，面向对象程序设计语言使用了**后期绑定**的概念：当向对象发送消息时，被调用的代码直到运行时才能确定。编译器确保被调用方法的存在，并对调用的参数和返回值类型检查（无法提供此类保证的语言称为弱类型），但是并不知道将被执行的确切代码。
​	**java默认动态绑定**，不需要额外关键字来实现多态。
如果用java来编写一个方法：

```
void doSomething(Shape shape){
	shape.erase();
	shape.draw();
	//.....
}
```

这个方法可以和任何Shape对话，它是独立于任何想要绘制和擦除的对象的具体类型

```
Circle circle = new Circle();
Triangle triangle = new Triangle();
Line line = new Line();
doSomething(Circle);
doSomething(triangle);
doSomething(line);
```

doSomething自动正确的调用，不管对象的确切类型。
**将导出类看作是它的基类的过程称为向上转型**

## 1.8 单根继承

除了C++以外的所有OOP语言，所有的类最终都继承自单一的基类，这个终极基是**Object**。

单根继承结构中所有对象都具有一个共用接口，单根继承机构保证所有对象都具备某些功能，可以在每个对象上执行基本操作。单根继承结构使得回收器的实现变得容易了多。由于所有对象都保证具有其类型信息，因此不会因无法确定对象的类型而陷入僵局，对于系统级操作尤为重要。

## 1.9 容器

**如果不知道解决某个特定问题需要多少个对象，或者它们将存活多久，那么就不知道如何存储这些对象**。对于面对对象设计中大多数问题的解决方案：创建另一种对象，这种新的对象类型持有对其他对象的引用。这个被称为**容器**的新对象在任何需要时都可以 扩充自己以容纳你置于其中的所有东西。因此不需要知道将来会把多少个对象置于容器中，只需要创建一个容器对象，然后让它处理所有细节。

**虽然单一类型的容器可以满足所有操作，但是还是需要对容器有所选择，这有两个原因**：

1. 不同容器提供了不同类型的接口和外部行为。堆栈相比于队列就具备不同的接口和行为，也不同于集合和列表的接口和行为，它们之中某种容器提供的解决方案可能比其他容器灵活的多。
2. 不同容器对于某些操作具有不同的效率。

### 1.9.1 参数化类型

单根继承机构意味着所有的对象都是object类型，所以可以存储object的容器可以存储任何东西，导致容器很容易被复用。

解决这个问题可以运用到转型，不过这里是**向下转型**，转型为更为具体的类型。向上转型是安全的，circle是一种Shape类型，但是不知道某个Object是circle还是shape，除非确切知道所要处理的对象类型。

向下转型和运行时检查需要额外的程序运行时间，这里可以以参数化类型机制的解决方案：参数化类型就是一个编译器可以自动定制作用于特定类型上的类。
例如：

```
ArrayList<Shape>shapes = new ArrayList<Shape>();
```

## 1.10 对象的创建和生命周期

使用对象，最关键的问题之一就是它们的生成和销毁的方式，每个对象为了生存都需要资源，尤其是内存，不再需要一个对象时，它必须被清理掉，使其占用的资源可以被释放和重用。

java采用**动态内存分配**方式，即在称为堆的内存池中动态地创建对象。在这种方式，直到运行时才知道需要多少对象，它们的生命周期如何，以及它们的具体类型是什么、。**每当想要创建新对象时，就要使用new关键字来构建此对象的动态实例。**

在堆上创建对象，编译器对它的生命周期一无所知。**java提供了垃圾回收器机制**，它可以自动发现对象何时不再被使用，并继而销毁它。

## 1.11 异常处理

自从编程语言问世以来，错误处理就始终是最困难的问题之一。java一开始就内置了异常处理，并且你必须使用它。它是唯一可接受的错误报告方式。

异常处理将错误处理直接置于编程中。**异常是一种对象**，它从出错地点被抛出，并被专门设计用来处理特定类型错误的相应的异常处理器“捕获”。异常提供了一种从错误状态进行可靠恢复的途径。

## 1.12 并发编程

程序中，彼此独立运行的部分称之为线程，多个独立运行的部分称之为**“并发”**。并发看起来简单，但是有个隐患：共享资源。如果有多个并发任务都要访问同一项资源，那么就会出问题。例如两个进程不能同时向一台打印机发送信息，为了解决这一问题，可以共享的资源，必须在使用期间被锁定，整个过程为：某个任务锁定某项资源，完成其任务，然后释放资源锁，使其它任务可以使用这项资源。

## 1.13 Java与Internet

。。。

# 第2章 一切都是对象

## 2.1 用引用操纵对象

尽管一切都看做对象，但操纵的标识符实际上是对象的一个**“引用”（reference）**。类比为遥控器（引用）来操纵电视机（对象）。遥控与电视机保持链接。

引用可以独立存在，并不要求一定需要和对象关联。

```
String s;
```

这里创建的只是引用，不是对象，且没有初始化，没有与对象关联。

## 2.1 必须由你创建所有对象

创建了引用，可以与一个新的对象关联，通常使用**new**操作符来实现。

```
String s = new String("asdf"); 
```

### 2.2.1 存储位置

五个不同的地方存储数据：

1. **寄存器**：速度最快，处于处理器内部。其数量有限，根据需求分配，不能直接控制。
2. **堆栈**：位于通用RAM（随机访问存储器）中，通过堆栈指针从处理器那里获得直接支持。指针上移，释放内存；指针下移，分配内存。
3. **堆**：一种通用的内存池（也位于RAM区），用于存放所有的Java对象。存储分配和清理比堆栈更耗时，但无需知道存储数据的存活时间，更灵活。
4. **常量存储**：通常直接存放在程序代码内部；在嵌入式系统中可以选择将其存放在ROM中。
5. **非RAM存储**：数据存活于程序之外，可以不受程序的任何控制，程序不运行也可存在，典型例子：**流对象和持久化对象**。

### 2.2.2 特例：基本类型

程序设计中经常要用到的一系列类型，特殊对待，看做是“基本”类型，不用new来创建，而是创建一个并非是引用的“自动”变量，这个变量直接存储“值”，并置于堆栈中，因此更加高效。

![基本类型](F:\tempphoto\捕获.PNG)

**boolean**类型占据内存空间没有明确，仅能取“**true**”或“**false**”

基本类型具有的包装器类可以在堆中创建一个非基本对象，表示对应的基本类型：

```
char c = ‘x’;
Character ch = new Character(c); 
或：
Character ch = new Character(‘x’); 
```

**Java SE5后提供自动包装功能：**

```
Character ch = ‘x’; 
```

```
char c = ch; 
```

**高精度数字**：

java提供了两个用于高精度计算的类：**BigInter**和**BigDecimal**。属于包装器类型，没有对应的基本类。

**前者对应任意精度的整数；后者支持任意精度的定点数**。

### 2.2.3 java中数组

java的主要目标之一就是安全性，确保数组会被初始化。当创建一个数组对象时，即创建了一个引用数组，并且每个引用都会自动被初始化为一个特定值，该值拥有自己的关键字null。

## 2.3 永远不需销毁对象

### 2.3.1 作用域

作用域决定了其内定义的变量名的可见性和生命周期。在C，C++，java中，**作用域由花括号的位置决定**。

```
{
 int x = 12;
 // Only x available
{
 int q = 96;
 // Both x & q available
}
 // Only x available
 // q is "out of scope"
} 
```

```
{
 int x = 12;
 {
Everything Is an Object 45
 int x = 96; // Illegal
 }
}
```

### 2.3.2 对象的作用域

**java对象不具备和基本类型一样的生命周期**。new一个对象，可以存活与作用域之外。

```
{
 String s = new String("a string");
} // End of scope 
```

引用s在作用域终点就消失了，但是s指向的String对象仍继续占据内存空间。

## 2.4 创建新的数据类型：类

用关键字**“class”**来表示：

```
class ATypeName { /* Class body goes here */ } 
```

即引入了一种新类型，可以new来创建这种类型的对象了。

### 2.4.1 字段和方法

定义了一个类，可以在类中设置两种类型元素：**字段（也称数据成员）**和**方法（也称成员函数）**。

**字段**可以是任何类型的对象或基本类型的一种。字段是对的某个对象的引用，则必须初始化。示例：

```
class DataOnly {
 int i;
 double d;
 boolean b;
} 
```

```
DataOnly data = new DataOnly();
```

可以给字段赋值，具体实现为：

```
data.i = 47;
data.d = 1.1;
data.b = false; 
```

**基本成员默认值**

若**类的某个成员是基本数据类型**，即使没有进行初始化，java也会确保它获得一个默认值。

![基本成员默认值](F:\tempphoto\捕获1.PNG)

当变量作为类的成员使用，才确保给定其默认值，不适用于局部变量，例如在某个方法中定义某个变量。

## 2.5 方法，参数，返回值

Java的方法决定了一个对象能够接受什么消息。

<font color=red>**方法基本组成**</font>：名称、参数、返回值、方法体。

```
ReturnType methodName( /* Argument list */ ) {
 /* Method body */
} 
```

返回类型描述了调用方法后从方法返回的值，参数列表给出了要传给方法的信息类型和名称。

**java中的方法只能作为类的一部分创建。方法只有通过对象才能被调用，且这个对象必须能够执行这个方法的调用。**示例：

```
objectName.methodName(arg1, arg2, arg3); 
```

### 2.5.1 参数列表

方法的参数列表指定要传递给方法什么样的信息。参数列表中必须指定每个所传递对象的类型和名字。Java中任何传递对象的场合一样，**传递的实际上是引用**，并且引用的类型必须正确。

```
int storage(String s) {
 return s.length() * 2;
} 
```

return：首先，它代表已经做完了，离开方法；其次，如果方法产生了一个值，这个值要放在return语句后面。

可以定义方法的返回值，如果不想有返回值，可以设置为void。

## 2.6 构建一个Java程序

### 2.6.1 名字可见性

java为了一个类库生成不会与其他名字混淆名字，Java设计者希望程序员反过来使用自己的Internet域名。

### 2.6.2 运用其他构件

在程序中使用预先定义好的类，可以使用关键字：**import**

import指示编译器导入一个包，就是类库。示例：

```
import java.util.ArrayList; 
```

如果util包含了众多的类，想使用其中的几个又不想逐一声明，则可以使用通配符“*****”。

```
import java.util.*; 
```

### 2.6.3 static关键字

创建类，即描述类的对象的外观与行为。执行new才获得对象，分配数据存储空间。则**存在两种情况无解**：

1. 只想为某特定域分配单一存储空间，不考虑创建多少对象。
2. 即使没有创建对象，也能调用这个方法

**static**关键字可以解决这两个问题。声明一个事物是static，这个域或方法不会与包含它的那个类的任何对象实例关联在一起。即使从未创建某个类的任何对象，也可以调用其static方法或访问其static域。（又称类方法和类数据）

**示例**：

```
class StaticTest {
 static int i = 47;
} 
```

```
StaticTest st1 = new StaticTest();
StaticTest st2 = new StaticTest(); 
```

无论创建多少个对象，StaticTest.i也只有一份存储空间，则st1和st2指向同一份存储空间。

**引用static变量的两种方法**：如上创建对象引用他；通过类名直接引用。

```
StaticTest.i++; 
```

类似逻辑也可以应用到方法上。

## 2.7 注释与嵌入式文档

多行注释：

```
/* This is a comment
 * that continues
 * across lines
 */ 
```

单行注释：

```
// This is a one-line comment 
```

### 2.7.1 注释文档

javadoc便是用于提取注释的工具，它是JDK安装的一部分。

### 2.7.2 语法

```
所有的javadoc命令都只能在“/**”注释中出现，于“/*”结束。
```

**javadoc的两种方式**：

1. 嵌入HTML
2. 使用“文档标签”

独立文档标签是以“**@**”字符开头的命令，且要置于注释行的最前面

共有三种类型的注释文档，分别对应于注释位置后面的三种元素：类、域和方法。

示例：

```
//: object/Documentation1.java
/** A class comment */
public class Documentation1 {
 /** A field comment */
 public int i;
 /** A method comment */
 public void f() {}
} ///:~ 
```

javadoc只能为public和protected成员进行文档注释。private和包内可访问成员的注释会被忽略掉。

### 2.7.3 标签示例

常见的几种可用于代码文档的javadoc标签

- @see：引用其他类

  @see标签允许用户引用其他类的文档。

- {@link package.class#member label} 

  与前述相似，用于行内。用“label”作为超链接文本而不用“See Also”

- {@docRoot}

  该标签产生到文档根目录的相对路径，用于文档树页面的显式超链接。

- **{@inheritDoc}** 

  该标签从当前这个类的最直接的基类中继承相关文档到当前的文档注释中。

- **@version** 

  ```
  @version version-information 
  ```

  version-information可以使你认为适合包含在版本说明中的重要信息。

- @author 

  ```
  @author author-information 
  ```

  可以使用多个标签列出全部的作者，但是它们必须连续放置。

- **@return** 

  ```
  @return description 
  ```

  用来描述返回值的含义，可以延续数行。

- **@deprecated** 

  指出一些旧特性已经由改进的新特性所取代，建议用户不要再使用这些旧特性，不就得将来它们很可能会被删除。

- **@throws** 

  异常

  ```
  @throws fully-qualified-class-name description 
  ```

示例：

```
//: object/HelloDate.java
import java.util.*;
/** The first Thinking in Java example program.
 * Displays a string and today’s date.
 * @author Bruce Eckel
 * @author www.MindView.net
 * @version 4.0
*/
public class HelloDate {
 /** Entry point to class & application.
 * @param args array of string arguments
 * @throws exceptions No exceptions thrown
 */
 public static void main(String[] args) {
 System.out.println("Hello, it’s: ");
 System.out.println(new Date());
 }
} /* Output: (55% match)
Hello, it’s:
Wed Oct 05 14:39:36 MDT 2005
*///:~ 
```

# 第3章 操作符

Java其实是建立在C++的基础之上的，很多语法之类都很类似。关于操作符也基本类似

## 3.1更简单的打印语句
java的打印语句示例：

```
System.out.println("a lot of type");
​```。
```

## 3.2使用java操作符
操作符接受一个或多个参数，并生成一个新值。其中**+,-,*,/,=**的用法与其他编程语言类似。关于“=”“==”“！=”这些操作符，它们可以操作所有对象。除此以外，<font color =red>**String类支持“+”，“+=”**</font>

## 3.3优先级
一个表达式中存在多个操作符，操作符的优先级就决定了各部分的计算顺序。java中的各个操作符的优先级如下：

```
.    ()    {}    ;    ,
++    --    ~    !(data type)
*    /    %
+    -
<<    >>    >>>
<    >    <=    >=    instanceof
==    !=
&
^
|
&&
||
?    :
=    *=     /=    %=
+=    -=    <<=    >>=
>>>=    &=    ^=    |=

```

## 3.4赋值运算符

赋值使用操作符“=”，即取右边的值，复制给左边。右值可以是任何常数，变量或者表达式。左值必须是一个明确的，已命名的变量。但是常数不可以为左值。
对基本类型的变量很简单，例如：

```
int a,b = 4;
a=b;
```
将b的值复制后给a，修改b的值不会影响到a的值。

**在为对象赋值的时候，如果对一个对象进行操作，<font color=red>我们操作的是对对象的引用</font>。**倘若将一个对象赋值给了另一个对象，实际上就是将引用从一个定法复制到另一个地方，即若将对象c赋值给对象d，则对象d修改，对象c也将修改。这里可以将引用看作家的地址，你把c的家的地址赋给了d，则d顺着地址找到的家和c是同一个家，则c中的家有什么变动，同样的d也会变动。
**浮点型的默认数据类型是double**，如果给float数据类型赋值，应为：

```
float i = 0.5f;
```

## 3.5算术操作符

java的算术运算符基本上与其他编程语言类似，其中包括加号（+），减号（-），除号（/）,乘号（`*`）以及取模操作符（%，即求余数），整数除法直接去掉结果的小数位。关于算术操作符，要注意+=、-=、*=、/=、%=这些符号，隐含了强制转换，强制转换为左右的数据类型。例如：

```
int i = 1;
i*=0.1;
Sysout.out.println(i);
```
上述代码输出结果为0，i*0.1之后强制转换为int型。
**对于字符串与基本类型的操作也要注意，例如：**

```
System.out .println(3+4+“Hello!”);      //输出：7Hello!
System.out.println(“Hello!”+3+4);      //输出：Hello!34
System.out.println(‘a’+1+“Hello!”);    //输出：98Hello!
```

### 3.5.1一元加、减操作符
一元减号（-）与加号（+）与二元的一样，编译器可以自动识别使用的是哪一种。例如

```
x=-a;
//或者如下
x=a*-b;
```
不过为了程序的可读性，一般写作

```
x=a*(-b);
```

## 3.6自动递增和递减操作
自动递增和递减操作符号为“++”、“--”。例如a是int型，则++a等价于（a=a+1）;关于递增和递减有前缀和后缀式两种表达式，位于前缀表达先进行加减值，再进行其他运算，后缀式与其相反。例如

```
import static net.mindview.util.Print.*;
public class AutoInc {
 public static void main(String[] args) {
 int i = 1;
 print("i : " + i);
 print("++i : " + ++i); // Pre-increment
 print("i++ : " + i++); // Post-increment
 print("i : " + i);
 print("--i : " + --i); // Pre-decrement
 print("i-- : " + i--); // Post-decrement
 print("i : " + i);
 }
} /* Output:
i : 1
++i : 2
i++ : 2
i : 3
--i : 2
i-- : 2
i : 1
*///:~ 
```

## 3.7关系操作符

关系操作符生成的是一个boolean结果，计算的是操作数之间的关系。如果关系真实，生成true，否则生成false。关系操作符包括：小于（<）、大于（>）、小于或等于（<=）、大于或等于（>=）、等于（==）以及不等于（！=）。**等于与不等于适用于所有类型**，其他比较符不适用于**boolean**。

## 3.8 逻辑操作符

逻辑操作符“与”（**&&**）、“或”（**||**）、“非”（**！**）根据参数的逻辑关系，生成一个布尔值。**逻辑操作符只能用于boolean，不可将一个非布尔值当做布尔值在逻辑表达式中使用**。如果在使用String值得地方使用了布尔值，布尔值会自动转换为文本形式。

### 3.8.1 短路

使用逻辑操作符，会出现一种短路情况。例如如果使用&&

```
if(a>10&&b<10){
    a=b;
};
```

如果a>10表达式是false，则整个if表达式肯定为false，则&&后的表达式被短路了，即b<10不会参与运算。同理如果是（||），则一个表达式为true，则其后的表达式都被短路了，不会参与运算。**这种情况有助于减少某些参数的运算错误。**例如在&&的表达式，只有前一个表达式为true，后一个表达式才可能下标不越界等等，如果前一个表达式为false，再运算后一个表达式，可能出现下标越界错误等

## 3.9直接常量

直接常量的后缀标明了其类型，如果是16进制数据，以**ox（0X）**开头，如果是八进制，则以**0**开头。对于输出数据类型，可以使用包装类的方法来确定要输出的进制数。例如：

```
int i = 15;
System.out.println(Integer.toHexString(i));//16进制数输出
System.out.println(Integer.toBinaryString(i));//2进制数输出
System.out.println(Integer.toOctalString(i));//8进制数输出1234
```

关于浮点型，java中还提供了一种指数的表达式，例如**1.39∗10−12， 1.39∗10−12**

```
float a = 1.39e-12f;
double b = 1.39E-12;
```

## 3.10 按位操作符

按位操作符用来操作整数基本数据类型中的单个比特，即二进制。按位操作符中有：按位与“**&**”，按位或“**|**”，按位异或“**^**”，按位非“**~**”以及与“=”联合操作的“&=”等。**（~是一元操作）**。  按位操作符与逻辑操作符有相同的效果，但是不会存在中途短路的情况。

示例：

```
int i = 5;//101
int b = 4;//100
int c = i&b;//100，即c=4
c = i|b;//101
c=i^b;//001即c=1
c=~i;//010
```

## 3.11 移位操作符

移位操作符的运算对象是二进制的位，**只可用来处理整数类型。**左移位操作符（<<）将操作数向左边移动一位（低位自动补0）。“有符号”右移位操作符（>>）将操作数向右移动。符号为正，高位插0，符号为负，高位插1；**java中增加了无符号右移位操作符（>>>）,**无论正负，高位插0。（C/C++没有）。<font color=red>**如果对char，byte，short类型进行移位处理，在移位之前要先转换为int类型。**</font>

移位可以与等号组合使用（**<<=,>>=,>>>=**），<font color=red>**如果对byte或者short进行这样的移位，可能得到的结果不是正确的结果**</font>。因为它要被转换为int类型，再移位，再截位再转换数据类型。

## 3.12 三元操作符

三元操作符称作为条件操作符，它的表达式形式为：

`  boolean-exp?value0:value1 `，如果boolean-exp（布尔表达式）为true，计算value0，否则计算value1。

示例：

```
int i=12;
int b = i>10?i:10;//b=12
```

## 3.13 字符串操作符

字符串操作符是用来连接不同的字符串。即+与+=。 如果表达式一一个字符串起头，那么后续所有操作数必须是字符串型。
例如：

```
String s="adsf";
String b="jkl";
String sb=s+b;//"adsfjkl"
sb+=s;//"adsfjkladsf"
```

## 3.14 类型转换操作符

**类型转换（cast）意味着模型重铸**，适当的时候，java会自动将一种数据转换为另一种数据类型，例如为浮点数赋予整数值，java会自动将整数值转换为浮点型。类型转换运算允许我们显示的进行这种类型转换。例如：

```
int i = 400;
long l = (long)i;//400
byte b = (byte)i;//-112123
```

**大容量数据类型转换为小容量数据类型可能就会出现数据错误**，而且浮点数数据类型转换为整数型数据类型是**将小数点后的数舍去掉**，如果想要实现四舍五入，可以使用java.lang.Math中的round()方法。 

示例：

```
float f = -29.5f;
System.out.println(Math.round(f));//输出-29
float ff = -29.51f;
System.out.println(Math.round(ff));//输出-301234
```

不同数据类型的数据进行运算，要格外注意.Java允许把任何基本数据类型转换成别的数据类型，但布尔型除外。**“类”数据类型不允许进行类型转换**。

## 3.15 操作符小结

```

```

# 第4章 控制执行流程

java使用了c所有的控制流程语句，涉及到的关键字有：**if-else、while、do-while、for、return、break以及选择语句break**.java并不支持goto语句。

## 4.1 true 和false 

条件语句都利用条件表达式的真或假来决定执行路径。

## 4.2 if-else

其中else部分是可选的。示例：

```
if(boolean-exp){
    statement;
}123
```

或者

```
if(boolean-exp){
    statement;
}else{
    statement;
}12345
```

亦或者

```
if(boolean-exp){
    statement;
}else if(boolean-exp){
    statement;
}else if(boolean-exp){
    statement;
}......
```

## 4.3 迭代

while、do-while、for用来控制循环，语句会重复执行，直到控制作用的布尔表达式得到假的结果位置。 
**while的循环表达式格式**：

```
while(boolean-exp){
    statement;
}123
```

循环刚开始时会计算一次布尔表达式；在语句的下一次迭代开始前再计算一次。

### 4.3.1do-while

**do-while的格式**：

```
do{
    statement;
}while(boolean-exp);123
```

与while循环语句的差别是多执行一次。

### 4.3.2 for

for循环可能是最长用到的迭代形式，这种迭代在第一次要进行初始化，随后进行条件测试，在每次迭代结束前进行某种形式的步进，其格式为：

```
for(initialization;boolean-exp;step){
    statement;
}123
```

具体实例如下，求1到10的整数和

```
int res = 0;
for(int i = 1;i<=10;i++){
    res+=i;
}
System.out.println(res);12345
```

其中**i的作用范围仅在for循环的范围内**。

### 4.3.3 逗号操作符

java中唯一用到逗号操作符的就是for循环的控制表达式。**在控制表达式的初始化和步进控制部分，可以使用一系列的逗号分隔语句。**

```
for(int i = 1,j=i+10;i<5;i++,j=i*2){
    System.out.pringln(j);
}123
```

**定义多个变量的这种能力只限于for循环使用**。

## 4.4 foreach语法

**Java SE5引入了更简洁的for语法用于数组和容器。**

示例遍历一个float数组：

```
public class ForEachFolat{
    public static void main(String[]args){
        Random rand = new Random(47);
        float[]f=new float[10];
        for(int i=0;i<10;i++){
            f[i] = rand.nextFloat();
        }
        for(float x:f){
            System.out.println(x);
        }
    }
}123456789101112
```

即定义了一个float类型的变量x，将数组f的每一个元素值赋予x，对x进行操作。 
从上述例句可以看到foreach的格式为：

```
for(datatype tempName:Name){
    statement;
}
```

<font color=red>**使用foreach，要事先存在该数组或对象才可以**。</font>

## 4.5 return

return的两个用途：

1. 指定一个方法返回什么值
2. 导致当前方法退出，并返回那个值

如果一个方法不是void，一定要有return语句，确保有返回。

## 4.6 break和continue

在任何迭代语句的主体部分都可以使用break和continue，其中**使用break强行退出循环**，不执行循环中的剩余语句，而**continue则是停止执行当前的迭代，退回到循环起始处**，开始下一次迭代。

```
//: control/BreakAndContinue.java
// Demonstrates break and continue keywords.
import static net.mindview.util.Range.*;

public class BreakAndContinue {
  public static void main(String[] args) {
    for(int i = 0; i < 100; i++) {
      if(i == 74) break; // Out of for loop
      if(i % 9 != 0) continue; // Next iteration
      System.out.print(i + " ");
    }
    System.out.println();
    // Using foreach:
    for(int i : range(100)) {
      if(i == 74) break; // Out of for loop
      if(i % 9 != 0) continue; // Next iteration
      System.out.print(i + " ");
    }
    System.out.println();
    int i = 0;
    // An "infinite loop":
    while(true) {
      i++;
      int j = i * 27;
      if(j == 1269) break; // Out of loop
      if(i % 10 != 0) continue; // Top of loop
      System.out.print(i + " ");
    }
  }
} /* Output:
0 9 18 27 36 45 54 63 72
0 9 18 27 36 45 54 63 72
10 20 30 40
*///:~12345678910111213141516171819202122232425262728293031323334
```

## 4.7 goto

goto是java的一个保留字，在语言中并未使用它；java没有使用goto，但是也可以完成goto的功能，使用了标签的机制。 
标签是后面带有冒号的标识符，例如：

```
label1：
```

**利用break或continue与标签配合，可以中断循环，直到标签的所在位置。** 
例如：

```
label1:
outer-iteration{
    inner-iteration{
        //....
        break;//(1)
        //.....
        continue;//(2)
        //....
        continue label1;//(3)
        //....
        break label1;//(4)
    }
}
```

在（1）中break中断内部迭代，回到外部迭代。在（2）中，continue使执行点移到内部迭代的起始点。在（3）处，**continue label1**同时中断内部迭代和外部迭代，**直接转到label1处；随后它继续迭代**，不过是从外部迭代开始，相当于等价一个break；在（4），**break label1**中断了所有迭代，并回到了label1，**但不重新进入迭代**，即完全中断了两个迭代。

```
//: control/LabeledFor.java
// For loops with "labeled break" and "labeled continue."
import static net.mindview.util.Print.*;

public class LabeledFor {
  public static void main(String[] args) {
    int i = 0;
    outer: // Can't have statements here
    for(; true ;) { // infinite loop
      inner: // Can't have statements here
      for(; i < 10; i++) {
        print("i = " + i);
        if(i == 2) {
          print("continue");
          continue;
        }
        if(i == 3) {
          print("break");
          i++; // Otherwise i never
               // gets incremented.
          break;
        }
        if(i == 7) {
          print("continue outer");
          i++; // Otherwise i never
               // gets incremented.
          continue outer;
        }
        if(i == 8) {
          print("break outer");
          break outer;
        }
        for(int k = 0; k < 5; k++) {
          if(k == 3) {
            print("continue inner");
            continue inner;
          }
        }
      }
    }
    // Can't break or continue to labels here
  }
} /* Output:
i = 0
continue inner
i = 1
continue inner
i = 2
continue
i = 3
break
i = 4
continue inner
i = 5
continue inner
i = 6
continue inner
i = 7
continue outer
i = 8
break outer
*///:~1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162
```

**如果想在中断循环的同时退出方法，可以使用一个return语句即可。** 
这些规则同样适用于while。

## 4.8 switch

switch被归划为一种选择语句，根据整数表达式，switch从一系列代码中选出一段去执行。格式为：

```
switch(变量){
    case 常量1：
        语句；
        break；
    case 常量2：
        语句；
        break；
    ...
    default：
        语句；
        break；
}123456789101112
```

每个case都以一个break结尾，可以使执行流程跳转到switch主体末尾，这个是可选的，如果省略了break，则继续执行下面的case语句。如果case中没有相符合的语句，则执行default语句。 
**switch（表达式）中的返回值必须是：byte，short，char，int，枚举，String，没有long类型**；case子句中的值必须是**常量**，且所有case值应该是不同的。 

**switch语句的执行效率比if稍微高一些**，具体数值不多的时候建议使用switch语句。

# 第5章 初始化与清理

**面向对象(OOP)与面向过程** 
二者都是一种思想，面向对象是相对于面向过程而言的。面向过程，强调的是功能行为。面向对象，将功能封装进对象，强调具备了功能的对象。 
面向对象更加强调运用人类在日常的思维逻辑中采用的思想方法与原则，如抽象、分类、继承、聚合、多态等。 
**面向对象的三大特征** 
**封装** (Encapsulation) 
**继承** (Inheritance) 
**多态** (Polymorphism)

**类与类之间的关系：** 
关联，继承，聚集，组合。 
类是对一类事物描述，是抽象的、概念上的定义，好比汽车的图纸 
对象是实际存在的该类事物的每个个体，因而也称实例(instance)，好比一辆辆具体的汽车 
面向对象程序设计的重点是类的设计 
定义类其实是定义类中的成员(成员变量和成员方法)

## 5.1 用构造器确保初始化

**为每个类都定义一个初始化方法，该方法提醒在使用其对象之前要调用该方法**。在java中，通过提供构造器，类的设计者可以确保每个对象都可以得到初始化。**关于这个初始化方法的命名，存在两个问题：**所取得任何名字都可能与类的某个成员名称相冲突；调用构造器是编译器的责任，必须让编译器知道应该调用哪个方法。所以采用了：<font color=red>**构造器采用和类相同的名称。**</font>

```
class Rock{
	Rock(){
	}
}
```

这样创建对象的时候，将会为对象分配存储空间，并调用相应的构造器，确保你在操作对象之前，它已经被初始化了
没有任何参数的构造器叫做默认构造器，即“无参构造器”。设计类的时候，**不显示声明类的构造器的话，程序会默认提供一个无参构造器；**一旦显示定义了类的构造器，那么默认的构造器就不再提供。其标准语法格式为：

```
修饰符 类名(参数列表){
	初始化语句；
}
```

**构造器的相关特征**：
	它具有与类相同的名称
	它不声明返回值类型。（与声明为void不同）
	**不能被<font color =red>static、final、synchronized、abstract、native</font>修饰，不能有return语句返回值**

根据参数不同，**构造器可以分为如下两类：**
隐式无参构造器（系统默认提供）
显式定义一个或多个构造器（无参、有参）
**注  意：**
Java语言中，每个类都至少有一个构造器
默认构造器的修饰符与所属类的修饰符一致
一旦显式定义了构造器，则系统不再提供默认构造器
一个类可以创建多个重载的构造器
**父类的构造器不可被子类继承**
<font color=red>**类中属性赋值的先后顺序：**</font>1.属性的默认初始化2数据的显示赋值3.通过构造器给属性初始化4.创建对象后，通过对象。方法的形式给属性赋值

## 5.2 方法重载

方法区:含字符串常量
静态域：声明为static的变量
类的方法：提供某种功能的实现

1)	实例：

```
public void eat(){}
public String getName(){}
public void setName(String n){}
```
格式： 权限修饰符 返回值类型（void:无返回值） 方法名（形参）{}

2）关于返回值类型：void：此方法不需要返回值
​					有返回值的方法:在方法的最后有return 语句。
3）方法内可以调用本类的其他方法或者属性，但是**不能在方法内再定义方法。**

当创建一个对象，也就给此对象分配到的存储空间取了一个名字。所谓方法则是给某个动作取得名字。通过使用名字，你可以引用所有的对象和方法。
**相同的词表达不同的含义——即重载**。在java中，方法的重载的格式等规定的一大原因是构造器造成的。构造器的名字由类名所决定，只能有一个构造器名，如果想要用多种方式创建一个对象怎么办？这个时候可以**使用方法名相同但是参数列表不同**的构造器来实现，即实现了方法的重载。

示例：

```
class Rock{
	int i;
	Rock(){
	}
	Rock(int value){
		this.i = value;
	}
	Rock(int a,int b){
		//.....
	}
	//.....
}
```
<font color=red>**方法重载的特点：**</font>
与返回值类型无关，只看参数列表，且参数列表必须不同。(**参数个数或参数类型以及参数的前后顺序**)。

**方法重载时候，要注意传入的实际参数和方法中的形参的比较。**如果实参小于形参，实参会被提升。如果实参大于形参，没有进行类型转换，则出现异常。例如将`void find(int i)`改为`void find(byte i)`。

## 5.3 this关键字

如果有同一类型的两个对象，分别为a，b。如何才能让这两个对象都能调用同一个方法？如何知道该方法是被哪一个对象调用？为此有一个专门的关键字：**this**。<font color=red>**this关键字只能在方法的内部使用**</font>，表示对“调用方法的那个对象”的引用。this在构造器中使用，表示该构造器正在初始化的那个对象。

**this表示当前对象或者当前正在创建的对象，可以调用类的属性，方法和构造器。**
**那么什么时候使用this关键字？**

1. 当在方法内需要用到调用该方法的对象时，就用this
 2. 当形参与成员变量重名的时候，如果方法内部需要使用成员变量，必须添加this来表明该变量是类成员
 3. 任意方法内，如果使用当前类的成员变量或者成员方法可以在其前面添加this，增加可读性。

```
public class Person{
	int age;
	String name;
	public Person(int age,String name){
		this.age = age;
		this.name = name;
	}
}
```

### 5.3.1 在构造器中调用构造器

通常this都是代表“这个对象”或者“当前对象”，本身表示对当前对象的引用。在构造器中，如果为this添加了参数列表，就有了不同的含义，产生了对符合此参数列表的构造器的调用，这样，调用其他构造器就有了直接的途径。

```
class Person{		// 定义Person类
	private String name ;		
	private int age ;			
	public Person(){	  // 无参构造
		System.out.println("新对象实例化") ;
	}
	public Person(String name){
		this();      // 调用本类中的无参构造方法
		this.name = name ;	
	}
	public Person(String name,int age){	
		this(name) ;  // 调用有一个参数的构造方法
		this.age = age;
	}
	public String getInfo(){	
		return "姓名：" + name + "，年龄：" + age ;
	}  
}

```

<font color = red>注意事项：</font>

  1. 使用this()必须放在构造器的首行！
  2. 使用this调用本类中其他的构造器，保证至少有一个构造器是不用this的。

### 5.3.2 static

static方法就是没有this的方法。在static方法内部是不能调用非静态方法，反过来可以。而且在没有任何对象的前提下，仅仅通过类本身来调用static方法。这是因为非静态方法在调用前要先创建该方法所属类的对象，而static则不用，static方法调用非静态方法时可能还未创建对象，所以不能通过static方法来调用非静态方法。

## 5.4 清理：终结处理和垃圾回收

java有垃圾收集器负责回收无用对象占据内存资源。但也有特殊情况：假定你的对象（**并非使用new**）获得了一块特殊的内存区域，由于垃圾回收器只知道释放那些经由new分配的内存，所以它不知道该如何释放该对象的这块特殊内存。**为了应对这种情况，java允许在类中定义一个名为finalize()方法。它的工作原理：**一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用器finalize()方法，并且在下一次垃圾回收动作发生时才会真正回收对象占用的内存。可以用finalize()方法，在垃圾回收时刻做一些重要的清理工作。

### 5.4.1 finalize()用途
finalize()不是作为通用的清理方法，**关于finalize()方法的真正用途需牢记：垃圾回收只和内存有关。**即使用垃圾回收器的唯一原因是回收程序不再使用的内存，对于垃圾回收相关的任何行为来说（尤其是finalize()方法），它们也必须同内存及回收有关。无论对象是如何创建的，垃圾回收器都会负责释放对象占据的所有内存，则这就将**finalize()方法的需求限制到了一种特殊情况，即通过某种创建对象方式以外的方式为对象分配了存储空间。**

**这种特殊情况是怎么来的？**之所以要有fianlize()方法，是由于在分配内存时可能采用了类似C语言中的做法，而非java中的通常做法。这种情况主要发生在使用“本地方法”的情况下，本地方法是一种在java中调用非java代码的方式。本地方法目前只支持C和C++，但他们可以调用其他语言写的代码，所以实际上可以调用任何代码。在非java代码中，也许会调用C的malloc()函数，系列在分配存储空间，而且除非调用了free()函数，否则存储空间将得不到释放，从而造成内存泄露。而free()函数是C和C++中的函数，所以需要在finalize()中用本地方法调用它。

### 5.4.2 终结条件

当对某个对象不再感兴趣——也就是可以被清理。这个对象应该处于某种状态，使它占用的内存可以被安全地释放。例如，要是对象代表了一个打开的文件，在对象被回收前程序员应该关闭这个文件，只要对对象中存在没有被适当清理的部分，程序就存在隐晦的缺陷。finalize()可以用来最终发现这个情况。

示例：

```
//: initialization/TerminationCondition.java
// Using finalize() to detect an object that
// hasn't been properly cleaned up.

class Book {
  boolean checkedOut = false;
  Book(boolean checkOut) {
    checkedOut = checkOut;
  }
  void checkIn() {
    checkedOut = false;
  }
  protected void finalize() {
    if(checkedOut)
      System.out.println("Error: checked out");
    // Normally, you'll also do this:
    // super.finalize(); // Call the base-class version
  }
}

public class TerminationCondition {
  public static void main(String[] args) {
    Book novel = new Book(true);
    // Proper cleanup:
    novel.checkIn();
    // Drop the reference, forget to clean up:
    new Book(true);
    // Force garbage collection & finalization:
    System.gc();
  }
} /* Output:
Error: checked out
*///:~

```
本例的终结条件是：所有book对象被当作垃圾回收前都应该签入（check in）。但在main方法中，由于程序员的错误，有一本书未被签入。要是没有finalize()方法来验证终结条件，很难发现这种错误。

`System.gc()`与`System.runFinalization()`不能确保`finalize()`方法一定被执行，没有办法保证`finalize()`会被调用

## 5.5 类的成员变量

属 性：对应类中的成员变量
行 为：对应类中的成员方法
Field = 属性 = 成员变量，Method =  (成员)方法 = 函数
**面向对象思想的落地法则：**

设计类，并设计类的成员（成员变量与成员方法）
通过类，来创建类的对象（类的实例化）
通过“对象.属性”或“对象.方法”来调用，完成相应的功能
<font color =red>**类及类的构成成分：属性，方法，构造器，代码块，内部类**</font>
**类的属性（成员变量）**
**成员变量vs局部变量**
都遵循变量声明的格式，都有作用域
**不同**：
1.声明的位置不一样，：成员变量声明在类里，方法外
​				局部变量：声明在方法里，方法的形参部分，代码块内。
2.成员变量的修饰符有四个:public,private,protected 缺省
局部变量没有修饰符，与所在的方法修饰符一致。
3.初始化值：一定会有初始化值。
成员变量:如果在声明的时候，不显示的赋值，那么不同的数据类型会有默认的初始化值 int, short , byte$\to$0 , float, double$\to$0.0  char$\to$空格  Boolean$\to$false ,string$\to$null
局部变量一定要显示赋值，（没有默认的初始化值,除了形参以外）
4.二者在内存中存放的位置：成员变量存在于堆空间中，局部变量在栈空间中。

## 5.6 成员初始化

**java尽力保证**：所有变量在使用之前都能够得到恰当的初始化。在使用变量时，注意判断变量是成员变量还是局部变量。如果是局部变量，在使用的时候没有显示的赋予初始值，会出错。

### 5.6.1 指定初始化
如果想为某个变量赋值，直接的方法就是在定义变量的地方为其赋值，或者用构造器与方法初始化非基本类型的对象，或者调用方法来初始化某些变量。（**关于变量的初始化要注意初始化程序的顺序**）

```
//: initialization/MethodInit2.java
public class MethodInit2 {
  int i = f();
  int j = g(i);
  int f() { return 11; }
  int g(int n) { return n * 10; }
} ///:~

```

## 5.7 构造器初始化

可以用构造器进行初始化。在运行时刻，可以调用方法或执行某些动作来确定初值，但要牢记：无法阻止自动初始化的进行，他将在构造器被调用前发生。例如：

```
public class Counter{
	int i;
	Counter(){i=7}
	//.....
}
```

这里i首先会被置0，然后变成7.对于所有的基本类型和对象的引用，包括在定义时已经指定初值的变量，这种情况都成立。

<font color=red>**关于变量的初始化顺序如前面所述**</font>：1.属性的默认初始化2数据的显示赋值3.通过构造器给属性初始化4.创建对象后，通过对象.方法的形式给属性赋值

### 5.7.1 静态数据初始化

无论创建多少个对象，静态数据都只占一份存储内存区域。**static关键字不能应用于局部变量，**因此它只能作用与域。如果一个域是静态的，且没有对它进行初始化，那么它就会获得基本类型的标准初值；如果它是一个对象引用，那么它的默认初始值就是null。

例如：

```
//: initialization/StaticInitialization.java
// Specifying initial values in a class definition.
import static net.mindview.util.Print.*;

class Bowl {
  Bowl(int marker) {
    print("Bowl(" + marker + ")");
  }
  void f1(int marker) {
    print("f1(" + marker + ")");
  }
}

class Table {
  static Bowl bowl1 = new Bowl(1);
  Table() {
    print("Table()");
    bowl2.f1(1);
  }
  void f2(int marker) {
    print("f2(" + marker + ")");
  }
  static Bowl bowl2 = new Bowl(2);
}

class Cupboard {
  Bowl bowl3 = new Bowl(3);
  static Bowl bowl4 = new Bowl(4);
  Cupboard() {
    print("Cupboard()");
    bowl4.f1(2);
  }
  void f3(int marker) {
    print("f3(" + marker + ")");
  }
  static Bowl bowl5 = new Bowl(5);
}

public class StaticInitialization {
  public static void main(String[] args) {
    print("Creating new Cupboard() in main");
    new Cupboard();
    print("Creating new Cupboard() in main");
    new Cupboard();
    table.f2(1);
    cupboard.f3(1);
  }
  static Table table = new Table();
  static Cupboard cupboard = new Cupboard();
} /* Output:
Bowl(1)
Bowl(2)
Table()
f1(1)
Bowl(4)
Bowl(5)
Bowl(3)
Cupboard()
f1(2)
Creating new Cupboard() in main
Bowl(3)
Cupboard()
f1(2)
Creating new Cupboard() in main
Bowl(3)
Cupboard()
f1(2)
f2(1)
f3(1)
*///:~

```
由输出可见，静态初始化只有在必要时刻才会进行。如果不创建table对象，也不引用table.b1或者table.b2，那么静态的Bowl b1和b2永远都不会被创建。
初始化顺序是先初始化静态对象，后为非静态对象。

### 5.7.3 显示的静态初始化

java允许将多个静态初始化动作组织成一个特殊的“静态子句”（静态块），例如：

```
public class Spoon{
	static int i;
	static{
		i = 47;
	}
}
```
**即一段跟在static关键字后面的代码，与其他静态初始化动作一样，这段代码仅执行一次：首次生成这个类的一个对象时，首次访问属于那个类的静态数据成员时：**

<font color=red>**示例**</font>：

```
//: initialization/ExplicitStatic.java
// Explicit static initialization with the "static" clause.
import static net.mindview.util.Print.*;
class Cup {
Cup(int marker) {
print("Cup(" + marker + ")");
}
void f(int marker) {
print("f(" + marker + ")");
}
}
class Cups {
static Cup cup1;
static Cup cup2;
static {
cup1 = new Cup(1);
cup2 = new Cup(2);
}
Cups() {
print("Cups()");
}
}
public class ExplicitStatic {
public static void main(String[] args) {
print("Inside main()");
Cups.cup1.f(99); // (1)
}
// static Cups cups1 = new Cups(); // (2)
// static Cups cups2 = new Cups(); // (2)
} /* Output:
Inside main()
Cup(1)
Cup(2)
f(99)
*///:~
```



### 5.7.4 非静态实例初始化

示例：

```
//: initialization/Mugs.java
// Java "Instance Initialization."
import static net.mindview.util.Print.*;

class Mug {
  Mug(int marker) {
    print("Mug(" + marker + ")");
  }
  void f(int marker) {
    print("f(" + marker + ")");
  }
}

public class Mugs {
  Mug mug1;
  Mug mug2;
  {
    mug1 = new Mug(1);
    mug2 = new Mug(2);
    print("mug1 & mug2 initialized");
  }
  Mugs() {
    print("Mugs()");
  }
  Mugs(int i) {
    print("Mugs(int)");
  }
  public static void main(String[] args) {
    print("Inside main()");
    new Mugs();
    print("new Mugs() completed");
    new Mugs(1);
    print("new Mugs(1) completed");
  }
} /* Output:
Inside main()
Mug(1)
Mug(2)
mug1 & mug2 initialized
Mugs()
new Mugs() completed
Mug(1)
Mug(2)
mug1 & mug2 initialized
Mugs(int)
new Mugs(1) completed
*///:~

```

## 5.8 数组初始化

数组是相同类型，用一个标识符名称封装到一起的一个对象序列或基本类型数据序列。数组是通过[]下标操作符来定义和使用的。定义一个数组：
`int []a1`或者`int a1[]`
两种格式都可以。

上述方式只是定义了数组的引用，并没有给数组对象本身分配任何空间。为了给数组创建相应的存储空间，必须写初始化表达式。关于数组的初始化可以有如下几种方式：
1.**直接赋值**（在已经数组的具体元素时）
`int []a1={1,2,3,4,5};`
2.**静态初始化**（分配空间和赋值同时进行）
**为数组分配空间**
**数组变量=new 数据类型[长度];**
静态初始化：`int a[] =new int[]{3,9,5};`
3.动态初始化（只为数组申请分配空间）
`int []a=new int[5];`
如果不确定数组中的具体元素，可以使用第三种方式初始化。**为数组分配好内存空间，每个数组元素都会有自己的默认初始值，int是0，boolean类型是false等等，与成员变量相同。**

前述的是一维数组，如果是二维数组或不规则二维数组呢。
**二维数组**
声明与申请存储空间：

```
int mat[][];
mat = new int[2][3];
Or      int mat[][]=new int[2][3];
Or 直接赋值 int mat[][]={{1,2,3},{4,5,6}};
```
Mat.length//返回二维数组的长度，即二维数组的行数
Mat[0].length//返回一维数组的长度，即二维数组的列数
**<font color=red>数组一旦初始化，长度是不可变的。</font>**
**不规则的二维数组。**
int mat[][];
mat=new int[2][];		//申请第一维的存储空间，即数组的行数
mat[0]=new int[2];		//申请第二维的存储空间]，每行的列数
mat[1]=new int[3];

其他操作与前述类似

### 5.8.1 可变参数列表

在java SE5中，添加了可变得参数列表，等价于数组

```
//下面采用数组形参来定义方法
public static void test(int a ,String[] books);
//以可变个数形参来定义方法
public static void test(int a ,String…books);

```
**格式：形参：数据类型…形参名**
该方法与同名的方法之间构成重载。
在调用可变个数的形参时，个数是0到无穷。
可变参数方法的使用与方法参数部分使用数组是一致的，即上述两种方法是一致的。
**方法的参数部分有可变形参，需要放在形参声明的最后**

## 5.9 枚举类型

在Java SE5中添加了**enum**关键字，使得我们在需要群组并使用枚举类型集的时候，可以很方便的处理。例如：

```
public enum Spiciness{
	NOT,MILD,MEDIUM,HOT,FLAMING
}
```

这里创建了一个名为Spiciness的枚举类型，它具有5个具名值，由于枚举类型的实例是常量，因此按照命名惯例它们都是用大写字母表示。

为了使用enum，需要创建一个该类型的引用，将其赋值给某个实例；

```
//: initialization/SimpleEnumUse.java

public class SimpleEnumUse {
  public static void main(String[] args) {
    Spiciness howHot = Spiciness.MEDIUM;
    System.out.println(howHot);
  }
} /* Output:
MEDIUM
*///:~

```

当你创建enum时，编译器会自动添加一些有用的特性，例如：它会创建toString()方法，显示某个enum实例的名字，编译器还会创建ordinal()方法，表示某个特定enum常量的声明顺序，以及**static values()方法**，按照声明顺序，产生由这些常量值构成的数组：

```
/: initialization/EnumOrder.java

public class EnumOrder {
  public static void main(String[] args) {
    for(Spiciness s : Spiciness.values())
      System.out.println(s + ", ordinal " + s.ordinal());
  }
} /* Output:
NOT, ordinal 0
MILD, ordinal 1
MEDIUM, ordinal 2
HOT, ordinal 3
FLAMING, ordinal 4
*///:~
```
在很大程度上，你是**可以将enum当做任何类来处理**。enum有一个特别实用的特性，即可以在是switch语句内使用：

```
//: initialization/Burrito.java

public class Burrito {
  Spiciness degree;
  public Burrito(Spiciness degree) { this.degree = degree;}
  public void describe() {
    System.out.print("This burrito is ");
    switch(degree) {
      case NOT:    System.out.println("not spicy at all.");
                   break;
      case MILD:
      case MEDIUM: System.out.println("a little hot.");
                   break;
      case HOT:
      case FLAMING:
      default:     System.out.println("maybe too hot.");
    }
  }	
  public static void main(String[] args) {
    Burrito
      plain = new Burrito(Spiciness.NOT),
      greenChile = new Burrito(Spiciness.MEDIUM),
      jalapeno = new Burrito(Spiciness.HOT);
    plain.describe();
    greenChile.describe();
    jalapeno.describe();
  }
} /* Output:
This burrito is not spicy at all.
This burrito is a little hot.
This burrito is maybe too hot.
*///:~

```

# 第6章 访问权限控制

如何把变动的事物与不变动的事物区分开来是面向对象设计中需要考虑的一个基本问题。在修改和完善代码的压力下，如何保证某些代码是不可变动，哪些是有权限可以变动的。为了解决这一问题，java提供了访问权限修饰词，供类库开发人员向客户端程序员指明哪些是可用的，哪些是不可用的，权限等级从大到小依次为：**public、protected、包访问权限（默认）、private。**

## 6.1 包：库单元

包内含有一组类，它们在单一的名字空间下被组织在了一起
例如在java标准发布中有一个工具库，它被组织在java.util名字空间之下。java.util中有一个叫做ArrayList的类。**使用ArrayList的一种方式是使用全名。**

```
public class FullQualification{
	public static void main(String[]args){
	java.util.ArrayList list = new java.util.ArrayList();
	}
}
```
另一种方式就是使用**import**语句

```
import java.util.ArrayList;
public class FullQualification{
	public static void main(String[]args){
	java.util.ArrayList list = new ArrayList();
	}
}
```

如果想导入util包下的其他类，可以使用`import java.util.*;`
导入包的意义是提供一个管理名字空间的机制。所有类成员的名称是彼此隔离的。

当编写一个java源代码文件时，此文件通常被称为编译单元，每个编译单元都必须有一个后缀名为.java，而在编译单元内则有一个public类，类的名称与文件名相同。

## 6.1.1 代码组织

Java可运行程序是一组可以打包并压缩为一个java文档文件的.class文件。java解释器负责这些文件的查找，装载和解释。

类库实际上是一组类文件，每个文件都有一个public类，以及任意数量的非public类。每个文件都有一个构件。**如果希望这些构件从属于一个群组，就可以使用关键字package。**如果使用package语句，必须是文件中除注释外第一句程序代码。

`package acess;`表明你声明该编译单元是名为access类库中的一部分。如果想使用access类库下的任何类，可以使用**import**语句或者使用完整名称。

### 6.1.2 独一无二的包名

包是有许多.class文件构成。可以将特定包下的所有.class文件都置于一个目录下。**这里需要解决两个问题：怎样创建独一无二的名称以及怎么查找有可能隐藏于目录结构中某处的类。**

按照惯例，package的名称的第一部分是**类的创建者的反顺序的Internet域名**。如果没有自己的域名，就得构造一组不大可能与他人重复的组合（例如姓名）。如果你打算发布你的java程序代码，还是有必要获得一个域名。

第二个部分就是将**package名称分解为你机器上的一个目录**。

### 6.1.3 定制工具库

可以创建自己的工具来减少或消除重复的程序代码。

示例：

```
//: net/mindview/util/Print.java
// Print methods that can be used without
// qualifiers, using Java SE5 static imports:
package net.mindview.util;
import java.io.*;

public class Print {
  // Print with a newline:
  public static void print(Object obj) {
    System.out.println(obj);
  }
  // Print a newline by itself:
  public static void print() {
    System.out.println();
  }
  // Print with no line break:
  public static void printnb(Object obj) {
    System.out.print(obj);
  }
  // The new Java SE5 printf() (from C):
  public static PrintStream
  printf(String format, Object... args) {
    return System.out.printf(format, args);
  }
} ///:~

```

### 6.1.4 使用包的忠告

无论何时创建包，都已经在给定包的名称的时候隐含地指定了目录结构。这个包必须位于其名称所指定的目录之中，而该目录必须是在以CLASSPATH开始的目录中可以查询到。编译过的代码一般放在与源代码的不同目录中。

## 6.2 java访问权限修饰词

**public、protected、private**这几个java访问权限修饰词在使用时是置于类中的每个成员的定义之前的——无论它是一个域还是一个方法。**如果不提供任何访问权限修饰词，意味着它是包访问权限修饰词。**

|  修饰符   | 类内部 | 同一个包 | 子类 | 任何地方 |
| :-------: | :----: | :------: | :--: | :------: |
|  private  |  YES   |          |      |          |
| （缺省）  |  YES   |   YES    |      |          |
| protected |  YES   |   YES    | YES  |          |
|  public   |  YES   |   YES    | YES  |   YES    |

### 6.2.1 包访问权限
默认访问权限没有任何关键字，即指包访问权限。意味着当前包中的所有其他类对那个成员都有访问权限，但对于这个包之外的所有类，这个成员却是private。

### 6.2.2 public：接口访问权限

使用关键字**public**，意味着public之后紧跟着的成员声明自己对每个人都是可用的。尤其是使用类库的客户端程序员。

**注意默认包**：如果类没有声明自己所在的包，java将这样的文件看作是隶属于该目录的默认包之中。

### 6.2.3 private: 你无法访问

**关键字private的**意思是：除了包含这个成员的类之外，其他任何类都无法访问这个成员。处于同一个包内的其他类是不可以访问private成员的，等于说自己隔离了自己。

默认的包访问权限通常已经提供了充足的隐藏措施。**使用类的客户端程序员是无法访问包访问权限成员的。**默认访问权限是一种我们常用的权限，同时也是一种忘记添加任何访问权限控制的自动得到的权限。

任何可以肯定知识该类的一个助手方法的方法，都可以把它指定为private，以确保不会再包内的其他地方误用到它，防止了你会去改变和删除这个方法。

### 6.2.4 protected：继承访问权限

关键字protected处理的是继承的概念，通过继承可以利用一个现有的类——基类，然后将新成员添加到该类中而不必碰该现有类。还可以改变该类的现有成员的行为。为了从现有类中继承，需要声明新类extends了一个类：
`class Foo extends Bar{}`

如果创建了一个新包，并自另一个包中的继承类，那么唯一可以访问的成员就是源包的public成员。基类的创建者希望有某个特定的成员，把对它的访问权限赋予派生类而不是所有类，这就需要protected来完成这一个工作。 

```
//: access/cookie2/Cookie.java
package access.cookie2;

public class Cookie {
  public Cookie() {
    System.out.println("Cookie constructor");
  }
  protected void bite() {
    System.out.println("bite");
  }
} ///:~

//: access/ChocolateChip2.java
import access.cookie2.*;

public class ChocolateChip2 extends Cookie {
  public ChocolateChip2() {
   System.out.println("ChocolateChip2 constructor");
  }
  public void chomp() { bite(); } // Protected method
  public static void main(String[] args) {
    ChocolateChip2 x = new ChocolateChip2();
    x.chomp();
  }
} /* Output:
Cookie constructor
ChocolateChip2 constructor
bite
*///:~

```

## 6.3 接口和实现

访问权限控制将权限边界划在了数据类型的内部有两个原因：

1. 设定客户端程序员可以使用和不可以使用的界限
2. 将接口和具体实现隔离。

## 6.4 类的访问权限

访问修饰词可以用于确定库中的哪些类对于该库的使用者是可用的。

- 每个编译单元（文件）都只能有一个public类。这表示每个编译单元都有一个单一的公共接口用public类实现。
- public类的名称与文件名完全匹配。
- 编译单元内不带public类也是可能
- 对于类的访问权限仅有两个选择：包访问权限和public。

# 第7章 复用类

达到复用这个目的的方法有两种：

1. 只需在新的类中产生现有类的对象。由于新的类是现有类的对象所组成，这种方法称之为**组合**
2. 按照现有类的类型来创建新类。无需改变现有类的形式，采用现有类的形式并在其中添加新代码。这种方式称之为**继承**。

组合和继承类似，都是利用现有类型生成新类型。

## 7.1 组合语法

组合，只需将对象引用至于新类中即可。对于基本类型数据，可以直接在新类中定义。

示例：

```
//: reusing/SprinklerSystem.java
// Composition for code reuse.

class WaterSource {
  private String s;
  WaterSource() {
    System.out.println("WaterSource()");
    s = "Constructed";
  }
  public String toString() { return s; }
}	

public class SprinklerSystem {
  private String valve1, valve2, valve3, valve4;
  private WaterSource source = new WaterSource();
  private int i;
  private float f;
  public String toString() {
    return
      "valve1 = " + valve1 + " " +
      "valve2 = " + valve2 + " " +
      "valve3 = " + valve3 + " " +
      "valve4 = " + valve4 + "\n" +
      "i = " + i + " " + "f = " + f + " " +
      "source = " + source;
  }	
  public static void main(String[] args) {
    SprinklerSystem sprinklers = new SprinklerSystem();
    System.out.println(sprinklers);
  }
} /* Output:
WaterSource()
valve1 = null valve2 = null valve3 = null valve4 = null
i = 0 f = 0.0 source = Constructed
*///:~

```

```
//: reusing/Bath.java
// Constructor initialization with composition.
import static net.mindview.util.Print.*;
class Soap {
private String s;
166 Thinking in Java Bruce Eckel
Soap() {
print("Soap()");
s = "Constructed";
}
public String toString() { return s; }
}
public class Bath {
private String // Initializing at point of definition:
s1 = "Happy",
s2 = "Happy",
s3, s4;
private Soap castille;
private int i;
private float toy;
public Bath() {
print("Inside Bath()");
s3 = "Joy";
toy = 3.14f;
castille = new Soap();
}
// Instance initialization:
{ i = 47; }
public String toString() {
if(s4 == null) // Delayed initialization:
s4 = "Joy";
return
"s1 = " + s1 + "\n" +
"s2 = " + s2 + "\n" +
"s3 = " + s3 + "\n" +
"s4 = " + s4 + "\n" +
"i = " + i + "\n" +
"toy = " + toy + "\n" +
"castille = " + castille;
}
public static void main(String[] args) {
Bath b = new Bath();
print(b);
}
} /* Output:
Inside Bath()
Soap()
s1 = Happy
s2 = Happy
s3 = Joy
s4 = Joy
i = 47
toy = 3.14
castille = Constructed
*///:~
```

**备注：**在对复用类进行初始化的时候再对里面引用的对象进行初始化，一开始设置为引用空指着null，减少不必要的负担。

## 7.2 继承语法

创建一个类，总是在继承，显示或隐式继承Object。

语法规则：`class Subclass extends Superclass{}`



![继承模型](http://img.blog.csdn.net/20171031091717107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```

```

关于继承语法，一般的规则是将所有的数据成员都指定为private，将所有的方法都指定为public，特殊情况下可以做出调整。
**继承的作用：**
继承的出现提高了代码的复用性。
继承的出现让类与类之间产生了关系，提供了多态的前提。
不要仅为了获取其他类中某个功能而去继承
**关于继承的其他规则：**
子类不能直接访问父类中私有的(private)的成员变量和方法
![子类不能直接访问父类private成员和方法](http://img.blog.csdn.net/20171031092845049?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

子类同样可以获取得到父类中声明的私有的属性或方法，只是由于封装的设计，使得子类不能够直接调用。
在子类中，可以使用父类中定义的方法和属性，也可以创建新的数据和方法。
**Java只支持单继承，不允许多重继承**
一个子类只能有一个父类
一个父类可以派生出多个子类

```
class SubDemo extends Demo{ }   //ok
class SubDemo extends Demo1,Demo2...//error

```
![继承规则](http://img.blog.csdn.net/20171031093058979?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 7.2.1 初始化基类

创建了一个导出类的对象，该对象包含了一个基类的子对象。这个子对象与直接用基类创建对象是一样的。区别在于后者来自外部，而基类的子对象被包装在导出类对象内部。

**对于基类的初始化至关重要，仅有一种方法来保证这一点**：在构造器中调用基类构造器来执行初始化，基类构造器具有执行基类初始化所需要的所有知识和能力。java会自动在导出类的构造器中插入对基类构造器的调用。例如：

```
//: reusing/Cartoon.java
// Constructor calls during inheritance.
import static net.mindview.util.Print.*;

class Art {
  Art() { print("Art constructor"); }
}

class Drawing extends Art {
  Drawing() { print("Drawing constructor"); }
}

public class Cartoon extends Drawing {
  public Cartoon() { print("Cartoon constructor"); }
  public static void main(String[] args) {
    Cartoon x = new Cartoon();
  }
} /* Output:
Art constructor
Drawing constructor
Cartoon constructor
*///:~

```

<font color=red>**带参数构造器**</font>
上述例子中各个类均含有默认的构造器，即不带参数。如果想调用带参数的基类构造器，这时候就必须使用**super关键字**，可以调用基类的构造器。

```
//: reusing/Chess.java
// Inheritance, constructors and arguments.
import static net.mindview.util.Print.*;

class Game {
  Game(int i) {
    print("Game constructor");
  }
}

class BoardGame extends Game {
  BoardGame(int i) {
    super(i);
    print("BoardGame constructor");
  }
}	

public class Chess extends BoardGame {
  Chess() {
    super(11);
    print("Chess constructor");
  }
  public static void main(String[] args) {
    Chess x = new Chess();
  }
} /* Output:
Game constructor
BoardGame constructor
Chess constructor
*///:~
```

## 7.3 代理

**代理为第三种关系，这是继承和组合之间的中庸之道**。

将一个成员的对象置于构造的类中（组合），与此同时在新类中暴露了该成员对象的所有方法（就像继承），例如，太空船需要一个控制模块：

```
//: reusing/SpaceShipControls.java

public class SpaceShipControls {
  void up(int velocity) {}
  void down(int velocity) {}
  void left(int velocity) {}
  void right(int velocity) {}
  void forward(int velocity) {}
  void back(int velocity) {}
  void turboBoost() {}
} ///:~
```

构造太空船的一种方法就是使用继承：

```
//: reusing/SpaceShip.java

public class SpaceShip extends SpaceShipControls {
  private String name;
  public SpaceShip(String name) { this.name = name; }
  public String toString() { return name; }
  public static void main(String[] args) {
    SpaceShip protector = new SpaceShip("NSEA Protector");
    protector.forward(100);
  }
} ///:~
```

SpaceShip包含了SpaceShipControls，但是与此同时SpaceShipControls的所有方法都在SpaceShip中暴露出来，**代理解决了这个难题**。

```
//: reusing/SpaceShipDelegation.java
public class SpaceShipDelegation {
  private String name;
  private SpaceShipControls controls =
    new SpaceShipControls();
  public SpaceShipDelegation(String name) {
    this.name = name;
  }
  // Delegated methods:
  public void back(int velocity) {
    controls.back(velocity);
  }
  public void down(int velocity) {
    controls.down(velocity);
  }
  public void forward(int velocity) {
    controls.forward(velocity);
  }
  public void left(int velocity) {
    controls.left(velocity);
  }
  public void right(int velocity) {
    controls.right(velocity);
  }
  public void turboBoost() {
    controls.turboBoost();
  }
  public void up(int velocity) {
    controls.up(velocity);
  }
  public static void main(String[] args) {
    SpaceShipDelegation protector =
      new SpaceShipDelegation("NSEA Protector");
    protector.forward(100);
  }
} ///:~
```

使用代理可以得到更多的控制力，可以选择只提供在成员对象中某个方法的子集。

## 7.4 结合使用组合和继承

示例：

```
//: reusing/PlaceSetting.java
// Combining composition & inheritance.
import static net.mindview.util.Print.*;

class Plate {
  Plate(int i) {
    print("Plate constructor");
  }
}

class DinnerPlate extends Plate {
  DinnerPlate(int i) {
    super(i);
    print("DinnerPlate constructor");
  }
}	

class Utensil {
  Utensil(int i) {
    print("Utensil constructor");
  }
}

class Spoon extends Utensil {
  Spoon(int i) {
    super(i);
    print("Spoon constructor");
  }
}

class Fork extends Utensil {
  Fork(int i) {
    super(i);
    print("Fork constructor");
  }
}	

class Knife extends Utensil {
  Knife(int i) {
    super(i);
    print("Knife constructor");
  }
}

// A cultural way of doing something:
class Custom {
  Custom(int i) {
    print("Custom constructor");
  }
}	

public class PlaceSetting extends Custom {
  private Spoon sp;
  private Fork frk;
  private Knife kn;
  private DinnerPlate pl;
  public PlaceSetting(int i) {
    super(i + 1);
    sp = new Spoon(i + 2);
    frk = new Fork(i + 3);
    kn = new Knife(i + 4);
    pl = new DinnerPlate(i + 5);
    print("PlaceSetting constructor");
  }
  public static void main(String[] args) {
    PlaceSetting x = new PlaceSetting(9);
  }
} /* Output:
Custom constructor
Utensil constructor
Spoon constructor
Utensil constructor
Fork constructor
Utensil constructor
Knife constructor
Plate constructor
DinnerPlate constructor
PlaceSetting constructor
*///:~
```

## 7.4.1 正确清理

如果想要某个类清理一些东西，就必须显式地编写一个特殊方法来做这个事，并要确保客户端程序员知晓他们必须要调用这一方法。

示例：

```
//: reusing/CADSystem.java
// Ensuring proper cleanup.
package reusing;
import static net.mindview.util.Print.*;
class Shape {
Shape(int i) { print("Shape constructor"); }
void dispose() { print("Shape dispose"); }
}
class Circle extends Shape {
Circle(int i) {
super(i);
print("Drawing Circle");
}
void dispose() {
print("Erasing Circle");
super.dispose();
}
}
class Triangle extends Shape {
Triangle(int i) {
super(i);
print("Drawing Triangle");
}
void dispose() {
print("Erasing Triangle");
super.dispose();
}
}
class Line extends Shape {
private int start, end;
Line(int start, int end) {
super(start);
this.start = start;
this.end = end;
print("Drawing Line: " + start + ", " + end);
}
void dispose() {
print("Erasing Line: " + start + ", " + end);
super.dispose();
}
}
public class CADSystem extends Shape {
private Circle c;
private Triangle t;
private Line[] lines = new Line[3];
public CADSystem(int i) {
super(i + 1);
Reusing Classes 175
for(int j = 0; j < lines.length; j++)
lines[j] = new Line(j, j*j);
c = new Circle(1);
t = new Triangle(1);
print("Combined constructor");
}
public void dispose() {
print("CADSystem.dispose()");
// The order of cleanup is the reverse
// of the order of initialization:
t.dispose();
c.dispose();
for(int i = lines.length - 1; i >= 0; i--)
lines[i].dispose();
super.dispose();
}
public static void main(String[] args) {
CADSystem x = new CADSystem(47);
try {
// Code and exception handling...
} finally {
x.dispose();
}
}
} /* Output:
Shape constructor
Shape constructor
Drawing Line: 0, 0
Shape constructor
Drawing Line: 1, 1
Shape constructor
Drawing Line: 2, 4
Shape constructor
Drawing Circle
Shape constructor
Drawing Triangle
Combined constructor
CADSystem.dispose()
Erasing Triangle
Shape dispose
Erasing Circle
Shape dispose
Erasing Line: 2, 4
Shape dispose
Erasing Line: 1, 1
Shape dispose
Erasing Line: 0, 0
Shape dispose
Shape dispose
*///:~
```

### 7.4.2 名称屏蔽

如果java基类拥有某个已被多次重载的方法名称，在导出类中重新定义该方法名称并不会屏蔽其在基类中的任何一个版本。因此，无论是在该层还是它的基类中对方法进行定义，重载机制都可以正常工作。
示例：

```
//: reusing/Hide.java
// Overloading a base-class method name in a derived
// class does not hide the base-class versions.
import static net.mindview.util.Print.*;

class Homer {
  char doh(char c) {
    print("doh(char)");
    return 'd';
  }
  float doh(float f) {
    print("doh(float)");
    return 1.0f;
  }
}

class Milhouse {}

class Bart extends Homer {
  void doh(Milhouse m) {
    print("doh(Milhouse)");
  }
}

public class Hide {
  public static void main(String[] args) {
    Bart b = new Bart();
    b.doh(1);
    b.doh('x');
    b.doh(1.0f);
    b.doh(new Milhouse());
  }
} /* Output:
doh(float)
doh(char)
doh(float)
doh(Milhouse)
*///:~
```

java SE5新增了@Override注解，并不是关键字，但是可以当做关键字使用，如果想要覆写某个方法可以使用这个注解，在你重载时而非重写的时候生成错误信息，防止你在不想重载的时候意外进行了重载。

## 7.5 组合与继承之间选择

组合和继承都允许在新的类中放置子对象，但是组合是显式地这样做，继承则是隐式地做，**二者的区别**在于：

- 组合技术通常用于想在新类中使用现有类的功能而非它的接口这种形式。即在新类中嵌入某个对象，让它实现需要的功能。新类用户看到的是为新类定义的接口，而非所嵌入对象的接口。**即需要在新类中嵌入一个现有类的private类**。

- 在继承的时候使用某个现有类，并开发一个它的特殊版本。通常，这意味者你在使用一个通用类，并为了某种特殊需要将其特殊化。**通常“is-a”的关系用继承表达，而“has-a”的关系用组合表达。**

## 7.6 protected关键字

实际项目中，经常想要将某些事物尽可能的隐藏起来，但仍然允许导出类的成员访问它们。关键字protected就是起的这个作用

## 7.7 向上转型

**新类是现有类的一种类型**。

假设有一个称为Instrument乐器的基类和一个称为Wind的导出类，由于继承可以确保基类中所有的方法在导出类同样有效。能够向基类发送的信息同样可以发送向导出类发送。这意味着我们可以准确地说Wind对象也是一种类型的Instrument

```
//: reusing/Wind.java
// Inheritance & upcasting.

class Instrument {
  public void play() {}
  static void tune(Instrument i) {
    // ...
    i.play();
  }
}

// Wind objects are instruments
// because they have the same interface:
public class Wind extends Instrument {
  public static void main(String[] args) {
    Wind flute = new Wind();
    Instrument.tune(flute); // Upcasting
  }
} ///:~
```

此例中，tune()方法可以接受Instrument引用。上述这种将wind引用转换为Instrument引用的动作称之为向上转型。即子类（导出类）到父类（基类）的类型转换，它可以自动进行，导出类是基类的一个超集，比基类包含更多方法，并且必须至少具备基类中所含有的方法。

**向上转型还可以用来作为选择组合还是继承的判断**，如果必须使用向上转型，则继承是必要的。

## 7.8 final关键字

final通常指这是无法改变的。

**可能使用final的有三种情况：数据，方法，类。**

### 7.8.1 final数据

许多编程语言都有某种方法，来向编译器告知一块数据是永恒不变的。有时数据永恒不变是很有用的，比如：

  1. 一个永不改变的编译时常量
  2. 一个在运行时被初始化的值，而你不希望它被改变。

在java中这类常量必须是基本数据类型并且是以final关键字表示，对这个常量进行定义的时候，**必须对其进行赋值**，

例如：`final int valueOne=15;`

**一个既是static有时final的域只占据一段不能改变的存储空间。**
当对对象引用而不是基本类型运用final，是，final使其引用恒定不变。一旦引用被初始化指向一个对象，就无法再把它改为指向另一个对象，然而，对象其自身是可以被修改的，java并未提供使其对象恒定不变的途径。

<font color=red>**空白final**</font>
java允许生成空白final。即被声明为final但又未给定初值的域。无论什么情况，编译器都确保空白final使用前必须被初始化。

```
//: reusing/BlankFinal.java
// "Blank" final fields.
class Poppet {
  private int i;
  Poppet(int ii) { i = ii; }
}
public class BlankFinal {
  private final int i = 0; // Initialized final
  private final int j; // Blank final
  private final Poppet p; // Blank final reference
  // Blank finals MUST be initialized in the constructor:
  public BlankFinal() {
    j = 1; // Initialize blank final
    p = new Poppet(1); // Initialize blank final reference
  }
  public BlankFinal(int x) {
    j = x; // Initialize blank final
    p = new Poppet(x); // Initialize blank final reference
  }
  public static void main(String[] args) {
    new BlankFinal();
    new BlankFinal(47);
  }
} ///:~
```

必须在域的定义处或者每个构造器中用表达式对final进行赋值。（**这里不可以有static表示**）

<font color=red>**final参数**</font>
java允许在参数列表中以声明的方式将参数指明为final。意味着无法再方法中更改参数引用所指向的对象。即你可以读参数，但是不可以修改参数。

### 7.8.2 final方法

**使用final方法的两个原因**：

1. 把方法锁定，以防任何继承类修改它的含义，即无法重写
2. 出于**效率**（过去）

<font color = red>**final和private关键字**</font>
类中所有的private方法都隐式的指定是final的。无法取用private方法，也就无法覆盖它。
如果你试图覆盖一个private方法，似乎是奏效的，但是没有任何意义。**覆盖只有在某方法是某基类的接口的一部分才会出现，即必须能将一个对象向上转型为它的基本类型并调用相同的方法。**如果某方法是private，它就不是基本类的接口的一部分。它仅是一些隐藏类中的程序代码，只不过具有相同的名称而已。

### 7.8.3 final类

当将某个类整体定义为final，则表明禁止继承，不希望它有子类。在final类中可以给方法添加final修饰词，但是这没有意义。**对于方法指明为final时要慎重**。

## 7.9 static关键字

**表示静态的，可以用来修饰属性，方法，代码块（初始化块），内部块。（<font color=red>除了构造器不行</font>）**

**1.	修饰属性：（类变量）**
a)	由类创建的所有对象，都共用一个属性
b)	当其中一个对象对此属性进行修改，会导致其他对象对此属性的调用VS 实例对象（非static修饰属性）

![静态](http://img.blog.csdn.net/20171031144220490?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

c)	类变量随着类的加载而加载，而且独一份
d)	静态的变量可以直接通过“**类.类变量**”的形式来调用。

**2.	修饰方法（类方法）**

a)	随着类的加载而加载，在内存中也是独一份
b)	可以直接通过“**类.类方法**”的方式调用。
c)	在静态方法内部，可以调用静态的属性或方法，而不能调用非静态的属性或方法。反之非静态的方法是可以调用静态的属性或方法，静态的生命周期更长，同时消亡被回收也要晚于非静态结构。

**被修饰后的成员具备以下特点：**
随着类的加载而加载
优先于对象存在
修饰的成员，被所有对象所共享
访问权限允许时，可不创建对象，直接被类调用
	因为不需要实例就可以访问static方法，因此**static方法内部不能有this**。(**也不能有super ? YES!**)
<font color =red>**重载的方法需要同时为static的或者非static的**</font>

## 7.10 方法重写

**定义**：在子类（导出类）中可以根据需要对从父类（基类）中继承来的方法进行改造，也称方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。

**要求**：

- 重写方法必须和被重写方法**具有相同的方法名称、参数列表和返回值类型**。
- 重写方法**不能使用比被重写方法更严格的访问权限**。
- 重写和被重写的方法须**同时为static的，或同时为非static**的
- 子类方法抛出的**异常不能大于**父类被重写方法的异常

```
public class Main{
	void find(){
		System.out.println("Main");
	}
	public static void main(String[]args){
		Main M= new Subclass();
		m.find();
	}
}
class Subclass extends Main{
	void find(){
		System.out.println("Subclass");
	}
}
//out:
Subclass
```

## 7.11 super关键字

在继承结构中，如果想要调用基类（父类）的构造器，被重写的方法，属性（域）等，可以使用关键字来实现。
<font color =red>**super修饰方法**</font>

当导出类重写基类的方法以后，在导出类中若要显式的调用父类的被重写的方法可以使用关键字super，格式为:`super.method();`

<font color =red>**super修饰属性（域）**</font>
当子父类出现同名成员的时候，可以用super进行区分，这和this有点类似。this代表本类对象应用，super代表父类的内存空间标识。**super追溯不仅限于直接父类**。

<font color =red>**super修饰构造器**</font>
通过在子类中使用super（形参列表）类显示调用父类中指定的构造器。
	子类中所有的构造器默认都会访问父类中空参数的构造器
	当父类中没有空参数的构造器时，子类的构造器必须通过this(参数列表)或者super(参数列表)语句指定调用本类或者父类中相应的构造器，且**必须放在构造器的第一行**，<font color=red>**this（形参列表）和super（形参列表）只能出现一个。**</font>

```
public class Student extends Person {
	private String school;
	public Student(String name, int age, String s) {
    super(name, age);
		school = s;
	}
 	public Student(String name, String s) {
		super(name);
		school = s;
 	}
 	public Student(String s) { // 编译出错: no super(),系统将调用父类无参数的构造方法。
 	school=s;
 	}
 }

```

所以设计一个类的时候，尽量设计一个空参的构造器，防止编译出错。

**关键字this与super的区别**

| No.  | 区别点     | this                                                   | super                                    |
| ---- | ---------- | ------------------------------------------------------ | ---------------------------------------- |
| 1    | 访问属性   | 访问本类中的属性，如果本类没有此属性则从父类中继续查找 | 访问父类中的属性                         |
| 2    | 调用方法   | 访问本类中的方法                                       | 直接访问父类中的方法                     |
| 3    | 调用构造器 | 调用本类构造器，必须放在构造器首行                     | 调用父类构造器，必须放在子类构造器的首行 |
| 4    | 特殊       | 表示当前对象                                           | 无此概念                                 |

## 7.12 初始化及类的加载

**所有static对象和static代码段都会在加载时依据程序中的顺序依次初始化。定义为static的东西只会被初始化一次**。

```
//: reusing/Beetle.java
// The full process of initialization.
import static net.mindview.util.Print.*;
class Insect {
private int i = 9;
protected int j;
Insect() {
print("i = " + i + ", j = " + j);
j = 39;
}
private static int x1 =
printInit("static Insect.x1 initialized");
static int printInit(String s) {
print(s);
return 47;
}
}
public class Beetle extends Insect {
private int k = printInit("Beetle.k initialized");
public Beetle() {
print("k = " + k);
print("j = " + j);
}
private static int x2 =
printInit("static Beetle.x2 initialized");
public static void main(String[] args) {
print("Beetle constructor");
Beetle b = new Beetle();
}
} /* Output:
static Insect.x1 initialized
static Beetle.x2 initialized
Beetle constructor
i = 9, j = 0
Beetle.k initialized
k = 47
j = 39
*///:~
```

# 第8章 多态

多态通过分离**做什么和怎么做**，从另一个角度将接口和实现分离。多态的作用是消除类型之间的耦合关系。

## 8.1 再论向上转型

已知对象既可以作为它自己本身的类型使用，也可以作为它的基本类型使用。这种把某个对象的引用视为其基类型的引用的做法被称作向上转型。
例如前述的基类Instrument和导出类Wind。如果在Music类中有一个方法表示演奏，假设它的引用是Instrument类，例如`tune(Instrument i)`,则Wind也可以调用这个方法，将自己当做Instrument类。如果这个方法的引用是Wind类，例如`tune(Wind i)`,则如果添加新的乐器进行演奏，例如Stringed类和Brass类，则不能调用这个方法，需要分别添加`tune(Stringed i)`和`tune(Brass i)`方法。这使得需要大量的工作。

也就是说只写这么一个简单的方法，仅接受基类作为参数，例如`tuen(Instrument i)`，而不是那些特殊的导出类。这样不管导出类是什么，都只要和基类打交道，而这正是多态所允许的。

## 8.2 转机

如前述的方法`tune(Instrument i)`方法，它接受一个一个Instrument引用，如何判断这个引用指向的是一个Wind对象，而不是Brass对象或者Stringed对象？实际上编译器不知道，**这里涉及到绑定这个话题**。

### 8.2 1 方法调用的绑定

**<font color=red>将一个方法调用同一个方法主体关联起来被称为绑定。</font>**在程序执行前进行绑定，叫做前期绑定（面向过程）。**这里解决前述问题的方法是后期绑定。**

**后期绑定的含义**：在运行时根据对象的类型进行绑定。后期绑定也可以叫做**动态绑定**或者**运行时绑定**。如果一种语言想要实现后期绑定，就必须实现某种机制，以便在运行时能够判断对象的类型，从而调用恰当的方法。也就是编译器一直不知道对象的类型，但是在对象调用机制能找到正确的方法体，并加以调用。

Java中除了**static和final**方法，其他所有方法都是后期绑定。

**关于多态或者可以这说：**Java中有两种类型：编译时类型和运行时类型。编译时类型由声明该变量时使用的类型决定的，运行时类型是由实际赋给该变量的对象决定的。若**编译时类型和运行时类型不一致，就出现多态**，<font color =red>**调用方法的版本，是实际赋给该变量的对象决定的**</font>。

```
Person是父类，Man是子类
Person p = new Man();//体现子类对象的多态性父类的引用指向子类对象。
```

一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属性和方法，

```
	Student m = new Student();
	m.school = “pku”; 	//合法,Student类有school成员变量
	Person e = new Student(); 
	e.school = “pku”;	//非法,Person类没有school成员变量
```

属性是在编译时确定的，编译时e为Person类型，没有school成员变量，因而编译错误。**即调用的方法父类和子类都有，即子类对父类方法的重写。（<font color=red>成员变量不具备多态性</font>）**

<font color=red>**示例**</font>：

```
//: polymorphism/music3/Music3.java
// An extensible program.
package polymorphism.music3;
import polymorphism.music.Note;
import static net.mindview.util.Print.*;
class Instrument {
	void play(Note n) { print("Instrument.play() " + n); }
	String what() { return "Instrument"; }
	void adjust() { print("Adjusting Instrument"); }
}
class Wind extends Instrument {
	void play(Note n) { print("Wind.play() " + n); }
	String what() { return "Wind"; }
	void adjust() { print("Adjusting Wind"); }
}
class Percussion extends Instrument {
	void play(Note n) { print("Percussion.play() " + n); }
	String what() { return "Percussion"; }
	void adjust() { print("Adjusting Percussion"); }
}
class Stringed extends Instrument {
	void play(Note n) { print("Stringed.play() " + n); }
	String what() { return "Stringed"; }
	void adjust() { print("Adjusting Stringed"); }
}
class Brass extends Wind {
	void play(Note n) { print("Brass.play() " + n); }
	void adjust() { print("Adjusting Brass"); }
}
class Woodwind extends Wind {
	void play(Note n) { print("Woodwind.play() " + n); }
	String what() { return "Woodwind"; }
}
public class Music3 {
// Doesn’t care about type, so new types
// added to the system still work right:
	public static void tune(Instrument i) {
		// ...
		i.play(Note.MIDDLE_C);
	}
	public static void tuneAll(Instrument[] e) {
		for(Instrument i : e)
		tune(i);
	}
	public static void main(String[] args) {
		// Upcasting during addition to the array:
		Instrument[] orchestra = {
			new Wind(),
            new Percussion(),
            new Stringed(),
            new Brass(),
            new Woodwind()
		};
		tuneAll(orchestra);
	}
} /* Output:
Wind.play() MIDDLE_C
Percussion.play() MIDDLE_C
Stringed.play() MIDDLE_C
Brass.play() MIDDLE_C
Woodwind.play() MIDDLE_C
*///:~
```

### 8.2.2 缺陷：“覆盖”私有方法

```
//: polymorphism/PrivateOverride.java
// Trying to override a private method.
package polymorphism;
import static net.mindview.util.Print.*;

public class PrivateOverride {
  private void f() { print("private f()"); }
  public static void main(String[] args) {
    PrivateOverride po = new Derived();
    po.f();
  }
}

class Derived extends PrivateOverride {
  public void f() { print("public f()"); }
} /* Output:
private f()
*///:~

```
我们期望输出是public f()，但是由于private方法被自动认为是一个final方法，对导出类是屏蔽的，因此这时候，Derived类的f()方法是一个全新的方法。
**结论：只有非private方法才可以被覆盖。导出类中，对于基类中private方法，最好采用不同的名字。**

### 8.2.3 缺陷：域与静态方法

**关于域，即前述的属性，它是不存在多态性的**。只有普通的方法调用可以是多态。

```
//: polymorphism/FieldAccess.java
// Direct field access is determined at compile time.

class Super {
  public int field = 0;
  public int getField() { return field; }
}

class Sub extends Super {
  public int field = 1;
  public int getField() { return field; }
  public int getSuperField() { return super.field; }
}

public class FieldAccess {
  public static void main(String[] args) {
    Super sup = new Sub(); // Upcast
    System.out.println("sup.field = " + sup.field +
      ", sup.getField() = " + sup.getField());
    Sub sub = new Sub();
    System.out.println("sub.field = " +
      sub.field + ", sub.getField() = " +
      sub.getField() +
      ", sub.getSuperField() = " +
      sub.getSuperField());
  }
} /* Output:
sup.field = 0, sup.getField() = 1
sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0
*///:~
```

如果某个方法是静态的，它的行为不具备多态性。因为静态方法是与类，而非与单个对象相关联。

```
//: polymorphism/StaticPolymorphism.java
// Static methods are not polymorphic.
class StaticSuper {
  public static String staticGet() {
    return "Base staticGet()";
  }
  public String dynamicGet() {
    return "Base dynamicGet()";
  }
}
class StaticSub extends StaticSuper {
  public static String staticGet() {
    return "Derived staticGet()";
  }
  public String dynamicGet() {
    return "Derived dynamicGet()";
  }
}
public class StaticPolymorphism {
  public static void main(String[] args) {
    StaticSuper sup = new StaticSub(); // Upcast
    System.out.println(sup.staticGet());
    System.out.println(sup.dynamicGet());
  }
} /* Output:
Base staticGet()
Derived dynamicGet()
*///:~
```

## 8.3 构造器和多态

**构造器不同于其他种类的方法，不具有多态性**。（他们实际上是static方法，不过是隐式声明）。

### 8.3.1 构造器的调用顺序

基类的构造器总是在导出类的构造过程中被调用，而且按照继承层次逐渐向上链接，以使每个基类的构造器都能得到调用。**关于构造器的调用顺序应当遵循**：

- 调用基类构造器，这个步骤会不断地反复递归下去，首先是构造这种层次结构的根，然后是下一层导出类，知道最底层导出类。
 - 按照声明顺序调用成员的初始化方法。
 - 调用导出类构造器主体。

### 8.3.2 继承与清理

通过组合和继承方法来创建新类，一般不必担心对象的清理问题，子对象通常都会留给垃圾回收器进行处理。如果确实遇到清理的问题，那么必须用心为新类创建dispose()方法（这个名称随意）。由于继承的缘故，如果有其他作为垃圾回收一部分的特殊清理工作，就必须在导出类中覆盖dispose()方法。**当覆盖被继承类的dispose()方法，务必调用基类版本的dispose()方法，否则，清理动作就不会发生。**

<font color=red>**示例**</font>：

```
//: polymorphism/Frog.java
// Cleanup and inheritance.
package polymorphism;
import static net.mindview.util.Print.*;

class Characteristic {
  private String s;
  Characteristic(String s) {
    this.s = s;
    print("Creating Characteristic " + s);
  }
  protected void dispose() {
    print("disposing Characteristic " + s);
  }
}

class Description {
  private String s;
  Description(String s) {
    this.s = s;
    print("Creating Description " + s);
  }
  protected void dispose() {
    print("disposing Description " + s);
  }
}

class LivingCreature {
  private Characteristic p =
    new Characteristic("is alive");
  private Description t =
    new Description("Basic Living Creature");
  LivingCreature() {
    print("LivingCreature()");
  }
  protected void dispose() {
    print("LivingCreature dispose");
    t.dispose();
    p.dispose();
  }
}

class Animal extends LivingCreature {
  private Characteristic p =
    new Characteristic("has heart");
  private Description t =
    new Description("Animal not Vegetable");
  Animal() { print("Animal()"); }
  protected void dispose() {
    print("Animal dispose");
    t.dispose();
    p.dispose();
    super.dispose();
  }
}

class Amphibian extends Animal {
  private Characteristic p =
    new Characteristic("can live in water");
  private Description t =
    new Description("Both water and land");
  Amphibian() {
    print("Amphibian()");
  }
  protected void dispose() {
    print("Amphibian dispose");
    t.dispose();
    p.dispose();
    super.dispose();
  }
}

public class Frog extends Amphibian {
  private Characteristic p = new Characteristic("Croaks");
  private Description t = new Description("Eats Bugs");
  public Frog() { print("Frog()"); }
  protected void dispose() {
    print("Frog dispose");
    t.dispose();
    p.dispose();
    super.dispose();
  }
  public static void main(String[] args) {
    Frog frog = new Frog();
    print("Bye!");
    frog.dispose();
  }
} /* Output:
Creating Characteristic is alive
Creating Description Basic Living Creature
LivingCreature()
Creating Characteristic has heart
Creating Description Animal not Vegetable
Animal()
Creating Characteristic can live in water
Creating Description Both water and land
Amphibian()
Creating Characteristic Croaks
Creating Description Eats Bugs
Frog()
Bye!
Frog dispose
disposing Description Eats Bugs
disposing Characteristic Croaks
Amphibian dispose
disposing Description Both water and land
disposing Characteristic can live in water
Animal dispose
disposing Description Animal not Vegetable
disposing Characteristic has heart
LivingCreature dispose
disposing Description Basic Living Creature
disposing Characteristic is alive
*///:~

```

销毁顺序应该和初始化顺序相反。

上述例子的Frog对象创建了它自己的成员对象，并且知道他们应该存活多久，因此frog对象知道何时调用dispose()去释放其成员对象。**如果这些成员对象存在于其他一个或多个对象共享的情况，**问题就变得复杂，不能简单地假设你可以调用dispose()了。这种情况下也许就必须使用引用计数来跟踪仍旧访问着共享对象的对象数量了。

<font color=red>**示例**</font>：

```
//: polymorphism/ReferenceCounting.java
// Cleaning up shared member objects.
import static net.mindview.util.Print.*;
class Shared {
private int refcount = 0;
private static long counter = 0;
private final long id = counter++;
public Shared() {
print("Creating " + this);
}
208 Thinking in Java Bruce Eckel
public void addRef() { refcount++; }
protected void dispose() {
if(--refcount == 0)
print("Disposing " + this);
}
public String toString() { return "Shared " + id; }
}
class Composing {
private Shared shared;
private static long counter = 0;
private final long id = counter++;
public Composing(Shared shared) {
print("Creating " + this);
this.shared = shared;
this.shared.addRef();
}
protected void dispose() {
print("disposing " + this);
shared.dispose();
}
public String toString() { return "Composing " + id; }
}
public class ReferenceCounting {
public static void main(String[] args) {
Shared shared = new Shared();
Composing[] composing = { new Composing(shared),
new Composing(shared), new Composing(shared),
new Composing(shared), new Composing(shared) };
for(Composing c : composing)
c.dispose();
}
} /* Output:
Creating Shared 0
Creating Composing 0
Creating Composing 1
Creating Composing 2
Creating Composing 3
Creating Composing 4
disposing Composing 0
disposing Composing 1
disposing Composing 2
disposing Composing 3
disposing Composing 4
Disposing Shared 0
*///:~
```

### 8.3.3 构造器内部的多态方法行为

如果一个构造器内部调用正在构造的对象的某个动态绑定方法，则会怎样？

如果要调用构造器内部的一个动态绑定方法，就要用到那个方法的被覆盖后的定义。然而这个调用的效果可能相当难以预料，因为被覆盖的方法在对象被完全构造之前就会被调用，可能造成一些难以发现的隐藏错误。

构造器的工作是创建对象，在构造器内部，整个对象可能只是部分形成——我们只知道基类对象已经进行初始化。如果我们在构造器中调用某个方法，可能这个方法所操纵的成员还未初始化——这会导致灾难。

示例：

```
//: polymorphism/PolyConstructors.java
// Constructors and polymorphism
// don't produce what you might expect.
import static net.mindview.util.Print.*;

class Glyph {
  void draw() { print("Glyph.draw()"); }
  Glyph() {
    print("Glyph() before draw()");
    draw();
    print("Glyph() after draw()");
  }
}	

class RoundGlyph extends Glyph {
  private int radius = 1;
  RoundGlyph(int r) {
    radius = r;
    print("RoundGlyph.RoundGlyph(), radius = " + radius);
  }
  void draw() {
    print("RoundGlyph.draw(), radius = " + radius);
  }
}	

public class PolyConstructors {
  public static void main(String[] args) {
    new RoundGlyph(5);
  }
} /* Output:
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
*///:~
```

**初始化顺序的实质**：

- 在其他任何事物发生之前，将分配给对象的存储空间初始化成二进制的零
- 如前所述那样调用基类构造器。此时，调用备覆盖后的draw()方法（要在调用RoundGlyph构造器之前调用），由于步骤一的缘故，此时发现radius的值为0
- 按照声明的顺序调用成员的初始化方法
- 调用导出类的构造器主体。



**编写构造器的准则**：

用尽可能简单的方法使对象进入正常状态；如果可以的话，避免调用其他方法。**构造器中唯一安全调用的那些方法是基类中的final方法**。

## 8.4 协变返回类型

**Java SE5**中添加了协变返回类型，它表示**在导出类中的被覆盖方法可以返回基类方法的返回类型的某种导出类型。**

<font color=red>**示例**</font>：

```
//: polymorphism/CovariantReturn.java

class Grain {
  public String toString() { return "Grain"; }
}

class Wheat extends Grain {
  public String toString() { return "Wheat"; }
}

class Mill {
  Grain process() { return new Grain(); }
}
class WheatMill extends Mill {
  Wheat process() { return new Wheat(); }
}

public class CovariantReturn {
  public static void main(String[] args) {
    Mill m = new Mill();
    Grain g = m.process();
    System.out.println(g);
    m = new WheatMill();
    g = m.process();
    System.out.println(g);
  }
} /* Output:
Grain
Wheat
*///:~
```

<font color=red>**Java SE5与Java较早版本之间的区别的主要差异**</font>是较早版本将强制<font color=red>**process()的覆盖版本必须返回Grain，而不能返回Wheat，**</font>尽管Wheat是从Grain导出的，因而也应该是一种合法的返回类型。协变返回类型允许返回更具体的Wheat类型。

## 8.5 用继承进行设计

首先选择组合，尤其是在不知道使用哪一种方式时。组合更加灵活，可以动态选择类型。

示例：

```
//: polymorphism/Transmogrify.java
// Dynamically changing the behavior of an object
// via composition (the "State" design pattern).
import static net.mindview.util.Print.*;

class Actor {
  public void act() {}
}

class HappyActor extends Actor {
  public void act() { print("HappyActor"); }
}

class SadActor extends Actor {
  public void act() { print("SadActor"); }
}

class Stage {
  private Actor actor = new HappyActor();
  public void change() { actor = new SadActor(); }
  public void performPlay() { actor.act(); }
}

public class Transmogrify {
  public static void main(String[] args) {
    Stage stage = new Stage();
    stage.performPlay();
    stage.change();
    stage.performPlay();
  }
} /* Output:
HappyActor
SadActor
*///:~

```

Stage对象包含一个对Actor的引用，儿Actor被初始化为HappyActor对象。这意味这performPlay()会产生某种特殊行为。引用在运行时可以与另一个不同的对象重新绑定起来，所以SadActor对象的引用可以在actor中被替代，然后由performPlay()产生的行为也随之改变。这样一来，我们在运行期间获得了动态灵活性（也称状态模式）)

<font color=red>**通用准则**</font>：**用继承表达行为间的差异，用字段表达状态上的变化**。

上述例子：通过继承得到了两个不同的类，用于表达act()发发的差异；而Stage通过运用组合使自己状态发生变化，这种状态的改变也就产生了行为的改变。

### 8.5.1 纯继承与扩展

纯继承就是纯粹的将基类中已经建立的方法才可以在导出类中被覆盖。这被称为纯粹的“**is-a**”关系。因为一个类的接口已经确定了它应该是什么样，继承可以确保所有导出类具有基类的接口，绝对不会少，导出类也将具有基类一样的接口。可以看作是一个纯粹的替代，导出类完全替代基类。

![纯继承](http://img.blog.csdn.net/20171109153040806?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

还有一种是他有着和基类相同的接口，但是它还具有额外方法实现其他特性，这个可以称作为“is-like-a”关系。

![导出类扩展](http://img.blog.csdn.net/20171109153841634?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

对于上述这种方法，有一种缺点就是导出类中扩展的接口不能被基类访问，因此，一旦我们向上转型了，就不能调用那些扩展方法。

### 8.5.2 向下转型和运行时类型识别

向上转型会丢失具体类型信息，可以通过向下转型——基类到导出类——获取类型信息。**向上转型是安全的，**因为基类不会具有大于导出类的接口，因此通过基类接口发送的消息保证都能被接受，但是对于向下转型，例如无法知道一个“几何形状”它确实是一个“圆”，它可以是“正方形”，“三角形”等等。

要解决这个问题，必须有某种方法来保证向下转型的安全性，不至于转型到一种错误类型，进而发出对象无法接受的消息。为了解决这个问题，java中提供了**instacneof**操作符，用来确保向下转型的正确性。

**x instanceof A：检验x是否为类A的对象，返回值为boolean型。**
<font color=red>**即对象a instanceof 类A 判断对象a是否是类A的一个实例，是的话返回true。**</font>

要求x所属的类与类A必须是子类和父类的关系，否则编译错误。
**<font color=red>如果x属于类A的子类B，x instanceof  A值也为true</font>**。

```
public class Main{
	int ss=1;
	public static void main(String[] args) {
		Main m=new Subbclass();
		if(m instanceof Subclass){
			Subclass t = (Subclass)m;
			t.getinfor();
			
		}else if(m instanceof Subbclass){
			Subbclass t=(Subbclass)m;
			t.getinfor();
		}
		
	}
	void getinfor(){
		System.out.println("Main");
	}
}
class Subclass extends Main{
	
	Subclass(){
		super();
		
	}
	void getinfor(){
		System.out.println("Subclass");
	}
	
}
class Subbclass extends Main{
	
	Subbclass(){
		super();
		
	}
	void getinfor(){
		System.out.println("Subbclass");
	}
	
}/*output:
Subbclass
*/
```

从父类到子类的类型转换必须通过强制类型转换实现，无继承关系的引用类型间的转换是非法的。

![子父类类型转换](http://img.blog.csdn.net/20171109160152149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 第9章 接口

接口和内部类为我们提供了一种将接口和实现分离的更加结构化的方法。在学习接口之前，必须学习**抽象类，它是普通的类和接口之间的中庸之道。**

## 9.1 抽象类和抽象方法

随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。我们希望通过父类这个通过接口操纵一系列类。类的设计应该保证父类和子类能够共享特征。有时将一个父类设计得非常抽象，以至于<font color=red>**它没有具体的实例**</font>，这样的类叫做**抽象类**,或者**抽象基类**。

1. 用abstract关键字来修饰一个类时，这个类叫做抽象类；
 2. 用abstract来修饰一个方法时，该方法叫做抽象方法。（	**抽象方法**：只有方法的声明，没有方法的实现。以分号结束：`abstract int abstractMethod( int a );`）
 3. 含有抽象方法的类必须被声明为抽象类。（否则，编译器会报错。）
 4. 抽象类不能被实例化。**抽象类是用来被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。<font color=red>若没有重写全部的抽象方法，仍为抽象类。</font>**
 5. **<font color=red>不能用abstract修饰属性、私有方法、构造器、静态方法、final的方法。即private，static，final。</font>**

（即使不能被实例化，但是还是有构造器）抽象方法所在的类一定是抽象类.抽象类中可以没有抽象方法。也可以同时有抽象方法，也可以有实体方法。

示例：

```
abstract class Main{
	private int i;
	public abstract void getInform();
	public void print(){
		System.out.println("Main");
	}
}
class Subclass extends Main{
	public void getInform(){
		System.out.println("Subclass");
	}
}
```

## 9.2 接口

**interface**关键字使抽象的概念更向前迈进。abstract关键字允许人们在类中创建一个或多个没有任何定义的方法——提供了接口部分，但是没有提供任何具体的实现部分，这些实现由此类的继承者们实现。interface这个关键字产生了一个**完全抽象的类**，他根本就没有提供任何具体实现，它允许创建者**确定方法名，参数列表和返回类型**，但是没有任何方法体。接口只提供了形式，并且**接口提供了多重继承**，总的来说就是：

  1. 有时必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是，Java不支持多重继承。有了接口，就可以得到多重继承的效果。
  2. 接口(interface)是<font color=red>**抽象方法**</font>和<font color=red>**常量值**</font>的定义的集合
  3. 从本质上讲，**接口是一种特殊的抽象类**，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现。
  4. 一个类可以实现多个接口，接口也可以继承其它接口**:`class A impelments InterfaceB,InterfaceC{}`**

<font color=red>**接口的特点：**</font>

1. 用关键字interfacet替代class来定义。可添加修饰符
2. 接口中的所有成员变量都默认是由**public static final**修饰的。
3. 接口中的所有方法都默认是由public abstract修饰的。
4. **没有构造器**并且采用多继承机制。

示例：

```
public interface Runner {
	int ID = 1;
	void start();
	public void run();
	void stop();
}
```

等价于

```
public interface Runner {
    public static final int ID = 1;
    public abstract void start();
    public abstract void run();
    public abstract void stop();
    }
```

在接口中public static final 或 public abstract 都是默认，可以缺省默认。
实现接口的类中必须提供接口中**所有方法**的具体实现内容，方可实例化。否则，仍为抽象类。接口的主要用途就是被实现类实现。（面向接口编程）。与继承关系类似，接口与实现类之间**存在多态性**

示例：

```
//: interfaces/music5/Music5.java
// Interfaces.
package interfaces.music5;
import polymorphism.music.Note;
import static net.mindview.util.Print.*;

interface Instrument {
  // Compile-time constant:
  int VALUE = 5; // static & final
  // Cannot have method definitions:
  void play(Note n); // Automatically public
  void adjust();
}

class Wind implements Instrument {
  public void play(Note n) {
    print(this + ".play() " + n);
  }
  public String toString() { return "Wind"; }
  public void adjust() { print(this + ".adjust()"); }
}

class Percussion implements Instrument {
  public void play(Note n) {
    print(this + ".play() " + n);
  }
  public String toString() { return "Percussion"; }
  public void adjust() { print(this + ".adjust()"); }
}

class Stringed implements Instrument {
  public void play(Note n) {
    print(this + ".play() " + n);
  }
  public String toString() { return "Stringed"; }
  public void adjust() { print(this + ".adjust()"); }
}

class Brass extends Wind {
  public String toString() { return "Brass"; }
}	

class Woodwind extends Wind {
  public String toString() { return "Woodwind"; }
}

public class Music5 {
  // Doesn't care about type, so new types
  // added to the system still work right:
  static void tune(Instrument i) {
    // ...
    i.play(Note.MIDDLE_C);
  }
  static void tuneAll(Instrument[] e) {
    for(Instrument i : e)
      tune(i);
  }	
  public static void main(String[] args) {
    // Upcasting during addition to the array:
    Instrument[] orchestra = {
      new Wind(),
      new Percussion(),
      new Stringed(),
      new Brass(),
      new Woodwind()
    };
    tuneAll(orchestra);
  }
} /* Output:
Wind.play() MIDDLE_C
Percussion.play() MIDDLE_C
Stringed.play() MIDDLE_C
Brass.play() MIDDLE_C
Woodwind.play() MIDDLE_C
*///:~
```

**一个类可以实现多个无关的接口**

```
interface Runner { public void run();}
interface Swimmer {public double swim();}
class Creator{public int eat(){…}} 
class Man extends Creator implements Runner ,Swimmer{
		public void run() {……}
		public double swim()  {……}
		public int eat() {……}
}

```

与继承关系类似，接口与实现类之间存在多态性
```
public class Test{
	public static void main(String args[]){
		Test t = new Test();
		Man m = new Man();
		t.m1(m);
		t.m2(m);
		t.m3(m);
	}
	public String m1(Runner f) { f.run(); }
	public void  m2(Swimmer s) {s.swim();}
	public void  m3(Creator a) {a.eat();}
}
```

## 9.3 完全解耦

<font color=red>**示例**</font>：

```
//: interfaces/classprocessor/Apply.java
package interfaces.classprocessor;
import java.util.*;
import static net.mindview.util.Print.*;

class Processor {
  public String name() {
    return getClass().getSimpleName();
  }
  Object process(Object input) { return input; }
}	

class Upcase extends Processor {
  String process(Object input) { // Covariant return
    return ((String)input).toUpperCase();
  }
}

class Downcase extends Processor {
  String process(Object input) {
    return ((String)input).toLowerCase();
  }
}

class Splitter extends Processor {
  String process(Object input) {
    // The split() argument divides a String into pieces:
    return Arrays.toString(((String)input).split(" "));
  }
}	

public class Apply {
  public static void process(Processor p, Object s) {
    print("Using Processor " + p.name());
    print(p.process(s));
  }
  public static String s =
    "Disagreement with beliefs is by definition incorrect";
  public static void main(String[] args) {
    process(new Upcase(), s);
    process(new Downcase(), s);
    process(new Splitter(), s);
  }
} /* Output:
Using Processor Upcase
DISAGREEMENT WITH BELIEFS IS BY DEFINITION INCORRECT
Using Processor Downcase
disagreement with beliefs is by definition incorrect
Using Processor Splitter
[Disagreement, with, beliefs, is, by, definition, incorrect]
*///:~
```

 创建一个**能够根据所传递的参数对象的不同而具有不同行为的方法**，被称为<font color =red>**策略设计模式**</font>。

这类方法包含所要执行的算法中固定不变的部分，而“策略”包含变化的部分。策略就是传递进去的参数对象，它包含要执行的代码。这里processor对象就是一个策略，在main()中可以看到三种不同类型的策略应用到了String类型的s对象上。

耦合过紧：

```
//: interfaces/filters/Waveform.java
package interfaces.filters;

public class Waveform {
  private static long counter;
  private final long id = counter++;
  public String toString() { return "Waveform " + id; }
} ///:~
```

```
//: interfaces/filters/Filter.java
package interfaces.filters;

public class Filter {
  public String name() {
    return getClass().getSimpleName();
  }
  public Waveform process(Waveform input) { return input; }
} ///:~
```

```
//: interfaces/filters/LowPass.java
package interfaces.filters;

public class LowPass extends Filter {
  double cutoff;
  public LowPass(double cutoff) { this.cutoff = cutoff; }
  public Waveform process(Waveform input) {
    return input; // Dummy processing
  }
} ///:~
```

```
//: interfaces/filters/HighPass.java
package interfaces.filters;

public class HighPass extends Filter {
  double cutoff;
  public HighPass(double cutoff) { this.cutoff = cutoff; }
  public Waveform process(Waveform input) { return input; }
} ///:~
```

```
//: interfaces/filters/BandPass.java
package interfaces.filters;

public class BandPass extends Filter {
  double lowCutoff, highCutoff;
  public BandPass(double lowCut, double highCut) {
    lowCutoff = lowCut;
    highCutoff = highCut;
  }
  public Waveform process(Waveform input) { return input; }
} ///:~
```

## 9.4 Java中的多重继承

如果要从一个非接口的类继承，那么只能从一个类中继承。其余的基元素都必须是接口。需要将所有的接口名都置于implements关键字之后，用逗号将它们一一隔开。具体类必须放在前面，后面跟着接口（否则编译回报错）。

使用接口防止客户端程序员创建该类的对象，确保仅仅只是建立了一个接口。<font color=red>**如果知道某事物应该成为一个基类，那么第一选择应该是使它成为一个接口**。</font>

## 9.5 通过继承来扩展接口

通过继承，可以很容易的在接口中添加新的方法声明，还可以通过继承在新接口中组合数个接口。

示例：

```
//: interfaces/HorrorShow.java
// Extending an interface with inheritance.

interface Monster {
  void menace();
}

interface DangerousMonster extends Monster {
  void destroy();
}

interface Lethal {
  void kill();
}

class DragonZilla implements DangerousMonster {
  public void menace() {}
  public void destroy() {}
}	

interface Vampire extends DangerousMonster, Lethal {
  void drinkBlood();
}

class VeryBadVampire implements Vampire {
  public void menace() {}
  public void destroy() {}
  public void kill() {}
  public void drinkBlood() {}
}	

public class HorrorShow {
  static void u(Monster b) { b.menace(); }
  static void v(DangerousMonster d) {
    d.menace();
    d.destroy();
  }
  static void w(Lethal l) { l.kill(); }
  public static void main(String[] args) {
    DangerousMonster barney = new DragonZilla();
    u(barney);
    v(barney);
    Vampire vlad = new VeryBadVampire();
    u(vlad);
    v(vlad);
    w(vlad);
  }
} ///:~
```

### 9.5.1 组合接口时的名字冲突

实现多继承时，可能会碰到一个小陷阱，例如`class A implements B,C{}`其中接口B和接口C的接口方法相同，则不会有什么问题，但是它们的名称或返回类型不同，就会出现问题。例如：

```
//: interfaces/InterfaceCollision.java
package interfaces;

interface I1 { void f(); }
interface I2 { int f(int i); }
interface I3 { int f(); }
class C { public int f() { return 1; } }

class C2 implements I1, I2 {
  public void f() {}
  public int f(int i) { return 1; } // overloaded
}

class C3 extends C implements I2 {
  public int f(int i) { return 1; } // overloaded
}

class C4 extends C implements I3 {
  // Identical, no problem:
  public int f() { return 1; }
}

// Methods differ only by return type:
//! class C5 extends C implements I1 {}
//! interface I4 extends I1, I3 {} ///:~
```

这时候困难就来了，覆盖，实现和重载搅和在一起了，重载类型仅通过返回类型是区分不开的。所以在组合的不同接口中使用相同的方法名通常会造成代码可读性的混乱，需要**尽量避免**。

## 9.6 适配接口

在接口中允许同一个接口具有不同的具体实现。在简单的情况中，它的体现形式通常是一个接受接口类型的方法，而该接口的实现和向该方法传递的对象则取决于方法的使用者。一种常见的用法就是**策略设计模式**与**适配器模式**。





## 9.7 接口中的域

放入接口的任何域都自动是static和final的。

### 9.7.1 初始化接口中域

接口中定义的域**不能是”空final“**，但是可以被非常量表达式初始化。域是static的，它们就可以在类第一次被加载时初始化。示例：

```
//: interfaces/RandVals.java
// Initializing interface fields with
// non-constant initializers.
import java.util.*;

public interface RandVals {
  Random RAND = new Random(47);
  int RANDOM_INT = RAND.nextInt(10);
  long RANDOM_LONG = RAND.nextLong() * 10;
  float RANDOM_FLOAT = RAND.nextLong() * 10;
  double RANDOM_DOUBLE = RAND.nextDouble() * 10;
} ///:~
```

## 9.8 嵌套接口

接口可以嵌套在类或其他接口中。示例：

```
//: interfaces/nesting/NestingInterfaces.java
package interfaces.nesting;

class A {
  interface B {
    void f();
  }
  public class BImp implements B {
    public void f() {}
  }
  private class BImp2 implements B {
    public void f() {}
  }
  public interface C {
    void f();
  }
  class CImp implements C {
    public void f() {}
  }	
  private class CImp2 implements C {
    public void f() {}
  }
  private interface D {
    void f();
  }
  private class DImp implements D {
    public void f() {}
  }
  public class DImp2 implements D {
    public void f() {}
  }
  public D getD() { return new DImp2(); }
  private D dRef;
  public void receiveD(D d) {
    dRef = d;
    dRef.f();
  }
}	

interface E {
  interface G {
    void f();
  }
  // Redundant "public":
  public interface H {
    void f();
  }
  void g();
  // Cannot be private within an interface:
  //! private interface I {}
}	

public class NestingInterfaces {
  public class BImp implements A.B {
    public void f() {}
  }
  class CImp implements A.C {
    public void f() {}
  }
  // Cannot implement a private interface except
  // within that interface's defining class:
  //! class DImp implements A.D {
  //!  public void f() {}
  //! }
  class EImp implements E {
    public void g() {}
  }
  class EGImp implements E.G {
    public void f() {}
  }
  class EImp2 implements E {
    public void g() {}
    class EG implements E.G {
      public void f() {}
    }
  }	
  public static void main(String[] args) {
    A a = new A();
    // Can't access A.D:
    //! A.D ad = a.getD();
    // Doesn't return anything but A.D:
    //! A.DImp2 di2 = a.getD();
    // Cannot access a member of the interface:
    //! a.getD().f();
    // Only another A can do anything with getD():
    A a2 = new A();
    a2.receiveD(a.getD());
  }
} ///:~
```

private接口不能在定义它的类之外被实现。

## 9.9 接口与工厂

接口是实现多重继承的途径，而生成遵循某个接口的对象的典型方式就是**工厂方法设计模式**。

工厂对象上调用的是创建方法，而该工厂对象将生成接口的某个实现的对象。

<font color=red>**示例**</font>：

```
//: interfaces/Factories.java
import static net.mindview.util.Print.*;

interface Service {
  void method1();
  void method2();
}

interface ServiceFactory {
  Service getService();
}

class Implementation1 implements Service {
  Implementation1() {} // Package access
  public void method1() {print("Implementation1 method1");}
  public void method2() {print("Implementation1 method2");}
}	

class Implementation1Factory implements ServiceFactory {
  public Service getService() {
    return new Implementation1();
  }
}

class Implementation2 implements Service {
  Implementation2() {} // Package access
  public void method1() {print("Implementation2 method1");}
  public void method2() {print("Implementation2 method2");}
}

class Implementation2Factory implements ServiceFactory {
  public Service getService() {
    return new Implementation2();
  }
}	

public class Factories {
  public static void serviceConsumer(ServiceFactory fact) {
    Service s = fact.getService();
    s.method1();
    s.method2();
  }
  public static void main(String[] args) {
    serviceConsumer(new Implementation1Factory());
    // Implementations are completely interchangeable:
    serviceConsumer(new Implementation2Factory());
  }
} /* Output:
Implementation1 method1
Implementation1 method2
Implementation2 method1
Implementation2 method2
*///:~
```

```
//: interfaces/Games.java
// A Game framework using Factory Methods.
import static net.mindview.util.Print.*;

interface Game { boolean move(); }
interface GameFactory { Game getGame(); }

class Checkers implements Game {
  private int moves = 0;
  private static final int MOVES = 3;
  public boolean move() {
    print("Checkers move " + moves);
    return ++moves != MOVES;
  }
}

class CheckersFactory implements GameFactory {
  public Game getGame() { return new Checkers(); }
}	

class Chess implements Game {
  private int moves = 0;
  private static final int MOVES = 4;
  public boolean move() {
    print("Chess move " + moves);
    return ++moves != MOVES;
  }
}

class ChessFactory implements GameFactory {
  public Game getGame() { return new Chess(); }
}	

public class Games {
  public static void playGame(GameFactory factory) {
    Game s = factory.getGame();
    while(s.move())
      ;
  }
  public static void main(String[] args) {
    playGame(new CheckersFactory());
    playGame(new ChessFactory());
  }
} /* Output:
Checkers move 0
Checkers move 1
Checkers move 2
Chess move 0
Chess move 1
Chess move 2
Chess move 3
*///:~
```

# 第10章 内部类

可以将一个类的定义放在另一个类的定义内部，这就是**内部类**。**内部类与组合是两种完全不同的概念**。内部类看起来就像是一种代码隐藏机制：将类置于其他类的内部。但是内部类远不止于此，它了解外围类，并能与之通信；而且你用内部类写出的代码更加清晰优雅。

## 10.1 创建内部类

创建内部类就是把类的定义治愈外围类里面。

示例：

```java
public class Parcel1 {
  class Contents {
    private int i = 11;
    public int value() { return i; }
  }
  class Destination {
    private String label;
    Destination(String whereTo) {
      label = whereTo;
    }
    String readLabel() { return label; }
  }	
  // Using inner classes looks just like
  // using any other class, within Parcel1:
  public void ship(String dest) {
    Contents c = new Contents();
    Destination d = new Destination(dest);
    System.out.println(d.readLabel());
  }
  public static void main(String[] args) {
    Parcel1 p = new Parcel1();
    p.ship("Tasmania");
  }
} /* Output:
Tasmania
*///:~
```

在外部类的非静态方法之外的任意位置创建某个内部类的对象，如示例所示，具体地指明这个对象的类型：OuterClassName.InnerClassName

```java
//: innerclasses/Parcel2.java
// Returning a reference to an inner class.

public class Parcel2 {
  class Contents {
    private int i = 11;
    public int value() { return i; }
  }
  class Destination {
    private String label;
    Destination(String whereTo) {
      label = whereTo;
    }
    String readLabel() { return label; }
  }
  public Destination to(String s) {
    return new Destination(s);
  }
  public Contents contents() {
    return new Contents();
  }
  public void ship(String dest) {
    Contents c = contents();
    Destination d = to(dest);
    System.out.println(d.readLabel());
  }
  public static void main(String[] args) {
    Parcel2 p = new Parcel2();
    p.ship("Tasmania");
    Parcel2 q = new Parcel2();
    // Defining references to inner classes:
    Parcel2.Contents c = q.contents();
    Parcel2.Destination d = q.to("Borneo");
  }
} /* Output:
Tasmania
*///:~
```

## 10.2 链接到外部类

生成一个内部类对象时，此对象与制造它的外围对象之间就有了一种联系，**它能访问其外围对象的所有成员，而不需要任何特殊条件。此外，内部类还拥有其外围类的所有元素的访问权。**

```java
//: innerclasses/Sequence.java
// Holds a sequence of Objects.
interface Selector {
	boolean end();
	Object current();
	void next();
}
public class Sequence {
	private Object[] items;
	private int next = 0;
	public Sequence(int size) { items = new Object[size]; }
	public void add(Object x) {
	if(next < items.length)
		items[next++] = x;
	}
	private class SequenceSelector implements Selector {
    private int i = 0;
    public boolean end() { return i == items.length; }
    public Object current() { return items[i]; }
    public void next() { if(i < items.length) i++; }
}
public Selector selector() {
return new SequenceSelector();
}
public static void main(String[] args) {
Sequence sequence = new Sequence(10);
for(int i = 0; i < 10; i++)
sequence.add(Integer.toString(i));
Selector selector = sequence.selector();
while(!selector.end()) {
System.out.print(selector.current() + " ");
selector.next();
}
}
} /* Output:
0 1 2 3 4 5 6 7 8 9
*///:~
```

## 10.3 使用.this与.new

**如果需要<font color=red>生成对外部类对象的引用</font>，可以使用外部类名字后面紧跟圆点和this**。

示例：

```java
//: innerclasses/DotThis.java
// Qualifying access to the outer-class object.

public class DotThis {
  void f() { System.out.println("DotThis.f()"); }
  public class Inner {
    public DotThis outer() {
      return DotThis.this;
      // A plain "this" would be Inner's "this"
    }
  }
  public Inner inner() { return new Inner(); }
  public static void main(String[] args) {
    DotThis dt = new DotThis();
    DotThis.Inner dti = dt.inner();
    dti.outer().f();
  }
} /* Output:
DotThis.f()
*///:~
```

如果需要告知某些其他对象，去**创建其某个内部类的对象**。要实现此目的，必须在new表达式中提供对其他外部类对象的引用，这需要使用.new语法。

```java
//: innerclasses/DotNew.java
// Creating an inner class directly using the .new syntax.

public class DotNew {
  public class Inner {}
  public static void main(String[] args) {
    DotNew dn = new DotNew();
    DotNew.Inner dni = dn.new Inner();
  }
} ///:~
```

**在拥有外部类对象之前是不可能创建内部类对象的**。但是如果创建的是嵌套类（静态内部类），那么它就不需要对外部类对象的引用。

## 10.4 内部类与向上转型

内部类向上转型为其基类，尤其是转型为一个接口时，某个接口的实现能够完全不可见且不可用，方面隐藏实现细节。

示例：

```java
public interface Destination{
	String readLabel();
}
public interface Contents{
	int value();
}
```

```java
//: innerclasses/TestParcel.java

class Parcel4 {
  private class PContents implements Contents {
    private int i = 11;
    public int value() { return i; }
  }
  protected class PDestination implements Destination {
    private String label;
    private PDestination(String whereTo) {
      label = whereTo;
    }
    public String readLabel() { return label; }
  }
  public Destination destination(String s) {
    return new PDestination(s);
  }
  public Contents contents() {
    return new PContents();
  }
}

public class TestParcel {
  public static void main(String[] args) {
    Parcel4 p = new Parcel4();
    Contents c = p.contents();
    Destination d = p.destination("Tasmania");
    // Illegal -- can't access private class:
    //! Parcel4.PContents pc = p.new PContents();
  }
} ///:~
```

## 10.5 在方法和作用域内的内部类

**例如在一个方法里或者作用域内定义内部类。**这被称之为<font color=red>**局部内部类**</font>。这种做法的缘由是：实现了某类型的接口，可以创建并返回对其的引用或者想要解决一个复杂问题，创建一个类来辅助但是又不希望这个类是公共可用的时候。

示例：

```
//: innerclasses/Parcel5.java
// Nesting a class within a method.
public class Parcel5 {
  public Destination destination(String s) {
    class PDestination implements Destination {
      private String label;
      private PDestination(String whereTo) {
        label = whereTo;
      }
      public String readLabel() { return label; }
    }
    return new PDestination(s);
  }
  public static void main(String[] args) {
    Parcel5 p = new Parcel5();
    Destination d = p.destination("Tasmania");
  }
} ///:~
```

作用域内嵌入一个内部类：

```
//: innerclasses/Parcel6.java
// Nesting a class within a scope.

public class Parcel6 {
  private void internalTracking(boolean b) {
    if(b) {
      class TrackingSlip {
        private String id;
        TrackingSlip(String s) {
          id = s;
        }
        String getSlip() { return id; }
      }
      TrackingSlip ts = new TrackingSlip("slip");
      String s = ts.getSlip();
    }
    // Can't use it here! Out of scope:
    //! TrackingSlip ts = new TrackingSlip("x");
  }	
  public void track() { internalTracking(true); }
  public static void main(String[] args) {
    Parcel6 p = new Parcel6();
    p.track();
  }
} ///:~
```

内部类被嵌入在if语句的作用域内，这个类与其他类都一起被编译了，但是在作用域之外，这个类是不可用的，除此之外与其他类一样。

## 10.6 匿名内部类

示例：

```
//: innerclasses/Parcel7.java
// Returning an instance of an anonymous inner class.

public class Parcel7 {
  public Contents contents() {
    return new Contents() { // Insert a class definition
      private int i = 11;
      public int value() { return i; }
    }; // Semicolon required in this case
  }
  public static void main(String[] args) {
    Parcel7 p = new Parcel7();
    Contents c = p.contents();
  }
} ///:~
```

原型示例：

```
//: innerclasses/Parcel7b.java
// Expanded version of Parcel7.java

public class Parcel7b {
  class MyContents implements Contents {
    private int i = 11;
    public int value() { return i; }
  }
  public Contents contents() { return new MyContents(); }
  public static void main(String[] args) {
    Parcel7b p = new Parcel7b();
    Contents c = p.contents();
  }
} ///:~
```

第一个示例中contests()方法**将返回值的生成与表示这个返回值的类定义结合在一起**，这种语法是指：“**<font color=red>创建一个继承自Contents的匿名内部类的对象”，通过new表达式返回的引用被自动向上转型为对Contents的引用</font>**

**<font color=red>如果定义一个匿名内部类，希望它使用一个其在外部定义的对象，那么编译器会要求其参数引用是final的，否则会得到编译错误信息</font>**。例如上述的示例，参数String dest必须有final修饰。

**如果想要在匿名内部类中做一些构造器的行为**，但是匿名内部类不可能有命名构造器，可以通过**实例初始化**，就能够达到为匿名内部类创建一个构造器的效果。

示例：

```
//: innerclasses/Parcel10.java
// Using "instance initialization" to perform
// construction on an anonymous inner class.

public class Parcel10 {
  public Destination
  destination(final String dest, final float price) {
    return new Destination() {
      private int cost;
      // Instance initialization for each object:
      {
        cost = Math.round(price);
        if(cost > 100)
          System.out.println("Over budget!");
      }
      private String label = dest;
      public String readLabel() { return label; }
    };
  }	
  public static void main(String[] args) {
    Parcel10 p = new Parcel10();
    Destination d = p.destination("Tasmania", 101.395F);
  }
} /* Output:
Over budget!
*///:~
```

## 10.7 嵌套类

**不需要内部类与外围类对象之间有联系，那么可以将内部类声明为static，这通常称之为<font color=red>嵌套类</font>**。普通的内部类隐式的保存了一个引用，指向创建它的外围类对象，然而内部类是static的时候，就不是这样了。意味着：

 - 创建嵌套类对象，不需要外围类的对象
 - **不能从嵌套类的对象中访问非静态的外围类对象**

普通内部类不能有static数据和static字段，也不能包含嵌套类，但是嵌套类可以包含这些东西。

```
public class E18_NestedClass {
164 Thinking in Java, 4th Edition Annotated Solution Guide
static class Nested {
void f() { System.out.println("Nested.f"); }
}
public static void main(String args[]) {
Nested ne = new Nested();
ne.f();
}
}
class Other {
// Specifying the nested type outside
// the scope of the class:
void f() {
E18_NestedClass.Nested ne =
new E18_NestedClass.Nested();
}
} /* Output:
Nested.f
*///:~
```

### 10.7.1 接口内部的类

嵌套类可以作为接口的一部分，**甚至可以在内部类实现外围接口**。

示例：

```
//: innerclasses/ClassInInterface.java
// {main: ClassInInterface$Test}

public interface ClassInInterface {
  void howdy();
  static class Test implements ClassInInterface {
    public void howdy() {
      System.out.println("Howdy!");
    }
    public static void main(String[] args) {
      new Test().howdy();
    }
  }
} /* Output:
Howdy!
*///:~
```

可以使用嵌套类放置测试代码

###  10.7.2  从多层嵌套类中访问外部类成员

一个内部类不管被嵌套多少层，都可以透明地访问它所嵌入的外围类的所有成员
示例：

```
//: innerclasses/MultiNestingAccess.java
// Nested classes can access all members of all
// levels of the classes they are nested within.

class MNA {
  private void f() {}
  class A {
    private void g() {}
    public class B {
      void h() {
        g();
        f();
      }
    }
  }
}	

public class MultiNestingAccess {
  public static void main(String[] args) {
    MNA mna = new MNA();
    MNA.A mnaa = mna.new A();
    MNA.A.B mnaab = mnaa.new B();
    mnaab.h();
  }
} ///:~
```

## 10.8 为什么需要内部类

**内部类实现一个接口与外围类实现这个接口的区别是**：后者不是总能享用到接口带来的方便，有时需要用到接口的实现。所以使用内部类最主要的原因是：每个内部类都能独立地继承自一个（接口）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类是没有影响的。（接口解决了部分问题，而内部类有效地实现了多重继承）

例如：**一个类中以某种方式实现两个接口，有两种选择，一种是使用单一类，另一种是使用内部类：**

**如果拥有的是抽象的类或具体的类，而不是接口，只能使用内部类才能够实现多重继承**

```
//: innerclasses/MultiImplementation.java
// With concrete or abstract classes, inner
// classes are the only way to produce the effect
// of "multiple implementation inheritance."
package innerclasses;

class D {}
abstract class E {}

class Z extends D {
  E makeE() { return new E() {}; }
}

public class MultiImplementation {
  static void takesD(D d) {}
  static void takesE(E e) {}
  public static void main(String[] args) {
    Z z = new Z();
    takesD(z);
    takesE(z.makeE());
  }
} ///:~
```

### 10.8.1 闭包与回调

**闭包（closure）**就是一个可调用的对象，它记录了一些信息，这些信息来自于创建它的作用域。通过这个定义，可以看出内部类是面向对象的闭包，因为它不仅包含外围类对象的信息，还自动拥有一个指向此外围类对象的引用，在此作用域内，内部类有权操作所有的成员，包括private成员。

通过内部类提供闭包的功能是回调优良的解决方法。

```
//: innerclasses/Callbacks.java
// Using inner classes for callbacks
package innerclasses;
import static net.mindview.util.Print.*;

interface Incrementable {
  void increment();
}

// Very simple to just implement the interface:
class Callee1 implements Incrementable {
  private int i = 0;
  public void increment() {
    i++;
    print(i);
  }
}	

class MyIncrement {
  public void increment() { print("Other operation"); }
  static void f(MyIncrement mi) { mi.increment(); }
}	

// If your class must implement increment() in
// some other way, you must use an inner class:
class Callee2 extends MyIncrement {
  private int i = 0;
  public void increment() {
    super.increment();
    i++;
    print(i);
  }
  private class Closure implements Incrementable {
    public void increment() {
      // Specify outer-class method, otherwise
      // you'd get an infinite recursion:
      Callee2.this.increment();
    }
  }
  Incrementable getCallbackReference() {
    return new Closure();
  }
}	

class Caller {
  private Incrementable callbackReference;
  Caller(Incrementable cbh) { callbackReference = cbh; }
  void go() { callbackReference.increment(); }
}

public class Callbacks {
  public static void main(String[] args) {
    Callee1 c1 = new Callee1();
    Callee2 c2 = new Callee2();
    MyIncrement.f(c2);
    Caller caller1 = new Caller(c1);
    Caller caller2 = new Caller(c2.getCallbackReference());
    caller1.go();
    caller1.go();
    caller2.go();
    caller2.go();
  }	
} /* Output:
Other operation
1
1
2
Other operation
2
Other operation
3
*///:~
```

上述例子展示了外围类实现一个接口与内部类实现该接口之间的差别。Callee1是简单的实现方式，Callee2继承了MyIncrement，后者已经有了一个不同的increment()方法，如果Callee2继承了MyIncrement，就不能为了Incrementable的用途而覆盖increment()方法，于是就只能使用内部类，独立地实现Incrementable。

### 10.8.2 内部类与控制框架

**应用控制框架**就是被设计用以解决某类特定问题的一个类或一组类。要运用某个应用程序框架，通常是继承一个或多个类，并覆盖某些方法。在覆盖后的方法中，编写代码定制应用程序框架提供的提供的通用解决方案，以解决特定的问题。

**控制框架**是一类特殊的应用程序框架，用来解决响应事件的需求。主要用来响应事件的系统被称作为**事件驱动系统**。应用程序设计中常见的问题之一是图形用户接口（GUI），几乎完全是事件驱动的系统。**Java Swing库就是一个控制框架，优雅地解决了GUI为题，并使用了大量的内部类。







## 10.9 内部类的继承

因为内部类的构造器必须连接到指向其外围类对象的引用，所以在继承内部类的时候，必须使用特殊的语法来说清它们之间的关系。

示例：

```
//: innerclasses/InheritInner.java
// Inheriting an inner class.

class WithInner {
  class Inner {}
}

public class InheritInner extends WithInner.Inner {
  //! InheritInner() {} // Won't compile
  InheritInner(WithInner wi) {
    wi.super();
  }
  public static void main(String[] args) {
    WithInner wi = new WithInner();
    InheritInner ii = new InheritInner(wi);
  }
} ///:~
```

InheritInner只继承自内部类，不是外围类，但是当要生成一个构造器时，必须在构造器内使用如下语法：`enclosingClassReference.super()`
这样才提供了必要的引用，编译才会通过。

## 10.10 内部类可以被覆盖吗

创建了一个内部类，然后继承其外围类并重新定义了此内部类，内部类可以被覆盖吗？

示例：

```
//: innerclasses/BigEgg.java
// An inner class cannot be overriden like a method.
import static net.mindview.util.Print.*;

class Egg {
  private Yolk y;
  protected class Yolk {
    public Yolk() { print("Egg.Yolk()"); }
  }
  public Egg() {
    print("New Egg()");
    y = new Yolk();
  }
}	

public class BigEgg extends Egg {
  public class Yolk {
    public Yolk() { print("BigEgg.Yolk()"); }
  }
  public static void main(String[] args) {
    new BigEgg();
  }
} /* Output:
New Egg()
Egg.Yolk()
*///:~
```

覆盖不起作用。当继承了某个外围类的时候，内部类并没有发生什么特别神奇的变化。两个内部类是完全独立的两个实体，各自在各自的命名空间。

如果明确的继承某个内部类：

示例：

```
//: innerclasses/BigEgg2.java
// Proper inheritance of an inner class.
import static net.mindview.util.Print.*;

class Egg2 {
  protected class Yolk {
    public Yolk() { print("Egg2.Yolk()"); }
    public void f() { print("Egg2.Yolk.f()");}
  }
  private Yolk y = new Yolk();
  public Egg2() { print("New Egg2()"); }
  public void insertYolk(Yolk yy) { y = yy; }
  public void g() { y.f(); }
}	

public class BigEgg2 extends Egg2 {
  public class Yolk extends Egg2.Yolk {
    public Yolk() { print("BigEgg2.Yolk()"); }
    public void f() { print("BigEgg2.Yolk.f()"); }
  }
  public BigEgg2() { insertYolk(new Yolk()); }
  public static void main(String[] args) {
    Egg2 e2 = new BigEgg2();
    e2.g();
  }
} /* Output:
Egg2.Yolk()
New Egg2()
Egg2.Yolk()
BigEgg2.Yolk()
BigEgg2.Yolk.f()
*///:~
```

**如果一个类A继承了类B，类A的内部类继承了类B的内部类，则类A内部类的构造器可以不用像上述小结中内部类继承那样提供必要的引用。如果类A继承了类B但是内部类没有继承类B中内部类，那么两个内部类是两个独立的实体**。

## 10.11 局部内部类

局部内部类不能有访问说明符，因为它不是外围类的一部分，但是他可以访问当前代码块内的常量，以及此外围类的所有成员。

示例：

```
//: innerclasses/LocalInnerClass.java
// Holds a sequence of Objects.
import static net.mindview.util.Print.*;

interface Counter {
  int next();
}	

public class LocalInnerClass {
  private int count = 0;
  Counter getCounter(final String name) {
    // A local inner class:
    class LocalCounter implements Counter {
      public LocalCounter() {
        // Local inner class can have a constructor
        print("LocalCounter()");
      }
      public int next() {
        printnb(name); // Access local final
        return count++;
      }
    }
    return new LocalCounter();
  }	
  // The same thing with an anonymous inner class:
  Counter getCounter2(final String name) {
    return new Counter() {
      // Anonymous inner class cannot have a named
      // constructor, only an instance initializer:
      {
        print("Counter()");
      }
      public int next() {
        printnb(name); // Access local final
        return count++;
      }
    };
  }	
  public static void main(String[] args) {
    LocalInnerClass lic = new LocalInnerClass();
    Counter
      c1 = lic.getCounter("Local inner "),
      c2 = lic.getCounter2("Anonymous inner ");
    for(int i = 0; i < 5; i++)
      print(c1.next());
    for(int i = 0; i < 5; i++)
      print(c2.next());
  }
} /* Output:
LocalCounter()
Counter()
Local inner 0
Local inner 1
Local inner 2
Local inner 3
Local inner 4
Anonymous inner 5
Anonymous inner 6
Anonymous inner 7
Anonymous inner 8
Anonymous inner 9
*///:~
```

**使用局部内部类而非匿名内部类的理由**：

1. 需要一个已命名的构造器，或者需要重载构造器，匿名内部类只能用于实例初始化，
2. 需要不止一个内部类的对象。

## 10.12 内部类标识符

每个类都会产生一个.class文件，其中包含了如何创建该类型的对象的全部信息，内部类也必须生成一个.class文件，以包含它们的class对象信息，这些类文件的命名有严格的规则：外围类的名字，加上$,再加上内部类的名字。例如前述例子生成的.class文件有：
`Counter.class`
`LocalInnerClass$1.class`
`LocalInnerClass$1LocalCounter.class`
`LocalInnerClass.class`
如果内部类是匿名的，编译器会简单产生一个数字作为其标识符。

# 第11章 持有对象

**如果一个程序只包含固定数量的且生命周期都是已知的对象，那么这是一个非常简单的程序。**
通常程序都是根据运行时才知道的某些条件去创建新对象，在此之前，不会知道所需对象的数量，甚至不知道确切的类型，这时候就不能依靠创建命名的引用来持有每一个对象，因为你不知道有多少个引用。**java有多种方式保存对象，例如数组**。但是数组具有固定的尺寸，在更一般的情况下，写程序的时候并不知道将要需要多少个对象，后者更复杂的方式存储对象。
java实用类库中还提供了一套相当完整的容器来解决这一问题，其中基本的类型是**List、Set、Queue和Map**。这些对象类型称之为**集合类**。java类库中使用了**Collection**这个名字来指代该类库的一个特殊子集，一般使用容器来程序来它们。<font color=red>**这里介绍java容器类库的基本知识，以及典型用法的基本介绍。**</font>

## 11.1 泛型和类型安全的容器

使用java SE5之前的容器的一个主要问题是编译器允许你向容器中插入不正确的类型。例如考虑一个apple对象的容器，可以使用容器ArrayList。例如将apple对象和orange对象都放置在容器中，然后将它们取出。正常情况下，java编译器会报告警告信息，如果没有使用泛型的话。Apple类型和Orange类型是有区别的，除了都是Object之外没有任何共性，因为ArrayList保存的是Object，因此不仅可以通过ArrayList的add()方法将Apple对象放进这个容器，也可以添加Orange对象。但是当你使用ArrayList的get()方法取出来得到的是Object引用，不是Apple对象，必须要转型为Apple。当你试图将Orange转型为Apple时，会产生异常。**如果想要定义保存Apple对象的ArrayList，可以声明`ArrayList<Apple>`,其中尖括号括起来的是<font color=red>类型参数（可以有多个）</font>，他指定了这个容器实例可以保存的类型。通过使用泛型，可以在<font color =red>编译期间</font>防止将错误类型的对象放置在容器中。**

示例：

```
//: holding/ApplesAndOrangesWithGenerics.java
import java.util.*;

public class ApplesAndOrangesWithGenerics {
  public static void main(String[] args) {
    ArrayList<Apple> apples = new ArrayList<Apple>();
    for(int i = 0; i < 3; i++)
      apples.add(new Apple());
    // Compile-time error:
    // apples.add(new Orange());
    for(int i = 0; i < apples.size(); i++)
      System.out.println(apples.get(i).id());
    // Using foreach:
    for(Apple c : apples)
      System.out.println(c.id());
  }
} /* Output:
0
1
2
0
1
2
*///:~
```

制定了某个类型作为泛型参数时，并不仅限于只能将该确切类型的对象放置到容器中。**向上转型也可以像作用于其他类型一样作用于泛型**。

示例：

```
import java.util.*;

class GrannySmith extends Apple {}
class Gala extends Apple {}
class Fuji extends Apple {}
class Braeburn extends Apple {}

public class GenericsAndUpcasting {
  public static void main(String[] args) {
    ArrayList<Apple> apples = new ArrayList<Apple>();
    apples.add(new GrannySmith());
    apples.add(new Gala());
    apples.add(new Fuji());
    apples.add(new Braeburn());
    for(Apple c : apples)
      System.out.println(c);
  }
} /* Output: (Sample)
GrannySmith@7d772e
Gala@11b86e7
Fuji@35ce36
Braeburn@757aef
*///:~
```

## 11.2 基本概念

java容器类库的用途是”保存对象“，其中可以划分为两个不同的概念：

1. **Collection** 一个独立元素的序列，这些元素都服从一条或多条规则。**List**必须按照插入的顺序保存元素，而**Set**不能有重复的元素。**Queue**按照队列规则来确定对象产生的顺序。
2.  **Map** 一组成对的”键值对“对象，允许你使用键来查找值。ArrayList允许你使用数字来查找值，某种意义上讲，他将数字与对象关联在一起。映射表允许使用另一个对象来查找 某个对象，也被称为”关联数组“，因为它将某些对象与另外一些对象关联在了一起，或者被称之为字典

示例：

```
List<Apple> apples = new ArrayList <Apple>();
List<Apple> apples = new LinkedList <Apple>();
```

Collection 接口概括了序列的概念——一种存放一组对象的方式。例如：用Integer对象填充了一个Collection，然后打印其所产生的容器中的所有元素。

```
//: holding/SimpleCollection.java
import java.util.*;

public class SimpleCollection {
  public static void main(String[] args) {
    Collection<Integer> c = new ArrayList<Integer>();
    for(int i = 0; i < 10; i++)
      c.add(i); // Autoboxing
    for(Integer i : c)
      System.out.print(i + ", ");
  }
} /* Output:
0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
*///:~
```

**<font color =red>ArrayList是最基本的序列类型</font>**。

## 11.3 添加一组元素

在java.util包中的**<font color=red>Arrays</font>**和**<font color =red> Collections</font>**类中都有很多实用的方法，可以在一个Collection中添加一组元素。**例如ArrayList.asList()方法和Collections.addAll()方法。**接受一个数组或者一个用逗号分割的元素列表。

示例：

```
//: holding/AddingGroups.java
// Adding groups of elements to Collection objects.
import java.util.*;

public class AddingGroups {
  public static void main(String[] args) {
    Collection<Integer> collection =
      new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
    Integer[] moreInts = { 6, 7, 8, 9, 10 };
    collection.addAll(Arrays.asList(moreInts));
    // Runs significantly faster, but you can't
    // construct a Collection this way:
    Collections.addAll(collection, 11, 12, 13, 14, 15);
    Collections.addAll(collection, moreInts);
    // Produces a list "backed by" an array:
    List<Integer> list = Arrays.asList(16, 17, 18, 19, 20);
    list.set(1, 99); // OK -- modify an element
    // list.add(21); // Runtime error because the
                     // underlying array cannot be resized.
  }
} ///:~
```

**Collection.addAll(**)方法运行起来很快，而且构造一个不含元素的Collection，然后调用Collections.addAll()这种方式很方便，因此是**首选**方式。

**Collection.addAll()成员方法只能接受另一个Collection对象作为参数，因此它不如Arrays.asList()或Collections.addAll()灵活。**

**可以直接使用Arrays.asList()输出，将其当做List，但是这种情况下，其底层表示的是数组，因此不能调整尺寸，**如果试图用add()或delete()方法在这种列表中添加或删除元素，就有可能会引发去改变数组尺寸的尝试，引发错误。

Arrays.asList()方法的限制是它对所产生的List的类型做出了最理想的假设，而并没有注意你对它会赋予什么样的类型。

示例：

```
//: holding/AsListInference.java
// Arrays.asList() makes its best guess about type.
import java.util.*;

class Snow {}
class Powder extends Snow {}
class Light extends Powder {}
class Heavy extends Powder {}
class Crusty extends Snow {}
class Slush extends Snow {}

public class AsListInference {
  public static void main(String[] args) {
    List<Snow> snow1 = Arrays.asList(
      new Crusty(), new Slush(), new Powder());

    // Won't compile:
    // List<Snow> snow2 = Arrays.asList(
    //   new Light(), new Heavy());
    // Compiler says:
    // found   : java.util.List<Powder>
    // required: java.util.List<Snow>

    // Collections.addAll() doesn't get confused:
    List<Snow> snow3 = new ArrayList<Snow>();
    Collections.addAll(snow3, new Light(), new Heavy());

    // Give a hint using an
    // explicit type argument specification:
    List<Snow> snow4 = Arrays.<Snow>asList(
       new Light(), new Heavy());
  }
} ///:~
```

如前述，当你创建snow4.可以在Arrays.asList()中间插入一条“线索”，以告诉编译器对于由Arrays.asList()所产生的List类型，实际的目标类型应该是什么。这称之为"显式类型参数说明"

## 11.4 容器的打印

示例：

```
//: holding/PrintingContainers.java
// Containers print themselves automatically.
import java.util.*;
import static net.mindview.util.Print.*;

public class PrintingContainers {
  static Collection fill(Collection<String> collection) {
    collection.add("rat");
    collection.add("cat");
    collection.add("dog");
    collection.add("dog");
    return collection;
  }
  static Map fill(Map<String,String> map) {
    map.put("rat", "Fuzzy");
    map.put("cat", "Rags");
    map.put("dog", "Bosco");
    map.put("dog", "Spot");
    return map;
  }	
  public static void main(String[] args) {
    print(fill(new ArrayList<String>()));
    print(fill(new LinkedList<String>()));
    print(fill(new HashSet<String>()));
    print(fill(new TreeSet<String>()));
    print(fill(new LinkedHashSet<String>()));
    print(fill(new HashMap<String,String>()));
    print(fill(new TreeMap<String,String>()));
    print(fill(new LinkedHashMap<String,String>()));
  }
} /* Output:
[rat, cat, dog, dog]
[rat, cat, dog, dog]
[dog, cat, rat]
[cat, dog, rat]
[rat, cat, dog]
{dog=Spot, cat=Rags, rat=Fuzzy}
{cat=Rags, dog=Spot, rat=Fuzzy}
{rat=Fuzzy, cat=Rags, dog=Spot}
*///:~
```

**Collection在每个槽中只能保存一个元素。此类容器包括**：**List**，它以特定的顺序保存一组元素；**Set**，元素不能重复；**Queue**，只允许容器在一端插入对象，并从另一端移除对象。
**Map在每个槽内保存了两个对象**，即**键**和与之相关联的**值**。

**ArrayList和LinkedList都是List类型，**他们都按照被插入的顺序保存元素。二者不同之处在于执行某行类型的操作性能，而且**LinkedList包含的操作多于ArrayList。**

**HashSet，TreeSet和LinkedHashSet都是Set类型，每个相同的项只有保存一次。**不同的Set实现存储元素的方式也不同，HashSet使用了相当复杂的方式来存储元素，如果存储顺序很重要，可以使用TreeSet，按照比较结果的升序保存对象；或者使用LinkedHashSet，它按照被添加的顺序保存对象。

**Map（也称关联数组）使得你可以用键来查找对象，**就像一个简单的数据库。键关联的对象称为值。键和值在Map中保存顺序并不是它们的插入顺序，因为HashMap实现使用的是一种非常快的算法来控制顺序。与HashSet一样，HashMap提供了最快的查询技术，也没有按照任何明显得顺序保存其元素。**而LinkedHashMap按照插入顺序保存键，同时还保留了HashMap的查询速度。**

## 11.5 List

**List**接口在Collection基础上添加了大量的方法，使得可以在List的中间插入和移除元素。由两种类型的List：

- 基本的ArrayList，它长于随机访问元素，但是在List的中间插入和移除元素时较慢。
 - LinkedList，通过代价较低的在List中间插入和删除操作，提供了优化的顺序访问，LinkedList在随机访问方面相对比较慢，但是它的特性集较ArrayList更大。

示例：

```
//: holding/ListFeatures.java
import typeinfo.pets.*;
import java.util.*;
import static net.mindview.util.Print.*;

public class ListFeatures {
  public static void main(String[] args) {
    Random rand = new Random(47);
    List<Pet> pets = Pets.arrayList(7);
    print("1: " + pets);
    Hamster h = new Hamster();
    pets.add(h); // Automatically resizes
    print("2: " + pets);
    print("3: " + pets.contains(h));
    pets.remove(h); // Remove by object
    Pet p = pets.get(2);
    print("4: " +  p + " " + pets.indexOf(p));
    Pet cymric = new Cymric();
    print("5: " + pets.indexOf(cymric));
    print("6: " + pets.remove(cymric));
    // Must be the exact object:
    print("7: " + pets.remove(p));
    print("8: " + pets);
    pets.add(3, new Mouse()); // Insert at an index
    print("9: " + pets);
    List<Pet> sub = pets.subList(1, 4);
    print("subList: " + sub);
    print("10: " + pets.containsAll(sub));
    Collections.sort(sub); // In-place sort
    print("sorted subList: " + sub);
    // Order is not important in containsAll():
    print("11: " + pets.containsAll(sub));
    Collections.shuffle(sub, rand); // Mix it up
    print("shuffled subList: " + sub);
    print("12: " + pets.containsAll(sub));
    List<Pet> copy = new ArrayList<Pet>(pets);
    sub = Arrays.asList(pets.get(1), pets.get(4));
    print("sub: " + sub);
    copy.retainAll(sub);
    print("13: " + copy);
    copy = new ArrayList<Pet>(pets); // Get a fresh copy
    copy.remove(2); // Remove by index
    print("14: " + copy);
    copy.removeAll(sub); // Only removes exact objects
    print("15: " + copy);
    copy.set(1, new Mouse()); // Replace an element
    print("16: " + copy);
    copy.addAll(2, sub); // Insert a list in the middle
    print("17: " + copy);
    print("18: " + pets.isEmpty());
    pets.clear(); // Remove all elements
    print("19: " + pets);
    print("20: " + pets.isEmpty());
    pets.addAll(Pets.arrayList(4));
    print("21: " + pets);
    Object[] o = pets.toArray();
    print("22: " + o[3]);
    Pet[] pa = pets.toArray(new Pet[0]);
    print("23: " + pa[3].id());
  }
} /* Output:
1: [Rat, Manx, Cymric, Mutt, Pug, Cymric, Pug]
2: [Rat, Manx, Cymric, Mutt, Pug, Cymric, Pug, Hamster]
3: true
4: Cymric 2
5: -1
6: false
7: true
8: [Rat, Manx, Mutt, Pug, Cymric, Pug]
9: [Rat, Manx, Mutt, Mouse, Pug, Cymric, Pug]
subList: [Manx, Mutt, Mouse]
10: true
sorted subList: [Manx, Mouse, Mutt]
11: true
shuffled subList: [Mouse, Manx, Mutt]
12: true
sub: [Mouse, Pug]
13: [Mouse, Pug]
14: [Rat, Mouse, Mutt, Pug, Cymric, Pug]
15: [Rat, Mutt, Cymric, Pug]
16: [Rat, Mouse, Cymric, Pug]
17: [Rat, Mouse, Mouse, Pug, Cymric, Pug]
18: false
19: []
20: true
21: [Manx, Cymric, Rat, EgyptianMau]
22: EgyptianMau
23: 14
*///:~
```

## 11.6 迭代器

任何容器类，都必须有某种方式可以插入元素并将它们再次取回。对于List，add()是掺入元素的方法之一，而get()是取出元素的方法之一。这里有个缺点：要使用容器，就必须对容器的确切类型编程。初看没事，但是考虑如下情况：如果原本是对着List编码，后来发现，如果能够把相同的代码应用于Set，将会显得非常方便，这是用如何才能不重写代码就可以应用于不同类型的容器？

**迭代器**的概念就是达成这一目的。**迭代器是一个对象**，它的工作是遍历并选择序列中的对象，而客户端程序员并不需要知道该序列的底层结构。此外，迭代器又被称为**轻量级对象**：创建它代价小，因此对于迭代器有各种**限制**：

  1. Iterator只能单向移动
  2. 使用方法iterator()要求容器返回一个Iterator。Iterator将准备好返回序列的第一个元素
  3. 使用**next()**获得序列中的下一个元素
  4. 使用**hasNext()**检查序列中是否还有元素
  5. 受用**remove()**将迭代器新近返回的元素删除

示例：

```
//: holding/SimpleIteration.java
import typeinfo.pets.*;
import java.util.*;

public class SimpleIteration {
  public static void main(String[] args) {
    List<Pet> pets = Pets.arrayList(12);
    Iterator<Pet> it = pets.iterator();
    while(it.hasNext()) {
      Pet p = it.next();
      System.out.print(p.id() + ":" + p + " ");
    }
    System.out.println();
    // A simpler approach, when possible:
    for(Pet p : pets)
      System.out.print(p.id() + ":" + p + " ");
    System.out.println();	
    // An Iterator can also remove elements:
    it = pets.iterator();
    for(int i = 0; i < 6; i++) {
      it.next();
      it.remove();
    }
    System.out.println(pets);
  }
} /* Output:
0:Rat 1:Manx 2:Cymric 3:Mutt 4:Pug 5:Cymric 6:Pug 7:Manx 8:Cymric 9:Rat 10:EgyptianMau 11:Hamster
0:Rat 1:Manx 2:Cymric 3:Mutt 4:Pug 5:Cymric 6:Pug 7:Manx 8:Cymric 9:Rat 10:EgyptianMau 11:Hamster
[Pug, Manx, Cymric, Rat, EgyptianMau, Hamster]
*///:~
```

迭代器：能够将遍历序列的操作与序列底层的结构分离。

## 11.6.1 ListIterator

**ListIterator是一种更强大的Iterator的子类型，它只能用于各种List类的访问**

**ListIterator**可以**双向移动**，还可以产生相对于迭代器在列表中指向的当前位置的前一个和后一个元素的**索引**，并且可以使用**set()**方法替换它访问过的最后一个元素。可以调用**listIterator()**方法产生一个指向List开始处的ListIterator，并且还可以通过调用**listIterator(n)**方法创建一个一开始就指向列表索引为n的元素处的ListIterator。

示例：

```
//: holding/ListIteration.java
import typeinfo.pets.*;
import java.util.*;

public class ListIteration {
  public static void main(String[] args) {
    List<Pet> pets = Pets.arrayList(8);
    ListIterator<Pet> it = pets.listIterator();
    while(it.hasNext())
      System.out.print(it.next() + ", " + it.nextIndex() +
        ", " + it.previousIndex() + "; ");
    System.out.println();
    // Backwards:
    while(it.hasPrevious())
      System.out.print(it.previous().id() + " ");
    System.out.println();
    System.out.println(pets);	
    it = pets.listIterator(3);
    while(it.hasNext()) {
      it.next();
      it.set(Pets.randomPet());
    }
    System.out.println(pets);
  }
} /* Output:
Rat, 1, 0; Manx, 2, 1; Cymric, 3, 2; Mutt, 4, 3; Pug, 5, 4; Cymric, 6, 5; Pug, 7, 6; Manx, 8, 7;
7 6 5 4 3 2 1 0
[Rat, Manx, Cymric, Mutt, Pug, Cymric, Pug, Manx]
[Rat, Manx, Cymric, Cymric, Rat, EgyptianMau, Hamster, EgyptianMau]
*///:~
```

## 11.7 LinkedList

**LinkedList**也像ArrayList一样实现了基本的List接口，但是它执行某些操作（List中间进行插入和移除）时比ArrayList更高效，在随机访问操作方面更逊一筹。**LinkedList还添加了可以使用其用作栈，队列或双端队列的方法。**这些方法有些只是名称有些差异，或许只存在些许差异，特别是在Queue中。例如：**getFirst()**和**element()**完全一样，它们都返回列表的头（第一个元素），并不移除它，如果List为空，则抛出异常。**peek()**方法与这两个方式只是稍有差异，他在列表为空的时候返回null。

**removeFirst()**与**remove()**一样，移除并返回列表头，如果列表为空抛出异常，**poll()**稍有差异，列表为空返回null。**addFirst()**和**add()**和**addLast()**相同，它们都将某个元素插入到列表的尾部。
**removeLast()**移除并返回最后一个元素。

<font color=red>**Queue接口是在LinkedList的基础上添加了element(),offer(),peek(),poll()和remove()方法，使其变成一个Queue实现。**</font>

示例：

```
//: holding/LinkedListFeatures.java
import typeinfo.pets.*;
import java.util.*;
import static net.mindview.util.Print.*;

public class LinkedListFeatures {
  public static void main(String[] args) {
    LinkedList<Pet> pets =
      new LinkedList<Pet>(Pets.arrayList(5));
    print(pets);
    // Identical:
    print("pets.getFirst(): " + pets.getFirst());
    print("pets.element(): " + pets.element());
    // Only differs in empty-list behavior:
    print("pets.peek(): " + pets.peek());
    // Identical; remove and return the first element:
    print("pets.remove(): " + pets.remove());
    print("pets.removeFirst(): " + pets.removeFirst());
    // Only differs in empty-list behavior:
    print("pets.poll(): " + pets.poll());
    print(pets);
    pets.addFirst(new Rat());
    print("After addFirst(): " + pets);
    pets.offer(Pets.randomPet());
    print("After offer(): " + pets);
    pets.add(Pets.randomPet());
    print("After add(): " + pets);
    pets.addLast(new Hamster());
    print("After addLast(): " + pets);
    print("pets.removeLast(): " + pets.removeLast());
  }
} /* Output:
[Rat, Manx, Cymric, Mutt, Pug]
pets.getFirst(): Rat
pets.element(): Rat
pets.peek(): Rat
pets.remove(): Rat
pets.removeFirst(): Manx
pets.poll(): Cymric
[Mutt, Pug]
After addFirst(): [Rat, Mutt, Pug]
After offer(): [Rat, Mutt, Pug, Cymric]
After add(): [Rat, Mutt, Pug, Cymric, Pug]
After addLast(): [Rat, Mutt, Pug, Cymric, Pug, Hamster]
pets.removeLast(): Hamster
*///:~
```

## 11.8 stack

”栈“通常是指”后进先出（LIFO）“的容器。有时栈也被称为”叠加栈“，因为最后压入栈中的元素总是第一个弹出。

<font color=red>**LinkedList能够直接实现栈的所有功能的方法，因此可以将LinkedList当做栈使用。**</font>

示例：

```
//: net/mindview/util/Stack.java
// Making a stack from a LinkedList.
package net.mindview.util;
import java.util.LinkedList;

public class Stack<T> {
  private LinkedList<T> storage = new LinkedList<T>();
  public void push(T v) { storage.addFirst(v); }
  public T peek() { return storage.getFirst(); }
  public T pop() { return storage.removeFirst(); }
  public boolean empty() { return storage.isEmpty(); }
  public String toString() { return storage.toString(); }
} ///:~
```

如果想在自己的代码中使用这个Stack类，当你在创建实例时，需要完整指定包名，或者更改这个类的名称，否则，就有可能与java.util包中的Stack发生冲突。

示例：

```
//: holding/StackCollision.java
import net.mindview.util.*;

public class StackCollision {
  public static void main(String[] args) {
    net.mindview.util.Stack<String> stack =
      new net.mindview.util.Stack<String>();
    for(String s : "My dog has fleas".split(" "))
      stack.push(s);
    while(!stack.empty())
      System.out.print(stack.pop() + " ");
    System.out.println();
    java.util.Stack<String> stack2 =
      new java.util.Stack<String>();
    for(String s : "My dog has fleas".split(" "))
      stack2.push(s);
    while(!stack2.empty())
      System.out.print(stack2.pop() + " ");
  }
} /* Output:
fleas has dog My
fleas has dog My
*///:~
```

现在，任何对Stack的引用都将选择net.mindview.util版本，而在选择java.util.Stack时，必须使用全限定名称。

## 11.9 Set

**Set不保存重复的元素。**如果你试图将相同对象的多个实例添加到Set中，那么它就会阻止这种重复现象。**Set最常用被使用的是测试归属性，可以很容易地查询某个对象是否在某个Set中，通常选择一个<font color =red>HashSet</font>实现，它专门对快速查找进行了优化。**

**Set具有和Collection完全一样的接口**，没有任何额外的功能。实际上Set就是Collection，二者行为不同。<font color=red>**Set是基于对象的值来确定归属性的。**</font>

HashSet输出顺序没有规律，因为其使用了散列，使得速度更快。HashSet所维护的顺序与TreeSet和LinkedSet都不同，它们的实现具有不同的元素存储方式。**TreeSet将元素存储在红-黑树数据结构中，而HashSet使用的是散列函数。LinkedHashList查询速度快是使用了散列，但是使用了链表来维护元素的插入顺序。**

如果相对结果排序，使用TreeSet来代替HashSet。

示例：

```
//: holding/SortedSetOfInteger.java
import java.util.*;

public class SortedSetOfInteger {
  public static void main(String[] args) {
    Random rand = new Random(47);
    SortedSet<Integer> intset = new TreeSet<Integer>();
    for(int i = 0; i < 10000; i++)
      intset.add(rand.nextInt(30));
    System.out.println(intset);
  }
} /* Output:
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29]
*///:~
```

## 11.10 Map

示例：

```
//: holding/Statistics.java
// Simple demonstration of HashMap.
import java.util.*;

public class Statistics {
  public static void main(String[] args) {
    Random rand = new Random(47);
    Map<Integer,Integer> m =
      new HashMap<Integer,Integer>();
    for(int i = 0; i < 10000; i++) {
      // Produce a number between 0 and 20:
      int r = rand.nextInt(20);
      Integer freq = m.get(r);
      m.put(r, freq == null ? 1 : freq + 1);
    }
    System.out.println(m);
  }
} /* Output:
{15=497, 4=481, 19=464, 8=468, 11=531, 16=533, 18=478, 3=508, 7=471, 12=521, 17=509, 2=489, 13=506, 9=549, 6=519, 1=502, 14=477, 10=513, 5=503, 0=481}
*///:~
```

关于Map，比较常用的是使用**containsKey()**和**containsvalue()**来查看他是否包含某个键或某个值。

示例：

```
//: holding/PetMap.java
import typeinfo.pets.*;
import java.util.*;
import static net.mindview.util.Print.*;
public class PetMap {
  public static void main(String[] args) {
    Map<String,Pet> petMap = new HashMap<String,Pet>();
    petMap.put("My Cat", new Cat("Molly"));
    petMap.put("My Dog", new Dog("Ginger"));
    petMap.put("My Hamster", new Hamster("Bosco"));
    print(petMap);
    Pet dog = petMap.get("My Dog");
    print(dog);
    print(petMap.containsKey("My Dog"));
    print(petMap.containsValue(dog));
  }
} /* Output:
{My Cat=Cat Molly, My Hamster=Hamster Bosco, My Dog=Dog Ginger}
Dog Ginger
true
true
*///:~
```

Map可以很容易扩展到多维情况上，例如跟踪一个拥有多个宠物的人，可以是`Map<Person,List<Pet>>`

示例：

```
//: holding/MapOfList.java
package holding;
import typeinfo.pets.*;
import java.util.*;
import static net.mindview.util.Print.*;

public class MapOfList {
  public static Map<Person, List<? extends Pet>>
    petPeople = new HashMap<Person, List<? extends Pet>>();
  static {
    petPeople.put(new Person("Dawn"),
      Arrays.asList(new Cymric("Molly"),new Mutt("Spot")));
    petPeople.put(new Person("Kate"),
      Arrays.asList(new Cat("Shackleton"),
        new Cat("Elsie May"), new Dog("Margrett")));
    petPeople.put(new Person("Marilyn"),
      Arrays.asList(
       new Pug("Louie aka Louis Snorkelstein Dupree"),
       new Cat("Stanford aka Stinky el Negro"),
       new Cat("Pinkola")));	
    petPeople.put(new Person("Luke"),
      Arrays.asList(new Rat("Fuzzy"), new Rat("Fizzy")));
    petPeople.put(new Person("Isaac"),
      Arrays.asList(new Rat("Freckly")));
  }
  public static void main(String[] args) {
    print("People: " + petPeople.keySet());
    print("Pets: " + petPeople.values());
    for(Person person : petPeople.keySet()) {
      print(person + " has:");
      for(Pet pet : petPeople.get(person))
        print("    " + pet);
    }
  }
} /* Output:	
People: [Person Luke, Person Marilyn, Person Isaac, Person Dawn, Person Kate]
Pets: [[Rat Fuzzy, Rat Fizzy], [Pug Louie aka Louis Snorkelstein Dupree, Cat Stanford aka Stinky el Negro, Cat Pinkola], [Rat Freckly], [Cymric Molly, Mutt Spot], [Cat Shackleton, Cat Elsie May, Dog Margrett]]
Person Luke has:
    Rat Fuzzy
    Rat Fizzy
Person Marilyn has:
    Pug Louie aka Louis Snorkelstein Dupree
    Cat Stanford aka Stinky el Negro
    Cat Pinkola
Person Isaac has:
    Rat Freckly
Person Dawn has:
    Cymric Molly
    Mutt Spot
Person Kate has:
    Cat Shackleton
    Cat Elsie May
    Dog Margrett
*///:~
```

Map可以返回它的键的Set，它的值的Collection，或者它的键值对的Set。keySet()方法产生了由在petPeople中所有键组成的Set，它在foreach语句中被用来迭代遍历该Map。

## 11.11 Queue

**队列**是一个典型的先进先出（FIFO）的容器，即从容器的一端放入事物，从另一端取出，事物放入的顺序和取出的顺序是相同的。**队列在并发编程中特别重要。**

**LinkedList提供了方法支持队列的行为**，并且实现了队列的接口，因此LinkedList可以是Queue的一种实现。

示例：

```
//: holding/QueueDemo.java
// Upcasting to a Queue from a LinkedList.
import java.util.*;

public class QueueDemo {
  public static void printQ(Queue queue) {
    while(queue.peek() != null)
      System.out.print(queue.remove() + " ");
    System.out.println();
  }
  public static void main(String[] args) {
    Queue<Integer> queue = new LinkedList<Integer>();
    Random rand = new Random(47);
    for(int i = 0; i < 10; i++)
      queue.offer(rand.nextInt(i + 10));
    printQ(queue);
    Queue<Character> qc = new LinkedList<Character>();
    for(char c : "Brontosaurus".toCharArray())
      qc.offer(c);
    printQ(qc);
  }
} /* Output:
8 1 1 1 5 14 3 1 0 1
B r o n t o s a u r u s
*///:~
```

**offer()**方法在允许的情况下，将一个元素插入到队尾，或者返回false。**peek()**和**element()**都将在不移除的情况下返回队头，但是peek()方法在队列为空的时候返回null，而**element()会抛出异常**。**poll()**和**remove()**方法都是移除并返回队列头部，**poll()**在队列为空返回null，后者抛出异常。

### 11.11.1 PriorityQueue

<font color=red>**优先级队列**</font>声明下一个弹出元素是最需要的元素（具有最高优先级）。某些消息比其他消息更重要，应该更快得到处理，那么它们的处理与它们何时到达无关。PriorityQueue添加到了Java SE5中，提供了这种行为的自动实现。

在PriorityQueue中调用offer()插入对象，会在队列中被排序，**默认是自然顺序。可以通过自己提供的Comparator来修改这一顺序。**PriorityQueue确保使用peek(),remove()和poll()方法是获取的元素是队列中优先级最高的元素。

示例：

```
//: holding/PriorityQueueDemo.java
import java.util.*;

public class PriorityQueueDemo {
  public static void main(String[] args) {
    PriorityQueue<Integer> priorityQueue =
      new PriorityQueue<Integer>();
    Random rand = new Random(47);
    for(int i = 0; i < 10; i++)
      priorityQueue.offer(rand.nextInt(i + 10));
    QueueDemo.printQ(priorityQueue);

    List<Integer> ints = Arrays.asList(25, 22, 20,
      18, 14, 9, 3, 1, 1, 2, 3, 9, 14, 18, 21, 23, 25);
    priorityQueue = new PriorityQueue<Integer>(ints);
    QueueDemo.printQ(priorityQueue);
    priorityQueue = new PriorityQueue<Integer>(
        ints.size(), Collections.reverseOrder());
    priorityQueue.addAll(ints);
    QueueDemo.printQ(priorityQueue);

    String fact = "EDUCATION SHOULD ESCHEW OBFUSCATION";
    List<String> strings = Arrays.asList(fact.split(""));
    PriorityQueue<String> stringPQ =
      new PriorityQueue<String>(strings);
    QueueDemo.printQ(stringPQ);
    stringPQ = new PriorityQueue<String>(
      strings.size(), Collections.reverseOrder());
    stringPQ.addAll(strings);
    QueueDemo.printQ(stringPQ);

    Set<Character> charSet = new HashSet<Character>();
    for(char c : fact.toCharArray())
      charSet.add(c); // Autoboxing
    PriorityQueue<Character> characterPQ =
      new PriorityQueue<Character>(charSet);
    QueueDemo.printQ(characterPQ);
  }
} /* Output:
0 1 1 1 1 1 3 5 8 14
1 1 2 3 3 9 9 14 14 18 18 20 21 22 23 25 25
25 25 23 22 21 20 18 18 14 14 9 9 3 3 2 1 1
       A A B C C C D D E E E F H H I I L N N O O O O S S S T T U U U W
W U U U T T S S S O O O O N N L I I H H F E E E D D C C C B A A
  A B C D E F H I L N O S T U W
*///:~
```

**重复是允许的，最小值拥有最高的优先级。**如果是String，空格也可以算作值，且比字母优先级高。

## 11.12 Collection和Iterator

如果继承了AbstractCollection，则无论如何都要强制去实现iterator()和size()，以便提供AbstractCollection没有实现，但是AbstractCollection中的其他方法会使用到的方法：

```
//: holding/CollectionSequence.java
import typeinfo.pets.*;
import java.util.*;

public class CollectionSequence
extends AbstractCollection<Pet> {
  private Pet[] pets = Pets.createArray(8);
  public int size() { return pets.length; }
  public Iterator<Pet> iterator() {
    return new Iterator<Pet>() {
      private int index = 0;
      public boolean hasNext() {
        return index < pets.length;
      }
      public Pet next() { return pets[index++]; }
      public void remove() { // Not implemented
        throw new UnsupportedOperationException();
      }
    };
  }	
  public static void main(String[] args) {
    CollectionSequence c = new CollectionSequence();
    InterfaceVsIterator.display(c);
    InterfaceVsIterator.display(c.iterator());
  }
} /* Output:
0:Rat 1:Manx 2:Cymric 3:Mutt 4:Pug 5:Cymric 6:Pug 7:Manx
0:Rat 1:Manx 2:Cymric 3:Mutt 4:Pug 5:Cymric 6:Pug 7:Manx
*///:~
```

如果你的类已经继承了其他类，那么就不能再继承AbstractCollection了。在这种情况下，要实现Collection，就必须实现该接口中的所有方法。这时候继承并提供创建迭代器的能力就会显得容易多了。

```
//: holding/NonCollectionSequence.java
import typeinfo.pets.*;
import java.util.*;

class PetSequence {
  protected Pet[] pets = Pets.createArray(8);
}

public class NonCollectionSequence extends PetSequence {
  public Iterator<Pet> iterator() {
    return new Iterator<Pet>() {
      private int index = 0;
      public boolean hasNext() {
        return index < pets.length;
      }
      public Pet next() { return pets[index++]; }
      public void remove() { // Not implemented
        throw new UnsupportedOperationException();
      }
    };
  }
  public static void main(String[] args) {
    NonCollectionSequence nc = new NonCollectionSequence();
    InterfaceVsIterator.display(nc.iterator());
  }
} /* Output:
0:Rat 1:Manx 2:Cymric 3:Mutt 4:Pug 5:Cymric 6:Pug 7:Manx
*///:~
```

## 11.13 Foreach迭代器

foreach语法主要用于数组，它也可以应用于任何Collection对象。之所以能工作，是因为Java SE5 引入了新的被称为**Iterable接口**，该接口包含了一个能够产生Iterator的iterator()方法，并且Iterator接口被foreach用来在序列中移动。因此，**如果创建了任何实现Iterable的类，都可以将它用于foreach语句中。**

示例：

```
​```
//: holding/IterableClass.java
// Anything Iterable works with foreach.
import java.util.*;

public class IterableClass implements Iterable<String> {
  protected String[] words = ("And that is how " +
    "we know the Earth to be banana-shaped.").split(" ");
  public Iterator<String> iterator() {
    return new Iterator<String>() {
      private int index = 0;
      public boolean hasNext() {
        return index < words.length;
      }
      public String next() { return words[index++]; }
      public void remove() { // Not implemented
        throw new UnsupportedOperationException();
      }
    };
  }	
  public static void main(String[] args) {
    for(String s : new IterableClass())
      System.out.print(s + " ");
  }
} /* Output:
And that is how we know the Earth to be banana-shaped.
*///:~

​```
```

在java SE5 中，大量的类都是Iterable类型，主要包括所有的Collection类（但是不包括各种Map）。

### 11.13.1 适配器方法惯用法

如果现有一个Iterable类，你想要添加一种或多种在foreach语句中使用这个类的方法。例如假设你希望可以选择以向前的方向或向后的方向迭代一个单词列表。如果直接继承这个类，并覆盖iterator()语句，你只能替换现有的方法，而不能实现选择。

一种解决方案就是所谓的适配器方法的惯用法。**当你有一个接口并需要另一个接口时，编写适配器就可以解决问题**。

示例：

```
//: holding/AdapterMethodIdiom.java
// The "Adapter Method" idiom allows you to use foreach
// with additional kinds of Iterables.
import java.util.*;

class ReversibleArrayList<T> extends ArrayList<T> {
  public ReversibleArrayList(Collection<T> c) { super(c); }
  public Iterable<T> reversed() {
    return new Iterable<T>() {
      public Iterator<T> iterator() {
        return new Iterator<T>() {
          int current = size() - 1;
          public boolean hasNext() { return current > -1; }
          public T next() { return get(current--); }
          public void remove() { // Not implemented
            throw new UnsupportedOperationException();
          }
        };
      }
    };
  }
}	

public class AdapterMethodIdiom {
  public static void main(String[] args) {
    ReversibleArrayList<String> ral =
      new ReversibleArrayList<String>(
        Arrays.asList("To be or not to be".split(" ")));
    // Grabs the ordinary iterator via iterator():
    for(String s : ral)
      System.out.print(s + " ");
    System.out.println();
    // Hand it the Iterable of your choice
    for(String s : ral.reversed())
      System.out.print(s + " ");
  }
} /* Output:
To be or not to be
be to not or be To
*///:~
```

```
//: holding/MultiIterableClass.java
// Adding several Adapter Methods.
import java.util.*;

public class MultiIterableClass extends IterableClass {
  public Iterable<String> reversed() {
    return new Iterable<String>() {
      public Iterator<String> iterator() {
        return new Iterator<String>() {
          int current = words.length - 1;
          public boolean hasNext() { return current > -1; }
          public String next() { return words[current--]; }
          public void remove() { // Not implemented
            throw new UnsupportedOperationException();
          }
        };
      }
    };
  }	
  public Iterable<String> randomized() {
    return new Iterable<String>() {
      public Iterator<String> iterator() {
        List<String> shuffled =
          new ArrayList<String>(Arrays.asList(words));
        Collections.shuffle(shuffled, new Random(47));
        return shuffled.iterator();
      }
    };
  }	
  public static void main(String[] args) {
    MultiIterableClass mic = new MultiIterableClass();
    for(String s : mic.reversed())
      System.out.print(s + " ");
    System.out.println();
    for(String s : mic.randomized())
      System.out.print(s + " ");
    System.out.println();
    for(String s : mic)
      System.out.print(s + " ");
  }
} /* Output:
banana-shaped. be to Earth the know we how is that And
is banana-shaped. Earth that how the be And we know to
And that is how we know the Earth to be banana-shaped.
*///:~
```

## 11.14 总结

![Collection接口继承树](http://img.blog.csdn.net/20171118114337115?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![Map接口继承树](http://img.blog.csdn.net/20171118114442737?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzM1ODAzMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 第12章 通过异常处理错误

异常处理是java中唯一正式的错误报告机制，并且通过编译器强制执行，使得构建能够与客户端代码可靠地沟通问题。

## 12.1 概念

对于异常处理的实现可以追溯到20世纪60年代的操作系统，甚至是BASIC语言中的on error goto 语句。

异常能够在问题出现时停下来处理，它旺往往能够降低错误处理代码的复杂度。

## 12.2 基本异常

**异常情形**是指阻止当前方法或作用域继续执行的问题。异常情形与普通问题有区分。后者在当前环境下能得到足够的信息，总能解决这个问题，而前者则无法。

**抛出异常后**：首先，同java其他对象的创建一样，将使用new在堆上创建异常对象。然后，当前执行路径被终止了，并且从当前环境中弹出对异常对象的引用。此时，异常处理机制接管程序，并开始寻找一个恰当的地方来继续执行程序。这个恰当的地方就是**异常处理程序**，它的任务就是将程序从错误状态中恢复，以使程序能要么换一种方式运行，要么继续运行下去。

### 12.2.1 异常参数

所有标准的异常类都有两个构造器：一个是默认构造器；另一个是接受字符串作为参数，以便能吧相关信息放入异常对象的构造器：

```
if(t == null)
throw new NullPointerException();
```

此外，能够抛出任意类型的Throwable对象，它是异常类型的根类型。

## 12.3 捕获异常

### 12.3.1 try块

在方法内部抛出异常（或在方法内部调用其他方法抛出了异常），这个方法将在抛出异常的过程中结束。要是不希望方法就此结束，在方法内设置一个特殊块来捕获异常。称为**try块**。它是跟在try关键字之后的普通程序块：

```
try {
// Code that might generate exceptions
}
```

### 12.3.2 异常处理程序

抛出的异常必须在某处得到处理。这个“地点”就是**异常处理程序**。异常处理程序紧跟在try块之后，以关键字**catch**表示：

```
try {
// Code that might generate exceptions
} catch(Type1 id1)|{
// Handle exceptions of Type1
} catch(Type2 id2) {
// Handle exceptions of Type2
} catch(Type3 id3) {
// Handle exceptions of Type3
}
// etc...
```

异常处理程序必须紧跟在try块之后。当异常被抛出，异常处理机制将负责搜寻参数与异常类型相匹配的第一个处理程序。然后进入catch子句执行，此时认为异常得到了处理。只有匹配catch子句才能得到执行；这与swith语句不同。

异常处理理论上有两种基本模型：

1. java支持终止模型。
2. 恢复模型：如果java想要实现类似恢复的行为，遇见错误就不能抛出异常，而是调用方法修正该错误。或者将try块放入while循环。

## 12.4 创建自定义异常

要定义异常类，必须从已有的异常类继承。

示例：

```
//: exceptions/InheritingExceptions.java
// Creating your own exceptions.
class SimpleException extends Exception {}
public class InheritingExceptions {
public void f() throws SimpleException {
System.out.println("Throw SimpleException from f()");
throw new SimpleException();
}
public static void main(String[] args) {
InheritingExceptions sed = new InheritingExceptions();
try {
sed.f();
} catch(SimpleException e) {
System.out.println("Caught it!");
}
}
} /* Output:
Throw SimpleException from f()
3 As do most languages, including C++, C#, Python, D, etc.
Caught it!
*///:~
```

也可以为异常类定义一个接受字符串参数的构造器：

```
//: exceptions/FullConstructors.java
class MyException extends Exception {
public MyException() {}
public MyException(String msg) { super(msg); }
}
public class FullConstructors {
public static void f() throws MyException {
System.out.println("Throwing MyException from f()");
throw new MyException();
}
public static void g() throws MyException {
System.out.println("Throwing MyException from g()");
throw new MyException("Originated in g()");
}
public static void main(String[] args) {
try {
f();
} catch(MyException e) {
e.printStackTrace(System.out);
}
try {
g();
} catch(MyException e) {
e.printStackTrace(System.out);
}
}
} /* Output:
Throwing MyException from f()
MyException
at FullConstructors.f(FullConstructors.java:11)
at FullConstructors.main(FullConstructors.java:19)
Throwing MyException from g()
MyException: Originated in g()
at FullConstructors.g(FullConstructors.java:15)
at FullConstructors.main(FullConstructors.java:24)
*///:~
```

异常处理程序中，调用了Throwable类声明的printStackTrack()方法，就像从输出中看到的，它将打印“从方法调用处知道异常抛出处”的方法调用序列。这里，信息被发送到了System.out，并自动地被捕获和显示在输出中。但是如果调用默认版本：

e.printStackTrace();

则信息将被输出到标准错误流。

### 12.4.1 异常与记录日志

使用java.util.logging工具将输出记录到日志中。

```
//: exceptions/LoggingExceptions.java
// An exception that reports through a Logger.
import java.util.logging.*;
import java.io.*;
class LoggingException extends Exception {
private static Logger logger =
Logger.getLogger("LoggingException");
public LoggingException() {
StringWriter trace = new StringWriter();
printStackTrace(new PrintWriter(trace));
logger.severe(trace.toString());
}
}
public class LoggingExceptions {
public static void main(String[] args) {
try {
throw new LoggingException();
} catch(LoggingException e) {
System.err.println("Caught " + e);
}
try {
throw new LoggingException();
Error Handling with Exceptions 319
} catch(LoggingException e) {
System.err.println("Caught " + e);
}
}
} /* Output: (85% match)
Aug 30, 2005 4:02:31 PM LoggingException <init>
SEVERE: LoggingException
at LoggingExceptions.main(LoggingExceptions.java:19)
Caught LoggingException
Aug 30, 2005 4:02:31 PM LoggingException <init>
SEVERE: LoggingException
at LoggingExceptions.main(LoggingExceptions.java:24)
Caught LoggingException
*///:~
```

静态的**Logger.getLogger()**方法创建了一个String参数相关联的Logger对象（通常与错误相关的包名和类名），这个Logger对象会将其输出发送到**System.err**。向Logger写入最简单的方式就是直接调用与日志记录消息的级别相关联的方法，这里使用severe()；为了产生日志记录消息，我们与获取异常抛出处的栈轨迹，但是printStackTrace()不会默认地产生字符串。为了获取字符串，需要使用重载的该方法。

我们需要捕获和记录其他人编写的异常，因此我们必须在异常处理程序中生成日志消息：

示例：

```
//: exceptions/LoggingExceptions2.java
// Logging caught exceptions.
import java.util.logging.*;
import java.io.*;
public class LoggingExceptions2 {
private static Logger logger =
Logger.getLogger("LoggingExceptions2");
static void logException(Exception e) {
StringWriter trace = new StringWriter();
e.printStackTrace(new PrintWriter(trace));
logger.severe(trace.toString());
}
public static void main(String[] args) {
try {
throw new NullPointerException();
} catch(NullPointerException e) {
logException(e);
}
}
} /* Output: (90% match)
Aug 30, 2005 4:07:54 PM LoggingExceptions2 logException
SEVERE: java.lang.NullPointerException
at LoggingExceptions2.main(LoggingExceptions2.java:16)
*///:~
```

对于异常类来说，getMessage()方法有点类似于toString()方法。异常也是对象的一种，可以继续修改这个异常类，得到更强的功能。但是使用程序包的客户端程序员可能仅仅是查看一下抛出异常类型，其他就不管。

## 12.5 异常说明

Java鼓励人们把方法可能会抛出的异常告知使用此方法的客户端程序员，提供了相应的语法（并强制使用这个语法），使你能以礼貌的方式告知客户端程序员某个方法可能会抛出异常类型，然后客户端程序员就可以进行相应的处理。这就是**异常说明**，属于方法声明的一部分，紧跟在形式参数列表之后。

异常说明使用了附加关键字**throws**，后面接一个潜在异常类型的列表：

`void f() throws TooBig, TooSmall, DivZero { //...`

代码必须和异常说明保持一致。

## 12.6 捕获所有异常

可以只写一个异常处理程序来捕获所有类型的异常。通过捕获异常类型的**基类Exception**，就可以做到这一点。

```

```

最好将其放置在处理程序的末尾。

**<font color=red>可调用的方法</font>**：

- String getMessage( )：获取详细信息
- String getLocalizedMessage( )：用本地语言表示的详细信息
- String toString( )：返回对Throwable的简单描述，要是有详细信息也将包含在内
- void printStackTrace( )
- void printStackTrace(PrintStream)
- void printStackTrace(java.io.PrintWriter)：打印Throwable和Throwable的调用栈轨迹。调用栈显示了“把你带到异常抛出点的地点”的方法调用序列。第一个版本输出到标准错误，后两个版本允许选择要输出的流。
- Throwable fillInStackTrace()：用于在Throwable对象的内部记录栈帧的当前状态
- getClass()：返回一个表示此对象类型的对象
- getName()：查询这个Class对象包含包信息的名称
- getSimpleName()：只产生类名称

**示例**：

```
//: exceptions/ExceptionMethods.java
// Demonstrating the Exception Methods.
import static net.mindview.util.Print.*;
Error Handling with Exceptions 323
public class ExceptionMethods {
public static void main(String[] args) {
try {
throw new Exception("My Exception");
} catch(Exception e) {
print("Caught Exception");
print("getMessage():" + e.getMessage());
print("getLocalizedMessage():" +
e.getLocalizedMessage());
print("toString():" + e);
print("printStackTrace():");
e.printStackTrace(System.out);
}
}
} /* Output:
Caught Exception
getMessage():My Exception
getLocalizedMessage():My Exception
toString():java.lang.Exception: My Exception
printStackTrace():
java.lang.Exception: My Exception
at ExceptionMethods.main(ExceptionMethods.java:8)
*///:~
```

### 12.6.1 栈轨迹

**printStackTrace()**方法提供的信息可以通过**getStackTrace()**方法来直接访问，这个方法将返回一个由栈轨迹中的元素所构成的数组，其中每一个元素都表示栈中的一帧。**元素0是栈顶元素**，并且是调用序列中最后一个方法调用。数组中最后一个元素和栈底是调用序列中的第一个方法调用。

**<font color=red>示例</font>**：

```
//: exceptions/WhoCalled.java
// Programmatic access to stack trace information.
public class WhoCalled {
static void f() {
// Generate an exception to fill in the stack trace
try {
throw new Exception();
} catch (Exception e) {
for(StackTraceElement ste : e.getStackTrace())
System.out.println(ste.getMethodName());
}
}
static void g() { f(); }
static void h() { g(); }
public static void main(String[] args) {
f();
System.out.println("--------------------------------");
g();
System.out.println("--------------------------------");
324 Thinking in Java Bruce Eckel
h();
}
} /* Output:
f
main
--------------------------------
f
g
main
--------------------------------
f
g
h
main
*///:~
```

### 12.6.2 重新抛出异常

希望将刚捕获的异常重新抛出，尤其是在使用Exception捕获所有异常时。

示例：

```
catch(Exception e) {
System.out.println("An exception was thrown");
throw e;
}
```

同一个try块的后续catch子句将忽略。

如果只是把当前异常对象重新抛出，那么**printStackTrace()**方法显示的将是原来异常抛出点的调用信息，而并非重新抛出点的信息。如果想要更新这个信息，可以调用**fillInStackTrace()**方法，将返回一个Throwable对象，通过把当前调用栈信息填入原来那个异常对象而建立：

示例：

```
//: exceptions/Rethrowing.java
// Demonstrating fillInStackTrace()
public class Rethrowing {
public static void f() throws Exception {
System.out.println("originating the exception in f()");
throw new Exception("thrown from f()");
}
public static void g() throws Exception {
try {
f();
} catch(Exception e) {
System.out.println("Inside g(),e.printStackTrace()");
e.printStackTrace(System.out);
throw e;
}
}
public static void h() throws Exception {
Error Handling with Exceptions 325
try {
f();
} catch(Exception e) {
System.out.println("Inside h(),e.printStackTrace()");
e.printStackTrace(System.out);
throw (Exception)e.fillInStackTrace();
}
}
public static void main(String[] args) {
try {
g();
} catch(Exception e) {
System.out.println("main: printStackTrace()");
e.printStackTrace(System.out);
}
try {
h();
} catch(Exception e) {
System.out.println("main: printStackTrace()");
e.printStackTrace(System.out);
}
}
} /* Output:
originating the exception in f()
Inside g(),e.printStackTrace()
java.lang.Exception: thrown from f()
at Rethrowing.f(Rethrowing.java:7)
at Rethrowing.g(Rethrowing.java:11)
at Rethrowing.main(Rethrowing.java:29)
main: printStackTrace()
java.lang.Exception: thrown from f()
at Rethrowing.f(Rethrowing.java:7)
at Rethrowing.g(Rethrowing.java:11)
at Rethrowing.main(Rethrowing.java:29)
originating the exception in f()
Inside h(),e.printStackTrace()
java.lang.Exception: thrown from f()
at Rethrowing.f(Rethrowing.java:7)
at Rethrowing.h(Rethrowing.java:20)
at Rethrowing.main(Rethrowing.java:35)
main: printStackTrace()
java.lang.Exception: thrown from f()
at Rethrowing.h(Rethrowing.java:24)
at Rethrowing.main(Rethrowing.java:35)
*///:~
```

调用fillInStackTrace()的那一行就成了异常的新发生地。

有可能在捕获异常之后抛出另一种异常，这么做，得到效果类似于使用fillInStrackTrace()，有关原来异常发生点的信息丢失，剩下的都是与新的抛出点有关的信息。

示例：

```
//: exceptions/RethrowNew.java
// Rethrow a different object from the one that was caught.
class OneException extends Exception {
public OneException(String s) { super(s); }
}
class TwoException extends Exception {
326 Thinking in Java Bruce Eckel
public TwoException(String s) { super(s); }
}
public class RethrowNew {
public static void f() throws OneException {
System.out.println("originating the exception in f()");
throw new OneException("thrown from f()");
}
public static void main(String[] args) {
try {
try {
f();
} catch(OneException e) {
System.out.println(
"Caught in inner try, e.printStackTrace()");
e.printStackTrace(System.out);
throw new TwoException("from inner try");
}
} catch(TwoException e) {
System.out.println(
"Caught in outer try, e.printStackTrace()");
e.printStackTrace(System.out);
}
}
} /* Output:
originating the exception in f()
Caught in inner try, e.printStackTrace()
OneException: thrown from f()
at RethrowNew.f(RethrowNew.java:15)
at RethrowNew.main(RethrowNew.java:20)
Caught in outer try, e.printStackTrace()
TwoException: from inner try
at RethrowNew.main(RethrowNew.java:25)
*///:~
```



### 12.6.3 异常链

捕获一个异常后抛出另一个异常，并且希望把原始异常的信息保存下来，这称为**<font color = red>异常链</font>**。所有的Throwable的子类构造器都可以接受一个cause对象作为参数。这个cause就用来表示原始异常，通过把原始异常传递给新的异常，使得即使在当前位置创建并抛出了新的异常，也能通过这个异常链追踪到异常最初发生的位置。

Throwable子类中，三种基本的异常类提供了带cause参数的构造器。它们是：**Error，Exception，RuntimeException**。如果要把其他类型的异常链接起来，应该使用**initCause()**方法而不是构造器。

示例：

```
//: exceptions/DynamicFields.java
Error Handling with Exceptions 327
// A Class that dynamically adds fields to itself.
// Demonstrates exception chaining.
import static net.mindview.util.Print.*;
class DynamicFieldsException extends Exception {}
public class DynamicFields {
private Object[][] fields;
public DynamicFields(int initialSize) {
fields = new Object[initialSize][2];
for(int i = 0; i < initialSize; i++)
fields[i] = new Object[] { null, null };
}
public String toString() {
StringBuilder result = new StringBuilder();
for(Object[] obj : fields) {
result.append(obj[0]);
result.append(": ");
result.append(obj[1]);
result.append("\n");
}
return result.toString();
}
private int hasField(String id) {
for(int i = 0; i < fields.length; i++)
if(id.equals(fields[i][0]))
return i;
return -1;
}
private int
getFieldNumber(String id) throws NoSuchFieldException {
int fieldNum = hasField(id);
if(fieldNum == -1)
throw new NoSuchFieldException();
return fieldNum;
}
private int makeField(String id) {
for(int i = 0; i < fields.length; i++)
if(fields[i][0] == null) {
fields[i][0] = id;
return i;
}
// No empty fields. Add one:
Object[][] tmp = new Object[fields.length + 1][2];
for(int i = 0; i < fields.length; i++)
tmp[i] = fields[i];
for(int i = fields.length; i < tmp.length; i++)
tmp[i] = new Object[] { null, null };
fields = tmp;
// Recursive call with expanded fields:
return makeField(id);
}
public Object
getField(String id) throws NoSuchFieldException {
return fields[getFieldNumber(id)][1];
}
public Object setField(String id, Object value)
throws DynamicFieldsException {
if(value == null) {
// Most exceptions don’t have a "cause" constructor.
// In these cases you must use initCause(),
// available in all Throwable subclasses.
DynamicFieldsException dfe =
328 Thinking in Java Bruce Eckel
new DynamicFieldsException();
dfe.initCause(new NullPointerException());
throw dfe;
}
int fieldNumber = hasField(id);
if(fieldNumber == -1)
fieldNumber = makeField(id);
Object result = null;
try {
result = getField(id); // Get old value
} catch(NoSuchFieldException e) {
// Use constructor that takes "cause":
throw new RuntimeException(e);
}
fields[fieldNumber][1] = value;
return result;
}
public static void main(String[] args) {
DynamicFields df = new DynamicFields(3);
print(df);
try {
df.setField("d", "A value for d");
df.setField("number", 47);
df.setField("number2", 48);
print(df);
df.setField("d", "A new value for d");
df.setField("number3", 11);
print("df: " + df);
print("df.getField(\"d\") : " + df.getField("d"));
Object field = df.setField("d", null); // Exception
} catch(NoSuchFieldException e) {
e.printStackTrace(System.out);
} catch(DynamicFieldsException e) {
e.printStackTrace(System.out);
}
}
} /* Output:
null: null
null: null
null: null
d: A value for d
number: 47
number2: 48
df: d: A new value for d
number: 47
number2: 48
number3: 11
df.getField("d") : A new value for d
DynamicFieldsException
at DynamicFields.setField(DynamicFields.java:64)
at DynamicFields.main(DynamicFields.java:94)
Caused by: java.lang.NullPointerException
at DynamicFields.setField(DynamicFields.java:66)
... 1 more
*///:~
```







## 12.7 Java标准

**Throwable对象可分为两种类型**：**Error**用来表示编译时和系统错误；**Exception**是可以被抛出的基本类型，在Java类库，用户方法及运行时故障中都可能抛出Exception型异常。所以Java程序员关心的基本类型通常是Exception。

### 12.7.1 RuntimeException

属于运行性异常的类型有很多，它们会自动被Java虚拟机抛出，所以不必在异常说明中把它们列出来。这些异常都是从RuntimeException类继承而来。不需要在异常说明中声明方法将抛出RuntimeException类型的异常。它们也被称为“**不受检查异常**”。这种异常属于错误，将被自动捕获。

如果RuntimeException没有被捕获而直达main()，那么在程序退出前将调用异常的printStackTrace()方法。

**<font color=red>注意</font>**：只能在代码中忽略RuntimeException类型的异常，其他类型异常的处理都是由编译器强制实施的。

## 12.8 使用finally进行清理

**对于一些代码，无论try块中的异常是否抛出，它们都能得到执行**，这通常适用与内存回收之外的情况。为了达到这个效果，可以在异常处理程序后面加上**finally子句**。

示例：

```
//: exceptions/FinallyWorks.java
// The finally clause is always executed.
class ThreeException extends Exception {}
public class FinallyWorks {
static int count = 0;
public static void main(String[] args) {
while(true) {
try {
4 C++ exception handling does not have the finally clause because it relies on destructors to accomplish this sort of cleanup.
Error Handling with Exceptions 333
// Post-increment is zero first time:
if(count++ == 0)
throw new ThreeException();
System.out.println("No exception");
} catch(ThreeException e) {
System.out.println("ThreeException");
} finally {
System.out.println("In finally clause");
if(count == 2) break; // out of "while"
}
}
}
} /* Output:
ThreeException
In finally clause
No exception
In finally clause
*///:~
```

### 12.8.1 final用来做什么

**当要把除内存之外的资源恢复到它们的初始状态时，就要用到finally子句**。这种需要清理的资源包括：**<font color=red>已经打开的文件或网络连接，在屏幕上画的图形，甚至可以是外部世界的某个开关</font>**。

示例：

```
//: exceptions/WithFinally.java
// Finally Guarantees cleanup.
public class WithFinally {
static Switch sw = new Switch();
public static void main(String[] args) {
try {
sw.on();
// Code that can throw exceptions...
OnOffSwitch.f();
} catch(OnOffException1 e) {
System.out.println("OnOffException1");
} catch(OnOffException2 e) {
System.out.println("OnOffException2");
} finally {
sw.off();
}
}
} /* Output:
on
off
*///:~
```

在异常没有被当前的异常处理程序捕获的情况下，异常处理机制也会在跳到更高一层的异常处理程序之前，执行finally子句。

示例：

```
//: exceptions/AlwaysFinally.java
// Finally is always executed.
import static net.mindview.util.Print.*;
class FourException extends Exception {}
public class AlwaysFinally {
public static void main(String[] args) {
print("Entering first try block");
try {
print("Entering second try block");
try {
throw new FourException();
} finally {
print("finally in 2nd try block");
}
} catch(FourException e) {
System.out.println(
"Caught FourException in 1st try block");
} finally {
System.out.println("finally in 1st try block");
}
}
} /* Output:
Entering first try block
Entering second try block
finally in 2nd try block
Caught FourException in 1st try block
finally in 1st try block
*///:~
```

### 12.8.2 在return中使用finally

因为finally子句总是会执行，所以在一个方法中，可以从多个点返回，并且可以保证重要的清理工作仍旧会执行。

### 12.8.3 缺憾：异常丢失

Java的异常实现也有瑕疵。异常作为程序出错的标志，决不应该被忽略，但它还是有可能被轻易地忽略。

示例：

```
//: exceptions/LostMessage.java
// How an exception can be lost.
class VeryImportantException extends Exception {
public String toString() {
336 Thinking in Java Bruce Eckel
return "A very important exception!";
}
}
class HoHumException extends Exception {
public String toString() {
return "A trivial exception";
}
}
public class LostMessage {
void f() throws VeryImportantException {
throw new VeryImportantException();
}
void dispose() throws HoHumException {
throw new HoHumException();
}
public static void main(String[] args) {
try {
LostMessage lm = new LostMessage();
try {
lm.f();
} finally {
lm.dispose();
}
} catch(Exception e) {
System.out.println(e);
}
}
} /* Output:
A trivial exception
```

```
//: exceptions/ExceptionSilencer.java
public class ExceptionSilencer {
public static void main(String[] args) {
try {
throw new RuntimeException();
} finally {
// Using ‘return’ inside the finally block
// will silence any thrown exception.
return;
}
}
} ///:~
```

## 12.9 异常的限制

**当覆盖方法时，只能抛出在基类方法的异常说明里列出的那些异常**。

示例：

```
//: exceptions/StormyInning.java
// Overridden methods may throw only the exceptions
// specified in their base-class versions, or exceptions
// derived from the base-class exceptions.
class BaseballException extends Exception {}
class Foul extends BaseballException {}
class Strike extends BaseballException {}
abstract class Inning {
public Inning() throws BaseballException {}
public void event() throws BaseballException {
// Doesn’t actually have to throw anything
}
public abstract void atBat() throws Strike, Foul;
public void walk() {} // Throws no checked exceptions
}
class StormException extends Exception {}
class RainedOut extends StormException {}
class PopFoul extends Foul {}
interface Storm {
public void event() throws RainedOut;
public void rainHard() throws RainedOut;
}
public class StormyInning extends Inning implements Storm {
// OK to add new exceptions for constructors, but you
// must deal with the base constructor exceptions:
public StormyInning()
throws RainedOut, BaseballException {}
public StormyInning(String s)
throws Foul, BaseballException {}
// Regular methods must conform to base class:
//! void walk() throws PopFoul {} //Compile error
// Interface CANNOT add exceptions to existing
// methods from the base class:
//! public void event() throws RainedOut {}
// If the method doesn’t already exist in the
// base class, the exception is OK:
public void rainHard() throws RainedOut {}
// You can choose to not throw any exceptions,
338 Thinking in Java Bruce Eckel
// even if the base version does:
public void event() {}
// Overridden methods can throw inherited exceptions:
public void atBat() throws PopFoul {}
public static void main(String[] args) {
try {
StormyInning si = new StormyInning();
si.atBat();
} catch(PopFoul e) {
System.out.println("Pop foul");
} catch(RainedOut e) {
System.out.println("Rained out");
} catch(BaseballException e) {
System.out.println("Generic baseball exception");
}
// Strike not thrown in derived version.
try {
// What happens if you upcast?
Inning i = new StormyInning();
i.atBat();
// You must catch the exceptions from the
// base-class version of the method:
} catch(Strike e) {
System.out.println("Strike");
} catch(Foul e) {
System.out.println("Foul");
} catch(RainedOut e) {
System.out.println("Rained out");
} catch(BaseballException e) {
System.out.println("Generic baseball exception");
}
}
} ///:~
```

## 12.10  构造器

涉及到构造器时，构造器会把对象设置为安全的初始状态，但还会有别的动作。比如打开一个文件，这样的动作只有在对象使用完毕并且用户调用了特殊的清理方法之后才能得以清理。如果在构造器内抛出了异常，这些清理行为也许就不能正常工作了。

示例：

```
//: exceptions/InputFile.java
// Paying attention to exceptions in constructors.
import java.io.*;
6 ISO C++ added similar constraints that require derived-method exceptions to be the same as, or derived from, the exceptions thrown by the base-class method. This is one case in which C++ is actually able to check exception specifications at compile time.
public class InputFile {
private BufferedReader in;
public InputFile(String fname) throws Exception {
try {
in = new BufferedReader(new FileReader(fname));
// Other code that might throw exceptions
} catch(FileNotFoundException e) {
System.out.println("Could not open " + fname);
// Wasn’t open, so don’t close it
throw e;
} catch(Exception e) {
// All other exceptions must close it
try {
in.close();
} catch(IOException e2) {
System.out.println("in.close() unsuccessful");
}
throw e; // Rethrow
} finally {
// Don’t close it here!!!
}
}
public String getLine() {
String s;
try {
s = in.readLine();
} catch(IOException e) {
throw new RuntimeException("readLine() failed");
}
return s;
}
public void dispose() {
try {
in.close();
System.out.println("dispose() successful");
} catch(IOException e2) {
throw new RuntimeException("in.close() failed");
}
}
} ///:~
```

对于在构造器阶段可能抛出异常，并且要求清理的类，最安全的使用方式是使用**嵌套的try子句**。

示例：

```
//: exceptions/Cleanup.java
// Guaranteeing proper cleanup of a resource.
public class Cleanup {
public static void main(String[] args) {
try {
InputFile in = new InputFile("Cleanup.java");
try {
String s;
int i = 1;
while((s = in.getLine()) != null)
; // Perform line-by-line processing here...
} catch(Exception e) {
System.out.println("Caught Exception in main");
e.printStackTrace(System.out);
} finally {
in.dispose();
}
} catch(Exception e) {
System.out.println("InputFile construction failed");
}
}
} /* Output:
dispose() successful
*///:~
```

对InputFile对象的构造在其自己的try语句块中有效，如果构造失败，将进入外部的catch子句，而dispose()方法不会被调用。但是，如果构造成功，我们肯定想确保对象能够被清理，因此在构造之后立即创建了一个新的try语句块。**执行清理的finally与内部的try语句块相关联。这种方式中，finally子句在构造失败是不会执行的，而在构成成功时将总是执行**。

这种通用的清理惯用法在构造器不抛出任何异常时也应该应用，**基本规则是**：**<font color =red>在创建需要清理的对象之后，立即进入一个try-finally语句块。</font>**

## 12.11 异常匹配

抛出异常的时候，异常处理系统会按照代码的书写顺序找出“最近”的处理程序。找到匹配的处理程序之后，它就认为异常将得到处理，然后就不再继续查找。

不要求抛出的异常同处理程序所声明的异常完全匹配。派生类的对象也可以匹配其基类的处理程序。**如果把捕获基类的catch子句放在最前面，这就会把派生类的异常全给屏蔽掉**。



## 12.12 其他可选方式

**异常处理的一个重要原则**：只有你在知道如何处理的情况下才捕获异常。

**异常处理的一个重要目标**：把错误处理的代码同错误发生的地点相分离。

被检查的异常使问题复杂化。它们强制你在可能还没准备好处理错误的时候被迫加上catch子句，这就导致了**吞食则有害**的问题。

```
try {
throw new Sneeze();
} catch(Annoyance a) {
// ...
} catch(Sneeze s) {
// ...
}
```

一旦这么做，虽然能通过编译，除非记得复查并改正代码，否则异常将会丢失。异常确实发生了，但“吞食”后它却完全消失了，因为编译器强迫你立刻写代码来处理异常。

### 12.12.1 把异常传递给控制台

最简单而又不用写多少代码就能保护异常信息的方法，就是把它们从main()传递到控制台。

示例：

```
//: exceptions/MainException.java
import java.io.*;
public class MainException {
// Pass all exceptions to the console:
public static void main(String[] args) throws Exception {
// Open the file:
FileInputStream file =
new FileInputStream("MainException.java");
// Use the file ...
// Close the file:
file.close();
}
} ///:~
```

main()作为一个方法也可以有异常说明，这里异常的类型是Exception。通过把它传递到控制台，就不必再main()里写try-catch子句了。

### 12.12.2 把“被检查的异常”转换为“不检查的异常”

编写自己使用的简单代码，从main()中抛出异常是很方便的，但是不是通用的方法。在一个普通方法里调用别的方法，要考虑“我不知道该怎么处理这个异常，但是又不想把它吞了，或者打印一些无用的信息“。JDK 1.4 的异常链提供了一种新的思路来解决这个问题：直接把”被检查的异常“包装进RuntimeException里面：

```
t r y {
// ... to do something useful
} catch(IDontKnowWhatToDoWithThisCheckedException e) {
throw new RuntimeException(e);
}
```

这种技巧给了你一种选择，你可以不写try-catch子句和/或异常说明，直接忽略异常，让它自己沿着调用栈往上“冒泡”。同时还可以用getCause()捕获并处理特定的异常。

示例：

```
//: exceptions/TurnOffChecking.java
// "Turning off" Checked exceptions.
import java.io.*;
import static net.mindview.util.Print.*;
class WrapCheckedException {
void throwRuntimeException(int type) {
try {
switch(type) {
case 0: throw new FileNotFoundException();
case 1: throw new IOException();
case 2: throw new RuntimeException("Where am I?");
default: return;
}
} catch(Exception e) { // Adapt to unchecked:
throw new RuntimeException(e);
}
}
}
class SomeOtherException extends Exception {}
public class TurnOffChecking {
public static void main(String[] args) {
WrapCheckedException wce = new WrapCheckedException();
// You can call throwRuntimeException() without a try
350 Thinking in Java Bruce Eckel
// block, and let RuntimeExceptions leave the method:
wce.throwRuntimeException(3);
// Or you can choose to catch exceptions:
for(int i = 0; i < 4; i++)
try {
if(i < 3)
wce.throwRuntimeException(i);
else
throw new SomeOtherException();
} catch(SomeOtherException e) {
print("SomeOtherException: " + e);
} catch(RuntimeException re) {
try {
throw re.getCause();
} catch(FileNotFoundException e) {
print("FileNotFoundException: " + e);
} catch(IOException e) {
print("IOException: " + e);
} catch(Throwable e) {
print("Throwable: " + e);
}
}
}
} /* Output:
FileNotFoundException: java.io.FileNotFoundException
IOException: java.io.IOException
Throwable: java.lang.RuntimeException: Where am I?
SomeOtherException: SomeOtherException
*///:~
```

## 12.13 异常使用指南

**应该在下列情况下使用异常**：

1. 在恰当的级别处理问题。（在知道该如何处理的情况下才捕获异常）
2. 解决问题并重新调用产生异常的方法
3. 进行少许修补，然后绕过异常发生的地方继续执行。
4. 用别的数据进行计算，用代替方法预计会返回的值
5. 把当前运行环境下能做的事情尽量做完，然后把相同的异常抛到更高层
6. 把当前运行环境下能做的事情尽量做完，然后把不同的异常抛到更高层
7. 终止程序
8. 进行简化（如果你的异常模式使问题变得太复杂，那用起来太麻烦）
9. 让类库和程序更安全

# 第13 章 字符串

## 13.1 不可变String

**String是不可变的**。String类中每一个看起来会修改String值的方法，实际上都是创建了一个全新的String对象，以包含修改后的字符串内容。最初的String对象丝毫未动。

示例：

```
//: strings/Immutable.java
import static net.mindview.util.Print.*;
public class Immutable {
public static String upcase(String s) {
return s.toUpperCase();
}
public static void main(String[] args) {
String q = "howdy";
print(q); // howdy
String qq = upcase(q);
print(qq); // HOWDY
print(q); // howdy
}
} /* Output:
howdy
HOWDY
howdy
*///:~
```

## 13.2 重载 ‘+’ 与StringBuilder

**String具有只读特性**，指向它的任何引用都不可能改变它的值。

不可变会带来效率问题。**操作符‘+’可以用来连接String**

示例：

```
//: strings/Concatenation.java
public class Concatenation {
public static void main(String[] args) {
String mango = "mango";
String s = "abc" + mango + "def" + 47;
System.out.println(s);
}
} /* Output:
abcmangodef47
*///:~
```

可硬用JDK自带的工具javap来反编译以上代码。

`javap -c Concatenation`

从jvm字节码中可以看到：编译器自动引入了java.lang.StringBuilder类。它更高效。

如果你要在toString()方法中使用循环，那么最好自己创建一个StringBuilder对象，用它来构造最终结果。

示例：

```
//: strings/UsingStringBuilder.java
import java.util.*;
public class UsingStringBuilder {
public static Random rand = new Random(47);
public String toString() {
StringBuilder result = new StringBuilder("[");
for(int i = 0; i < 25; i++) {
result.append(rand.nextInt(100));
result.append(", ");
}
result.delete(result.length()-2, result.length());
result.append("]");
return result.toString();
}
public static void main(String[] args) {
UsingStringBuilder usb = new UsingStringBuilder();
System.out.println(usb);
}
} /* Output:
[58, 55, 93, 61, 61, 29, 68, 0, 22, 7, 88, 28, 51, 89, 9, 78, 98, 61, 20, 58, 16, 40, 11, 22, 4]
*///:~
```

StringBuilder提供了丰富全面的方法，包括：insert()、replace()、substring()、reverse()、append()、toString()、delete().

## 13.3 无意识的递归

如果你希望toString()方法打印出对象的内存地址，或许会考虑使用this关键字：

```
//: strings/InfiniteRecursion.java
// Accidental recursion.
// {RunByHand}
import java.util.*;

public class InfiniteRecursion {
  public String toString() {
    return " InfiniteRecursion address: " + this + "\n";
  }
  public static void main(String[] args) {
    List<InfiniteRecursion> v =
      new ArrayList<InfiniteRecursion>();
    for(int i = 0; i < 10; i++)
      v.add(new InfiniteRecursion());
    System.out.println(v);
  }
} ///:~
```

当你创建了InfiniteRecursion对象，并打印出来，会得到一串非常长的异常。当代码运行`"InfiniteRecursion address : " + this`发生了自动转换，编译器试着将this转换为一个String，通过调用this上的toString()方法，于是就发生了递归。所以你不应该使用this，应该调用**super.toString()**方法。

## 13.4 String上的操作

|             方法             | 参数，重载版本 | 应用 |
| :--------------------------: | :------------: | :--: |
|            构造器            |                |      |
|           length()           |                |      |
|           charAt()           |                |      |
|    getChars(), getBytes()    |                |      |
|        toCharArray()         |                |      |
| equals(), equalsIgnoreCase() |                |      |
|         compareTo()          |                |      |
|          contains()          |                |      |
|       contentEquals()        |                |      |
|      equalsIgnoreCase()      |                |      |
|       regionMatcher()        |                |      |
|         startsWith()         |                |      |
|          endsWith()          |                |      |
|   indexOf(), lastIndexOf()   |                |      |
| substring() (subSequence())  |                |      |
|           concat()           |                |      |
|          replace()           |                |      |
| toLowerCase() toUpperCase()  |                |      |
|            trim()            |                |      |
|          valueOf()           |                |      |
|           intern()           |                |      |

## 13.5 格式化输出

### 13.5.1 printf()

C语言中的printf()并不能像java那样连接字符串，他使用一个简单的格式化字符串，加上要插入其中的值，然后将其格式化输出。

`printf("Row 1: [%d %f]\n", x, y);`

### 13.5.2 System.out.format()

java SE5引入的format方法可用于**PrintStream或PrintWriter对象**。**format()方法模仿自C的printf().**

示例：

```
//: strings/SimpleFormat.java

public class SimpleFormat {
  public static void main(String[] args) {
    int x = 5;
    double y = 5.332542;
    // The old way:
    System.out.println("Row 1: [" + x + " " + y + "]");
    // The new way:
    System.out.format("Row 1: [%d %f]\n", x, y);
    // or
    System.out.printf("Row 1: [%d %f]\n", x, y);
  }
} /* Output:
Row 1: [5 5.332542]
Row 1: [5 5.332542]
Row 1: [5 5.332542]
*///:~
```

### 13.5.3 Formatter类

在java中，所有新的格式化功能都由java.util.Formatter类处理。

示例：

```
//: strings/Turtle.java
import java.io.*;
import java.util.*;

public class Turtle {
  private String name;
  private Formatter f;
  public Turtle(String name, Formatter f) {
    this.name = name;
    this.f = f;
  }
  public void move(int x, int y) {
    f.format("%s The Turtle is at (%d,%d)\n", name, x, y);
  }
  public static void main(String[] args) {
    PrintStream outAlias = System.out;
    Turtle tommy = new Turtle("Tommy",
      new Formatter(System.out));
    Turtle terry = new Turtle("Terry",
      new Formatter(outAlias));
    tommy.move(0,0);
    terry.move(4,8);
    tommy.move(3,4);
    terry.move(2,5);
    tommy.move(3,3);
    terry.move(3,3);
  }
} /* Output:
Tommy The Turtle is at (0,0)
Terry The Turtle is at (4,8)
Tommy The Turtle is at (3,4)
Terry The Turtle is at (2,5)
Tommy The Turtle is at (3,3)
Terry The Turtle is at (3,3)
*///:~
```

### 13.5.4 格式化说明符

插入数据时，如果想要控制空格和对齐，需要更加精细复杂的格式修饰符：

```
%[argument_index$][flags][width][.precision]conversion
```

**默认情况下，数据是右对齐**，不过可以通过使用“-”标志来改变对齐方向。**与width相对的是precision**，用来指明最大尺寸。不是所有类型的数据都能使用precision，而且，应用不同类型的数据转换时，precision的意义也不同。**将precision应用于String时，表示打印String时输出字符的最大数量。将precision应用于浮点数时，表示小数部分要显示出来的位数（默认6位。）**小数位过多舍去，过少补零。整数无小数位，无法应用precision，否则报异常。

示例：

```
//: strings/Receipt.java
import java.util.*;

public class Receipt {
  private double total = 0;
  private Formatter f = new Formatter(System.out);
  public void printTitle() {
    f.format("%-15s %5s %10s\n", "Item", "Qty", "Price");
    f.format("%-15s %5s %10s\n", "----", "---", "-----");
  }
  public void print(String name, int qty, double price) {
    f.format("%-15.15s %5d %10.2f\n", name, qty, price);
    total += price;
  }
  public void printTotal() {
    f.format("%-15s %5s %10.2f\n", "Tax", "", total*0.06);
    f.format("%-15s %5s %10s\n", "", "", "-----");
    f.format("%-15s %5s %10.2f\n", "Total", "",
      total * 1.06);
  }
  public static void main(String[] args) {
    Receipt receipt = new Receipt();
    receipt.printTitle();
    receipt.print("Jack's Magic Beans", 4, 4.25);
    receipt.print("Princess Peas", 3, 5.1);
    receipt.print("Three Bears Porridge", 1, 14.29);
    receipt.printTotal();
  }
} /* Output:
Item              Qty      Price
----              ---      -----
Jack's Magic Be     4       4.25
Princess Peas       3       5.10
Three Bears Por     1      14.29
Tax                         1.42
                           -----
Total                      25.06
*///:~
```

### 13.5.5 Formatter转换

下列表格包含了最常用的类型转换：

|  d   |  整数型（十进制）  |
| :--: | :----------------: |
|  c   |    Unicode字符     |
|  b   |     Boolean值      |
|  s   |       String       |
|  f   |  浮点数（十进制）  |
|  e   | 浮点数（科学计数） |
|  x   |  整数（十六进制）  |
|  h   | 散列码（十六进制） |
|  %   |      字符“%”       |

示例：

```
//: strings/Conversion.java
import java.math.*;
import java.util.*;

public class Conversion {
  public static void main(String[] args) {
    Formatter f = new Formatter(System.out);

    char u = 'a';
    System.out.println("u = 'a'");
    f.format("s: %s\n", u);
    // f.format("d: %d\n", u);
    f.format("c: %c\n", u);
    f.format("b: %b\n", u);
    // f.format("f: %f\n", u);
    // f.format("e: %e\n", u);
    // f.format("x: %x\n", u);
    f.format("h: %h\n", u);

    int v = 121;
    System.out.println("v = 121");
    f.format("d: %d\n", v);
    f.format("c: %c\n", v);
    f.format("b: %b\n", v);
    f.format("s: %s\n", v);
    // f.format("f: %f\n", v);
    // f.format("e: %e\n", v);
    f.format("x: %x\n", v);
    f.format("h: %h\n", v);

    BigInteger w = new BigInteger("50000000000000");
    System.out.println(
      "w = new BigInteger(\"50000000000000\")");
    f.format("d: %d\n", w);
    // f.format("c: %c\n", w);
    f.format("b: %b\n", w);
    f.format("s: %s\n", w);
    // f.format("f: %f\n", w);
    // f.format("e: %e\n", w);
    f.format("x: %x\n", w);
    f.format("h: %h\n", w);

    double x = 179.543;
    System.out.println("x = 179.543");
    // f.format("d: %d\n", x);
    // f.format("c: %c\n", x);
    f.format("b: %b\n", x);
    f.format("s: %s\n", x);
    f.format("f: %f\n", x);
    f.format("e: %e\n", x);
    // f.format("x: %x\n", x);
    f.format("h: %h\n", x);

    Conversion y = new Conversion();
    System.out.println("y = new Conversion()");
    // f.format("d: %d\n", y);
    // f.format("c: %c\n", y);
    f.format("b: %b\n", y);
    f.format("s: %s\n", y);
    // f.format("f: %f\n", y);
    // f.format("e: %e\n", y);
    // f.format("x: %x\n", y);
    f.format("h: %h\n", y);

    boolean z = false;
    System.out.println("z = false");
    // f.format("d: %d\n", z);
    // f.format("c: %c\n", z);
    f.format("b: %b\n", z);
    f.format("s: %s\n", z);
    // f.format("f: %f\n", z);
    // f.format("e: %e\n", z);
    // f.format("x: %x\n", z);
    f.format("h: %h\n", z);
  }
} /* Output: (Sample)
u = 'a'
s: a
c: a
b: true
h: 61
v = 121
d: 121
c: y
b: true
s: 121
x: 79
h: 79
w = new BigInteger("50000000000000")
d: 50000000000000
b: true
s: 50000000000000
x: 2d79883d2000
h: 8842a1a7
x = 179.543
b: true
s: 179.543
f: 179.543000
e: 1.795430e+02
h: 1ef462c
y = new Conversion()
b: true
s: Conversion@9cab16
h: 9cab16
z = false
b: false
s: false
h: 4d5
*///:~
```

### 13.5.6 String.format()

**String.format()方法是一种static方法**，它接受与Formatter.format()方法一样的参数，但返回一个String对象。

示例：

```
//: strings/DatabaseException.java

public class DatabaseException extends Exception {
  public DatabaseException(int transactionID, int queryID,
    String message) {
    super(String.format("(t%d, q%d) %s", transactionID,
        queryID, message));
  }
  public static void main(String[] args) {
    try {
      throw new DatabaseException(3, 7, "Write failed");
    } catch(Exception e) {
      System.out.println(e);
    }
  }
} /* Output:
DatabaseException: (t3, q7) Write failed
*///:~
```

```
//: net/mindview/util/Hex.java
package net.mindview.util;
import java.io.*;

public class Hex {
  public static String format(byte[] data) {
    StringBuilder result = new StringBuilder();
    int n = 0;
    for(byte b : data) {
      if(n % 16 == 0)
        result.append(String.format("%05X: ", n));
      result.append(String.format("%02X ", b));
      n++;
      if(n % 16 == 0) result.append("\n");
    }
    result.append("\n");
    return result.toString();
  }
  public static void main(String[] args) throws Exception {
    if(args.length == 0)
      // Test by displaying this class file:
      System.out.println(
        format(BinaryFile.read("Hex.class")));
    else
      System.out.println(
        format(BinaryFile.read(new File(args[0]))));
  }
} /* Output: (Sample)
00000: CA FE BA BE 00 00 00 31 00 52 0A 00 05 00 22 07
00010: 00 23 0A 00 02 00 22 08 00 24 07 00 25 0A 00 26
00020: 00 27 0A 00 28 00 29 0A 00 02 00 2A 08 00 2B 0A
00030: 00 2C 00 2D 08 00 2E 0A 00 02 00 2F 09 00 30 00
00040: 31 08 00 32 0A 00 33 00 34 0A 00 15 00 35 0A 00
00050: 36 00 37 07 00 38 0A 00 12 00 39 0A 00 33 00 3A
...
*///:~
```

## 13.6 正则表达式

java中，字符串操作主要集中在**String，StringBuffer和StringTokenizer**类。与正则表达式相比较，它们只能提供相当简单的功能。

正则表达式提供了一种完全通用的方式，能够解决各种字符串处理相关问题：**匹配，选择，编辑和验证**。

### 13.6.1 基础

正则表达式就是以某种方式描述字符串。

找一个数，可能有一个负号在最前面：`-?`

`\d`表示一位数字。

`\\`表示插入一个正则表达式的反斜线，后面的字符具有特殊意义

`\\\\`表示插入一个普通反斜线

`+`表示一个或多个之前的表达式

`-?\\d+`表示可能有一个负号，后面跟着一位或多位数字

**利用String类內建的功能**，可以检查一个String是否匹配如上所述正则表达式。

示例：

```
//: strings/IntegerMatch.java

public class IntegerMatch {
  public static void main(String[] args) {
    System.out.println("-1234".matches("-?\\d+"));
    System.out.println("5678".matches("-?\\d+"));
    System.out.println("+911".matches("-?\\d+"));
    System.out.println("+911".matches("(-|\\+)?\\d+"));
  }
} /* Output:
true
true
false
true
*///:~
```

正则表达式中，括号有着分组的效果，而竖直线`|`则表示或操作。因为+在正则表达式中有其他含义，所以必须使用\\将其转义。

`(-|\\+)？`

String类自带了一个正则表达工具——**split()**方法，功能是：将字符串从正则表达式匹配的地方切开

**示例**：

```
//: strings/Splitting.java
import java.util.*;

public class Splitting {
  public static String knights =
    "Then, when you have found the shrubbery, you must " +
    "cut down the mightiest tree in the forest... " +
    "with... a herring!";
  public static void split(String regex) {
    System.out.println(
      Arrays.toString(knights.split(regex)));
  }
  public static void main(String[] args) {
    split(" "); // Doesn't have to contain regex chars
    split("\\W+"); // Non-word characters
    split("n\\W+"); // 'n' followed by non-word characters
  }
} /* Output:
[Then,, when, you, have, found, the, shrubbery,, you, must, cut, down, the, mightiest, tree, in, the, forest..., with..., a, herring!]
[Then, when, you, have, found, the, shrubbery, you, must, cut, down, the, mightiest, tree, in, the, forest, with, a, herring]
[The, whe, you have found the shrubbery, you must cut dow, the mightiest tree i, the forest... with... a herring!]
*///:~
```

`\W`表示非单词字符（单词小写为`\w`，表示一个单词字符）

原始字符串中，与正则表达式匹配的部分，在最终结果中都不存在了。

**String.split()还有一个重载的版本，允许限制字符串分割的次数**

String类自带的最后一个正则表达式工具是替换。

**示例**：

```
//: strings/Replacing.java
import static net.mindview.util.Print.*;

public class Replacing {
  static String s = Splitting.knights;
  public static void main(String[] args) {
    print(s.replaceFirst("f\\w+", "located"));
    print(s.replaceAll("shrubbery|tree|herring","banana"));
  }
} /* Output:
Then, when you have located the shrubbery, you must cut down the mightiest tree in the forest... with... a herring!
Then, when you have found the banana, you must cut down the mightiest banana in the forest... with... a banana!
*///:~
```

### 13.6.2 创建正则表达式

![](F:\tempphoto\1.PNG)

**创建字符类的典型方式**以及一些预定义的类：

![](F:\tempphoto\2.PNG)

逻辑操作符：

![](F:\tempphoto\3.PNG)

![](F:\tempphoto\4.PNG)

**示例**：

```
//: strings/Rudolph.java

public class Rudolph {
  public static void main(String[] args) {
    for(String pattern : new String[]{ "Rudolph",
      "[rR]udolph", "[rR][aeiou][a-z]ol.*", "R.*" })
      System.out.println("Rudolph".matches(pattern));
  }
} /* Output:
true
true
true
true
*///:~
```

### 13.6.3 量词

- **贪婪型**：为所有可能的模式发现尽可能多的匹配。
- **勉强型**：用问号来指定，匹配满足模式所需的最少字符数
- **占有型**：只有在Java语言中才有，常常用于防止正则表达式失控。

![](F:\tempphoto\5.PNG)

表达式X必须用圆括号括起来。

**CharSequence**

接口CharSequence从CharBuffer、String、StringBuffer、StringBuilder类之中抽象出了字符序列的一般化定义：

```
interface CharSequence {
	charAt(int i);
	length();
	subSequence(int start,| int end);
	toString();
}
```

这些类都实现了该接口。

### 13.6.4 Pattern和Matcher

构建正则表达式对象。只需导入java.util.regex包，然后用**static Pattern.compile()**方法来编译你的正则表达式。你的String类型的正则表达式生成一个Pattern对象。接下来，把你想要检索的字符串传入Pattern对象的Matcher()方法。matcher()方法会生成一个Matcher对象。

**示例**：

```
//: strings/TestRegularExpression.java
// Allows you to easily try out regular expressions.
// {Args: abcabcabcdefabc "abc+" "(abc)+" "(abc){2,}" }
import java.util.regex.*;
import static net.mindview.util.Print.*;

public class TestRegularExpression {
  public static void main(String[] args) {
    if(args.length < 2) {
      print("Usage:\njava TestRegularExpression " +
        "characterSequence regularExpression+");
      System.exit(0);
    }
    print("Input: \"" + args[0] + "\"");
    for(String arg : args) {
      print("Regular expression: \"" + arg + "\"");
      Pattern p = Pattern.compile(arg);
      Matcher m = p.matcher(args[0]);
      while(m.find()) {
        print("Match \"" + m.group() + "\" at positions " +
          m.start() + "-" + (m.end() - 1));
      }
    }
  }
} /* Output:
Input: "abcabcabcdefabc"
Regular expression: "abcabcabcdefabc"
Match "abcabcabcdefabc" at positions 0-14
Regular expression: "abc+"
Match "abc" at positions 0-2
Match "abc" at positions 3-5
Match "abc" at positions 6-8
Match "abc" at positions 12-14
Regular expression: "(abc)+"
Match "abcabcabc" at positions 0-8
Match "abc" at positions 12-14
Regular expression: "(abc){2,}"
Match "abcabcabc" at positions 0-8
*///:~
```

通过调用Pattern.matcher()方法，并传入一个字符串参数，我们得到了一个Matcher对象。使用Matcher上的方法，我们将能够判断各种不同类型的匹配是否成功：

```
boolean matches()
boolean lookingAt()
loolean find()
boolean find(int start)
```

其中**matches()**方法用来判断整个输入字符串是否匹配正则表达式模式，而**lookingAt()**用来判断该字符串的始部分是否能够匹配模式。



**find()**

Matcher.find()方法可用来在CharSequence中查找多个匹配：

**示例**：

```
//: strings/Finding.java
import java.util.regex.*;
import static net.mindview.util.Print.*;

public class Finding {
  public static void main(String[] args) {
    Matcher m = Pattern.compile("\\w+")
      .matcher("Evening is full of the linnet's wings");
    while(m.find())
      printnb(m.group() + " ");
    print();
    int i = 0;
    while(m.find(i)) {
      printnb(m.group() + " ");
      i++;
    }
  }
} /* Output:
Evening is full of the linnet s wings
Evening vening ening ning ing ng g is is s full full ull ll l of of f the the he e linnet linnet innet nnet net et t s s wings wings ings ngs gs s
*///:~
```

第二个find()能够接收一个整数作为参数，该整数表示字符串中字符的位置，并以其作为搜索的起点。

**组(Groups)**

组是用括号划分的正则表达式，可以根据组的编号来引用某个组。组号为0表示整个表达式，组号为1表示被第一对括号括起来的组，依次类推。

- public int groupCount()：返回该匹配器的模式中的分组数，0组不包括在内
- public String group()：返回前一次匹配操作的第0组
- public String group(int i)：返回在前一次匹配操作期间指定的组号
- 。。。。。

**示例**：

```
//: strings/Groups.java
import java.util.regex.*;
import static net.mindview.util.Print.*;

public class Groups {
  static public final String POEM =
    "Twas brillig, and the slithy toves\n" +
    "Did gyre and gimble in the wabe.\n" +
    "All mimsy were the borogoves,\n" +
    "And the mome raths outgrabe.\n\n" +
    "Beware the Jabberwock, my son,\n" +
    "The jaws that bite, the claws that catch.\n" +
    "Beware the Jubjub bird, and shun\n" +
    "The frumious Bandersnatch.";
  public static void main(String[] args) {
    Matcher m =
      Pattern.compile("(?m)(\\S+)\\s+((\\S+)\\s+(\\S+))$")
        .matcher(POEM);
    while(m.find()) {
      for(int j = 0; j <= m.groupCount(); j++)
        printnb("[" + m.group(j) + "]");
      print();
    }
  }
} /* Output:
[the slithy toves][the][slithy toves][slithy][toves]
[in the wabe.][in][the wabe.][the][wabe.]
[were the borogoves,][were][the borogoves,][the][borogoves,]
[mome raths outgrabe.][mome][raths outgrabe.][raths][outgrabe.]
[Jabberwock, my son,][Jabberwock,][my son,][my][son,]
[claws that catch.][claws][that catch.][that][catch.]
[bird, and shun][bird,][and shun][and][shun]
[The frumious Bandersnatch.][The][frumious Bandersnatch.][frumious][Bandersnatch.]
*///:~
```





**start()与end()**

start()返回先前匹配的起始位置的索引，而end()返回所匹配的最后字符的索引加一的值。匹配操作失败调用start()或end()会产生IllegalStateException。

**示例**：

```
//: strings/StartEnd.java
import java.util.regex.*;
import static net.mindview.util.Print.*;

public class StartEnd {
  public static String input =
    "As long as there is injustice, whenever a\n" +
    "Targathian baby cries out, wherever a distress\n" +
    "signal sounds among the stars ... We'll be there.\n" +
    "This fine ship, and this fine crew ...\n" +
    "Never give up! Never surrender!";
  private static class Display {
    private boolean regexPrinted = false;
    private String regex;
    Display(String regex) { this.regex = regex; }
    void display(String message) {
      if(!regexPrinted) {
        print(regex);
        regexPrinted = true;
      }
      print(message);
    }
  }
  static void examine(String s, String regex) {
    Display d = new Display(regex);
    Pattern p = Pattern.compile(regex);
    Matcher m = p.matcher(s);
    while(m.find())
      d.display("find() '" + m.group() +
        "' start = "+ m.start() + " end = " + m.end());
    if(m.lookingAt()) // No reset() necessary
      d.display("lookingAt() start = "
        + m.start() + " end = " + m.end());
    if(m.matches()) // No reset() necessary
      d.display("matches() start = "
        + m.start() + " end = " + m.end());
  }
  public static void main(String[] args) {
    for(String in : input.split("\n")) {
      print("input : " + in);
      for(String regex : new String[]{"\\w*ere\\w*",
        "\\w*ever", "T\\w+", "Never.*?!"})
        examine(in, regex);
    }
  }
} /* Output:
input : As long as there is injustice, whenever a
\w*ere\w*
find() 'there' start = 11 end = 16
\w*ever
find() 'whenever' start = 31 end = 39
input : Targathian baby cries out, wherever a distress
\w*ere\w*
find() 'wherever' start = 27 end = 35
\w*ever
find() 'wherever' start = 27 end = 35
T\w+
find() 'Targathian' start = 0 end = 10
lookingAt() start = 0 end = 10
input : signal sounds among the stars ... We'll be there.
\w*ere\w*
find() 'there' start = 43 end = 48
input : This fine ship, and this fine crew ...
T\w+
find() 'This' start = 0 end = 4
lookingAt() start = 0 end = 4
input : Never give up! Never surrender!
\w*ever
find() 'Never' start = 0 end = 5
find() 'Never' start = 15 end = 20
lookingAt() start = 0 end = 5
Never.*?!
find() 'Never give up!' start = 0 end = 14
find() 'Never surrender!' start = 15 end = 31
lookingAt() start = 0 end = 14
matches() start = 0 end = 31
*///:~
```



**Pattern标记**

。。。。



### 13.6.5 split()

**split()**方法将输入字符串断开成字符串对象数组，断开边界由下列正则表达式确定：

```
String[] split(CharSequence input)
String[] split(CharSequence input, int limit)
```

示例：

```
//: strings/SplitDemo.java
import java.util.regex.*;
import java.util.*;
import static net.mindview.util.Print.*;

public class SplitDemo {
  public static void main(String[] args) {
    String input =
      "This!!unusual use!!of exclamation!!points";
    print(Arrays.toString(
      Pattern.compile("!!").split(input)));
    // Only do the first three:
    print(Arrays.toString(
      Pattern.compile("!!").split(input, 3)));
  }
} /* Output:
[This, unusual use, of exclamation, points]
[This, unusual use, of exclamation!!points]
*///:~
```

### 13.6.6 替换操作

- **replaceFirst(String replacement)**：以参数字符串replacement替换掉第一个匹配成功的部分。
- **replaceAll(String replacement)**：以参数字符串replacement替换所有匹配成功的部分
- **appendReplacement(StringBuffer sbuf, String replacement)**:执行渐进式替换，不想前述两个只替换第一个匹配或全部匹配。它允许你调用其他方法来生成或处理replacement，使你能够以编程的方式将目标分隔成组，从而具备更加强大的替换功能。
- appendTail(StringBuffer sbuf)：在执行了一次或多次appendReplacement()之后，调用此方法可以将输入字符串余下的部分复制到sbuf中。

**示例**：

```
//: strings/TheReplacements.java
import java.util.regex.*;
import net.mindview.util.*;
import static net.mindview.util.Print.*;

/*! Here's a block of text to use as input to
    the regular expression matcher. Note that we'll
    first extract the block of text by looking for
    the special delimiters, then process the
    extracted block. !*/

public class TheReplacements {
  public static void main(String[] args) throws Exception {
    String s = TextFile.read("TheReplacements.java");
    // Match the specially commented block of text above:
    Matcher mInput =
      Pattern.compile("/\\*!(.*)!\\*/", Pattern.DOTALL)
        .matcher(s);
    if(mInput.find())
      s = mInput.group(1); // Captured by parentheses
    // Replace two or more spaces with a single space:
    s = s.replaceAll(" {2,}", " ");
    // Replace one or more spaces at the beginning of each
    // line with no spaces. Must enable MULTILINE mode:
    s = s.replaceAll("(?m)^ +", "");
    print(s);
    s = s.replaceFirst("[aeiou]", "(VOWEL1)");
    StringBuffer sbuf = new StringBuffer();
    Pattern p = Pattern.compile("[aeiou]");
    Matcher m = p.matcher(s);
    // Process the find information as you
    // perform the replacements:
    while(m.find())
      m.appendReplacement(sbuf, m.group().toUpperCase());
    // Put in the remainder of the text:
    m.appendTail(sbuf);
    print(sbuf);
  }
} /* Output:
Here's a block of text to use as input to
the regular expression matcher. Note that we'll
first extract the block of text by looking for
the special delimiters, then process the
extracted block.
H(VOWEL1)rE's A blOck Of tExt tO UsE As InpUt tO
thE rEgUlAr ExprEssIOn mAtchEr. NOtE thAt wE'll
fIrst ExtrAct thE blOck Of tExt by lOOkIng fOr
thE spEcIAl dElImItErs, thEn prOcEss thE
ExtrActEd blOck.
*///:~
```

### 13.6.7 reset()

通过**reset()**方法，可以将现有的Matcher对象应用于一个新的字符序列。

**示例**：

```
//: strings/Resetting.java
import java.util.regex.*;

public class Resetting {
  public static void main(String[] args) throws Exception {
    Matcher m = Pattern.compile("[frb][aiu][gx]")
      .matcher("fix the rug with bags");
    while(m.find())
      System.out.print(m.group() + " ");
    System.out.println();
    m.reset("fix the rig with rags");
    while(m.find())
      System.out.print(m.group() + " ");
  }
} /* Output:
fix rug bag
fix rig rag
*///:~
```

使用**不带参数的reset()方法**，可以将Matcher对象重新设置到当前字符序列的起始位置。

### 13.6.8 正则表达式与Java I\O

**正则表达式在一个文件中进行搜索匹配操作**。JGre.java的灵感源自Unix上的grep。它有两个参数：文件名以及要匹配的正则表达式。输出的是有匹配的部分以及匹配部分在行中的位置。

**示例**：

```
//: strings/JGrep.java
// A very simple version of the "grep" program.
// {Args: JGrep.java "\\b[Ssct]\\w+"}
import java.util.regex.*;
import net.mindview.util.*;

public class JGrep {
  public static void main(String[] args) throws Exception {
    if(args.length < 2) {
      System.out.println("Usage: java JGrep file regex");
      System.exit(0);
    }
    Pattern p = Pattern.compile(args[1]);
    // Iterate through the lines of the input file:
    int index = 0;
    Matcher m = p.matcher("");
    for(String line : new TextFile(args[0])) {
      m.reset(line);
      while(m.find())
        System.out.println(index++ + ": " +
          m.group() + ": " + m.start());
    }
  }
} /* Output: (Sample)
0: strings: 4
1: simple: 10
2: the: 28
3: Ssct: 26
4: class: 7
5: static: 9
6: String: 26
7: throws: 41
8: System: 6
9: System: 6
10: compile: 24
11: through: 15
12: the: 23
13: the: 36
14: String: 8
15: System: 8
16: start: 31
*///:~
```

## 13.7 扫描输入

从文件或者标准输入读取数据一般的解决之道就是：读入一行文本，对其进行分词，然后使用Integer、Double等类的各种解析方法来解析数据：

```
//: strings/SimpleRead.java
import java.io.*;

public class SimpleRead {
  public static BufferedReader input = new BufferedReader(
    new StringReader("Sir Robin of Camelot\n22 1.61803"));
  public static void main(String[] args) {
    try {
      System.out.println("What is your name?");
      String name = input.readLine();
      System.out.println(name);
      System.out.println(
        "How old are you? What is your favorite double?");
      System.out.println("(input: <age> <double>)");
      String numbers = input.readLine();
      System.out.println(numbers);
      String[] numArray = numbers.split(" ");
      int age = Integer.parseInt(numArray[0]);
      double favorite = Double.parseDouble(numArray[1]);
      System.out.format("Hi %s.\n", name);
      System.out.format("In 5 years you will be %d.\n",
        age + 5);
      System.out.format("My favorite double is %f.",
        favorite / 2);
    } catch(IOException e) {
      System.err.println("I/O exception");
    }
  }
} /* Output:
What is your name?
Sir Robin of Camelot
How old are you? What is your favorite double?
(input: <age> <double>)
22 1.61803
Hi Sir Robin of Camelot.
In 5 years you will be 27.
My favorite double is 0.809015.
*///:~
```

**StringReader**将String转为化可读的流对象，然后用这个对象来构造BufferReader对象。

**readLine()**方法将一行输入转为String对象。如果两个输入值在同一行，必须分解这个行。 Java SE5新增了Scanner类。

**示例**：

```
//: strings/BetterRead.java
import java.util.*;

public class BetterRead {
  public static void main(String[] args) {
    Scanner stdin = new Scanner(SimpleRead.input);
    System.out.println("What is your name?");
    String name = stdin.nextLine();
    System.out.println(name);
    System.out.println(
      "How old are you? What is your favorite double?");
    System.out.println("(input: <age> <double>)");
    int age = stdin.nextInt();
    double favorite = stdin.nextDouble();
    System.out.println(age);
    System.out.println(favorite);
    System.out.format("Hi %s.\n", name);
    System.out.format("In 5 years you will be %d.\n",
      age + 5);
    System.out.format("My favorite double is %f.",
      favorite / 2);
  }
} /* Output:
What is your name?
Sir Robin of Camelot
How old are you? What is your favorite double?
(input: <age> <double>)
22
1.61803
Hi Sir Robin of Camelot.
In 5 years you will be 27.
My favorite double is 0.809015.
*///:~
```

Scanner的构造器可以接受任何类型的输入对象，包括File对象、InputStream、String或者像此例中的Readable对象。有了Scanner，所有的输入、分词以及翻译的操作都隐藏在不同类型的next方法中。

### 13.7.1 Scanner定界符

默认情况，Scanner根据空白符对输入进行分词。可以用正则表达式指定自己所需的定界符：

```
//: strings/ScannerDelimiter.java
import java.util.*;

public class ScannerDelimiter {
  public static void main(String[] args) {
    Scanner scanner = new Scanner("12, 42, 78, 99, 42");
    scanner.useDelimiter("\\s*,\\s*");
    while(scanner.hasNextInt())
      System.out.println(scanner.nextInt());
  }
} /* Output:
12
42
78
99
42
*///:~
```

用`useDelimiter()`来设置定界符。还有一个delimiter()方法，用来返回当前正在作为定界符使用的Pattern对象。

### 13.7.2 用正则表达式扫描

除了能够扫描基本类型之外，还可以使用自定义的正则表达式进行扫描，在扫描复杂数据的时候非常有用。

示例：

```
//: strings/ThreatAnalyzer.java
import java.util.regex.*;
import java.util.*;

public class ThreatAnalyzer {
  static String threatData =
    "58.27.82.161@02/10/2005\n" +
    "204.45.234.40@02/11/2005\n" +
    "58.27.82.161@02/11/2005\n" +
    "58.27.82.161@02/12/2005\n" +
    "58.27.82.161@02/12/2005\n" +
    "[Next log section with different data format]";
  public static void main(String[] args) {
    Scanner scanner = new Scanner(threatData);
    String pattern = "(\\d+[.]\\d+[.]\\d+[.]\\d+)@" +
      "(\\d{2}/\\d{2}/\\d{4})";
    while(scanner.hasNext(pattern)) {
      scanner.next(pattern);
      MatchResult match = scanner.match();
      String ip = match.group(1);
      String date = match.group(2);
      System.out.format("Threat on %s from %s\n", date,ip);
    }
  }
} /* Output:
Threat on 02/10/2005 from 58.27.82.161
Threat on 02/11/2005 from 204.45.234.40
Threat on 02/11/2005 from 58.27.82.161
Threat on 02/12/2005 from 58.27.82.161
Threat on 02/12/2005 from 58.27.82.161
*///:~
```

当next()方法配合指定的正则表达式使用时，将找到下一个匹配该模式的输入部分，调用match()方法就可以获得匹配的结果。

## 13.8 StringTokenizer

在引入正则表达式和Scanner之前，分割字符串的唯一方法是使用StringTokenizer来分词。

示例：

```
//: strings/ReplacingStringTokenizer.java
import java.util.*;

public class ReplacingStringTokenizer {
  public static void main(String[] args) {
    String input = "But I'm not dead yet! I feel happy!";
    StringTokenizer stoke = new StringTokenizer(input);
    while(stoke.hasMoreElements())
      System.out.print(stoke.nextToken() + " ");
    System.out.println();
    System.out.println(Arrays.toString(input.split(" ")));
    Scanner scanner = new Scanner(input);
    while(scanner.hasNext())
      System.out.print(scanner.next() + " ");
  }
} /* Output:
But I'm not dead yet! I feel happy!
[But, I'm, not, dead, yet!, I, feel, happy!]
But I'm not dead yet! I feel happy!
*///:~
```

# 第14章 类型信息

类型信息主要讨论java是如何让我们在运行时识别对象和类的信息的。

## 14.1 为什么需要RTTI

**RTTI**：在运行时识别一个对象的类型

## 14.2 Class对象

类是程序的一部分，每个类都有一个Class对象，**每当编写并编译了一个新类，就会产生一个Class对象**。为了生成这个类的对象，运行这个程序的Java虚拟机使用**“类加载器**”这个子系统。

所有的类都是在第一次使用时，动态加载到JVM中的。当程序创建第一个对类的静态成员的引用，就会加载这个类。证明了构造器也是类的静态方法。

类加载器首先检查这个类的Class对象是否已经加载。如果尚未加载，默认的类加载器就会根据类名查找.class文件。一旦某各类的Class对象被加载到内存，它就被用来创建这个类的所有对象。

- `Class.forName("Gum")`

  这个方法是Class类的一个static成员，Class对象就和其他对象一样，可以获取并操作它的引用。`forName()`是取得Class对象引用的一种方法。

```
//: typeinfo/toys/ToyTest.java
// Testing class Class.
package typeinfo.toys;
import static net.mindview.util.Print.*;

interface HasBatteries {}
interface Waterproof {}
interface Shoots {}

class Toy {
  // Comment out the following default constructor
  // to see NoSuchMethodError from (*1*)
  Toy() {}
  Toy(int i) {}
}

class FancyToy extends Toy
implements HasBatteries, Waterproof, Shoots {
  FancyToy() { super(1); }
}

public class ToyTest {
  static void printInfo(Class cc) {
    print("Class name: " + cc.getName() +
      " is interface? [" + cc.isInterface() + "]");
    print("Simple name: " + cc.getSimpleName());
    print("Canonical name : " + cc.getCanonicalName());
  }
  public static void main(String[] args) {
    Class c = null;
    try {
      c = Class.forName("typeinfo.toys.FancyToy");
    } catch(ClassNotFoundException e) {
      print("Can't find FancyToy");
      System.exit(1);
    }
    printInfo(c);	
    for(Class face : c.getInterfaces())
      printInfo(face);
    Class up = c.getSuperclass();
    Object obj = null;
    try {
      // Requires default constructor:
      obj = up.newInstance();
    } catch(InstantiationException e) {
      print("Cannot instantiate");
      System.exit(1);
    } catch(IllegalAccessException e) {
      print("Cannot access");
      System.exit(1);
    }
    printInfo(obj.getClass());
  }
} /* Output:
Class name: typeinfo.toys.FancyToy is interface? [false]
Simple name: FancyToy
Canonical name : typeinfo.toys.FancyToy
Class name: typeinfo.toys.HasBatteries is interface? [true]
Simple name: HasBatteries
Canonical name : typeinfo.toys.HasBatteries
Class name: typeinfo.toys.Waterproof is interface? [true]
Simple name: Waterproof
Canonical name : typeinfo.toys.Waterproof
Class name: typeinfo.toys.Shoots is interface? [true]
Simple name: Shoots
Canonical name : typeinfo.toys.Shoots
Class name: typeinfo.toys.Toy is interface? [false]
Simple name: Toy
Canonical name : typeinfo.toys.Toy
*///:~
```

### 14.2.1 类字面常量

Java还提供了另一种方法来生成对Class对象的引用，及使用**类字面常量**。示例：`FancyToy.class;`

这样做更简单，安全，在编译时就会受到检查，并且根除对`forName()`的方法调用，更高效。

类字面常量不仅可以应用于普通的类，还可以应用于**接口，数组以及基本数据类型**。对于基本数据类型的包装器类，还有一个标准字段TYPE。

![](G:\tempphoto\1.jpg)

当使用`.class`来创建对Class对象的引用时，不会自动初始化该Class对象，为了使用类实际做的准备工作包含三个步骤：**加载；链接；初始化**。

示例：

```
//: typeinfo/ClassInitialization.java
import java.util.*;

class Initable {
  static final int staticFinal = 47;
  static final int staticFinal2 =
    ClassInitialization.rand.nextInt(1000);
  static {
    System.out.println("Initializing Initable");
  }
}

class Initable2 {
  static int staticNonFinal = 147;
  static {
    System.out.println("Initializing Initable2");
  }
}

class Initable3 {
  static int staticNonFinal = 74;
  static {
    System.out.println("Initializing Initable3");
  }
}

public class ClassInitialization {
  public static Random rand = new Random(47);
  public static void main(String[] args) throws Exception {
    Class initable = Initable.class;
    System.out.println("After creating Initable ref");
    // Does not trigger initialization:
    System.out.println(Initable.staticFinal);
    // Does trigger initialization:
    System.out.println(Initable.staticFinal2);
    // Does trigger initialization:
    System.out.println(Initable2.staticNonFinal);
    Class initable3 = Class.forName("Initable3");
    System.out.println("After creating Initable3 ref");
    System.out.println(Initable3.staticNonFinal);
  }
} /* Output:
After creating Initable ref
47
Initializing Initable
258
Initializing Initable2
147
Initializing Initable3
After creating Initable3 ref
74
*///:~
```

**如果一个static域不是final，在对它访问前，总是要求在它被读取前，要先进行链接和初始化**。如果一个static final值是编译期常量，那么这个值不需要对类进行初始化就可以被读取。

### 14.2.2 泛化的Class引用

示例：

```
//: typeinfo/GenericClassReferences.java

public class GenericClassReferences {
  public static void main(String[] args) {
    Class intClass = int.class;
    Class<Integer> genericIntClass = int.class;
    genericIntClass = Integer.class; // Same thing
    intClass = double.class;
    // genericIntClass = double.class; // Illegal
  }
} ///:~
```

为了在使用泛化的Class引用时放松限制，**使用通配符“`?`”**，表示任何事物。

示例：

```
//: typeinfo/WildcardClassReferences.java

public class WildcardClassReferences {
  public static void main(String[] args) {
    Class<?> intClass = int.class;
    intClass = double.class;
  }
} ///:~
```

创建一个Class引用，**限定为某种型号或者该类型的任何子类型**，可将通配符与**extends**关键字相结合，创建一个范围。

示例：

```
//: typeinfo/BoundedClassReferences.java

public class BoundedClassReferences {
  public static void main(String[] args) {
    Class<? extends Number> bounded = int.class;
    bounded = double.class;
    bounded = Number.class;
    // Or anything else derived from Number.
  }
} ///:~
```

示例：

```
//: typeinfo/toys/GenericToyTest.java
// Testing class Class.
package typeinfo.toys;

public class GenericToyTest {
  public static void main(String[] args) throws Exception {
    Class<FancyToy> ftClass = FancyToy.class;
    // Produces exact type:
    FancyToy fancyToy = ftClass.newInstance();
    Class<? super FancyToy> up = ftClass.getSuperclass();
    // This won't compile:
    // Class<Toy> up2 = ftClass.getSuperclass();
    // Only produces Object:
    Object obj = up.newInstance();
  }
} ///:~
```

如果是超类，编译器会允许你声明超类引用是“**某个类，它是FancyToy的超类**”。因为getSuperClass()方法返回的是基类，由于这种含糊性，`up.getNewInstance()`返回值不是精确类型，而是Object。

### 14.2.3 新的转型语法

Java SE5 添加了用于Class引用转型语法，**即`cast()`方法**：

```
//: typeinfo/ClassCasts.java

class Building {}
class House extends Building {}

public class ClassCasts {
  public static void main(String[] args) {
    Building b = new House();
    Class<House> houseType = House.class;
    House h = houseType.cast(b);
    h = (House)b; // ... or just do this.
  }
} ///:~
```

`cast()`方法接受参数对象，将其转型为Class引用类型。上述代码实现的功能与最后一行传统的转换一样，对于无法使用普通转型的情况显得非常有用。

java SE5 中另一个新特性：`Class.asSubclass()`，该方法允许你将一个类对象转型为更加具体的类型。

## 14.3 类型转换前先做检查

传统的类型转换错误会抛出一个转换异常。RTTI在java中有一种形式，即关键字`instanceof`。返回一个布尔值，告诉我们对象是不是某个特定类型的实例。

`instanceof`使用限制：只可将其与命名类型进行比较，不能与Class对象做比较。

### 14.3.1 使用类字面常量

示例：

```
//: typeinfo/pets/LiteralPetCreator.java
// Using class literals.
package typeinfo.pets;
import java.util.*;

public class LiteralPetCreator extends PetCreator {
  // No try block needed.
  @SuppressWarnings("unchecked")
  public static final List<Class<? extends Pet>> allTypes =
    Collections.unmodifiableList(Arrays.asList(
      Pet.class, Dog.class, Cat.class,  Rodent.class,
      Mutt.class, Pug.class, EgyptianMau.class, Manx.class,
      Cymric.class, Rat.class, Mouse.class,Hamster.class));
  // Types for random creation:
  private static final List<Class<? extends Pet>> types =
    allTypes.subList(allTypes.indexOf(Mutt.class),
      allTypes.size());
  public List<Class<? extends Pet>> types() {
    return types;
  }	
  public static void main(String[] args) {
    System.out.println(types);
  }
} /* Output:
[class typeinfo.pets.Mutt, class typeinfo.pets.Pug, class typeinfo.pets.EgyptianMau, class typeinfo.pets.Manx, class typeinfo.pets.Cymric, class typeinfo.pets.Rat, class typeinfo.pets.Mouse, class typeinfo.pets.Hamster]
*///:~
```





### 14.3.2 动态的instanceof

示例：

```
//: typeinfo/PetCount3.java
// Using isInstance()
import typeinfo.pets.*;
import java.util.*;
import net.mindview.util.*;
import static net.mindview.util.Print.*;

public class PetCount3 {
  static class PetCounter
  extends LinkedHashMap<Class<? extends Pet>,Integer> {
    public PetCounter() {
      super(MapData.map(LiteralPetCreator.allTypes, 0));
    }
    public void count(Pet pet) {
      // Class.isInstance() eliminates instanceofs:
      for(Map.Entry<Class<? extends Pet>,Integer> pair
          : entrySet())
        if(pair.getKey().isInstance(pet))
          put(pair.getKey(), pair.getValue() + 1);
    }	
    public String toString() {
      StringBuilder result = new StringBuilder("{");
      for(Map.Entry<Class<? extends Pet>,Integer> pair
          : entrySet()) {
        result.append(pair.getKey().getSimpleName());
        result.append("=");
        result.append(pair.getValue());
        result.append(", ");
      }
      result.delete(result.length()-2, result.length());
      result.append("}");
      return result.toString();
    }
  }	
  public static void main(String[] args) {
    PetCounter petCount = new PetCounter();
    for(Pet pet : Pets.createArray(20)) {
      printnb(pet.getClass().getSimpleName() + " ");
      petCount.count(pet);
    }
    print();
    print(petCount);
  }
} /* Output:
Rat Manx Cymric Mutt Pug Cymric Pug Manx Cymric Rat EgyptianMau Hamster EgyptianMau Mutt Mutt Cymric Mouse Pug Mouse Cymric
{Pet=20, Dog=6, Cat=9, Rodent=5, Mutt=3, Pug=3, EgyptianMau=2, Manx=7, Cymric=5, Rat=2, Mouse=2, Hamster=1}
*///:~
```

### 14.3.3 递归计数

示例：

```
//: typeinfo/PetCount4.java
import typeinfo.pets.*;
import net.mindview.util.*;
import static net.mindview.util.Print.*;

public class PetCount4 {
  public static void main(String[] args) {
    TypeCounter counter = new TypeCounter(Pet.class);
    for(Pet pet : Pets.createArray(20)) {
      printnb(pet.getClass().getSimpleName() + " ");
      counter.count(pet);
    }
    print();
    print(counter);
  }
} /* Output: (Sample)
Rat Manx Cymric Mutt Pug Cymric Pug Manx Cymric Rat EgyptianMau Hamster EgyptianMau Mutt Mutt Cymric Mouse Pug Mouse Cymric
{Mouse=2, Dog=6, Manx=7, EgyptianMau=2, Rodent=5, Pug=3, Mutt=3, Cymric=5, Cat=9, Hamster=1, Pet=20, Rat=2}
*///:~
```

## 14.4 注册工厂

**工厂方法设计模式**：将对象的创建工作交给类自己去完成。工厂方法就是Factory接口中的create()方法：

```
//: typeinfo/factory/Factory.java
package typeinfo.factory;
public interface Factory<T> { T create(); } ///:~
```

基类Part包包含一个工厂对象的列表，对于应这个又createRandom()方法产生的类型，他们的工厂都被添加到了partFactories List中，从而被注册到了基类中。

示例：

```
//: typeinfo/RegisteredFactories.java
// Registering Class Factories in the base class.
import typeinfo.factory.*;
import java.util.*;

class Part {
  public String toString() {
    return getClass().getSimpleName();
  }
  static List<Factory<? extends Part>> partFactories =
    new ArrayList<Factory<? extends Part>>();	
  static {
    // Collections.addAll() gives an "unchecked generic
    // array creation ... for varargs parameter" warning.
    partFactories.add(new FuelFilter.Factory());
    partFactories.add(new AirFilter.Factory());
    partFactories.add(new CabinAirFilter.Factory());
    partFactories.add(new OilFilter.Factory());
    partFactories.add(new FanBelt.Factory());
    partFactories.add(new PowerSteeringBelt.Factory());
    partFactories.add(new GeneratorBelt.Factory());
  }
  private static Random rand = new Random(47);
  public static Part createRandom() {
    int n = rand.nextInt(partFactories.size());
    return partFactories.get(n).create();
  }
}	

class Filter extends Part {}

class FuelFilter extends Filter {
  // Create a Class Factory for each specific type:
  public static class Factory
  implements typeinfo.factory.Factory<FuelFilter> {
    public FuelFilter create() { return new FuelFilter(); }
  }
}

class AirFilter extends Filter {
  public static class Factory
  implements typeinfo.factory.Factory<AirFilter> {
    public AirFilter create() { return new AirFilter(); }
  }
}	

class CabinAirFilter extends Filter {
  public static class Factory
  implements typeinfo.factory.Factory<CabinAirFilter> {
    public CabinAirFilter create() {
      return new CabinAirFilter();
    }
  }
}

class OilFilter extends Filter {
  public static class Factory
  implements typeinfo.factory.Factory<OilFilter> {
    public OilFilter create() { return new OilFilter(); }
  }
}	

class Belt extends Part {}

class FanBelt extends Belt {
  public static class Factory
  implements typeinfo.factory.Factory<FanBelt> {
    public FanBelt create() { return new FanBelt(); }
  }
}

class GeneratorBelt extends Belt {
  public static class Factory
  implements typeinfo.factory.Factory<GeneratorBelt> {
    public GeneratorBelt create() {
      return new GeneratorBelt();
    }
  }
}	

class PowerSteeringBelt extends Belt {
  public static class Factory
  implements typeinfo.factory.Factory<PowerSteeringBelt> {
    public PowerSteeringBelt create() {
      return new PowerSteeringBelt();
    }
  }
}	

public class RegisteredFactories {
  public static void main(String[] args) {
    for(int i = 0; i < 10; i++)
      System.out.println(Part.createRandom());
  }
} /* Output:
GeneratorBelt
CabinAirFilter
GeneratorBelt
AirFilter
PowerSteeringBelt
CabinAirFilter
FuelFilter
PowerSteeringBelt
PowerSteeringBelt
FuelFilter
*///:~
```

## 14.5 instanceof与Class的等价性

查询类型信息时，以instanceof的形式（即以instanceof的形式红isInstance()的形式）与直接比较Class对象有一个很重要的差别。

示例：

```
//: typeinfo/FamilyVsExactType.java
// The difference between instanceof and class
package typeinfo;
import static net.mindview.util.Print.*;

class Base {}
class Derived extends Base {}	

public class FamilyVsExactType {
  static void test(Object x) {
    print("Testing x of type " + x.getClass());
    print("x instanceof Base " + (x instanceof Base));
    print("x instanceof Derived "+ (x instanceof Derived));
    print("Base.isInstance(x) "+ Base.class.isInstance(x));
    print("Derived.isInstance(x) " +
      Derived.class.isInstance(x));
    print("x.getClass() == Base.class " +
      (x.getClass() == Base.class));
    print("x.getClass() == Derived.class " +
      (x.getClass() == Derived.class));
    print("x.getClass().equals(Base.class)) "+
      (x.getClass().equals(Base.class)));
    print("x.getClass().equals(Derived.class)) " +
      (x.getClass().equals(Derived.class)));
  }
  public static void main(String[] args) {
    test(new Base());
    test(new Derived());
  }	
} /* Output:
Testing x of type class typeinfo.Base
x instanceof Base true
x instanceof Derived false
Base.isInstance(x) true
Derived.isInstance(x) false
x.getClass() == Base.class true
x.getClass() == Derived.class false
x.getClass().equals(Base.class)) true
x.getClass().equals(Derived.class)) false
Testing x of type class typeinfo.Derived
x instanceof Base true
x instanceof Derived true
Base.isInstance(x) true
Derived.isInstance(x) true
x.getClass() == Base.class false
x.getClass() == Derived.class true
x.getClass().equals(Base.class)) false
x.getClass().equals(Derived.class)) true
*///:~
```

**instanceof和isInstance()**生成的结果完全一样，**equals()和==**也一样，但是二者结论不同：**前者保持了类型的概念**：“你是这个类吗？或者你是这个类的派生类吗？”而后者就没有考虑继承关系：“它或者是这个类型，或者不是”；

## 14.6 反射：运行时的类信息

如果不知道某个对象的确切类型，RTTI可以告诉你，但是有一个限制：这个类型在编译时必须已知，这样才能使用RTTI识别它，并利用这些信息做一些有用的事情。

**Class类与java.lang.reflect类库**一起对反射的概念进行了支持，该类库包含了**Filed、method、constructor类**（每个类都实现了member接口）。这些类型的对象是由jvm在运行时创建的，用以表示未知类里对应的成员变量。

可以用Constructor创建新对象，用**get()**和**set()**方法读取和修改与Field对象关联的字段，用**invoke()**方法调用与method对象关联的方法；另外，可以调用**getField()**、**getMethods()**和**getConstructor()**等便利的方法。

对于反射机制来说，.class文件在编译是是不可获取的，所以是在运行时打开和检查.class文件。

### 14.6.1 类方法提取器

示例：

```
//: typeinfo/ShowMethods.java
// Using reflection to show all the methods of a class,
// even if the methods are defined in the base class.
// {Args: ShowMethods}
import java.lang.reflect.*;
import java.util.regex.*;
import static net.mindview.util.Print.*;

public class ShowMethods {
  private static String usage =
    "usage:\n" +
    "ShowMethods qualified.class.name\n" +
    "To show all methods in class or:\n" +
    "ShowMethods qualified.class.name word\n" +
    "To search for methods involving 'word'";
  private static Pattern p = Pattern.compile("\\w+\\.");
  public static void main(String[] args) {
    if(args.length < 1) {
      print(usage);
      System.exit(0);
    }
    int lines = 0;
    try {
      Class<?> c = Class.forName(args[0]);
      Method[] methods = c.getMethods();
      Constructor[] ctors = c.getConstructors();
      if(args.length == 1) {
        for(Method method : methods)
          print(
            p.matcher(method.toString()).replaceAll(""));
        for(Constructor ctor : ctors)
          print(p.matcher(ctor.toString()).replaceAll(""));
        lines = methods.length + ctors.length;
      } else {
        for(Method method : methods)
          if(method.toString().indexOf(args[1]) != -1) {
            print(
              p.matcher(method.toString()).replaceAll(""));
            lines++;
          }
        for(Constructor ctor : ctors)
          if(ctor.toString().indexOf(args[1]) != -1) {
            print(p.matcher(
              ctor.toString()).replaceAll(""));
            lines++;
          }
      }
    } catch(ClassNotFoundException e) {
      print("No such class: " + e);
    }
  }
} /* Output:
public static void main(String[])
public native int hashCode()
public final native Class getClass()
public final void wait(long,int) throws InterruptedException
public final void wait() throws InterruptedException
public final native void wait(long) throws InterruptedException
public boolean equals(Object)
public String toString()
public final native void notify()
public final native void notifyAll()
public ShowMethods()
*///:~
```

## 14.7 动态代理

java的动态代理比代理的思想更加向前迈进一步，可以动态地创建代理并动态地处理对所代理方法的调用。**在动态代理上所做的所有调用都会被重定向到单一的调用处理器上，它的工作是揭示调用的类型并确定相应的对策**。

示例：

```
//: typeinfo/SimpleDynamicProxy.java
import java.lang.reflect.*;

class DynamicProxyHandler implements InvocationHandler {
  private Object proxied;
  public DynamicProxyHandler(Object proxied) {
    this.proxied = proxied;
  }
  public Object
  invoke(Object proxy, Method method, Object[] args)
  throws Throwable {
    System.out.println("**** proxy: " + proxy.getClass() +
      ", method: " + method + ", args: " + args);
    if(args != null)
      for(Object arg : args)
        System.out.println("  " + arg);
    return method.invoke(proxied, args);
  }
}	

class SimpleDynamicProxy {
  public static void consumer(Interface iface) {
    iface.doSomething();
    iface.somethingElse("bonobo");
  }
  public static void main(String[] args) {
    RealObject real = new RealObject();
    consumer(real);
    // Insert a proxy and call again:
    Interface proxy = (Interface)Proxy.newProxyInstance(
      Interface.class.getClassLoader(),
      new Class[]{ Interface.class },
      new DynamicProxyHandler(real));
    consumer(proxy);
  }
} /* Output: (95% match)	
doSomething
somethingElse bonobo
**** proxy: class $Proxy0, method: public abstract void Interface.doSomething(), args: null
doSomething
**** proxy: class $Proxy0, method: public abstract void Interface.somethingElse(java.lang.String), args: [Ljava.lang.Object;@42e816
  bonobo
somethingElse bonobo
*///:~
```

通过调用静态方法`Proxy.newProxyInstance()`可以创建动态代理，这个方法需要得到一个类加载器，一个希望该代理实现的接口列表以及InvocationHandler接口的一个实现。

invoke()方法中传递进来了代理对象，以防你需要区分请求的来源。

示例：

```
//: typeinfo/SelectingMethods.java
// Looking for particular methods in a dynamic proxy.
import java.lang.reflect.*;
import static net.mindview.util.Print.*;

class MethodSelector implements InvocationHandler {
  private Object proxied;
  public MethodSelector(Object proxied) {
    this.proxied = proxied;
  }
  public Object
  invoke(Object proxy, Method method, Object[] args)
  throws Throwable {
    if(method.getName().equals("interesting"))
      print("Proxy detected the interesting method");
    return method.invoke(proxied, args);
  }
}	

interface SomeMethods {
  void boring1();
  void boring2();
  void interesting(String arg);
  void boring3();
}

class Implementation implements SomeMethods {
  public void boring1() { print("boring1"); }
  public void boring2() { print("boring2"); }
  public void interesting(String arg) {
    print("interesting " + arg);
  }
  public void boring3() { print("boring3"); }
}	

class SelectingMethods {
  public static void main(String[] args) {
    SomeMethods proxy= (SomeMethods)Proxy.newProxyInstance(
      SomeMethods.class.getClassLoader(),
      new Class[]{ SomeMethods.class },
      new MethodSelector(new Implementation()));
    proxy.boring1();
    proxy.boring2();
    proxy.interesting("bonobo");
    proxy.boring3();
  }
} /* Output:
boring1
boring2
Proxy detected the interesting method
interesting bonobo
boring3
*///:~
```

项目：使用动态代理来编写一个系统以实现事务，。。。。

## 14.8 空对象

引入**空对象**的思想，可以接受传递给它的所代表的对象的消息，但是将返回表示为实际上并不存在任何“真实”对象的值。**通过这种方式可以假设所有的对象都是有效的**。

```
//: typeinfo/Person.java
// A class with a Null Object.
import net.mindview.util.*;

class Person {
  public final String first;
  public final String last;
  public final String address;
  // etc.
  public Person(String first, String last, String address){
    this.first = first;
    this.last = last;
    this.address = address;
  }	
  public String toString() {
    return "Person: " + first + " " + last + " " + address;
  }
  public static class NullPerson
  extends Person implements Null {
    private NullPerson() { super("None", "None", "None"); }
    public String toString() { return "NullPerson"; }
  }
  public static final Person NULL = new NullPerson();
} ///:~
```

- 示例
  ![](G:\tempphoto\2.JPG)

  我们将创建一个定义操作（在这里，是客户的名称）的` AbstractCustomer `抽象类，和扩展了` AbstractCustomer`类的实体类。工厂类` CustomerFactory` 基于客户传递的名字来返回 `RealCustomer `或` NullCustomer`对象。

  `NullPatternDemo`，我们的演示类使用 `CustomerFactory` 来演示空对象模式的用法。

  - 首先创建一个抽象类

    ```
    public abstract class AbstractCustomer {
       protected String name;
       public abstract boolean isNil();
       public abstract String getName();
    }
    ```

  - 接着创建扩展上述类的实体类：

    ```
    public class RealCustomer extends AbstractCustomer {
     
       public RealCustomer(String name) {
          this.name = name;    
       }
       
       @Override
       public String getName() {
          return name;
       }
       
       @Override
       public boolean isNil() {
          return false;
       }
    }
    ```

    ```
    public class NullCustomer extends AbstractCustomer {
     
       @Override
       public String getName() {
          return "Not Available in Customer Database";
       }
     
       @Override
       public boolean isNil() {
          return true;
       }
    }
    ```

  - 步骤三：创建工厂类

    ```
    public class CustomerFactory {
       
       public static final String[] names = {"Rob", "Joe", "Julie"};
     
       public static AbstractCustomer getCustomer(String name){
          for (int i = 0; i < names.length; i++) {
             if (names[i].equalsIgnoreCase(name)){
                return new RealCustomer(name);
             }
          }
          return new NullCustomer();
       }
    }
    ```

  - 步骤四：使用工厂类，给予客户传递的名字获取对象

    ```
    public class NullPatternDemo {
       public static void main(String[] args) {
     
          AbstractCustomer customer1 = CustomerFactory.getCustomer("Rob");
          AbstractCustomer customer2 = CustomerFactory.getCustomer("Bob");
          AbstractCustomer customer3 = CustomerFactory.getCustomer("Julie");
          AbstractCustomer customer4 = CustomerFactory.getCustomer("Laura");
     
          System.out.println("Customers");
          System.out.println(customer1.getName());
          System.out.println(customer2.getName());
          System.out.println(customer3.getName());
          System.out.println(customer4.getName());
       }
    }
    ```


## 14.9 接口与类型信息









# 第15章 泛型

