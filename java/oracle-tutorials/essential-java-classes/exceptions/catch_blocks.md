## The catch Blocks

你可以给try块关联一个或多个异常处理器，通过在try块后面添加一个或多个catch语句。 在try块结尾和catch块开头之间不能有其他代码：

```
try {

} catch (ExceptionType name) {

} catch (ExceptionType name) {

}

```

每一个catch块都是一个异常处理器，其参数的类型代表这个异常处理器可以处理的异常的类型。参数的类型，ExceptionType，声明了一个异常处理器可以处理的异常类型，且ExceptionType必须是Throwable类的子类。异常处理器可以通过变量name来访问异常。


catch块包含调用异常处理器时会执行的代码。当一个异常处理器是第一个可以处理抛出的异常类型的处理器时，运行时系统会调用这个异常处理器。如果被抛出的异常可以赋值给一个异常处理器的参数时，运行时系统认为匹配成功。


下面是writeList方法中的两个异常处理器：

```
try {

} catch (IndexOutOfBoundsException e) {
    System.err.println("IndexOutOfBoundsException: " + e.getMessage());
} catch (IOException e) {
    System.err.println("Caught IOException: " + e.getMessage());
}

```

异常处理器可以做很多事情，而不仅仅是打印错误信息或者终止程序。它们可以使程序从错误中恢复、提示用户来做某个决定或者通过异常链将错误传递给更高一层的异常处理器。


### 使用一个异常处理器catch多个异常类型


在java se7及以后，一个单独的catch块可以捕获多个异常类型。这个特性可以减少重复的代码，避免开发者为了简化程序而只捕获较宽泛的异常类型。


在catch语句中，声明异常处理器可以处理的异常类型，使用'|'符号，将每一种异常类型隔开：

```
catch (IOException|SQLException ex) {
    logger.log(ex);
    throw ex;
}

```

** 注意，如果使用一个异常处理器处理多种异常类型，那么catch语句的参数隐含是final的。在上述例子中，catch语句的参数ex是final的，因此，你不能在catch块中给它重新赋值。**



























