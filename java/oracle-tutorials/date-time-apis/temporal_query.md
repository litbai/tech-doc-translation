### Temporal Query

可以使用TemporalQuery对象来检索一个temporal-based对象的信息。

#### Predefined Queries

TemporalQueries类提供了一些预先定义的query，当应用不知道具体的temporal-based类型的时候也可以使用的一些方法。跟adjusters一样，预先定义的query也推荐使用static import 。


预先定义的query--precision，会返回一个具体的temporal-based 对象支持的最小ChronoUnit，下面是示例：

```
TemporalQueries query = TemporalQueries.precision();

System.out.printf("LocalDate precision is %s%n", LocalDate.now().query(query));

System.out.printf("LocalDateTime precision is %s%n", LocalDateTime.now().query(query));

System.out.printf("Year precision is %s%n", Year.now().query(query));

System.out.printf("YearMonth precision is %s%n", YearMonth.now().query(query));

System.out.printf("Instant precision is %s%n", Instant.now().query(query));

```


输出如下：


```
LocalDate precision is Days
LocalDateTime precision is Nanos
Year precision is Years
YearMonth precision is Months
Instant precision is Nanos

```


#### Custom Queries

你可以创建自定义的queries。为了创建自定义的query，你需要实现TemporalQuery接口，实现里面的queryFrom(TemporalAccessor)方法。下面的CheckDate示例，实现了两个自定义的query。


```
// return true if the passed-in date occurs during one of the family vacations. Because the query compares the month and day only, the check succeeds even if Temporal types are not the same.

public Boolean queryFrom(TemporalAccessor date) {
	int month = date.get(ChronoField.MONTH_OF_YEAR);
	int day = date.get(ChronoField.DAY_OF_MONTH);
	
	if (month == Month.APRIL.getValue() && day >=3 and day <= 8) {
		return Boolean.TRUE;
	}
	
	if (month == Month.AUGUST.getValue() && day >= 8 && day <=14 ) {
		return Boolean.TRUE;
	}
	
	return Boolean.FALSE;
}


```

```
public static Boolean isFamilyBirthday(TemporalAccessor date) {
	int month = date.get(ChronoField.MONTH_OF_YEAR);
	int day   = date.get(ChronoField.DAY_OF_MONTH);
	
	 if ((month == Month.APRIL.getValue()) && (day == 3))
        return Boolean.TRUE;
    
    if ((month == Month.JUNE.getValue()) && (day == 18))
        return Boolean.TRUE;
        
     if ((month == Month.MAY.getValue()) && (day == 29))
        return Boolean.TRUE;

    return Boolean.FALSE;   

}

```


因为TemporalAccessor是一个函数式接口，所以你可以使用lambda表达式，而不用自己写一个class：


```
Boolean isFamilyVacation = date.query(new FamilyVacations());

Boolean isFamilyBirthday = date.query(FamilyBirthday::isFamilyBirthday);


if (ifFamilyVacation.booleanValue() || isFamilyBirthday.booleanValue()) {
	System.out.printf("%s is an important date!%n", date);
} else {
	System.out.printf("%s is not an important date.%n", date);
}

```








































