## 有下限的通配符

有上限的通配符一节介绍了一个有上限的通配符限制了泛型的类型为一个特定的类或者其子类，通过使用extends关键字。同样的，一个有下限的通配符限定了泛型类型为一个特定的类或者其超类。

一个有下限的通配符，通过使用super关键字和通配符'?'来定义： '<? super A>'

** 注意，你可以为通配符指定一个上限，或者一个下限，但是你不能同时指定二者**

假设你正在写一个方法，这个方法可以将Integer对象放入一个list。为了更大的灵活性，你想要这个方法可以工作于List<Integer>, List<Number>以及List<Object>--任何可以存储Integer对象的list。


为了编写这个可以同时工作于Integer以及其父类的方法，你可以使用有下限的通配符语法'List<? super Integer>'。'List<Integer>'比'List<? super Integer>'的限制更加严格，因为前者只能匹配一个Integer类型的list，而后者可以匹配Integer及其父类的list。


下面的代码将1和10加入list的末尾：

```
public static void addNumbers(List<? super Integer> list) {
	for (int i = 1; i <= 10; i++) {
		list.add(i);
	}
}

```


ps: 自体会，这种技术使用的场景应该很少，暂没想到使用此技术的直观场景。
























