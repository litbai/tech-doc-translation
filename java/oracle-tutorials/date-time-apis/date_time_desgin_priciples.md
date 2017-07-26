### Date-Time Design Principles

Date-Time API遵循如下的几个设置原则：


#### Clear

API中的方法名都是仔细斟酌过的，恰如其分的表达了这个方法的行为。

#### Fluent

Date-Time API提供了一个流畅的接口，使得代码更易读。因为绝大多数的方法不允许null参数，不返回null值，所以多个方法可以进行链式调用，结果也是很容易理解，例如：

```
LocalDate today = LocalDate.now();
LocalDate payday= today.with(TemporalAdjusters.lastDayOfMonth()).minusDays(2);


#### Immuable

Date-Time API中的大多数类都是不可变类，意味着，一旦创建了一个对象，这个对象就不能被修改。为了修改一个不可变对象，你只能创建一个新对象。这也意味着，Date-Time API是线程安全的。这也是API中大量用来创建对象的方法名前缀都有of from with的原因。没有构造方法、没有setter，例如：


```
LocalDate dateOfBirth = LocalDate.of(2012, Month.MAY, 14);
LocalDate dirstBirthday = dateOfBirth.plusYears(1);

```

#### Extensive


Date-Time API是可扩展的。例如，你可以定义自己的adjusters和queries，你甚至可以构建自己的日历系统。















```