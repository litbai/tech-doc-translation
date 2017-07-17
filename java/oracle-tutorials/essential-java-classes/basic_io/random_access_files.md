### Random Access Files

随机访问文件，正如名字所示，允许非顺序的或称为随机的访问文件的内容。为了随机的访问文件，你需要打开文件，改变当前的读写位置，然后从这个位置开始读文件或写文件。


随机访问功能由接口SeekableByteChannel接口定义。SeekableByteChannel扩展了Channel I/O，引入了position的概念。你可以设置和查询当前的position，然后从当前的position进行数据读写。SeekableByteChannel接口包含如下几个易用的方法：

* position() 返回channel当前的position
* position(long) 设置channel的positon
* read(ByteBuffer) 从channel读取数据到buffer中
* write(ByteBuffer) 将buffer中的数据写入channel
* truncate(long) 截断与channel连接的文件

上一节讲到了，Path.newByteChannel方法会返回一个SeekableByteChannel的实例。在默认的文件系统中，你可以直接使用这个channel，或者你也可以将它强制转换为FileChannel，这样你可以使用FileChannel更多的特性,例如你可以使用内存映射文件来提高访问速度、锁定文件的一块区域、或者从一个绝对的位置来读写byte而不影响channel当前的position。


下面的代码片段打开了一个文件，然后使用newByteChannel方法来对文件进行了读写。返回的 SeekableByteChannel被强转为FileChannel。然后，从文件开头处读取了12字节数据，然后从这个位置写入了"I was here!"这个字符串。接下来文件的position被设置到最后，先前读取的12字节数据被append到文件尾端。最后，"I was here!"被append到文件尾端，文件关闭。

```
String s = "I was here!\n";
byte[] data = s.getBytes();

ByteBuffer out = ByteBuffer.wrap(data);
ByteBuffer copy = ByteBuffer.allocate(12);

try (FileChannel fc = FileChannel.open(file, READ, WRITE)){
	int nread;
	do {
		nread = fc.read(copy);
	} while(nread != -1 && copy.hasRemaining());
	
	fc.position(0);
	while (out.hasRemaining()) {
		fc.write(out);
	}
	out.rewind();
	
	long length = fc.size();
	fc.position(length - 1);
	copy.flip();
	while (copy.hasRemaining()) {
		fc.write(copy);
	}
	while (out.hasRemaining()) {
		fc.write(out);
	}
} catch (IOException x) {
	System.out.println("I/O Exception: " + x);
}

```




























