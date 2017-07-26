### Temporal Adjuster


TemporalAdjuster接口，定义在java.time.temporal包中，仅提供了一个方法，是一个函数式接口，方法定义如下：

```
 Temporal adjustInto(Temporal temporal);

```

这个方法接收一个Temporal类型的参数，返回一个调整后的值。


#### Predefined Adjusters

TemporalAdjusters类提供了很多预定义的adjusters，这些adjusters包括找到一个月的第一天和最后一天、一年的第一天和最后一天、一个月的最后一个周三、一个指定日期之后的第一个周二等等。这个预先定义的adjusters被定义为静态方法，期望使用static import来进行静态导入。


下面的代码示例使用了几个TemporalAdjusters中的方法，通过与with方法结合，基于原始日期构建了新日期：

```
LocalDate date = LocalDate.of(2010, Month.OCTOBER, 15);
DayOfWeek dotw = date.getDayOfWeek();

System.out.printf("%s is on a %s%n", date, dotw);

System.out.printf("first day of Month: %s%n", date.with(TemporalAdjusters.firstDayOfMonth()));

System.out.printf("first Monday of Month: %s%n", date.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY)));

System.out.printf("last day of month: %s%n", date.with(TemporalAdjusters.lastDayOfMonth()));

System.out.printf("first day of next month: %s%n", date.with(TemporalAdjusters.firstDayOfNextMonth()));

System.out.printf("first day of next year: %s%n", date.with(TemporalAdjusters.firstDayOfNextYear()));

System.out.printf("first day of Year: %s%n", date.with(TemporalAdjusters.firstDayOfYear()));

```


上述程序的输出为： 

```
2017-07-24 is on a MONDAY
first day of Month: 2017-07-01
first Monday of Month: 2017-07-03
last day of month: 2017-07-31
first day of next month: 2017-08-01
first day of next year: 2018-01-01
first day of Year: 2017-01-01

```


#### Custom Adjusters

你可以创建自定义的adjuster。为了创建自定义的adjuster，你需要实现TemporalAdjuster接口。下面的PayDayAdjuster类就是自定义的adjuster。PayDayAdjuster接收一个日期，返回下一次的薪水支付日期。假设薪水支付为一个月两次：每个月的15号和最后一号，如果上述算出的日期为周末，则使用上周五为支付日期：


```
public Temporal adjustInto(Temporal input) {
	LocalDate date = LocalDate.from(input);
	int day;
	if (date.getDayOfMonth() < 15) {
		day = 15;
	} else {
		day = date.with(TempraolAdjusters.lastDayOfMonth()).getDayOfMonth();
	}
	
	date = date.withDayOfMonth(day);
	
	if (date.getDayOfWeek() == DayOfWeek.SATURDAY 
	|| date.getDayOfWeek() == DayOfWeek.SUNDAY) {
		date = date.with(TemporalAdjusters.previous(DayOfWeek.FRIDAY));
	}
	return input.with(date);
}

```

```
LocalDate nextPayDay = date.with(new PaydayAdjuster());

```


2013年的6月15号和6月30号都是周末，所以，分别输入6月3号和18号，返回结果为：


```
Given the date:  2013 Jun 3
the next payday: 2013 Jun 14

Given the date:  2013 Jun 18
the next payday: 2013 Jun 28

```





























