### Concurrent Collections

java.util.concurrent包中包含了对java集合框架的一些补充。它们可以很容易的根据collection interface接口进行归类：

* BlockingQueue: 定义了一个FIFO的数据结构。当你向一个满队列中添加元素或者检索空队列时，它会阻塞或者超时。
* ConcurrentMap: 它是java.util.Map的一个子接口，定义了很多有用的原子操作。这些操作包括删除或者替换key-value对（仅当key存在时），或者当key不存在时，添加key-value对。保证这些操作的原子性可以避免使用同步代码。ConcurrentMap的标准的通用实现是ConcurrentHashMap，是HashMap的并发版本。
* ConcurrentNavigableMap: 是ConcurrentMap的子接口，它支持近似匹配。ConcurrentNavigableMap的标准通用实现类是ConcurrentSkipListMap，是TreeMap的并发版本。


所有上述的集合都能避免内存一致性错误，通过在集合中添加元素和后续的访问以及删除元素之间建立一个happens-before关系。