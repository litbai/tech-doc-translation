## Numbers

这一节首先讨论了java.lang包下的Number类，它的子类，同时，讨论了在什么情况下你要使用包装类而不是基本数据类型。

这一节也介绍了PrintStream和DecimalFormat类，这些类提供了格式化数字输出的方法。

最后，讨论了java.lang包中的Math方法。它包含了很多数学方法，以补充java语言内建支持的几种简单运算符。这个类的方法提供了三角函数、指数函数以及其他运算。

### Number类

当操作数字时，大部分时间你会使用基本数据类型，例如：

```
int i = 500;
float gpa = 3.65f;
byte mask = 0xff;

```

但是，在有些场景下，你需要使用对象，java平台提供了与每种基本数据类型对应的包装类，这些类将基本数据类型包装在了一个对象中。通常，包装是由编译器来完成的--如果你在期望使用对象的地方使用了基本数据类型，编译器会自动将基本数据类型包装为包装类。同样的，当你在需要使用基本数据类型的地方，使用了包装类，那么编译器会自动执行拆箱操作。


所有的数字包装类都是抽象类Number的子类：

![](objects-numberHierarchy.gif)


** 注意，还有另外其他4个类也是Number的子类，但未在此处讨论。BigDecimal和BigInteger是在高精度场景下使用，AtomicInteger和AtomicLong在多线程应用中使用 **


下面列出了你应该使用Number类的实例，而不是基本数据类型的3种场景：

* 方法参数，参数类型为对象类型（通常在操作集合时使用）
* 使用类预定义的常量，例如MIN_VALUE和MAX_VALUE，提供了数据类型的上下限
* 使用类方法将基本数据类型转化为包装类或者将包装类转为基本数据类型、将包装类转为string或者从String转为包装类，或者在数字系统之间进行装换（decimal、octal、hexadecimal、binary）。

下面的表格列出了所有的Number的子类都实现了的实例方法：

|Method|Description|
|------|-----------|
|byte byteValue()|
|short shortValue()|
|int intValue()|
|long longValue()|
|float floatValue()|
|double doubleValue()|将Number对象的值转换为基本数据类型|
|int compareTo(Byte anotherByte)|
|int compareTo(Double anotherDouble)|
|int compareTo(Float anotherFloat)|
|int compareTo(Integer anotherInteger)|
|int compareTo(Long anotherLong)|
|int compareTo(Short anotherShort)| 比较Number对象|
|boolean equals(Object obj)|判断一个对象与参数中的对象是否相等，这个方法返回true，如果obj不为null，且obj与这个对象是同一个类型，且有相同的数值。对于Double和Float对象，有一些特殊性，具体参考Java API文档。|

每一个Number类都包含其他的一些功能方法，用来将数字和字符串相互转化，或者数字与数字相互转化。下面的表格列出了Integer类的这些方法，其他Number类的方法时类似的：

|Method|Description|
|------|-----------|
|static Integer decode(String s)| 将一个String编码为integer。参数可以是decimal、octal、hexadecimal形式|
|static int parseInt()|返回一个十进制Integer|
|static int parseInt(String s, int radix)|根据String参数，返回一个指定类型的整数|
|String toString()|返回Integer数值的字符串表示|
|static String toString(int i)|返回一个指定整数的字符串表示|
|static Integer valueOf(int i)|返回具有参数数值的包装对象|
|static Integer valueOf(String s, int radix)|根据radix，返回s对应的十进制数值|


### 格式化Number输出

前面我们已经见过print和println方法，将字符串打印到标准输出流中(System.out)。因为所有的数字都可以被转换为字符串，你可以使用这些方法打印任意的字符串和数字的拼接。java编程语言还有其他的方法，可以使你以多种格式来来控制输出的数字。

#### printf和format方法

the java.io包中包括一个PrintStream类，这个类有两个格式化方法，你可以使用这两个方法老替代print和println方法。这两个方法，format和printf，具备同样的功能。我们使用过的System.out就是一个PrintStream类的对象，所以你可以在System.out上调用PrintStream的方法，例如：

```
System.out.format(...);

```
format和printf两个方法的语法是相同的：

```
public PrintStream format(String format, Object... args)

```


上图中方法的参数format是一个字符串类型，指定了输出的个数，args是一个被输出的参数列表，一个简单的示例如下：

```
System.out.format("The value of the float variable is %f, " +
     while the value of the integer variable is %d, " +
     "and the string is %s", floatVar, intVar, stringVar); 

```

方法的第一个参数，format，是一个格式化字符串，指定了第二个参数args将被如何格式化。格式化字符串包括普通的文本和格式化区分符。格式化区分符是一些特殊的字符，可以用来格式化args。Object... args语法代表可变参数，意思是这个方法可以接受任意个数的参数。

格式化区分符以‘%’开头，以一个转换符结尾。转换符是一个字符，暗示了将被格式化的参数的类型。在'%'和转换符之间，你可以添加可选的标识符。java中有很多转换符、标识符和区分符，在java.util.Formatter类的文档中有详细介绍。

```
int i = 461012;
System.out.format("The value of i is: %d%n", i);

```

上述示例中，%d代表一个十进制整数，%n是一个与平台无关的换行符。

printf和format方法被重载了，每个方法还有另外一个版本：

```
public PrintStream format(Locale l, String format, Object... args)

```

如果想要打印French的数字(对于浮点数，French系统使用逗号,来替代小数点.)，你可以使用如下代码：

```
System.out.format(Locale.FRANCE, "The value of the float variable is %f, while the value of the integer variable is %d, and the string is %s%n", floatVar, intVar, stringVar);

```

#### 一个举例

下面的表格列出了一些转换符和标识符：

|Conventer|Flag|Explanation|
|---------|----|-----------|
|d||十进制数字|
|f||float|
|n|||平台无关的换行符，你应该总是使用%n而不是\n|
|tB||一个data&time格式|
|td,te|date&time格式，用两位数字表示一个月的日期，td会在个位日期之前补0|
|ty,tY|date&time格式，ty用两位数表示年份，tY用四位数|
|tM|date&time格式，用两位数表示分钟|
|tp|date&time格式，地域相关的am和pm|
|tm|date&time格式，用两位数表示月份|
|tD|date&time格式，格式为%tm%td%ty|
||08|8位宽，位数不足，0来凑|
||+|表示正负数，正数带标志+，负数待标志-|
||,|特定于地域的数字分隔|
||-|左对齐|
||.3|小数位后面保留3位|
||10.3|10位宽，右对齐，其中小数点后保留3位|


下面的代码示例展示了如何使用这些格式化转换符：

```
import java.util.Calendar;
import java.util.Locale;

public class TestFormat {
    
    public static void main(String[] args) {
      long n = 461012;
      System.out.format("%d%n", n);      //  -->  "461012"
      System.out.format("%08d%n", n);    //  -->  "00461012"
      System.out.format("%+8d%n", n);    //  -->  " +461012"
      System.out.format("%,8d%n", n);    // -->  " 461,012"
      System.out.format("%+,8d%n%n", n); //  -->  "+461,012"
      
      double pi = Math.PI;
      
      System.out.format("%f%n", pi);       // -->  "3.141593"
      System.out.format("%.3f%n", pi);     // -->  "3.142"
      System.out.format("%10.3f%n", pi);   // -->  "     3.142"
      System.out.format("%-10.3f%n", pi);  // -->  "3.142"
      System.out.format(Locale.FRANCE,
                        "%-10.4f%n%n", pi); // -->  "3,1416"

      Calendar c = Calendar.getInstance();
      System.out.format("%tB %te, %tY%n", c, c, c); // -->  "May 29, 2006"

      System.out.format("%tl:%tM %tp%n", c, c, c);  // -->  "2:34 am"

      System.out.format("%tD%n", c);    // -->  "05/29/06"
    }
}


```

** 注意，这一节仅覆盖了基本的format和printf方法。更多的细节可以在Basic I/O这一章的“Formatting”一节找到。到时将介绍使用String.format方法得到转换后的字符串。**


#### DecimalFormat类

你可以使用java.text.DecimalFormat方法来控制数字的前面和后面填充0、填充其他字符、千位分隔符、小数点分隔符。DecimalFormat类提供了很多灵活的格式化数字的格式，但是它也很复杂。

下面的示例创建了一个DecimalFormat类的对象，myFormatter，通过给DecimalFormat的构造函数传递一个格式化字符串。format()方法是DecimalFormat从NumberFormat继承而来：

```
import java.text.*;

public class DecimalFormatDemo {

   static public void customFormat(String pattern, double value ) {
      DecimalFormat myFormatter = new DecimalFormat(pattern);
      String output = myFormatter.format(value);
      System.out.println(value + "  " + pattern + "  " + output);
   }

   static public void main(String[] args) {

      customFormat("###,###.###", 123456.789);
      customFormat("###.##", 123456.789);
      customFormat("000000.000", 123.78);
      customFormat("$###,###.###", 12345.67);  
   }
}

output:

123456.789  ###,###.###  123,456.789
123456.789  ###.##  123456.79
123.78  000000.000  000123.780
12345.67  $###,###.###  $12,345.67

```

下面的表格解释了每种输出：

|Value|Pattern|Output|Explanation|
|123456.789|###,###.###|123,456.789|#号代表一个数字，逗号代表一个千位分隔符的占位符，句号代表小数点占位符|
|123456.789|###.##|小数点后有3位，但是pattern只有两位，此时采取4舍5入法，保留两位小数|
|123.78|000000.000|000123.780|pattern指定了头部和尾部的0|
|12345.67|$###,###.###|$12,345.67|在数字前面加$|


#### 基本运算之外的高级运算

java编程语言支持基本的运算，通过使用运算符：+, -, x, /和%。java.lang包中的Math类提供了一些方法和常量来完成更复杂的计算。

Math类中的方法都是static的，所以你可以通过类名来调用它们：

```
Math.cos(angle)

```

** 注意，使用static import特性，你不用每次都带上前缀Math：
import static java.lang.Math.*; 然后你可以如下方式调用Math类的方法：cos(angle);**


#### 常量和基本方法

Math类包括两个常量: Math.E 和 Math.PI

Math类还包括超过40个静态方法，下面的表格列出了一些基本的方法：

|Method|Description|
|------|-----------|
|double abs(double d)|
|float abs(float f)|
|int abs(int i)|
|long abs(long lng)| 返回一个数字绝对值|
|double ceil(double d)|向上取整, 返回double型|
|double floor(double d)|向下取整，返回double型|
|double rint(double d)|返回与当前数最接近的整数， 返回double型|
|long round(double d)|返回与当前数最接近的整数，返回long型|
|int round(float f)|返回与当前数最接近的整数，返回int型|
|double min(double arg1, double arg2)|
|float min(float arg1, float arg2)|
|int min(int arg1, int arg2)|
|long min(long arg1, long arg2)| 返回两个参数中较小的一个|
|double max(double arg1, double arg2)|
|float max(float arg1, float arg2)|
|int max(int arg1, int arg2)|
|long max(long arg1, long arg2)|返回两个参数中较大的一个|


下面的示例程序，BasicMathDemo，展示了如何使用这些方法：

```
public class BasicMathDemo {
    public static void main(String[] args) {
        double a = -191.635;
        double b = 43.74;
        int c = 16, d = 45;

        System.out.printf("The absolute value " + "of %.3f is %.3f%n", 
                          a, Math.abs(a));

        System.out.printf("The ceiling of " + "%.2f is %.0f%n", 
                          b, Math.ceil(b));

        System.out.printf("The floor of " + "%.2f is %.0f%n", 
                          b, Math.floor(b));

        System.out.printf("The rint of %.2f " + "is %.0f%n", 
                          b, Math.rint(b));

        System.out.printf("The max of %d and " + "%d is %d%n",
                          c, d, Math.max(c, d));

        System.out.printf("The min of of %d " + "and %d is %d%n",
                          c, d, Math.min(c, d));
    }
}

```

程序的输出为：

```
The absolute value of -191.635 is 191.635
The ceiling of 43.74 is 44
The floor of 43.74 is 43
The rint of 43.74 is 44
The max of 16 and 45 is 45
The min of 16 and 45 is 16

```


#### 指数和对数方法

下面的表格列出了Math类的指数和对数方法：

|Method|Description|
|------|-----------|
|double exp(double d)|返回以e为底，指数为d的结果|
|double log(double d)|返回以e为底，基数为d的结果|
|double pow(double base, double exponent)|返回以base为底， exponent为指数的结果|
|double sqrt(double d)|返回d的算术平方根|


程序示例如下：

```
public class ExponentialDemo {
    public static void main(String[] args) {
        double x = 11.635;
        double y = 2.76;

        System.out.printf("The value of " + "e is %.4f%n",
                          Math.E);

        System.out.printf("exp(%.3f) " + "is %.3f%n",
                          x, Math.exp(x));

        System.out.printf("log(%.3f) is " + "%.3f%n",
                          x, Math.log(x));

        System.out.printf("pow(%.3f, %.3f) " + "is %.3f%n",
                          x, y, Math.pow(x, y));

        System.out.printf("sqrt(%.3f) is " + "%.3f%n",
                          x, Math.sqrt(x));
    }
}

```

程序输出：

```
The value of e is 2.7183
exp(11.635) is 112983.831
log(11.635) is 2.454
pow(11.635, 2.760) is 874.008
sqrt(11.635) is 3.411

```


#### 三角函数

Math类也提供了一系列三角函数，下面的表格列出了这些函数。这些方法的参数值都是弧度，你可以使用toRadians方法将度数转换为弧度。

|Method|Description|
|------|-----------|
|double sin(double d)|
|double cos(double d)|
|double tan(double d)|
|double asin(double d)| 反三角函数|
|double acos(double d)|
|double atan(double d)|
|double atan2(double y, double x)|将直角坐标(x, y)转换为极坐标(r, theta), 返回theta|
|double toDegrees(double d)|
|double toRadians(double d)|


下面是程序示例：

```
public class TrigonometricDemo {
    public static void main(String[] args) {
        double degrees = 45.0;
        double radians = Math.toRadians(degrees);
        
        System.out.format("The value of pi " + "is %.4f%n",
                           Math.PI);

        System.out.format("The sine of %.1f " + "degrees is %.4f%n",
                          degrees, Math.sin(radians));

        System.out.format("The cosine of %.1f " + "degrees is %.4f%n",
                          degrees, Math.cos(radians));

        System.out.format("The tangent of %.1f " + "degrees is %.4f%n",
                          degrees, Math.tan(radians));

        System.out.format("The arcsine of %.4f " + "is %.4f degrees %n", 
                          Math.sin(radians), 
                          Math.toDegrees(Math.asin(Math.sin(radians))));

        System.out.format("The arccosine of %.4f " + "is %.4f degrees %n", 
                          Math.cos(radians),  
                          Math.toDegrees(Math.acos(Math.cos(radians))));

        System.out.format("The arctangent of %.4f " + "is %.4f degrees %n", 
                          Math.tan(radians), 
                          Math.toDegrees(Math.atan(Math.tan(radians))));
    }
}


The output of this program is as follows:

The value of pi is 3.1416
The sine of 45.0 degrees is 0.7071
The cosine of 45.0 degrees is 0.7071
The tangent of 45.0 degrees is 1.0000
The arcsine of 0.7071 is 45.0000 degrees
The arccosine of 0.7071 is 45.0000 degrees
The arctangent of 1.0000 is 45.0000 degrees

```


#### 随机数

random()方法返回一个0.0到1.0之间的伪随机数，包括下限0.0但是不包括上限1.0。换句话说，0.0 <= Math.random() < 1.0。如果想得到其他范围的随机数，你可以对Math.random()的返回值做一些计算。例如，为了得到0到9之间的随机数，你可以：

```
int number = (int) (Math.random() * 10);

```


当你只需要一个随机数时，使用Math.random可以很方便。如果你想生成一系列随机数，你可以创建一个Random类的实例，调用这个实例的方法生成随机数。


### 总结

你可以使用每种基本类型对应的包装类，来将一个基本数据类型包装为一个对象。java编译器会自动在需要的时候进行装箱和拆箱操作。


Number的子类包括一些常量和有用的方法。MIN_VALUE和MAX_VALUE分别包含一个类型的对象能够包含的最大值和最小值。byteValue和shortValue以及其他类似的方法可以将一种数字类型转换为另一种。valueOf方法可以将字符串装换为数字，toString方法可以将数字转换为字符串。


为了格式化一个包含数字的字符串，你可以使用PrintStream的printf和format方法。或者，你也可以使用NumberFormat类，使pattern来自定义数字的输出格式。


Math类包含很多静态方法来执行一些数学计算，包括指数、对数以及三角函数。Math类同样包含一些基础的数学操作，例如abs，round，以及产生随机数的random方法。




























