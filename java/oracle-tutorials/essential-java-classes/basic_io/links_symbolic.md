### Links, Symbolic or Otherwise


前几节已经介绍过java.nio.file包，尤其重点介绍了Path类，它是"link aware"的。Path类的每一个方法，在遇到符号链接时，要么自行决定默认应该怎么做，要么提供了可配置项，允许你来指定怎么做。


到目前为止，我们讨论的都是符号链接，或称为软连接，但是一些文件系统同样支持硬链接。硬链接比符号链接有更严格的限制，如下：

* 硬链接的target必须存在
* 通常不允许在目录上建立硬链接
* 硬链接不允许跨分区或volume，因此，它不能跨文件系统存在
* 一个硬链接看起来和表现起来都跟一个普通文件一样，你很难区分
* 一个硬链接跟它的target有相同的属性，它们有相同的文件权限、时间戳等等，所有的属性都是一样的。


因为以上的限制，硬链接通常不如软链接应用的广泛，但是Path的方法可以无缝的处理硬链接。


#### Creating a Symbolic Link


如果你的文件系统支持的话，你可以使用createSymbolicLink(Path, Path, FileAttriute<?> ...)来创建一个符号链接。第二个Path类型的参数代表目标文件或者目录，它可能不存在。下面的代码创建了一个符号链接，使用了默认的权限：

```
Path newLink = ...;
Path target = ...;

try {
	Files.createSymbolicLink(newLink, target);
} catch (IOException x) {
	System.err.println(x);
} catch (UnsupportedOperationException x) {
	System.err.println(x);
}


```


#### Creating a Hard Link


你可以使用createLink(Path, Path)方法，来创建一个硬链接。第二个Path类型的参数指定了target，它对应的文件必须存在，否则会抛出NoSuchFileException。示例代码：


```
Path newLink = ...;
Path existingFile = ...;

try {
	Files.createLink(newLink, existingFile);
} catch (IOException x) {
	System.err.println(x);
} catch (UnsupportedOperationException x) {
	System.err.println(x);
}

```


#### Detecting a Symbolic Link

如果想看一个Path实例是否代表一个符号链接，你可以使用isSymbolicLink(Path)方法：

```
Path file = ...;

boolean isSymbolicLink = Files.isSymbolicLink(file);

```


#### Finding the Target of a Link


你可以使用readSymbolicLink(Path)方法来得到一个符号链接的target：


```
Path link = ...;
try {
	System.out.format("Target of Link '%s' is '%s'%n", link, Files.readSymbolicLink(link));
} catch (IOException x) {
	System.err.println(x);
}

```


如果传入的link不是一个符号链接，方法将会抛出NotLinkException。




































