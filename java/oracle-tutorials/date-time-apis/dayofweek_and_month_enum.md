### DayOfWeek and Month Enum

Date-Time API提供了一周7天和一年12个月的枚举类。


#### DayOfWeek

DayOfWeek枚举有7个常量，对应一周之内的7天，从MONDAY到SUNDAY。MONDAY到SUNDAY的数值从1到7，使用常量使你的代码可读性更佳。


DayOfWeek枚举类同时提供了很多方法。例如，下面的代码给Monday加上了3天，输出结果"THURSDAY":

```
System.out.println("%s%n", DayOfWeek.MONDAY.plus(3));

```

通过getDisplayName(TextStyle, Locale)方法，你可以得到表示week信息的String。TextStyle枚举可以让你指定需要的格式： FULL、NARROW(通常只有一个字母)或者SHORT(缩写)。示例如下：


```
DayOfWeek dow = DayOfWeek.MONDAY;
Locale locale = Locale.getDefault();

System.out.println(dow.getDisplayName(TextStyle.FULL, locale));
System.out.println(dow.getDisplayName(TextStyle.NARROW, locale));
System.out.println(dow.getDisplayName(TextStyle.SHORT, locale));

```

在英文环境下，输出如下：

```
Monday
M
Mon

```

在中文环境下，输出如下：

```
星期一
一
星期一

```


#### Month

Month枚举包含12个月份，从JANUARY到DECEMBER。就像DayOfWeek一样，Month枚举是强类型，各个枚举的值从1(JANUARY)到12(DECEMBER)。使用枚举，可以让你的代码可读性更强。


Month枚举也有一个getDisplayName(TextStyle, Locale)方法，示例如下：

```
Month month = Month.AUGUST;
Locale locale = Locale.getDefault();
System.out.println(month.getDisplayName(TextStyle.FULL, locale));
System.out.println(month.getDisplayName(TextStyle.NARROW, locale));
System.out.println(month.getDisplayName(TextStyle.SHORT, locale));

```

在英文的环境下，输出如下：

```
August
A
Aug

```

在中文环境下：

```
七月
7
七月

```

貌似中文格式不太好预测。。






















