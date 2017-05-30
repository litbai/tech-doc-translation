#### Miscellaneous Methods in System


本节介绍上面几节没有涉及到的System类中的其他方法。


* arraycopy方法可以将一个数组的内容拷贝到另一个数组中。
* currentTimeMillis和nanoTime方法可以用来获取某个操作执行的时间。为了以毫秒为单位测量时间间隔，调用currentTimeMillis两次，从操作开始和结束处分别调用一次，然后将两次获得的毫秒数相减。类似的，调用nanoTime方法两次，然后相减可以获得一个操作执行的纳秒数。



** 再次提醒，currentTimeMillis和nanoTime方法的准确度取决于底层操作系统提供的时间服务，不能假设这两个方法得到的时间戳一定是准确的。也不要使用这两个方法来表示当前时间，如果想执行与时间和日期相关的操作，使用更高级的方法，例如 java.util.Calendar.getInstance和java.time包中的类。**


* System.exit()方法可以关闭JVM，此方法需要传递一个int类型的参数，表示退出时的状态。这个状态码可以被启动应用的进程获取。通常，状态码0表示正常退出，其他值表示错误码。


































