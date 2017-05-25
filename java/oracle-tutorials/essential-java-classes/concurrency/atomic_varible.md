### Atomic Varibale

java.util.concurrent.atomic 包定义了一些支持单个变量原子操作的类。所有的类都有get和set方法，它们的工作方式就和read和write volatile变量一样。也就是说，set方法和其后对同一个对象的get方法确立了一个happens-before关系。原子方法compareAndSet还具有内存一致性的特性，适用于整数原子变量的简单原子算术方法。

为了阐述如何使用包中的这些类，让我们回过头来看Counter类：

```
class Counter {
	private int c = 0;
	
	public void increment() {
		c++;
	}
	
	public void decrement() {
		c--;
	}
	
	public int value() {
		return c;
	}
}

```

使Counter类变为线程安全的一种方法是使用synchronized，如下所示：

```
class SynchronizedCounter {
	private int c = 0;
	
	public synchronized void increment() {
		c++;
	}
	
	public synchronized void decrement() {
		c--;
	}
	
	public synchronized int value() {
		return c;
	}
}

```


对于Counter这样简单的类来说，synchronization是可以接受的方法。但是对于更复杂的类来说，我们希望尽可能的避免使用不必要的synchronization而降低程序的活跃性。使用AtomicInteger替换int，可以使我们不使用synchronization的同时，避免线程干扰，如下所示：

```
class AtomicCounter {
	private AtomicInteger c = new AtomicInteger(0);
	
	public void increment() {
		c.incrementAndGet();
	}
	
	public void decrement() {
		c.decrementAndGet();
	}
	
	public int value() {
		return c.get();
	}
}

```
























