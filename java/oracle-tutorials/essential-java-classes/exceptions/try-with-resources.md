## The try-with-resource Statement

try-with-resource语句是一个声明了一个或多个资源的try语句。一个资源是一个对象，程序使用完这个资源之后，必须关闭它。try-with-resource语句保证了在语句的结尾会关闭每一个资源。任意实现了java.lang.AutoClosebale接口的对象，包括实现了java.io.Closeable接口的对象，都可以被用作一个资源。


下面的示例读取一个文件的第一行。它使用了一个BufferedReader的实例来从文件中读取数据。BufferedReader是一个资源，它必须在程序使用完之后关闭。


```

static String readFirstLineFromFile(String path) throws IOException {

	try (BufferedReader br = new BufferedReader(new FileReader(pth))) {
		return br.readLine();
	}
}

```

在上述示例中，try-with-resource语句中声明的资源就是一个BufferedReader实例。声明语句位于try关键字后面的小括号中。BufferedReader类，java se 7及以后，实现了接口java.lang.AutoCloseable.因为BufferedReader的实例是在try-with-resource语句中声明的，所以它将会被关闭，无论try语句块是正常执行还是出现错误(br.readLine()抛出IOException)。


在java se7之前，你可以使用finally块来保证一个资源被关闭，无论try语句正常执行还是抛出异常。下面的例子使用了finally语句块来保证资源被及时关闭：

```
static String readFirstLineFromFileWithFinallyBlock(String path)
                                                     throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        if (br != null) br.close();
    }
}

```


但是，在上述示例中，如果readLine()和close()方法同时抛出异常，那么readFirstLineFromFinallyBlock方法将会抛出finally块中的异常，try语句中抛出的异常被抑制了。相反，如果使用try-with-resource语句块，如果readLine()和close()方法同时抛出异常，那么readFirstLineFromFile方法将会抛出try块中的异常，try-with-resource语句块中抛出的异常被抑制了。在java se 7及以后，你可以追踪被抑制的异常。


你可以在try-with-resource语句中定义多个资源，下面的示例搜索了zip文件中的所有文件名，并且创建了一个文本文件，将这些文件名保存在这个文本文件中：


```
public static void writeToFileZipFileContents(String zipFileName, String outputFileName) throws java.io.IOException {
	java.nio.charset.Charset charset = java.nio.charset.StandardCharsets.US_ASCII;
	java.io.file.Path outputFilePath = java.nio.file.Paths.get(outputFileName);
	
	try (java.util.zip.ZipFile zf = new java.util.zip.ZipFile(zipFIleName);
	java.io.BufferedWriter writer = java.nio.file.Files.newBufferedWriter(outputFilePath, charset)
	) {
		for (java.util.Enumeration entries = zf.entries(); entries.hasMoreElements();) {
			String newLine = System.getProperty("line.separator");
			String zipEntryName = (java.util.zip.ZipEntry)()entries.nextElements()).getName + newLine;
			
			writer.write(zipEntryName, 0, zipEntryname.length);
		}
		
	}
	
}

```

在上述示例中，try-with-resource语句包含两个资源：ZipFile和BufferedWriter。当try块中的代码执行完后，BufferedWriter和ZipFile对象的close方法将会自动的依次被执行。注意，资源的close方法执行顺序与资源社声明的顺序相反。


下面的示例使用try-with-resource语句自动的关闭java.sql.Statement对象。

```
public static void viewTable(Connection con) throws SQLException {
	String query = "select * from Coffees";
	try (Statment stmt = con.createStatement()) {
	
		ResultSet rs = stmt.executeQuery(query);
		
		while (rs.next) {
			String coffeeName = rs.getString("COF_NAME");
			int supplierID = stmt.getInt("SUP_ID");
			float price = rs.getFloat("PRICE");
			int sales = rs.getInt("SALES");
			int total = rs.getInt("TOTAL");
			System.out.println(coffeeName + ", " + supplierID + ", " + price + ", " + sales + ", " + total);
		}
	
	} catch (SQLException) {
		JDBCTutorialUtilities.printSQLException(e);
	}
}

```

java.sql.Statement资源是JDBC4.1及以后的组成部分。


** 一个try-with-resource语句可以有catch和finally块，就像普通的try语句一样。在try-with-resource语句中，所有的catch或者finally语句都将在声明的资源被关闭后运行。**


### 抑制异常(Suppressed Exceptions)

与try-with-resource语句关联的代码块可以抛出异常。在上述示例writeToFileZipFIleContents方法中，try块中可以抛出异常，try-with-resource语句块中最多可以抛出2个异常(当关闭ZipFile和BufferedWriter对象的时候)。如果try块中抛出一个异常，try-with-resource语句中也抛出1个异常，那么try-with-resource语句中被排除的异常将会被抑制， writeToFileZipFileContents方法抛出的异常将会是try块中抛出的那个异常。你可以获取到被抑制的异常，通过在try块中抛出的异常对象上调用getSuppressed()方法，这个方法是在Throwable类中定义的。


### 实现了AutoCloseable或Closeable接口的类


可以查看javadoc来获取实现了AutoCloseable和Closeable接口的类。Closeable接口继承了AutoCloseable接口。Closeable接口中的close方法跑出了IOException类型的异常，而AutoCloseable接口的close方法抛出的是Exception类型的异常。因此，AutoCloseable接口的子类可以覆盖close方法的行为，抛出更具体的异常，例如IOException或者不抛出异常。











































