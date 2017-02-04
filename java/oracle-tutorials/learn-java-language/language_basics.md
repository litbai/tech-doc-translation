# Language Basics

* 变量：前面已经学习过，对象将它们的状态存储在域中。但是，Java编程语言也会使用术语变量（Variable）。此部分介绍了变量，包括变量的命名规则和明明习惯，基本数据类型（原始类型、字符串、数组），默认值和字面量。
* 操作符：这一部分描述了Java的操作符。首先介绍了最常使用的操作符，最后介绍最不常使用的操作符。每次讨论均包括你可以直接编译和运行的代码示例。
* 表达式、语句、代码块：操作符可以用来构建表达式，此表达式可以求值；表达式是语句的核心组成部分；语句可以组成代码块。这一节会讨论表达式、语句和代码块，并提供示例代码。
* 控制语句流：这一节描述了如何在Java语言中控制语句流程。它包括：判断、循环、分支语句流，可以使你有条件的执行特定的语句。

## 变量

一个对象将其状态保存在fields中：

```
int cadence = 0;
int speed = 0;
int gear = 1;

```

在“什么是对象”一节中介绍了域的概念，但是你可能会有一些疑问，比如：域的命名规则和风格是什么？除了int之外，还有什么其他的数据类型？fields在其声明的时候进行初始化？如果没有显示初始化，fields会不会被分配一个默认值？我们将会在这一节中讨论这些问题，但是在讨论之前，首先要意识到一些技术区别。在Java编程语言中，术语“filed”和“variable”都会被使用，许多开发者都会混淆二者，因为它们看起来经常指的是同样的东西。

Java编程语言定义了如下类型的变量：

* 实例变量（非静态域）：从技术的角度来说，对象将他们的状态存储在非静态域中，也就说声明的时候不带static关键字的域。非静态域称为实例变量是因为不同的对象有不同的值，一个自行车对象的currentSpeed域跟另一个自行车对象的currentSpeed域是不同的。
* 类变量（静态域）：类变量是用static修饰符声明的变量。这个关键字告诉编译器，有且仅有一个变量的拷贝存在，无论这个类被实例化多少次。一种特定类型的自行车的齿轮个数可以标记为static的，因为属于此类的自行车的实例均有相同的齿轮个数。代码 static int numGears = 6;会创建一个静态域。除此之外，关键字final也可以用来修饰齿轮数这个域，表示此域的值永远不会改变。
* 局部变量：类似于对象在域中存储它的状态，一个方法使用局部变量（local variables）来存储它的局部状态。声明一个局部变量的语法类似于声明一个域，并没有特殊的关键字来声明局部变量，完全取决于这个变量被声明的位置--声明在方法实现体之中的变量称为局部变量。因此，局部变量仅对声明它的方法可见，它们对于类的其余部分是不可见的。
* 参数：我们已经见过参数的例子，无论是在Bicycle类还是Hello World类中。回想一下main方法的签名：public static void main(String[] args)。在这里，args变量就是main方法的参数。你需要记住的是，参数总是归类为变量而不是域。这也适用于其他接受参数的结构，例如构造方法和异常处理器，你将会在后续的教程中学习到。

鉴于以上的说明，当讨论域和变量时，此教程接下来的部分将会使用如下的指南。如果我们讨论的是通常概念上的域（不包括局部变量和参数），我们就简单的称为域。如果我们的上下文适用于上述所有的情况，我们简称为变量。如果当前的上下文需要区分域和变量，我们将会采用更具体的术语（静态域，局部变量等）。你可能也会偶尔看到术语“member”（成员）。一个类型的域、方法和嵌套类统称为它的成员。


##### 命名

每一种程序语言都有它自己的命名规范和约定，java语言也是如此。java语言对变量命名的规则和约定可以总结如下：

* 变量名字是大小写敏感的。一个变量的名字可以试任意合法的标识符--一个无限定长度的Unicode字符和数字的序列，以字母、$符号、或者下划线开头。java约定俗成，声明变量时总是以字母开头，而不是_或者$,但这仅是程序员之间的通识，而不是语言的强制规定。除此之外，根据约定，$符号应该总是避免使用在标识符中。你可能会发现，在一些情况下，自动生成的命名会包含$符号，但是你自己命名的变量，总是应该避免使用它。同样，下划线也应该避免在标识符开头使用，尽管变量以_开头在技术上是允许的，实践中不鼓励使用，空格也不允许在标识符中。
* 接下来的字符可以是字母，数字，$符号，或者下划线。这里同样有一些约定，当为你的变量选取名字的时候，使用单词的全拼而不是神秘的缩写。这样可以使你的代码更易于阅读和理解。如果这样的话，可以使你的代码自动文档化（命名如果遵循好的规范，可以使你的代码自解释，无需添加额外的注释）。类似于cadence, speed, and gear这样的域名，相比于类似s,c,g这样的缩写更直观，意思更明确。但是需要记住的是，你选取的名字不能是已有的关键字或者保留字。
* 如果你的变量名字仅包含一个单词，那么使用小写字母的全拼。如果变量由多于一个单词，使用驼峰命名法。如果变量存储一个常量，例如static final int NUM_GEARS = 6;约定全部使用大写字母，并且每个单词之间用下划线隔开。根据约定，下划线只在此场景中使用，在任何其他场景中都不应该使用下划线。


### 原始数据类型（基本数据类型）

java编程语言是静态类型语言，意味着所有的变量必须先声明才能使用。变量声明包括变量的类型和名字，例如：int gear = 1;这行代码表示一个名字为gear的变量，其存储了一个整形值，初始值为1。一个变量的数据类型决定了它可以存储的值以及可以对它进行的操作。除了int类型，java还支持其他的基本数据类型。一个基本数据类型是语言提前定义好的，并且类型的名字是保留字。java支持的8种基本数据类型包括：

* byte：byte数据类型是8位有符号的二进制补码。它有最小值-128和最大值127.byte数据类型可以节省内存，当内存十分紧张时。它也可以在变量的值受限的时候，用来取代int。
* short：byte数据类型是16位有符号的二进制补码.它的最小值-32768和最大值32767，其使用场景与byte类似，可以用来节省内存。
* int：默认情况下，int数据类型是32位有符号的二进制补码，最小值-（2的31次方），最大值（2的31次方减1）.java8之后，你可以用int数据类型表示无符号的32位整数，数据范围为0到2的32次方减1。当需要使用int无符号整数时，使用Integer包装类。静态方法，诸如compareUnsigned，divideUnsigned等已经加到了Integer类中，用以支持对无符号int整数的操作。
* long：byte数据类型是64位有符号的二进制补码。在java8之后，可以使用无符号的long型，静态方法，诸如compareUnsigned，divideUnsigned等已经加到了Long类中，用以支持对无符号long型整数的操作。
* float：float数据类型是单精度的32位的符合IEEE 754标准的浮点数。它的数据范围将会在后续章节讨论。类似short和byte，当你需要节省内存时，可以试用float。当需要精确的数值时，永远不要使用此数据类型。这时候，你需要java.math.BigDecimal。
* double：float数据类型是单精度的64位的符合IEEE 754标准的浮点数.如果你想使用浮点数，默认使用double数据类型，类似的，当需要精确的数值时，永远不要使用double数据类型。
* boolean：boolean数据类型只有两个值：true和false。在需要简单的标志时，可以使用此数据类型，这种数据类型代表了一个bit的信息，但是它的size却不是一个bit。
* char：char数据类型是一个简单的16bit的Unicode字符。它有最小值'\u0000',最大值'\uffff'。

##### 默认值

当声明一个域的时候，不总是需要分配一个值。一个声明但没初始化的域将会被编译器分配一个默认值。通常来说，根据不同的数据类型，默认值将会是0或者null。但是，依赖于这种默认值，通常被认为是坏的编程风格。下面的表格总结了以上8种基本数据类型和复合类型的默认值：

|Date Type |Defaule Value(for field)|
|----------|------------------------|
| byte|0|
|short|0|
|int	|0|
|long|0L|
|folat|0.0f|
|double|0.0d|
|char|'\u0000'|
|boolean|false|
|String or any object|null|

##### 字面量

你可能注意到了，当初始化一个基本数据类型时，并没有使用new关键字。基本数据类型是一种特殊的程序语言内建的数据类型，它们不是从类创建的对象。一个字面量是一个固定值的代码表示，字面量直接用代码来表示，并不需要计算。如下所示，可以将一些基本数据类型分配一个字面量值：

```
boolean result = true;
char capitalC = 'C';
byte b = 100;
short s = 10000;
int i = 100000;

```

##### 整数字面量

一个整数字面量如果尾部加一个字母l或者L，则表示它的类型是long型，否则就是int型。推荐读者使用大写字母L，因为小写字母l经常与数字1混淆。

byte，short，int和long型都可以由int型字面量来创建。超过int范围的long型变量可以由long型字面量来创建。整型字面量可以由如下数字表示法来表达：

* 十进制：以10为基数，由0到9之间的数字组成，这是我们常见的数字表示法
* 十六进制：以16为基数，由0到9的数字和A到F的字母组成
* 二进制：以2为基数，由0和1组成（自从java7，你可以创建二进制数字字面量）

通常来说，编程时我们绝大部分时间都只使用十进制，但是如果你需要使用其他数字表示法，下面展示了正确的语法。前缀0x表示十六进制，前缀0b表示二进制：

```
int decVal = 26; // The number 26, in decimal
int hexVal = 0x1a; //  The number 26, in hexadecimal
int binVal = 0b11010; // The number 26, in binary

```

##### 浮点数字面量

浮点数字面量如果尾部加上F或者f，则表示类型为float型，否则默认为double型，或者你也可以在尾部加上D或者d来显示指定此字面量为double型。浮点数字面量也可以用科学计数法来表示，通过使用E或者e，配合在尾部加F或者f，亦或D或d，举例如下：

```
double d1 = 123.4;
// same value as d1, but in scientific notation
double d2 = 1.234e2;
float f1  = 123.4f;
```

##### 字符和字符串字面量

char型和String型字面量可以由任意的Unicode(UTF-16)字符组成。如果你的编辑器和文件系统兼容此编码，你可以直接在代码中使用。如果你的编辑器不允许此编码，你可以使用Unicode转译码来表示，类似'\u0108'，表示char型时使用单引号，表示String型时使用双引号。Unicode转译序列也可以用在程序的其他地方（例如域名），而不仅仅是char型和String型字面量。

java编程语言一些特殊的转译序列，例如： \b (backspace), \t (tab), \n (line feed), \f (form feed), \r (carriage return), \" (double quote), \' (single quote), and \\ (backslash).

特殊的null字面量可以被任何引用类型(reference type)来使用。null可以被赋值到除基本数据类型外的任意变量。除了检测它的存在之外，你不能使用null值做任何其他的事情。因为，null通常在程序中作为一个标志，来暗示一些值是无法使用的。

最后，还有一个特殊的字面量，class，通常是类名.class，例如String.class，这指的是String类本身这个对象。

##### 在数字字面量中使用下划线

在java SE7之后，数字字面量中可以使用下划线，这个特性可以使你用下划线将一串数字隔开，提高代码的可读性。

举个例子，如果你的代码里包括很多位的数字，你可以用下划线将数字3个一组隔开，就像你使用逗号、空格作为分隔符一样。下面的例子展示了如何在数字字面量中使用下划线：

```
long creditCardNumber = 1234_5678_9012_3456L;
long socialSecurityNumber = 999_99_9999L;
float pi = 3.14_15F;
long hexBytes = 0xFF_EC_DE_5E;
long hexWords = 0xCAFE_BABE;
long maxLong = 0x7fff_ffff_ffff_ffffL;
byte nybbles = 0b0010_0101;
long bytes = 0b11010010_01101001_10010100_10010010;

```

下面的几种情况不能使用下划线：

* 数字的开头或结尾
* 浮点数的小数点旁边
* F和L标志前面
* 在必须出现字符或数字的位置

```
// Invalid: cannot put underscores, adjacent to a decimal point
float pi1 = 3_.1415F;
// Invalid: cannot put underscores, adjacent to a decimal point
float pi2 = 3._1415F;
// Invalid: cannot put underscores, prior to an L suffix
long socialSecurityNumber1 = 999_99_9999_L;

// OK (decimal literal)
int x1 = 5_2;
// Invalid: cannot put underscores, At the end of a literal
int x2 = 52_;
// OK (decimal literal)
int x3 = 5_______2;

// Invalid: cannot put underscores, in the 0x radix prefix
int x4 = 0_x52;
// Invalid: cannot put underscores, at the beginning of a number
int x5 = 0x_52;
// OK (hexadecimal literal)
int x6 = 0x5_2; 
// Invalid: cannot put underscores, at the end of a number
int x7 = 0x52_;

```

### 数组

一个数组是一个容器对象，它可以保存固定数量的同一类型的值。数组的长度在创建的时候就需要指定。数组创建之后，它的长度就固定了。我们在Hello World程序中，已经见过数组的例子，这一节将更加详细的介绍数组,下图展示了一个长度为10的数组：

![](objects-tenElementArray.gif)

数组中的每一个item称为element，可以通过数字索引来获取每一个element。索引从0开始，第9个元素，需要索引8来获取。

下面的代码，创建了整型数组，放入数组一些值，并把每个元素打印到标准输出：

```
class ArrayDemo {
	public static void main(String[] args) {
		int[] anArray;
		anArray = new int[10];
		anArray[0] = 100;
		anArray[1] = 200;
		anArray[2] = 300;
		anArray[3] = 400;
		anArray[4] = 500;
		anArray[5] = 600;
		
		System.out.println("Element at index 0 is " + anArray[0]);	
		System.out.println("Element at index 1 is " + anArray[1]);
		System.out.println("Element at index 2 is " + anArray[2]);
		System.out.println("Element at index 3 is " + anArray[3]);
		System.out.println("Element at index 4 is " + anArray[4]);

	}
}

```

最后的输出结果是：

```
Element at index 0: 100
Element at index 1: 200
Element at index 2: 300
Element at index 3: 400
Element at index 4: 500

```

实际编程中，我们会使用循环结构来迭代数组元素，而不是上述单行处理。但是，上述的示例清晰的展示了数组的语法，在流程控制一节中你将会学习循环结构。

##### 声明一个变量来引用数组

上述的示例使用如下的代码声明了一个数组：

```
int[] anArray;
```

像其他类型的变量声明一样，数组声明包括2部分：数组的类型和它的名字，数组的类型表示为type[]，其中，type是数组包含的元素的类型，方括号是一个特殊标志，表示此变量指向的是一个数组，数组的长度不是变量类型的组成部分。数组的名字可以是任何合法标识符，就像其他类型的变量声明一下，声明并不会真实的创建数组，它只是简单的告诉编译器，这个变量保存的是一个特定的类型。你可以声明各种类型的数组变量，如下：

```
byte[] anArrayOfBytes;
short[] anArrayOfShorts;
long[] anArrayOfLongs;
float[] anArrayOfFloats;
double[] anArrayOfDoubles;
boolean[] anArrayOfBooleans;
char[] anArrayOfChars;
String[] anArrayOfStrings;

//You can also place the brackets after the array's name:
// this form is discouraged
float anArrayOfFloats[];

```

##### 创建、初始化、访问数组

创建数组的一种方式是使用new操作符。

```
anArray = new int[10];
```
给数组元素赋值：

```
anArray[0] = 100; // initialize first element
anArray[1] = 200; // initialize second element
anArray[2] = 300; // and so forth

```
通过数字索引，访问数组元素：

```
System.out.println("Element 1 at index 0: " + anArray[0]);
System.out.println("Element 2 at index 1: " + anArray[1]);
System.out.println("Element 3 at index 2: " + anArray[2]);

```

或者，你也可以使用简短的预发来创建并初始化一个数组：

```
int[] anArray = { 
    100, 200, 300,
    400, 500, 600, 
    700, 800, 900, 1000
};

```

这样创建的数组的长度，取决于大括号之间提供数值的个数。

你也可以创建一个数组的数组，称为多维数组，通过使用2个或多个方括号，同时，访问数组元素也需要同样多的方括号个数。在java编程语言中，一个多维数组是那些数组元素也是数组的数组，不同于C或者Fortran语言中的数组。不像C语言中每行的长度是固定的，java中的多维数组，其行的长度是可变的，如下所示：

```
class MultiDemArrayDemo {
	public static void main(Stirng[] args) {
		Stirng names = {
			{"Mr. ", "Mrs. ", "Ms. "},
			{"Smith", "Jones"}
		}
		System.out.println(name[0][0] + name[1][0]);
		System.out.println(name[0][2] + name[1][1])
	}
}
```

最后，你可以使用内建的length属性来获取数组的长度,例如：

```
 System.out.println(anArray.length);
 
```

##### 数组拷贝

System类有个数组拷贝的方法arraycopy，你可以使用此方法进行高效的数组拷贝：

```
public static void arraycopy(Object src, int srcPos,
                             Object dest, int destPos, int length)

```

这个方法中的两个Object型参数分别指定了源数组和目标数组，3个int型参数分别指定了源数组的起始位置，目标数组的起始位置和需要拷贝的元素的个数。

下面的程序，创建了一个char型数组，它使用System.arraycopy方法拷贝了一些子元素：

```
class ArrayCopyDemo {
    public static void main(String[] args) {
        char[] copyFrom = { 'd', 'e', 'c', 'a', 'f', 'f', 'e',
			    'i', 'n', 'a', 't', 'e', 'd' };
        char[] copyTo = new char[7];

        System.arraycopy(copyFrom, 2, copyTo, 0, 7);
        System.out.println(new String(copyTo));
    }
}

```

##### 数组操作

数组是在编程语言中是一个一个十分强大有用的工具。java SE提供了许多针对数组的常用操作。例如，System.arraycopy方法提供了拷贝数组的功能，仅用1行代码就可以实现数组拷贝的功能，避免了自己手动迭代实现数组拷贝的重复劳作。

为了方便开发，java SE在java.util.Arrays类中提供了许多常见的对数组进行操作的方法（通用任务，类似拷贝、排序、搜索等）。例如，你也可以使用Arrays类的copyOfRange方法来替代arraycopy方法，唯一的不同是此方法不要求目标数组的存在，此方法的返回值就是你需要的目标数组，如下代码所示：

```
class ArrayCopyOfDemo {
    public static void main(String[] args) {
        
        char[] copyFrom = {'d', 'e', 'c', 'a', 'f', 'f', 'e',
            'i', 'n', 'a', 't', 'e', 'd'};
            
        char[] copyTo = java.util.Arrays.copyOfRange(copyFrom, 2, 9);
        
        System.out.println(new String(copyTo));
    }
}

```

运行如上程序，你可以发现，尽管代码行数减少了，但是结果是一样的。注意到copyOfRange方法，第二个参数是起始索引，拷贝会包含此索引，而第三个是结束索引，拷贝不包括此索引。

Arrays类提供的其他一些有用的功能列表如下：

* 搜索特定元素，例如binarySearch方法
* 比较两个数组，例如equals方法
* 用特定的值填充数组的每一个元素，fill方法
* 对数组排序。你可以使用顺序排序的sort方法，或者并行排序的parallelSort方法，此方法在java 8中引入，在多处理器对大型数组排序时，效率较高。


### 总结

java编程语言同时使用变量和域作为它的术语。实例变量（非静态域）对每个实例来说是唯一的。类变量（静态域）是那些使用static修饰符声明的域，这些变量仅存在一个，而不管这个类创建了多少实例。局部变量是在方法内部短暂存储的变量。参数是给方法提供额外信息的变量，局部变量和方法都统称为变量而不是域。当给你的变量命名时，最好遵循良好的编程风格。

java编程语言中8种基本数据类型为：byte, short, int, long, float, double, boolean, and char. 编译器会给域类型为上述的基本类型的域分配合理的默认值，对于局部变量，编译器不会分配默认值。一个字面量是一个代表固定值的程序代码。一个数组是存在固定数量的同一类型的容器，数组的长度在数组创建的时候就指定好了。创建完成之后，数组长度不变。、



































