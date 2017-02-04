## The Java Technology Phenonenom

每个人都在谈论java技术，但什么是java？接下来的课程解释了java如何同时作作为一门编程语言和一个平台，并对java技术可以帮助你做什么提供了一个概述。

### 1. java技术

Java既是一门编程语言又是一个平台。

##### 1.1 java编程语言

Java是一种高级编程语言，它有如下特征：

	* 简单(Simple)
	* 平台中立性(Architecture neutral)
	* 面向对象(Object oriented)
	* 可移植性(Portable)
	* 分发(Distributed)
	* 高性能(High performance)
	* 多线程(Multithreaded)
	* 鲁棒性(Robust)
	* 动态语言(Dynamic)
	* 安全(Secure)
	
	以上的每一个特性都在 《The Java Language Environment》进行了解释，这是 James Gosling and Henry McGilton 写的一本白皮书。
	
在Java语言中，所有的源代码都以普通文本文件的形式编写，并且以.java作为扩展名。这些源代码文件随后被java编译器编译为.class文件。一个class文件的内容并不能直接被处理器运行，class文件包含的内容称为字节码(bytecode)。字节码是JVM的机器语言。java运行工具接下来会在一个JVM实例中运行你的应用。下面是java程序运行过程图解：
![java程序运行过程](getStarted-compiler.gif)

许多不同的操作系统都可以安装JVM，同样的.class文件可以在Windows、Solaris、Linux或者Mac OS上运行。有一些虚拟机，例如HotSpot，在运行时会做一些额外的操作来提升你的应用的性能。这些操作可能包括找出性能瓶颈、重新编译程序的高频使用代码片段。通过JVM，同样的应用可以运行在不同的平台，如下图所示：

![](jvm.gif)


##### 1.2 java平台

一个平台指程序运行的硬件或软件环境。我们前面已经提到许多流行的平台，像Windows，linux，Solaris OS 和 Mac OS。大部分的平台可以描述为操作系统和底层硬件的组合。java平台不同于其他平台之处在于它仅是一个只包含软件的平台，运行在其他基于硬件的平台之上。

java平台包括如下两个组件：

* The Java Virtual Machine
* The Java Application Programming Interface (API)

我们前面已经介绍了JVM，它是java平台的基础，并且被安装到各种基于硬件的平台上。

API是一个可以提供许多实用功能的的软件组件的集合。它将相关的类和接口以库的形式进行分组，这些库被称为包(packages)。Java API和JVM将程序与底层的硬件进行了隔离，如下图所示：

![](java-platform.gif)

作为一个独立的平台环境，java平台可以比native code稍慢，但是编译器和虚拟机技术的进步使java平台的性能越来越接近native code，同时保持可移植性。

### 2. Java技术可以做什么

任何实现完整的java平台均可以提供如下特性：

* 	开发工具：开发工具提供了你进行编译、运行、监控、调试、文档化应用需要的一切。作为一个新的开发者，你将使用的主要工具是javac编译器、the java launcher, and the javadoc 文档化工具。
* 	API。API提供了java语言的核心功能。它提供了一系列开箱即用的功能类，它包括基本的对象、网络和安全、XML处理和数据库访问以及很多其他的功能。API的核心非常庞大，如果想预览它的内容，可以参考Java Platform Standard Edition 8 Documentation。
* 	开发技术。JDK提供了包括Java Web和Java Plug-In等的标准实现机制，它可以帮助你部署你的应用程序给用户使用。
* 	用户界面工具。JavaFX, Swing, and Java 2D toolkits
* 	集成库。集成库包括Java IDL、JDBC、JNDI、RMI等，可以是我们访问数据库和远程对象。

### 3. Java如何改变我的生活

我们不能保证学习java可以给你带来财富、声望甚至一个工作。但是相对于其他编程语言，他可能使你的程序结构更好，并且需要付出更少的努力。我们相信java技术可以帮助你做到如下的事情：

* 快速入门。尽管java编程语言是一个功能强大的面向对象的语言，但是它很容易学习，特别是对于已经熟悉C或者C++的程序员来说。
* 编写更少的代码。经过对一些程序指标（class的数量、方法的数量等），表明使用java编写的程序比使用C++编写的程序体积少4倍。
* 编写更好的代码。java编程语言鼓励更好的编程实践，并且有自动垃圾回收机制来避免内存泄露。它的面向对象特性、javaBean组件架构以及使用广泛容易扩展的API，可以使你重用已有的代码，并且减少程序的bug。
* 更快速的开放应用程序。java编程语言比C++简单，正因为如此，你的开发效率可以提升2倍，代码行数也变得更少。
* 避免平台依赖性。相比其他语言，使用java可以很容易的实现跨平台性。
* 更容易的分发软件。通过使用Java Web Start软件，用户可以仅通过点击鼠标即可启动应用。启动应用时会自动进行版本检查，这可以保证你的程序始终位于最新版本。如果有更新可用，Java Web Start 会自动安装更新。













	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	