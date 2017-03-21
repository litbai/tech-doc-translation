### 给包命名

全世界的开发者都在使用java编程语言，这些开发人员很可能会使用同样的类名。事实上，前面的示例程序已经这样做了：它定义了一个Rectangle类，但是在java.awt包中已经存在一个名为Rectangle的类。同时，编译器允许这两个同名的类，因为它们位于不同的包中。类的权限定名包含包名，例如在graphics包中的Rectangle类的全限定名为graphics.Rectangle，而java.awt包中的Rectangle类的全限定名为java.awt.Rectangle。

上述的规则工作的很好，除非开发者使用了同样的包名+类名。那么应该怎么阻止这种情况的法伤？依靠惯例。


#### 命名惯例

包名按惯例全部使用小写字母，这样可以避免与其他的类或者接口命名冲突。

公司使用它们反转的因特网域名来作为它们包名的开头，例如，com.example.mypackage包是由example.com公司的程序员创建的mypackage包。


根据上述惯例，公司内部可能会发生命名冲突，这种情况需要在公司内部自己处理，或者加入region或者加入project名字(例如com.example.region.mypackage)。

java编程语言的包都是以java或javax开头。


某些情况下，因特网域名可能不会合法的包名。当因特网域名包含连字符或者其他特殊字符时，就会发生这种问题。如果包名以数字或者其他不合法字母开头或者包名包含java的保留字，例如'int'。在这些特殊情况下，推荐的解决方式是加上一个下划线，例如：


|Domain Name|Package Name Prefix|
|-----------|-------------------|
|hyphnated-name.example.org|org.example.hyphnated_name|
|example.int|int_.example|
|123name.example.com|com.example._123name|




































