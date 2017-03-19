## 泛型擦除

在泛型擦除的过程中，java编译器擦除了所有的类型参数，使用泛型的限定类型或者Object类替换泛型。

考虑如下泛型类，代表了链表中的一个结点：

```
public class Node<T> {
	private T data;
	private Node<T> next;
	
	public Node(T data, Node<T> next) {
		this.data = data;
		this.next = next;
	}
	
	public T getData() {
		return data;
	}
}

```

因为类型参数T是无限定的，所以编译器会用Object类来替换T：

```
public class Node {

    private Object data;
    private Node next;

    public Node(Object data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Object getData() { return data; }
    // ...
}

```

下面的例子，泛型类Node使用一个有限定的类型参数：

```
public class Node<T extends Comparable<T>> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}

```

java编译器会使用第一个限定类Comparable，来替换类型参数T ：

```
public class Node {

    private Comparable data;
    private Node next;

    public Node(Comparable data, Node next) {
        this.data = data;
        this.next = next;
    }

    public Comparable getData() { return data; }
    // ...
}

```

























