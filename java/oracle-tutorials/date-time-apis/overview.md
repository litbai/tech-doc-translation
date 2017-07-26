### Overview

有两种表示时间的基本方式。一种方式是用人能够理解的方式来表示时间，例如年月日，时分秒。另一种，称为机器时间，计算从一个起始点到现在持续的时间，称为纪元，通过以纳秒为单位来衡量。Date-Time 包提供了丰富的类来表示日期和时间。Date-Time API中的而一些类用来代表机器时间，而另一些类更适合以人类能够理解的方式来处理时间。


首先，你要决定你需要的是哪一方面的时间，其次，选取合适的类，最后用选取的类完成需求。当选取基于时间的类时，你首先需要决定使用human time or machine time。然后你需要确定你需要时间的哪一方面，是日年月日还是时分秒？需不需要时区？


** Terminology：本教程中，对于Date-Time API中用来操作日期和时间的类，例如Instant、LocalDateTime、Zone的DateTime等，被称为temporal-based classes。提供其他支持的类型，例如TemporalAdjuster接口、DayOfWeek枚举，不属于temporal-based classes。 **


例如，你可以使用LocalDate对象代表出生日期，因为绝大多数人都在同一天庆祝他们的生日，而不管他们身在出生地还是在其他地方。如果你关注的是占星术，你可能需要使用LocalDateTime对象来表示出生的日期和具体的时间。或者你可以使用Zone的DateTime，它还包含时区信息。如果你想创建一个时间戳，那么你可能需要使用Instant类，它可以让你比较两个时间点。


下方的表格列出了java.time包中用来操作日期和时间的 temporal-based classes。表格中的✔️号，表示一个类包含了特定的信息，“soString Output”一栏展示了使用类的toString方法的输出格式，"Where Discussed"一栏列出了相关教程的链接。



|Class or Enum|Year|Month|Day|Hours|Minutes|Seconds.|Zone Offset|Zone ID|toString output|Where Discussed|
|------------|----|----|----|----|----|----|----|----|--------------|-----------------------|
|Instant|✘|✘|✘|✘|✘|✘|✘|✘|2013-08-20T15:16:26.355Z|Instant Class|
|LocalDate|✔️|✔️|✔️|✘|✘|✘|✘|✘|2013-08-20|Date Classes|
|LocalDateTime|✔️|✔️|✔️|✔️|✔️|✔️|✘|✘|2013-08-20T15:16:26.355|Date and Time Class|
|ZonedDateTime|✔️|✔️|✔️|✔️|✔️|✔️|✔️|✔️|2013-08-20T15:16:26.355+09:00[Asia/Tokyo]|Time Zone and Offset Classes|
|LocalTime|✘|✘|✘|✔️|✔️|✔️|✘|✘|15:16:26.355|Date and time Classes|
|MonthDay|✘|✔️|✔️|✘|✘|✘|✘|✘|--08-20|Date Classes|
|Year|✔️|✘|✘|✘|✘|✘|✘|✘|2013|Date Classes|
|YearMonth|✔️|✔️|✘|✘|✘|✘|✘|✘|2013-08|Date Classes|
|Month|✘|✔️|✘|✘|✘|✘|✘|✘|AUGUST|Date Classes|
|OffsetDateTime|✔️|✔️|✔️|✔️|✔️|✔️|✔️|✘|2013-08-20T08:16:26.954-07:00|Time Zone and Offset Classes|
|OffsetTime|✘|✘|✘|✔️|✔️|✔️|✔️|✘|08:16:26.957-07:00|Time Zone and Offset Classes|
|Duration|✘|✘|..|..|..|✔️|✘|✘|PT20H(20 hours)|Period and Duration|
|Period|✔️|✔️|✔️|✘|✘|✘|...|...|P10D(10 days)|Period and Duration|


* . : Seconds以纳秒为单位

* .. : 不存储此信息，但是有方法可以获取此信息

* ... :  当向一个ZonedDateTime对象plus一个Period对象时，会考虑夏令时和其他的当地时间区别




























