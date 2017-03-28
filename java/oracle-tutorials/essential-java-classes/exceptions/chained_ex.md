## 异常链

一个应用处理一个异常的常见方式是抛出另一个异常。实际上，第一个异常引起了第二个异常。如果可以知道哪个异常引起了当前异常，是非常有用的：Chained Exceptions可以使开发人员或者这个信息。

下面是Throwable类的方法和构造器，这些方法和构造器可以支持异常链：

```
Throwable getCause()
Throwable initCause(Throwable)
Throwable(String, Throwable)
Throwable(Throwable)

```


上述方法中，ininCause(Throwable)方法的Throwable参数，以及构造器中的Throwable参数，是引起当前异常的那个异常。getCause()方法返回引起当前异常的exception，initCause可以设置引起当前异常的exception。


下面的示例展示了怎样使用一个异常链：

```
try {

} catch (IOException e) {
	throw new SampleException("Other IOException", e);
}

```


在上述示例中，当一个IOException异常被捕获时，一个新的SampleException异常被创建，并且cause设置为了捕获到的IOException异常，然后将SampleException异常抛出。


### 访问Stack Trace信息

现在，我们假设高层的异常处理器想要得到得到stack trace的镜像。


** 定义：一个stack trace提供了当前线程的执行历史信息，并且列出了当异常发生时调用的方法名和类名。当发生一个异常时，stack trace是调试的绝佳工具。**


下面的代码展示了如何调用异常对象的getStackTrace()方法：

```
catch (Exception cause) {
	StackTraceElements elements[] = cause.getStackTrace();
	for (int i = 0, n = elements.length; i < n; i++) {
		System.err.println(elements[i].getFileName() + ":" + elements[i].getLineNumber() + ">> " + elements[i].getMethodName() + "()");
	}
}

```


### Logging API


下面的代码片段会在异常发生时，打印一些日志。但是，不是采用手动的解析堆栈信息并将信息输出给System.error，而是采用java.util.logging包中的logging的方式。






























