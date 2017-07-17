### Creating and Reading Directories

先前讨论的一些方法，例如delete方法，可以用于普通文件、符号链接和目录。但是，如果列出文件系统最高层的所有目录？如何列出目录的内容？如何创建目录？


#### Listing a File System's Root Directories


你可以使用FileSystem.getRootDirectories方法来列出所有的根目录。这个方法返回一个Iterable对象，你可以使用for-each来遍历它。

下面的代码片段打印出了文件系统的根目录： 在Unix下，只是一个简单的'/'

```
Iterable<Path> dirs = FileSystems.getDefault().getRootDirectories();

for (Path name : dirs) {
	System.out.println(name);
}

```

#### Creating a Directory

你可以使用createDirectory(Path, FileAttribute<?>) 方法来创建一个目录。如果你不指定任何FileAttribute的话，新创建的目录将会有默认的权限，例如：

```
Path dir = ...;
Files.createDirectory(dir);

```

下面的代码片段在POSIX系统上创建了一个新的目录，并指定了权限：

```
Set<PosixFilePermission> perms = PosixFilePermissions.fromString("rwxr-x---");

FileAttribute<Set<PosixFilePermission>> attr = PosixFilePermissions.asFileAttribute(perms);

Files.createDirectory(file, attr);

```

如果想创建一系列目录，你可以使用createDirectories(Path, FileAttribute<?>)方法，它会一次性创建所有的目录，如果中间有目录不存在，它会先创建中间目录，类似于mkdir -p命令。就像createDirectory(Path, FileAttribute<?>) 一样，你也可以指定可选的文件属性。下面的示例使用了默认的属性：

```
Files.createDirectories(Paths.get("foo/bar/test"));

```

上述语句可能创建多个目录，从高到低。如果foo目录不存在，那么会首先创建foo目录，然后创建bar目录，最后创建了test目录。

注意，这个方法可能存在创建了一些目录后失败的情况，那么此时就出现创建了部分目录，而失败了部分目录的情况。、


#### Creating a Temporary Directory

你可以创建一个临时目录，通过使用如下的createTempDirectory方法之一：


* createTempDirectory(Path, String, FileAttribute<?>...)
* createTempDirectory(String, FileAttribute<?>...)


第一个方法允许你指定临时目录创建的位置，第二个方法会在默认的临时文件存放目录中创建临时目录,系统临时文件存放目录由系统属性指定： java.io.tmpdir=/var/folders/2m/03l09hf57z7drf0ds77v4...


#### Listing a Directory's Contents

你可以使用newDirectoryStream(Path)方法来列出一个目录中的所有内容。这个方法会返回一个实现了DirectoryStream接口的对象。实现了DirectoryStream接口的类同时也实现了Iterable接口，所以你可以迭代一个directory stream， 读取它的所有对象。即使对于很大的目录，这个方法也可以很好的工作。

** 注意：返回的DirectoryStream是一个stream。如果你没有使用try-with-resource语句，你需要在finally块中手动关闭这个stream **

下面的代码片段用来打印一个目录的内容：

```
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) {
	for (Path file : stream) {
		System.out.println(file.getFileName);
	}
} catch (IOException | DirectoryIteratorException e) {
	System.err.println(x);
}

```


这个方法返回一个目录的所有内容：文件、符号链接和子目录以及隐藏文件，如果你想指定返回的内容，你可以实现下面将描述的newDirectoryStream()方法之一。

如果迭代过程中发生了异常，将会抛出一个DirectoryIteratorException，并且以IOException为cause。


#### Filtering a Directory Listing By Using Globbing


如果你只想列出满足一定条件的内容，你可以使用newDirectoryStream(Path, String)方法，提供了一个内置的glob filter。


例如，下面的代码过滤出以.java, .class和.jar结尾的文件：


```
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir, "*{java,class,jar}")) {
	for (Path entry : stream) {
		System.out.println(entry.getFileName());
	}
} catch (IOException x) {
	System.err.println(x);
}

```


#### Writing Your Own Directory Filter


在一些情况下，模式匹配不能满足你的需求，这时候你希望编写自定义的filter。你可以通过实现DirectoryStream.Filter<T>接口来自定义filter。这个接口仅包含一个方法： accept，来判断一个文件是否满足搜索的需求。


例如：

```
DirectoryStream.Filter<Path> filter = new DirectoryStream.Filter<Path>() {
	@Override
	public boolean accept(Path file) throws IOException {
		try {
			return Files.isDirectory(path);
		} catch (IOException x) {
			System.err.println(x);
			return false;
		}
	}
};

```

一旦创建了自定义的filter，就可以将它传递给newDirectoryStream(Path, DirectoryStream.Filter<? super Path>)方法。下面的代码，使用了上面的isDirectory filter 只打印出一个目录的子目录：

```
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir, filter)) {
	for (Path entry : stream) {
		System.out.println(entry.getFileName());
	}
} catch (IOException x) {
	System.out.println(x);
}

```


































