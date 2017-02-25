## 枚举类型

枚举是一个特殊的数据类型，它可以使变量固定为预先定义的常量。变量必须等于预先定义的常量之一。常见的枚举的例子包括罗盘方向(NORTH SOUTH EAST WEST), 一周的天数(SUNDAY, MONDAY, TUESDAT..).

因为枚举是常量，所以枚举的名字都是大写字母。在java编程语言中，你需要使用enum关键字来定义一个枚举类型。例如，你定义一个星期的枚举类型：

```
public enum Day {
	SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

```

你应该在需要表示一个固定的常量集合的时候，使用枚举类型。包括天然的枚举类型，例如太阳系的星球或者你在编译期就知道所有可能值的数据集--菜单、命令行标识符等。

```
public class EnumTest {
    Day day;
    
    public EnumTest(Day day) {
        this.day = day;
    }
    
    public void tellItLikeItIs() {
        switch (day) {
            case MONDAY:
                System.out.println("Mondays are bad.");
                break;
                    
            case FRIDAY:
                System.out.println("Fridays are better.");
                break;
                         
            case SATURDAY: case SUNDAY:
                System.out.println("Weekends are best.");
                break;
                        
            default:
                System.out.println("Midweek days are so-so.");
                break;
        }
    }
    
    public static void main(String[] args) {
        EnumTest firstDay = new EnumTest(Day.MONDAY);
        firstDay.tellItLikeItIs();
        EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
        thirdDay.tellItLikeItIs();
        EnumTest fifthDay = new EnumTest(Day.FRIDAY);
        fifthDay.tellItLikeItIs();
        EnumTest sixthDay = new EnumTest(Day.SATURDAY);
        sixthDay.tellItLikeItIs();
        EnumTest seventhDay = new EnumTest(Day.SUNDAY);
        seventhDay.tellItLikeItIs();
    }
}

```

java编程语言的枚举类型比其他语言更强大。enum关键字定义了一个类（称为枚举类型）。枚举类的主体可以包括方法和其他的域。编译器会在创建一个枚举的时候，自动的加一些特殊的方法。例如，枚举类有一个values方法，会按声明的顺序返回一个包含所有枚举值的数组。这个方法通常和for-each结构一起使用来迭代枚举类型的所有可能的值，实例代码如下：

```
for (Planet p : Planet.values()) {
    System.out.printf("Your weight on %s is %f%n", p, p.surfaceWeight(mass));
}

```

	* 注意，所有的枚举类型隐式的集成了java.lang.Enum类。因为java只支持单继承，因此枚举类型不能再继承其他类 *
	
在下面的例子中，Planet是一个枚举类型，代表太阳系的星球。他们有属性mass和radius。每一个枚举常量声明的时候都需要mass和radius参数，这些参数在创建枚举常量时传到构造器中。java编程语言要求必须在定义任何域和方法之前，首先定义枚举常量。同时，当有域和方法时，最后一个枚举常量必须以分号结尾。

	* 注意，枚举类型的构造函数必须是package-private或者private的。它自动的创建你在类的开始定义的枚举常量，你不能自己调用构造方法来创建一个枚举实例*
	
除了属性和构造方法，Planet还有自己的方法，可以计算每个星球的表面重力和每个物体在星球的重量。示例代码如下：

```
public enum Planet {
    MERCURY (3.303e+23, 2.4397e6),
    VENUS   (4.869e+24, 6.0518e6),
    EARTH   (5.976e+24, 6.37814e6),
    MARS    (6.421e+23, 3.3972e6),
    JUPITER (1.9e+27,   7.1492e7),
    SATURN  (5.688e+26, 6.0268e7),
    URANUS  (8.686e+25, 2.5559e7),
    NEPTUNE (1.024e+26, 2.4746e7);

    private final double mass;   // in kilograms
    private final double radius; // in meters
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
    private double mass() { return mass; }
    private double radius() { return radius; }

    // universal gravitational constant  (m3 kg-1 s-2)
    public static final double G = 6.67300E-11;

    double surfaceGravity() {
        return G * mass / (radius * radius);
    }
    double surfaceWeight(double otherMass) {
        return otherMass * surfaceGravity();
    }
    public static void main(String[] args) {
        if (args.length != 1) {
            System.err.println("Usage: java Planet <earth_weight>");
            System.exit(-1);
        }
        double earthWeight = Double.parseDouble(args[0]);
        double mass = earthWeight/EARTH.surfaceGravity();
        for (Planet p : Planet.values())
           System.out.printf("Your weight on %s is %f%n",
                             p, p.surfaceWeight(mass));
    }
}

```
	






























