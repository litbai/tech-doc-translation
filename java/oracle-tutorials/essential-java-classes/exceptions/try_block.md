## The try Block

构造异常处理器的第一个步骤是将可能抛出异常的代码包围在一个try代码块中，一个try块结构如下所示：

```
try {
	code
}
catch and finally blocks...

```

上述代码中，用code标志的部分，包含一行或多行代码，这段代码可能会抛出一个异常（catch和finally语句后面会解释）。


为了给ListOfNumbers类的writeList方法构造一个异常处理器，你需要将writeLsit方法可能会抛出异常的代码用try块来围住。你可以采取多种方法。你可以使用try块围住每一个可能会抛出异常的代码，然后分别来catch并处理异常。你也可以将writeList方法的所有代码放在一个try块中，一次性处理catch多个异常并处理。下面的示例使用了第二种方法，因为writeList方法比较简短：

```
private List<Integer> list;
private static final int SIZE = 10;

public void writeList() {
    PrintWriter out = null;
    try {
        System.out.println("Entered try statement");
        out = new PrintWriter(new FileWriter("OutFile.txt"));
        for (int i = 0; i < SIZE; i++) {
            out.println("Value at: " + i + " = " + list.get(i));
        }
    }
    catch and finally blocks  . . .
}

```


如果在try块中发生了错误，那么这个异常将会被关联的一个异常处理器进行处理。为了给try块关联一个异常处理器，你必须在后面加上一个或多个catch语句，下一节将详细介绍catch语句。























