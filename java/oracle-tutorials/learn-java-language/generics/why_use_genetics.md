### 为什么使用泛型

简单来说，使用泛型可以在定义类、接口和方法的时候，用类型（类和接口）作为参数。就像我们熟悉的方法的形参一样，type parameters（类型参数）提供了一种使用同样的代码处理不同输入的方式。不同点在于形参的输入是值，但是类型参数的输入是类型(The difference is that the inputs to formal parameters are values, while the inputs to type parameters are types)。


使用泛型的优势有：

* 编译期强类型检查。java编译器会对泛型代码进行强类型检查，如果代码不是类型安全的，则会报错。修正编译器错误要比修正运行期错误简单的多。
* 杜绝了强制类型转换。考虑如下的代码：

```
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0);

```

如果使用泛型，代码不需要强制类型转换：

```
List<String> list = new ArrayList<String>();
list.add("hello");
String s = list.get(0);   // no cast

```

* 支持程序员实现泛型算法。通过使用泛型，程序员可以实现泛型算法，这个算法可以应用在不同类型的集合上，可以自定义，是类型安全的且易于阅读。































