### Lesson: Concurrency

计算机用户很自然的认为他们的系统可以同时做很多事情。他们可以同时使用word编辑文档、下载文件、管理打印机和音频流等。即使是单个应用程序也会在同一时间做很多事情。例如，音频流应用肯定在同时读取网络中的数字音频、解压缩、管理回放以及更新显示。即使是word程序在忙于格式化文版或者更新显示时，也总是时刻准备着响应键盘和鼠标事件。像这样的软件被称为并发软件。

java平台通过java编程语言本身和java类库的并发支持，可以方便的进行并发编程。从5.0版本开始，java平台还包括了高级的并发API。这个教程介绍了java平台基础的并发支持，并且总结了java.util.concurrent包中的一些高级的并发API。