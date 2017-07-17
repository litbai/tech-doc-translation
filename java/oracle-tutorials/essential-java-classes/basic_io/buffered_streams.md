### Buffered Streams

到目前为止，我们见过的大多数示例使用的都是没有缓冲的I/O.这说明，每次读或者写请求都会由底层的OS来直接操作。这样的程序效率很低，因为每次读写请求都会触发访问磁盘、网络或者其他相对昂贵的操作。

为了减少这种开销，java平台实现了buffered I/O流。缓冲输入流会从一个称为"buffer"的内存区域读取数据，原生的API只会在"buffer"为空时才会被调用。同理，缓冲输出流会往"buffer"中写数据，当"buffer"满的时候，才会调用原生的write API。


一个程序可以通过包装，将一个非缓冲流包装为一个缓冲流，通过将一个非缓冲流对象传递给缓冲流对象的构造函数。接下来让我们修改CopyCharacters类，来使用具有缓冲功能的I/O。

```
inputStream = new BufferedReader(new FileReader("xanadu.txt"));
outputStream = new BufferedWriter(new FileWriter("characteroutput.txt"));

```

JDK提供了4种缓冲相关的类来包装非缓冲流： BufferedInputStream、BufferedOutputStream创建具备缓冲功能的字节流，BufferedReader和BufferedWriter创建具备缓冲功能的字符流。



### Flushing Buffered Stream

通常，在某个关键时刻需要手动将"buffer"中的数据写入磁盘，而不用等到"buffer"为满的状态，这称为 flushing the buffer。


一些具备缓冲功能的输出类支持autoflush，通过在构造函数中传递一个参数来表示是否开启autoflush。当autoflush开启的时候，一些特定的事件会引起"buffer"被flush。例如，一个开启了autoflush的PrintWriter对象，在每次调用println和format的时候都会刷新缓冲区。


如果想手动刷新缓冲区，可以直接调用flush方法。flush方法可以在任意的输出流对象上调用，但是如果这个对象不具有缓冲功能，那么调用这个方法，just do onthing。

























