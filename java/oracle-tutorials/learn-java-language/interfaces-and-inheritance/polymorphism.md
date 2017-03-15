### Polymorphism

polymorphism在字典中的定义指的是一个生物学中的准则，一个器官或者物种可以有多种不同形式的状态。这个准则也可以应用到面向对象编程和类似于java的编程语言中。一个类的子类们可以定义它们自己独特的行为，也可以共享父类的功能。

多态的概念可以使用Bicycle的改版程序来阐述。例如，；一个printDescription方法可以添加到类中，打印出存在实例中的所有数据：

```
public void printDescription(){
    System.out.println("\nBike is " + "in gear " + this.gear
        + " with a cadence of " + this.cadence +
        " and travelling at a speed of " + this.speed + ". ");
}

```

为了说明java编程语言中多态的特性，利用两个子类，MountainBike和RoadBike。对于MountainBike，添加一个String类型的域：suspension，代表bike是否有前置的减震器Front。或者，一个bike同时有前置和后置的减震器，Dual。

```
Here is the updated class:

public class MountainBike extends Bicycle {
    private String suspension;

    public MountainBike(
               int startCadence,
               int startSpeed,
               int startGear,
               String suspensionType){
        super(startCadence,
              startSpeed,
              startGear);
        this.setSuspension(suspensionType);
    }

    public String getSuspension(){
      return this.suspension;
    }

    public void setSuspension(String suspensionType) {
        this.suspension = suspensionType;
    }

    public void printDescription() {
        super.printDescription();
        System.out.println("The " + "MountainBike has a" +
            getSuspension() + " suspension.");
    }
} 

```

注意被重写的printDescription()方法。除了打印了父类的信息外，关于suspension的额外的数据也被打印了出来。

下一步创建了RoadBike类，因为公路车有及瘦的轮胎，所以加入一个属性表示轮胎的宽度，下面是RoadBike的代码：

```
public class RoadBike extends Bicycle{
    // In millimeters (mm)
    private int tireWidth;

    public RoadBike(int startCadence,
                    int startSpeed,
                    int startGear,
                    int newTireWidth){
        super(startCadence,
              startSpeed,
              startGear);
        this.setTireWidth(newTireWidth);
    }

    public int getTireWidth(){
      return this.tireWidth;
    }

    public void setTireWidth(int newTireWidth){
        this.tireWidth = newTireWidth;
    }

    public void printDescription(){
        super.printDescription();
        System.out.println("The RoadBike" + " has " + getTireWidth() +
            " MM tires.");
    }
}

```

再一次注意被重写的printDescription()方法，这一次，tireWidth信息被打印了出来。

下面是一个测试程序，每一个变量被分配了三种Bike类型之一，随后打印出了每一个变量。

```
public class TestBikes {
  public static void main(String[] args){
    Bicycle bike01, bike02, bike03;

    bike01 = new Bicycle(20, 10, 1);
    bike02 = new MountainBike(20, 10, 5, "Dual");
    bike03 = new RoadBike(40, 20, 8, 23);

    bike01.printDescription();
    bike02.printDescription();
    bike03.printDescription();
  }
}

```

上述程序的输出如下：

```
Bike is in gear 1 with a cadence of 20 and travelling at a speed of 10. 

Bike is in gear 5 with a cadence of 20 and travelling at a speed of 10. 
The MountainBike has a Dual suspension.

Bike is in gear 8 with a cadence of 40 and travelling at a speed of 20. 
The RoadBike has 23 MM tires.

```


JVM会根据变量指向的真实对象调用合适的方法。它不会调用变量被定义时指定的类型。这种行为称为虚方法调用，阐明了java编程语言重要的多态性质。



### 隐藏域

在一个类的内部，一个域可以与父类的域具有相同的名字，这时这个域就会隐藏父类的同名域，即使它们的类型不同。在子类内部，父类的同名域不能通过名字被直接访问。父类的域必须通过super关键字才能访问，下一节将介绍super关键字的用法，通常来说，我们不推荐隐藏父类的域，因为这样的代码加大了读懂的难度。


### 使用super关键字

#### 访问父类的成员





















