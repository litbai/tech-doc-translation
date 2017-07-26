### Parsing and Formatting

Date-Time API中temporal-based classes提供了parse方法，来将一个string类型的参数，转换为对应的temporal-based class。这些类还提供了一个format方法，可以进行格式化的显示。在这两种情况下，处理过程是类似的：你向DateTimeFormatter提供一个pattern，使用DateTimeFormatter的工厂方法来创建一个实例，然后将这个实例传递给parse或者format方法。


DateTimeFormatter类提供了一些预定义的formatter，你也可以创建自己的formatter。


如果在转换过程中出现了问题，parse和format方法会抛出异常。因此，你的parse相关的代码应该捕获DateTimeParseException，format相关的代码应该捕获DateTimeException。

DateTimeFormatter类是不可变的，因此是线程安全的。你可以(并且总是应该)将一个formatter声明为一个static final的常量。


#### Parsing

LocalDate类中接收单个参数的parse方法，会使用默认的ISO_LOCAL_DATE formatter。如果你想自己制定formatter，那么你需要使用两个参数的方法：parse(CharSequence, DateTimeFormatter)。下面的代码使用了预先定义的BASIC_ISO_DATE formatter，它会使用19590709来表示1959年7月9日：

```
String in = "19590709";
LocalDate date = LocalDate.parse(in, DateTimeFormatter.BASIC_ISO_DATE);

```


你可以定义自己的formatter。下面的代码示例，自定义了formatter， 其pattern为"MMM d yyyy"。这个pattern指定了3位来表示月份，1位来表示天数，4位来表示年份。这个formatter可以正确的识别 “Jan 2 2003” 或者"Mar 23 1994"。但是，如果你指定的pattern为“MMM dd yyyy”，那么你就必须保证日期为两位数，如果小于10，则必须补0，例如"JUn 03 2003";


```
String input = ...;
try {
	DateTimeFormatter formatter = DateTimeFormatter.ofPattern("MMM d yyyy");
	LocalDate date = LocalDate.parse(input, formatter);
	System.out.printf("%s%n", date);
} catch (DateTimeParseException e) {
	 System.out.printf("%s is not parsable!%n", input);
    throw e;      // Rethrow the exception.
}

// ’date‘ has been successfully parsed

```


#### Formatting


format(DateTimeFormatter)方法，根据传入的formatter，将一个temporal-based对象转化为一个字符串表示。下面的代码，将一个ZonedDateTime，转化为"MMM d yyy hh:mm a"的格式。


```
ZoneId leavingZone = ...;
ZonedDateTime departure = ...;

try {
    DateTimeFormatter format = DateTimeFormatter.ofPattern("MMM d yyyy  hh:mm a");
    String out = departure.format(format);
    System.out.printf("LEAVING:  %s (%s)%n", out, leavingZone);
}
catch (DateTimeException exc) {
    System.out.printf("%s can't be formatted!%n", departure);
    throw exc;
}


```


































