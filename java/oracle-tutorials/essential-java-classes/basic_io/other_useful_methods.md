### Other Useful Methods


本章列出了其他一些，在特定情况下，比较有用的方法。

#### Determining MIMIE Type

为了判断一个文件的MIME，你可能会使用probeContentType方法：

```
try {
	String type = Files.probeContentType(filename);
	if (type == null) {
		Sysyem.err.format("'%s' has an unknown filetype.%n", filename);
	} else if (! "text/plain".equals(type)) {
		 System.err.format("'%s' is not" + " a plain text file.%n", filename);
        continue;
	} catch (IOException x) {
    System.err.println(x);
	}
}

```

需要注意的是，当文件类型不确定时，probeContentType会返回null。


这个方法的实现是平台特定的，而且并不是完全可靠的。文件的类型由平台默认的类型检测器来决定。例如，如果检测器根据文件的后缀.class，判断文件类型是application/x-java，这可能是不可靠的。


如果默认的FileTypeDetector不能满足你的需求，你可以自定义FileTypeDetector。


#### Default File System

为了得到默认的文件系统，可以使用FileSystems.getDefault()方法。通常，可以链式的调用FileSystems的方法，如下所示：

```
PathMatcher matcher = FileSystems.getDefault().getPathMatcher("glob:*.*");

```


#### Path String Sepatator


路径分隔符，在POSIX文件系统中，是正斜杠'/'，在Windows上是反斜杠'\'，其他的文件系统也可能使用其他的分隔符。为了得到默认文件系统的路径分隔符，你可以使用如下的方法：

```
String separator = File.sepatator;
String separator = FileSystem.getDefault().getSepatator();

```


#### File System's File Store


一个文件系统有1个或多个File Store来保存文件和目录。file store代表底层的存储设备。在UNIX操作系统中，每个被挂载的文件系统由一个file store代表。在Windows中，每一个卷对应一个file store，例如C盘，D盘-- C:  D: 等等。 


为了得到文件系统的所有file store，你可以使用getFileStores()方法。这个方法返回一个Iterable，允许你使用加强的for循环来迭代所有的根目录：

for (FileStore store : FileSystems.getDefault().getFileStores()) {
	...
}


如果你想得到一个特定的文件所在的file store，你可以使用Files.getFileStore(Path)方法：


```
Path file = ...;
FileStore store = Files.getFileStore(file);

```


DiskUsages示例就使用了getFileStores方法。

















