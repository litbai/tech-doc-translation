### Data Streams

数据流支持对基本数据类型和String类型的二进制I/O操作。所有的数据流都实现了DataInput或者DataOutput接口。本节讨论实现了上述接口的使用最广泛的两个实现类，DataInputStream和DataOutputStream。


下面的DateStreams示例(http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/essential/io/examples/DataStreams.java)，首先往文件里写了一些记录，然后从文件中读取了这些data。每一条记录包含于发票有关的3个数值，如下表所示：

|order in record|data type|data desc|output method|input method|sample value|
|-------|-------|-------|------|------|------|----|
|1|double|item price|DataOutputSteam.writeDouble|DataInputStream.readDouble|19.99|
|2|int|unit count|DataOutputSteam.writeInt|DataInputStream.readInt|12|
|3|String|item desc|DataOutputSteam.writeUTF|DataOutputSteam.readUTF|"Java T-shirt"|


让我们一起来看下DataStreams类的核心代码。首先，程序定义了一些常量，包含需要写入的文件名和模拟的各项数据：

```
static final String dataFile = "invoicedata";

static final double[] prices = {19.99, 9.99, 15.99, 3.99, 4.99};
static final int[] units = {12, 8, 13, 29, 50};
static final String[] descs = {"Java T-shitr", "Java Mug", "Duck Juggling Dolls", "Java Pin", "Java Key Chain"};


```


接下来，DataStreams打开了一个OutputStream。因为DataOutputStream的实例仅仅可以通过包装已有的字节流对象来创建，这里包装了一个BufferedOutputStream对象。

```
out = new DataOutputStream(new BufferedOutputStream(new FileOutputStream(dataFile)));

```


然后，DataStreams将这些模拟的记录写入输出流：

```
for (int i = 0; i < prices.length; i++) {
	out.writeDouble(prices[i]);
	out.writeInt(units[i]);
	out.writeUTF(descs[i]);
}

```

writeUTF方法将使用UTF-8编码来写String。UTF-8是一种可变长度的编码，对于普通的英文字符，它仅仅占用一个字节空间，且跟ASCII的编码一致。


接下来，DataStreams开始读取刚才创建的文件。首先，它创建了一个输入流，和一些变量用来保存读取的值，像DataOutputStream一样，DataInputStream实例也只能通过包装已有的字节流来创建。


```
in = new DataInputStream(new BufferedInputStream(new FileInputStream(dataFile)));

double price;
int unit;
String desc;
double total = 0.0;

try {
	while (true) {
		price = in.readDouble();
		unit = in.readInt();
		desc = in.readUTF();
		System.out.format("You ordered %d units of %s at $%.2f%n", unit, desc, price);
		total += unit * price;
	}
} catch (EOFException e) {
}

```


** 注意，DataStreams通过捕获一个EOFException来检测是否已读到文件末尾，而不是通过返回码（例如-1）。所有DataInput的实现类都使用了EOFException来替代返回码，因为读取的值本身就可能为-1，所以不能通过-1来表示EOF。**


同时，我们注意到，每一次特定的读操作都对应一个特定的写操作。读写的匹配性由程序员来保证，因为输入流仅由简单的二进制数据组成，并没有额外的信息来表示数据类型，也没有任务信息标志一个data从哪里开始，到哪里结束。


DataStreams示例使用了一个非常糟糕的编程实践：它使用了浮点数来表示货币的值。通常，在需要高精度的情况下，不建议使用浮点数，尤其是表示十进制的小数，因为一些数值（比如0.1），是没有对应的二进制表示的。


对于货币类型，应该选择使用BigDecimal，或者转化成最低的单位--分。不幸的是，BigDecimal是一个引用类型，无法使用DataStream进行读写。但是，可以使用对象流来读写引用类型的对象，下一节将详细介绍。






































