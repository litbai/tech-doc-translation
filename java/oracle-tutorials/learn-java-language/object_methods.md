### Object类

Object类，在java.lang包中，位于类层次结构的顶端。每一个类都是Object类的子类，无论是直接子类还是间接的。你使用和编写的每一个类都继承了Object类的实例方法。你可能不会使用所有的这些方法，但是，如果你想使用这些方法，那么，你应该重写这些方法。Object类的方法有如下几个：

```
protected Object clone() throws CloneNotSupportException

public boolean equals(Object obj)

protected void finalize() throws Throwable

public final Class getClass()

public int hashCode()

public String toString()

```

另外，notify、notifyAll和wait方法在线程同步领域扮演者重要角色，后面的章节会介绍，本章暂不涉及，总共有5个此类方法：

```
public final void notify()
public final void notifyAll()
public final void wait()
public final void wait(long timeout)
public final void wait(long timeout, int nanos)

```

#### clone() 方法

如果一个类或者它的子类，实现了Cloneable接口，你可以使用clone()方法来创建一个此类对象的拷贝。为了创建一个克隆体，你将会写如下代码：

```
aCloneableObject.clone();

```

Object类中clone()方法的实现会检查调用clone()方法的对象所在的类有没有实现Cloneable接口。如果没有，clone()方法将抛出CloneNotSupportException异常。异常处理将会在后续介绍。当前，你只需要知道，如果你想重写clone方法，那么你需要如下定义：

```
protected Object clone() throws CLoneNotSupportException  或者是

public Object clone() throws CLoneNotSupportException

```

如果类实现了Cloneable接口，Object类的clone方法将会创建与源对象相同类的另外一个对象，并且将新对象的属性初始化为与源对象对应的属性相同的值。

使你的类可克隆的最简单的方法就是实现Cloneable接口，然后重写clone()方法。

对于一些类来说，Object类clone()方法的默认实现可以正常工作。但是，如果一个对象包含另一个对象的引用，记为objExternal，你可能需要重写clone方法才能满足你的需求。否则，源对象对ocjExternal的改变，将会影响克隆对象，因为克隆对象引用的objExternal也被改变了。这意味着源对象和克隆对象并不是独立的--为了解耦它们，你必须实现clone方法，创建objExternal的一个克隆。然后源对象和克隆对象各自引用不同的objExternal，二者解耦。

#### equals方法


equals方法比较两个对象是否相等，返回一个boolean值。Object类中的equals方法使用判等运算符==来判断两个对象是否相同。对于基本数据类型，equals方法可以得到正确的结果。对于引用类型，则不一定符合期望。Object类提供的equals方法实现，会测试两个对象引用是否相同，也就是被比较的两个对象是否是同一个对象。

为了判断两个对象是否是相等的（包含同样的信息，而不一定非要是同一个对象），你必须实现自己的equals方法，下面的Book类的示例：

```
public class Book {
    ...
    public boolean equals(Object obj) {
        if (obj instanceof Book)
            return ISBN.equals(((Book)obj).getISBN()); 
        else
            return false;
    }
}

```

下面的测试代码测试两个Book对象是否相同,结果输出"objects are equal"，虽然firstBook和secondBook是不同的引用，但是它们的ISDN是相同的。


```
// Swing Tutorial, 2nd edition
Book firstBook  = new Book("0201914670");
Book secondBook = new Book("0201914670");
if (firstBook.equals(secondBook)) {
    System.out.println("objects are equal");
} else {
    System.out.println("objects are not equal");
}

```

** 如果==运算符不符合你的期望，你应该总是覆盖equals方法。同时，如果你覆盖了equals方法，你也必须覆盖hashCode方法。 **


#### finalize()方法

Object类提供了一个回调方法，finalize()。这个方法可能会在一个对象被垃圾回收的时候回调。Object实现的finalize方法没有做任何事情，实现体为空。你可以实现finalize方法来进行清理工作，例如释放资源等。

finalize方法可能会被系统自动调用，但是它在什么时候被调用，甚至于会不会被调用，都是不确定了。因此，你不应该依赖于此方法来做清理工作。例如，如果你在执行了IO操作之后，不手动关闭文件描述符，期望使用finalize方法来关闭文件描述符，你可以会用尽系统规定的最大文件描述符打开个数。


#### getClass()方法

你不能覆盖getClass()方法。它被定义为final的本地方法。

getClass方法返回一个Class类型的对象，这个对象有一些方法，通过这些方法你可以获取关于这个类的信息，例如类名（getSimpleName()方法）、它的父类（getSuperClass()）,它实现的接口(getInterfaces())等。示例代码如下：

```
void printClassName(Object obj) {
    System.out.println("The object's" + " class is " +
        obj.getClass().getSimpleName());
}

```

Class类，在java.lang包中，内部有大量的方法(多于50个)。例如，你可以测试一个类是否是一个注解（isAnnotation()）, 是否是一个接口（isInterface()），是否是一个枚举类型（isEnum()）。你可以查看一个类的域（getFields()），查看类的方法（getMethods()）等等。

#### hashCode方法

hashCode方法的返回值是对象的hash code，是对象在内存中存放的地址，以16进制表示法表示。

根据定义，如果两个对象是相等的，那么它们的hash code必须是相等的。如果你覆盖了Object类的equals方法，你改变了判断两个对象是否相等的方式，这时候Object默认的hashCode方法就失效了。因此，如果你覆盖了equals方法，你必须同时覆盖hashCode方法。

#### toString()方法

你应该总是考虑为你的类重写toString方法。Object类的toString方法返回了对象的一个字符串表示，在调试的时候尤其有用。一个对象的字符串表示完全依赖于这个对象，所以你需要为你自己编写的类重写toString方法。

你可以使用toString和System.out.println方法打印一个对象的文本表示，例如打印一本书的信息：

```
System.out.println(firstBook.toString());

```

如果你重写了toString方法，那么代码输出可能为下面的形式：

```
ISBN: 0201914670; The Swing Tutorial; A Guide to Constructing GUIs, 2nd Edition

```















