## Upper Bounded Wildcards

你可以使用一个有上限的通配符来放松对一个变量的限制。例如，你想要写一个方法，这个方法可以接收List<Integer>, List<Double>以及List<Number>类型的参数，你可以使用有上限的通配符来完成这个任务。

声明一个有上限的通配符，你需要使用通配符符号'?'，后面跟上extend关键字，然后跟上限定类型。注意，上述的extends关键字表示或者extends一个class或者implements一个interface。


为了编写一个可以工作于List<Number>以及Number的子类，比如Integer、Double、Float的方法，你可以使用： 'List<? extends Number>'。List<Number>比List<? extends Number>的限制要大，因为前者只能匹配List<Number>类的对象，而后者可以可以匹配List<Number>, List<Integer>以及任意的Number的子类的List对象。


考虑下面的process方法：

```
public static void process(List<? extends Foo> list) { /* ... */ }

```

有上限的通配符'<? extends Foo>'，Foo可以是任意类型，可以匹配Foo以及Foo的任意子类型。process方法可以访问List中的元素，并且以Foo的类型来接收这个元素：

```
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}

```

在上面的foreach语句中，elm变量迭代了list中的每一个元素。Foo类中定义的任意方法，都可以通过elm变量来调用：

```
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}

```

下面的代码，使用了一个Interger的List，打印出： sum = 6.0:

```
List<Integer> li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));

```


而一个Double类型的List同样可以调用sumOfList方法：

```
List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));

```


































