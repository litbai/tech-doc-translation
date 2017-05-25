### Memory Consistency Errors

当不同的线程以不同的视角看共享内存时，可能发生内存一致性错误。引起内存一致性错误的原因非常复杂，超出了本教程的范围。幸运的是，开发人员无需理解这些原因的细节。我们只需要知道如何避免内存一致性错误即可。


避免内存一致性错误的关键是理解happens-before关系。这个关系仅仅是一个保证，它保证了一条语句写内存的结果对另一条语句是可见的。考虑如下的示例，假设我们定义并初始化了一个简单的int类型的域：

```
int counter = 0;

```

counter域被两个线程共享，A和B。假设A对counter执行了加1操作：

```
counter++;

```

然后，B执行了打印操作：

```
System.out.println(counter);

```

如果这两条语句在同一个线程中执行，我们可以保证打印出的counter值为1。但是，如果这两个语句分别在不同的线程中执行，打印出的counter值可能为0。因为，以为没有人保证A对counter的更改一定会对B可见，除非开发人员在这两条语句之间制定一个happens-before关系。


有一些动作会建立happens-before关系。其中一个便是synchronization，在后续的章节中会介绍。


我们已经见过两种动作可以创建happens-before关系：

* 当一条语句调用Thread.start的时候，跟这条语句有happens-before关系的所有语句也和新线程执行的语句有happens-before关系。导致新线程创建的代码对新线程是可见的。
* 当一个线程A终止了，导致调用了Thread.join方法等待这个线程A执行完毕的另一个线程B重新开始执行，此时已经终止的线程A的执行结果对线程B是可见的。


如果想知道建立了happens-before关系的所有动作，参考：https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html#MemoryVisibility





















































