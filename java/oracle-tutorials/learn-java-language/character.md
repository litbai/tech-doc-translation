## Characters

如果你想使用单独的一个字符，大部分时间，你会使用原始数据类型char，例如：

```
char ch = 'a';
char uniChar = '\u03A9';
cahr[] charArray = {'a', 'b', 'c', 'd', 'e'};

```

但是，在有些场景下，你可能想使用一个对象，例如方法的参数类型是一个对象。java编程语言提供了一个包装了，可以将char包装为一个Character对象。一个Character类型的对象只包含一个域，它的类型为char。Character类同样也提供了一些有用的静态方法来对字符进行操作。

你可以用如下语法创建一个Character对象：

```
Character ch = new Character('a');

```

java编译器在某些特殊情况下，也会为你自动创建一个Character的对象。比如，当你向一个期望参数为对象的方法传入原始类型参数时，java编译器会自动的将char包装为Character对象。这个特性称为autoboxing--或者是unboxing，如果过程是相反的话。

** 注意， Character类是不可变类，因此一旦创建一个Character对象，那么这个对象将不能被改变。 **

下面的表格列出了Character类的一些有用的方法，但是没有列出全部的方法。想要查看完整的方法列表（多于50个），参考java.lang.Character的API。


|Method|Description|
|------|-----------|
|boolean isLetter(char ch)|判断指定的字符是否是字母|
|boolean isDigit(char ch)|判断指定字符是否是数字|
|boolean isWhitespace(char ch)|判断指定字符是否是空格|
|boolean isUpperCase(char ch)|判断指定字符是否是大写字母|
|boolean isLowerCase(char ch)|判断指定字符是否是小写字母|
|char toUpperCase(char ch)|将指定字符转换为大写字母|
|char toLowerCase(char ch)|将指定字符转换为小写字母|
|toString(char ch)|返回单个字符的string|


### 转译序列(Escape Sequences)

一个字符前面加一个'\'符号，称为转译序列，对于编译器来说有特殊的语义。下面的表格列出了转译序列：


|Escape Sequence|Description|
|---------------|-----------|
|\t|insert a tab in the text at this point|
|\b|backspace|
|\n|new line|
|\r|回车|
|\f|换页符|
|\'|单引号|
|\"|双引号|
|\\| \反斜杠|


当在print语句中遇到转译序列时，编译器会相应的来解释这个转译序列。例如，如果你想讲引号放入引号之间，你就必须要使用转译序列\",为了打印这面这个句子：

```
She said "Hello!" to me.

```

你应该使用如下方式：


```
System.out.println("She said \"Hello!\" to me.");

```























