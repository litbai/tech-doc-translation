### Thread Pools

java.util.concurrent包中的大多数executor实现使用了线程池，线程池由工作线程组成。工作线程独立与Runnable和Callable任务存在，经常被用来执行不同的任务。


使用工作线程可以最小化线程创建的开销。在大型应用中，线程对象占用了很多内存，分配和回收这些线程引入了巨大的内存管理开销。


一个常见的线程池类型是固定大小的线程池。这种线程池总是有固定书面语的活跃线程，如果一个线程在运行过程中遇到意外情况终结了，它自动创建一个新线程。当要执行的任务数超过线程池中的线程数时，额外的任务将放到一个内部队列中，当有空闲线程时就把这些任务分给空闲的线程。


固定大小线程池的一个显著优势就是应用可以使用它进行优雅的降级。为了理解这一点，举个例子。想象一个web服务器应用，对于每个request都用一个独立的线程去处理。如果这个应用仅仅是为每个HTTP请求创建一个新线程，此时系统收到了超过它处理能力的请求数量，则当创建这些线程的开销超过系统的处理能力时，应用将会突然停止响应所有的请求。如果创建的线程个数是有上限的，应用可能无法及时的同时响应所有的请求，但是它将在系统可以维持正常运转的同时，尽快的响应这些请求。

创建一个固定大小线程池的简单方法是调用java.util.concurrent.Executors的newFixedThreadPool方法。Executors类同时也提供了如下的工厂方法：


* newCachedThreadPool方法创建一个executor，这个executor有一个可扩展的线程池。这种executor适用于处理很多简短任务的应用。
* newSingleThreadPool方法创建了一个executor，这个executor只有一个线程，每次只能执行一个任务。
* ScheduledExecutorService版本的工厂方法。



如果上面提供了工厂方法都不满足你的需求，你可以创建一个java.util.concurrent.ThreadPoolExecutor的实例，或者一个java.util.concurrent.ScheduleThreadPoolExecutor的实例，它们提供了额外的选项。































