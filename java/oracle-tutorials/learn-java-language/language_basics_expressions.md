## Expressions, Statements, and Blocks

我们已经学习了变量和操作符，是时候学习表达式、语句和代码块了。操作符可以用来构建表达式，用来计算值；表达式是语句的核心组成部分，语句可以组织成代码块。

### 表达式

一个表达式是由变量、操作符、方法调用组成的一个结构，根据语言的语法来构建的，计算得到一个值。你已经见过简单的表达式例子，如下所示：

```
int cadence = 0;
anArray[0] = 100;
System.out.println("Element 1 at index 0: " + anArray[0]);

int result = 1 + 2; // result is now 3
if (value1 == value2) 
    System.out.println("value1 == value2");
```

表达式的返回类型由表达式中使用的元素决定。表达式 cadence = 0 返回一个int，因为赋值操作符返回与它左边数值相同的数据类型。在这个例子中，cadence是int。表达式也可以返回其他类型，例如boolean或者String。

java编程语言允许你使用简单的表达式构建复杂的表达式，只要类型相符。例如 1 * 2 * 3

### 语句

语句大体类似于自然语言中的句子。一个语句组成了一个完成的执行单元。下面的表达式可以通过在尾部加上；构成一个语句：

* 复制表达式
* ++ -- 表达式
* 方法调用
* 对象创建表达式

上述的语句称为表达式语句，例如：

```
// assignment statement
aValue = 8933.234;
// increment statement
aValue++;
// method invocation statement
System.out.println("Hello World!");
// object creation statement
Bicycle myBike = new Bicycle();

```

除了表达式语句，还有另外两种语句：声明语句和控制流语句。声明语句声明了一个变量，我们已经见过例子：double a = 8933.234;最后，控制流语句控制哪个语句将被执行，下一节将会介绍控制流语句。

### 代码块

一个代码块是一组语句，它们位于一个括号中，代码块可以出现在任何可以出现单语句的地方。例如：

```
class BlockDemo {
     public static void main(String[] args) {
          boolean condition = true;
          if (condition) { // begin block 1
               System.out.println("Condition is true.");
          } // end block one
          else { // begin block 2
               System.out.println("Condition is false.");
          } // end block 2
     }
}


```






























