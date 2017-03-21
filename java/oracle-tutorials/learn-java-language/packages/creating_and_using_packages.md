## 创建并使用package


为了更容易的找到和使用类，避免命名冲突，进行访问控制，程序开发者需要将一组相关的类打包在一起。

** 定义：一个package是一组相关类的集合，提供了访问控制和命名空间管理。注意，这里的类指的是：classes、interfaces、enumerations和annotations。Enumerations是特殊类型的classes，annotation types是特殊类型的interfaces，因此，在本节中，types用来值classes和interfaces。**


java平台提供的types，根据这些类的功能，分布在不同的package中：基础的类在java.lang包中，输入输出类在java.io包中等等。你也可以将你的类放在包中。


假设你正在编写一组代表图形的类，例如circles、rectangles、lines、points。同时编写了一个接口，Draggable，实现此类的接口可以通过鼠标进行拖拽。


```
//in the Draggable.java file
public interface Draggable {
    ...
}

//in the Graphic.java file
public abstract class Graphic {
    ...
}

//in the Circle.java file
public class Circle extends Graphic
    implements Draggable {
    . . .
}

//in the Rectangle.java file
public class Rectangle extends Graphic
    implements Draggable {
    . . .
}

//in the Point.java file
public class Point extends Graphic
    implements Draggable {
    . . .
}

//in the Line.java file
public class Line extends Graphic
    implements Draggable {
    . . .
}


```


你应该将这些类和接口放在一个package中，这么做有几个原因：

* 你和其他开发人员能很容易的意识到这些类是相关的。
* 你和其他开发人员知道在哪里可以找到与图形相关的功能。
* 你的类不会与其他包中的同名类冲突，因为它们在不同的命名空间。
* 你可以访问包内其他的类，而使这些类对在包外是不可访问的。

























