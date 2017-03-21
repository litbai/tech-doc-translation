### 管理源代码和class文件

java平台的许多实现依赖于层级文件系统来管理源代码和class文件，尽管java language specification并没有强制要求这么做。使用层级文件系统的策略如下：


将一个类、接口、枚举或者注解类型的源代码放到一个文本文件中，文本文件命名为types的简单名字，扩展名为.java。例如：


```
//in the Rectangle.java file 
package graphics;
public class Rectangle {
   ... 
}

```

然后，将源代码文件放到一个目录中，这个目录的名字表明了这个类所在的包名：

```
.....\graphics\Rectangle.java

```


包成员的全限定名和它所在的文件的路径是一致的，假设windows系统下，路径分隔符为'\'：

* class name - graphics.Rectangle
* pathname to file - graphics\Rectangle.java


前面一节曾讲过，按照惯例，一个公司使用它的因特网域名的反转名作为包名。域名为example.com的公司Example，将会使用前缀com.example作为公司所有包名的前缀。包名的每一个部分对应一个子目录。所以，如果Example公司有一个包com.example.graphics，包里有一个类Rectangle.java，那么它应该包含如下的文件路径：

```
....\com\example\graphics\Rectangle.java

```


当你编译一个源文件时，编译器会为源文件中定义的每一个类创建一个输出文件。输出文件的命名为类名+.class扩展名。例如，源文件如下：

```
//in the Rectangle.java file
package com.example.graphics;
public class Rectangle {
      . . . 
}

class Helper{
      . . . 
}

```

则编译后的生成的文件为：

```
<path to the parent directory of the output files>\com\example\graphics\Rectangle.class
<path to the parent directory of the output files>\com\example\graphics\Helper.class

```


类似于.java源文件，class文件也位于一系列的目录中，这个目录映射出这个类所在的包名。但是，.class文件的路径名不一定要与.java文件的路径名相同。你可以将源文件和目标文件放在不同的目录下：

```
<path_one>\sources\com\example\graphics\Rectangle.java

<path_two>\classes\com\example\graphics\Rectangle.class

```

这样的话，你只需给到其他开发者你的classes所在的目录即可，而不会暴露源代码。你也需要这样来管理你的源文件和目标文件，只有这样，编译器和JVM才可以找到你的程序中使用到的所有Type。

classes的全路径名<path_two>\classes，称为class path，通过系统变量CLASSPATH来指定。编译器和JVM都会利用此路径和包名来构建class文件的路径。
假如'<path_two>\classes'在你的class path中，并且你的包名为'com.example.graphics'，那么，编译器和JVM会在如下路径中来寻找.class文件：

```
<path_two>\classes\com\example\graphics

```

一个class path可能包含多个path，由分号（windows）或者冒号（colon）分隔。另外，编译器和JVM默认会在当前路径和包含classes的jar包中寻找class文件，因此，这些路径默认在class path中。


#### 设置 CLASSPATH 变量


如果想查看CLASSPATH变量的值，可以使用如下的命令：


* In Windows: C:\> set CLASSPATH
* In Unix: % echo $CLASSPATH


如果想删除当前的CLASSPAHT变量，使用如下命令：

* In Windows: C:\> set CLASSPATH=
* In Unix: % unset CLASSPATH; export CLASSPATH


如果想设置CLASSPAHT变量，使用如下命令：

* In Windows: C:\> set CLASSPATH=C:\users\george\java\classes
* In Unix: % CLASSPATH=/home/george/java/classes; export CLASSPATH















































