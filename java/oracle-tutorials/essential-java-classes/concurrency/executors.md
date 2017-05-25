### Executors

在前面的所有示例中，task和thread的关系都很耦合，task由Runnable对象来定义，而thread由Thread对象定义。这对于小应用来说，可以很好的工作，但对于大规模应用，将线程的创建、管理与应用的其他部分分离开来是大有裨益的。线程创建和管理的功能被executors对象封装了。接下去的三节将详细的介绍executors。


* Executors interfaces：本节介绍了三种executor对象类型
* Thread Pools：本节介绍了最常见的一种executor实现--线程池
* Fork/Join，JDK7引入，主要功能是可以充分挖掘多处理器性能。