## 类型推测

类型推测是java编译器可以根据每个方法调用和相应的方法声明来决定实际的类型参数的一种能力。推测算法可以推测方法参数的类型、返回值的类型。最终，推测算法会试图找到可以使方法运行的最具体的类型。

为了解释上述观点，在下面的示例中，推测算法会推测传递给pick方法的第二个参数类型是Serializable：

```
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());

```

### 类型推测和泛型方法

针对泛型方法，引入了类型推测机制，可以使你像调用普通方法那样来调用泛型方法，不需要在<>中指定具体的类型。请看如下的示例，BoxDemo类：

```
public class BoxDemo {

	public static <U> void addBox(U u, List<Box<U>> boxes) {
		Box<U> box = new Box<>();
		box.set(u);
		boxex.add(box);
	}
	
	public staic <U> void outputBoxes(List<Box<U>> boxes) {
		int counter = 0;
		for (Box<U> box : boxes) {
			U boxContents = box.get();
			System.out.println("Box #" + counter + "contains [" + boxContents.toString() + "]");
			counter++;
		}
	}
	
	public static void main(String[] args) {
		ArrayList<Box<Integer>> boxes = new ArrayList<>();
		BoxDemo<Integer>.addBox(Integer.valueOf(10), boxes);
		BoxDemo.addBox(Integer.valueOf(20), boxes);
		BoxDemo.addBox(Integer.valueOf(30), boxes);
		BoxDemo.outputBoxes(boxes);
	}
}

```

上述代码的输出如下：

```
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]

```


泛型方法addBox定义了一个参数类型U。通常，java编译器可以在泛型方法调用时推测出类型参数。因此，大多数情况下，在调用泛型方法时，你不需要显示的指定具体的类型。例如，为了调用泛型方法addBox，你可以显示的指定具体类型：

```
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);

```

或者，你也可以忽略具体的类型，java编译器会自动的推测出类型参数为Integer：

```
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);

```


### 类型推测与泛型类和非泛型类构造函数


注意，构造函数可以支持泛型(例如java.util.HashMap.Node)，无论是泛型类或者非泛型类都可以定义泛型构造函数。例如：

```

class MyClass<X> {
  <T> MyClass(T t) {
    // ...
  }
}

```

如果以如下的方式实例化MyClass：

```
new MyClass<Integer>("")

```

上述语句创建了一个参数化类型MyClass<Integer>的实例，显示的指定了类型Integer作为形参的实参。注意MyClass<X>类的构造函数有一个形参T。编译器会推测String是形参T的实参（因为传递给构造函数的实际参数是String对象）。

java7之前的编译器可以推测出泛型构造函数的实参，就像推测泛型方法的实参一样。但是，java se7及以后的编译器可以推测出被实例化的泛型类的实参，如果你使用了'<>'符号的话，这个特性称为diamond，示例：

```
MyClass<Integer> myObject = new MyClass<>("");

```

在这个例子中，编译器会推测泛型类MyClass<X>的形参X的实参是Integer。编译器还可以推测构造函数的形参T的实参是String。


** 需要注意的是，推测算法只应用在推测调用参数、目标对象以及返回值中。推测算法并不使用后面程序的结果。没理解，不好翻译。（ It is important to note that the inference algorithm uses only invocation arguments, target types, and possibly an obvious expected return type to infer types. The inference algorithm does not use results from later in the program.）**


### 目标类型（target types）

java编译器可以利用target type 来推测泛型方法调用的类型参数。一个表达式的target type是java编译器根据这个表达式出现的位置期望的数据类型。考虑如下的方法：Collections.emptyList，它的声明如下：

```
static <T> List<T> emptyList();

```

在考虑如下的赋值语句：

```
List<String> listOne = Collections.emptyList();

```

这个语句期望得到一个List<String>的实例，这个List<String>就称为target type。因为方法emptyList返回类型为List<T>，编译器会推测形参T的实参在这个场景下一定是String，java 7和java 8都可以做出这样的推测。或者，你可以使用你个显示的类型指定T的实际类型，如下：

```
List<String> listOne = Collections.<String>emptyList();

```

但是，这这个上下文中，显示指定类型不是必要的。但是，在某些场景下，确实需要显示指定类型参数：

```
void processStringList(List<String> stringList) {
    // process stringList
}

```

假设你想要调用方法processStringList，传递一个empty list。在java se7中，下面的语句无法通过编译：

```
processStringList(Collections.emptyList());

```

java se 7 编译器会报错：

```
List<Object> cannot be converted to List<String>

```

编译器需要一个类型参数T的值，所以它从Object类开始推测。结果，Collections.emptyList()这个方法调用返回了一个List<Object>类型的值，与processStringList方法的参数是不兼容的。因此，在java7中，你必须显示的指定类型参数：

```
processStringList(Collections.<String>emptyList());

```


但是，java8中可以不必显示指定类型参数了。target type的概念已经被拓展了，它也包括了方法参数，例如上述processStringList方法的参数。在这种情况下，processStringList方法需要一个List<Stirng>类型的参数。Collections.emptyList()方法返回类型为List<T>，因为通过target type，编译器可以推测出形参T的实参类型为String，因为，在java se8中，下面的语句是可以通过编译的：

```
processStringList(Collections.emptyList());

```






























