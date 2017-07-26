### Period and Duration


当你需要表示一段时间时，Date-Time API提供了几个有用的类和方法： Duration、Period类和ChronoUnit.between()方法。一个Duration使用时间(秒、纳秒)来衡量一段时间，而一个Period使用日期(年月日)来衡量一段时间。


** 注意：一天的Duration就是24小时，而一天的Period，当有时区存在时，跟时间有关。 **


#### Duration

Duration适合用来衡量机器时间，例如处理Instant对象。一个Duration对象使用秒和纳秒来衡量时间，而不会根据年、月、日信息，尽管它也提供了转化为天、小时和分钟的方法：toDays、toHours、toMinutes。Duration可能有负值，如果创建它的endPoint小于startPoint的话。

```
Instant t1, t2;
...

long ns = Duration.between(t1, t2).toNanos();

```

```
Instant start;
...

Duration gap = Duration.ofSeconds(10);
Instant later =start.plus(gap);

```

一个Duration对象跟时间线不是一一对应的，因为它不包含时区和日光节约时间。往ZonedDateTime对象加上一天的Duration，它会精确的加24h，而不管时区和日光节约时间。


#### ChronoUnit


ChronoUnit枚举，在前面的章节也涉及过，定义了衡量时间的单元。当你想用某种单元来测试两个时间之间的间隔时，ChronoUnit.between()方法非常有用。between方法的参数是Temporal类型，所有它可以接收所有Temporal-based的对象，但是它只返回用一种时间单位衡量的数量，例如：

```
Instant previous, current, gap;
...

current = Instant.now();

if (previous != null) {
	gap = ChronoUnit.MILLIS.between(previous, current);
}

```


#### Period


如果想定义date-based的一段时间，请使用Period类。Period类提供了很多有用的方法，例如getMonths, getDays, getYears等。


Period表示的时间，使用months, days, years来表示，如果你只想使用一种单位，那么请使用ChronoUnit.between()方法。


下面的代码，表示了你的年龄，假设出生日期为1960.1.1. Period类可以用来衡量年、月、日，如果只想看总天数，那么你可以使用ChronoUnit.DAYS.between()方法：


```
LocalDate today = LocalDate.now();
LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

Period p = Period.between(birthday, today);
long p2 = ChronoUnit.DAYS.between(birthday, today);

System.out.println("You are " + p.getYears() + " years, " + p.getMonths() + " months, and " + p.getDays() + " day old. ( " + p2 + " days total)");


```


你可以使用下面的Birthday示例，来计算离你下一次生日还有多少时间。Period类用来计算月份和日期，ChronoUnit.between()方法可以来计算总天数：


```
LocalDate today = LocalDate.now();

LocalDate birthday = LocalDate.of(1960, Month.JANUARY, 1);

LocalDate nextBirDay = birthday.withYear(today.getYear);

if (nextBirDay.isBefore(today) || nextBirDay.isEqual(today)) {
	nextBirDay = nextBirDay.plusYears(1);
}

Period p = Period.between(today, nextBirDay);
long totalDays = ChronoUnit.DAYS.between(today, nextBirDay);

System.out.println("There are " + p.getMonths() + " months, and " +
                   p.getDays() + " days until your next birthday. (" +
                   totalDays + " total)");

```


上述代码没有考虑时区信息。如果你出生在澳大利亚，但是现在生活在班加罗尔，那么时区信息会影响你的生日日期。在这种情况下，可以联合使用ZonedDateTime和Period，当你给一个Period添加一个ZonedDateTime时，时区信息会被反应出来。
























