### 编写final的类和方法

你可以将类的部分或者全部方法声明为final的。你在方法声明中，使用final关键字来将方法声明为final的，那么子类将无法重写这个方法。Object类中就有一些final方法，例如getClass、notify、wait方法等，都是final的。

如果一个方法的实现不应该被改变，而且这个方法对于对象状态的一致性至关重要，那么，你应该将方法声明为final的。代码示例：

```
class ChessAlgorithm {
    enum ChessPlayer { WHITE, BLACK }
    ...
    final ChessPlayer getFirstPlayer() {
        return ChessPlayer.WHITE;
    }
    ...
}

```

在构造函数中调用的方法通常应该是final的，如果一个构造器调用了一个非final的方法，一个子类又重写了这个方法的话，这可能产生一些意外的不可接受的结果。

注意，你同样可以将整个类声明为final的，这个类不能被继承。这在一些场景下十分有效，例如，当创建一个不可变类的时候，就像String类一样。






















