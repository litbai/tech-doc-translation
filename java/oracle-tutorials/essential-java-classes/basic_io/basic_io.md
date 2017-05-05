## Lession: Basic I/O

这一章的教程覆盖了java平台与I/O相关的类。首先讲解了I/O stream，一个简化了I/O操作的强大的概念。本教程也涉及到serialization, 可以让程序将对象写到stream中，或者从stream中读取对象。最后本教程着重讲解了file I/O和文件系统操作，包括随机访问文件。


在I/Ostream一节中涉及的大部分类都在java.io包中。 File I/O一节涉及的大部分类都位于java.nio.file包中。


I/O Streams:

* Byte Streams： 处理基本的二进制数据的I/O操作
* Character Streams：处理字节数据的I/O操作，会根据本地字符集，自动进行字符和字节之间的翻译
* Buffered Streams：通过减少访问native API的频率，优化了I/O操作
* Scanning and formatting：运行程序读取或输出格式化文本
* I/O from the Command Line：描述Standard Streams 和 Console对象
* Data Streams：处理原始数据类型和String的二进制I/O
* Object Streams：处理对象的二进制I/O


File I/O(Featuring NIO.2)

* What is a Path? 介绍了文件系统中path的概念
* The Path Class: 介绍了java.nio.file包中基础类
* Path Operations: 介绍了Path类的方法
* File Operations: 介绍了很多file I/O方法的通用概念
* Checking a File or Directory: 介绍了如何检查一个文件是否存在以及它的可见性级别
* Deleteing a File or Directory
* Copying a File or Directory
* Moving a File or Directory
* Managing Metadata: 解释了如何读取和改写文件属性
* Reading，Writing，and Creating Files：介绍了读写文件的stream和channel方法
* Random Access Files: 介绍了如何以非连续的方式读取文件
* Creating and Reading Directories：介绍了针对directory的特定API，例如如何列出一个目录中的内容
* Links，Symbolic or OtherWise：介绍了符号链接和硬链接的知识
* Walking the File Tree: 介绍了如何递归的访问文件树中的每个文件和目录
* Finding Files：介绍了如何使用模式来匹配文件
* Watching a Directory for Changes：介绍了如何使用watch service来检测一个目录中添加、删除或者更新文件操作
* Other Useful Methods：介绍了上述教程没有涉及到的方法
* Legacy File I/O Code：介绍了如何使用Path类提供的功能替换原先使用java.io.File类实现的功能。并提供了一个java.io.File类到java.nio.Path类的方法mapping表格。


### Summary

### Questions and Exercises

### The I/O Classes in Action


后面的教程'Custom Networking'的很多示例，使用了本章教程涉及的很多知识点。

































