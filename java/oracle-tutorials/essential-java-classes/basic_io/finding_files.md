### Finding Files

如果你以前写过shell脚本，那么你很可能会用到模式匹配来搜寻文件。实际上，你可能经常使用模式匹配。如果你没有使用过，模式匹配就是用一些特殊的字符和普通的字符来创建一个模式，然后可以用文件夹与这个模式进行比较。例如，在大多数的shell脚本中，星号*, 可以匹配任务数量的字符。例如，下面的命令会列出当前目录中以.html结尾的所有文件：

```
% ls *.html
```

java.nio.file包中提供了对这种特性的支持。每一个文件系统实现都提供了一个PathMatcher。你可以通过FileSystem类的getPathMater(String)方法来得到一个PathMatcher对象。下面的代码片段从default file system中得到一个PathMatcher对象：

```
String pattern = ...;
PathMatcher matcher = FileSystems.getDefault().getPathMathcer("glob: " + pattern);

```

传递给getPathMatcher的字符串参数，指定了需要的语法和pattern。上述示例指定了使用glob语法。

glob语法很容易使用，并且很灵活。但是如果你更喜欢使用正则表达式的话，你也可以指定使用正则表达式语法。正则表达式会在下一章进行讲解。


如果你想使用其他形式的模式匹配，那么你需要创建你自己的PathMatcher类，本章的示例使用glob语法。


一旦你创建了一个PathMatcher的实例，你就可以用它来进行匹配了。PathMatcher接口只有一个方法： matches,它接收一个Path类型的参数，返回一个布尔值，例如：

```
PathMatcher matcher = FileSystems.getDefault().getPathMatcher("glob:*.{java,class}");

Path file = ...;

if (matcher.matches(file)) {
	System.out.println(file);
}

```


#### Recursive Pattern Matching


搜寻指定的文件，通常与遍历文件树一同使用。很多时候，你知道你要找的文件就在系统中，但是你不知道它的具体位置，或者你想要搜寻整个目录树，找出所有符合某种模式的文件。


下面的Find示例就会遍历一个文件树，搜寻指定模式的文件。Find跟UNIX中的find命令功能类似，但是删减了跟多功能。你可以扩展Find程序，添加需要的功能。例如，find命令支持-prune参数，来排除一个subtree。你可以通过在preVisitDirectory方法中返回SKIP_SUBTREE来完成这个功能。为了实现-L选项，它可以跟随符号链接，你可以使用接受4个参数的walkFileTree方法，并且传递FOLLOW_LINKS参数进去（这时候，你需要确保在vifitFile方法中测试循环引用）。


如果想运行Find程序，使用如下格式：

```
% java Find <path> -name "<glob_pattern>"

```


注意， 上述的"<blob_pattern>",是在双引号之中的，这是为了不让shell来把里面的特殊字符进行转译，例如：

% java Find . -name "*.{class,java}" 


Find类的源代码如下：


```
public class Find {
	public static class Finder extends SimpleFileVisitor<Path> {
		private final PathMatcher matcher;
		private int numMatches = 0;
		
		Finder(String pattern) {
			matcher = FileSystems.getDefault().getPathMatcher("glob:" + pattern);
		}
		
		void match(Path file) {
			Path name = file.getFileName();
			if (name != null && matcher.matchers(name)) {
				numMatches++;
				System.out.println(file);
			}
		}
		
		void done() {
			System.out.println("Matched: " + numMatches);
		}
		
		@Override
		public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) {
			match(file);
			return CONTINUE;
		}
		
		@Override
		public FileVisitResult preVisitDirectory(Path dir, BasicFileAttributes attrs) {
			match(dir);
			return CONTINUE;
		}
		
		@Override
		public FileVisitResult visitFileFailed(Path file, IOException exc) {
			System.err.println(exc);
			return CONTINUE;
		}
		
		static void usage() {
			System.err.println("java Find <path> -name \"<glob_pattern>\"");
		}
		System.exit(-1);
	}
	
	public static void main(String[] args) throws IOException{
		if (args.length < 3 || !args[1]).equals("-name")) {
			usage();
		}
		Path dir = Paths.get(args[0]);
		String pattern = args[2];
		
		Finder finder = new Finder(pattern);
		Files.walkFileTree(dir, finder);
		finder.done();
	}
}

```























