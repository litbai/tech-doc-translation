### Other Configuration Utilities


本节介绍一下其他的几个配置工具。

The Preferences API allows applications to store and retrieve configuration data in an implementation-dependent backing store. Asynchronous updates are supported, and the same set of preferences can be safely updated by multiple threads and even multiple applications. For more information, refer to the Preferences API Guide.

An application deployed in a JAR archive uses a manifest to describe the contents of the archive. For more information, refer to the Packaging Programs in JAR Files lesson.

The configuration of a Java Web Start application is contained in a JNLP file. For more information, refer to the Java Web Start lesson.

The configuration of a Java Plug-in applet is partially determined by the HTML tags used to embed the applet in the web page. Depending on the applet and the browser, these tags can include <applet>, <object>, <embed>, and <param>. For more information, refer to the Java Applets lesson.


java.util.ServiceLoader类提供了一个简单的Service Proder功能。一个Service Provider是Service(一个public的接口或者抽象类)的一个实现。Service Provider中的类通常实现了servicer中定义接口或者继承了servicer中定义的抽象类。Service provider可以安装为扩展。也可以通过将它们放在classpath，或者使用平台特定的方式来获取service provider。


















