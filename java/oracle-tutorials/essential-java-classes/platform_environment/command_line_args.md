### Command-Line Arguments

一个java应用可以在命令行中接收任意数量的参数。这样，用户可以在应用启动的时候传递一些配置信息。

当用户启动应用的时候，在命令行中输入参数，参数的位置位于类名之后。例如，假设一个应用名为Sort，其功能是对文件按行进行排序。为了使其对friends.txt文件的内容进行排序，可以在命令行中输入文件名参数：

```
java Sort friends.txt

```


当一个应用启动时，运行时系统会将命令行参数以字符串数组的方式传递给应用的main方法。在前面的例子中，传递给main方法的参数是一个字符串数组，但是它只包含一个元素："friends.txt"。


#### Echoing Command-Line Arguments


下面的Echo程序会打印命令行中传递的所有参数：

```
public class Echo {
    public static void main (String[] args) {
        for (String s: args) {
            System.out.println(s);
        }
    }
}

```

```
java Echo Drink Hot Java
Drink
Hot
Java

```

注意，输出中每个单词单独占用一行。因为，应用默认使用空白字符来分隔命令行参数，如果想将Drink, Hot, Java表示成一个参数，则需要使用""将它们括起来：


```
java Echo "Drink Hot Java"

Drink Hot Java

```


#### 解析命令行中的“数字”参数


如果一个应用需要支持数字类型的命令行参数，它必须将字符串类型的参数转换成数字，示例代码如下所示：

```
int firstArg;
if (args.length > 0) {
	try {
		firstArg = Integer.parseInt(args[0]);
	} catch (NumberFormatException e) {
		System.out.println("Argument " + args[0] + " must be an integer.");
		System.exit(1);
	}
}

```

parseInt方法会抛出一个NumberFormatException，如果args[0]的格式不符合数字格式的话。所有的Number类--Integer、Long、Float、Double等，都有一个parseXXX方法，可以将字符串转化为数字。



















