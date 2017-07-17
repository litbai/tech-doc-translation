### The Path Class

Path 类，在JDK7中才引入，是java.nio.file包的入口类。如果你的程序中使用了file I/O，你将会学习这个类的强大特性。


** 版本问题： 如果你有JDK7之前的文件操作相关的代码，你也可以使用Path类提供的许多特性，听过File.toPath()方法。 **


就像它的名字暗示的那样，Path类代表文件系统中的一个路径。一个Path对象包含文件名和目录列表，用来检查、定义和操作文件。


一个Path对象跟底层的OS有关。在Solaris OS中，Path使用Solaris 的语法，例如： /home/joe/foo， 在Microsoft Windows上，一个Path使用windows OS的语法，例如： C:\home\joe\foo。一个Path与底层系统有关，不是平台无关的。你不能拿一个来自Solaris文件系统的Path对象跟一个来自Windows的Path对象做比较。


一个与Path对应的文件或者目录可能不存在。你可以创建一个Path对象，然后对它进行各种操作：你可以append it、extract pieces of it、compare it to another path。在合适的时间，你可以使用Files类提供的方法来判断一个Path对应的文件是否存在，创建与这个Path对应的文件、打开文件或者删除文件、改变它的权限等等。


下面的一节将详细介绍Path类。




























