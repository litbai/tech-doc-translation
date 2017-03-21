### 使用包成员

组成一个包的types称为包成员。

如果想在包外使用一个public类型的包成员，你必须使用如下的方式之一：

* 使用类的全限定名来引用包成员
* import 包成员
* import 整个包

每种方式都适用于特定的场景，下面将一一介绍。

#### 使用全限定名来引用包成员

到目前为止，教程中的所有示例使用的都是类的简单名称，例如Rectangle和StackOfInts。在同一个包中你可以使用类的简单名称来引用包成员，因为默认已引入包中的所有成员。


但是，如果你想在一个包中引用另一个包的成员，并且没有使用import语句导入，你必须使用成员的全限定名--包含包名+类名。下面是graphics包中Rectangle类的全限定名：graphics.Rectangle。


你可以使用此权限定名创建graphics.Rectangle的一个实例：

```
graphics.Rectangle myRect = new graphics.Rectangle();

```

在使用频次不高的情况下，使用全限定名是适用的。当你一个类使用很频繁时，重复的使用很冗长的全限定名就很枯燥，而且使代码变得难以阅读。

作为一种替代方案，你可以import这个包的成员或者整个包，然后使用类的简单名字。


#### Importing a Package Member


为了在当前文件中导入一个类，你需要将import语句放在文件的开头，但是如果文件有package语句的话，import语句必须放在package语句之后。例如：

```
import graphics.Rectangle;

```

现在，你可以使用Rectangle的简单名字来引用Rectangle类：

```
Rectangle myRectangle = new Rectangle();

```

如果你只导入一个包的几个类，那么这种方式是十分适合的，但是，如果你使用了一个包中的很多类，你可以导入整个包。


#### importing an entire package


为了导入包中的所有类，你可以使用import语句，加上一个特殊的符号'*':

```
import graphics.*;

```

现在你可以使用简单名称来引用包中的任意类或者接口：

```
Circle myCircle = new Circle();
Rectangle myRectangle = new Rectangle();

```

import语句中的通配符'*'，仅可以代表包中的所有类型。它不能匹配包中的部分类型。例如，下面的语句不能匹配包中所有以A开头的类型：

```
import graphics.A*; // 错误，无法通过编译

```

** 另一个需要注意的点就是，你可以使用import语句导入嵌套类，虽然不常用。例如，如果graphics.Rectangle类包含一个有用的嵌套类，比如 Rectangle.DoubleWide或者Rectangle.Square，你可以导入Rectangle类和它的嵌套类，通过如下两个import语句：

```
import graphics.Rectangle;
import graphics.Rectangle.*;

```

注意，第二个import语句不会导入Rectangle类。

另一个不常见的import形式就是静态导入(import static)语句，下面会讲到。


** 注意：为了方便开发者编写程序，java编译器每一个源文件自动导入了两个包：1. java.lang包 2.当前包，即当前源文件所在的包


#### package 的层次结构

乍一看，包貌似是具有层次结构的，但是并非如此。例如，java API中有包：java.awt、java.awt.color、java.awt.font和其他以java.awt开头的包。但是，java.awt.color、java.awt.font和其他的java.awt.xxxx包，并不包含在java.awt中。包前缀java.awt只是用来显式的表示这些包之间有关系，但不是包含的关系。

语句 import java.awt.*;导入了java.awt包中的所有类，但是它没有导入java.awt.color、java.awt.font或者其他java.awt.xxxx包中的类。如果你想同时使用java.awt包和java.awt.color包中的类，你需要同时导入这两个包：

```
import java.awt.*;
import java.awt.color.*;

```

#### 名字歧义

如果一个包与另一个包中有同名的成员，并且当前文件同时导入了这两个包，此时，你必须使用全限定名来引用这两个成员。例如，graphics中定义了类Rectangle，java.awt包中也定义了一个Rectangle类，如果同时导入这两个类，那么下面的语句就变得有歧义：

```
Rectangle rect;

```

在这种情形下，你必须使用成员的全限定名来表示你想要使用的是哪一个Rectangle类：

```
graphics.Rectangle rect;

```


#### 静态导入语句

在某些特殊情况下，可能你需要频繁的使用一个类的静态方法或者常量。如果每次访问都在前面加上类名的前缀，会让代码显得很杂乱。静态导入技术可以使你导入想使用的常量和静态方法，当你访问这些常量或者静态方法的时候无需每次都带上类名的前缀。


java.lang.Math类定义了常量PI和许多静态方法，包括计算正弦、余弦、正切、平方、最大值、最小值、指数函数等许多有用的方法，例如：

```
public static final double PI 
    = 3.141592653589793;
public static double cos(double a)
{
    ...
}

```

如果想在当前类使用这些方法或者常量，你需要带上Math类型，如下所示：

```
double r = Math.cos(Math.PI * theta);

```

此外，你也可以使用静态导入语句，导入java.lang.Math类的静态成员，这样，你就无需每次访问静态成员都加上类型前缀了，Math类的静态成员可以逐个导入或者全部导入：


```
import static java.lang.Math.PI; // 导入一个

import static java.lang.Math.*; //导入所有

```

一旦你导入了静态成员，那么你就可以用简单的名字来访问这些成员，例如，前面的代码可以简化成如下形式：

```
double r = cos(PI * theta);

```

当然，你也可以编写自己的类，这个类可以包含常用的静态方法和常量，然后你可以使用静态导入技术导入这些静态成员，例如：

```
import static mypackage.MyConstants.*;

```

** 注意，要非常谨慎的使用静态导入。过度的使用静态导入会使代码难以阅读和维护。因为，阅读代码的人不知道哪一个类定义了哪一个静态成员。另一方面，通过适当的使用静态导入，可以是代码更简单易读。**















































