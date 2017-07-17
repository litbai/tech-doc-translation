### I/O from the Command Line

在命令行环境下，一个程序通过命令行与用户进行交互。java平台使用两种工具提供了这种交互方式：通过标准的流和通过Console类。

### 标准流

标准流是操作系统的一个特性。通常，它们从键盘读取输入，然后将输出显示在显示屏上。操作系统还支持通过命令行来操作文件或者在两个程序之间交互。


java平台支持3种标准流：标准输入(System.in)、标准输出(System.out)、标准错误(System.err)。这3个对象已经提前定义好了，并且你不用显示的open它们。标注输出和标准错误都是用来进行输出操作。将错误单独分离开来，可以使用户在得到错误提示的同时，将错误信息进行存档。

你可能任务标准流是字符流，但是，因为历史的原因，它们是字节流。System.out和System.err是PrintStream的实例。尽管在技术上来说，它们确实是字节流，但是PrintStream使用了一个内部的字节流对象来实现了很多字符流的功能。

而System.in是一个字节流对象，它没有字符流的特性。如果想在标准输入中使用字符流，那么你需要使用InputStreamReader来包装System.in，例如：

```
InputStreamReader cin = new InputStreamReader(System.in);

```


#### The Console

...暂使用不到



















