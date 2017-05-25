### Thread Interference

考虑一个简单的类Counter：

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


Counter被设计有两个方法，increment方法将c加1，decrement方法将c减1。但是，如果一个Counter对象被多个线程引用，这些线程之间将会产生干扰，使得increment和decrement不能按预期正常工作。

乍一看，对Counter对象的操作不可能产生多线程的干扰，因为对c的操作都是单条语句。但是，即使是单条语句也可能被翻译成虚拟机的多条指令。我们不会详细介绍虚拟机执行的具体指令--目前只需要知道上述的语句c++会被分解为如下3个步骤即可：

1. 得到c的当前值
2. 将上述得到的值加1
3. 将新的c值写回内存


假设Thread A调用了increment方法，同时Thread B调用了decrement方法。如果c的初始值为0，它们的交叉执行顺序可以如下：

1. Thread A：Retrieve c
2. Thread B：Retrieve c
3. Thread A: Increment retrieved value; result is 1.
4. Thread B: Decrement retrieved value; result is -1.
5. Thread A: Store result in c; c is now 1;
6. Thread B: Store result in c; c is now -1;


看，Thread A的结果丢失了！被Thread B的结果覆盖了！上述特殊的交叉执行顺序仅仅是可能发生的一种情形。在不同的情形下，有可能Thread B的结果丢失，或者二者都正常运行。因为这是不可预测的，所以线程干扰的bug很难发现和修复。



































