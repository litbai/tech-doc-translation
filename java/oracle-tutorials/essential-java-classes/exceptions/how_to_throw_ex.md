## How to Throw Exceptions

其他地方必须先抛出一个异常，你才可以捕获异常。任何代码都可以抛出一个异常：你自己的代码、别人写的代码(比如，jdk中的代码)或者java运行时环境都可以抛出异常。不管在哪里抛出异常，都必须使用throw关键字。


你可能注意到了，java平台提供了很多异常类。所有的类都是Throwable的子类，程序可以通过类型来区分各种异常。


你也可以创建自己的异常类，代表你的类可能发生的错误。实际上，如果你是一个package developer，你可能必须创建自己的异常集，让其他使用你提供的package的开发者区分你的异常和其他包的异常或者java平台中定义的异常。


你也可以创建异常链，下面的小节会详细介绍。


### throw语句


所有的方法必须使用throw子句来抛出一个异常。throw语句需要一个简单的参数：一个Throwable类型的对象。Throwable对象是任意的Throwable类及其子类的一个实例，如下所示：

```
throw someThrowableObject;

```

让我们举个实际的例子。下面的pop方法，会移除stack头部的元素，并且返回这个元素：

```
public Object pop() {
    Object obj;

    if (size == 0) {
        throw new EmptyStackException();
    }

    obj = objectAt(size - 1);
    setObjectAt(size - 1, null);
    size--;
    return obj;
}

```


pop方法首先检查stack中是否有元素。如果stack是空的，那么pop方法将创建一个EmptyStackException类型的对象并抛出这个异常对象。

注意到，pop方法声明中并不包含throw子句。因为EmptyStackException是unchecked异常，所以，pop方法可以不使用throw子句进行声明。


### Throwable类和它的子类

下面的图是Throwable类和比较重要的一些子类的继承结构图。你可以看到，Throwable类有两个直接子类：Error和Exception。

![](exceptions-throwable.gif)


### Error 类

当发生动态链接错误或其他java虚拟机错误的时候，java虚拟机会抛出一个Error。普通的程序通常不会捕获也不会抛出Errors。


### Exception 类


绝大部分的程序会抛出或者捕获Exception类和其子类。一个Exception表示程序出现了一个问题，但是并非严重的系统问题。你编写的绝大部分程序会抛出和捕获Exceptions而不是Errors。


java平台定义了很多Exception类的子类。这些子类表示了可能会发生的错误类型。例如，IllegalAccessException暗示了一个特殊的方法不能被访问，而NegativeArraySizeException异常暗示一个程序试图创建一个size为负数的数组。


Exception的一个子类，RuntimeException，暗示错误的使用API。runtime异常的一个常见例子是NullPointerException，当一个方法试图访问一个null对象的成员时，会抛出此异常。后面的章节' Unchecked Exceptions — The Controversy '讨论了为什么大多数应用不应该抛出RuntimeException或者RuntimeException的子类类型的异常。























































