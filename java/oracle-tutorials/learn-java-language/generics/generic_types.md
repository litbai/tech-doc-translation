## Generic Types

一个泛型类型(Generic Types)是一个以type作为参数的泛型类或者接口。

### 一个简单的Box类

一开始的示例，Box类是一个非泛型的普通类。它只需要两个方法：set和get：

```
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}

```

因为它的方法的参数和返回值都是Object类，所以你可以传递除了基本数据类型之外的任意类型的参数。在编译期，我们无法判定传递的真实类型是什么。一个地方的代码可能会给set()方法传递一个Integer对象，然后使用get方法得到这个Integer对象。但是另一个地方的代码可能会传入一个String对象（任意引用类型都是合法的），这样的话前面那个地方的的get()方法就会产生运行时异常。

### Box类的泛型版本

一个泛型类的定义形式如下：

```
class name<T1, T2, ..., Tn> { /* ... */ }

```

类型参数部分，定义在尖括号中，类名之后。它定义了类型参数(type parameters，也称为type variables)T1,T2...Tn.

为了将Box类改为泛型版本吗，你需要创建一个泛型声明，通过将代码“public class Box”改为"public class Box<T>"。这样，引入了类型参数T，这个T可以在类的任意地方使用。改版后的Box类如下所示：

```
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}

```

我们可以看到，所有的Object出现的地方都被换为了类型参数T。一个类型参数可以是你定义的任意非基本类型：任意的class，任意的interface，任意数组类型，甚至是另一个类型参数。


### 类型参数命名惯例


根据惯例，类型参数名称推荐使用单独的大写的字母。这种惯例可能与你已经知道的其他的命名惯例差距比较大，但是这样命名是有根据的：如果没有这个约定，我们将很难辨别一个类型参数和一个普通的类或者接口。

最常使用的泛型参数如下所示：

* E-Element，通常在集合框架中使用
* K-Key
* N-Number
* T-Type
* V-Value
* S,U,V etc-2nd, 3rd, 4th types


你将会在Java SE API和下面的教程中看到这些名字。


### 调用和实例化一个泛型类型

为了引入一个泛型类，你必须进行一个泛型类型调用(generic type invocation)，使用实参来替换T，例如：

Box<Integer> integerBox;


你可以将泛型类型调用想象为一个普通的方法调用，但是跟给方法传递参数不同的是，你需要传递一个类型参数给Box类，在此例中是Integer。


** Type Parameter vs Type Argument：许多开发者会使用术语Type Parameter和Type Argument，但是这两个术语并不相同。当编码时，一个开发者需要提供type argument来创建一个参数化类型。因为，Foo<T>中的T是一个type parameter，而Foo<String>中的String是type argument。本节遵守上面的定义，可以简单理解为type parameter为形参，而type argument是实参。 **


就像其他的变量声明一样，上面的代码并没有创建一个新的Box对象。它只是简单的声明了一个变量，integerBox，这个变量将会引用一个Box<Integer>对象。

一个泛型类型调用通常称为参数化类型（An invocation of a generic type is generally known as a parameterized type）。

如果想实例化这个类，你需要使用new关键字,并且在类名和括号之间加上<Integer>:

```
Box<Integer> integerBox = new Box<Integer>();

```

### Diamond


Java SE7以后，你可以将用'<>'来替代类型参数'<Integer>'，只要编译器能够根据上下文决定或者推测出类型参数。这对尖括号'<>'，称为diamond。所以你可以使用如下代码来创建一个Box<Integer>的实例：

```
Box<Integer> integerBox = new Box<>();

```

### 多元类型参数

根据前面所述，一个泛型类可以有多个类型参数，例如，泛型类OrderedPair，实现了泛型Pair接口：

```
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
	this.key = key;
	this.value = value;
    }

    public K getKey()	{ return key; }
    public V getValue() { return value; }
}

```

下面的代码创建了两个OrderedPair类的实例：

```
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");

```

’new OrderedPair<String, Integer>‘这行代码将形参K替换为实参String，形参V替换为实参Integer。因为编译器具有自动装箱功能，所以，你可以传递String和int类型参数，如上所示。


根据前面介绍的java 7之后的diamond特性，编译器可以通过声明语句'OrderedPair<String, Integer>'推测出K和V的实参，所以可以利用diamond特性来简化代码：

```
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");

```

创建泛型接口和创建泛型类遵循同样的规则。


### 参数化类型

你可以使用参数化类型来替换一个类型形参，如下所示：


```
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>());

```























































