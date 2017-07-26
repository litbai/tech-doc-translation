### Lession: Date-Time Overview

虽然Time看起来是一个简单的对象，即使是一个普通的手表也能提供比较准确的日期和时间。但是，仔细来看，你会发现细微的复杂性，很多因素都会影响你对时间的理解。例如，1.31号加上一个月，对于闰年和平年得到的结果就不一样。时区概念同样增加了时间的复杂度，例如，一个国家一年可能会短时间内经历一次夏令时，有些国家1年经历几次夏令时，而有些国家1年都经历不到1次夏令时。

Date-Time API使用了ISO-8601定义的日历系统作为默认的日历系统。这个日历基于Gregorian日历系统，已经被用于代表日期和时间的事实标准。Date-Time API中的核心类命名跟下面的几个示例类似： LocalDateTime, ZonedDateTime, OffsetDateTime。所有这些类都使用ISO日历系统。如果你想使用其他日历系统，例如Hijrah 或 Thai Buddhist,你可以利用java.time.chrono包的支持，或者你可以创建自己的日历系统。


Date-Time API使用 Unicode Common Locale Date Repository。这个仓库支持世界各国语言，并且包括世界上最大的locale data。关于这个仓库的信息已经被翻译为数百种语言。Date-Time API也支持Time-Zone Database。该数据库提供了自1970以来全球每一个时区的变化信息，自从引入概念以来的主要时区都有历史记录。


















