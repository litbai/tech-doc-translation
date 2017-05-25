### Interrupts

一个中断是对线程的一个暗示，表示一个线程应该停下手头正在做的事情，转而去做其他的事情。而具体改做什么事情，是通过程序开发者指定的一个线程该如何应对中断来确定的，但是通常应对中断的方式就是中止线程的执行。

一个线程通过调用目标线程对象的interrup方法来发送一个中断。如果想线程机制正常运行，首先需要被中断的线程支持自身可以被中断。


### 中断支持

一个线程如何支持自身被中断？这是由当前它正在做什么决定的。如果一个线程在频繁的调用声明抛出InterruptedException的方法，那么它将在捕获异常后，执行catch中的语句，然后继续执行完run方法中的代码，最后return。例如，假设SleepMessage类中核心的信息循环代码是在一个Runnable对象的run方法中，那么它可以被修改为如下代码来支持被中断：

```
for (int i = 0; i < importantInfo.length; i++) {
    // Pause for 4 seconds
    try {
        Thread.sleep(4000);
    } catch (InterruptedException e) {
        // We've been interrupted: no more messages.
        return;
    }
    // Print a message
    System.out.println(importantInfo[i]);
}


```

许多方法都会抛出InterruptedException，例如sleep方法，它被设计为当发生中断时，停止当前的工作，立即返回。


如果一个线程运行了很长时间都没有调用声明抛出InterruptedException的任何方法，该怎么办？它必须阶段性的调用Thread.interrupted方法，如果确实发生了中断，这个方法将返回true，例如：

```
for (int i = 0; i < input.length; i++) {
	heavyCrunch(input[i]);
	if (Thread.interrupted()) {
		// we have been interrupted: no more crunching
		return;
	}
}

```


在上述示例中，我们只是测试是否发生了中断，如果是，则退出。在更复杂的应用中，抛出一个InterruptedException更有意义：

```
if (Thread.interrupted()) {
	 // 调用时， 还要处理InterruptedException， 奇怪。。。
    throw new InterruptedException();
}

```

这样的话，可以将处理中断的代码集中在catch子句中。


### 中断码标识


中断机制是通过一个中断标识来实现的，这个中断标识称为中断码。调用Thread.interrupt会将这个标志置位。当一个线程调用静态方法Thread.interrupted方法来检查中断时，中断标识将会被复位（清除）。非静态的isInterrupted方法，是一个线程用来查询另一个线程状态的，不会改变中断码标识。


按照惯例，任何抛出InterruptedException， 然后退出的方法，都将清除中断标识位。但是，中断标识可能会被另一个线程调用interrupt方法再次设置。




























































