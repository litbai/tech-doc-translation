## 通配符使用指南

学习泛型编程的过程中非常容易迷惑的一点是决定何时使用有上限的通配符，何时使用有下限的通配符。这一节提供了一些你在编程时可以参考的指南。


为了接下来的讨论，想象一些变量提供的两种功能：

* An 'In' Variable: 一个'in'变量给代码提供数据。想象一个拷贝方法，它有两个参数:copy(src, dest)。src参数提供了被拷贝的数据，因为它属于'in'参数。
* An 'Out' Variable: 一个'out'变量用来存放随处可用的数据。在上述的copy方法中，dest参数接收数据，因此它是'out'参数。


当然，一些变量可能既有'in'功能，又有'out'功能，这份指南中也涉及了这部分知识。


当决定是否使用通配符以及使用哪种通配时，你可以遵循下面的'In' 和 'Out'原则：

* 一个'In'变量被定义为有上限的通配符，使用extends关键字
* 一个'Out'变量被定义为有下限的通配符，使用super关键字
* 当仅使用'In'变量继承自Object类的方法时，使用无限定的通配符
* 当既使用变量的'In'功能，又使用'Out'功能时，不适用通配符。


上面的指南不适用于方法的返回值。应该避免在返回值中使用通配符，因为这样会强制程序员处理这个通配符。

一个定义为List<? extends ...>的变量可以非正式的认为它是read-only的，但是严格意义上并非如此。假设有下面的两个类：


```
class NaturalNumber {

    private int i;

    public NaturalNumber(int i) { this.i = i; }
    // ...
}

class EvenNumber extends NaturalNumber {

    public EvenNumber(int i) { super(i); }
    // ...
}

```


考虑如下代码：

```
List<EvenNumber> le = new ArrayList<>();
List<? extends NaturalNumber> ln = le;
ln.add(new NaturalNumber(35));  // compile-time error


```

在上述代码中，因为List<EvenNumber>是List<? extends NaturalNumber>的子类型，你可以将le赋值给ln。但是你不能使用ln添加一个元素。下面的操作是允许的：

* 你可以添加一个null
* 你可以调用clear
* 你可以得到一个iterator并且调用remove方法
* 你可以捕获通配符，并且改变你从list中读取的元素

你可以看到通过'List<? extends NaturalNumber>'定义的List严格上来说并不是只读的，但是你可以认为它是只读的，因为你不能存储一个新元素(null除外)或者**改变已经存在于list中的元素??(我理解为不能set某一个index的值)**(because you cannot store a new element or change an existing element in the list)。




















































