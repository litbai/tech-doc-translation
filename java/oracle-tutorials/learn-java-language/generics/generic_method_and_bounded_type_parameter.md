## 泛型方法和限定类型参数

有限定的类型参数是实现泛型方法的关键。考虑如下的方法，它计算一个数组中大于某个指定值elem的元素的个数：

```
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}

```

这个方法的实现是很明了的，但是它通不过编译，因为操作符'>'仅能用于类似short、int、double、long、float、char等的基本数据类型。你不能使用'>'比较两个对象。为了解决这个问题，使用一个限定类型参数, 限定参数必须是Comparable<T>接口的子类型：

```
public interface Comparable<T> {
    public int compareTo(T o);
}


public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}

```
































