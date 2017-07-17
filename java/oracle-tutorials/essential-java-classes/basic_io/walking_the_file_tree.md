### Walking the File Tree

你是否曾写过一个应用，它会递归的遍历一个目录下的所有文件？或许你需要删除目录下的所有.class文件，或者搜寻自从去年就没访问过的所有文件。你可以使用FileVisitor接口来遍历一个目录树。


#### The FileVisitor Interface

为了遍历目录树，你需要实现一个FileVisitor。一个FileVisitor在遍历的一些关键点定义了特定的行为，这些关键点包括：访问文件时、访问目录前、访问目录后、发生错误时。这个接口有4个方法，分别对应上述的关键点。


* preVisitDirectory： 在访问一个目录的内容前调用
* postVisitDirectory：在访问完一个目录的所有内容后调用。如果遇到了错误，会将错误向上传递。
* visitFile：访问文件时调用。这时候，会将一个BasicFileAttributes对象传递给这个方法，你就可以获取文件的一些基本属性，当然你也可以自行访问文件的其他属性，比如你可以使用DosFileAttributeView来判断一个文件是否设置了hidden位。
* visitFileFailed：当文件不可访问时会调用这个方法，通常是没读的权限。方法的参数为一个异常对象，你可以选择是否继续抛出这个对象、将错误打印到控制台或者忽略这个额错误。


如果你不想实现了FileVisitor的所有方法，你可以extends SimpleFileVisitor类。这个类，实现了FileVisitor接口的方法，它会访问所有的文件，在遇到错误的时候，会直接向上抛出这个异常。你可以继承这个类，并且覆盖需要自定义行为的方法。


下面是使用SimpleFileVisitor的示例，它打印出了所有的entry。它会打印entry是一个普通文件、符号链接、目录，还是其他不确定的文件。它还以字节为单位打出了每个文件的size。途中遇到的每个异常都被打印到console中。


```
import static java.nio.file.FileVisitResult.*;

public static class PrintFiles extends SimpleFileVisitor<Path> {

	@Override
	public FileVisitResult visitFile(Path file, BasicFileAttributes attr) {
		if (attr.isSymbolicLink()) {
			System.out.format("Symbolic link: %s ", file);
		} else if (attr.isRegularFile()) {
			System.out.format("Regular file: %s ", file);
		} else {
			System.out.format("Other: %s ", file);
		}
		System.out.println("(" + attr.size() + "bytes)");
		return CONTINUE;
	}
	
	@Override
	public FileVisitResult postVisitDirectory(Path dir, IOException exc) {
		System.out.format("Directory: %s%n", dir);
		return CONTINUE;
	}
	
	@Override
	public FileVisitResult visitFileFailed(Path file, IOException exc) {
		System.err.println(exc);
		return CONTINUE:
	}
	
}

```


#### Kickstarting the Process

一旦实现了你自己的FileVisitor，你怎么start遍历？Files类中提供了两个walkFileTree方法。

* walkFileTree(Path, FileVisitor)
* walkFileTree(Path, Set<FileVisitorOption>, int, FileVisitor)


第一个方法只需要一个起始点和一个FileVisitor即可。你可以如下调用：

```

Path startingDir = ...;
PrintFiles pf = new PrintFiles();
Fiels.walkFileTree(startingDir, pf);

```

第二个方法允许你额外的制定一个限制，它可以限制递归的深度。除此之外，你还可以制定一个FileVisitorOption的集合。如果你想确保能够完整的遍历完文件树，你可以指定递归的深度为Integer.MAX_VALUE。


你可以指定FileVisitorOption enum，比如 FOLLOW_LINKS，它表示应该处理符号链接本身而不是它的target。


```
import static java.nio.file.FileVisitResult.*;

Path startingDir = ...;

EnumSet<FileVisitOption> opts = EnumSet.of(FOLLOW_LINKS);

PrintFiles pf = new PrintFiles();
Files.walkFileTree(startingDir, opts, Integer.MAX_VALUE, pf);

```


#### Considerations When Creating a FileVisitor


文件树遍历采用深度优先策略，但是你不能假设子目录之间的访问顺序。

如果你的程序将会改变文件系统，那么你需要好好考虑如何实现你自己的FileVisitor。

例如，如果你在写一个递归删除的程序，你需要在删除一个目录之前，先删除它里面的所有文件。在这种情况下，你需要在postVisitDirectory()方法中删除这个目录。


如果你正在写一个递归的拷贝文件，在你拷贝一个目录的文件之前，你需要在preVisitDirectory方法中创建新目录。如果你想保留源目录的属性（类似于cp -p命令），你需要在文件拷贝结束后，在postVisitDirectory方法中修改属性。Copy类的例子就是这么做的。

http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java

如果你正在写一个文件搜索的程序，你应该在visitFile方法中执行比较操作。这个方法会搜索所有满足规则的文件，但是它不搜索目录。如果你想同时搜索文件和目录，你必须在preVisitDirectory或者postVisitDirectory方法中执行比较操作，参考Find示例。 

http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/essential/io/examples/Find.java



你还需要决定你是否需要follow symbolic link。例如，如果你正在执行删除文件的操作，那么可能following symbolic是不推荐的。如果你正在拷贝一个文件树，你可能需要follow symbolic link。默认的，walkFileTree dont follow symbolic link。


visitFile方法在文件是file类型才执行。如果你指定了FOLLOW_LINKS选项，你的文件树又有指向父目录的环路，那么这个循环的目录将会在visitFileFailed方法中抛出FileSystemLoopException。下面的代码展示了如何捕获一个环路link：

```
@Overried
public FileVisitResult visitFileFailed(Path file, IOException exc) {
	if (exc instanceof FileSystemLoopException) {
		System.err.println("cycle detected: " + file);
	} else {
		System.err.format("Unable to copy: %s: %s%n", file, exc);
	}
}


```


#### Controlling the Flow

也许你想要遍历目录树找到某个特定的目录，在找到这个目录后，终止遍历。也许，你想跳到某些文件。

FileVisitor方法返回一个FileVisitResult对象。你可以通过返回一个特定的FileVisitResult对象，来终止遍历过程，或者控制是否遍历一个目录，


* CONTINUE：表示遍历程序继续执行。如果preVisitDirectory方法返回CONTINUE，那么接下去会遍历这个目录。
* TERMINATE：终止遍历过程。
* SKIP_SUBTREE：当preVisitDirectory方法返回这个值的时候，这个目录及其子目录都将被跳过遍历，这个breach将被pruned out of the tree。
* SKIP_SIBLINGS：当preVisitDirectory返回这个值的时候，这个目录将不会被访问，postVisitDirectory也不会被调用，剩余的兄弟节点也不会被访问。如果在postVisitDirectory中返回这个值的话，剩余的兄弟节点将不会被访问。本质上，在这个指定的目录中，将不会再发生任何动作。

下面的代码表示，任何名称为SCCS的目录都将被跳过：

```
import static java.nio.file.FileVisitResult.*;

public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) {
	if (dir.getFileName().toString().equals("SCCS")) {
		return SKIP_SUBTREE;
	}
	return CONTINUE;
}

```


下面的代码表示，一旦访问到一个指定的文件，这个文件将会被打印到标准输出，同时终止遍历过程：


```
import static java.nio.file.FileVisitResult.*;

Path lookingFor = ...;

public FileVisitResult visitFile(Path file, BisicFileAttributes attr) {
	if (file.getFileName().equals(lookingFor)) {
		System.out.println("Located file: " + file);
		return TERMINATE;
	}
	return CONTINUE;
}

```

#### Examples


下面的几个代码示例阐述了遍历文件的机制：

* Find： 递归的遍历文件树搜索与glob模式匹配的文件和目录
* Chmod：递归改变一个文件树的权限，仅仅适用于POSIX系统
* Copy：递归的拷贝文件
* WatchDir：监视一个目录创建、删除、修改一个文件的事件。启动这个程序时，如果指定了-r选项，则会监视整个文件树，更多关于文件通知服务的信息，见下面的章节： Watching a Directory Changes.






























