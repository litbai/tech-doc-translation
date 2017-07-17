### Formatting


实现了formatting的流对象或者是PrintWriter的实例或者是PrintStream的实例，一个是字符流，一个是字节流。

在编程中，可能仅仅会使用PrintStream的System.out和System.err对象。当你需要创建一个具备格式化功能的输出流时，总是使用PrintWriter而非PrintStream。

像所有的字节流和字符流对象一样，PrintStream和PrintWriter的对象也实现了一系列标准的write方法，来执行简单的字节或字符输出。初次之外，PrintStream和PrintWriter类都实现了同样的格式化方法，将内部数据转化为一定的格式进行输出。格式化方法包括两种：

* print和println：对单个数据进行标准的格式化
* format：可以基于一个格式化串，格式化任意数量的数据，并提供了多种精度的格式。


#### print和println方法

print和println方法用于输出一个单独的值，通过调用合适的toString方法，进行输出。例如：

```
public class Root {
	public static void main(String[] args) {
		int i = 2;
		double r = Math.sqrt(i);
		
		System.out.print("The sqeare root of ");
		System.out.print(i);
		System.out.print(" is ");
		System.out.print(r);
		System.out.println(".");
		
		i = 5;
		r = Math.sqrt(i);
		System.out.println("The square root of " + i + " is " + r + ".");
	}
}

```


#### The format Method

format方法可以根据一个格式化的字符串，来格式化多个参数。格式化字符串由静态的文本和format specifier组成。静态的文本会原样输出，而format specifier起到格式化的作用。

格式化字符串支持很多特性。在本节，我们只会介绍一些最基本的特性。如果想查看完整的描述，查看API： https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html#syntax。


下面的Root2例子使用了format格式化了2个值：

```
public class Root2 {
	public static void main(String[] args) {
		int i = 2;
		double r = Math.sqrt(i);
		System.out.format("The square root of %d is %f.%n", i, r);
	}
}

```

上述程序的输出为：

```
The square root of 2 is 1.414214.

```


上面的示例使用了3个format specifier，分别为%d, %f, %n. 所有的format specifier都以%开头，以1-2个特殊字符结尾，这些特殊字符是提前约定的，每个字符代表不同的格式化方式。上述的3个format specifier代表的格式化方式为：

* d将一个interger格式化为一个十进制数。
* f将一个float转化为一个十进制数。
* n输出一个平台特定的换行符。


还有其他的几种format specifier:

* x将一个integer格式化为一个16进制数
* s将任意值格式化为字符串
* tB将一个日期格式化为一个locale-specifier的月份名称。


除此之外，还有很多format specifier。


** Note： 除了%%和%n之外，所有的format specifier都必须匹配一个参数，如果缺失匹配的参数，将会抛出异常。
	在java编程语言中， '\n'总是对应字符'\u000A'。当你想要换行时，不要使用\n，而应该使用%n来产生平台特定的换行。

**


除了d、n、s等这个约定字符外，format specifier还可以包含其他的元素，来进一步格式化字符串。下面的示例使用了尽可能多的format specifier：

```
public class Format {
	public static void main(String[] args) {
		System.out.format("%f, %1$+020.10f %n", Math.PI);
	}
}

```

程序的输出为：

```
3.141593, +00000003.1415926536

```

其他这些所有元素都是可选项，下面的图片将这个很长的格式化字符串进行了分解：

![](io-spec.gif)


上述指定的格式化元素必须按序出现，从右到左，依次为：

* Precision： 对于浮点数而言，这指定了精确的位数对于字符串或者其他类型，这指的是最大的宽度，如果字符串超过了这个宽度，会被截断。
* Width：需要格式化的值的最小宽度，如果需要格式户的值的宽度不够，将会进行填充，貌似使用blanks从左边填充。
* Flags：上述示例中，使用+ Flag指定了输出需要带符号，0 Flags指定了填充字符为'0'。还有其他的Flag，比如'-'（指定填充的方向为右边), ','指定了将数值加入千位分隔符。注意，有些Flag是互斥的，不能同时使用。
* Argument Index，可以让你精确的制定需要匹配的参数的index(索引从1开始)，你还可以使用'<'来表示与前一个specifier匹配相同的参数，所以上述的实例还可以这样写： 

```
System.out.format("%f, %<+020.10f %n", Math.PI);

```


































