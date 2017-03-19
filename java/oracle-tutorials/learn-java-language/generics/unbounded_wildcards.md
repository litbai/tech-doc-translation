## 无限定的通配符

无限定的通配符使用符号'?'， 例如List<?>，意思是一个不知道的类型的List。在两种场景下可能会用到无限定的通配符：

* 如果你在写一个方法，这个方法可以利用Object类的方法来实现，即你无须调用类型参数的方法，跟下面一条其实可以归为1条。
* 当你的代码使用的方法不依赖于类型参数时。例如，List.size()和List.clear()方法，实际上，我们会经常利用Class<?>，因为Class<T>中的大部分方法不依赖于T。


考虑如下方法，printList:

```
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}

```

printList()方法的目的是将一个列表遍历打印，而无需关注列表里存的具体类型，但是它限定了参数为List<Object>，这就产生了问题--它只可以接收List<Object>类型的参数，无法接收List<Integer>, List<String>等其他类型列表的参数，因为后两者不是前者的子类型。为了达到目的，可以使用无限定的通配符技术，如下：

```
public static void printLsit(List<?> list) {
	for (Object elem : list) {
		System.out.println(elem + "");
	}
	System.out.println();
}

```

因为对于任意的实体类型A，List\<A\> 是List\<?\> 的子类型，现在，你可以使用printList()方法打印任意类型的list：

```
List<Integer> li = Arrays.asList(1, 2, 3);
List<String> ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);

```


** 注意，Arrays.asList()，是一个静态工厂方法，它将一个数组转化为一个固定size的list。 **


认识到List<?>和List<Object>是不同的，这一掉非常重要。你可以插入一个Object或者任意类型到List<Object>中。但是你只可以向List<?>插入null。后续的章节会有更多关于在什么场景下如何定义通配符的信息。




































