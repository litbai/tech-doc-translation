### Legacy File I/O Code

#### Interoperability With Legacy Code

在JDK7发布之前，java.io.File类是文件I/O操作的入口类，但是它又一些缺点。

* 很多方法在失败时没有抛出异常，这时候无法获取有用的错误信息。例如，如果删除一个文件时失败了，程序会收到一个"delete fail"的信息，但是它并不会告诉你删除失败是因为文件不存在，还是因为用户没有权限，或者其他原因。
* rename方法不能跨平台使用
* 没有对符号链接的真正支持
* 对元数据的支持有限，新的API支持文件权限、文件拥有者和其他安全属性
* 访问文件元数据时效率不高
* File的很多方法无法应用到大规模的文件上。如果请求对一个很大的目录进行listFile操作，可能会引起服务器挂起。大型目录还会引起内存资源问题，导致拒绝服务
* 老的API不能写出可靠的方法来遍历文件树，并且当出现符号链接循环引用时也不能恰当额处理


你可能会有一些老的，使用java.io.File的代码，但是你想使用java.nio.file.Path提供的新特性，且不想做很大的改动。

java.io.File类提供了一个toPath方法，它可以将一个老式的File实例，转化为一个java.nio.file.Path实例，如下所示：

```
Path input = file.toPath();

```

接下去你就可以使用Path类提供的丰富特性了。


例如你想删除一个文件，那么老式的代码可能如下：

```
file.delete();

```

使用新API的代码，则改为如下形式：

```
Path p = file.toPath();
Files.delete(p);

```

相反的，Path.toFile()方法，可以将一个Path实例转化为一个File实例。


#### Mapping java.io.File Functionality to java.nio.file


因为文件I/O API在JDK7被完全重新设计了，如果你想使用java.nio.file提供的新特性，你可以使用前面介绍的File.toPath()方法。但是，如果你不想使用这个方法，或者仅仅使用这个方法无法满足你的需求，你必须重写你的代码。


下面的表格列出了新老API对应的功能，但是仅列出了通常的一些功能，你可以参考java.nio.file API获取更多的信息。


|java.io.File Functionality|java.nio.file Functionality|Tutorial Coverage|
|--------------------------|---------------------------|-----------------|
|java.io.File|java.nio.file.Path|The Path Class|
|java.io,RandomAccessFile|The seekableByteChannel|Random Access Files|
|File.canRead, canWrite, canExecute|Files.isReadable, isWritable, isExecutable for UNIX file system|Checking a File or Directory    Managing Metadata|
|File.isDirectory() isFile() length()|Files.isDirectory(Path, LinkOption...)  Files.isRegularFile(Path, LinkOption...)  Files.size(Path)|Managing Metadate|
|File.lastModified()  setLastModified(long)|Files.getLastModifiedTime(Path, LinkOption...)  Files.setLastModifiedTime(Path, FileTime)|Managing Metadata|
|setExecutable, setReadable, setReadOnly setWritable|Files.setAttribute(Path, String, Object, LinkOption...)|Managing Metadata|
|new File(parent, "newfile")|parent.resolve("newfile")|Path Operation|
|File.renameTo|File.move|Moving a File or Directory|
|File.delete|Files.delete|Deleting a File or Directory|
|File.createNewFile|Files.createFile|Creating Files|
|Files.deleteOnExit|使用DELETE_ON_CLOSE选项，在createFile方法中|Creating Files|
|File.createTempFile|Files.createTempFile(Path, String, FileAttributes<?>)|Reading and Writing Files by Using Channel I/O|
|File.exists|Files.exist   Files.notExists|Verfying the Existence of a File|
|File.compareTo equals|Path.compareTo  equals|Comparing Two Paths|
|File.getAbsolutePath getAbsoluteFile|Path.toAbsolutePath|Convertig a Path|
|File.getCanonicalPath getCanonicalFile|Path.toRealPath  normalize|Removing Redundancies From a Path|
|File.toURI|Path.toURI|Converting a Path|
|File.isHidden|Files.isHidden|Retrieving Information About the Path|
|File.list  listFiles|Files.newDirectoryStream|Listing a directory's Contents|
|file.mkdir   mkdirs|Files.createDirectory|Creating a Dierctory|
|File.listRoots|FileSystem.getRootDirectories|Listing a File System's Root Directories|
|File.getTotalSpace   getFreeSpace  getUsableSpace|FileStore.getTotalSpace  getUnallocatedSpace getUsableSpace |File Store Attributes|






