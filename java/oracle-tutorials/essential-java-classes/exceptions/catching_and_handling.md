## 捕获并处理异常

这一节介绍如何使用异常处理器3个组件--try、catch和finally块--来编写一个异常处理器。然后介绍了java se 7引入的try-with-resource语句。try-with-resource语句适用于使用Closeable类型的资源时的场景。


本节的最后通过一个示例来分析当各种情况下异常发生时，具体的程序执行流程。


下面的示例代码定义并实现了类ListOfNumbers。当调用ListOfNumbers类的构造函数时，ListOfNumbers创建了一个包含10个元素的ArrayList。ListOfNumbers中还定义了一个方法writeList，这个方法将一个list列表中的值写入了一个文件OutFile.txt中：


```
// Note: This class will not compile yet.
import java.io.*;
import java.util.List;
import java.util.ArrayList;

public class ListOfNumbers {

    private List<Integer> list;
    private static final int SIZE = 10;

    public ListOfNumbers () {
        list = new ArrayList<Integer>(SIZE);
        for (int i = 0; i < SIZE; i++) {
            list.add(new Integer(i));
        }
    }

    public void writeList() {
	// The FileWriter constructor throws IOException, which must be caught.
        PrintWriter out = new PrintWriter(new FileWriter("OutFile.txt"));

        for (int i = 0; i < SIZE; i++) {
            // The get(int) method throws IndexOutOfBoundsException, which must be caught.
            out.println("Value at: " + i + " = " + list.get(i));
        }
        out.close();
    }
}

```

上述粗体部分的第一行代码是构造方法调用。这个构造方法在文件上初始化了一个输出流。如果这个文件不能被打开，这个构造器会抛出一个IOException。粗体部分的第二行代码是调用List类的get方法，如果它的参数太小(小于0)或者太大(大于list中的元素数)，这个方法会抛出一个IndexOutOfBoundException。


如果你试图编译ListOfNumbers类，编译器会打印一条错误信息，这条错误信息是关于FileWriter类的构造函数抛出一个IOException的。但是，编译器并没有打印get()方法会抛出的异常信息。这是因为由FileWriter类的构造函数抛出的异常IOException是已受检异常，但是get()方法抛出的异常IndexOutOfBoundsException，是一个未受检异常。


现在你已经熟悉了ListOfNumber类，并且知道了它可能在哪里会抛出异常，你已经准备好写一个异常处理器来捕获并处理这些异常了。






























