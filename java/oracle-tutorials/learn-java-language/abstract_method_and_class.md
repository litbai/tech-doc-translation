### 编写抽象方法和抽象类

一个抽象类是声明时使用abstract关键字的类，它可以包含抽象方法也可以不包含抽象方法。抽象类不能被实例化，但是它们可以被子类继承。

一个抽象方法，是在方法声明中使用abstract关键字的方法，这种方法没有实现体：

```
abstract void moveTo(double deltaX, double deltaY);

```

如果一个类包含抽象反法，那么这个类必须定义为抽象类：

```

public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}

```

当一个抽象类被子类继承之后，子类通常会提供父类所有抽象方法的实现体。但是，如果子类不提供所有抽象方法的实现体，那么这个子类也必须声明为抽象类。

** 注意，接口中的非default和static方法，默认是抽象方法，因此，abstract关键字可以省略 **


#### 抽象类 vs  接口

抽象类跟接口有很多相同的地方。它们都不可以被实例化，它们都可能包含一些方法，这些方法可以有实现体也可以没有。但是，使用抽象类，你可以定义非static和final的域，定义public、protected和private的实体方法。如果使用接口，所有的域默认都是public static final的常量，所有的方法都必须是public的。除此之外，你只可以继承一个类，不论这个类是普通类还是抽象类，但是你可以实现多个接口。

什么时候使用抽象类，什么时候使用接口？

在以下的情况下考虑使用抽象类：

* 想要在一些关联紧密的类间共享代码
* 你需要除了public之外的访问修饰符
* 你想要声明非static和final的方法。这可以让你定义只能被它所属的对象访问和修改的方法

在以下的情况下考虑使用接口：

* 相关的类可能会实现你的接口。例如，comparable和Cloneable接口，就被很多不相关的类实现了。
* 你想要定义一个特殊的数据类型的行为，但是不关系谁来实现这个行为，比如Map接口
* 你想要利用类型多继承的优势


一个抽象类的样例是JDK中的AbstractMap，是java集合框架的一部分。它的子类，包括HashMap、TreeMap、ConcurrentHashMap，共享了很多在AbstractMap定义的方法（get、put、isEmpty、containsKey、containesValue）

JDK中实现了多个接口的例子是HashMap类，实现了Serializable、Cloneable和Map<K,V>接口。通过它实现的接口，你可以得到一个HashMap(无论是哪个开发者或者组织来实现的)的实例类型都可以被clone、serializable（表示这个对象可以被转化成字节流），并且拥有Map的功能。除此之外，Map接口已经被很多default方法进行了增强，这些方法包括merge、forEach等，已经实现了Map接口的类不受影响。

** 注意，许多软件库同时使用了抽象类和接口，HashMap实现了几个接口，同时继承了AbstractMap类。

### 抽象类举例

假设在一个面向对象的画图应用中，你可以画圆形、矩形、直线、Bezier曲线和许多其他的图形。这些对象有一些通用的状态（position、orientation、line color、fill color等）和一些通用的行为（moveTo, rotate，resize，draw等）。一些状态和行为对于所有的图形来说是相同的（例如position、fill color、和moveTo）。其他的行为需要不同的实现（例如resize，draw）。所有的GraphicObject都必须能够draw和resize，只是实现方式不同。这是使用抽象父类的典型场景。你可以利用这些图形的相同点，让这些图形都继承一个抽象父类，下图是可能的一个继承结构：

![](classes-graphicObject.gif)


首先，你声明一个抽象类GraphicObject，声明所有图形共享的成员变量和方法，例如当前的位置position、移动moveTo方法。GraphicObject同样声明了抽象方法，draw和resize，需要被所有的子类来提供不同的实现。GraphicObject的定义可能如下：

```
abstract class GraphicObject {
    int x, y;
    ...
    void moveTo(int newX, int newY) {
        ...
    }
    abstract void draw();
    abstract void resize();
}

```

实现子类如果没有声明为abstract的，则必须提供父类抽象方法的实现体：

```
class Circle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
class Rectangle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}

```

### 抽象类实现接口


在接口一章中，我们提到，一个类实现接口，则必须实现接口中的所有方法。如果你想定义一个类，实现一个接口，但是不实现所有的抽象方法，那么这个类必须声明为abstract的：

```
class Circle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}
class Rectangle extends GraphicObject {
    void draw() {
        ...
    }
    void resize() {
        ...
    }
}

```

这种情况下，class X必须声明为抽象的，因为它没有实现接口Y中的所有方法，但是XX实现了所有的方法，则可以声明为普通类

### 类成员

一个抽象类可能有static域和static方法。你可以使用这些static成员，就像使用普通类的静态成员一样。





























