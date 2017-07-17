### Path Operations

Path类包含很多方法用来获取path的信息、访问path的元素、将path转化为其他形式或者提取部分path。同样，还有很多方法用来匹配路径、去除路径中的冗余。本节将会涉及这些方法，这些方法有时称为语义操作，因为操作Path本身，并不会影响真实的文件或者文件系统。

#### Creating a Path

一个Path的实例包含用来定位一个文件或目录的所有信息。一旦我们定义了一个Path实例，它就被提供了一个或多个name。Path可能会包含一个root节点或者一个文件名称，但是这是非必须的。一个Path也可能仅仅内包含一个单一的目录或者文件名。


你可以使用如下的Paths.get()方法来轻松的创建一个Path实例：

```
Path p1 = Paths.get("/tmp/foo");
Path p2 = Paths.get(args[0]);
Path p3 = Paths.get(URI.create("file:///Users/joe/FileTest.java"));

```


Paths.get()方法是如下代码的缩写：

```
Path p4 = FileSystems.getDefault().getPath("/users/sally");

```

下面的代码创建了 /u/joe/logs/foo.log， 假设你的家目录是： /u/joe/的话：

```
Path p5 = Paths.get(System.getProperty("user.home"), "logs", "foo.log");

```

#### Retrieving Information about a Path


你可以认为Path使用一个序列来存储这些name。目录结构中最高的元素，其index是0，最低的元素索引为[n-1]， n为Path种包含的name元素的个数。Path类提供了方法来检索Path中一个单独的元素或者子序列，通过使用这些index。


![](io-dirStructure.gif)


下面的代码片段定义了一个Path实例，然后调用了一些方法来获取这个实例的信息：


```
Path path = Paths.get("/home/joe/foo");

System.out.format("toString: %s%n", path.toString());
System.out.format("getFileName: %s%n", path.getFileName());
System.out.format("getName(0): %s%n", path.getName(0));
System.out.format("getNameCount: %d%n", path.getNameCount());
System.out.format("subpath(0,2): %s%n", path.subpath(0,2));
System.out.format("getParent(): %s%n", path.getParent());
System.out.format("getRoot: %s%n", path.getRoot());

```


下面的表格列出了上述程序的输出结果：


|method invoked|solaris OS|Windows|comment|
|--------------|-------------|-------------|-----------------|
|toString|/home/joe/foo|C:\home\joe\foo|返回代表这个路径的字符串。如果你使用Filesystems.getDefault().getPath()或者是Paths.get()方法来创建Path对象，那么创建时会做一些语法上的容错机制。例如：在unix os中，这两个方法会将下面的路径： //home/joe/foo  纠正为： /home/joe/foo;|
|getFileName|foo|foo|返回文件名，或者返回路径中最后一个元素|
|getName(0)|home|home|返回指定索引的元素，0索引指的是离root最近的元素|
|getNameCount()|3|3|返回路径中的element个数|
|subpath(0,2)|home/joe|home\joe|返回子路径（不包含root，exclude endindex）|
|getParent()|/home/joe|\home\joe|返回父路径|
|getRoot|/|C:\\|返回root元素|


上述程序使用的是绝对路径，下面将使用相对路径来查看一下各个方法的返回值：

```
Path path = Paths.get("sally/bar"); // solaris

Path path = Paths.get("sally\bar"); // windows

```


|method invoke| solaris os| windows os|
|----|----|----|
|toString()|sally/bar|sally\bar|
|getFileName|bar|bar|
|getName(0)|sally|sally|
|getNameCount()|2|2|
|subpath(0,1)|sally|sally|
|getParent()|sally|sally|
|getRoot|**null**|**null**|



#### Removing Redundancies From a Path


许多文件系统使用"."来表示当前目录，使用".."表示parent目录。你可能会遇到一个路径包含冗余信息的情况。一种可能的情况为：服务器配置了它的日志存放在"/dir/logs/."目录，这里，"/."就是冗余的信息，因为只使用"/dir/logs"就能正确的表示路径，这时你可能想要去掉这个信息。

下面的两个路径同样存在冗余信息：

```
/home/./joe/foo
/home/sally/../joe/foo

```


normalize()方法可以去除这些冗余信息，类似于"."或者"/.."这种。上面的两个路径都将被标准化为 /home/joe/foo.

有一点需要额外的注意，normalized()方法只做语义上的标准化操作，它不会去检查真实的文件是否存在。例如上述的第二个例子，如果sally是一个符号链接，比如指向/usr/demo, 那么，去掉sally之后，这个路径指向的实际文件就变掉了，由/home/usr/joe/foo 变成了 /home/joe/foo，有可能/home/joe/foo根本不存在。


为了保证在标准化时，还能定位到正确的文件，你可以使用toRealPath()方法，下面将介绍这个方法。

#### Converting a Path

你可以使用3种方法来转化路径。如果你想将path转化为浏览器可以打开的方式，你可以使用toUri方法，例如：

```
Path p1 = Paths.get("/home/logfile");
System.out.format("%s%n", pa.toUri()); // file:///home/logfile

```


toAbsolutePath()方法将一个path转化为绝对路径。如果path本身就已经是绝对路径，那么它会直接返回。当处理用户输入的文件名时，toAbsolutePath方法将会很有用处，例如：

```
public class FileTest {
	public static void main(String[] args) {
		if (args.length < 1) {
			System.out.println("usage: FileTest file");
			System.exit(-1);
		}
		
		Path inputPath = Paths.get(args[0]);
		// 讲一个路径转化为绝对路径。通常，这意味着将当前目录的路径添加到要转换的路径的前面。
		Path fullPath = inputPath.toAbsolutePath();
	}
}

```

toAbsolutePath方法将用户输入转换成一个绝对路径，调用这个绝对路径上的一些方法，将会返回期望的值。path路径指向的文件可能在物理上不存在。

toRealPath方法返回一个存在的文件的真实路径，调用方法时，可以传入一些可选的参数：

* 如果文件系统支持符号链接，并且传入的参数指定了支持符号链接，那么它将会解析符号链接（目前在JDK8，貌似还不支持符号链接）。
* 如果Path是一个相对路径，它返回绝对路径
* 如果Path包含冗余信息，它会去除这些冗余信息


如果path代表的文件不存在或者无法访问，那么toRealPath方法会抛出异常，你可以捕获异常，进行一些操作。例如：

```
try {
	Path fp = path.toRealPath();
} catch (NoSuchFileException x) {
	System.err.format("%s: no such " + file or directory%n", path);
} catch (IOException x) {
	System.err.format("%s%n", x);
}

```


### Joining Two Paths


你可以使用resolve方法来结合两个路径。你可以传入一个部分路径，它不包含root节点，然后你可以将这个部分路径append到另一个路径上。

例如，考虑下面的代码片段：

```
// solaris --> /home/joe/foo/bar
Path p1 = Paths.get("/home/joe/foo");
System.out.format("%s%n", p1.resolve("bar"));

// windows  --> C:\home\joe\foo\bar

Path p1 = Paths.get("C:\\home\\joe\\foo");
System.out.format("%s%n", p1.resolve("bar"));

// 如果传入的path参数是一个绝对路径，那么原样返回 --> /home/joe
Paths.get("foo").resolve("/home/joe");  

```


#### Creating a Path Between Two Paths

在编写file I/O代码时，常见的一个需求是在两个Path之间建立一个Path。你可以使用relativize方法来完成这个需求。这个方法在被调用的对象和方法参数之间建立一个path，这个新的path是原始path的相对路径。


例如，考虑如下的两个相对路径：

```
Path p1 = Paths.get("joe");
Path p2 = Paths.get("sally");

```

如果没有其他信息的话，默认joe和sally在同一目录下，是兄弟节点。如果从joe导航到sally，需要先回到parent目录，然后再导航到sally：

```

Path p1_to_p2 = p1.relativize(p2); // ../sally
Path p2_to_p1 = p2.relativize(p1); // ../joe

```


考虑如下更复杂的示例：

```
Path p1 = Paths.get("/home");
Path p3 = Paths.get("/home/sally/bar");
Path p1_to_p3 = p1.relativize(p3); // sall/bar

Path p3_to_p1 = p3.relativize(p1); // ../..

```

在上述示例中，p1和p3有相同的父节点，home。为了从home导航到bar，你首先需要走到下一级目录，sally, 然后再往下一级，到达bar。而从bar到home需要连升两级。


如果两个path中只有一个包含root节点，那么两个path之间的相对路径将无法建立。如果两个path都有root节点，那么相对路径能否建立取决于系统支持不支持。

recursive Copy程序中使用了relativize和resolve方法。


#### Comparing Two Paths

Path类支持equals方法，你可以利用此方法来判断两个路径是否相等。startsWith和endsWith方法可以判断一个路径是否以一个特定的字符串开头或结尾。这些方法很好用，示例：


```
Path path = "/Users/chengyu/javaPrj";

Path otherPath = "/Users/chengyu/javaPrj/xxx.txt";

Path beginning = Paths.get("/Users");
Path ending = Paths.get("javaPrj");

if (path.equsls(otherPath)) {
	// ...
} else if (path.startsWith(beginning)) {
	// ...
} else if (path.endingWith(ending)) {
	// ...
}

```


Path类实现了Iterable接口。iterator方法返回一个对象，然后你可以迭代这个对象，访问它的所有元素。第一个元素是最接近root节点的元素。


```
Path path = ...;
for (Path name : path) {
	System.out.println(name);
}

```


Path类也实现了Comparable接口，你可以使用compareTo方法比较两个Path对象，如果想排序，也可以方便的实现。


你还可以将Path对象放到Collection中。

当你需要确认两个Path对象是否定位同一个文件时，你可以使用Files类的isSameFile方法。

```
Path p1 = ...;
Path p2 = ...;

if (Files.isSameFile(p1, p2)) {
    // Logic when the paths locate the same file
}

```
































