### High Level Concurrency Object

目前为止，本章教程主要集中在java平台最开始提供的底层的并发相关的API。这些API足以应付简单的任务，但是对于更高级的任务，则需要更高级、更上层的API，尤其是对于那些需要充分利用现代的多处理器和多核特性的大规模并行应用来说。


本节我们将会介绍一些JDK5.0引入的高级并发特性。这些特性的实现大部分在java.util.concurrent包中。它们同样是java集合框架引入的，新的并发数据结构。

* Lock 对象，支持锁语法，简化了许多并发应用程序
* Executors定义了启动和管理线程的高级API。java.util.concurrent包中提供的各种Executor实现，提供了适用于大规模应用的线程池管理。
* Concurrent collections ，支持并发的集合可以很容的管理大规模的数据集合，而且极大的降低了synchronization的使用。
* Atomic variables：可以尽量减少synchronization的使用，同时还可以避免内存一致性错误
* ThreadLocalRandom：JDK7引入，提供了在多线程间生成随机数的有效方式。































