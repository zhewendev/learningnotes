# 基本数据与常量池

1. **int的自动拆箱和装箱只在-128到127范围中进行**，超过该范围的两个integer的 == 判断是会返回false的。byte都是相等的，因为范围就在-128到127之间

2. 发现char也是在0到127之间自动拆箱

3. 包装类的比较时先比较是否是同一个类，不是的话直接返回false.

4. 基本数据类型的包装类型可以在常量池查找对应值的对象，找不到就会自动在常量池创建该值的对象。

   而String类型可以通过intern来完成这个操作。

   ```java
   上面自动拆箱和装箱的原理其实与常量池有关。
   3.1存在栈中：
   public void(int a)
   {
   int i = 1;
   int j = 1;
   }
   方法中的i 存在虚拟机栈的局部变量表里，i是一个引用，j也是一个引用，它们都指向局部变量表里的整型值 1.
   int a是传值引用，所以a也会存在局部变量表。
   
   3.2存在堆里：
   class A{
   int i = 1;
   A a = new A();
   }
   i是类的成员变量。类实例化的对象存在堆中，所以成员变量也存在堆中，引用a存的是对象的地址，引用i存的是值，这个值1也会存在堆中。可以理解为引用i指向了这个值1。也可以理解为i就是1.
   
   3.3包装类对象怎么存
   其实我们说的常量池也可以叫对象池。
   比如String a= new String("a").intern()时会先在常量池找是否有“a"对象如果有的话直接返回“a"对象在常量池的地址，即让引用a指向常量”a"对象的内存地址。
   public native String intern();
   Integer也是同理。
   ```

   ```java
   public static Integer valueOf(int i) {
       if (i >= IntegerCache.low && i <= IntegerCache.high)
           return IntegerCache.cache[i + (-IntegerCache.low)];
       return new Integer(i);
   }
   private static class IntegerCache {
       static final int low = -128;
       static final int high;
       static final Integer cache[];
   
       static {
           // high value may be configured by property
           int h = 127;
           String integerCacheHighPropValue =
               sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
           if (integerCacheHighPropValue != null) {
               try {
                   int i = parseInt(integerCacheHighPropValue);
                   i = Math.max(i, 127);
                   // Maximum array size is Integer.MAX_VALUE
                   h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
               } catch( NumberFormatException nfe) {
                   // If the property cannot be parsed into an int, ignore it.
               }
           }
           high = h;
   
           cache = new Integer[(high - low) + 1];
           int j = low;
           for(int k = 0; k < cache.length; k++)
               cache[k] = new Integer(j++);
   
           // range [-128, 127] must be interned (JLS7 5.1.7)
           assert IntegerCache.high >= 127;
       }
   
       private IntegerCache() {}
   }
   ```

   JDK1.7后，常量池被放入到堆空间中，这导致intern()函数的功能不同

   ```java
   [java] view plain copy
   String s = new String("1");  
   s.intern();  
   String s2 = "1";  
   System.out.println(s == s2);  
   
   String s3 = new String("1") + new String("1");  
   s3.intern();  
   String s4 = "11";  
   System.out.println(s3 == s4);  
   输出结果为：
   
   [java] view plain copy
   JDK1.6以及以下：false false  
   JDK1.7以及以上：false true
   ```

   JDK 1.7后，intern方法还是会先去查询常量池中是否有已经存在，如果存在，则返回常量池中的引用，这一点与之前没有区别，区别在于，如果在常量池找不到对应的字符串，则不会再将字符串拷贝到常量池，而只是在常量池中生成一个对原字符串的引用。

   那么其他字符串在常量池找值时就会返回另一个堆中对象的地址。

# 构造器

1. **不能被<font color =red>static、final、synchronized、abstract、native</font>修饰，不能有return语句返回值**
2. **父类的构造器不可被子类继承**
3. <font color=red>**类中属性赋值的先后顺序：**</font>1.属性的默认初始化2数据的显示赋值3.通过构造器给属性初始化4.创建对象后，通过对象。方法的形式给属性赋值
4. 初始化引用的一种方式，**惰性初始化**。

# 方法

1. 不能在方法中再定义方法。
2. 方法重载是在一个类中，使用方法名相同但是参数列表不同（参数个数，参数类型）<font color=red>**只看参数列表**</font>，与返回类型也无关
3. 重写方法必须和被重写方法具有**相同的方法名称、参数列表和返回值类型**，同时为static或非static，子类异常不大于父类异常。访问权限不能更严格。
4. 只有非private方法才可以被覆盖；不然则视为子类中的新方法

# this

1. <font color=red>**this**关键字只能在方法内部使用</font>
2. 在方法中调用同类中另一个方法，直接调用就可以，不用添加this，编译器默认添加。
3. 明确指出对当前对象的引用时，需要使用this。例如：`return this`
4. 使用this调用本类中其他的构造器，保证至少有一个构造器是不用this的。且this语句必须是放在构造器首行

# static

1. static 方法就是没有this的方法，不能调用非静态方法。
2. 不能用于局部变量
3. 初始化时先静态对象，再非静态对象。且静态对象只初始化一次
4. 不创建对象或访问静态数据，静态初始化不会进行
5. **static静态块只执行一次，访问静态方法或创建对象等，首次要初始化这个静态块**
6. 重载的方法需要同时是static或非static的

# finalize()

1. **fianlize()需求限制情况**：通过某种创建对象方式以外的方式为对象分配了存储空间
2. finalize()是的对象应该处于某种状态，使它占用的内存可以被安全地释放。如果对象打开了一个文件，回收前要关闭文件。finalize()可以用来发现这种情况
3. 调用垃圾回收或finalize()：`System.gc`,`System.runFinalization()`
4. 垃圾回收的顺序与对象初始化顺序相反

# 类的成员变量

1. 成员变量存在堆空间，有初始值；局部变量在栈空间，要显式赋值
2. **变量定义的先后顺序决定了初始化的顺序。变量定义散布于方法定义之间，它们仍旧会在任何方法（构造器）被调用之前得到初始化**
3. 

# 数组

1. 可变参数列表和自动包装机制可以和谐共处。
2. 可变参数增加了重载的复杂性，可以加入一个非可变参数

# enum

1. 注意枚举类中**values()**方法和**ordinal()**方法。

# 继承

1. 为了继承，一般基类的所有数据成员都指定为private，所有方法指定为public
2. 慎用继承，判断是否需要从新类向基类进行向上转型

# 代理

1. 代理是组合和继承之外的第三种关系，二者的中庸

# 清理对象

1. 在类生命周期内执行一些必须清理动作，需显式编写特殊方法来执行，且确保客户端程序员知晓他们必须要调用这个方法。首要动作将这一清理动作置于finally子句中，预防异常的出现。
2. **<font color =red>执行类的特定清理动作，顺序同生成顺序相反</font>**（基类元素仍旧存活）

# 组合

1. 组合用于想在新类中使用现有类的功能而非它的接口这种情形。一般在**新类中嵌入一个现有类的private对象**。

# final

1. **final数据必须是基本数据类型且在定义时必须对其赋值**，或者在构造器处为其赋值(此时不能有空参数构造器)。
2. 既是static又是final的域，将用大写表示，下划线分隔
3. final参数意味不可在方法中更改引用所指对象。

- > final关键字可以修饰类、方法和引用。
  >
  > **修饰类**，该类不能被继承。并且这个类的对象在堆中分配内存后地址不可变。
  >
  > **修饰方法**，方法不能被子类重写。
  >
  > **修饰引用**，引用无法改变，对于基本类型，无法修改值，对于引用，虽然不能修改地址值，但是可以对指向对象的内部进行修改。


# 绑定

1. **后期绑定**就是在运行时根据对象的类型进行绑定（也称动态绑定或运行时绑定）。
2. Java中**除了static**方法和**final**方法（private属于final方法）之外，其他方法都是后期绑定。

# 多态

1. 只有普通的方法调用是多态的，域和静态方法是不存在

2. 构造器不具有多态性

3. **在构造器中调用一个动态绑定方法，就要用到那个方法的被覆盖的定义。这个效果难以预料，这个方法操纵的成员可能还没有进行初始化**

4. **方法重载的优先级匹配**：

   ```java
   public static void main(String[] args) {
           方法重载优先级匹配 a = new 方法重载优先级匹配();
           //普通的重载一般就是同名方法不同参数。
           //这里我们来讨论当同名方法只有一个参数时的情况。
           //此时会调用char参数的方法。
           //当没有char参数的方法。会调用int类型的方法，如果没有int就调用long
           //即存在一个调用顺序char -> int -> long ->double -> ..。
           //当没有基本类型对应的方法时，先自动装箱，调用包装类方法。
           //如果没有包装类方法，则调用包装类实现的接口的方法。
           //最后再调用持有多个参数的char...方法。
           a.eat('a');
           a.eat('a','c','b');
       }
       public void eat(short i) {
           System.out.println("short");
       }
       public void eat(int i) {
           System.out.println("int");
       }
       public void eat(double i) {
           System.out.println("double");
       }
       public void eat(long i) {
           System.out.println("long");
       }
       public void eat(Character c) {
           System.out.println("Character");
       }
       public void eat(Comparable c) {
           System.out.println("Comparable");
       }
       public void eat(char ... c) {
           System.out.println(Arrays.toString(c));
           System.out.println("...");
       }
   
   //    public void eat(char i) {
   //        System.out.println("char");
   //    }
   ```


# 向下转型

1. java中提供了instanceof 操作符确保向下转型的正确性
2. x instanceof A：检验x是否是类A的对象。如果x属于类A的子类B，也为true，转换后调用子类B重写的方法。

~~~java
public static void main(String[] args) {
        Son son = new Son();
        //首先先明确一点，转型指的是左侧引用的改变。
        //father引用类型是Father，指向Son实例，就是向上转型，既可以使用子类的方法，也可以使用父类的方法。
        //向上转型,此时运行father的方法
        Father father = son;
        father.smoke();
        //不能使用子类独有的方法。
        // father.play();编译会报错
        father.drive();
        //Son类型的引用指向Father的实例，所以是向下转型，不能使用子类非重写的方法，可以使用父类的方法。
        //向下转型，此时运行了son的方法
        Son son1 = (Son) father;
        //转型后就是一个正常的Son实例
        son1.play();
        son1.drive();
        son1.smoke();
        ```
        //因为向下转型之前必须先经历向上转型。

        //        在向下转型过程中，分为两种情况：
        //
        //        情况一：如果父类引用的对象如果引用的是指向的子类对象，
        //        那么在向下转型的过程中是安全的。也就是编译是不会出错误的。
            //因为运行期Son实例确实有这些方法
            Father f1 = new Son();
            Son s1 = (Son) f1;
            s1.smoke();
            s1.drive();
            s1.play();
        //        情况二：如果父类引用的对象是父类本身，那么在向下转型的过程中是不安全的，编译不会出错，
        //        但是运行时会出现java.lang.ClassCastException错误。它可以使用instanceof来避免出错此类错误。
        //因为运行期Father实例并没有这些方法。
            Father f2 = new Father();
            Son s2 = (Son) f2;
            s2.drive();
            s2.smoke();
            s2.play();
        //向下转型和向上转型的应用，有些人觉得这个操作没意义，其实可以用于方法参数中的类型聚合，然后具体操作再进行分解。
        //比如add方法用List引用类型作为参数传入，传入具体类时经历了向下转型
        add(new LinkedList());
        add(new ArrayList());

        //总结
        //向上转型和向下转型都是针对引用的转型，是编译期进行的转型，根据引用类型来判断使用哪个方法
        //并且在传入方法时会自动进行转型（有需要的话）。运行期将引用指向实例，如果是不安全的转型则会报错。
        //若安全则继续执行方法。

    }
    public static void add(List list) {
        System.out.println(list);
        //在操作具体集合时又经历了向上转型
//        ArrayList arr = (ArrayList) list;
//        LinkedList link = (LinkedList) list;
    }
~~~



# 抽象类

1. 不能用abstract修饰属性，私有方法，构造器，静态方法，final的方法，即private，static，final。
2. 如果基类是抽象类，使用多态时无需再向下转型

# 接口

1. 接口支持多重继承，确定方法名，参数列表和返回类型
2. 接口是**抽象方法**和**常量值**的定义集合，**无构造器**。
3. 接口中定义的域不能是“空final”。
4. 接口中的域在第一次加载被初始化，发生在任何域首次被访问时。

# 内部类

1. **它能访问其外围对象的所有成员，而不需要任何特殊条件。此外，内部类还拥有其外围类的所有元素的访问权。**

2. 需要生成对外部类对象的引用，可以使用：**<font color=red>外部类名字.this</font>**

3. 告知某些其他对象，创建其某个内部类的对象，需要使用：<font color=red>**.new**</font>

4. 拥有外部类对象之前不可能创建内部类对象的（除了静态内部类）

5. **内部类可以定义在方法里，作用域内，称为<font color=red>局部内部类</font>**，该内部类仅在方法内或作用域内可用。

6. 匿名内部类：将返回值的生成与表示这个返回值的类定义结合在一起。**这种语法是指**：创建一个继承自基类的匿名类的对象，通过new表达式返回的引用被自动向上转型为基类的引用

7. **定义一个匿名内部类，并且希望它使用一个在其外部定义的对象，那么编译器回要求其参数引用的是final**

8. **不需要内部类与外围类对象之间有联系，那么可以将内部类声明为static，这通常称之为<font color=red>嵌套类</font>**。

9. **普通内部类不能有static数据和static字段**，也不能包含嵌套类，但是嵌套类可以包含这些东西。

10. 嵌套类可以作为接口的一部分，**甚至可以在内部类实现外围接口**。

11. 如果是抽象类或者具体地类，只有使用内部类才能够实现多重继承。

12. 如果一个类只继承了 内部类，不是外围类，但是当要生成一个构造器时，必须在构造器内使用：`enclosingClassReference.super()`，这样才提供了必要的引用

13. **如果一个类A继承了类B，类A的内部类继承了类B的内部类，则类A内部类的构造器可以不用像上述小结中内部类继承那样提供必要的引用。如果类A继承了类B但是内部类没有继承类B中内部类，那么两个内部类是两个独立的实体**。

14. 用局部内部类而非匿名内部类的两个理由

    ```java
    //本节讨论内部类以及不同访问权限的控制
    //内部类只有在使用时才会被加载。
    //外部类B
    public class B{
        int i = 1;
        int j = 1;
        static int s = 1;
        static int ss = 1;
        A a;
        AA aa;
        AAA aaa;
        //内部类A
    
        public class A {
    //        static void go () {
    //
    //        }
    //        static {
    //
    //        }
    //      static int b = 1;//非静态内部类不能有静态成员变量和静态代码块和静态方法，
            // 因为内部类在外部类加载时并不会被加载和初始化。
            //所以不会进行静态代码的调用
            int i = 2;//外部类无法读取内部类的成员，而内部类可以直接访问外部类成员
    
            public void test() {
                System.out.println(j);
                j = 2;
                System.out.println(j);
                System.out.println(s);//可以访问类的静态成员变量
            }
            public void test2() {
                AA aa = new AA();
                AAA aaa = new AAA();
            }
    
        }
        //静态内部类S，可以被外部访问
        public static class S {
            int i = 1;//访问不到非静态变量。
            static int s = 0;//可以有静态变量
    
            public static void main(String[] args) {
                System.out.println(s);
            }
            @Test
            public void test () {
    //            System.out.println(j);//报错，静态内部类不能读取外部类的非静态变量
                System.out.println(s);
                System.out.println(ss);
                s = 2;
                ss = 2;
                System.out.println(s);
                System.out.println(ss);
            }
        }
    
        //内部类AA，其实这里加protected相当于default
        //因为外部类要调用内部类只能通过B。并且无法直接继承AA，所以必须在同包
        //的类中才能调用到(这里不考虑静态内部类)，那么就和default一样了。
        protected class AA{
            int i = 2;//内部类之间不共享变量
            public void test (){
                A a = new A();
                AAA aaa = new AAA();
                //内部类之间可以互相访问。
            }
        }
        //包外部依然无法访问，因为包没有继承关系，所以找不到这个类
        protected static class SS{
            int i = 2;//内部类之间不共享变量
            public void test (){
    
                //内部类之间可以互相访问。
            }
        }
        //私有内部类A，对外不可见，但对内部类和父类可见
        private class AAA {
            int i = 2;//内部类之间不共享变量
    
            public void test() {
                A a = new A();
                AA aa = new AA();
                //内部类之间可以互相访问。
            }
        }
        @Test
        public void test(){
            A a = new A();
            a.test();
            //内部类可以修改外部类的成员变量
            //打印出 1 2
            B b = new B();
    
        }
    }
    
    
    //另一个外部类
    class C {
        @Test
        public void test() {
            //首先，其他类内部类只能通过外部类来获取其实例。
            B.S s = new B.S();
            //静态内部类可以直接通过B类直接获取，不需要B的实例，和静态成员变量类似。
            //B.A a = new B.A();
            //当A不是静态类时这行代码会报错。
            //需要使用B的实例来获取A的实例
            B b = new B();
            B.A a = b.new A();
            B.AA aa = b.new AA();//B和C同包，所以可以访问到AA
    //      B.AAA aaa = b.new AAA();AAA为私有内部类，外部类不可见
            //当A使用private修饰时，使用B的实例也无法获取A的实例，这一点和私有变量是一样的。
            //所有普通的内部类与类中的一个变量是类似的。静态内部类则与静态成员类似。
        }
    }
    ```

- **内部类的加载**：

  > 1  内部类是延时加载的，也就是说只会在第一次使用时加载。不使用就不加载，所以可以很好的实现单例模式。
  >
  > 2 不论是静态内部类还是非静态内部类都是在第一次使用时才会被加载。
  >
  > 3 对于非静态内部类是不能出现静态模块（包含静态块，静态属性，静态方法等）
  >
  > 4 非静态类的使用需要依赖于外部类的对象，详见上述对象innerClass 的初始化。

  **简单来说，类的加载都是发生在类要被用到的时候。内部类也是一样**

  > 1 普通内部类在第一次用到时加载，并且每次实例化时都会执行内部成员变量的初始化，以及代码块和构造方法。
  >
  > 2 静态内部类也是在第一次用到时被加载。但是当它加载完以后就会将静态成员变量初始化，运行静态代码块，并且只执行一次。当然，非静态成员和代码块每次实例化时也会执行。

  总结一下Java类代码加载的顺序，万变不离其宗。

  > **规律一**、初始化构造时，先父后子；只有在父类所有都构造完后子类才被初始化
  >
  > **规律二**、类加载先是静态、后非静态、最后是构造函数。
  >
  > 静态构造块、静态类属性按出现在类定义里面的先后顺序初始化，同理非静态的也是一样的，只是静态的只在加载字节码时执行一次，不管你new多少次，非静态会在new多少次就执行多少次
  >
  > **规律三**、java中的类只有在被用到的时候才会被加载
  >
  > **规律四**、java类只有在类字节码被加载后才可以被构造成对象实例

  局部内部类只能在定义该内部类的方法内实例化，不可以在此方法外对其实例化。

- **匿名内部类**：

  ```java
  new 父类构造器（参数列表）|实现接口（）  
      {  
       //匿名内部类的类体部分  
      }
  ```

  > 1 　**匿名内部类不能有构造方法**。
  >
  > 2 　匿名内部类不能定义任何静态成员、方法和类。
  >
  > 3 　匿名内部类不能是public,protected,private,static。
  >
  > 4 　只能创建匿名内部类的一个实例。
  >
  > 5   一个匿名内部类一定是在new的后面，用其隐含实现一个接口或实现一个类。
  >
  > 6 　因匿名内部类为局部内部类，所以局部内部类的所有限制都对其生效。

  ```java
   public class 匿名内部类 {
  
  }
  interface D{
      void run ();
  }
  abstract class E{
      E (){
  
      }
      abstract void work();
  }
  class A {
  
          @Test
          public void test (int k) {
              //利用接口写出一个实现该接口的类的实例。
              //有且仅有一个实例，这个类无法重用。
              new Runnable() {
                  @Override
                  public void run() {
  //                    k = 1;报错，当外部方法中的局部变量在内部类使用中必须改为final类型。
                      //因为方外部法中即使改变了这个变量也不会反映到内部类中。
                      //所以对于内部类来讲这只是一个常量。
                      System.out.println(100);
                      System.out.println(k);
                  }
              };
              new D(){
                  //实现接口的匿名类
                  int i =1;
                  @Override
                  public void run() {
                      System.out.println("run");
                      System.out.println(i);
                      System.out.println(k);
                  }
              }.run();
              new E(){
                  //继承抽象类的匿名类
                  int i = 1;
                  void run (int j) {
                      j = 1;
                  }
  
                  @Override
                  void work() {
  
                  }
              };
          }
  
  }
  ```

- **匿名内部类里的final**

  > 我们给匿名内部类传递参数的时候，若该形参在内部类中需要被使用，那么该形参必须要为final。也就是说：当所在的方法的形参需要被内部类里面使用时，该形参必须为final。
  >
  > 为什么必须要为final呢？
  >
  > 首先我们知道在内部类编译成功后，它会产生一个class文件，该class文件与外部类并不是同一class文件，仅仅只保留对外部类的引用。当外部类传入的参数需要被内部类调用时，从java程序的角度来看是直接被调用：

  简单理解就是，拷贝引用，为了避免引用值发生改变，例如被外部类的方法修改等，而导致内部类得到的值不一致，于是用final来让该引用不可改变。

- 我们一般都是利用构造器来完成某个实例的初始化工作的，但是匿名内部类是没有构造器的！那怎么来初始化匿名内部类呢？使用构造代码块！利用构造代码块能够达到为匿名内部类创建一个构造器的效果。
- 

# 注解

1. @SuppressWarnings("...")

   表示只有有关不受检查的异常的警告信息应该被抑制。



# 容器

1. 如果容器对象是自动包装类，注意equals()方法。且注意例如remove(2)与remove(Integer.valueOf(2))之间的不同。
2. java的Iterator只能单向移动，能够将遍历序列操作与序列底层结构分离。
3. **ListIterator是一种更强大的Iterator的子类型，它只能用于各种List类的访问**。可以双向移动，还可以产生相对于迭代器在列表中指向的当前位置的前一个和后一个元素的索引，并且可以使用set()方法替换它访问过的最后一个元素。可以调用listIterator()方法产生一个指向List开始处的ListIterator，并且还可以通过调用listIterator(n)方法创建一个一开始就指向列表索引为n的元素处的ListIterator。
4. **LinkedList能够直接实现栈的所有功能的方法，因此可以将LinkedList当做栈使用。**
5. Set不保存重复的元素，测试归属性性一般选择HashSet实现，专门对快速查找进行了优化。
6. Set具有和Collection一样的接口，二者行为不同。



# 字符串

1. String连接

   ```java
   @Test
   public void contact () {
       //1连接方式
       String s1 = "a";
       String s2 = "a";
       String s3 = "a" + s2;
       String s4 = "a" + "a";
       String s5 = s1 + s2;
       //表达式只有常量时，编译期完成计算
       //表达式有变量时，运行期才计算，所以地址不一样
       System.out.println(s3 == s4); //f
       System.out.println(s3 == s5); //f
       System.out.println(s4 == "aa"); //t
   
   }
   
   ```

- String类型的intern

  ```java
  public void intern () {
      //2：string的intern使用
      //s1是基本类型，比较值。s2是string实例，比较实例地址
      //字符串类型用equals方法比较时只会比较值
      String s1 = "a";
      String s2 = new String("a");
      //调用intern时,如果s2中的字符不在常量池，则加入常量池并返回常量的引用
      String s3 = s2.intern();
      System.out.println(s1 == s2);
      System.out.println(s1 == s3);
  }
  ```

- StringBuffer和StringBuilder
  底层是继承父类的可变字符数组value

- ```java
  //Stringbuffer在大部分涉及字符串修改的操作上加了synchronized关键字来保证线程安全，效率较低。
  
  //String类型在使用 + 运算符例如
  
  String a = "a"
  
  a = a + a;//时，实际上先把a封装成stringbuilder，调用append方法后再用tostring返回，所以当大量使用字符串加法时，会大量地生成stringbuilder实例，这是十分浪费的，这种时候应该用stringbuilder来代替string。
  ```

  扩容：

  ```java
  #注意在append方法中调用到了一个函数
  
  ensureCapacityInternal(count + len);
  该方法是计算append之后的空间是否足够，不足的话需要进行扩容
  
  public void ensureCapacity(int minimumCapacity) {
      if (minimumCapacity > 0)
          ensureCapacityInternal(minimumCapacity);
  }
  private void ensureCapacityInternal(int minimumCapacity) {
      // overflow-conscious code
      if (minimumCapacity - value.length > 0) {
          value = Arrays.copyOf(value,
                  newCapacity(minimumCapacity));
      }
  }
  /*如果新字符串长度大于value数组长度则进行扩容
  
  扩容后的长度一般为原来的两倍 + 2；
  
  假如扩容后的长度超过了jvm支持的最大数组长度MAX_ARRAY_SIZE。
  
  考虑两种情况
  
  如果新的字符串长度超过int最大值，则抛出异常，否则直接使用数组最大长度作为新数组的长度。
  */
  private int hugeCapacity(int minCapacity) {
      if (Integer.MAX_VALUE - minCapacity < 0) { // overflow
          throw new OutOfMemoryError();
      }
      return (minCapacity > MAX_ARRAY_SIZE)
          ? minCapacity : MAX_ARRAY_SIZE;
  }
  
  ```

- 不可变：

  ```java
  final int[] value={1,2,3} ；
  int[] another={4,5,6};
   value=another;    //编译器报错，final不可变 value用final修饰，编译器不允许我把value指向堆区另一个地址。
  但如果我直接对数组元素动手，分分钟搞定。
  
   final int[] value={1,2,3};
   value[2]=100;  //这时候数组里已经是{1,2,100}   所以String是不可变，关键是因为SUN公司的工程师。
   在后面所有String的方法里很小心的没有去动Array里的元素，没有暴露内部成员字段。
  
  private final char value[]//这一句里，private的私有访问权限的作用都比final大。而且设计师还很小心地把整个String设成final禁止继承，避免被其他人继承后破坏。所以String是不可变的关键都在底层的实现，而不是一个final。考验的是工程师构造数据类型，封装数据的功力。
  ```

  > 1 首先final修饰的类只保证不能被继承，并且该类的对象在堆内存中的地址不会被改变。
  >
  > 2 但是持有String对象的引用本身是可以改变的，比如他可以指向其他的对象。
  >
  > 3 final修饰的char数组保证了char数组的引用不可变。但是可以通过char[0] = ‘a’来修改值。不过String内部并不提供方法来完成这一操作，所以String的不可变也是基于代码封装和访问控制的。

# 代码块

- **局部代码块**

  > 位置：局部位置（方法内部）
  >
  > 作用：限定变量的生命周期，尽早释放，节约内存
  >
  > 调用：调用其所在的方法时执行

  ```java
  public class 局部代码块 {
  @Test
  public void test (){
      B b = new B();
      b.go();
  }
  }
  class B {
      B(){}
      public void go() {
          //方法中的局部代码块，一般进行一次性地调用，调用完立刻释放空间，避免在接下来的调用过程中占用栈空间
          //因为栈空间内存是有限的，方法调用可能会会生成很多局部变量导致栈内存不足。
          //使用局部代码块可以避免这样的情况发生。
          {
              int i = 1;
              ArrayList<Integer> list = new ArrayList<>();
              while (i < 1000) {
                  list.add(i ++);
              }
              for (Integer j : list) {
                  System.out.println(j);
              }
              System.out.println("gogogo");
          }
          System.out.println("hello");
      }
  }
  ```

- **构造代码块**：

  > 位置：类成员的位置，就是类中方法之外的位置
  >
  > 作用：把多个构造方法共同的部分提取出来，共用构造代码块
  >
  > 调用：每次调用构造方法时，都会优先于构造方法执行，也就是每次new一个对象时自动调用，对 对象的初始化

  ```java
  class A{
      int i = 1;
      int initValue;//成员变量的初始化交给代码块来完成
      {
          //代码块的作用体现于此：在调用构造方法之前，用某段代码对成员变量进行初始化。
          //而不是在构造方法调用时再进行。一般用于将构造方法的相同部分提取出来。
          //
          for (int i = 0;i < 100;i ++) {
              initValue += i;
          }
      }
      {
          System.out.println(initValue);
          System.out.println(i);//此时会打印1
          int i = 2;//代码块里的变量和成员变量不冲突，但会优先使用代码块的变量
          System.out.println(i);//此时打印2
          //System.out.println(j);//提示非法向后引用，因为此时j的的初始化还没开始。
          //
      }
      {
          System.out.println("代码块运行");
      }
      int j = 2;
      {
          System.out.println(j);
          System.out.println(i);//代码块中的变量运行后自动释放，不会影响代码块之外的代码
      }
      A(){
          System.out.println("构造方法运行");
      }
  }
  public class 构造代码块 {
      @Test
      public void test() {
          A a = new A();
      }
  }
  ```

- **静态代码块**：

  ```java
  /*位置：类成员位置，用static修饰的代码块
  
   作用：对类进行一些初始化  只加载一次，当new多个对象时，只有第一次会调用静态代码块，因为，静态代码块                  是属于类的，所有对象共享一份
  
   调用: new 一个对象时自动调用
  
   public class 静态代码块 {
  */
  @Test
  public void test() {
      C c1 = new C();
      C c2 = new C();
      //结果,静态代码块只会调用一次，类的所有对象共享该代码块
      //一般用于类的全局信息初始化
      //静态代码块调用
      //代码块调用
      //构造方法调用
      //代码块调用
      //构造方法调用
  }
  
  }
  class C{
      C(){
          System.out.println("构造方法调用");
      }
      {
          System.out.println("代码块调用");
      }
      static {
          System.out.println("静态代码块调用");
      }
  }
  ```

  **执行顺序 静态代码块 —–> 构造代码块 ——-> 构造方法**

# java类与包

- 为什么一个文件中只能有一个public的类

  编译器在编译时，针对一个java源代码文件（也称为“编译单元”）只会接受一个public类。否则报错。

- 为什么这个public的类的类名必须和文件名相同？

  是为了方便虚拟机在相应的路径中找到相应的类所对应的字节码文件

- **主函数可以被重载**，但是JVM只识别main（String[] args），其他都是作为一般函数。这里面的args知识数组变量可以更改，其他都不能更改。

  一个java文件中可以包含很多个类，每个类中有且仅有一个主函数，但是每个java文件中可以包含多个主函数，在运行时，需要指定JVM入口是哪个。例如一个类的主函数可以调用另一个类的主函数。不一定会使用public类的主函数。



# java 回调机制

- 模块间调用：

  - 同步调用

    > 同步调用是最基本并且最简单的一种调用方式，类A的方法a()调用类B的方法b()，一直等待b()方法执行完毕，a()方法继续往下走。这种调用方式适用于方法b()执行时间不长的情况，因为b()方法执行时间一长或者直接阻塞的话，a()方法的余下代码是无法执行下去的，这样会造成整个流程的阻塞。

  ![](https://images2015.cnblogs.com/blog/801753/201702/801753-20170221201001413-1766758208.png)

  - 异步调用

    > 异步调用是为了解决同步调用可能出现阻塞，导致整个流程卡住而产生的一种调用方式。类A的方法方法a()通过**新起线程的方式调用类B**的方法b()，代码接着直接往下执行，这样无论方法b()执行时间多久，都不会阻塞住方法a()的执行。
    >
    > 但是这种方式，由于方法a()不等待方法b()的执行完成，在方法a()需要方法b()执行结果的情况下（视具体业务而定，有些业务比如启异步线程发个微信通知、刷新一个缓存这种就没必要），必须通过一定的方式对方法b()的执行结果进行监听。
    >
    > 在Java中，可以使用Future+Callable的方式做到这一点

    ![](https://images2015.cnblogs.com/blog/801753/201702/801753-20170221201512429-1532730453.png)

  - 回调

    > 类A的a()方法调用类B的b()方法 
    > 类B的b()方法执行完毕主动调用类A的callback()方法 
    > 这样一种调用方式组成了上图，也就是一种双向的调用方式。

    ![](https://images2015.cnblogs.com/blog/801753/201702/801753-20170221205712070-824897248.png)

  实例：TOM做题
  数学老师让Tom做一道题，并且Tom做题期间数学老师不用盯着Tom，而是在玩手机，等Tom把题目做完后再把答案告诉老师。

  ```java
    //回调指的是A调用B来做一件事，B做完以后将结果告诉给A，这期间A可以做别的事情。
      //这个接口中有一个方法，意为B做完题目后告诉A时使用的方法。
      //所以我们必须提供这个接口以便让B来回调。
      //回调接口，
      public interface CallBack {
          void tellAnswer(int res);
      }
  ```

  ```java
   //老师类实例化回调接口，即学生写完题目之后通过老师的提供的方法进行回调。
      //那么学生如何调用到老师的方法呢，只要在学生类的方法中传入老师的引用即可。
      //而老师需要指定学生答题，所以也要传入学生的实例。
  public class Teacher implements CallBack{
      private Student student;
  
      Teacher(Student student) {
          this.student = student;
      }
  
      void askProblem (Student student, Teacher teacher) {
          //main方法是主线程运行，为了实现异步回调，这里开启一个线程来操作
          new Thread(new Runnable() {
              @Override
              public void run() {
                  student.resolveProblem(teacher);
              }
          }).start();
          //老师让学生做题以后，等待学生回答的这段时间，可以做别的事，比如玩手机.\
          //而不需要同步等待，这就是回调的好处。
          //当然你可以说开启一个线程让学生做题就行了，但是这样无法让学生通知老师。
          //需要另外的机制去实现通知过程。
          // 当然，多线程中的future和callable也可以实现数据获取的功能。
          for (int i = 1;i < 4;i ++) {
              System.out.println("等学生回答问题的时候老师玩了 " + i + "秒的手机");
          }
      }
  
      @Override
      public void tellAnswer(int res) {
          System.out.println("the answer is " + res);
      }
  }
  ```

  ```java
  //学生的接口，解决问题的方法中要传入老师的引用，否则无法完成对具体实例的回调。
      //写为接口的好处就是，很多个学生都可以实现这个接口，并且老师在提问题时可以通过
      //传入List<Student>来聚合学生，十分方便。
  public interface Student {
      void resolveProblem (Teacher teacher);
  }
  ```

  ```java
  public class Tom implements Student{
  
      @Override
      public void resolveProblem(Teacher teacher) {
          try {
              //学生思考了3秒后得到了答案，通过老师提供的回调方法告诉老师。
              Thread.sleep(3000);
              System.out.println("work out");
              teacher.tellAnswer(111);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
      }
  ```

  ```java
  public class Test {
      public static void main(String[] args) {
          //测试
          Student tom = new Tom();
          Teacher lee = new Teacher(tom);
          lee.askProblem(tom, lee);
          //结果
  //        等学生回答问题的时候老师玩了 1秒的手机
  //        等学生回答问题的时候老师玩了 2秒的手机
  //        等学生回答问题的时候老师玩了 3秒的手机
  //        work out
  //        the answer is 111
      }
  }
  ```

- **多线程的“回调”**：

  Java多线程中可以通过callable和future或futuretask结合来获取线程执行后的返回值。实现方法是通过get方法来调用callable的call方法获取返回值。

  其实这种方法本质上不是回调，回调要求的是任务完成以后被调用者主动回调调用者的接口。而这里是调用者主动使用get方法阻塞获取返回值。

  ```java
  public class 多线程中的回调 {
      //这里简单地使用future和callable实现了线程执行完后
      public static void main(String[] args) throws ExecutionException, InterruptedException {
          ExecutorService executor = Executors.newCachedThreadPool();
          Future<String> future = executor.submit(new Callable<String>() {
              @Override
              public String call() throws Exception {
                  System.out.println("call");
                  TimeUnit.SECONDS.sleep(1);
                  return "str";
              }
          });
          //手动阻塞调用get通过call方法获得返回值。
          System.out.println(future.get());
          //需要手动关闭，不然线程池的线程会继续执行。
          executor.shutdown();
  
      //使用futuretask同时作为线程执行单元和数据请求单元。
      FutureTask<Integer> futureTask = new FutureTask(new Callable<Integer>() {
          @Override
          public Integer call() throws Exception {
              System.out.println("dasds");
              return new Random().nextInt();
          }
      });
      new Thread(futureTask).start();
      //阻塞获取返回值
      System.out.println(futureTask.get());
  }
  @Test
  public void test () {
      Callable callable = new Callable() {
          @Override
          public Object call() throws Exception {
              return null;
          }
      };
      FutureTask futureTask = new FutureTask(callable);
  
  }
  }
  ```

# 异常

![](https://images0.cnblogs.com/blog/381060/201311/22185952-834d92bc2bfe498f9a33414cc7a2c8a4.png)

> **非检查异常**（unckecked exception）：Error 和 RuntimeException 以及他们的子类。javac在编译时，不会提示和发现这样的异常，不要求在程序处理这些异常。所以如果愿意，我们可以编写代码处理（使用try…catch…finally）这样的异常，也可以不处理。
>
> **对于这些异常，我们应该修正代码，而不是去通过异常处理器处理 。这样的异常发生的原因多半是代码写的有问题**，如除0错误ArithmeticException，错误的强制类型转换错误ClassCastException，数组索引越界ArrayIndexOutOfBoundsException，使用了空对象NullPointerException等等。
>
> **检查异常**（checked exception）：除了Error 和 RuntimeException的其它异常。javac强制要求程序员为这样的异常做预备处理工作（使用try…catch…finally或者throws）。在方法中要么用try-catch语句捕获它并处理，要么用throws子句声明抛出它，否则编译不会通过。
>
> 这样的异常一般是由程序的运行环境导致的**。因为程序可能被运行在各种未知的环境下，而程序员无法干预用户如何使用他编写的程序，于是程序员就应该为这样的异常时刻准备着**。如SQLException , IOException,ClassNotFoundException 等。

异常是在执行某个函数时引发的，而函数又是层级调用，形成调用栈的，因为，只要一个函数发生了异常，那么他的所有的caller都会被异常影响。当这些被影响的函数以异常信息输出时，就形成的了**异常追踪栈**。

异常最先发生的地方，叫做**异常抛出点**

![](https://images2017.cnblogs.com/blog/858860/201709/858860-20170911121451422-233079767.png)

```java
public class 异常处理方式 {

@Test
public void main() {
    try{
        //try块中放可能发生异常的代码。
        InputStream inputStream = new FileInputStream("a.txt");

        //如果执行完try且不发生异常，则接着去执行finally块和finally后面的代码（如果有的话）。
        int i = 1/0;
        //如果发生异常，则尝试去匹配catch块。
        throw new SQLException();
        //使用1.8jdk同时捕获多个异常，runtimeexception也可以捕获。只是捕获后虚拟机也无法处理，所以不建议捕获。
    }catch(SQLException | IOException | ArrayIndexOutOfBoundsException exception){
        System.out.println(exception.getMessage());
        //每一个catch块用于捕获并处理一个特定的异常，或者这异常类型的子类。Java7中可以将多个异常声明在一个catch中。

        //catch后面的括号定义了异常类型和异常参数。如果异常与之匹配且是最先匹配到的，则虚拟机将使用这个catch块来处理异常。

        //在catch块中可以使用这个块的异常参数来获取异常的相关信息。异常参数是这个catch块中的局部变量，其它块不能访问。

        //如果当前try块中发生的异常在后续的所有catch中都没捕获到，则先去执行finally，然后到这个函数的外部caller中去匹配异常处理器。

        //如果try中没有发生异常，则所有的catch块将被忽略。

    }catch(Exception exception){
        System.out.println(exception.getMessage());
        //...
    }finally{
        //finally块通常是可选的。
        //无论异常是否发生，异常是否匹配被处理，finally都会执行。

        //finally主要做一些清理工作，如流的关闭，数据库连接的关闭等。
    }
 
```

一个try至少要跟一个catch或者finally

```java
try {
        int i = 1;
    }finally {
        //一个try至少要有一个catch块，否则， 至少要有1个finally块。但是finally不是用来处理异常的，finally不会捕获异常。
    }
}
```

异常出现时该方法后面的代码不会运行，即使异常已经被捕获。这里举出一个奇特的例子，在catch里再次使用try catch finally

```java
@Test
public void test() {
    try {
        throwE();
        System.out.println("我前面抛出异常了");
        System.out.println("我不会执行了");
    } catch (StringIndexOutOfBoundsException e) {
        System.out.println(e.getCause());
    }catch (Exception ex) {
    //在catch块中仍然可以使用try catch finally
        try {
            throw new Exception();
        }catch (Exception ee) {

        }finally {
            System.out.println("我所在的catch块没有执行，我也不会执行的");
        }
    }
}
//在方法声明中抛出的异常必须由调用方法处理或者继续往上抛，
// 当抛到jre时由于无法处理终止程序
public void throwE (){
//        Socket socket = new Socket("127.0.0.1", 80);

        //手动抛出异常时，不会报错，但是调用该方法的方法需要处理这个异常，否则会出错。
//        java.lang.StringIndexOutOfBoundsException
//        at com.javase.异常.异常处理方式.throwE(异常处理方式.java:75)
//        at com.javase.异常.异常处理方式.test(异常处理方式.java:62)
        throw new StringIndexOutOfBoundsException();
    }
```

- **throws**

  throws是另一种处理异常的方式，它不同于**try…catch…finally**，throws仅仅是将函数中可能出现的异常向调用者声明，而自己则不具体处理。

  采取这种异常处理的原因可能是：方法本身不知道如何处理这样的异常，或者说让调用者处理更好，调用者需要为可能发生的异常负责。

- **finally**

  finally块不管异常是否发生，**只要对应的try执行了，则它一定也执行**。只有一种方法让finally块不执行：`System.exit()`。因此finally块通常用来做资源释放操作：**关闭文件，关闭数据库连接等等**

  当一个线程在执行try语句块或者catch语句块被打断（interrupted）或者被终止（killed），与其相对应的finally语句块可能不会执行。更极端是是运行try语句块或者catch语句块突然死机，断电等。

- **throw 异常抛出语句**

  程序员也可以通过throw语句手动显式的抛出一个异常。throw语句的后面必须是一个异常对象。

  **throw 语句必须写在函数中**，执行throw 语句的地方就是一个异常抛出点，它和由JRE自动形成的异常抛出点没有任何差别

- **异常的链化**：

  **以一个异常对象为参数构造新的异常对象。新的异对象将包含先前异常的信息**。这项技术主要是异常类的一个带Throwable参数的函数来实现的。这个当做参数的异常，我们叫他根源异常（cause）。

  下面是一个例子，演示了异常的链化：从命令行输入2个int，将他们相加，输出。输入的数不是int，则导致getInputNumbers异常，从而导致add函数异常，则可以在add函数中抛出

  ```java
  public static void main(String[] args)
  {
      
      System.out.println("请输入2个加数");
      int result;
      try
      {
          result = add();
          System.out.println("结果:"+result);
      } catch (Exception e){
          e.printStackTrace();
      }
  }
  //获取输入的2个整数返回
  private static List<Integer> getInputNumbers()
  {
      List<Integer> nums = new ArrayList<>();
      Scanner scan = new Scanner(System.in);
      try {
          int num1 = scan.nextInt();
          int num2 = scan.nextInt();
          nums.add(new Integer(num1));
          nums.add(new Integer(num2));
      }catch(InputMismatchException immExp){
          throw immExp;
      }finally {
          scan.close();
      }
      return nums;
  }
  
  //执行加法计算
  private static int add() throws Exception
  {
      int result;
      try {
          List<Integer> nums =getInputNumbers();
          result = nums.get(0)  + nums.get(1);
      }catch(InputMismatchException immExp){
          throw new Exception("计算失败",immExp);  /////////////////////////////链化:以一个异常对象为参数构造新的异常对象。
      }
      return  result;
  }
  
  /*
  请输入2个加数
  r 1
  java.lang.Exception: 计算失败
      at practise.ExceptionTest.add(ExceptionTest.java:53)
      at practise.ExceptionTest.main(ExceptionTest.java:18)
  Caused by: java.util.InputMismatchException
      at java.util.Scanner.throwFor(Scanner.java:864)
      at java.util.Scanner.next(Scanner.java:1485)
      at java.util.Scanner.nextInt(Scanner.java:2117)
      at java.util.Scanner.nextInt(Scanner.java:2076)
      at practise.ExceptionTest.getInputNumbers(ExceptionTest.java:30)
      at practise.ExceptionTest.add(ExceptionTest.java:48)
      ... 1 more
  
  */
  ```

  ![](https://images2017.cnblogs.com/blog/858860/201709/858860-20170913181056469-1080507673.png)

自定义的异常：
如果要自定义异常类，则扩展Exception类即可，因此这样的自定义异常都属于检查异常（checked exception）。如果要自定义非检查异常，则扩展自RuntimeException

按照国际惯例，自定义的异常应该总是包含如下的构造函数：

- 一个无参构造函数
- 一个带有String参数的构造函数，并传递给父类的构造函数。
- 一个带有String参数和Throwable参数，并都传递给父类构造函数
- 一个带有Throwable 参数的构造函数，并传递给父类的构造函数。

```java
public class IOException extends Exception
{
    static final long serialVersionUID = 7818375828146090155L;

    public IOException()
    {
        super();
    }

    public IOException(String message)
    {
        super(message);
    }

    public IOException(String message, Throwable cause)
    {
        super(message, cause);
    }

    
    public IOException(Throwable cause)
    {
        super(cause);
    }
}
```

父类方法throws 的是2个异常，子类就不能throws 3个及以上的异常。父类throws IOException，子类就必须throws IOException或者IOException的子类

- **finally中的return语句会抑制（消灭）前面try或catch中的异常**

  ```java
  class TestException
  {
      public static void main(String[] args)
      {
          int result;
          try{
              result = foo();
              System.out.println(result);           //输出100
          } catch (Exception e){
              System.out.println(e.getMessage());    //没有捕获到异常
          }
          
          
          try{
              result  = bar();
              System.out.println(result);           //输出100
          } catch (Exception e){
              System.out.println(e.getMessage());    //没有捕获到异常
          }
      }
      
      //catch中的异常被抑制
      @SuppressWarnings("finally")
      public static int foo() throws Exception
      {
          try {
              int a = 5/0;
              return 1;
          }catch(ArithmeticException amExp) {
              throw new Exception("我将被忽略，因为下面的finally中使用了return");
          }finally {
              return 100;
          }
      }
      
      //try中的异常被抑制
      @SuppressWarnings("finally")
      public static int bar() throws Exception
      {
          try {
              int a = 5/0;
              return 1;
          }finally {
              return 100;
          }
      }
  }
  ```

- **finally中的异常会覆盖或（消灭）前面try或catch中的异常**

  ```java
  class TestException
  {
      public static void main(String[] args)
      {
          int result;
          try{
              result = foo();
          } catch (Exception e){
              System.out.println(e.getMessage());    //输出：我是finaly中的Exception
          }
          
          
          try{
              result  = bar();
          } catch (Exception e){
              System.out.println(e.getMessage());    //输出：我是finaly中的Exception
          }
      }
      
      //catch中的异常被抑制
      @SuppressWarnings("finally")
      public static int foo() throws Exception
      {
          try {
              int a = 5/0;
              return 1;
          }catch(ArithmeticException amExp) {
              throw new Exception("我将被忽略，因为下面的finally中抛出了新的异常");
          }finally {
              throw new Exception("我是finaly中的Exception");
          }
      }
      
      //try中的异常被抑制
      @SuppressWarnings("finally")
      public static int bar() throws Exception
      {
          try {
              int a = 5/0;
              return 1;
          }finally {
              throw new Exception("我是finaly中的Exception");
          }
          
      }
  }
  ```

  建议：

  - 不要在fianlly中使用return。
  - 不要在finally中抛出异常。
  - 减轻finally的任务，不要在finally中做一些其它的事情，**finally块仅仅用来释放资源是最合适的。**
  - 将尽量将所有的return写在函数的最后面，而不是try ... catch ... finally中。