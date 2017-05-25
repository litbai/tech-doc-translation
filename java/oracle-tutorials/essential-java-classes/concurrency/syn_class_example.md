### A Synchronized Class Example

下面的类，SynchronizedRGB，定义了一个代表颜色的对象。每个对象使用3个整数来代表一个颜色，一个字符串变量表示颜色名称。

```
public class SynchronizedRGB {
	// values must be between 0 and 255
	private int red;
	privete int green;
	private int blue;
	private String name;
	
	private void check(int red, int green, int blue) {
		if (red < 0 || red > 255 || green < 0 || green > 255 || blue < 0 || blue > 255) {
		return new illegalArgumentException();
		}
	}
	
	public SynchronizedRGB(int red, int green, int blue, String name) {
		check(red, green, blue);
		this.red = red;
		this.green = green;
		this.blue = blue;
		this.name = name;
	}
	
	public void set(int red, int green, int blue, String name) {
		check(red, green, blue);
		synchronized (this) {
			this.red = red;
			this.green = green;
			this.blue = blue;
			this.name = name;
		}
	}
	
	pubic synchronized int getRGB() {
		return ((red << 16) | (green << 8) | blue);
	}
	
	public synchronized String getName() {
		return name;
	}
	
	public synchronized void invert() {
		red = 255 - red;
		green = 255 - green;
		blue = 255 - blue;
		name = "Inverse of " + name;
	}
	
}

```


当使用SynchronizedRGB时，需要十分小心的避免多线程看到不一致的对象状态。假设，一个线程执行了下面的代码：

```

SynchronizedRGB color = new SynchronizedRGB(0, 0, 0, "Pitch Black");
...

int myColorInt = color.getRGB(); // statement 1
String myColorName = color.getName(); // statement 2

```

如果另外一个线程在上述的statement 1和 statement 2之间，调用了color.set()方法，那么myColorInt和myColorName就不匹配了。为了避免这个后果，需要将这两条语句绑在一起：

```
synchronized(color) {
	int myColorInt = color.getRGB();
	String myColorName = color.getName();
}

```

这种不一致的风险仅存在于可变对象中，对于不可变版本的SynchronizedRGB则不存在这种顾虑。



























