### Processes and Threads

在并发编程中，有两种基本的执行单元:进程和线程。在java编程语言中，并发编程主要指多线程编程。但是，进程同样很重要。

一个计算机系统通常有很多活跃的进程和线程。即使在一个只有单个执行单元的计算机系统中，这也是真实存在的，虽然实际上同一时间只会有一个线程执行。单核的处理器时间，通过操作系统的一个称为时间片的特性，在多个进程和线程之间共享。

现代的计算机通常都具备多于一个处理器，或者多核处理器。这极大的增强了系统并发的执行进程和线程的能力。但是，即使在仅拥有一个单核处理器的简单系统中，并发依然是可行的。


#### 进程


一个进程有自己独立的执行环境。一个进程通常有一个完整的、私有的执行资源集。尤其是每个进程有自己独立的内存空间。


进程通常被当做程序或者应用的同义词。但是，在用户的角度来看的单个应用，实际上可能是由多个进程协作来运行的。为了便于进程间的通信，大部分的操作系统支持进程间通信（Inter Process Communication）资源，例如pipe和socket。IPC不仅仅用于同一个系统间的进程通信，还可以应用在不同系统间的进行之间进行通信。

#### 线程

线程有时被称为轻量级进程。进程和线程都有一个执行环境，但是创建一个线程要比创建一个进程需要更少的资源。


线程存在于进程中--每一个进程至少包含一个线程。一个进程中的多个线程共享进程的资源，包括内存和打开的文件资源。这提升了效率，但是带来了潜在的问题--线程间通信。

多线程执行是java平台的一个基本特性。每一个应用至少有一个线程--或者多个线程，如果你算上垃圾回收和信号处理等这些系统级线程的话。但是在应用开发者的角度，你起始于一个线程，即main线程。这个线程可以创建其他的线程，这部分知识下一节将会介绍。






























