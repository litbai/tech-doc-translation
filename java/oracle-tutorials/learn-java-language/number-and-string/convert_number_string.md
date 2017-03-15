### 转换数字和字符串

#### 将字符串转换为数组

我们经常需要将用户输入的字符串转换为数字。Number类的子类（Byte, Short, Integer, Float, Double等）都提供了一个valueOf方法，可以将一个字符串转换为一个数字。下面是一个示例，它从命令行接受用户输入的两个字符串参数，然后将这两个参数转换为数字，最后对这两个数字作了一些算术运算：

```
public class ValueOfDemo {
    public static void main(String[] args) {

        // this program requires two 
        // arguments on the command line 
        if (args.length == 2) {
            // convert strings to numbers
            float a = (Float.valueOf(args[0])).floatValue(); 
            float b = (Float.valueOf(args[1])).floatValue();

            // do some arithmetic
            System.out.println("a + b = " +
                               (a + b));
            System.out.println("a - b = " +
                               (a - b));
            System.out.println("a * b = " +
                               (a * b));
            System.out.println("a / b = " +
                               (a / b));
            System.out.println("a % b = " +
                               (a % b));
        } else {
            System.out.println("This program " +
                "requires two command-line arguments.");
        }
    }
}

```

当在命令行中输入4.5和87.2时，上述程序的输出结果如下所示：

```
a + b = 91.7
a - b = -82.7
a * b = 392.4
a / b = 0.0516055
a % b = 4.5

```

** 注意，每一个Number的子类，都提供了一个parseXXXX()方法，可以将一个字符串装换为一个原始数据类型。因为这个方法直接返回一个原始数据类型，所以上述程序如果使用parseFloat()方法，将更直接。**

```
float a = Float.parseFloat(args[0]);
float b = Float.parseFloat(args[1]);

```

#### 数组转换为字符串

有时你需要将一个数字转换为字符串。下面是一些方法：

```
int i;
// Concatenate "i" with an empty string; conversion is handled for you.
String s1 = "" + i;

// The valueOf class method.
String s2 = String.valueOf(i);

```

每一个Number的子类都包含一个方法toString()，可以将原始数据类型转换为字符串：

```
int i;
double d;
String s3 = Integer.toString(i); 
String s4 = Double.toString(d); 

```

```
public class ToStringDemo {
    
    public static void main(String[] args) {
        double d = 858.48;
        String s = Double.toString(d);
        
        int dot = s.indexOf('.');
        
        System.out.println(dot + " digits " +
            "before decimal point.");
        System.out.println( (s.length() - dot - 1) +
            " digits after decimal point.");
    }
}


```

上述程序的输出为：

```
3 digits before decimal point.
2 digits after decimal point.

```
















