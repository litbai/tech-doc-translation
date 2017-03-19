## 泛型、继承和子类型

我们已经知道，将一种类型的对象赋予指向另一种对象类型的变量是可以的，只要这两种类型是兼容的。例如，你可以将Integer对象赋予Object变量，因为Object是Integer的超类：

```
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK

```

在面向对象领域中，这成为‘is a’的关系，即Integer is a Object。因为Integer是一种Object，所以这种操作是允许的。另外，Integer也是一种Number，所以下面的语句也是合法的：

```
public void someMethod(Number n) { /* ... */ }

someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK

```


对于泛型来说，也是一样的道理。你可以声明一个泛型类型，将Number类作为类型参数，然后，你可以调用add方法，传入任何Number类的子类：

```
class Box<T> {
	private List<T> boxes = new ArrayList<>();
	public void add(T t) {
		boxes.add(t);
	}
}

Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK

```

现在，考虑一下下面的方法：

```
public void boxTest(Box<Number> n) { /* ... */ }

```


这个方法可以接受什么类型的参数，通过查看方法的签名，你可以看到它只接受一个参数，这个参数的类型是Box<Number>。这意味着什么？你可以传递Box<Integer>或者Box<Double>类型的参数给这个方法么？答案是不可以，因为Box<Integer>和Box<Double>不是Box<Number>的子类。

当涉及泛型时，这一点会存在很强的误导作用，认清这一点是十分重要的。Box<Integer>并不是Box<Number>的子类，尽管Integer是Number的子类。

![](generics-subtypeRelationship.gif)


** 给定两个类型A和B（例如Integer和Number），MyClass\<A\>和MyClass\<B\>并没有任何关系，无论A和B是否有关系。MyClass\<A\>和MyClass\<B\>唯一的关系是它们的父类都是Object。 **


### 泛型类和子类型

你可以通过extends和implements关键字来创建一个泛型类的子类。一个类或者接口的类型参数和另一个类或接口的类型参数之间的关系是由extends和implements关键字决定的。

以集合类为例，ArrayList<E>实现了List<E>， 并且，List<E>继承了Collection<E>。因为，ArrayList<String> 是 List<String>的子类，List<String>是Collection<String>的子类。只要保持类型参数一致(在上例中是String)，泛型类之间就会保持这种关系。

![](generics-sampleHierarchy.gif)


现在我们来定义一个自己的list接口，PayloadList，对于每一个元素e，可以接收一个可选的泛型类型P，声明如下：


```
interface PayloadList<E,P> extends List<E> {
  void setPayload(int index, P val);
  ...
}

```

下面几种参数化的payLoadList都是List<String>的子类型：

```
PayloadList<String,String>
PayloadList<String,Integer>
PayloadList<String,Exception>

```


![](generics-payloadListHierarchy.gif)

























