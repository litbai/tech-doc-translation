## Operators

我们已经学习了如何声明和初始化变量，你可以想要通过变量来做些事情。学习java编程语言的操作符是一个好的开始。操作符是特殊的标志，它可以对一个、两个或者是3个运算数执行特定的操作，并返回一个结果。

因为我们正在探索java的运算符，知道哪个运算符拥有最高的优先级是很有帮助的。下面表格中列出了各个运算符的优先级。越接近表格的上方，运算符的优先级越高。优先级高的运算符比优先级低的运算符先执行。同一行的运算符拥有相同的优先级。荡同一个表达式出现相同优先级的运算符时，必须有一个规则决定首先执行哪个运算符。<b>除了赋值运算符之外，所有的二元运算符均从左到右执行，赋值运算符从右到左执行。</b>


|Operators|Precedence|
|---------|----------|
|postfix,词尾运算符 | expr++ expr--|
|unary,一元运算符|++expr, --expr, +expr, -expr, ~, !|
|乘除法| * 、/ 、%|
|加减法| +、 -|
|关系运算符| <, >, <=, >=, instanceof|
|相等运算符| ==, !=|
|位运算符 and | &|
|位运算符 异或| ^|
|位运算符 或| | |
|逻辑运算符 and | && |
|逻辑运算符 or | || |
|三元运算符| ? : |
|赋值运算符| =, +=, -=, *=, /=, %=, &=, ^=, |=, <<=, >>=, >>>=|

通常来说，一些运算符比另一些出现的频率要高一些，例如，赋值运算符=出现的频率要远高于无符号右移运算符>>>。根据以上的想法，本章首先讨论那些出现频率比较高的运算符，最后讨论出现频率较低的运算符。对于每种运算符的讨论，都会提供可直接编译运行的代码，仔细研究代码的输出，可以帮助你加深对所学知识的理解。


### 赋值、算数和一元操作符

##### 赋值操作符

你将会遇到的最常见的操作符就是赋值操作符“=”。你在Bicycle类中见过此操作符，它将右边的值赋给左边的变量。这个操作符还可以将对象引用赋值给变量。

```
int cadence = 0;
int speed = 0;
int gear = 1;

```

##### 算术操作符

java编程语言提供了执行加减乘除的操作符。这些操作符跟数学之中的加减乘除相当。看起来比较陌生的一个操作符是“%”，执行取余操作。下面的程序演示了算术运算符的使用。

```
class ArithmeticDemo {

    public static void main (String[] args) {

        int result = 1 + 2;
        // result is now 3
        System.out.println("1 + 2 = " + result);
        int original_result = result;

        result = result - 1;
        // result is now 2
        System.out.println(original_result + " - 1 = " + result);
        original_result = result;

        result = result * 2;
        // result is now 4
        System.out.println(original_result + " * 2 = " + result);
        original_result = result;

        result = result / 2;
        // result is now 2
        System.out.println(original_result + " / 2 = " + result);
        original_result = result;

        result = result + 8;
        // result is now 10
        System.out.println(original_result + " + 8 = " + result);
        original_result = result;

        result = result % 7;
        // result is now 3
        System.out.println(original_result + " % 7 = " + result);
    }
}

```

程序输出如下：

```
1 + 2 = 3
3 - 1 = 2
2 * 2 = 4
4 / 2 = 2
2 + 8 = 10
10 % 7 = 3

```

你可以通过将算操作符和复制操作符结合起来，例如：x += 1, x = x + 1; "+"操作符也可以用来连接两个字符串，如下所示：

```
class ConcatDemo {
    public static void main(String[] args){
        String firstString = "This is";
        String secondString = " a concatenated string.";
        String thirdString = firstString+secondString;
        System.out.println(thirdString);
    }
}
```

##### 一元操作符

一元操作符只需要一个操作数，它们执行值得自增、自减，表达式取负，或者boolean值取反等操作。下面的程序演示了一元操作符的用法：

```
class UnaryDemo {

    public static void main(String[] args) {

        int result = +1;
        // result is now 1
        System.out.println(result);

        result--;
        // result is now 0
        System.out.println(result);

        result++;
        // result is now 1
        System.out.println(result);

        result = -result;
        // result is now -1
        System.out.println(result);

        boolean success = false;
        // false
        System.out.println(success);
        // true
        System.out.println(!success);
    }
}

```

自增、自减操作符可以用在操作数的头部或者尾部。代码result++;和代码++result;都会使result的值增加1。唯一的不同点在于，前置版本++result的值等于自增后的值，而result++的值等于自增前的值。如果你仅仅是执行一个简单的自增或者自减操作，这两种操作都可以使用。但是如果这个自增或者自减操作是整体表达式的一部分，那么你需要小心的考虑二者的不同。下面的代码演示了前置和后置一元自增运算符的使用：

```
class PrePostDemo {
    public static void main(String[] args){
        int i = 3;
        i++;
        // prints 4
        System.out.println(i);
        ++i;			   
        // prints 5
        System.out.println(i);
        // prints 6
        System.out.println(++i);
        // prints 6
        System.out.println(i++);
        // prints 7
        System.out.println(i);
    }
}

```

### 判等、关系和条件操作符

##### 判等和关系操作符

判等和关系运算符判断一个操作数大于、小于、等于或者是不等于另一个操作数。这里的大部分运算符你已经很熟悉了，记住，当你需要判断两个基本数据类型值是否相等的时候要用“==”，而不是“=”。下面的程序演示了上述运算符的使用：

```
class ComparisonDemo {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if(value1 == value2)
            System.out.println("value1 == value2");
        if(value1 != value2)
            System.out.println("value1 != value2");
        if(value1 > value2)
            System.out.println("value1 > value2");
        if(value1 < value2)
            System.out.println("value1 < value2");
        if(value1 <= value2)
            System.out.println("value1 <= value2");
    }
}

```

上述程序的输出为：

```
value1 != value2
value1 <  value2
value1 <= value2

```

##### 条件操作符

条件操作符"&&"和"||"对两个boolean表达式执行条件AND和OR操作。这两个操作具有短路行为，这意味着第二个表达式只有在需要的时候才会计算。下面的代码演示了条件运算符的使用：

```
class ConditionalDemo1 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        if((value1 == 1) && (value2 == 2))
            System.out.println("value1 is 1 AND value2 is 2");
        if((value1 == 1) || (value2 == 1))
            System.out.println("value1 is 1 OR value2 is 1");
    }
}


```

另一个条件操作符是?: , 可以认为此操作符是一个简化的if-then-else语句。这个操作符还被称为三元操作符，因为它又3个操作数。下面的例子，可以翻译为：如果someCondition为true，将value1的值赋给result，否则将value2的值赋给result。

```
class ConditionalDemo2 {

    public static void main(String[] args){
        int value1 = 1;
        int value2 = 2;
        int result;
        boolean someCondition = true;
        result = someCondition ? value1 : value2;

        System.out.println(result);
    }
}

```

##### 类型比较操作符 instanceof

instanceof操作符将一个对象和一个特定的类型进行比较。你可以使用此操作符判断一个对象是否是一个类的实例，一个子类的实例，或者一个实现了特定接口的实例，示例代码如下：

```
class InstanceofDemo {
    public static void main(String[] args) {

        Parent obj1 = new Parent();
        Parent obj2 = new Child();

        System.out.println("obj1 instanceof Parent: "
            + (obj1 instanceof Parent));
        System.out.println("obj1 instanceof Child: "
            + (obj1 instanceof Child));
        System.out.println("obj1 instanceof MyInterface: "
            + (obj1 instanceof MyInterface));
        System.out.println("obj2 instanceof Parent: "
            + (obj2 instanceof Parent));
        System.out.println("obj2 instanceof Child: "
            + (obj2 instanceof Child));
        System.out.println("obj2 instanceof MyInterface: "
            + (obj2 instanceof MyInterface));
    }
}

class Parent {}
class Child extends Parent implements MyInterface {}
interface MyInterface {}

```

输出如下：

```
obj1 instanceof Parent: true
obj1 instanceof Child: false
obj1 instanceof MyInterface: false
obj2 instanceof Parent: true
obj2 instanceof Child: true
obj2 instanceof MyInterface: true

```

<b>当使用instanceof操作符时，记住，null不是任何类型的实例！</b>

### 位运算和位移操作符

java编程语言也提供了对于整数的按位进行运算的和位移操作符。本节讨论的操作符出现的频率不高。因此，本节篇幅不大，目前是让你知道有这些操作符的存在。

一元位操作符“~”，反转一个位，它同样可以应用到整数类型，使得所有的1变成0，所有的0变成1。例如，一个byte有8位，将~操作符应用于"00000000"，结果为"11111111"。

操作符“<<” 将每一位向左移动1位，操作符 ">>"将每一位向右移动一位，二者都是有符号移位。 位模式在左，移动的位数在右。无符号右移操作符">>>"，将每一位向右移动后，在最左边补0，而有符号右移">>"会在最左边补符号位(正为0，负为1)。示例代码如下,程序输出为2：

```
class BitDemo {
    public static void main(String[] args) {
        int bitmask = 0x000F;
        int val = 0x2222;
        // prints "2"
        System.out.println(val & bitmask);
    }
}

```


















