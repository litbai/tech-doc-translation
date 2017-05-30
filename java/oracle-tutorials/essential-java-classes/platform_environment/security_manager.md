

### The Security Manager

一个安全管理器是定义了应用安全策略的对象。这个策略规定了哪些动作是不安全的或者系统敏感的。安全策略否定的所有动作都会抛出SecurityException异常。一个应用也可以查询安全管理器他所允许的所有安全的actions。


通常一个web applet运行时，会有一个安全管理器，这个安全管理器是由浏览器或者java web start plugin提供的。其他类型的应用运行时通常没有安全管理器，除非应用自己定义了一个安全管理器。如果没有安全管理器的话，那么这个应用不具有安全策略，不会对任何action做限制。


#### Interacting with the Security Manager

安全管理器的类型为SecurityManager， 为了或者这个安全管理器，你需要调用：

```

SecurityManager appsm = System.getSecurityManager();

```

如果没有安全管理器，appsm为null。

一旦获得了安全管理器，就可以请求它来判断指定的事件是否合法。很多标准库中的类都是这么做的。例如。System.exit()方法，用户来shutdown jvm，会先调用SecurityManager.checkExit方法来确保当前线程有终结程序运行的权限。


SecurityManager还定义了很多其他的方法来验证各种类型的动作。例如， SecurityManager.checkAccess用来验证线程是否具有访问权限，SecurityManager.checkPropertyAccess验证是否可以访问特定的系统属性。每个操作都有自己的checkXXX()方法。


除此之外，需要注意的是，checkXXX()方法代表一个操作已经被安全管理器做保护。通常，一个应用不需要直接调用checkXXX()方法。


#### Recognizing a Security Violation


当安全管理器存在的时候，很多安全管理器不允许的操作可能会抛出一个SecurityException异常，即使这个方法没有显示的声明会抛出SecurityException。例如，下面读取文件信息的代码：

```
reader = new FileReader("xanadu.txt");

```


当xanadu.txt文件存在且没有安全管理器的时候，这行代码可以正常执行。但是，假设这行语句插入到一个applet中，applet通常配备安全管理器，它不允许读取文件信息，将会抛出如下异常：



```
appletviewer fileApplet.html
Exception in thread "AWT-EventQueue-1" java.security.AccessControlException: access denied (java.io.FilePermission characteroutput.txt write)
        at java.security.AccessControlContext.checkPermission(AccessControlContext.java:323)
        at java.security.AccessController.checkPermission(AccessController.java:546)
        at java.lang.SecurityManager.checkPermission(SecurityManager.java:532)
        at java.lang.SecurityManager.checkWrite(SecurityManager.java:962)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:169)
        at java.io.FileOutputStream.<init>(FileOutputStream.java:70)
        at java.io.FileWriter.<init>(FileWriter.java:46)
...

```

上面抛出的异常类型为java.security.AccessControlException，它是SecurityException的子类。






























