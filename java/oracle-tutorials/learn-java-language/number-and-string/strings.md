### Stirngs

字符串作为一个字符序列，在java编程语言中使用广泛，在java编程语言中，字符串是一个对象。

Java平台提供了String类来操作字符串。

#### 创建一个字符串

创建字符串的最直接的方式为：

```
String greeting = "Hello world!";

```

上述代码中，"Hello world!"是一个字符串字面量--在双引号之间的一个字符序列。当你在程序中遇到一个字符串时，编译器会根据字符串的值创建一个String对象--此例中，字符串的值为Hello world!


类似创建其他的对象，你也可以使用new关键字来创建一个字符串对象。String类有13个构造器，可以接受各种类型的不同参数来创建一个字符串，例如可以接受一个字符数组：

```
char[] helloArray = {'h', 'e', 'l', 'l', 'o'};
String hello = new String(helloArray);
System.out.println(hello);

```

** 注意，String类是不可变类，一旦创建了一个String对象，则它不可改变。String类有很多方法，下面我们会讨论其中的一些方法，这些方法看起来像是要改变一个字符串。但是，因为字符串是不可变的，所以这些方法实际上只是创建并返回了一个包含操作结果的新对象而已。**

#### 字符串长度

获取一个对象信息的方法称为访问器方法。你可以使用的String类的一个访问器方法是length()方法，返回字符串中包括的字符的个数。示例如下：

```
String palindrome = "Dot saw I was Tod";
int len = palindrome.length();

```


一个回文指的是一个单词或句子，它是对称的--从前往后或者从后往前拼都是一样的，不区分大小写和标点符号。下面是一个简单但是效率很低的翻转一个回文串额方法。它调用了String的charAt()方法：

```
public class StringDemo {
    public static void main(String[] args) {
        String palindrome = "Dot saw I was Tod";
        int len = palindrome.length();
        char[] tempCharArray = new char[len];
        char[] charArray = new char[len];
        
        // put original string in an 
        // array of chars
        for (int i = 0; i < len; i++) {
            tempCharArray[i] = 
                palindrome.charAt(i);
        } 
        
        // reverse array of chars
        for (int j = 0; j < len; j++) {
            charArray[j] =
                tempCharArray[len - 1 - j];
        }
        
        String reversePalindrome =
            new String(charArray);
        System.out.println(reversePalindrome);
    }
}

```


程序输出如下：

```
doT saw I was toD

```

为了完成字符串的反转，上述程序首先将字符串转化为一个字符数组，然后反转这个字符数组，然后根据这个反转的数组创建一个新字符串。String类有一个方法，getChars()，可以将一个字符串或者字符串的一部分，转换成一个字符数组，所以我们可以用如下代码来替代for循环：

```
palindrome.getChars(0, len, tempCharArray, 0);

```


#### 连接字符串

String类有一个方法，可以连接两个字符串：

```
string1.concat(string2); 

```

你也可以连接两个字符串字面量：

```
"My name is ".concat("Rumplestiltskin");

```

更常见的连接两个字符串的操作是通过'+'操作符：

```
"Hello," + " world" + "!"

```

'+'操作符在print语句中使用的非常广泛：

```
String string1 = "saw I was ";
System.out.println("Dot " + string1 + "Tod");

```

这种连接可以连接多个不同的对象，对于非String类型的对象，将会调用它的toString()方法将对象装换为String。

** 注意，java编程语言不允许字面量字符串跨越多行，所以，你必须在每一行的末尾使用'+'操作符来连接多于1行的字符串，例如下面这种写法在print语句中也非常常见。 **

```
String quote = 
    "Now is the time for all good " +
    "men to come to the aid of their country.";

```


#### 创建格式化字符串

你已经使用过printf和format方法来打印格式化的数字。String类有一个等价的方法format(),这个方法可以返回一个格式化后的String对象。

使用String的静态format方法，你可以创建一个格式化的字符串，你可以重复使用这个字符串，而不是把它一次性的打印出来。例如，你可以使用：

```
System.out.printf("The value of the float " +
                  "variable is %f, while " +
                  "the value of the " + 
                  "integer variable is %d, " +
                  "and the string is %s", 
                  floatVar, intVar, stringVar); 
                  
// 你可以使用如下format方法替换上述的打印方法
// 这样的话，你不仅可以把格式化的字符串打印出来，还可以留作它用

String fs;
fs = String.format("The value of the float " +
                   "variable is %f, while " +
                   "the value of the " + 
                   "integer variable is %d, " +
                   " and the string is %s",
                   floatVar, intVar, stringVar);
System.out.println(fs);


```































