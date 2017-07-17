## 字符流

Java平台使用Unicode编码来存储字符。字符流I/O自动将其内部的格式转换为本地的字符集格式。在西方locale下，本地字符集通常是8位的ASCII超集。

对于大多数应用，字符流比字节流要更简单一些。字节流相关的类会自动将输入输出转化为本地字符集，如果不知道使用的字符集，那么字符毫无意义。一个使用字符流的程序会自适应本地字符集，而且时刻准备着国际化--不需要额外的付出。

如果国际化不是优先级很高的事情，你可以直接使用字符流相关的类而不必关系字符集相关的事宜。如果后面需要考虑国际化的时候，你的程序也可以在改动不大的情况下快速实现国际化。


### 使用字符流

所有的字符流相关的类都是Reader和Writer的子类或者子接口。就像字节流一样，也有更具体的用于文件I/O操作的字符流：FileReader和FileWriter。 下面的 CopyCharacter示例展示了这些类的用法：


```
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;


public class CopyCharacters {

	public static void main(String[] args) {
		FileReader inputStream = null;
		FileWriter outputStream = null;
		
		try {
			inputStream = new FileReader("xanadu.txt");
			outputStream = new FileWriter("characteroutput.txt");
			
			int c;
			
			while ((c = inputStream.read()) != -1) {
				outputStream,writer(c);
			}
		} finally {
			if (inputStream != null) {
				inputSteam.close();
			}
			if (outputStream != null) {
				outputStream.close();
			}
		}
		
	}
	
}


```

CopyCharacters类与CopyBytes类非常类似。主要的不同点在于CopyCharacters使用了FileReader和FileWriter而不是FileInputStream和FileOutPutStream来进行输入输出。注意，CopyBytes和CopyCharacters都使用了一个int变量，作为输入输出方法的返回值和参数。但是，在CopyCharacters类中，使用的int类型的变量c使用它的后16位来持有一个character的值，而在CopyBytes中，int类型的变量b使用了它的后8位来存储一个byte的数据。


### 使用字节流的字符流

字符流通常包装了字节流。也就是说，字符流也是使用字节流来进行实际上（物理上）的I/O操作，但是字符流封装了字节和字符之间的转换操作。举例，FileReader，使用了FileInputStream，同理，FileWriter使用了FileOutPutStream。


JDK中存在两种通用的进行字节到字符转换的“bridge”流，InputStreamReader和OutputStreamWriter。如果JDK已有的字符流无法满足你需要的话，你可以使用它们来创建字符流。接下来的socket lesson和networking trail将会展示如何通过socket相关的字节流来创建字符流。


### 面向“Line”的I/O

字符I/O操作通常不仅仅是操作一个字符，而是一个unit。一个常见的unit是一行：一个字符串，结尾是一个换行符。一个换行符可能是'\r\n'(win), '\r'(linux)或者'\n'(mac)。支持所有的换行符可以使程序员读取在任意操作系统上创建的文件。

接下来让我们修改CopyCharacters类，使用面向行的I/O。我们将会使用以前没用过的BufferedReader和PrintWriter类。我们将会在Buffered I/O和Formatting两节中更深入的探讨这两个类。现在，我们仅仅使用它们提供的line-oriented I/O操作。


CopyLines类调用了BufferedReader的readLine方法和PrintWriter的println方法，来执行一次性输入输出一行的操作。


```
import java.io.FileReader;
import java.io.FileWriter;
import java.io.BufferedReader;
import java.io.PrintWriter;
import java.io.IOException;

public class CopyLines {
	public static void main(String[] args) throws IOException {
		BufferedReader br = null;
		PrintWriter pw = null;
		
		try {
		  br = new BufferedReader(new FileReader("xanadu.txt"));
		  pw = new PrintWriter(new FileWriter("characteroutput.txt"));
		  
		  String l;
		  while( (l = br.readLine()) != null) {
		  	 if (br != null) {
		  	 	br.close();
		  	 }
		  	 if (pw != null) {
		  	 	pw.close();
		  	 }
		  }
		}
	}
}

```


readLine()方法，返回一个文本行。然后通过println()方法，将这个文本输出，输出时会自动在文本的后面加一个当前OS对应的换行符，新加的换行符可能与之前的换行符不一样。


除了字符和line，还有许多其他的方式来操作文本。参考 Scanning and Formatting一节，获取更多信息。












































