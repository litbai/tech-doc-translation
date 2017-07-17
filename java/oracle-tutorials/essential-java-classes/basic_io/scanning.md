### Scanning

Scanner对象具有将输入分解为一个个的token，然后将这些token转化为指定的类型的功能。

#### Breaking Input into Tokens

默认情况下，一个Scanner对象会使用space来分隔token。（White space包括blanks、tabs、换行符等， 参考Character.isWhitespace）.下面的示例程序ScanXan，读取了xanadu.txt中的每一个单词，然后将它们逐行打印。


```
import java.io.*;
import java.util.Scanner;


public class ScanXan() {
	public static void main(String[] args) {
		Scanner s = null;
		try {
			s = new Scanner(new BufferedReader(new FileReader("xanadu.txt")));
		while (s.hasNext()) {
			System.out.println(s.next());
		}
		} finally {
			if (s != null) {
				s.close();
			}
		}
	}
}

```


我们注意到，在程序的最后，调用了Scanner的close方法。虽然scanner不是一个stream，但是我们也需要在操作完成后显示的调用它的close方法，来表示我们已经对底层的stream操作完毕。


如果想自定义分隔符，那么需要调用useDelimiter()方法，指定一个正则表达式。例如，假设你想要使用','作为分隔符，后面跟上可选的空白字符，那么你可以这么指定： s.useDelimiter(",\\s*");


#### Translating Individual Tokens


上述的ScanXan类将所有的token都看做简单的字符串类型。Scanner同样支持所有的java基本数据类型(除了char)，还支持BigInteger和BigDecimal。同时，数字类型还支持千位分隔符。因此，在US locale下，Scanner可以正确的将"32,767"转换为整数。


这里，必须强调的是locale属性，因为千位分隔符和小数符号是与地区相关的。因此，下面的程序可能在别的locale无法正确的运行，如果没有指定要使用的locale为US的话。

下面的ScanSum类读取了一个double类型的列表，然后将它们相加：

```
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.Scanner;
import java.util.Locale;


public class ScanSum {
	public static void main(String[] args) throws IOException{
		Scanner s = null;
		double sum = 0;
		
		try {
			s = new Scanner(new BufferedReader(new FileReader("usnumbers.txt")));
			s.useLocale(Locale.US);
			
			while (s.hasNext()) {
				if (s.hasNextDouble()) {
					sum += s.nextDouble();
				} else {
					s.next();
				}
			} finally {
				s.close();
			}
			System.out.println(sum);
		}
	}
	
}

```

usnumbers.txt文件的内容如下：

```
8.5
32,767
3.14159
1,000,000.1

```


输出为： “1032778.74159”。不同的locale可能输出格式不同，因为System.out是一个PrintStream对象，它没有提供修改locale的方法。我们可以设置整个程序的locale，或者我们可以使用formatting，下一节将介绍formatting。

















