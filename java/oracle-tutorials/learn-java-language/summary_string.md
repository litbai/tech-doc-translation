### String和Character的总结

大部分时间，如果你需要一个单独的字符，你将会使用基本数据类型char。但是有时你需要一个字符对象，比如传递一个对象参数给方法，这时你需要Character对象。一个Character的对象，只包含一个域，它的类型是char。Character类同事提供了一些有用的方法，用来操作字符。


String是一个字符序列，java编程语言中大量使用了String。在java编程语言中，String是一个对象，String类有多于60个的方法和13个构造器。


大部分时间，你会这样创建一个字符串,而不是使用String的构造方法： 

```
String s = "Hello world!";

```

String类包含很多检索子串的方法，然后你可以通过'+'操作符，很容易的拼接新字符串。

String类同时包含一个功能方法，例如split(), toLowerCase(),valueOf()。在将用户输入的字符串转变为number时，这些方法将十分有用。Number类同样也有将字符串转变为数字或者相反操作的功能方法。

除了String类，JDK中还包括一个StringBuilder类。使用StringBuilder类操作字符串有时会得到更好的性能。StringBuilder类提供了一些有用的操作字符串的方法，比如reverse方法。

一个String可以通过StringBuilder的构造方法转换为StringBuilder。一个StringBuilder可以通过toString方法，转换为String。

























