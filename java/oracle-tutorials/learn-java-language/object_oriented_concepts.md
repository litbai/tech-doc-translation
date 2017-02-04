## Object-Oriented Programming Concepts

如果你以前从来没有用过面向对象编程语言，你将会在编写代码之前学习一些基本的概念。这一章将会介绍对象、类、继承、接口、包。每一次讨论都会集中在这些概念与现实世界的联系，同时会介绍在java语言中与这些概念有关的语法。

### 什么是“对象”？

对象是理解面向对象技术的关键。看看周围你会发现现实世界中的对象示例：你的狗、书桌、电视、自行车等。

现实世界对象包括2种特征：它们都有状态和行为。狗有自己的状态（名字、颜色、繁殖、饥饿）和行为（吠叫、叼球、摇尾巴）。自行车有自己的状态（齿轮、踏板、速度）和行为（更换齿轮、更换踏板、刹车）。辨别现实世界对象的状态和行为是思考面向对象编程的一个好的开始。

现在花几分钟观察一下你周围的现实世界的对象。对于每一个对象，问自己两个问题：这个对象可能处于什么状态？以及这个对象可能发生什么行为。将你的观察写下来。当你做完之后，你将会注意到现实世界的对象非常复杂。你的桌面电脑可能仅有两种可能状态（开机和关机），和两种可能的行为（开机和关机）。但是你的收音机可能会有额外的状态（开、关、音量、电台）和行为（开、关、音量调高、音量调低、搜台、扫描、调频）。你可能会注意到，一些对象还包括其他对象，这种现实世界的观察将会全部翻译到面向对象编程的世界。下面的图描述了对象的内容：

![](concepts-object.gif)

软件中的对象概念类似于现实世界的对象：它们都由状态和行为组成。一个软件对象将它的状态存储在Fileds（一些编程语言称为变量）中，然后通过方法暴露它们的行为（一些编程语言称为函数）。方法对对象的状态进行操作，是类与类之间交流的主要机制。隐藏对象的内部状态，所有交互均通过对象方法来完成，这称为数据封装--面向对象编程的一个基本概念。

想象一个自行车，如下所示：

![](concepts-bicycleObject.gif)

通过自行车的状态（当前速度、脚踏板、齿轮）并且提供改变对象状态的方法，对象控制了外界可以对其进行的操作。举个例子，如果自行车仅有6个齿轮，一个改变齿轮的方法会拒绝少于1大约6的值。
将代码绑定在一个软件对象中，可以有很多好处，包括：

* 模块化：一个对象的源代码可以被写完，并且可以独立于另外的对象进行维护。一旦创建，一个对象可以轻松的在应用之间传递。
* 信息隐藏：通过仅使用方法来进行对象间交互，可以隐藏其内部的实现。
* 代码重用：如果一个对象已经存在（可能由其他程序员编写完成），你就可以在程序中使用那个对象，这样的话，就可以由专家来完成、测试、调试一些复杂的、面向特定任务的对象，你可以放心的在你的代码中使用。
* 可插拔和易于调试：如果一个对象被确认是有问题的，你可以从你的应用中把它移除，并用另外一个具有相同功能的类替代。这类似于现实世界修复机械问题，如果一个螺丝有问题，你换个好螺丝，而不是整台机器。

### 什么是类？

现实世界中，我们经常发现同样类型的许多独立的对象。现实中有上千不同的自行车，所有这些自行车都是按同样的模型来制造的。每一辆自行车都基于同样的蓝图进行制造，因此这些自行车拥有相同的部件。在面向对象领域，我们说一辆自行车是自行车类的一个实例。一个类是一个蓝图，所有独立的对象均根据此蓝图创建。

下面的Bicycle类就是一个可能的实现：

```
class Bicycle {
	int cadence = 0;
	int speed = 0;
	int gear = 1;
	
	void changeCadence(int newValue) {
		cadence = newValue;
	}
	
	void changeGear(int newVluae) {
		gear = newVluae;
	}
	
	void speedUp(int increment) {
		speed += increment;
	}
	
	void applyBrakes(int decrement) {
		speed -= decrement;
	}
	
	void printStatus() {
		System.out.println("cadence:" + cadence + " speed:" + speed + " gear:" + gear);
	}
}

```

java的语法对你来说是新的，但是这个类的设计是基于前面讨论的自行车对象。cadence、speed、gear域代表了对象的状态，方法（changeCadence、changeGear、speedUp等）定义了外界与对象的交互。

你可能注意到Bicycle类不包含main方法。那是因为这不是一个完整的应用，它只是自行车的蓝图，可能会在应用中使用。创建新自行车对象的责任术语你的应用中的其他类。

接下来是BicycleDemo类，创建了2Bicycle对象，并且调用了它们的方法：

```
class BicycleDemo {
	public static void main(String[] args) {
		// create two different Bicycle objects
		Bicycle bike1 = new Bicycle();
		Bicycle bike2 = new Bicycle();
		
		// invoke methods on those objects
		bike1.changeCadence(50);
       	bike1.speedUp(10);
       	bike1.changeGear(2);
       	bike1.printStates();
	
       	bike2.changeCadence(50);
       	bike2.speedUp(10);
       	bike2.changeGear(2);
       	bike2.changeCadence(40);
       	bike2.speedUp(10);
       	bike2.changeGear(3);
       	bike2.printStates();
	}
}

```
 上述的测试程序的输出两个自行车对象最后的脚踏板节奏、速度和齿轮状态：
 
 cadence:50 speed:10 gear:2
 cadence:40 speed:20 gear:3


### 什么是继承？

不同类型的对象之间通常一些共同点。举个例子，山地车、公路车、双人自行车，都有自行车的通用特性（速度、脚踏板、齿轮）。但是每个自行车对象又有自己的独特特性：双人自行车有两个座位和把手，公路车有赛车把手，一些山地车有额外的链环，可以有较低的传动比。

面向对象编程允许一个类从其他类继承通用的状态和行为。在自行车的例子中，Bicycle现在成为了MountainBike, RoadBike, 和TandemBike的基类。在java编程语言中，每一个类只允许有一个直接基类，而每个基类可以有许多不同的子类，Bicycle类的继承结构如下所示：

![](concepts-bikeHierarchy.gif)

创建一个子类的语法很简单。在类声明的开始，使用extends关键字，extends关键字后面跟上需要继承的类的名字：

```
class MountainBike extends Bicycle {

    // new fields and methods defining 
    // a mountain bike would go here

}
```

继承使得MountainBike具有和Bicycle同样的fields和methods，这样，MountainBike类的代码就可以集中在使其独特的那部分特性上。这可以使子类的代码更易阅读。但是，你必须保证针对父类的状态和行为，有完善的文档介绍，因为父类的代码不会出现在子类中。


### 什么是接口？

正如你已经学到的，对象通过它们暴露的方法定义了与外界的交互方式。方法组成了一个对象与外界交互的接口。举个例子，电视的电源键，是你和电视交互的接口。你按下电源键，来打开电视或者关闭电视。

一个接口的最常见的形式，是一组没有实现代码的相关的方法。一个自行车的行为，如果以接口来定义，可能会是如下的形式：

```
interface Bicycle {

    //  wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);
}
```

为了实现这个接口，你要在类声明中使用implements关键字：

```
class ACMEBicycle implements Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}

```

实现一个接口允许一个类对它提供的行为变得更加正式。接口是类与外界交互的一个契约，并且这个契约会被编译器强制执行。如果你的类声明实现一个接口，那么这个接口里定义的所有方法都必须被你的类以源代码的形式来实现，这样才能通过编译。

	注意，为了真正的来编译ACMEBicycle类，你需要在类定义的开始加上public关键字，后续的教程会解释这样做的原因。



### 什么是包？

一个包是将相关的一系列类和接口组织起来的一个namespace(命名空间)。从概念上来讲，你可以认为包类似于你电脑上的文件夹。你可以在一个文件夹中保存html文件，在另一个文件夹里存放图片，在另一个文件夹里存在脚本等。因为由java编写的应用可能会由成百上千的的类组成，将这些不用的类和接口组织成不同的包是很有实际意义的。

Java平台提供了大量的类库（包集合），你可以在应用中方便的使用。这些类库称为API（Application Programming Interface）。它的包代表了通用程序中常见的任务。举个例子，一个String的对象包含了字符串的状态和行为；一个File对象可以让程序员很容易的创建、删除、检查、比较或者修改文件系统中的文件；一个Socket对象可以让程序员使用网络socket；各种GUI对象控制了诸如按钮、复选框等于图形用户接口相关的组件。有上千的类供你选择，这可以使程序员集中精力设计自己的独特应用上面，而不是使你应用工作的一些基础设施。

<a href='https://docs.oracle.com/javase/8/docs/api/index.html'>Java Platform API Specification</a> 包含了所有包、接口、类、域、方法的完整列表。在你的浏览器中打开链接，并加入收藏夹。作为一个程序员，这个文档将成为你以后最重要的参考文档之一。



















