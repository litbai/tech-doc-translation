### The Temporal Package

java.time.temporal包提供了一些接口、类和枚举，用来支持date和time操作，尤其是时间和日期计算。


这些接口比较底层。通常来说，应用层面的代码不应该针对Temporal接口编程，而应该使用更高层的LocalDate或者ZonedDateTime这些类。这种情况就跟，你编程时应该使用String而不是CharSequece接口一样。


#### Temporal and TemopralAccessor


Temporal接口提供了用来访问temporal-based 对象的一个框架，所有的temporal-based 类都实现了这个接口，例如 Instant、LocalDateTime、ZonedDateTime。这个接口提供了用来加减时间的方法，使得基于时间的计算更加容易，并且在各个具体类中保持一致。TemporalAccessor接口仅仅提供了一些只读操作，Temporal接口扩展自TemporalAccessor接口。


Temporal对象由一些域来定义，这些域由TemporalField代表。ChronoField枚举实现了TemporalField接口，提供了很多预定义的常量，例如 DYA_OF_WEEK, MINUTE_OF_HOUR, MONTH_OF_YEAR等。


这个域的单位由TemporalUnit接口定义。ChronoUnit枚举实现了TemporalUnit接口。ChronoFiled.DAY_OF_WEEK是ChronoUnit.DAYS和ChronoUnit.WEEKS的组合。ChronoField和ChronoUnit枚举将在下一节介绍。


Temporal接口中定义的关于计算时间和日期的方法，需要传递TemporalAmount类型的参数。Period和Duration类实现了TemporalAmount接口。


#### ChronoField nad IsoFields


ChronoField枚举实现了TemporalField接口，提供了用来访问日期和时间的一系列常量，例如 CLOCK_HOUR_OF_DAY, NANO_OF_DAY, DAY_OF_YEAR. 这些常量可以用来在某些方面表达时间，例如一年的第三周、一天的第11小时，一个月的第一个Monday。当你遇到一个不知道具体类型的Temporal对象时，你可以使用TemporalAccessor.isSupported(TemporalField)方法来判断这个对象是否支持一个特定的域。下面的代码返回false，暗示着LocalDate不支持ChronoField.CLOCK_HOUR_OF_DAY。

```
boolean isSupported = LocalDate.now().isSupported(ChronoField.CLOCK_HOUR_OF_DAY);

```

#### ChronoUnit


ChronoUnit枚举实现了TemporalUnit接口，提供了一系列基于日期和时间的标准单元，从微秒到千年。注意到，并非所有的类都支持所有的单元。例如，Instant类不支持ChronoUnit.MONTHS或者ChronoUnit.YEARS. TemporalAccessor.isSupported(TemporalUnit)方法可以来判断一个Temporal对象是否支持指定的unit。

```
// true
boolean isSupported = instant.isSupported(ChronoUnit.DAYS);

```




























