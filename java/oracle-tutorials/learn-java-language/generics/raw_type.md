## Raw Types

一个泛型类或接口的无参形式成为原始类型，例如，下面的泛型类Box：

```
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}

```

你可以通过传递类型实参来替换T，创建一个Box的实例：

```
Box<Integer> intBox = new Box<>();

```

如果忽略类型实参，你创建了一个Box<T>的原始类型：

```
Box rawBox = new Box();

```

因此，Box是泛型类Box<T>的raw type。但是，一个非泛型类或者接口不是一个raw type。


在JDK5.0之前，还没有泛型特性，所以在java5加入泛型特性之后，为了保证以前的代码的兼容性，允许使用raw type。当使用raw type时，你实际上得到了pre-generics behavior，一个包含Object对象的Box。为了向后兼容，将一个参数化类型赋值给一个原始类型是可以的：

```
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK

```

但是如果你将一个原始类型赋值给一个参数化类型，你会得到一个警告：

```
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion

```

如果你使用一个原始类型来进行泛型方法的调用，同样会得到警告：

```
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)

```

上述的警告表明，原始类型忽略了泛型类型检查，将发现不安全代码的时机推迟到了运行期。因为你应该避免使用raw type。


### Unchecked Error Message


前面提到，当混合使用老代码和泛型代码时，你可能会遇到如下所示的警告：

```
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

```


这在操作raw types时会发生，比如如下代码：

```
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}


```

"unchecked"表示编译器没有足够的信息来执行类型检查以保证类型安全。"unchecked"警告默认是关闭的，虽然编译器给出了一个暗示。为了看到所有的“unchecked”警告，使用如下的设置-Xlint:unchecked来重新编译你的代码。


使用 -Xlint:unchecked来重新编译代码，展示了如下的额外信息：

```
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning

```

为了完全禁止unchecked警告，使用参数 -Xlint:-unchecked。注解@SuppressWarnings("unchecked")也会抑制unchecked警告。




