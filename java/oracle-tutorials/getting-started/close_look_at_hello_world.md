## A Closer Look at the "Hello World!" Application

你现在已经看到了Hello World程序，或许你已经编译并运行了此程序，你可能好奇它是如何运行的。让我们再一次回顾Hello World程序：

```
/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}

```
这段代码主要由三部分组成，注释、类声明、main方法。下面的解释可以使你初步理解这段代码，但是更深层次的含义需要等你到完成整个教程之后才会慢慢理解。

##### 源代码注释
注释会被编译器忽略，但是它会对其他程序员有所帮助。java编程语言支持3种注释方式。

* 单行注释： // this is comment
* 多行注释 

``` 
	/*  
		this is multi line comment
	
		just for example
	
	 */

```

* document注释：以/**开头，以\*\结尾，编译器同样会忽略此注释，但是javadoc 工具会根据此注释自动生成文档。

```
/**
 * this is test 
 * @author chengyu
 * @version 17/1/20
 */

```

##### 类定义

```
class classname {
    . . .
}

```

 类定义以关键字class开始，接类名，然后是大括号，大括号内是类的代码。
 
 
 ##### main方法
 
 在java语言中，每个应用都必须包含一个main方法，签名如下：
 
```
public static void main(String[] args)
```
args数组是系统运行时传给应用的参数，例如 java HelloWorld arg1 arg2 , 每一个参数称为命令行参数，命令行参数可以使用在不需要重新编译程序的情况下控制应用的行为，例如一看排序程序可能允许用户制定排序时以降序的形式，通过以下命令行参数可以实现：-descending。最后，

```
System.out.println("Hello World!");
```

这行代码使用了代码库中System类的out对象的println方法，向控制台打印一条信息。