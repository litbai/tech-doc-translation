### Time Zone and Offset Classes


一个时区是地球上的一个区域，标准时间会使用这个概念。每一个时区由一个标识符标志，标识符格式通常为region/city(例如，Asia/Tokyo)，每个时区还会有一个相对于格林尼治时间的偏移量，例如，Tokyo的偏移量是+09:00.


#### ZoneId and ZoneOffset

Date-Time API提供了两个类来定义时区和偏移量：

* ZoneId：指定了时区标识符，并且提供了规则用来在Instant和LocalDateTime之间转换
* ZoneOffset：指定了一个相对于Greenwich/UTC时间的偏移量


相对于Greenwich/UTC时间的偏移量通常是整小时数，但是也有例外。下面的代码，将打印出偏移量不是整小时数的时区信息：


```
Set<String> allZones = ZoneId.getAvailableZoneIds();
LocalDateTime dt = LocalDateTime.now();

List<String> zoneList = new ArrayList<>(allZones);
Collections.sort(zoneList);

...

for (String s : zoneList) {
	ZoneId zone = ZoneId.of(s);
	ZonedDateTime zdt = dt.atZone(zone);
	ZoneOffset offset = zdt.getOffset();
	int secondsOfHour = offset.getTotalSeconds() % 3600;
	
	if (secondsOfHour != 0) {
		System.out.printf("%35s %10s%n", zone, offset);
	}
}

```


#### The Date-TIme Classes

Date-Time API 提供了3个temporal-based Classes来处理Zone：

* ZonedDateTime: date + time + zone
* OffsetDateTime: date + time + offset, 但是不包含time zone ID
* OffsetTime: time + offset, 但是不包含time zone ID


那么什么时候使用OffsetDateTime，什么时候使用ZoneDateTime。如果你正在写一个复杂的软件系统，你需要自定义计算时间和日期的规则，或者你需要往DB中存储一个时间戳，仅仅需要知道相对于格林尼治时间的绝对偏移量，那么你可能更应该使用OffsetDateTime。

尽管上述的3个类都维持了一个相对于格林尼治时间的偏移量，但是，只有ZoneDateTime类遵循了ZoneRules，ZoneRules定义了每个标准时区相对于格林尼治时间的偏移量。例如，在很多时区，为了节约能源，会有日光节约时间这个概念。在夏天的某段时间内，会将时间往前调整1小时，在结束时再将时间调回来，ZonedDateTime可以处理这种情况。我国实行过5年的日光节约时间规则，后来废弃了，因为有利有弊。


#### ZonedDateTime


ZonedDateTime类，实际上，是结合了LocalDateTime和ZoneId。它代表一个date + time ，并且with Zone。


下面的代码，使用ZonedDateTime来定义了从圣弗朗西斯科到东京的航班的起飞时间，在圣弗朗西斯科，使用的是America/Los_Angeles的时区。创建ZonedDateTime实例的时候，使用了withZoneSameInstant和plusMinutes方法，代表了650分钟后到达日本东京的时间。ZoneRules.isDayLightSavings方法可以来判断一个时区是否使用了日光节约时间。


你可以使用一个DateTimeFormatter对象来格式化ZonedDateTime实例：

```
DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM d yyyy hh:mm a");

LocalDateTime leaving = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
ZoneId  leavingZone = ZoneId.of("America/Los_Angeles");
ZonedDateTime departure = ZonedDateTime.of(leaving, leavingZone);

try {
	String out1 = departure.format(format);
	System.out.printf("LEAVING: %s (%s)%n", out1, leavingZone);
} catch (DateTimeException ex) {
	System.out.printf("%s can't be formatted!%n", departure);
	throw ex;
}

ZoneId = arrivingZone = ZoneId.of("Asia/Tokyo");
ZonedDateTime arrival = departure.withZoneSameInstant(arrivingZone).plusMinutes(650);

try {
	String out2 = arrival.format(format);
	System.out.printf("ARRIVING: %s (%s)%n", out2, arrivingZone);
} catch (DateTimeException ex) {
	System.out.printf("%s can't be formatted!%n", arrival);
	throw ex;
}

if (arrivingZone.getRules().isDaylightSaving(arrival.toInstant())){
	System.out.printf("	(%s daylight saving time will be in effect.)%n", arrivingZone);
} else {
	System.out.printf(" (%s standard time wil be in effect.)%n", arrivingZone);
}

```

上述代码的输出为：

```
LEAVING: Jul 20 2013  07:30 PM (America/Los_Angeles)
ARRIVING: Jul 21 2013 10:20 PM (Asia/Tokyo)
	(Asia/Tokyo standard time will be in effect.)

```


#### OffsetDateTime


OffsetDateTime实际上是LocalDateTime和ZoneOffset的结合体。它用来代表date + time + offset from Greenwich/UTC time(+/-hours:minutes, such as +06:00 or -08:00).


下面的示例代码使用了OffsetDateTime和TemporalAdjuster.lastDay()方法来找到2013年7月的最后一个星期四。

```
LocalDateTime localDate = LocalDateTime.of(2013, Month.JULY, 20, 19, 30);
ZoneOffset offset = ZoneOffset.of("-08:00");

OffsetDateTime offsetDate = OffsetDateTime.of(localDate, offset);
offsetDateTime lastThursday = offsetDate.with(TemporalAdjusters.lastInMonth(DayOfWeek.THURSDAY));

System.out.printf("The last Thursday in July 2013 is the %sth.%n", lastThursday.getDayOfMonth());

```

上述代码的输出为(这个例子并没有说明offset的作用。。)：

```
The last Thursday in July 2013 is the 25th.

```


#### OffsetTime

OffsetTime类，实际上，结合了LocalTime和ZoneOffset。使用OffsetTime的场景和使用OffsetDateTime的场景类似。



















