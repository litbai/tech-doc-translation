## Generics Methods

泛型方法是指引入了类型参数的方法。它类似于声明一个泛型类型，但是这个类型参数的范围仅限于声明它的方法内部。静态的和非静态的方法都可以使用泛型，构造函数也可以使用泛型。

声明泛型方法的语法包括一个尖括号，尖括号里是类型参数，这个尖括号必须在返回值的前面。对于静态的泛型方法，类型参数必须在方法的返回值前面,简单来所，声明泛型方法时把<T>这种类型参数紧紧放在返回值前面就可以了。

Util类包含一个泛型方法，compare，这个方法会比较两个Pair对象：

```
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}


public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}

```

调用泛型方法的完整语法为：

```
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);

```

上述调用显示的指定了类型参数<Integer, String>，通常，这可以省略，编译器可以推测出类型实参：

```
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);

```

这个特性称为类型推测，可以使你以调用一个普通方法的形式来调用泛型方法，不需要在尖括号中显示的制定类型实参。






