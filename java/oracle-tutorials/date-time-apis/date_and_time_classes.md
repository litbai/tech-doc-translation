### Date and Time Classes

#### LocalTime

LocalTime类跟其他前缀是Local的类一样，但是它只处理时间(时分秒纳秒)。这个类在表示人类能够理解的时间时，很有用。例如：电影开始时间、图书馆的开放时间等。它也可以被用来创建一个数字时钟，示例如下：

```
LocalTime thisSec;

for(;;) {
	thisSec = LocalTime.now();
	
	// 具体实现看应用场景
	display(thisSec.getHour(), thisSec.getMonute(), thisSec.getSecond());
}

```

LocalTime没有存储关于时区和日光节约时间的信息。


#### LocalDateTime

LocalDateTime能同时处理日期和时间信息，它不包括时区信息，是Date-Time API的核心类之一。这个类用来表示日期（month-day-year）和时间（hour-monute-second-nanosecond），实际上，它结合了LocalDate和localTime。这个类可以用来表示一个特殊的事件，比如NBA总决赛第一场开始时间为2017年6月08号，22：30分。注意，这个22：30分指的是当地时间，即勇士队主场金州的时间，而此时中国北京时间可能为6月09号上午11:30。如果你想加入时区信息，那么你需要使用Zone的DateTime类或者OffsetDateTime类。


除了各个基于时间的类都会提供的now方法之外，LocalDateTime类还有很多其他的方法来创建实例。from()方法可以将另一个temporal-based实例转化为LocalDateTime的实例。还有很多用于加减日期和时间的方法，下面展示了其中一些方法的使用示例：

```

System.out.printf("now: %s%n", LocalDateTime.now());

System.out.printf("Apr 15,1994 @ 11:30am: %s%n", LocalDateTime.of(1994, Month.APRIL, 15, 11, 30));

System.out.printf("now(from instant): %s%n", LocalDateTime.ofInstant(Instant.now(), ZoneId.systemDefault()));

System.out.printf("6 month from now: %s%n", LocalDateTime.now().plusMonths(6));

System.out.printf("6 months ago: %s%n", LocalDateTime.now().minusMonths(6));

```

上述代码的输出为：

```
now: 2013-07-24T17:13:59.985
Apr 15, 1994 @ 11:30am: 1994-04-15T11:30
now (from Instant): 2013-07-24T17:14:00.479
6 months from now: 2014-01-24T17:14:00.480
6 months ago: 2013-01-24T17:14:00.481


```














