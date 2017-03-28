## The Catch or Specify Requirement

合法的java编程语言必须遵循 Catch or Specify Requirement。这表示：对于可能会抛出某种异常的一段代码，必须使用如下的方式之一来包围这段代码：

* 使用try-catch语句来捕获异常。
* 一个方法在声明时说明它可能抛出异常。这个方法必须包含一个throws语句，将可能抛出的异常列表列出。


不遵循'The Catch or Specify Requirement'原则的代码无法通过编译。

并不是所有的异常都遵守' Catch or Specify Requirement'。我们需要了解基本的3种异常类型，这3种类型中，只有一种是需要遵循上述原则的。


### 三种异常类型

第一个异常称为受检异常(checked exception)。这种异常是程序可以预料到并且可以采取错误恢复正常执行流的一种异常。例如，假设一个应用提示用户在输入框中输入一个文件名，然后通过将文件名传给FileReader类的构造函数来打开这个文件。通常情况下，用户会提供一个存在的可读的文件名，因此程序可以正常执行。但是，有时用户提供了一个不存在的文件名，这时候FileRead的构造函数就会抛出java.io.FileNotFoundException异常。一个健壮的程序将会捕获这个异常，提醒用户输入一个正确的文件名。


受检异常遵守'Catch or Specify Requirement'。所有的异常都是受检异常，除了RuntimeException以及它的子类。


第2种异常类型是error。这是在应用外部发生的异常，应用通常无法预料到这种异常，也无法从这种异常进行恢复。例如，假设应用成功的打开了一个文件，但是因为硬件或者系统故障无法读取这个文件。这个无法读取的将抛出一个java.io.IOError。一个应用可以捕获这个异常，提醒用户系统故障，但是这时候直接退出程序，并且打印一个stack trace也是合理的。


Errors不用遵循'Catch or Specify Requirement'。Errors由Error类以及它的子类来表示。


第3种类型的异常称为运行时异常。这是在程序内部发生的错误，但是不在应用的预料之中，并且应用无法从这种异常中恢复。这种类型的异常往往暗示程序的bug，例如逻辑错误或者不正确的使用的API。考虑前面关于文件的示例，如果由于逻辑错误将null对象传给了FileReader的构造函数，那么构造函数将会抛出NullPointerException异常。应用当然可以捕获这个异常，但是修复这个bug更有意义。


运行时异常不必遵循'Catch or Specify Requirement'。运行时异常由RuntimeException以及它的子类来表示。


Errors和runtime exceptions统称为unchecked exceptions。


### 避开catch和specify

一些开发者认为'Catch or Specify Requirement'是异常机制中的一个严重缺陷，然后通过使用unchecked exceptions替代checked exceptions来避开' Catch or Specify Requirement'。通常，我们不推荐这样做，在下面的小节' Unchecked Exceptions — The Controversy '中将会讨论如何合理的使用unchecked exceptions。




























