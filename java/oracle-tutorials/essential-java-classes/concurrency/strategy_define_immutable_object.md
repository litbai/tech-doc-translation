### A Strategy for Defining Immutable Objects

下面是创建不可变对象的一个简单的策略。并非所有的不可变对象都遵循这面的规则。这并不代表类的创建者过于草率--或者他们有很好的理由确信他们编码的类的对象在创建后永远不会发生改变。但是，他们的策略需要复杂的分析，不适用于初学者。

1. 不提供任何"setter"方法--可能改变对象的域的方法，包括基本数据类型域和引用数据类型域
2. 将所有的域用final关键字和private访问限定符修饰
3. 不允许创建子类来覆盖父类的方法。最简单的方法就是将类声明为final的。一个更复杂一些的方法是将构造方法声明为private的，提供一个工厂方法来创建实例。
4. 如果对象的域是引用数据类型，且指向一个可变对象，那么我们需要保证这个对象不被改变：
	* 不要提供可以改变这些对象的方法
	* 不要将这个对象的引用分享出去。不要存储传递给构造方法的对象引用，如果需要这个对象，则创建这个对象的copy，存储这个拷贝对象的引用。同样的，当需要返回对象的引用域时，返回这个引用的copy而不是原始引用。
	
	
对SynchronizedRGB应用上述策略：

1. SynchronizedRGB类中有两个setter方法。第一个方法，set，可以随意的改变对象，所以在不可变版本中应该删除此方法。第二个方法，invert，可以更改为返回一个新对象，而不是改变当前对象。
2. 所有的域都已经是private的，需要进一步声明为final的
3. 需要将SynchronizedRGB类声明为final的
4. 在SynchronizedRGB类中，只有一个域是引用数据类型(name)， 但name是String类型，本身就是不可变对象，所以无须担心name对象被改变。


应用以上的步骤，我们可以得到ImmutableRGB：

```
final public class ImmutableRGB {
	// values must be between 0 and 255
	final private int red;
	final private int green;
	final private int blue;
	final private String name;
	
	priate void check(int red, int green, int blue) {
		if (red < 0 || red > 255 || green < 0 || green > 255 || blue < 0 || blue > 255) {
			throw new IllegalArgumentException();
		}
	}
	
	
	public ImmutableRGB(int red, int green, int blue, String name) {
		check(red, green, blue);
		this.red = red;
		this.green = green;
		this.blue = blue;
		this.name = name;
	}
	
	public int getRGB() {
		return ((red << 16) | (green << 8) | blue);
	}
	
	public Strig getName() {
		return name;
	}
	
	public ImmutableRGB invert() {
		return new ImmutableRGB(255 - red, 255 - green ,255 - blue, "Inverse of " + name);
	}
}

```
	

	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	