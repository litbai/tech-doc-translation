## 声明方法会抛出的异常

前面的章节展示了如何编写一个异常表处理器。有时，在异常发生时及时捕获并处理异常是合适的，但是在另一些情况下，交给call stack中更高层次的方法来处理这个异常可能是更好的方式。例如，如果你提供了ListOfNumbers类，你可能不会考虑到所有会使用这个类的开发者的所有需求。在这种情况下，不去捕获异常，而是允许让call stack中更高层次的方法（开发者自己写的方法）去捕获并处理这个异常可能是更好的方式。


如果writeList方法么有捕获已受检异常，它必须声明可能抛出的异常类型，通过使用throw语法。让我们来修改原始的writeList方法，为此方法声明可能会抛出的异常而不是在方法中catch这个异常, 下面是原始的writeList方法：

```
public void writeList() {
	PrintWriter out = new PrintWriter(new FileWriter("OutFile.txt"));
	for (int i = 0; i < SIZE; i++) {
		out.println("Value at: " + i + " = " + list.get(i));
	}
	out.close();
}

```

为了声明writeList方法会抛出异常，可以在方法声明中加上throw分局。throw句由throws关键字和一个由逗号分隔的异常类型组成。throw子句位于方法的参数后面，方法体前面，示例如下：

```

public void writeList() throws IOException, IndexOutOfBoundsException {
	// ...
}

```

还记得吗？IndexOutOfBoundsException是一个未受检异常，所以throws子句中可以不包括这种类型异常的声明，非强制的。你可以如下声明：

```
public void writeList() throws IOException {
	// ...
}

```



































