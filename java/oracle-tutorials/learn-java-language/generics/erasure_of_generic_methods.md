## 泛型方法擦除

java编译器也会擦除泛型方法的参数中的类型参数，考虑如下的泛型方法：

```
// Counts the number of occurrences of elem in anArray.
//
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}

```

因为T是无限定的，所有，java编译器会使用Object类来替换类型参数T：

```
public static int count(Object[] anArray, Object elem) {
	int cnt = 0;
	for (Object e : anArray) {
		if (e.equals(elem)) {
			++cnt;
		}
		return cnt;
	}
}

```

假设已经定义了如下的类：

```
class Shape { /* ... */}
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }

```

我们编写了一个泛型方法，用来画不同的形状：

```
public static <T extends Shape> void draw(T shape) {
	/* ... */
}

```

java编译器会使用Shape来替换T：

```
public static void draw(Shape shape) { /* ... */ }

```



























```