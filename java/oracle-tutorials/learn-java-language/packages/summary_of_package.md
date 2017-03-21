### 创建和使用package的总结

为了把一个type放在一个包中，你需要使用package语句，并且需要将package语句放在包含此类的源文件的最开始。


如果想使用包中定义的public type，你需要采取如下3种方式之一：

* 使用type的权限定名
* import type
* import 整个package


源文件和class文件的路径与报名具有映射关系。


有时，你必须设置CLASSPATH系统变量，才能使编译器和JVM找到你的.calss文件。


