## 不可具体化类型（Non-Reifiable Types）

类型擦除一节讲到了编译器擦除类型参数信息的过程。类型擦除在遇到可变参数时，会遇到平时见不到的一些问题。


### 不可具体化类型

一个可具体化类型指的是类型信息在运行期是可以完全获得的。这包括基本数据类型、非泛型类、raw types、无限定的通配符调用。

不可具体化类型指的是那些在编译器被类型擦除过程擦除了信息--没有使用无限定通配符定义的泛型类调用。一个不可具体化类型，在运行期，没有保留完整的类型信息。不可具体化类型的例子包括：List<String>，List<Number>等。JVM在运行期无法区别这两个类型。在’泛型的限制‘一节中，介绍了不能使用不可具体化类型的一些场景：不能使用instanceof表达式，不能作为数组元素。


### Heap Pollution

当一个参数化类型变量指向的对象不是它定义的那种类型的时候，就会发生heap pollution.如果程序中执行了一些操作，这些操作在编译时出现了unchecked警告，那么heap pollution就有可能发生。一个unchecked警告，通常是由于无法确定涉及到某个参数化类型的操作（例如cast或者method call）是否正确的时候产生的，或者在编译器或者在运行期(取决于编译期类型检查的规则限制)。例如，当混合使用raw type和参数化类型时或者执行unchecked cast的时候，可能会发生heap pollutiuon。


在正常的情况下，当所有的代码一同编译时，编译器会报告unchecked warning，来提醒你潜在的heap pollution。但是，如果你分别编译代码的不同部分，就难以检测到潜在的heap pollution。如果你确定你的代码编译时没有这样的警告信息，那么就不会发生heap pollution。


### 使用不可具体化类型作为可变参数时的潜在漏洞


包含可变参数的泛型方法可能会引起heap plollution.

考虑如下的ArrayBuilder类：

```
public class ArrayBuilder {
	public static <T> void addToList(List<T> listArg, T... elements) {
		for (T x : elements) {
			listArg.add(x);
		}
	}
	
	public static void faultyMethod(List<String> ... l) {
		Object[] obhectArray = l; // valid
		objectArray[0] = Arrays.asList(42);
		String s = l[0].get(0); // ClassCastException thrown here
	}
}

```

下面的例子，HeapPollutionExample使用了ArrayBuilder类：

```
public class HeapPollutionExample {

  public static void main(String[] args) {

    List<String> stringListA = new ArrayList<String>();
    List<String> stringListB = new ArrayList<String>();

    ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
    ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
    List<List<String>> listOfStringLists =
      new ArrayList<List<String>>();
    ArrayBuilder.addToList(listOfStringLists,
      stringListA, stringListB);

    ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  }
}

```

在编译这段代码时，编译器会产生关于addToList方法的如下警告：


```
warning: [varargs] Possible heap pollution from parameterized vararg type T

```

当编译器遇见一个可变参数的方法时，它将形参转化为一个数组。但是，java编程语言不允许创建参数化类型的数组。在ArrayBuilder的addToList方法中，编译器将可变参数的形参T... elements翻译为T[] elements。但是，因为类型擦除，编译器又将T[] elements翻译为Object[] elements, 结果，就有可能造成heap pollution。

下面的语句将可变参数形参l赋予了一个Object类数组：

```
Object[] objectArray = l;

```

上述语句可能引入heap pollution。一个不匹配可变参数l的参数化类型的值可能会赋值给objectArray，间接的赋值给了l。但是，编译器不会针对这条语句发出一个unchecked warning。编译器已经在将可变参数的形参List<String>... l转换为List[] l的时候发出了警告。这条语句是合法的，变量l的类型是List[]，list[]是Object[]的子类型。


结果，当你将任意类型的list赋予objectArray的任意元素的时候，编译器并没有发出一条警告或者错误，如下所示：

```
objectArray[0] = Arrays.asList(42);

```

上述语句将objectArray的第一个元素赋予了一个Integer类型的List。


假设你使用如下语句调用ArrayBuilder.faultyMethod:

ArrayBuilder.faultyMethod(Arrays.asList("hello!"), Arrays.asList("world!"));


在运行期，JVM会发生ClassCastException：

```
String s = l[0].get(0); // ClassCastException thrown here

```

objectArray的第一个元素存储的类型是List<Integer>, 但是上述语句期望的类型是List<String>.


### 当使用不可具体化类型的可变参数时抑制警告


如果你定义了可变参数方法，参数类型为形式化参数，并且你确定此方法不会因为不当的处理可变参数形参，产生ClassCastException或者其他类似的Exception的话，你可以阻止编译器产生这种类型的警告，通过给静态的非构造方法(这里包括static方法、final方法)加下面的注解，

```
@SafeVarargs

```


@SafeVarargs注解是方法定义的一部分(The @SafeVarargs annotation is a documented part of the method's contract)。这个注解声称方法的实现不会不恰当的操作可变参数。

使用如下的方式也可以抑制编译器产生类似的警告，但是不推荐使用。

还有，这个方法也不会在方法实际调用的地方抑制警告（However, this approach does not suppress warnings generated from the method's call site.） 

```
@SuppressWarnings({"unchecked", "varargs"})

```






































