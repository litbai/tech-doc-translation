## The finally Block

finally块在try块退出的时候执行。这保证了finally块即使在异常发生时仍然会执行。但是finally块不只是用在异常处理中--它可以避免程序员在偶然的执行return、continue或者break语句后忘记释放资源。将cleanup代码放在finally块中是比较好的编程实践，即使没有涉及异常处理。

writeList方法中的try块打开了一个PrintWriter。程序应该在writeList方法退出之前释放这个资源。但是，比较复杂的地方在于，writeList方法的try块可能以3种方式中的一种来退出：

1. new FileWriter语句失败，抛出IOException
2. list.get(i)语句失败，抛出IndexOutOfBoundException
3. writeList方法正常执行，try块正常退出


运行时系统总会执行finally块，而不按管在try块中会发生什么情况。因此，finally块非常适合用来做清理工作。


下面的示例代码，在writeList方法的finally块中执行了关闭PrintWriter操作：

```
finally {
	if (out != null) {
		System.out.println("Closing PrintWriter");
		out.close();
	} else {
		System.out.println("PrintWriter not open");
	}
}

```

** 注意：finally块是阻止资源泄露的关键工具。当关闭一个文件或者是恢复一个资源时，可以将相关代码放在finally块中，来保证资源总是被关闭。**


在上述情况下，可以考虑使用try-with-resource语句，它可以在代码执行完毕后自动的释放系统资源，try-with-resource语句在后面的小节会详细介绍。


























