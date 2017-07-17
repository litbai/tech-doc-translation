### Deleting a File or Directory

你可以删除文件、目录、符号链接。当删除符号链接时，它只删除链接，并不会删除target。对于目录来说，删除目录时，目录必须为空，不为空则失败。

Files类提供了两个delete方法。

delete(path)方法在删除失败时会抛出异常。例如，如果文件不存在，则抛出NoSuchFileException。你可以捕获这个异常，做一些处理。

```
try {
	Files.delete(path);
} catch (NoSuchFileException x) {
	System.err.format("%s: no such file or directory%n", path);
} catch (DirectoryNotEmptyException x) {
	System.err.format("%s not empty%n", path);
} catch (IOException) {
	System.err.format(x); // file permission problem
}


deleteIfExists(path)方法同样会删除文件，但是如果文件不存在，它不会抛出异常。当你有多个线程删除同一个文件时，你不想在一个线程完成后，其他线程抛出异常的情况下，静默失败是很有用的。


























```