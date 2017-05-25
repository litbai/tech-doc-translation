### Guarded Blocks

线程通常需要协调它们之间的行为。最常见的协调行为的方式是警戒块或保护快。警戒块是一个代码块，开始于不断的轮询一个条件，知道这个条件为true，代码块才继续向下执行。如果想正确的使用警戒块，需要如下几个步骤：


假设guardedJoy是一个方法，在其他线程将共享变量joy设置为true之前，它不能向下执行。这个方法，理论上来说，可以一直循环判断joy变量的值，如果为true，则继续执行。但是，这种循环判断浪费了处理器的时间，因为在条件满足之前，它一直在空转。

```
public void guardedJoy() {
	// Simple loop guard. Wastes processor time. Dont do this
	while (!joy) {
		
	}
	System.out.println("Joy has been achieved!");
}

```

一个效率更高的警卫会调用Object.wait来挂起当前线程。调用wait方法后，直到另一个线程发布了一个暗示某种特殊事件已经发生的提醒之前，程序都不会return，尽管发生的这个事件并不一定是调用wait方法的线程所期待的事件。


```
public synchronized void guardJoy() {
	// This guard only loops once for each special event, which may not be the event we are waiting for
	while (!joy) {
		try {
			wait();
		} catch (InterruptedException e){}
	}
	System.out.println("Joy and efficiency has been achieved!");
}

```

	注意：你应该总是在一个判断condition的loop中调用wait方法。不要假设发生了中断意味着你期待的特殊事件就发生了，或者发生了中断，while条件就不满足了。（Don't assume that the interrupt was for the particular condition you were waiting for, or that the condition is still true.）


像其他可以挂起线程的方法一样，wait可能抛出InterruptedException。在本例中，我们可以忽略异常，因为我们根本不关心异常，我们只关心joy的值。


不知你有没有注意到，后面的这个方法加了synchronized关键字。假设我们在d对象上调用了wait方法。当一个线程调用d.wait()时，它必须首先拥有d的内部锁，否则程序会报java.lang.IllegalMonitorStateException。在synchronized方法中调用wait方法，可以保证我们首先获取了对象的内部锁。具体可以参考Object.wait方法的注释。


当wait被调用时，当前线程会释放已获取的内部锁，并被挂起。在后续的某个时间点，另一个线程将会获取同样的对象锁，调用Object.notifyAll()方法，通知所有在这把锁上等待的线程，发生了something。


```
public synchronized notifyJoy() {
	joy = true;
	notifyAll();
}

```

当这个线程释放内部锁时，第一个线程将会获取内部锁，在wait方法调用处恢复执行。


	注意：还有一个Obecjt.notify()方法，只会唤醒一个线程。因为notify方法没有指定要唤醒哪个线程，它只用在大规模的并行应用中--也就是说，一个有很多个线程的应用，所有的线程都做同样的工作。在这样的应用中，你不必关心哪个线程将被唤醒。
	

让我们使用警戒块来创建一个Producer-Consumer应用。这种类型的应用在两个线程之间共享数据：生产者用来生产数据，消费者，则处理这些数据。这两个线程使用一个共享对象来通信。二者的合作是必须的：消费者线程必须在生成者生产出数据之后才能处理这个数据，生产者也必须在消费者处理完数据之后才能交付新的数据。

在下面的示例中，数据是一系列的文本，通过Drop对象来共享：


```

public class Drop {
	private String message;
	private boolean empty = true;
	
	public synchronized String take() {
		while (empty) {
			try {
				wait();
			} catch (InterruptedException e) {
				// just ignore
			}
		}
		System.out.println("take the message--->" + message);
		empty = true;
		notifyAll();
		return message;
	}
	
	
	public synchronized void put(String msg) {
		while (!empty) {
			try {
				wait();
			} catch (InterruptedException e) {
				// just innore
			}
		}
		message = msg;
		System.out.println("putting the msg--->" + message);
		empty = false;
		notifyAll();
	}
	
	public static void main(String[] args) {
		Drop drop = new Drop();
		new Thread(new Producer(drop)).start();
		new Thread(new Consumer(drop)).start();
	}

}

class Producer implements Runnable {
	private Drop drop;
	
	public Producer(Drop drop) {
		this.drop = drop;
	}
	
	@Override
	public void run() {
		String[] messages = {"one", "two", "three", "four"};
		Random random = new Random();
		for (String msg : messages) {
			drop.put(msg);
			try {
				Thread.sleep(random.nextInt(5000));
			} catch (InterruptedException e) {
				// just ignore
			}
		}
		drop.put("Done!");
	}
}

class Consumer implements Runnable {
	private Drop drop;
	
	public Consumer(Drop drop) {
		this.drop = drop;
	}
	
	@Override
	public void run() {
		Random random = new Random();
		for (String msg = drop.take(); !"Done!".equals(msg); msg = drop.take()) {
			System.out.format("MESSAGE RECEIVED: %s%n", msg);
			try {
				Thread.sleep(random.nextInt(5000));
			} catch (InterruptedException e) {
				// just ignore
			}
		}
	}
}


```


	注意：为了解释警戒块的用途，我们编写了Drop类。为了避免重新发明轮子，当你需要编写自己的数据共享对象时，首先检查一下java.util.concurrent包中有没有已经编写好的数据结构。
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	





















