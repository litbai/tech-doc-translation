### Environment Variables

许多操作系统会利用环境变量(environment variables)来给用户传递配置信息。类似于java平台的Properties，环境变量也是key/value对，key和value都是字符串。设置和使用环境变量的惯用方式随着操作系统和命令行解析器不同而不同。如果想要学习如何将环境变量传递给应用，参考你的系统文档。


#### Querying Environment Variables

在java平台之上，应用会调用System.getenv来获取环境变量的值。如果调用没有参数的getenv方法，则返回一个只读的Map实例，key为环境变量的name，value为环境变量的值，下面是示例代码：

```
import java.util.Map;

public class EnvMap {

	public static void main(String args[]) {
		Map<String, String> env = System.getenv();
		for (String envname : env.keySet()) {
			System.out.format("%s=%s%n", envName, env.get(envName));
		}
	}
	
}

```

如果调用getenv(String name)方法，则会返回环境变量name对应的值。如果没有此环境变量，则返回null，示例如下：

```
public class Env {
	public static void main(Stirng[] args) {
		for (String env : args) {
			String value = System.getenv(env);
			if (value != null) {
				System.out.format("%s=%s%n", env, value);
			} else {
				System.out.format("%s is not assigned. %n", env);
			}
		}
	}
}

```


#### Platform Dependency Issues

不同的操作系统有不同的环境变量。例如，windows对环境变量名是大小写不敏感的，但是UNIX是区分大小写的。不同的操作系统使用环境变量的方式也不一样。Windows会在一个环境变量名为USERNAME的变量中存储用户名，而UNIX可能会使用USER或者LOGNAME变量。


为了最大化程序的可移植性，当一个变量值可以用System.getProperty()方法得到时，不要再用getenv方法。例如，如果操作系统提供了一个用户名，其对应的system property永远为user.name，而环境变量名则依据操作系统的不同而不同。
















































