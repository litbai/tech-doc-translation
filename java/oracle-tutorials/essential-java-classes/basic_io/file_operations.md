### File Operations

Files类是java.nio.file包的另一个重要的入口类。这个类提供了很多有用的static方法，来读、写、操作文件个目录。Files的方法的参数是Path对象，在继续下面的教程之前，你应该熟悉一下下面的几个概念：

* 释放系统资源
* 捕获异常
* 可变参数
* 原子操作
* 方法链
* Glob
* Link Awareness


#### Releasing System Resource

文件操作API中使用的很多resource，比如 streams或者channels，都实现或者继承了java.io.Closable接口。对于Closable资源，当你使用完毕的时候，需要close。忘记关闭资源会影响程序的性能。JDK7新推出的try-with-resource，可以帮你自动关闭资源。


#### Catching Exceptions


当操作文件I/O时，很容易发生无法预料的异常：比如文件不存在、程序没有权限访问文件系统、默认的文件系统实现不支持某个特定的功能等等，可能会遇见各种各样的问题。


所有会访问文件系统的方法都会抛出IOException。在try-with-resource语句中捕获这些异常是很好的实践。try-with-resource提供的最方便的功能是，编译器可能自动帮你生成关闭资源的代码，参看如下代码：

```
Charset charset = Charset.forName("US-ASCII");

String s = "This is a demo";
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
	writer.write(s, 0, s.length());
} catch (IOException e) {
	System.err.format("IOException: %s%n", e);
}

```


或者，你也可以在try语句中处理各种I/O操作，然后捕获各种异常来做相应的处理工作。如果你的代码打开了一个stream或者channel，你应该在finally语句中关闭它们。上面使用try-with-resource的代码，可以转化为如下的代码：

```
Charset charset = Charset.forName("US-ASCII");

String s = "...";

BufferedWriter writer = null;

try {
	writer = Files.newBufferedWriter(file, charset);
	writer.write(s, 0, s.length());
} catch (IOException e) {
	System.err.format("IOException: %s%n", e);
} finally {
	if (writer != null) {
		writer.close();
	}
}

```

除了IOException，还有许多特定的异常继承自FileSystemException。FileSystemException有很多有用的方法： getFile()方法可以返回涉及到的file、getMessage()方法可以返回具体信息，getReason()方法可以返回为什么文件操作失败、getOtherFile()方法可以返回涉及到的其他文件。


下面的代码展示了如何使用getFile方法：


```
try () {
	...
} catch (NoSuchFileException x) {
	System.err.format("%s dose not exit%n", x.getFile());
}

```

为了简洁明了，本教程的很多示例都没有具体的处理异常，但是你的程序应该总是异常处理的代码。


#### Varargs

一些Files的方法可以接收任意数量的参数。例如，下面的方法签名中，CopyOption后面的省略号表示这个方法可以接收多个参数，称为可变参数：

```
Path Files.move(Path, Path, CopyOption...)

```


当一个方法接收一个可变参数时，你可以传入一个逗号分隔的参数列表或者一个数组。

```
import static java.nio.file.StandatdCopyOption.*;


Path source = ...;
Path target = ...;

Files.move(source, target, REPLACE_EXISTING, ATOMIC_MOVE);

```


#### Atomic Operations


Files中的一些方法，例如 move， 在一些文件系统中，可以一次性执行多个操作，这些操作是原子的。

一个原子的文件操作指的是一个操作，这个操作不能被中断，或者不能被部分执行部分不执行。这个操作要么全都执行，要么失败，都不执行。当有多个进程同时操作文件系统的同一区域时，这是很重要的，你需要保证每个进程独占这个区域。


#### Method Chaining


许多file I/O方法支持方法链。

你首先调用一个方法，得到一个返回值。然后在返回值上，你又可以调用另外一个方法，得到另一个返回值，and so on，例如：

```
String value = Charset.defaultCharset().decode(buf).toString();

UserPrincipal group = file.getFileSystem().getUserPrincipalLookupService().lookupPrincipalByName("me");

```

方法链可以创建紧凑的代码，避免声明只使用一次的临时变量。


#### What Is a Glob?

Files类中有两个方法接收名称为glob的参数，那么什么是Glob？

你可以使用glob语法来指定模式匹配的行为，glob是Unix中用来进行模式匹配的。


一个glob模式由一个字符串指定，用来匹配其他字符串，例如目录或者文件名。Glob语法遵循下面几个简单的规则：


* 一个星号，*， 匹配任意数量的字符（包括0个字符）
* 两个星号，**，类似于一个星号，但是跨越了目录的限定，常用语匹配完整的路径
* 一个问号，？，匹配一个字符
* 大括号，{},指定了一些子模式，例如
	* {sum, moon, stars} 可以匹配 "sum", "moon", "stars"
	* {temp\*, tmp\*} 可以匹配任意以temp和tmp开头的字符串
* 方括号，[]，可以匹配一系列的单个字符或者当有'-'字符时，可以匹配一个字符范围，例如：
	* [aeiou]可以匹配a e i o u
	* [0-9]可以匹配0-9的数字
	* [A-Z]可以匹配大写字母
	* [a-z, A-Z]可以匹配大小写字母
在方括号中，*，？和\代表自身，其特殊含义失效
* 所有其他字符匹配自身
* 为了匹配*, ? 或者其他具有特殊含义的字符，你可以使用转译字符'\'对它们进行转译。例如，'\\'匹配一个'\'， '\?'匹配'?'。



下面有一些glob语法的例子：

* \*.html 匹配所有以html结尾的文件
* ??? 匹配有3个字符的字符串
* *[0-9]* 匹配包含数字的字符串
* \*.{htm, html, pdf} 匹配任意以.htm, .html, .pdf结尾的文件
* a?*.java 匹配以a开头，后面跟上至少一个字符，以.java结尾的文件。
* {foo*, *[0-9]*} 匹配任意以foo开头，或者包含数字的字符串。


** 注意，如果你在键盘输入glob模式，并且它包含至少一个特殊字符，你必须将这个特殊字符放入引号、使用转译字符或者使用命令行中支持的任意转译机制**



glob语法功能强大，易于使用。但是，如果它满足不了你的要求，你也可以使用正则表达式。想了解更多关于glob语法的信息，可以查看FileSystem类的getPathMatcher方法的API： https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getPathMatcher-java.lang.String-


#### Link Awareness

Files类是一个"link aware". Files类的每一个方法，或者会检测如果文件是一个符号链接怎么办，或者会提供一个选项，当遇到符号链接时，允许你自己配置该怎么做。


































