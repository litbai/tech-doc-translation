## The Hello World Application

这一章的几个小节详细描述了如何编译和运行java Hello World程序的步骤。第一小节介绍了如何使用NetBeans IDE开发一个Hello World程序。NetBeans是一个集成开发环境，它大大简化了软件开发过程。NetBeans可以运行在Windows和Linux平台。接下来的两小节提供了需要运行Hello World程序的平台特定的指令，而没有使用IDE。如果你遇到问题，请先咨询“常见问题”一节，这一节提供了一些新手常见的问题。

### "Hello World!" for the NetBeans IDE

### "Hello World!" for Microsoft Windows and Linux

是时候开始编写你的第一个程序了。接下来的命令适用于Windows vista, Windows 7 和 8.

* Checklist：编写你的第一个程序，你需要：
	* JDK8
	* 文本编辑器，例如Notepad或者Vi
* 创建你的第一个应用
第一个应用，HelloWorldApp，我只会简单的输出“Hello World!”，创建这个程序，你需要：
	* 创建一个源文件
	一个源文件包含java代码，它可以被你和其他程序员理解，你可以使用任意的文本编辑器来编辑此源文件。
	* 编译源文件，得到.class文件
	java编译器（javac命令）会将源代码翻译成JVM能够理解的指令，这些指令称为字节码。
	* 运行程序
	java应用启动工具（java application launcher tool，java命令）启动JVM来运行你的程序。
	
##### 创建源文件
```
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}
```

##### 将源文件编译为.class文件

javac HelloWorldApp.java，注意此处文件需要带后缀名.java，且需要在文件所在的目录执行此命令。

##### 运行

java -cp . HelloWorldApp，注意此处不需要带后缀名.class，-cp是指定classpath，此命令的意思是指定搜寻的classpath为当前目录