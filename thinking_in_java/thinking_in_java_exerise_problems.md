# 5 初始化与清理

1. 定义就被初始化了的String域与另一个通过构造器初始化的String域，这两者的差异？

```
The s1 field is initialized before the constructor is entered; technically, so is the s2 field, which is set to null as the object is created. The more flexible s2 field lets you choose what value to give it when you call the constructor, whereas s1 always has the same value.
```

1. 

   * ```
     //: initialization/E11_FinalizeAlwaysCalled.java
     /****************** Exercise 11 ****************
     
     - Modify Exercise 10 so your
     - finalize() will always be called.
       ***********************************************/
       56 Thinking in Java, 4th Edition Annotated Solution Guide
       package initialization;
       public class E11_FinalizeAlwaysCalled {
       protected void finalize() {
       System.out.println("finalize() called");
       }
       public static void main(String args[]) {
       new E11_FinalizeAlwaysCalled();
       System.gc();
       System.runFinalization();
       }
       } /* Output:
       finalize() called
       *///:~Calling System.gc( ) and System.runFinalization( ) in sequence will
     probably but not necessarily call your finalizer (The behavior of finalize has been
     uncertain from one version of JDK to another.) The call to these methods is just
     a request; it doesn’t ensure the finalizer will actually run. Ultimately, nothing
     guarantees that finalize( ) will be called.
     ```

