### Instant Class

Date-Time API的另一个核心类是Instant类，它代表了时间轴上的一个点，可以具体到纳秒。这个类可以用来生成时间戳，代表机器时间。

```
Instant timestamp = Instant.now();

```

Instant的实例会计算自从1970年1月1日开始至指定日期走过的时间总数，1970年1月1日又称为EPOCH。发生在EPOCH之前的Instant，数值为负，发生在EPOCH之后的Instant，数值为正。


Instant还提供了两个常量，MIN代表最小的Instant， MAX代表最大的Instant。


Instant的toString方法会返回如下格式,这个格式遵循ISO-8601标准：


```
2017-07-23T00:58:42.355Z

```


Instant类提供了很多方法来对Instant实例进行操作。例如下面的plus和minus方法可以用来增加和减少指定的时间：

```
Instant oneHourLater = Instant.now().plus(1, ChronoUnit.HOURS);

```


isBefore和isAfter方法可以用来比较两个Instant。until方法返回两个Instant直接相差的时间，如下所示：

```
Long secondsFromEpoch = Instant.ofEpochSecond(0L).until(Instant.now(), ChronoUnit.SECONDS);

```

Instant类不适合用来处理人类容易理解的时间，比如年月日等。如果你想对这些单元进行计算，你可以将Instant对象转换为LocalDateTime或者ZonedDateTime对象，然后处理这些单元。下面的代码将一个Instant对象转换为一个LocalDateTime对象，使用了LocalDateTime的ofInstant方法和默认的时区，然后以更易读的方式打印出了日期和时间：

```
Instant now = Instant.now();
LocalDateTime ldt = LocalDateTime.ofInstant(now, ZoneId.systemDefault);
System.out.printf("%s %d %d at %d:%d%n", ldt.getMonth(), ldt.getDayOfMonth(), ldt.getYear(), ldt.getHour(), ldt.getMinute());

```


代码输出如下：

```
JULY 23 2017 at 9:16

```

ZonedDateTime 和 OffsetDateTime都可以转换为Instant对象，但是反过来没有那么简单。为了将一个Instant对象转换为一个ZonedDateTime或者OffsetDateTime对象，你需要提供一个时区信息或者偏移量信息。






















