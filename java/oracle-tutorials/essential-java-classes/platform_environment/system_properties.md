### System Properties

在Properties一节中，我们介绍了一个应用可以使用Properties对象来维护它的配置。java平台也使用一个Properties对象来维护它自己的配置。System类持有一个Properties对象，这个对象维护了当前java工作环境的各项配置。系统配置包括当前用户、java的版本、路径分隔符等，下面的表格描述了一些最重要的系统配置：

|Key|Meaning|
|---------------------|-------------------------------|
|file.separator|路径分隔符，"/"forUNIX，"\"for Windows|
|java.class.path|classpath，可以找到包含class的jar包,多个classpath之间用'path.separator'分隔|
|java.home|JRE的安装路径--/Library/Java/JavaVirtualMachines/jdk1.8.0_91.jdk/Contents/Home/jre|
|java.vendor|java开发商--Oracle Corporation|
|java.vendor.url|java开发商网址--http://java.oracle.com|
|java.version|jre版本号 -- 1.8.0_91|
|line.separator|\n for UNIX|
|os.arch|x86_64|
|os.name|Mac OS X|
|os.version|10.11.5|
|path.separator|: for UNIX, ; for Windows|
|user.dir|工作目录--/Users/chengyu/JavaPrj|
|user.home|用户主目录--/Users/chengyu|
|user.name|用户名--chengyu|


** 安全方面的考虑：访问系统属性通常需要安全管理器(Security Manager)来进行权限验证。在applet中通常是这种形式。 **


#### Reading System Properties


System类有两个方法可以读取配置属性：getProperties()和getProperty().


其中，getProperty方法又有两个重载的版本。简单的版本接收一个String类型的参数，这个参数指定要访问的属性的key，方法的返回值是对应key的value，如果没有name为key的属性，则返回null。

```
System.getProperty("path.separator");

```

第二个版本的getProperty方法接收两个String类型的参数。其中第一个参数为key，第二个参数为返回的默认值，如果不存在这个key的话。例如下面的代码会访问一个名为"subliminal.message"的系统属性，这不是一个合法的属性，如果直接调用System.getProperty("subliminal.message"),则正常情况下会返回null，如果调用System.getProperty("subliminal.message", "Buy StayPuft Marshmallows!"),则会返回"Buy StayPuft Marshmallows!


System.getProperties()方法则会返回一个Properties对象，这个对象包含了所有的系统属性。


#### Writing System Properties


为了改变已有的系统属性，需要使用System.setProperties方法。这个方法接收一个Properties的对象作为参数，这个对象包含了所有需要设置的系统属性，将取代System类原先的props域。

** 注意：改变系统属性，具有潜在的风险，需要考虑周全。许多系统属性在初始化之后就不会再读了，只是提供了一种信息。改变一些属性可能会有一些意想不到的副作用。**



接下去的示例，PropertiesTest，创建了一个Properties对象，并且使用myProperties.txt文件初始化了这个对象, 文件内容如下：

	subliminal.message=Buy StayPuft Marshmallows
	

然后PropertiesTest	类使用System.getProperties方法，设置了新的系统属性：


```
import java.io.FileInputStream;
import java.util.Properties;


public class PropertiesTest {
	public static void main(String[] args) throws Exception{
		FileInputStream propFile = new FileInputStream("myProperties.txt");
		Properties prop = new Properties(System.getProperties());
		prop.load(propFile);
		
		System.setProperties(prop);
		System.getProperties().list(System.out)
	}

}

```


注意，PropertiesTest类在创建prop时，使用的方式为：

Properties prop = new Properties(System.getProperties());


这个语句创建了一个Properties对象prop，构造函数中传入的参数为应用启动时初始化的系统属性对象。然后应用从文件myProperties.txt中加载了额外的参数到对象prop中，并且将System的properties设置为prop。这段代码的作用是将myProperties.txt文件中定义的属性append到已有的系统属性中。


上述示例有两点需要注意：

* 创建Properties对象时，可以调用无参的构造函数，例如

```
	Properties prop = new Properties();

```
* 上述代码可能会覆盖系统属性，如果myProperties.txt文件中，包含有与原系统属性同名的属性时，假如myProperties.txt包含如下的一行文本：

	java.vendor=Acme Software Company

那么将更改原来的java.vendor属性。所以，要小心，避免无意的覆盖已有的系统属性。


System.setProperties方法可以改变运行时系统属性。这种改变不是持久的，也就是说下次系统启动时，这种改变就不会生效了，运行时系统(runtime system)会在每次启动应用时重新初始化系统属性。如果想要每次启动都包含你上次的配置，那么你需要在每次系统shutdown的时候，将当前系统的配置写入一个文件中，下一次系统启动时，再次读取此文件中的内容。







































