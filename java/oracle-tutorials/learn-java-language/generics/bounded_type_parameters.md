## 有限定的类型参数

有时候我们想要限制类型参数可以传递的实参类型。例如，一个操作数字的方法可能只想接受Number类和它的子类型。这就是有限制的类型参数。

为了声明一个有限定的类型参数，需要在实参名后面加上extends关键字，后面跟上它的超类名字，此例中超类就是Number。注意，在这个上下文中，extends关键字的意思是extends(对于类来说)或者implements(对于接口来说)。

```
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}

```

上面的代码会报错，因为我们调用inspect方法的时候传递了一个String类型的参数：

```
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("some text");
                                  ^
1 error

```

除了可以在实例化泛型类型的对象时使用限定类型，你也可以调用限定类型的方法：

```
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}

```


上例中，因为限定了T的超类是Integer，所以你可以在isEven()方法中，调用n.intValue方法，如果不指定限定类型，你是无法调用intValue()方法的。


### 多类型限制

前面的例子介绍了如何声明限定类型，除此之外，一个类型参数可以被限制为多种类型：

```
<T extends B1 & B2 & B3>

```

如上所示，T既是B1的类型，又是B2和B3的类型，其中B1 B2 B3最多有一个是Class类型。如果B1 B2 B3有一个是Class，其余是Interface，那么java语法规定，Class必须放在前面，例如：

```
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }

```

如果不将A放在最前面，你会得到一个编译器错误：

```
class D <T extends B & A & C> { /* ... */ }  // compile-time error

```














