### Synchronized Methods

java编程语言提供了两种基本的同步方式：同步方法和同步语句。二者中，同步语句更复杂一些，将在下一节详细介绍，本节集中介绍同步方法。

为了声明一个同步方法，仅仅需要在方法签名中加入synchronized关键字：

```
public class SynchronizedCounter {
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

假设counter是SynchronizedCounter的一个实例，那么将这些方法声明为同步方法会有两个作用：

* 首先，在同一个对象上对同步方法的两次调用不可能交叉执行。当一个线程执行counter对象的一个同步方法时，其他调用counter对象的同步方法的线程都将被阻塞，直到第一个线程执行完那个同步方法。
* 第二，当一个同步方法退出时，它自动与后续将被调用的同一个对象上的同步方法之间建立了一个happens-before关系，这保证了对象状态的改变对所有的线程都是可见的。


注意，构造方法不能是同步的--构造方法的声明中加入synchronized关键将产生语法错误。同步的构造方法没有任何意义，因为只有创建这个对象的线程才应该能在创建过程中访问这个对象。


	注意：当创建一个将被多个线程共享的对象时，当心不要过早的将这个对象的引用泄露出去。例如，假设你想要维持一个列表instances，里面存放一个类的所有实例。你可能会在构造函数中加入如下代码：
	
	```
	instances.add(this)；
	
	```
	但是，一旦这样做之后，其他的线程就可以通过instances访问这个对象，即使这个对象的构造函数还没结束。
	
	
同步方法是一个可以防止线程干扰和内存一致性问题的简单策略。如果一个对象对多个线程都是可见的，那么对于这个对象的域的读和写都应该通过同步方法。（但也有例外：final修饰的域，当对象创建完毕就不能改变了。一旦对象创建完毕，final域就可以安全的使用非同步方法来访问。）同步方法简单有效，但是可能伴随着liveness(活跃性)问题。































