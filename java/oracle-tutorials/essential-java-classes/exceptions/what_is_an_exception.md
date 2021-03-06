## What Is an Exception?

术语exception是短语'exceptional event'的简写形式。

** 定义： 一个异常是在程序执行过程中发生的一个事件，这个事件打破了正常的程序执行流程。**


当执行一个方法过程中发生错误时，这个方法会创建一个对象，并且将这个对象交给运行时系统。这个对象，称为异常对象，包含了错误信息：比如当错误发生时程序的状态以及错误的类型等。创建一个异常对象并且将它交给运行时系统的过程称为throwing an exception.


当一个方法抛出一个异常之后，运行时系统试图找到能处理这个异常的something。这个可以处理异常的'something'可能的集合就是到抛出异常的方法为止，已经调用的方法的列表。这个方法列表称为调用栈(call stack)：


![](exceptions-callstack.gif)


运行时系统会搜索call stack，试图找到一个包含处理异常代码块的方法。这个代码块被称为异常处理器。这个搜索从发生错误的方法开始，以方法调用的相反的顺序来搜索call stack中所有的方法。当找到一个合适的异常处理器之后，运行时系统将这个异常交给这个处理器。如果被抛出的异常类型与一个异常处理器可以处理的类型是匹配的，那么这个异常处理器被认为是合适的处理器。


被选中的异常处理器称为catch the exception。如果运行时系统搜索了call stack中所有的方法，但是没有找到合适的异常处理器，如下图所示，那么，运行时系统终止，或称为程序退出。


![](exceptions-errorOccurs.gif)


使用异常来管理错误，相比于传统的错误管理技术有很多优势，下面的章节会详细介绍。



























