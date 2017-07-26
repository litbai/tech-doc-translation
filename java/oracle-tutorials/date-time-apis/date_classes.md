### Date Classes

Date-Time API提供了4个类来专门的处理日期信息，而不处理时间和时区信息。在那种情况下应该使用哪个类，可以根据类名来做判断：LocalDate、YearMonth、MonthDay、Year。


#### LocalDate

一个LocalDate代表ISO日历系统中的年月日，不包括时间信息。你可以使用LocalDate来记录一个重大事件，例如生日或者婚礼日期。下面的示例，使用了LocalDate的of和with方法来创建了LocalDate示例：


```
LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
LocalDate nextWed = date.with(TemporalAdjusters.next(DayOfWeek.WEDNESDAY));

```

除了这些常见的方法，LocalDate类还提供了获取日期信息的getter。getDayOfWeek()方法会返回星期信息，例如：

```
DayOfWeek dotw = LocalDate.of(2012, Month.JULY, 9).getDayOfWeek();

```


下面的代码使用TemporalAdjuster来得到指定日期的下一个Wednesday：

```
LocalDate date = LocalDate.of(2000, Month.NOVEMBER, 20);
TemopralAdjuster adj = TemporalAdjusters.next(DayofWeek.WEDNESDAY);
LocalDate nextWed = date.with(adj);
Ssytem.out.println("For the date of %s, the next Wednesday is %s.%n", date, nextWed);

```

上述程序的输出为：

```
For the date of 2000-11-20, the next Wednesday is 2000-11-22.

```


#### YearMonth


YearMonth代表指定的年月。下面的示例，使用了lengthOfMonth方法来得到指定年月的每个月的天数：


```
YearMonth date = YearMonth.now();
System.out.printf("%s: %d%n", date, date.lengthOfMonth());

YearMonth date2 = YearMonth.of(2010, Month.FEBRUARY);
System.out.printf("%s: %d%n", date2, date2.lengthOfMonth());

YearMonth date3 = YearMonth.of(2012, Month.FEBRUARY);
System.out.printf("%s : %s%n", date3, date3.lengthOfMonth());

```

上述代码的输出如下：


```
2013-06: 30
2010-02: 28
2012-02: 29

```


#### MonthDay

MonthDay代表某个月的特定的一天，如下1月1日元旦：

下面的示例使用了MonthDay.isValidYear方法来判断，对于2010年，有没有2月29日，返回为false，说明2010年是平年而不是闰年：

```
MonthDay date = MonthDay.of(Month.FEBRUARY, 29);
boolean validLeapYear = date.isValidYear(2010);

```


#### Year

Year的实例表示某一年。下面的示例，使用Year.isLeap()方法来判断一个年份是否是闰年：

```
boolean validLeapYear = Year.of(2012).isLeap();

```






















