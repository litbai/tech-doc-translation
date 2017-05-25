### Thread Objects

每一个线程都与Thread类的一个对象相关联。使用Thread对象创建多线程应用，有两种基本的策略：

* 直接创建线程，然后自己管理线程。这时，只需要在系统想要执行一个异步任务的时候，创建一个Thread类的实例，然后自己管理即可。
* 将线程管理从你的应用程序中抽象出来，将需要执行的任务扔给一个executor。

本节只包括如何使用Thread对象的知识，Executor的知识将会在后面的high-level concurrency objects一节中介绍。