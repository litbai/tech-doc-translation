### Summary of Interface

除了Object类，一个类仅可以有一个直接父类。一个类继承了它的所有父类的域和方法，无论是直接父类还是间接父类。一个子类可以重写它继承的方法，它也可以隐藏继承的域和方法（注意，隐藏域通常是不好的编程实践）。

Object类在类层次结构的顶端。所有的类都是Object类的子类，并且继承了Object类的方法。一些比较重要的从Object类继承的方法有：toString(), hashCode(), equals, clone 和 getClass.

你可以通过使用final关键字声明一个final类，使这个类无法被其他类继承。同样的，你也可以将一个方法申明为final的，子类则无法重写这个方法。

一个抽象类只可以被子类继承，而不能被实例化。一个抽象类可以包含抽象方法--抽象方法是只声明但没有提供实现体的方法。子类来提供抽象方法的实现。