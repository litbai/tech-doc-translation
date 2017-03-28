## Unchecked Exceptions -- The Controversy

因为java编程语言不需要方法来catch或者声明Unchecked Exceptions(RuntimeException、 Error以及它们的子类)，所以有些开发者就只编写抛出Unchecked Exceptions的代码或者让他们自己编写的异常类都继承自RuntimeException。 这两种方式都可以顺利通过编译，并且不用在代码中使用catch来捕获并处理异常。尽管这看起来减轻了开发者的工作量，使得代码变得简单，但是，它绕开了'catch or specity'原则的初衷，可能会给用户带来问题。


为什么设计者决定强制一个方法声明它可能会抛出的异常？一个方法可能抛出的异常声明是方法签名的一部分。那些调用这个方法的用户必须知道一个方法可能会抛出何种异常，这样他们才可以决定怎么应对这些异常情况。这些异常抛出声明就跟返回值声明一样，是方法签名的一部分，是公开的接口。


下一个问题可能是：如果方法声明中包含异常声明，并且作为方法API的一部分是很有用途的，那么为什么runtime Exceptions不需要声明？运行时异常往往代表了程序有bug，并且客户端代码不能合理的从这个异常中恢复或者处理这个异常。这些异常包括：算术错误，比如除以0；指针错误，比如访问一个null对象的成员；索引错误，比如试图访问的索引超出了list的长度。


运行时异常可能发生在任意的地方。如果每一个方法调用都需要声明会抛出RuntimeException，那么代码就会变得很繁杂，降低了阅读性。因此，编译器不强制要求方法声明抛出RuntimeException类型的异常，也不要求方法捕获RuntimeException类型的异常。

抛出运行时异常的一个典型例子是用户错误的进行了方法调用。例如，一个方法可以检查它的参数是否是null。如果一个参数是null，这个方法可能会抛出一个NullPointerException，这个异常就是一个Unchecked Exception。


通常来说，不抛出一个RuntimeException异常，或者创建RuntimeException类的子类，仅仅是因为你可以避免在方法声明中带throws子句。


下面是异常使用的原则：如果一个客户端可以合理的恢复一个异常，那么将这个异常定义为checked异常。如果客户端不能恢复异常，则将这个异常定义为Unchecked异常。

















