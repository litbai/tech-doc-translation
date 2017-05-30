### PATH and CLASSPATH


本节介绍如何在Microsoft，Solaris和Linux上使用PATH和CLASSPATH环境变量。你可以查看JDK中的安装说明获取最新的信息。


安装完JDK之后，JDK目录通常具有如下的结构：

![](environment-directories.gif)


bin目录包括编译器和启动器(launcher, Launcher是JRE中用于启动程序入口main()的类).


#### Update the PATH Environment Variable（Microsoft Windows）

如果不设置PATH环境变量，你也可以运行java应用，或者你可以通过设置PATH环境变量来更方便的运行java应用。


如果你想更方便的执行操作(javac.exe, java.exe, jps.exe等)，那么请设置PATH环境变量，这样的话你就无需每次都要使用完整的命令路径了。如果你不设置PATH环境变量，那么你需要每次在执行java命令的时候，指定它的全路径，类似于：

```
C:\Java\jdk1.7.0\bin\javac MyClass.java

```

PATH环境变量的值是一系列由;分隔的目录，MicroSoft Windows会按从左到右的顺序查找PATH指定的目录中的命令。你应该只在PATH环境变量中设置一个bin目录，因为如果你设置多个，后面的也会被忽略，所以当你需要更换JDK时，同时更改PATH路径中的bin目录。


下面是一个PATH环境变量的设置示例：


```
C:\Java\jdk1.7.0\bin;C:\Windows\System32\;C:\Windows\;C:\Windows\System32\Wbem;D:\mysql\5.7\bin

```

设置PATH环境变量，使其长久有效，是很有用的，这样每次系统启动时它已经有效，无需每次都设置。如果想环境变量长期有效，在计算机-属性-高级-环境变量中进行设置。


当你编辑环境变量PATH的时候，你可能发现它的值类似下面这样：

```
%JAVA_HOME%\bin;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem

```

注意，用%括起来的变量是已有的其他环境变量。如果这些环境变量被列出来了，例如JAVA_HOME，那么你可以编辑它的值。如果你找不到一个环境变量，例如SystemRoot，那么表示它是操作系统定义的特殊环境变量。SystemRoot表示Microsoft windows的系统文件夹路径。为了获取一个环境变量的值，在命令行中输入如下命令：   echo %SystemRoot%


#### Update the PATH Variable(Solaris and Linux)


如果不设置PATH环境变量，你也可以运行java应用，或者你可以通过设置PATH环境变量来更方便的运行java应用。


如果你想更方便的执行操作(javac, java, jps等)，那么请设置PATH环境变量，这样的话你就无需每次都要使用完整的命令路径了。如果你不设置PATH环境变量，那么你需要每次在执行java命令的时候，指定它的全路径，类似于：


```
% /usr/local/jdk1.7.0/bin/javac MyClass.java

```

查看path变量是不是已经设置过了，可以执行如下命令：

```
% java -version

```

如果输出了正确的信息，那说明已经设置过path变量了，如果提示'java: Command not found', 那么你需要设置path变量。


为了永久的设置path变量，你需要在startup文件中进行设置：

for C shell， 编辑startup文件，(~/.cshrc)

```
set path=(/usr/local/jdk1.7.0/bin $path)

```

for bash, 编辑startup文件(~/.bashrc):

```
PATH=/usr/local/jdk1.7.0/bin:$PATH
export PATH

```


#### Checking the CLASSPATH varibale(All Platforms)


CLASSPATH 变量是告诉应用去哪里找class文件。

定义classpath的的一种比较好的方式是通过在命令行中加入 -cp 参数。这样可以为每个应用设置CLASSPATH而不会影响其他应用。设置CLASSPATH是很微妙的，我们应该小心行事。


classpath的默认值是"."，表示当前目录。如果使用-cp指定了CLASSPATH，那么会覆盖默认的"."。


windows使用如下命令查看是否已设置CLASSPATH：  echo %CLASSPATH%

Linus使用如下命令查看是否已设置CLASSPATH： echo $CLASSPATH


如果想修改CLASSPATH变量，参考前面设置PATH变量的方法。


class path通配符允许你直接指定包含所有jar文件的目录，而不用指定具体的每个jar文件。如果想获取更多的关于class path通配符以及清除CLASSPATH环境变量的信息，参考： https://docs.oracle.com/javase/8/docs/technotes/tools/windows/classpath.html












































