### Autoboxing and Unboxing

自动装箱是指java编译器自动将基本数据类型转换为其对应的包装类的对象。例如，将一个int转换为Integer，一个double转换为Double等等。如果转换是逆向的，称为拆箱(unboxing)。

下面是最简单的自动装箱的例子：

```
Character ch = 'a';

```

剩余的例子使用了泛型。下一章将介绍泛型。考虑如下的代码：

```
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(i);

```

上述代码中，尽管你传给add()方法的是int类型，但是编译并没有报错。因为Li是一个Integer对象的list，并不是int类型的list，你可以想知道为什么编译器没有报错。编译器没有报错是因为它根据int类型的i自动创建了一个Integer对象，并且将这个Integer对象加入了li中。因为，代码运行时，其实是这样的：

```
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2)
    li.add(Integer.valueOf(i));

```

将基本数据类型的值转变为对应的包装类的对象称为autoboxing。java编译器将会在以下情况进行autoboxing：

* 向一个参数类型为对象类型的方法传递基本数据类型
* 使用基本数据类型对包装类变量进行赋值


现在，考虑如下的方法：

```
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i: li)
        if (i % 2 == 0)
            sum += i;
        return sum;
}

```

以为操作符'%'和'+='并不适用于Integer对象，你可能想知道java编译器为什么没有发布错误。java编译器没有报错是因为它调用了Integer的intValue()方法在运行时将Integer对象转为了int：

```
public static int sumEven(List<Integer> li) {
    int sum = 0;
    for (Integer i : li)
        if (i.intValue() % 2 == 0)
            sum += i.intValue();
        return sum;
}

```

将一个包装类对象转换为对应的基本数据类型的过程称为unboxing。java编译器会在以下情况对一个包装类对象进行unboxing操作：

* 向参数为基本数据类型的的方法传递一个包装类对象参数
* 将包装类对象赋值给基本数据类型变量


```
import java.util.ArrayList;
import java.util.List;

public class Unboxing {

    public static void main(String[] args) {
        Integer i = new Integer(-8);

        // 1. Unboxing through method invocation
        int absVal = absoluteValue(i);
        System.out.println("absolute value of " + i + " = " + absVal);

        List<Double> ld = new ArrayList<>();
        ld.add(3.1416);    // Π is autoboxed through method invocation.

        // 2. Unboxing through assignment
        double pi = ld.get(0);
        System.out.println("pi = " + pi);
    }

    public static int absoluteValue(int i) {
        return (i < 0) ? -i : i;
    }
}

```


自动装箱和拆箱的特性可以使开发者编写干净、易读的代码。下面的表格列出了基本数据类型和对应的包装类，java编译器会根据这个表格信息进行autoboxing和unboxing：

|Privitive type|Wrapper class|
|boolean|Boolean|
|byte|Byte|
|char|Character|
|float|Float|
|int|Integer|
|long|Long|
|short|Short|
|double|Double|

























