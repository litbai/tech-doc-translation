### 创建一个package

为了创建一个package，你需要为你的package想一个名字，在包含类定义的源文件的首行添加一个package语句，这样你就将你的类放入了一个package中。

package语句必须位于源文件的首行。每一个源文件只能有一个package语句，这条规则适用于所有的types。

** 注意，如果你在一个源文件中声明了多个类，那么只能有一个类是pubic的，而且这个类名必须与源文件的名字一致。例如，你可以在源文件Circle.java中定义public class Circle ，在 Draggable.java源文件中定义public interface Draggable，在源文件Day.java中定义public enum Day。
你可以在同一个源文件中包含非public的类(虽然不推荐这么做，除非这个非public的类非常小，而且与public的类联系非常紧密)，但是只有被public修饰的类可以在包外访问。所有的顶级且非public的类都是package-private的。**


如果你将上一节定义的接口和类放在一个名字为graphics的包中，你将需要6个源文件，如下所示：

```
//in the Draggable.java file
package graphics;
public interface Draggable {
    . . .
}

//in the Graphic.java file
package graphics;
public abstract class Graphic {
    . . .
}

//in the Circle.java file
package graphics;
public class Circle extends Graphic
    implements Draggable {
    . . .
}

//in the Rectangle.java file
package graphics;
public class Rectangle extends Graphic
    implements Draggable {
    . . .
}

//in the Point.java file
package graphics;
public class Point extends Graphic
    implements Draggable {
    . . .
}

//in the Line.java file
package graphics;
public class Line extends Graphic
    implements Draggable {
    . . .
}

```

如果你不使用package语句，你的类将会在一个没有名字的包中。通常来说，一个无名包只在存放小体积且临时的应用，或者你刚开始编程时的写的测试代码。否则，classes和interfaces都应该在一个有名字的包中。

































