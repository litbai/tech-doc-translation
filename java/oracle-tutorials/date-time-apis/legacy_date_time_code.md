### Legacy Date-Time Code


在Java SE 8发布之前，java的Date和Time机制，是由java.util.Date、java.util.Calendar和java.util.TimeZone类以及它们的子类来实现的，例如java.util.GregorianCalendar，这些类有如下的一些缺点：

* Calendar 类不是类型安全的
* 因为类是可变的，所以不是线程安全的
* 应用程序的bug的很常见，因为缺少类型安全以及month的个数为11，而不是12


#### Interoperability with Legacy Code


可能你的应用中有使用java.util.date和time的代码，你想在改动不大的情况下，可以利用java.time包提供的各种特性。


JDK8中增加了在java.util和java.time包中的对象之间转换的方法：

* Calendar.toInstant() 将一个calendar对象转换为一个Instant
* GregorianCalendar.toZonedDateTime() 将一个GregorianCalendar对象转换为一个ZonedDateTime
* GregorianCalendar.from(ZonedDateTime) 根据ZonedDateTime对象创建一个GregorianCalendar对象
* Date.from(Instant) 根据Instant对象创建Date对象
* Date.toInstant() 将一个Date对象转换为一个Instant对象
* TimeZone.toZoneId() 将一个TimeZone对象转换为一个ZoneId


下面的示例将Calendar示例转换为一个ZonedDateTime实例。注意，当将一个Instant转换为ZonedDateTime时，你必须提供一个时区：

```
Calendar now = Calendar.getInstance();
ZonedDateTime zdt = ZonedDateTime.ofInstant(now.toInstant(), ZoneId.systemDefault());

```

```
Instant inst = date.toInstant();
Date newDate = Date.from(inst);

```

```
GregorianCalendar cal = ...;

TimeZone tz = cal.getTimeZone();
int tzoffset = cal.get(Calendar.ZONE_OFFSET);

ZonedDateTime zdt = cal.toZonedDateTime();

GregorianCalendar newCal = GregorianCalendar.from(zdt);

LocalDateTime ldt = zdt.toLocalDateTime();

LocalDate date = zdt.toLocalDate();
localTime time = zdt.toLocalTime();

```


#### Mapping java.util.Date and Time Functionality to java.time


因为Date和Time类在JDK8被完全重新设计了，所以你不能仅通过简单的换一换调用方法就可以利用java.time包的新特性。如果你想利用java.time包提供的新特性，最简单的方法是使用toInstant和toZonedDateTime方法来进行转换。但是，如果你不想使用上述方法或者仅使用上述方法无法满足你的需求，那么你必须重写你的代码了。


Overview一节中列出的table是概览java.time设计风格的一个很好的入口点。


在java.util.Date API 和 java.time.LocalDateTime等新的API之间没有一一对应的方法，但是下面的表格还是列出了在功能上可以一一对应的方法：


|java.util Functionality|java.time Functionality|Comments|
|--------------------|-------------------|------------------|
|java.util.Date|java.time.Instant|Instant和Date类相似。它们都代表时间轴上的一个点，跟时区无关，代表epoch-seconds（since 1970.01.01 T00:00:00） Date.from(Instant) 和Date.toInstant()方法可以将二者转换|
|java.util.GergorianCalendar|java.time.ZonedDateTime|ZonedDateTime类被设计用来替代GergorianCalendar，它们提供了类似的特性。使用GregrianCalendar.from(ZonedDateTime)和GregorianCalendar.to()方法可以将二者转换|
|java.util.TimeZone|java.time.ZoneId、java.time.ZoneOffset|ZoneId类有一个时区标识符和这个时区的rules。ZoneOffset可以指定一个与Greenwich/UTC的偏移量，但是没有时区信息|
|GregorianCalendar with the date 1970.01.01|java.time.LocalTime|只使用时分秒信息，可以使用LocalTime类|
|GregorianCalendar with time set to 00:00|java.time.LocalDate|只是用年月日信息，可以使用LocalDate类|



#### Date and Time Formatting


java.time.format.DateTimeFormatter类提供了强大的功能，让你可以格式化日期格式。但是，在新的Temporal-based 类中，你仍然可以使用java.util.Formatter和String.format.










































