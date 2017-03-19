## Wildcard capture and helper methods

在一些情况下，java编译器可以推测出通配符的真实类型。例如，一个list可以定义为List<?>，但是，当计算一个表达式时，编译器会根据上下文来推测一个特定的类型。这个过程称为wildcard capture。

大部分情况下，你无须耽误通配符匹配问题，除非你收到一条错误信息，这条信息包含'capture of'字段。

下面的WildcardError示例，在编译时，它会产生一条capture error：

```
public class WildcardError {
	void foo(List<?> i) {
		i.set(0, i.get(0));
	}
}

```

在上述示例中，编译器会推测List的实参为Object。当foo方法调用List.set(int, E)时，编译器不能确定插入的对象的类型，所以产生了一个错误。当这种类型的错误发生时，编译器会认为你正在给变量赋错误的类型值。这也是java加入泛型机制的原因，可以保证在编译器是类型安全的，从而避免直到运行期才出错。


当使用jdk 7的javac命令编译上述的代码时，会产生如下的错误：

```
WildcardError.java:6: error: method set in interface List<E> cannot be applied to given types;
    i.set(0, i.get(0));
     ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
1 error

```

在这个例子中，编译器试图做一个安全的操作，那么怎样才能绕过这个编译器错误？你可以通过编写一个私有的辅助方法修复这个问题，这个辅助方法可以用来capture the wildcard. 比如创建一个辅助函数fooHelper，如下所示：

```
public class WildcardFixed {
	void foo(List<?> i) {
		fooHelper(i);
	}
	
	// helper method created so that the wildcard can be caprure through type inference
	
	private <T> void fooHelper(List<T> l) {
		l.set(0, l.get(0));
	}
}

```

通过辅助函数，编译器可以推测出T is  CAP#1, 在方法调用中被匹配的对象。这样，就可以通过编译了。

根据惯例，赋值函数 通常命名为 originalMethodNameHelper，比如上例中方法名为foo，则辅助方法命名为fooHelper。


现在，考虑一个更复杂的例子，WildcardErrorBad：

```

public class WildcardErrorBad {
	void swapFirst(List<? extends Number> l1, List<? extends Number l2>) {
		Number temp = l1.get(0);
		l1.set(0, l2.get(0)); // expected a CAP#1 extends Number, but got a CAP#2 extends Number, same bound, but different types
		l2.set(0, temp);  // expected a CAP#1 extends Number, got a Number
	}
}

```

在上例中，代码中试图执行一个类型安全的操作。例如，考虑如下的方法调用：


```
List<Integer> li = Arrays.asList(1, 2, 3);
List<Double>  ld = Arrays.asList(10.10, 20.20, 30.30);
swapFirst(li, ld);

```

虽然List<Integer>和List<Double>都满足条件List<? extends Number>, 但是，很明显，我们不能将一个从List<Integer>列表中拿出的Integer对象放到List<Double>中。


如果使用Oracle JDK的javac命令来编译上述代码，会得到如下的错误信息：

```
WildcardErrorBad.java:7: error: method set in interface List<E> cannot be applied to given types;
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:10: error: method set in interface List<E> cannot be applied to given types;
      l2.set(0, temp);      // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:15: error: method set in interface List<E> cannot be applied to given types;
        i.set(0, i.get(0));
         ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
3 errors

```


在这种情况下，没有辅助方法可以绕过编译器错误，因为这段代码本身就是错误的。





















































