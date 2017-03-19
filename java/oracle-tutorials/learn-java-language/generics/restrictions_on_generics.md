## 使用泛型的限制


如果想更有效的使用泛型，必须考虑如下的限制：

### 不能使用基本数据类型实例化泛型类型

考虑如下的参数化类型：

```
class Pair<K, V> {
	private K key;
	private V value;
	
	public Pair(K key, V value) {
		this.key = key;
		this.value = value;
	}
}

```

当创建一个Pair类的实例时，你不能使用基本数据类型来替换类型参数K和V：

```
Pair<int, char> p = new Pair<>(8, 'a'); // 编译器错误

```

你只可以使用非基本数据类型来替换类型参数:

```
Pair<Integer, Character> p = new Pair<>(8, 'a');

```

注意，java编译器的自动装箱特性会自动把8包装为Integer，把'a'包装为Character:

```
Pair<Integer, Character> p = new Pair<>(Integer.valueOf(8), new Character('a'));

```

### 不能实例化参数化类型


你不能创建一个类型参数的实例。例如，下面的代码会引起编译器错误：

```
public static <E> void append(List<E> list) {
	E elem = new E(); // 编译器错误
	list.add(elem);
}

```

作为一个解决方法，你可以通过反射来创建一个参数化类型的实例：

```
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
	E elem = cls.newInstance();
	list.add(elem);
}

```

你可以使用如下方式来调用append方法：

```
List<String> ls = new ArrayList<>();
append(ls, String.class);

```

### 不能声明类型为参数化类型的静态域


一个类的静态域是类级别的变量，它被所有的实例共享。因此，类型为type parameters的静态域是不允许被创建的，考虑如下的类：

```
public class MobileDevice<T> {
	private static T os;
}

```

如果运行创建类型参数的静态域，那么下面的代码就会变得不明确：

```
MobileDevice<Smartphone> phone = new MobileDevice<>();
MobileDevice<Pager> pager = new MobileDevice<>();
MobileDevice<TabletPC> pc = new MobileDevice<>();

```

因为静态域os被phone pager 和pc共享，所以，os的真实类型到底是什么？它不可能同时为SmartPhone、Pager和TabletPC。所以，你不能创建一个类型为参数类型的静态域。


### 不能对参数化类型使用cast或者instanceof语法

因为java编译器擦除了所有的类型参数，你不能在运行时确定真实类型到底是哪个：

```
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
        // ...
    }
}

```

你可能会传给rtti几个参数类型：ArrayList<Integer>, ArrayList<String>, LinkedList<Character>...


运行时无法追踪这些信息，所以无法区分ArrayList<Integer>和ArrayList<String>。你能做的只能是使用一个无限制的通配符来确认参数是否是一个ArrayList：

```
public static void rtti(List<?> list) {
	if (list instanceod ArrayList<?>) { // OK; instanceof需要一个确定的类型
		//..
	}
}

```

通常，你不能强制转换一个参数化类型，除非它又一个无限定的通配符，例如：

```
List<Integer> li = new ArrayList<>();
List<Number> ln = (List<Number>)li; // 编译器错误

```

但是，在一些情况下，如果编译器知道一个类型参数总是合法的，这时候就允许使用强制类型转换，例如：

```
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1; // OK， 因为ArrayList<String>是List<String>的subtype

```


### 不能创建参数化类型数组


你不能创建一个参数化类型数组，例如，下面的代码是通不过编译的：

```
List<Integer>[] arrayOfLists = new List<Integer>[2]; // 编译时错误

```

下面的代码说明了当不同类型的元素插入数组时发生了什么：

```
Object[] strings = new String[2];
strings[0] = "hi";
strings[1] = 100; // ArrayStoreException is thrown

```

如果使用泛型列表做同样的事情，结果如下：

```
Object[] stringLists = new List<String>[]; // compiler error, but pretend it's allowed 
stringLists[0] = new ArrayList<String>(); // ok
stringLists[1] = new ArrayList<Integer>(); // An ArrayStoreException should be thrown, but the runtime can't detect it.

```

如果允许创建参数化类型数组，那么上述代码可以正常运行，不会抛出预期的ArrayStoreException。


### cannot create、catch、throw 参数化类型的对象

一个泛型类不能直接或间接的继承Throwable类，例如，下面的类不能通过编译：

```
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error


````


方法不能catch参数化类型的对象:

```
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}

```

但是，你可以在throw语句中，使用类型参数：

```
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}

```


### 如果泛型参数被擦除后得到相同的raw type的话，这时不能实现方法重载


在类型擦除之后，一个类不能有两个重载的方法：

```
pubic class Example {
	public void print(Set<String> strSet){}
	public void print(Set<Integer> inSet){}
}

```

重载的方法将会有相同的签名、相同的classfile representation，这将会引起编译器错误。
































