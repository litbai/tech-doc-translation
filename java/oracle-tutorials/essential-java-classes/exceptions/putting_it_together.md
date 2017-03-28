## Putting It All Together

前面的章节介绍了如何给ListOfNumbers类的writeList方法构建try, catch 和finally块。本节，我们会分析整段代码，看看程序的运行流程。


当把所有的组件都放在一起后，writeList方法如下：

```
public void writeList() {
    PrintWriter out = null;

    try {
        System.out.println("Entering" + " try statement");

        out = new PrintWriter(new FileWriter("OutFile.txt"));
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
        }
    } catch (IndexOutOfBoundsException e) {
        System.err.println("Caught IndexOutOfBoundsException: "
                           +  e.getMessage());
                                 
    } catch (IOException e) {
        System.err.println("Caught IOException: " +  e.getMessage());
                                 
    } finally {
        if (out != null) {
            System.out.println("Closing PrintWriter");
            out.close();
        } 
        else {
            System.out.println("PrintWriter not open");
        }
    }
}

```

前面提到过，上述方法中的try块有3种路径，下面列出其中两种：

1. try块中的语句执行失败，抛出异常。它可能是执行new FileWriter()时抛出的IOException，也可能是执行list.get(i)时抛出的IndexOutOfBoundsException。
2. try块正常执行，没有异常。

下面让我们一起来看一下，当发生上述两种情况时，分别发生了什么。


### 情景1：发生异常

假设执行new FileWriter()时发生了错误，抛出了一个IOException。当FileWriter抛出一个IOException时，运行时系统马上停止执行try块，方法也停止执行。然后，运行时系统会从call stack的顶端开始寻找一个合适的异常处理器。在本例中，当IOException发生时，FileWriter的构造函数位于调用栈的顶端。但是，FileWriter的构造函数并没有一个合适的异常处理器，因此，运行时系统检查方法调用栈的下一个方法--writeList方法(在此之前应该先检查new PrintWriter()？？)。writeList方法有两个异常处理器--一个可以处理IOException，另一个可以处理IndexOutOfBoundsException。


运行时系统会按catch块的顺序来逐一匹配异常处理器。第一个catch块的参数的IndexOutOfBoundsException，不匹配。接下来catch块的参数是IOException，匹配成功，此时，运行时系统停止搜寻。现在，运行时系统已经找打了一个合适的异常处理器，这个异常处理器中的代码将会被执行。


当异常处理器执行完毕，运行时系统将控制权给到finally块。finally块中的代码将被执行，无论上述异常是怎么处理的。在这个场景下，FileWriter没有被成功的open，因此也无需close。当finally块中的代码执行结束，程序会继续执行finally块之后的语句。


下面是当抛出IOException时，ListOfNumbers程序的输出：

```
Entering try statement
Caught IOException: OutFile.txt
PrintWriter not open 

```


### 场景2： try块正常执行，无异常

在这个场景中，try块中的语句都成功的执行，没有异常被抛出。当执行完try块中的语句时，运行时系统将控制权给到了finally块。因为程序运行正常，PrintWriter被open了，所有在finally块中，它可以被关闭。finally块执行完毕后，程序继续执行finally块后面的第一条语句。


下面是当没有异常产生时，程序的输出：

```

Entering try statement
Closing PrintWriter


```






























































