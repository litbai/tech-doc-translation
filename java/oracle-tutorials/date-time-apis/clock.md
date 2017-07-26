### Clock

很多Temopral-based class提供了一个无参的now()方法，使用系统的clock和默认的时区来返回当前的日期和时间。这些类还提供了一个接收一个Clock类型的参数的now方法，允许你传递一个Clock类型的参数。


当前的日期和时间依赖于时区，为了创建一个国际化的应用，你需要使用Clock来确保返回当前时区的日期和时间。因此，尽管Clock参数是可选的，它允许你的代码使用别的时区，或者使用一个固定的时钟。


Clock是一个抽象类，因此你不能创建它的实例，你可以使用下面的工厂方法：

* Clock.offset(Clock, Duration)
* Clock.systemUTC()
* Clock.fixed(Instant, ZoneId)

































