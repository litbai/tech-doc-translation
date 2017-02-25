## 控制流语句

源代码中的语句通常是从上到下按顺序执行的。控制流语句，可以打破顺序执行的壁垒，它通过判断、循环、分支，使你的程序有条件的执行特定的代码块。这一章描述了java编程语言支持的判断语句（if-then, if-then-else,switch），循环语句（for, while, do-while）和分支语句（break, continue, return）。

### 判断语句

##### if-then 语句

if-then语句是控制流语句中最基本的一个语句。它告诉你的程序当特定的情况发生的时候执行特定的代码。例如，Bicycle类只有当车子在运行的时候才允许通过刹车减速。一个可能的applyBrakes方法的实现如下：

```
void applyBrakes() {
    // the "if" clause: bicycle must be moving
    if (isMoving){ 
        // the "then" clause: decrease current speed
        currentSpeed--;
    }
}

```

如果判断条件为false，则程序跳到if-then语句之后的部分。除此之外，如果then语句只包含一条语句的话，大括号可以省略，但不推荐这么做。是否去掉大括号是个人的编程风格问题，去掉大括号可能使程序变得脆弱。常见的错误是当then语句中需要加入第二条语句，此时很容易忘记加上大括号，编译器会帮你发现这一点，你会得到错误信息。

```
void applyBrakes() {
    // same as above, but without braces 
    if (isMoving)
        currentSpeed--;
}

```

##### if-then-else 语句

if-then-else语句提供了第二条可能的路径，当if语句的判断条件为false的时候。你可以在applyBrakes方法中使用if-then-else语句，当车子在没有运动的状态下执行刹车操作时做另外一些动作。下面的例子中，这个动作只是简单地打印一条错误信息提示自行车已经停止。

```
void applyBrakes() {
    if (isMoving) {
        currentSpeed--;
    } else {
        System.err.println("The bicycle has already stopped!");
    } 
}

```

下面的程序IfElseDemo，根据测试分数分配一个等级，得到90以上，得到A，80以上得到B，等等。

```
class IfElseDemo {
    public static void main(String[] args) {
        int testscore = 76;
        char grade;

        if (testscore >= 90) {
            grade = 'A';
        } else if (testscore >= 80) {
            grade = 'B';
        } else if (testscore >= 70) {
            grade = 'C';
        } else if (testscore >= 60) {
            grade = 'D';
        } else {
            grade = 'F';
        }
        System.out.println("Grade = " + grade);
    }
}

```

此例中，你可能已经注意到了，testscore的值满足不止一个条件，比如76>=70,同时76>=60，但是，一旦一个条件满足了，相应的语句就会执行，剩余的条件不会再被评估。


### switch语句

与if-then和if-then-else语句不同的是，switch语句可以有很多可能的执行路径。一个switch语句可以接受char，byte，short，int和它们的包装类型，以及enum、String类型。

下面的代码，SwitchDemo，声明了一个名字为month的int型变量，代码根据month的值打印月份的名字：

```
public class SwitchDemo {
    public static void main(String[] args) {

        int month = 8;
        String monthString;
        switch (month) {
            case 1:  monthString = "January";
                     break;
            case 2:  monthString = "February";
                     break;
            case 3:  monthString = "March";
                     break;
            case 4:  monthString = "April";
                     break;
            case 5:  monthString = "May";
                     break;
            case 6:  monthString = "June";
                     break;
            case 7:  monthString = "July";
                     break;
            case 8:  monthString = "August";
                     break;
            case 9:  monthString = "September";
                     break;
            case 10: monthString = "October";
                     break;
            case 11: monthString = "November";
                     break;
            case 12: monthString = "December";
                     break;
            default: monthString = "Invalid month";
                     break;
        }
        System.out.println(monthString);
    }
}

```

上面的代码，将打印August。switch语句的主体称为switch block。在switch block中的一条语句可以被1个或多个case语句或者default语句来标记。switch语句计算它的表达式，然后执行满足条件的case标签后面的所有语句。

决定使用if—then-else语句还是switch语句的依据是：代码的可读性以及充当判断条件的表达式。if-then语句可以根据值得范围或者条件来判定表达式，但是switch语句仅仅可根据单一的整数值、枚举值或者String对象来进行条件判读。

另一个有意思的知识点是break语句。一条break语句终止了switch语句的执行。如果没有break语句，那么在满足条件的case标签后的所有的语句都将被顺序的执行，而不管后续的case语句中的表达式是否为true。示例代码：

```
public class SwitchDemoFallThrough {

    public static void main(String[] args) {
        java.util.ArrayList<String> futureMonths =
            new java.util.ArrayList<String>();

        int month = 8;

        switch (month) {
            case 1:  futureMonths.add("January");
            case 2:  futureMonths.add("February");
            case 3:  futureMonths.add("March");
            case 4:  futureMonths.add("April");
            case 5:  futureMonths.add("May");
            case 6:  futureMonths.add("June");
            case 7:  futureMonths.add("July");
            case 8:  futureMonths.add("August");
            case 9:  futureMonths.add("September");
            case 10: futureMonths.add("October");
            case 11: futureMonths.add("November");
            case 12: futureMonths.add("December");
                     break;
            default: break;
        }

        if (futureMonths.isEmpty()) {
            System.out.println("Invalid month number");
        } else {
            for (String monthName : futureMonths) {
               System.out.println(monthName);
            }
        }
    }
}

```

上面代码的输出为：

```
August
September
October
November
December

```

从技术的角度来说，最后的break语句可以省略不写，因为代码执行到最后会自己跳出switch语句。我们推荐使用break语句，因为这更容易修改代码和减少出错的可能。default部分会处理所有没有被case语句处理的情况。下面的代码，SwitchDemo2，显示了一个语句可能有多个case标签：

```
class SwitchDemo2 {
    public static void main(String[] args) {

        int month = 2;
        int year = 2000;
        int numDays = 0;

        switch (month) {
            case 1: case 3: case 5:
            case 7: case 8: case 10:
            case 12:
                numDays = 31;
                break;
            case 4: case 6:
            case 9: case 11:
                numDays = 30;
                break;
            case 2:
                if (((year % 4 == 0) && 
                     !(year % 100 == 0))
                     || (year % 400 == 0))
                    numDays = 29;
                else
                    numDays = 28;
                break;
            default:
                System.out.println("Invalid month.");
                break;
        }
        System.out.println("Number of Days = "
                           + numDays);
    }
}

```

##### 在switch语句中使用String

在java SE7及以后的版本，你可以在switch语句中使用String对象。如下所示：

```
public class StringSwitchDemo {

    public static int getMonthNumber(String month) {

        int monthNumber = 0;

        if (month == null) {
            return monthNumber;
        }

        switch (month.toLowerCase()) {
            case "january":
                monthNumber = 1;
                break;
            case "february":
                monthNumber = 2;
                break;
            case "march":
                monthNumber = 3;
                break;
            case "april":
                monthNumber = 4;
                break;
            case "may":
                monthNumber = 5;
                break;
            case "june":
                monthNumber = 6;
                break;
            case "july":
                monthNumber = 7;
                break;
            case "august":
                monthNumber = 8;
                break;
            case "september":
                monthNumber = 9;
                break;
            case "october":
                monthNumber = 10;
                break;
            case "november":
                monthNumber = 11;
                break;
            case "december":
                monthNumber = 12;
                break;
            default: 
                monthNumber = 0;
                break;
        }

        return monthNumber;
    }

    public static void main(String[] args) {

        String month = "August";

        int returnedMonthNumber =
            StringSwitchDemo.getMonthNumber(month);

        if (returnedMonthNumber == 0) {
            System.out.println("Invalid month");
        } else {
            System.out.println(returnedMonthNumber);
        }
    }
}

```

switch中表达式中的String，将与case语句中的表达式进行对比，就像使用String.equals方法一样。同时，在switch和case中的表达式都不能为null。switch语句支持String只是编译器层面的实现，在编译的过程中，编译器会根据源代码的含义进行转换，将字符串类型转换成与整数类型兼容的格式。不同的Java编译器可能采用不同的方式来转换，并采用不同的优化策略。经过转换，Java 虚拟机看到的仍然是与整数类型兼容的类型。这里要注意的是，在case字句中对应的语句块中仍然需要使用String的equals方法来进行字符串比较，这是因为哈希函数在映射的时候可能存在冲突，这样更加保险。例如：

```
public class StringSwitchDemo {

    public static int getMonthNumber(String month) {

        int monthNumber = 0;

        if (month == null) {
            return monthNumber;
        }

        switch (month.toLowerCase()) {
            case "january":
            	if("january".equals(month.toLowerCase())){
                	monthNumber = 1;
                	break;
                }
            case "february":
            	if("february".equals(month.toLowerCase())) {
                	monthNumber = 2;
                	break;
                }
            default: 
                monthNumber = 0;
                break;
        }

        return monthNumber;
    }


```

## while和do-while语句

while语句会在特定的条件满足时，持续不断的执行一段代码。它的语法为：

```
while (expression) {
	statement(s)
}

```

while语句首先计算表达式，此表达式返回一个Boolean值。如果表达式的值为true，while语句会执行statement(s)。while语句会不断的判断表达式的值，知道表达式为false，终止执行。利用循环语句打印1到10的代码如下：

```
class WhileDemo {
    public static void main(String[] args){
        int count = 1;
        while (count < 11) {
            System.out.println("Count is: " + count);
            count++;
        }
    }
}

```

java编程语言同样提供了do-while语句，语法如下：

```
do {
     statement(s)
} while (expression);


```

do-while和while之间的区别在于，do-while语句在循环的底部才判断条件是否满足，因此，do-while中的语句至少会执行一次，如下所示：

```
class DoWhileDemo {
    public static void main(String[] args){
        int count = 1;
        do {
            System.out.println("Count is: " + count);
            count++;
        } while (count < 11);
    }
}

```

### for语句

for语句提供了一种紧凑的方式，来对数据范围进行迭代。程序员通常称其为for循环，因为它跟循环语句的工作方式很类似。for语句的通常结构如下：

```
for (initialization; termination; increment) {
    statement(s)
}

```

当使用上述的for语法时，注意：

* initialization表达式初始化循环，它只在循环开始的时候执行一次
* 当termination表达式为false时，循环终止
* increment表达式在每次循环之后调用，常见形式为增加或者减少一个值

下面的程序演示了使用for循环来打印1到10之间的数字：

```
class ForDemo {
    public static void main(String[] args){
         for(int i=1; i<11; i++){
              System.out.println("Count is: " + i);
         }
    }
}

```

注意，上述的代码是如何在initialization表达式中声明变量的。变量的有效范围是从声明的地方一直到for循环的最后一条语句，所以，在initialization中声明的变量也可以在termination和increment表达式中使用。如果for循环中的变量不需要在循环体外使用的话，那么在initialization表达式中声明此变量是一个好的实践。变量名i,j,k通常用来控制循环，在initialization表达式中声明这些变量，限制了它们的生存范围，同时会避免一些错误。

for循环中的3个表达式是可选的，例如，你可以使用如下代码来创建一个无限循环：

```
// infinite loop
for ( ; ; ) {
    
    // your code goes here
}

```

for循环还有另外一种形式，专门用来对Collections和Arrays进行迭代。这种形式通常被称为增强的for循环，它可以使你的循环更紧凑、易读。示例如下：

```
class EnhancedForDemo {
    public static void main(String[] args){
         int[] numbers = 
             {1,2,3,4,5,6,7,8,9,10};
         for (int item : numbers) {
             System.out.println("Count is: " + item);
         }
    }
}

```

我们推荐你在可以使用增强的for循环的时候尽量使用它来代替原先的遍历形式。

### 分支语句

##### break语句

break语句分为两种，带标签的和不带标签的。你已经在switch语句中见过不带标签的形式。你也可以使用不带标签的break语句来终止一个for，while以及do-while循环，如下所示：

```
class BreakDemo {
    public static void main(String[] args) {

        int[] arrayOfInts = 
            { 32, 87, 3, 589,
              12, 1076, 2000,
              8, 622, 127 };
        int searchfor = 12;

        int i;
        boolean foundIt = false;

        for (i = 0; i < arrayOfInts.length; i++) {
            if (arrayOfInts[i] == searchfor) {
                foundIt = true;
                break;
            }
        }

        if (foundIt) {
            System.out.println("Found " + searchfor + " at index " + i);
        } else {
            System.out.println(searchfor + " not in the array");
        }
    }
}

```

上述程序在一个数组中寻找数字12。当找到12时，break语句终止了for循环，程序继续执行for循环之后的语句。

不带标签的break语句终止最深层次的switch、for、while和do-while循环，但是，一个带标签的break语句，可以用来终止外层的循环体。下面的程序与上一个程序类似，但是使用了嵌套的for循环在二维数组中搜寻特定的值。当目标值找到时，带标签的break语句终止了外层的for循环。

```
class BreakWithLabelDemo {
    public static void main(String[] args) {

        int[][] arrayOfInts = { 
            { 32, 87, 3, 589 },
            { 12, 1076, 2000, 8 },
            { 622, 127, 77, 955 }
        };
        int searchfor = 12;

        int i;
        int j = 0;
        boolean foundIt = false;

    search:
        for (i = 0; i < arrayOfInts.length; i++) {
            for (j = 0; j < arrayOfInts[i].length; j++) {
                if (arrayOfInts[i][j] == searchfor) {
                    foundIt = true;
                    break search;
                }
            }
        }

        if (foundIt) {
            System.out.println("Found " + searchfor + " at " + i + ", " + j);
        } else {
            System.out.println(searchfor + " not in the array");
        }
    }
}

```

break语句终止了带标签的语句，注意，它不是将控制流转移到标签上，而是跳出标签，将控制流转移到标签之后的语句。

##### continue语句

continue语句会跳过当前正在迭代的for、while、do-while循环，进入下一次迭代。无标签的continue形式会跳到最内部的循环体的底部，然后重新开始下一次循环。下面的程序统计了一个字符串中包含字母p的个数：

```

class ContinueDemo {
    public static void main(String[] args) {

        String searchMe = "peter piper picked a " + "peck of pickled peppers";
        int max = searchMe.length();
        int numPs = 0;

        for (int i = 0; i < max; i++) {
            // interested only in p's
            if (searchMe.charAt(i) != 'p')
                continue;

            // process p's
            numPs++;
        }
        System.out.println("Found " + numPs + " p's in the string.");
    }
}

```

带标签的continue语句会跳过外部被标签标记的循环体的当前一次循环，示例如下：

```
class ContinueWithLabelDemo {
    public static void main(String[] args) {

        String searchMe = "Look for a substring in me";
        String substring = "sub";
        boolean foundIt = false;

        int max = searchMe.length() - substring.length();

    test:
        for (int i = 0; i <= max; i++) {
            int n = substring.length();
            int j = i;
            int k = 0;
            while (n-- != 0) {
                if (searchMe.charAt(j++) != substring.charAt(k++)) {
                    continue test;
                }
            }
            foundIt = true;
                break test;
        }
        System.out.println(foundIt ? "Found it" : "Didn't find it");
    }
}

```

##### return 语句

最后一个分支语句是return语句。return语句终止了当前方法的执行，控制流转移到方法被调用的地方。return语句有两种形式，一种是返回一个值，另一种不返回值。return后的值得类型必须与方法返回值一致，当方法返回值为void时，用不带返回值的return语句形式。示例：

```
return count; // 返回一个值
return; // 不带返回值
```





































