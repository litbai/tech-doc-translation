### File I/O (Feature NIO.2)

** 注意，本教程是在JDK7的基础上编写 **

java.nio.file 包以及与它相关的java.nio.file.attribute包对文件的I/O操作提供了极大的支持。尽管API包含很多的class，你只需要关注其中的几个即可。你将会发现这些API非常直观和好用。


本章教程首先从一个问题开始： what is a path? 然后，介绍了 Path Class， Path类是整个package的入口。接下去解释了Path类中与句法操作有关的方法。然后，教程转向了另一个重要的入口类，Files类，它包含处理文件操作的很多方法。首先介绍了很多文件操作的通用概念，然后介绍了checking、deleting、copying和moving相关的文件操作。


本教程还介绍了如何管理元数据，接下去介绍File I/O和directory I/O。 还介绍了Random Access File，以及符号链接和硬链接相关的操作。


接下去，讨论了更高层、更强大的知识。首先，介绍了如何递归的访问文件树；然后介绍了如何使用通配符来搜索文件；最后，介绍了如何监控一个目录的change事件。


最后的最后，如果你有在JDK7之前编写的I/O相关的代码，本文提供了一个老接口--新接口的映射。还介绍了File.toPath方法，这个方法可以让开发者使用新API而无需重写已有的代码。





















