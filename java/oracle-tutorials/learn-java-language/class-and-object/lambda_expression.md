## Lambda Expression

使用匿名类的一个问题是如果你的匿名类实现非常简单，例如实现了一个仅包含一个方法的接口，匿名类的语法看起来非常的笨拙而且不清晰。在这种情况下，你可以尝试将一个方法作为参数传递给另一个方法，比如当有人点击按钮时将会触发什么动作。Lambda表达式可以使你做到这些，将功能作为方法的参数，或者说将代码作为数据。

前面有关匿名类的章节，介绍了如何在不指定类名的情况下实现一个类。尽管匿名类通常比有名字的类更简洁，但是对于仅有一个方法的类来说，即使是匿名类仍然有些过于笨重。Lambda表达式可以使你更简洁的表达仅有一个方法的类的实例。

##### 适合使用Lambda Expressions的用例

假设你在创建䘝社交网络应用。你想要实现一个功能：允许管理员做一些操作，例如对某些满足一定条件的会员发公信息，下面的表格描述了这个用例：

|Field| Description|
|-----|------------|
|Name|对某些会员做一些操作|
|Actor|管理员|
|Preconditions|管理员登录系统|
|Postconditions|此操作只针对满足条件的会员|
|Main Success Scenario| 1. 管理员定义规则 2. 管理员定义操作 3. 管理员选择提交按钮 4. 系统找到所有满足规则的会员 5. 系统对这些会员执行定义的操作|
|Extensions|管理员可以在定义操作和提交之前预览满足条件的会员|
|Frequency of Occurrence| 一天允许多次操作|

假设社交网络应用的会员由下述的Person类代表:

```
public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;

    public int getAge() {
        // ...
    }

    public void printPerson() {
        // ...
    }
}

```

假设你的社交网络应用的会员存储在List<Persion>中。

本节首先使用最原始的方式来实现这个用例，随后它使用局部类和匿名类来升级代码，最后使用lambda表达式来有效且简洁的改进了用例。

##### 创建可以找到符合一定规则的会员的方法

方法1：一种简单的方式是创建几个方法，每个方法寻找符合一个特征的会员，例如满足性别和年龄特征。下面的方法可以打印出比规定年龄大的会员：

```
public static void printPersonsOlderThan(List<Person> roster, int age) {
    for (Person p : roster) {
        if (p.getAge() >= age) {
            p.printPerson();
        }
    }
}

```

** 一个List是一个有序的集合。一个集合是把一些元素组织到一个单元中的对象。集合被用来存储、检索、操作、联系一组数据。 **

使用这种方式的程序显得比较脆弱，当引入新特性时，代码就不适用了。假设你更新程序，改变了Person的结构，引入了其他的成员变量。这时候你不得不修改很多API来适应这种修改。除此之外，这种方式引入了不必要的限制，如果你想打印比规定年龄小的会员，你该怎么办？

方法2：创建更通用的搜索方法

下面的方法比printPersonsOlderThan方法更通用，它打印了在一定年龄范围内的成员：

```
public static void printPersonsWithinAgeRange(
    List<Person> roster, int low, int high) {
    for (Person p : roster) {
        if (low <= p.getAge() && p.getAge() < high) {
            p.printPerson();
        }
    }
}

```

如果你想打印指定性别且在一定年龄段的会员，该怎么办？如果你想修改Person类，加入其它的属性，比如个人情感信息或者地理位置信息，该怎么办？尽管这个方法比printPersonsOlderThan方法更通用，为每一个可能的搜索条件都创建一个方法仍然会产生很多破碎的代码。你可以将定义规则的代码剥离出去，写到一个单独的类中。

方式3：将规则代码分离到一个局部类中

```
public static void printPersons(List<Person> roster, CheckPerson tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}

```

```
interface CheckPerson {
    boolean test(Person p);
}

```

下面的类通过实现方法test，实现了CheckPerson接口，这个方法过滤出了在美国有选择权的会员：如果person的性别是male，且年龄在18到25之间，它返回true。

```
class CheckPersonEligibleForSelectiveService implements CheckPerson {
    public boolean test(Person p) {
        return p.gender == Person.Sex.MALE &&
            p.getAge() >= 18 &&
            p.getAge() <= 25;
    }
}

```

为了使用这个类你需要创建一个类的实例，并且调用printPersons方法：

```
printPersons(roster, new CheckPersonEligibleForSelectiveService());

```

尽管这个方法更健壮一些，但是如果你想改变Person类的结构，你也要重写方法--你必须写额外的代码：一个新接口和一个局部类用来搜寻满足条件的会员。因为CheckPersonEligibleForSelectiveService实现了一个接口，你可以为每一次方法调用传递一个匿名类对象。

方式4：在匿名类中定义搜寻规则

```
printPersons(roster, new CheckPerson() {
        public boolean test(Person p) {
            return p.getGender() == Person.Sex.MALE
                && p.getAge() >= 18
                && p.getAge() <= 25;
        }
    }
);

```

这种方式减少了代码量，因为你不必为一个新的规则创建一个新类。但是考虑到CheckPerson类仅有一个方法，匿名类的语法还是显得比较笨重。在这种情况下，你可以使用lambda表达式来取代匿名类。

方式5：使用lambda表达式来定义搜寻规则

CheckPerson是一个函数式接口。一个函数式接口是仅包含一个抽象方法的接口（函数式接口可能包括一个或多个默认方法或者静态方法）。因为一个函数式接口仅包括一个抽象方法，你可以忽略将要实现的此方法名。你可以使用lambda表达式来替代匿名类，示例如下：

```
printPersons(roster, (Person p) -> 
            p.getGender() == Person.Sex.MALE && p.getAge() >= 18 && p.getAge() <= 25 );


```

你可以使用标准的函数式接口取代CheckPerson接口，进一步减少代码量。

方式6：使用lambda表达式和标准函数式接口

考虑CheckPerson接口：

```
interface CheckPerson {
    boolean test(Person p);
}

```

这是一个非常简单的接口。它是一个函数式接口，因为它只包含一个抽象方法。这个方法接受一个参数，返回一个boolean值。这个方法很简单，简单到不值得定义。因此，JDK定义了一些标准的函数式接口，你可以在java.util.function包中找到这些接口。


例如，你可以使用Predicate<T>接口来替代CheckPerson接口，这个接口仅包含一个方法boolean test(T t),如下所示：

```
interface Predicate<T> {
    boolean test(T t);
}

```

Predicate<T>是一个泛型接口。泛型类使用尖括号<>定义了一个或多个类型参数。这个接口仅包括一个类型参数。当你使用真是类型来声明或者实例化一个泛型类时，你得到一个参数化类型。例如，参数化类型Predicate<Person>代码如下：

```
interface Predicate<Person> {
    boolean test(Person t);
}

```

这个参数化类型包含一个方法与CheckPerson接口的test方法具有同样返回值和参数的方法。因此，你可以使用Predicate<T>来取代CheckPerson，示例：

```
public static void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}

```

```
printPersonsWithPredicate(roster, p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25
);

```

这种凡是不是在你的方法中使用lambda表达式的唯一方式，下面提供了使用lambda表达式的其他方式。


方式7：让lambda表达式贯穿你的应用

重新考虑printPersonsWithPredicate方法，看你还可以在哪些地方使用lambda表达式：

```
public static void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester) {
    for (Person p : roster) {
        if (tester.test(p)) {
            p.printPerson();
        }
    }
}

```

这个方法检查了在roster List中每一个Person实例，看它们是不是满足tester规定的规则。如果这个person实例满足条件，则会调用此实例的printPersron方法。

如果不调用printPerson方法，你可以对每个符合规则的Person实例，定义一个其他的动作。你可以使用lambda表达式来定义这个动作。假设你想要一个类似于printPerson方法的lambda表达式，接口一个Person类型的参数，无返回值。记住，要想使用lambda表达式，你需要实现一个函数式接口。所以，你需要一个函数式接口，这个接口有一个接受Person类实例参数并且无返回值的的方法。Consumer<T>接口包含了一个方法 void accept(T t)，符合你的需求。下面的方法替换了Person类的printPerson方法，通过使用一个 Consumer<Person>类的实例，调用accept方法：

```
public static void processPersons(
    List<Person> roster,
    Predicate<Person> tester,
    Consumer<Person> block) {
        for (Person p : roster) {
            if (tester.test(p)) {
                block.accept(p);
            }
        }
}

```

```
processPersons(roster, 
	p -> p.getGender() == Person.Sex.MALE 
	&& p.getAge() >= 18 
	&& p.getAge() <= 25,
	p -> p.printPerson());

```

如果你想要对Person实例做更多的操作而不仅仅是打印信息，那怎么办？假如你想验证会员的个人资料或者检索他的联系方式。在这种情况下，你需要一个函数式接口，这个接口包括一个返回某个值的方法。函数式接口Function<T, R>包括一个方法R apply(T t)。下面的方法检索了由参数mapper指定的数据，然后执行一个由参数block指定的操作

```
public static void processPersonsWithFunction(
    List<Person> roster,
    Predicate<Person> tester,
    Function<Person, String> mapper,
    Consumer<String> block) {
    for (Person p : roster) {
        if (tester.test(p)) {
            String data = mapper.apply(p);
            block.accept(data);
        }
    }
}

```

下面的方法取出了每个Person实例的email地址信息：

```
processPersonsWithFunction(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);

```

方式8：更多的使用泛型

考虑processPersonsWithFunction方法，下面是这个方法的泛型版本：

```
public static <X, Y> void processElements(
    Iterable<X> source,
    Predicate<X> tester,
    Function <X, Y> mapper,
    Consumer<Y> block) {
    for (X p : source) {
        if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
        }
    }
}

```

为了打印符合条件的会员的e-mail地址信息，调用方法如下：

```
processElements(
    roster,
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25,
    p -> p.getEmailAddress(),
    email -> System.out.println(email)
);

```

上述的方法调用执行了如下的动作：

* 从集合中获取元素类型。在此例中，它从roster集合中获取了Person对象。注意roster集合，它是一个List类型，同样也是一个Iterable类型。
* 过滤出符合Predicate tester条件的对象。在此例中，Predicate对象是一个lambda表达式，定义了一个规则。
* 使用Function对象mapper将每一个过滤出来的会员对象，映射到一个值。此例中，Function对象是一个lambda表达式，其返回一个对象的email地址。
* 通过Consumer对象block对每一个映射的值执行一个动作。在此例中，Consumer对象是一个lambda表达式，打印了Function对象返回的每一个会员的email地址信息。

你可以使用聚合操作来替代所有这些动作。


方式9：使用接受lambda表达式作为参数的聚合操作

下面的代码使用聚合操作打印了符合规则的会员的email地址信息：

```
roster
    .stream()
    .filter(
        p -> p.getGender() == Person.Sex.MALE
            && p.getAge() >= 18
            && p.getAge() <= 25)
    .map(p -> p.getEmailAddress())
    .forEach(email -> System.out.println(email));


```

下方的表格将processElements方法的每个操作符映射到了相应的聚合操作：

| provessElements Action | Aggregate Operation |
|------------------------|---------------------|
|获得一个对象源| Stream<E> stream()|
|根据规则过滤对象|Stream<T> filter (Predicate<? super T> predicate)|
|根据Function 对象将一组对象映射为对应的值|<R> Stream<R> map(Function<? super T, ? extends R> ampper)|
|对每一个对象执行Consumer对象定义的动作|void forEach(Consumer<? super T> action)|


上述的filter、map和forEach操作是聚合操作。聚合操作处理来自一个stream的元素，而不是直接来自于集合的元素（这也是为什么代码示例中调用的第一个方法时stream方法）。一个stream是一个元素序列。跟集合不同，它不是一个存储元素的数据结构，stream会从一个源中以流水线的形式获取值，这个源可能是集合等。一个流水线是一个stream操作符序列，在这个例子中就是filter-map-forEach。初次之外，聚合操作接受的参数通常为lambda表达式，可以使你自定义操作。更多关于聚合操作的知识，可以参考聚合操作一章。

##### GUI应用中的lambda表达式

在一个图形用户界面程序中，为了处理各种事件，例如键盘事件、手表事件、滑动事件等，你需要创建事件处理器，这些处理器通常需要实现一个特定的接口。通常，事件处理器接口是函数式接口，它们通常只有一个方法。

在JavaFX实例代码HelloWorld.java中，你可以使用lambda表达式来替换匿名类。方法调用语句：btn.setOnAction指定了当你选中btn按钮的时候的动作。这个方法需要一个EventHandler<ActionEvent>类型的接口，接口EventHandler<ActionEvent>仅包含一个方法，void handle(T event)。这个接口是一个函数式接口，因此你可以使用下面的lambda表达式来替换匿名类：

```
 btn.setOnAction(new EventHandler<ActionEvent>() {

            @Override
            public void handle(ActionEvent event) {
                System.out.println("Hello World!");
            }
        });
        
  // 上述的匿名类可以用下面的lambda表达式来替换
  
  btn.setOnAction(event -> System.out.println("Hello World!"));

```

##### lambda表达式语法

一个lambda表达式由以下几部分组成：

* 一个在括号中的，以逗号分隔的形参列表：而且，在只有一个参数的情况下，你可以忽略小括号，例如下面的lambda表达式也是合法的：

```
p -> p.getGender() == Person.Sex.MALE && p.getAge >= 18 && p.getAge <= 25

```

* 箭头符号： ->
* 方法体：由一条单独的表达式或者一个语句块组成。如果你指定了一个单独的表达式，java会在运行时计算并返回这个表达式的值。或者，你可以使用一个return语句：

```
p -> { return p.getGender() == Person.Sex.MALE && p.getAge >= 18 && p.getAge <= 25; }

```

return语句不是表达式，在lambda表达式中，你必须将语句用{}包起来，但是，你不需要将一个返回void的方法调用用{}括起来。例如，下面是一个合法的lambda表达式：

```
email -> System.out.println(email);

```

请注意，一个lambda表达式很像一个方法声明，你可以将lambda表达式认为是一个匿名方法--没有名字的方法。下面的例子，Calculator，是一个接受多个参数的lambda表达式：

```
public class Calculator {
  
    interface IntegerMath {
        int operation(int a, int b);   
    }
  
    public int operateBinary(int a, int b, IntegerMath op) {
        return op.operation(a, b);
    }
 
    public static void main(String... args) {
    
        Calculator myApp = new Calculator();
        IntegerMath addition = (a, b) -> a + b;
        IntegerMath subtraction = (a, b) -> a - b;
        System.out.println("40 + 2 = " +
            myApp.operateBinary(40, 2, addition));
        System.out.println("20 - 10 = " +
            myApp.operateBinary(20, 10, subtraction));    
    }
}

```

##### 在闭包(Enclosing Scope)中访问局部变量

类似局部类和匿名类，lambda表达式可以俘获变量(capture variables)。它们可以跟外部范围一样访问局部变量。但是，与匿名类和局部类不同的是，lambda表达式没有任何隐蔽(Shadowing)问题。lambda表达式只是语法范围层面。这意味着它们不从任何父类中继承任何的name或者引入一个新的范围level。在lambda表达式中声明一个变量，就像它们是在外部范围被声明的一样，下面的实例阐述了这一点：

```
import java.util.function.Consumer;

public class LambdaScopeTest {

    public int x = 0;

    class FirstLevel {

        public int x = 1;

        void methodInFirstLevel(int x) {
            
            // The following statement causes the compiler to generate
            // the error "local variables referenced from a lambda expression
            // must be final or effectively final" in statement A:
            //
            // x = 99;
            
            Consumer<Integer> myConsumer = (y) -> 
            {
                System.out.println("x = " + x); // Statement A
                System.out.println("y = " + y);
                System.out.println("this.x = " + this.x);
                System.out.println("LambdaScopeTest.this.x = " +
                    LambdaScopeTest.this.x);
            };

            myConsumer.accept(x);

        }
    }
  public static void main(String... args) {
        LambdaScopeTest st = new LambdaScopeTest();
        LambdaScopeTest.FirstLevel fl = st.new FirstLevel();
        fl.methodInFirstLevel(23);
    }
}

```
上述代码的输出如下：

```
x = 23
y = 23
this.x = 1
LambdaScopeTest.this.x = 0

```

如果你将lambda表达式myConsumer的参数y换成参数x，如下所示，那么编译器会报错:"variable x is already defined in method methodInFirstLevel(int)"，因为lambda表达式不会引入一个新的范围级别。

```
Consumer<Integer> myConsumer = (x) -> {
    // ...
}

```

因此，你可以直接访问外部范围的域，方法和局部变量。例如，lambda表达式直接访问了方法methodInFirstLevel的参数x。为了访问外部类的变量，使用关键字this，在此例中，this.x指向FirstLevel类的成员变量x。

但是，类似于局部类和匿名类，lambda表达式只可以访问外部范围的那些为final或者effectively final的局部变量和参数。例如，假设你将下面的赋值语句加上时：

```
void methodInFirstLevel(int x) {
    x = 99; // local variables referenced from a lambda expression must be final or effectively final
    // ...
}

```

因为加了这个赋值语句，FirstLevel.x变量机不再是effectively final的了。因此，java编译器会产生如下的一条错误信息：“local variables referenced from a lambda expression must be final or effectively final”

##### 目标类型

你如何定义一个lambda表达式的类型？考虑如下的lambda表达式：

```
p -> p.getGender() == Person.Sex.MALE
    && p.getAge() >= 18
    && p.getAge() <= 25
    
```

这个lambda表达式在如下的连个方法中被使用：

* public static void printPersons(List<Person> roster, CheckPerson tester)
* public void printPersonsWithPredicate(List<Person> roster, Predicate<Person> tester)

当调用printPersons方法时，它期望一个CheckPerson的数据类型，所以lambda表达式是次类型。但是，当调用printPersonsWithPredicate方法时，它期望一个Predicate<Person>的数据类型，所以此时lambda表达式是此类型。方法期望的数据类型称为target type。为了判断lambda表达式的类型，java编译器使用lambda的上下文的target type。只有在java编译器可以判断出一个target type的情况下，你才可以使用lambda表达式：

* 变量声明
* 赋值
* 返回值
* 数组初始化
* 方法或者构造器参数
* lambda表达式实现体
* 条件语句?:
* 类型转换表达式

##### target type 和方法参数

对于方法参数，java编译器使用两种其他的语言特性来判断target type：重载解析(overload resolution)和类型参数推测(type argument inference)。

考虑如下的两个函数式接口：（java.lang.Runnable 和 java.util.concurrent.Callable<V>）

```
public interface Runnable {
    void run();
}

public interface Callable<V> {
    V call();
}

```

Runnable.run()方法没有返回值，而Callable<V>.call方法有返回值。

假设你有如下重载过的的invoke方法：

```
void invoke(Runnable r) {
    r.run();
}

<T> T invoke(Callable<T> c) {
    return c.call();
}

```

执行下面的语句，你将会调用哪个invoke方法：

```
String s = invoke(() -> "done");

```

invoke(Callable<T>)方法将会被执行，因此这个方法有返回值。invoke(RUnnable)没有返回值。在这种情况下，lambda表达式"() -> "done" "的类型为Callable<T>.

##### 序列化

你可以序列化一个表达式，如果它的target type和它capture的参数是可序列化的。但是，类似于内部类，序列化lambda表达式也是不推荐的一种做法。


##### 方法引用

你可以使用lambda表达式创建匿名方法。但是，有时一个lambda表达式只是简单的调用了一个已经存在的方法。在这些情况下，直接通过方法名来引用方法通常更清晰。你可以使用方法引用技术这样做，它们是更简洁、更易读的lambda表达式，对于已经有名字的方法来说。

考虑下面的Person类：

```
public class Person {

    public enum Sex {
        MALE, FEMALE
    }

    String name;
    LocalDate birthday;
    Sex gender;
    String emailAddress;

    public int getAge() {
        // ...
    }
    
    public Calendar getBirthday() {
        return birthday;
    }    

    public static int compareByAge(Person a, Person b) {
        return a.birthday.compareTo(b.birthday);
    }}


```

假设你的忘了应用程序的会员以Person数组的形式存在，你想将他们按年龄进行排序。你可以使用如下的代码：

```
Person[] rosterAsArray = roster.toArray(new Person[roster.size()]);

class PersonAgeComparator implements Comparator<Person> {
	public int compare(Person a, Person b) {
		return a.getBirthday().compareTo(b.getBirthday());
	}
}

Arrays.sort(rosterAsArray, new PersonAgeComparator());

```

调用的sort方法的签名如下：

```
static <T> void sort(T[] a, Comparator<? super T> c)

```

我们注意到Comparator接口是一个函数式接口，因此，我们可以使用lambda表达式，而不是定义一个类实现Comparator然后创建改实现类的一个实例：

```
Arrays.sort(rosterAsArray, (Person a, Person b) -> {return a.getBirthday().compareTo(b.getBirthday());});

```

但是，比较两个Person对象的birth dates的方法已经在Person类中实现：Person.compareByAge()，你可以在lambda表达式中调用这个方法：

```
Arrays.sort(rosterAsArray, (a, b) -> Person.compareByAge(a, b));

```

因为这个表达式调用了一个已经存在的方法，你可以使用方法调用来替换lambda表达式：

```
Arrays.sort(rosterAsArray, Person::compareByAge);

```

方法引用Person::compareByAge在语义上与lambda表达式(a, b) -> Person.compareByAge(a, b)是一致的。都有如下特征：

* 它们的形参列表是从Comparator<Person>.compare方法拷贝而来，是（Person, Person）
* 它们的实现体调用了方法Person.compareByAge

##### 方法引用的种类

|Kind| Example|
|----|---------|
|静态方法引用| ContainingClass::staticMethodName|
|实例方法引用| containingObject""instanceMethodName|
|特定类型的任意对象的实例方法引用|ContainingType::methodName|
|构造方法引用|ClassName:new|

* 静态方法引用，上述代码已有示例
* 实例方法引用，下面的代码是实例方法引用的示例：

```
class ComparisonProvider {
    public int compareByName(Person a, Person b) {
        return a.getName().compareTo(b.getName());
    }
        
    public int compareByAge(Person a, Person b) {
        return a.getBirthday().compareTo(b.getBirthday());
    }
}
ComparisonProvider myComparisonProvider = new ComparisonProvider();
Arrays.sort(rosterAsArray, myComparisonProvider::compareByAge);

```

方法引用myComparisonProvider::compareByAge调用了对象myComparisonProvider的compareByAge方法。JRE会推测方法参数的类型，此例中为(Person, Person)。

* 特定类型的任意对象的实例方法引用，下面为代码调用了String的compareToIgnoreCase方法，这个方法又：

```
String[] stringArray = { "Barbara", "James", "Mary", "John",
    "Patricia", "Robert", "Michael", "Linda" };
Arrays.sort(stringArray, String::compareToIgnoreCase);

```

关于此种类型的方法引用参考博客：http://moandjiezana.com/blog/2014/understanding-method-references/


##### 构造方法引用

你可以通过new关键字，使用同样的方式来引用一个构造器。下面的方法从一个集合中拷贝数据到另一个集合：

```
public static <T, SOURCE extends Collection<T>, DEST extends Collection<T>>
    DEST transferElements(
        SOURCE sourceCollection,
        Supplier<DEST> collectionFactory) {
        
        DEST result = collectionFactory.get();
        for (T t : sourceCollection) {
            result.add(t);
        }
        return result;
}

```

函数式接口Supplier包含一个无参方法get，这个方法返回一个对象。所以，你可以使用lambda表达式的方式调用transferElements方法：

```
Set<Person> rosterSetLambda =
    transferElements(roster, () -> { return new HashSet<>(); });

```

你也可以使用一个构造器引用替代上述的lambda表达式：

```
Set<Person> rosterSet = transferElements(roster, HashSet::new);

```

java编译器会推测你想创建一个HashSet集合，包含Person元素。或者你可以指定如下的具体格式：

```
Set<Person> rosterSet = transferElements(roster, HashSet<Person>::new);

```

## 什么时候使用嵌套类、局部类、匿名类、lambda表达式

在嵌套类一章中，曾经提到过，嵌套类可以使你在逻辑上组织一些类，这些类只会在一个地方使用。嵌套类可以增强封装性、创建更可读、易维护的代码。局部类、匿名类和lambda表达式也有这些优势，但是，它们适用于更具体的情况：

* 局部类：如果你需要创建多于一个实例，访问类的构造器或者引入一个新的、有名字的类型。
* 匿名内部类：如果你需要声明域或者额外的方法
* lambda表达式：
	* 如果你正在封装一个独立的行为单元，想要将这个行为传给其他代码。例如，当你想要在集合的每一个元素上执行特定的动作时、当一个处理过程完成时、当一个处理过程出错时，你可以使用lambda表达式。
	* 当你需要一个函数式接口的实例时。
	
* 嵌套类：如果你的需求跟局部类的需求相同，你想要使类型使用范围更广，并且你不需要访问局部变量或者方法参数。
	* 如果你需要访问外部类的实例的非公有域和方法，使用非静态嵌套类（既我们通常说的内部类）
	* 如果你不需要访问外部类的实例的非公有域和方法，使用静态嵌套类







































