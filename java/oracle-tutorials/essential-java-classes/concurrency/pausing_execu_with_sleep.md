### Pausing Execution with Sleep


Thread.sleep可以使当前线程停止执行指定的时间段。这是一种使系统中其他线程可以获得处理器时间的有效方法。The sleep method can also be used for pacing, as shown in the example that follows, and waiting for another thread with duties that are understood to have time requirements, as with the SimpleThreads example in a later section.


sleep方法有两个重载的版本。其中一个指定了需要睡眠的微秒时间，另一个同时指定了微秒和纳秒时间。但是，这些睡眠时间并不保证一定是精确地，因为它们受限于底层的操作系统层。同时，线程在睡眠过程中可以被打断，后面我们将会查看示例。在任何情况下，你都不能假设，调用sleep方法可以精确的将线程挂起指定时间。

下面的SleepMessage示例，使用了sleep方法，每4s打印一条信息：

```

public class SleepMessage {
	public static void main(String[] args) throws InterruptedException{
		String importantInfo[] = {"Mars eat oats", "Does eat oats", "Little lambs eat ivy", "A kid will eat ivy too"};
		
		for (int i = 0; i < importantInfo.length; i++) {
			// pause for 4 seconds
			Thread.sleep(4000);
			// print a message
			System.out.println(importantInfo[i]);
		}
	}
}


```

注意，main方法声明它将抛出InterruptedException。这是sleep方法声明抛出的异常，当另外的线程interrupt当前正在sleep的线程时，将会抛出这个异常。因为上述的示例，没有定义其他线程来触发打断操作，所以无须捕获此异常。























