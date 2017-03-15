### 使用super关键字

如果你的方法覆盖了父类的方法，你可以通过super关键字来调用父类的方法。你也可以使用super关键字访问一个隐藏域（我们不推荐隐藏域，因为它使得程序难以理解）。考虑如下的类，SuperClass:

```
public class Superclass {

    public void printMethod() {
        System.out.println("Printed in Superclass.");
    }
}

```

有一个subclass，名字为SubClass，覆盖了printMethod()方法:

```
public class Subclass extends Superclass {

    // overrides printMethod in Superclass
    public void printMethod() {
        super.printMethod();
        System.out.println("Printed in Subclass");
    }
    public static void main(String[] args) {
        Subclass s = new Subclass();
        s.printMethod();    
    }
}

```


在SubClass的内部，方法名printMethod指的是SubClass中重写过的printMethod()方法，覆盖了父类的同名方法。因此，为了调用从父类中继承过来的printMethod方法，SubClass必须使用一个全限定名，使用super关键字。编译并且执行SubClass，将得到如下的输出：

```
Printed in Superclass.
Printed in Subclass

```

#### SubClass 的构造器

下面的代码阐述了如何使用super关键字调用父类的构造器。回忆一下前面介绍过的Bicycle类和MountainBike类，下面是MountainBike的构造器，使用super关键字调用了父类的构造器：

```
public MountainBike(int startHeight, 
                    int startCadence,
                    int startSpeed,
                    int startGear) {
    super(startCadence, startSpeed, startGear);
    seatHeight = startHeight;
}   

```

调用父类构造器，必须在子类构造器的第一行。语法如下所示：

```
super();  // 无参构造器
super(parameter list); // 有参构造器

```

** 注意：如果一个构造器没有显示的调用父类的构造器，java编译器会自动的插入一个对父类无参构造器的调用。如果父类没有一个无参构造器，编译器将发出一条错误。Object类有无参的构造器，所以如果父类直接为Obect，那么不显示的调用父类构造器也不会出现问题。**



如果子类的构造器调用了父类的一个构造方法，无论显式还是隐式，你可能会认为这儿会发生一个构造器调用链，一直到Object类。实际上，确实是这样。这称为构造器链，如果你有一个很长的类继承结构，你需要注意到此点。





































