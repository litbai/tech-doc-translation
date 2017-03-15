## Lession: Interface and Inheritance


* Interfaces

	前面的教程中已经见过实现接口的例子。在本章，你可以学习到关于接口的更多知识--接口是干什么的？为什么你需要写接口？怎么定义接口？
	
* Inheritance

	这一节描述了你可以从一个类派生出另一个类的实现方式，即一个子类如何从一个父类中继承域和方法。你将会知道所有的类都从Object类衍生而来、子类如何改变从父类中继承的方法。这一节同样会设计类似于接口的抽象类。
	
	
### 接口

对于不同的程序员小组来说，很多情况下，达成一个统一的契约是十分重要的，这份契约规定了他们的软件会如何工作。每一个小组应该可以编写自己的代码，而无需知道其他小组编写的代码细节。通常来说，接口就是这样一种契约。

例如，想象一个未来社会，由电脑控制的自动驾驶汽车，可以在城市街道载送乘客，而无需认工操作。自动驾驶汽车制造商编写可以执行自动驾驶操作的软件--停车、启动、加速、左转等等。另一个制造商，是电子导航制造商，制作计算机系统，这个系统可以接受GPS数据，无线传送交通状况，然后使用这些信息来驾驶汽车。

汽车制造商必须发布一个符合工业标准的接口，此接口描述了调用哪些方法可以使汽车（此制造商制造的任何汽车）移动。导航系统制造商，可以编写软件，调用汽车制造商提供的接口中描述的方法，来操作汽车。双方都无需知道另一方的具体实现细节。事实上，每一方都拥有他们软件的专利权，保留随时修改软件的权利，只要继续遵守原先发布的接口即可。


#### java中的接口

在java编程语言中，一个接口是一个引用类型，跟类相似，但是它只可以包含常量、方法签名、默认方法、静态方法和嵌套类。只有default和static方法可以有实现体。接口不能被实例化，它们可以被类实现或者被其他接口继承。定义一个接口，类似于定义一个类：

```
public interface OperateCar {
	// constant declarations, id any
	
	// method signatures
	
	// An enum with values RIGHT, LEFT
   int turn(Direction direction, double radius, double startSpeed, double endSpeed);
   int changeLanes(Direction direction, double startSpeed, double endSpeed);
   int signalTurn(Direction direction, boolean signalOn);
   int getRadarFront(double distanceToCar, double speedOfCar);
   int getRadarRear(double distanceToCar, double speedOfCar);
         ......
   // more method signatures
}

```

注意，方法签名没有实现体，并且已分号结尾。

如果想使用一个接口，你需要编写一个类实现这个接口。当一个可实例化的类实现了接口的时候，它需要给接口中定义的所有方法提供实现体，例如：

```
public class OperateBMW760i implements OperateCar {

    // the OperateCar method signatures, with implementation --
    // for example:
    int signalTurn(Direction direction, boolean signalOn) {
       // code to turn BMW's LEFT turn indicator lights on
       // code to turn BMW's LEFT turn indicator lights off
       // code to turn BMW's RIGHT turn indicator lights on
       // code to turn BMW's RIGHT turn indicator lights off
    }

    // other members, as needed -- for example, helper classes not 
    // visible to clients of the interface
}


```

在前面的自动驾驶汽车的例子中，汽车生产商将会实现接口。雪佛兰和丰田的实现当然不同，但是它们需要遵守同一个接口。导航系统生产商，是这个接口的客户端，将会创建一个使用GPS数据、数字地图和交通数据来驾驶汽车的系统。导航系统生产商将会调用接口的方法来trun、changeLanes、break、accelerate等。

#### 将接口作为API

上述的自动驾驶汽车的例子，展现了一个被作为一个工业标准的API的接口。API在商业软件产品中也很常见。通常，一个公司会卖一个软件包给另一家想要使用这个软件包的公司，这个软件包中包含各种复杂的方法。举个例子，比如一个数字图像处理的软件包可能会被卖给制作面向用户的图形程序的公司，比如美图。图像处理公司编写自己的类来实现一个接口，这个接口将会公开给它的客户。然后它的客户，将会根据方法签名来调用图象处理方法，并得到返回值。虽然图像处理公司的API是公开给它的客户的，但是API的具体实现是保密的，实际上，该公司后续可能会改变它的实现，只要它继续实现提供给客户的原来的接口。


#### 定义一个接口

一个接口的定义包括修饰符、关键字interface、接口名，实现的其他接口以及实现体，如下所示：

```
public interface GroupedInterface extends Interface1, Interface2, Interface3 {

    // constant declarations
    
    // base of natural logarithms
    double E = 2.718282;
 
    // method signatures
    void doSomething (int i, double x);
    int doSomethingElse(String s);
}

```

public访问修饰符表明这个接口可以被任何package的任何类使用。如果你没有使用public作为修饰符，那么修饰符就是默认的package-private的。一个接口可以继承另一个接口，就像子类继承父类一样。但是，一个类只能继承唯一一个其他类，而一个接口则可以继承任意多的接口。接口的声明包括了一个以逗号分隔的继承列表。

##### 接口实现体

接口实现体可以包括抽象方法、默认方法和静态方法。接口中的抽象方法以分号结尾，没有大括号（一个抽象方法不包括实现体）。默认方法以关键字default来修饰，静态方法以关键字static来修饰。所有的抽象、默认和静态方法都是默认public的，所有你在定义方法的时候可以省略public关键字。除此之外，一个借口还可以包含常量声明。在接口中定义的所有的常量值默认都是public static final的，再一次提醒，你可以在定义常量的时候省略这些修饰符。


### 实现一个接口

如果想定义一个实现接口的类，你需要使用implements关键字。一个类可以实现多个接口，此时implements关键字后面是一个以逗号分隔的接口列表。按照惯例，如果类定义中包含extends关键字，implements关键字出现在extends关键字的后面。

#### 一个样例接口，Relatable

Relatable接口是一个比较两个对象的size大小的接口：

```
public interface Relatable {
        
    // this (object calling isLargerThan)
    // and other must be instances of 
    // the same class returns 1, 0, -1 
    // if this is greater than, 
    // equal to, or less than other
    public int isLargerThan(Relatable other);
}

```

如果你想要比较两个同类对象的大小，不管它们是哪个类的对象，这个类应该implements Relatable接口。

任意类都可以实现Relatable接口，只要这个类的对象之间可以比较大小。对于字符串，可以比较字符个数；对于books，可以比较书的页数；对于students，可以比较体重；对于平面图形，可以比较面积；对于三维图形，可以比较体积。所有这些类都可以实现isLargerThan()方法。

#### 实现Relatable接口

```
public class RectanglePlus 
    implements Relatable {
    public int width = 0;
    public int height = 0;
    public Point origin;

    // four constructors
    public RectanglePlus() {
        origin = new Point(0, 0);
    }
    public RectanglePlus(Point p) {
        origin = p;
    }
    public RectanglePlus(int w, int h) {
        origin = new Point(0, 0);
        width = w;
        height = h;
    }
    public RectanglePlus(Point p, int w, int h) {
        origin = p;
        width = w;
        height = h;
    }

    // a method for moving the rectangle
    public void move(int x, int y) {
        origin.x = x;
        origin.y = y;
    }

    // a method for computing
    // the area of the rectangle
    public int getArea() {
        return width * height;
    }
    
    // a method required to implement
    // the Relatable interface
    public int isLargerThan(Relatable other) {
        RectanglePlus otherRect 
            = (RectanglePlus)other;
        if (this.getArea() < otherRect.getArea())
            return -1;
        else if (this.getArea() > otherRect.getArea())
            return 1;
        else
            return 0;               
    }
}

```

** 注意：在接口中定义的isLargerThan()方法，接受一个Relatable类型的对象参数。上述代码中:RectanglePlus otherRect = (RectanglePlus)other;这一行代码将other转换成了RectanglePlus类型的实例。类型转换告诉编译器一个对象的真实类型。直接在other对象上调用getArea()方法将会产生编译错误，因为编译器不知道other的实际类型是RectanglePlus。
**


### 将一个接口作为一个Type

当你定义一个新接口时，你正在定义一个新的引用数据类型。任何可以使用其他数据类型名称的地方都可以使用接口名。如果你定义了一个引用类型的变量，这个变量的类型是一个接口，给这个变量赋值的任意对象都必须是实现了此接口的一个类的实例。

例如，下面是找到两个对象中的最大值的方法，任意实现了Relatable接口的类的实例都可以使用这个方法：

```
public Object findLargest(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ((obj1).isLargerThan(obj2) > 0)
      return object1;
   else 
      return object2;
}

```	

通过将object1和object2转换成Relatable类型，你可以调用isLargerThan方法。

如果你的很多类都实现了Relatable接口，那么这些类的实例都可以调用findLargest方法--只要是同一个类的两个对象。同样的，它们也可以通过如下方法进行比较：

```

public Object findSmallest(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ((obj1).isLargerThan(obj2) < 0)
      return object1;
   else 
      return object2;
}

public boolean isEqual(Object object1, Object object2) {
   Relatable obj1 = (Relatable)object1;
   Relatable obj2 = (Relatable)object2;
   if ( (obj1).isLargerThan(obj2) == 0)
      return true;
   else 
      return false;
}

```

这些方法可以被Relatable对象调用，而不论它们的实现类继承了哪些类。当它们实现Relatable接口时，它们可以是实现类的类型、父类的类型以及Relatable的类型。这样就有了多继承的特性，它们的行为既可以来自于父类又可以来自于接口。


#### 升级的接口

考虑一个你已经开发的接口DoIt：

```
public interface DoIt {
   void doSomething(int i, double x);
   int doSomethingElse(String s);
}

```

假设，后来你想给接口增加第三个方法，所以新接口变为：

```
public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   boolean didItWork(int i, double x, String s);
   
}


```

如果你这样做了，所有实现了DoIt接口的类都将被破坏，因为它们没有实现新添加的didItWork方法。依赖于此接口的程序员将会大声的抱怨。

试图在一开始的时候就向所有使用你接口的用户提供完善的接口。如果你想要给接口添加新方法，你有几种选择。你可以创建一个DoItPlus接口继承DoIt接口：


```
public interface DoItPlus extends DoIt {

   boolean didItWork(int i, double x, String s);
   
}

```

现在依赖你的原接口的用户仍然可以使用原接口，也可以升级新的接口。

或者，你也可以将新方法定义为default方法。下面的代码定义了一个叫didItWork的default方法：


```
public interface DoIt {

   void doSomething(int i, double x);
   int doSomethingElse(String s);
   default boolean didItWork(int i, double x, String s) {
       // Method body 
   }
   
}

```

现在，你必须给default方法提供实现体。你也可以在接口中定义static方法。如果用户的类实现了这个接口，那么它们就可以使用新的default方法或者static方法，而无需修改或者重新编译它们的代码。


#### default 方法

上述几节介绍了自动驾驶汽车的例子，制造商发布了工业标准的接口，这个接口描述了哪些方法可以被调用以操纵汽车。如果制造商想要给汽车添加新特性，例如：飞行功能，那该怎么办？这些制造商将需要定义新方法，其他客户端公司(比如，电子导航设备制造商)将可以获得接口的新特性。那么，这些方法应该以何种方式进行声明？如果你添加新的方法到接口，那么实现者将不得不重现编写代码实现这个新添加的方法。如果你将方法声明为static的，那么程序员会认为此方法为功能函数，不是必须的、核心的方法。

default方法可以让你向库中的接口中添加新方法，并且保证与依赖原来的接口编写的代码的二进制兼容性。

考虑下面的接口，TimeClient：

```
import java.time.*; 
 
public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
}

```


SimpleTimeClient实现了这个接口：


```
package defaultmethods;

import java.time.*;
import java.lang.*;
import java.util.*;

public class SimpleTimeClient implements TimeClient {
    
    private LocalDateTime dateAndTime;
    
    public SimpleTimeClient() {
        dateAndTime = LocalDateTime.now();
    }
    
    public void setTime(int hour, int minute, int second) {
        LocalDate currentDate = LocalDate.from(dateAndTime);
        LocalTime timeToSet = LocalTime.of(hour, minute, second);
        dateAndTime = LocalDateTime.of(currentDate, timeToSet);
    }
    
    public void setDate(int day, int month, int year) {
        LocalDate dateToSet = LocalDate.of(day, month, year);
        LocalTime currentTime = LocalTime.from(dateAndTime);
        dateAndTime = LocalDateTime.of(dateToSet, currentTime);
    }
    
    public void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second) {
        LocalDate dateToSet = LocalDate.of(day, month, year);
        LocalTime timeToSet = LocalTime.of(hour, minute, second); 
        dateAndTime = LocalDateTime.of(dateToSet, timeToSet);
    }
    
    public LocalDateTime getLocalDateTime() {
        return dateAndTime;
    }
    
    public String toString() {
        return dateAndTime.toString();
    }
    
    public static void main(String... args) {
        TimeClient myTimeClient = new SimpleTimeClient();
        System.out.println(myTimeClient.toString());
    }
}

```


假设你想要往TimeClient中添加新功能，例如通过ZonedDateTime对象（类似LocalDateTime对象，不过它存储了时区信息）定义一个时区：


```
public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
        int hour, int minute, int second);
    LocalDateTime getLocalDateTime();                           
    ZonedDateTime getZonedDateTime(String zoneString);
}


```

跟随着TimeClient接口的改变，你将不得不修改SimpleTimeClient类，实现新添加的getZonedDateTime方法。但是，你可以不将getZonedDateTime方法定义为抽象的，你可以定义一个默认方法。

```
package defaultmethods;
 
import java.time.*;

public interface TimeClient {
    void setTime(int hour, int minute, int second);
    void setDate(int day, int month, int year);
    void setDateAndTime(int day, int month, int year,
                               int hour, int minute, int second);
    LocalDateTime getLocalDateTime();
    
    static ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString +
                "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }
        
    default ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }
}


```


你在接口中指定了一个方法定义，在方法签名的开头使用了default关键字。在接口中声明的所有方法，包括default方法，都是隐式public的，所以你可以省略public修饰符。以上述的方式，你不必修改SimpleTimeClient类，并且SimpleTimeClient以及任何实现了TimeClient的类都将可以调用在TimeClient接口中定义的这个default方法。下面的代码示例，调用了来自SimpleTimeClient类的getZonedDateTime()方法。


```
package defaultmethods;
 
import java.time.*;
import java.lang.*;
import java.util.*;

public class TestSimpleTimeClient {
    public static void main(String... args) {
        TimeClient myTimeClient = new SimpleTimeClient();
        System.out.println("Current time: " + myTimeClient.toString());
        System.out.println("Time in California: " +
            myTimeClient.getZonedDateTime("Blah blah").toString());
    }
}

```

#### 继承包含default方法的接口


当你继承包含一个default方法接口时，你可以：

* 不涉及default方法，那么你的接口将会继承父接口的default方法
* 重新定义default方法，使其成为抽象方法
* 重写default方法


一、假设你使用如下形式extends接口TimeClient：


```
public interface AnotherTimeClient extends TimeClient { }

```

那么，所有实现了AnotherTimeClient接口的类都将使用TimeCLient的默认方法getZoneDateTime。

二、假设你使用如下如下形式extends接口TimeClient:

```
public interface AbstractZoneTimeClient extends TimeClient {
    public ZonedDateTime getZonedDateTime(String zoneString);
}

```

那么，任何实现了AbstractZoneTimeClient接口的类都必须为getZonedDateTime方法提供实现体，这个方法是个抽象方法，跟接口中其他非默认方法是一样的。

三、假设你使用如下如下形式extends接口TimeClient:

```
public interface HandleInvalidTimeZoneClient extends TimeClient {
    default public ZonedDateTime getZonedDateTime(String zoneString) {
        try {
            return ZonedDateTime.of(getLocalDateTime(),ZoneId.of(zoneString)); 
        } catch (DateTimeException e) {
            System.err.println("Invalid zone ID: " + zoneString +
                "; using the default time zone instead.");
            return ZonedDateTime.of(getLocalDateTime(),ZoneId.systemDefault());
        }
    }
}

```

那么，所有实现了HandleInvalidTimeZoneClient接口的类将会使用HandleInvalidTimeZoneClient接口定义的getZonedDateTime方法，而不是TimeClient接口中定义的getZonedDateTime方法。


#### 静态方法

除了default方法，你也可以在接口中定义static方法。static方法可以很容易的为你的库组织辅助方法（helper methods）。你可以在一个接口中定义static方法，而不必去另一个类中定义这些辅助方法。下面的代码示例定义了一个static方法，得到与时区标识符对应的ZoneId对象，如果没有与时区标识符对应的zoneId对象，它使用系统默认的时区。

```
public interface TimeClient {
    // ...
    static public ZoneId getZoneId (String zoneString) {
        try {
            return ZoneId.of(zoneString);
        } catch (DateTimeException e) {
            System.err.println("Invalid time zone: " + zoneString +
                "; using default time zone instead.");
            return ZoneId.systemDefault();
        }
    }

    default public ZonedDateTime getZonedDateTime(String zoneString) {
        return ZonedDateTime.of(getLocalDateTime(), getZoneId(zoneString));
    }    
}

```

就像类中的静态方法，你在接口中定义静态方法也需要使用static关键字。再一次声明，接口中所有的方法声明，包括静态和default方法，都隐含是public的，所有你可以在方法定义中省略public修饰符。


#### default方法和已有的库的结合

default方法是你可以向已有的接口中增加新特性，系统会保证新老版本接口的代码的二进制级别的兼容。特别的，default方法可以使你向接口中添加已lambda表达式作为参数的方法。这一节描述了Comparator接口是如何被default和static方法进行增强的。

考虑前面在classes一章定义的Card和Deck类。下面的示例重写了Card和Deck类，将它们作为接口。Card接口包含两个枚举类型（Suit和Rank）和两个抽象方法（getSuit和getRank）。

```
package defaultmethods;

public interface Card extends Comparable<Card> {
    
    public enum Suit { 
        DIAMONDS (1, "Diamonds"), 
        CLUBS    (2, "Clubs"   ), 
        HEARTS   (3, "Hearts"  ), 
        SPADES   (4, "Spades"  );
        
        private final int value;
        private final String text;
        Suit(int value, String text) {
            this.value = value;
            this.text = text;
        }
        public int value() {return value;}
        public String text() {return text;}
    }
    
    public enum Rank { 
        DEUCE  (2 , "Two"  ),
        THREE  (3 , "Three"), 
        FOUR   (4 , "Four" ), 
        FIVE   (5 , "Five" ), 
        SIX    (6 , "Six"  ), 
        SEVEN  (7 , "Seven"),
        EIGHT  (8 , "Eight"), 
        NINE   (9 , "Nine" ), 
        TEN    (10, "Ten"  ), 
        JACK   (11, "Jack" ),
        QUEEN  (12, "Queen"), 
        KING   (13, "King" ),
        ACE    (14, "Ace"  );
        private final int value;
        private final String text;
        Rank(int value, String text) {
            this.value = value;
            this.text = text;
        }
        public int value() {return value;}
        public String text() {return text;}
    }
    
    public Card.Suit getSuit();
    public Card.Rank getRank();
}

```

Deck接口包括多个方法，这些方法用来操作deck中的cards：

```
package defaultmethods; 
 
import java.util.*;
import java.util.stream.*;
import java.lang.*;
 
public interface Deck {
    
    List<Card> getCards();
    Deck deckFactory();
    int size();
    void addCard(Card card);
    void addCards(List<Card> cards);
    void addDeck(Deck deck);
    void shuffle();
    void sort();
    void sort(Comparator<Card> c);
    String deckToString();

    Map<Integer, Deck> deal(int players, int numberOfCards)
        throws IllegalArgumentException;

}

```

PlayingCard类实现了接口Card，类实现了Deck接口。StandardDeck类实现了抽象方法Deck.sort，代码如下：

```
public class StandardDeck implements Deck {
    
    private List<Card> entireDeck;
    
    // ...
    
    public void sort() {
        Collections.sort(entireDeck);
    }
    
    // ...
}


```

Collections.sort方法，可以对List的实例进行排序，其中List中的元素必须实现Comparable接口。entireDeck成员是一个List实例，而且它包含的元素是Card类型，Card实现了Comparable接口。PlayingCard类实现了Comparable.compareTo方法，代码如下：

```
public int hashCode() {
    return ((suit.value()-1)*13)+rank.value();
}

public int compareTo(Card o) {
    return this.hashCode() - o.hashCode();
}

```

compareTo方法的实现，使得StandardDeck.sort()方法，首先根据suit进行排序，然后再根据rank进行排序。

但是，如果你想先根据rank字段排序，再根据suit排序，怎么办？你将需要实现Comparator接口，指定一个新的排序规则，然后使用sort(List<T> list, Comparaotr<? super T> c)方法（此版本的sort方法包含一个Comparator参数）。你可以在StandardDeck类中定义如下的方法：

```
public void sort(Comparator<Card> c) {
    Collections.sort(entireDeck, c);
}  

```

使用这个方法，你可以指定Collection.sort方法使用何种规则来对Card类的实例进行排序。一种方式是实现Comparator接口来指定你的排序规则。SortByRankThenSuit类示例如下：

```
package defaultmethods;

import java.util.*;
import java.util.stream.*;
import java.lang.*;

public class SortByRankThenSuit implements Comparator<Card> {
    public int compare(Card firstCard, Card secondCard) {
        int compVal =
            firstCard.getRank().value() - secondCard.getRank().value();
        if (compVal != 0)
            return compVal;
        else
            return firstCard.getSuit().value() - secondCard.getSuit().value(); 
    }
}

```

下面的代码，调用sort方法，首选根据rank排序，然后根据suit排序：

```
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(new SortByRankThenSuit());

```

但是，这个方法太繁琐了。如果你可以自己定义你想要对什么进行排序，而不是你想怎样排序的话，那就更好了。设想一下，如果你是开发Comparator接口的开发者。通过向接口Comparator中加入什么样的default和static方法可以使其他的开发者更容易的指定排序规则。

首先，如果你想要仅仅以rank字段来对Card进行排序，而不管suit。你可以如下形式调用StandardDeck.sort方法：

```
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(
    (firstCard, secondCard) ->
        firstCard.getRank().value() - secondCard.getRank().value()
); 

```

因为Comparator接口是一个函数式接口，你可以使用一个lambda表达式作为sort函数的参数。在上述示例中，lambda表达式比较了两个整形值。

如果可以仅在调用Card.getRank方法的时候创建一个Comparator接口的实例，就简化了开发者的代码。特别的，如果开发者可以创建一个Comparator接口的实例，这个实例可以比较任意的对象，只要这个对象有一个类似于getValue或者hashCode的返回一个数值类型的方法，那将非常有用。Comparator接口已经使用static方法comparaing()进行了增强:

```
myDeck.sort(Comparator.comparing((card) -> card.getRank()));  

```

在这个例子中，你还可以使用一个方法引用：

```
myDeck.sort(Comparator.comparing(Card::getRank)); 

```

这个调用更好的阐明了对什么进行排序，而不是怎样进行排序。（This invocation better demonstrates what to sort rather than how to do it）。

Comparator接口还提供了其他的static方法来进行增强，例如comparingDouble、comparingLong，这些方法可以比较其他数据类型。


假设程序员想要创建一个根据多个规则进行排序的Comparator接口，例如如何先根据rank排序，再根据suit进行排序？向前面的代码，你可以使用lambda表达式来指定这些规则：

```
StandardDeck myDeck = new StandardDeck();
myDeck.shuffle();
myDeck.sort(
    (firstCard, secondCard) -> {
        int compare =
            firstCard.getRank().value() - secondCard.getRank().value();
        if (compare != 0)
            return compare;
        else
            return firstCard.getSuit().value() - secondCard.getSuit().value();
    }      
); 

```

如果你可以从一系列Comparator实例构建一个Comparator实例的话，就更简单了。Comparator接口通过thenComparing方法进行了加强：

```
myDeck.sort(
    Comparator
        .comparing(Card::getRank)
        .thenComparing(Comparator.comparing(Card::getSuit)));

```

Comparator接口也提供了其他几个default方法，thenComparingDouble 和thenComparingLong等，可以使你对其他数据类型进行排序。

假设开发人员想要创建一个能够逆序排序的Comparator对象。例如，怎样对card对象以rank字段降序排序？像以前一样，你可以指定另一个lambda表达式。但是，如果能够通过调用一个方法reverse一个已有的Comparator，那将更加简单。Comparator接口通过reverse方法进行了增强：

```
myDeck.sort(
    Comparator.comparing(Card::getRank)
        .reversed()
        .thenComparing(Comparator.comparing(Card::getSuit)));

```

上述示例阐明了Comparator接口如何通过default方法、static方法、lambda表达式和方法引用来创建更具表达式的库方法，开发人员可以快速的推出他们正在怎样调用方法。你也可以使用这些特性来增强你自己的接口。
（This example demonstrates how the Comparator interface has been enhanced with default methods, static methods, lambda expressions, and method references to create more expressive library methods whose functionality programmers can quickly deduce by looking at how they are invoked. Use these constructs to enhance the interfaces in your libraries.）


### 总结

一个接口可以包括方法签名、默认方法、静态方法和常量。其中，默认方法和静态方法可以有实现体。

一个实现了接口的类必须实现接口中定义的所有抽象方法。

你可以在使用任何类型的地方使用一个接口的名字。


























