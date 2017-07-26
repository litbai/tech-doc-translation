### The Date-Time Packages

Date-Time API 主要由java.time包组成，另外还有其他4个package：

#### java.time

代表Date和Time的核心类都在此包中。它包括与date、time、datetime、time zones、instants、duration、clocks相关的类。这些类基于ISO-8601标准日历系统，它们是不可变类，是线程安全的。


#### java.time.chrono

代表非ISO-8601的其他日历系统。你也可以创建自己的日历系统，本教程不涉及此包的详细介绍。

#### java.time.format

包括用来格式化和解析date和time的类。


#### java.time.temporal


可扩展的API，主要是框架和类库开发者使用，允许date、time、querying、adjustment类之间的交互。Field和Unit是在这个包中定义的。


#### java.time.zone

支持时区的类，包括时区的偏移、时间规则等。它与时区有关，大多数情况下，开发者只应使用其中的ZonedDateTime、ZoneId和ZoneOffset类。














