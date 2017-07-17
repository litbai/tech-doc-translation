### Checking a File or Directory

你有一个Path的实例来代表一个文件或者目录，但是这个文件或者目录是否真实存在？它是否可读？可写？可执行？


#### Verifying the Existence of a File or Directory

Path类中的方法是语义上的，意味着它仅仅对Path实例进行操作，不会影响物理上的文件。但是，最终，你一定会访问文件系统来确定Path代表的文件或目录是否存在。你可以使用exists()或者notExists()方法。这里，需要注意的是， !Files.exist(path) 和 Files.notExist(path)不是等价的，当你测试一个文件是否物理存在时，返回结果可能如下：

* 文件存在, 返回true
* 文件不存在， 返回false
* 文件状态未知，返回false。当程序无权访问文件时，会返回这个结果

如果exists和notExist都返回false，那么这个文件是否存在，无法确定。


#### Checking File Accessibility


为了确认程序可以访问一个文件，你可以使用isReadable(path), isWritable(path), isExecutable(path)来判断。

下面的程序片段验证了一个文件是否存在、是否可执行：


```
Path file = ...'

boolean isRegularExecutableFile = Files.isRegularFile(file) & Files.isReadable(file) & Files.isExecutable(file);

```

** 注意，一旦方法执行完毕，并不保证接下来程序一定可以访问文件。一个常见的安全隐患是，程序首先检查可访问性，然后访问文件。在检查和访问期间，文件的权限可能发生改变。更多信息，请自行google TOCTTOU。**


#### Checking Weather Two Paths Locate the Same File

当你有一个文件系统，它支持符号链接时，则有可能发生两个不同的Path指向同一个文件的可能性。isSameFile(path, path)可以比较两个路径，判断它们是否定位同一个文件，例如：

```
Path p1 = ...;
Path p2 = ...;

if (Files.isSameFile(p1, p2)) {
	// ...
}

```


























