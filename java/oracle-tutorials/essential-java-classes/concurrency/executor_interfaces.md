### Executor Interfaces

java.util.concurrent 包定义了三种executor接口：

* Executor, 一个最简单的接口，它支持启动新任务
* ExecutorService，是Executor的子接口，增加了生命周期管理的特性，包括单个任务和executor本身的生命周期。
* ScheduledExecutorService, 是ExecutorService的子接口，它支持定期执行任务或在将来的某个时间点执行任务。


通常，一个executor对象会声明为以上三个接口类型之一，而不是一个executor的具体实现类。


#### The Executor Interface

Executor接口只定义了一个方法execute，作为通常的创建线程语法的建议替代者。如果r是一个Runnable对象，e是一个Executor对象，那么，你可以将

```
()new Thread(r)).start()

```

替换为

```
e.execute(r);

```


但是execute的具体实现则可能没有这么直接。当然，最底层的语法是创建一个新线程，然后立即启动这个线程。根据Executor实现的不同，execute方法可能做的事情是一样的，但是它更可能使用一个已经存在的工作线程来运行r，或者将r放入一个队列中，等待空闲的工作线程（后面的Thread Pools一节会详细介绍）。

java.util.concurrent包的Executor实现，充分利用了更高级的ExecutorService和ScheduledExecutorService接口，尽管它们工作在基本的Executor接口之上。


#### The ExecutorService Interface

ExecutorService接口除了继承了Executor接口的execute方法之外，还提供了一个类似的，但是功能更强大的submit方法。像execute方法一样，submit方法也接收一个Runnable类型的参数，但是还有重载的另一个版本，它可以接受Callable类型的参数，这种方式可以运行task返回一个值。submit方法返回一个Future对象，通过这个对象可以获取Callable的返回值，并且可以管理Callbale和Runnable对象的状态。


#### The ScheduledExecutorService Interface


ScheduledExecutorService还提供了一个方法：schedule，它可以延期执行Runnable和Callable任务。除此之外，这个接口还定义了scheduleAtFixRate和scheduleWithFixedDelay方法，可以以指定的间隔时间重复的执行任务。




























