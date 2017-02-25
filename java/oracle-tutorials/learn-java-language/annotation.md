## Annotations

Annotations, 一种元数据，提供了非程序本身的数据信息。Annotations对其注解的代码的行为没有直接的影响。Annotations有很多用途：

* 提供编译时信息：编译器可以使用Annotation来检测错误或者抑制警告信息；
* 编译期和部署期处理：软件工具可以通过处理注解信息，产生代码、xml和其他文件。
* 运行时处理：一些注解可在运行时被检查到。

本章教程解释了注解可以用在哪里，如何应用注解，java se预定义的注解有哪些，如何将注解与可插拔式系统相结合写出具备强制类型检查的代码，如何实现可重复注解。

### 注解基础知识

#### 注解的形式

注解的最简单的形式如下所示：

```
@Entity

```

@符号向编译器表明这是个注解类型，下面的代码，注解的名字为OVerride

```
@Override
void mySuperMethod() {}

```

注解可以包含元素，这些元素可以有名字也可以没有名字，元素可以有值：

```
@Author(name = "Benjamin Franklin", date = "2017/02/17")
calss MyClass() {}

@SuppressWarnings(value = "unchecked")
void myMethod() {}

```

如果注解中没有元素，则小括号可以省略，就像@Override一样。

同样，可以在一个元素(类、方法、域等)上声明多个注解：

```
@Author(name = "Jane Doe")
@EBook
class MyClass { ... }

```

如果声明的多个注解类型相同，称为可重复的注解：

```
@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }

```

java se 8支持可重复注解。注解类型可以是在java.lang 或者 java.lang.annotation包中预定义的类型，例如Override和SuppressWarnings。你也可以定义自己的注解。Author和Ebook注解就是自定义的注解。

#### 注解可以用在哪里

注解可以标注在声明中：类声明、方法声明、域声明以及其他程序元素的声明。当在声明中使用注解时，按照约定，每一个注解，单独占一行。

对于java se 8，注解可以应用到任何使用类型的地方(use of type)。这种注解称为类型注解(type annotation)，下面是一些示例：

* 类实例创建表达式： new @Interned MyObject();
* 强制类型转换： myString = (@NonNull String) str;
* implement 语句： class UnmodifiableList<T> implements @Readonly List<@Readonly T> {...}
* throw语句：void monitorTemperature() throws @Critical TemperatureException {...}

#### 声明一个注解类型

许多注解的作用是用代码来取代注释。假设一个软件小组通常会在类的开始用注释提供一些重要的信息：

```
public class Generation3List extends Generation2List {

   // Author: John Doe
   // Date: 3/17/2002
   // Current revision: 6
   // Last modified: 4/12/2004
   // By: Jane Doe
   // Reviewers: Alice, Bill, Cindy

   // class code goes here

}

```

如果想用注解来达到同样的效果，你必须首先定义注解类型，语法如下：

```
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}

```

注解的定义与接口的定义非常类似，除了interface关键字前面会有一个@符号（@=AT，as in Annotation Type）。Annotation是一种特殊的接口，接口的概念下一章将详细介绍。当前，你无须理解接口。上述的注解包括元素声明，看起来像是方法声明，注意，这些元素可以定义默认值。

当定义完注解类型之后，你可以使用这个注解，给它填充元素值，如下所示：

```
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   // Note array notation
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {

// class code goes here

}

```

为了使上面定义的注解@ClassPreamble的信息可以使用java-doc产生文档，你必须使用@Document注解来注解@ClassPreamble注解，@Document称为元注解：

```
// import this to use @Documented
import java.lang.annotation.*;

@Documented
@interface ClassPreamble {

   // Annotation element definitions
   
}

```

### 预定义的注解类型

java se API中预定了一些注解。一些注解被java编译器使用，而另一些注解使用在其他注解上，称为元注解。

##### 被java编译器使用的注解

在java.lang包预定义的注解包括：@Deprecated、@Override、@SuppressWarnings。

* @Deprecated注解表面被标记的元素已经被弃用了，这些元素不应该在程序中再继续使用。当代码中使用了被@Deprecated注解标注的方法、类或者域时，编译器会发出一条警告。当一个元素被弃用了，它也应该使用javadoc 的 @deprecated标记进行文档化，如下所示。无论是在comment还是在注解中使用的@符号并非是巧合，它们在概念上是相关的。同时，注意到javadoc tag是以小写字母开头的，而注解则是以大写字母D开头的。

```
// Javadoc comment follows
    /**
     * @deprecated
     * explanation of why it was deprecated
     */
    @Deprecated
    static void deprecatedMethod() { }
}

```

* @Override注解会通知编译器：被标注的元素将要覆盖父类中的一个元素。方法覆盖将会在下一章介绍。注意到，当需要覆盖一个方法时，虽然@Override注解并不是必须的，但是它可以避免一些错误。如果一个被@Override标注的方法没有正确的覆盖父类中的方法，编译器会产生一条错误。

```
// mark method as a superclass method
   // that has been overridden
   @Override 
   int overriddenMethod() { }

```

* @SuppressWarnings注解会通知编译器：抑制特殊的警告信息。下面的例子中，deprecated方法被使用，编译器会发出一条警告。在这种情况下，使用@SuppressWarnings注解可以阻止编译器发出这条警告。

每一个编译器警告属于一个警告类型。jav语言规范定义了两种警告类型：deprecation and unchecked。当设计泛型时，如果使用jdk5以下的版本，可能会产生一条unchecked警告。为了阻止多种类型的警告，使用如下的语法：

```
@SuppressWarnings({"unchecked", "deprecation"})

```


```
// use a deprecated method and tell 
   // compiler not to generate a warning
   @SuppressWarnings("deprecation")
    void useDeprecatedMethod() {
        // deprecation warning
        // - suppressed
        objectOne.deprecatedMethod();
    }

```

* @SafeVarargs注解：当应用于方法时，表面方法内部不会对方法的参数做一些不安全的操作。当使用这个注解时，与varargs相关的unchecked警告会被阻止。
* @FunctionalInterface注解：在java se 8中引入，表面一个接口是一个函数式接口。


#### 用于其他注解的注解

应用于其他注解的注解称为元注解。java.lang.annotation包中预定义了几种元注解类型。

* @Retention注解：表面被标记的注解存储在何种级别：

	* RetentionPolicy.SOURCE:被标记的注解仅存在于源代码中，编译器会忽视这些注解。
	* RetentionPolicy.CLASS：编译期会保留这些注解，但是会被JVM忽略。
	* RetentionPolicy.RUNTIME：注解信息会保留到运行期，既JVM会识别这些注解。

* @Document注解：表面当使用被@Document元注解标注的注解时，这些注解应该使用javadoc生成文档。
* @Target注解：表面被标记的注解可以应用到何种元素上：
	* ElementType.ANNOTATION_TYPE：可以应用到其他注解上
	* ElementType.CONSTRUCTOR：可以应用到构造器上
	* ElementType.FIELD：可以应用到域上
	* ElementType.LOCAL_VARIABLE：可以应用到局部变量
	* ElementType.METHOD：可以应用到方法上
	* ElementType.PACKAGE：可以应用到包声明上
	* ElementType.PARAMETER：可以应用到方法参数上
	* ElementType.TYPE：可以应用到一个类的任意元素
	
* @Inherited注解：表明一个注解可以从它的父类中继承。当一个程序员查询一个类被标记的特定注解时，如果这个类没有被这个特定的注解标记，那么这个类的父类必须被这个注解标记。@Inherited注解仅用于类声明。

* @Repeatable注解：java 8引入，表面一个注解可以在同一个声明中被使用多次。


### Type Annotations and Pluggable Type Systems

在java 8之前，注解只可以被应用到声明中。java 8发布后，注解也可以被应用到任意的type use上。这表明，凡是在使用类型的场景，就可以使用注解。使用类型的场景包括：实例创建表达式： new、类型转换cast、implements语句和throws语句。这种形式的注解称为type annotation，在前面的一节中已展示了几个示例。

type annotations是为了支持java程序对强类型检查的分析能力。java 8 并没有提供一个类型检查框架，但是它允许你自己写一个类型检查框架，以可插拔模块的实现形式，与java编译器结合来进行类型检查。

例如，如果你想确保你程序中一个特殊的变量永远不会被设置为null值，你希望避免NullPointerException。你可以写一个自定义的插件来自行检查。然后你可以用注解来标注这个特殊变量，表明它永远不会别设置为null。变量声明也许是类似如下的形式：

```
@NonNull String str;

```

当你编译这段代码时，在命令行中引入NonNULL模块，编译器如果检测到变量可能会被赋予null值，则编译器就会打印一条告警，提醒你修改代码，避免错误。当你修正了代码，去除了所有的警告，则程序运行时就不会产生潜在的错误。

你可以使用多个类型检查模块，每一个模块检查特定类型的错误。这样，你就可以在java类型系统之上编程，当需要的时候，加入自定义的检查逻辑。

当你明智的使用type annotation和pluggable type checkers，你可以写出更健壮的代码。

在很多情况下，你必须自己写类型检查模块。有很多第三方机构已经帮你做了这个事情。例如，你可以使用华盛顿大学创建的类型检查框架。这个框架中有一个NonNull模块、一个正则表达式模块和一个互斥锁模块。想要了更多的信息，请参考： https://net.zju.edu.cn/index_3.html?url=http://types.cs.washington.edu/checker-framework/


### 可重复注解

在一些情况下，你可能想要在一个声明或者type use上使用多个相同的注解。从java 8版本，你可以这么做。

例如，你在编写一个时间服务类，可以使你在指定的时间进行调度，类似UNIX系统的cron服务。现在，你想定期运行一个方法--doPeriodicCleanup,在每个月的月底以及每个周五的上午11：00。你创建了一个@Schedule注解，在doPeriodicCleanup上使用了两次。第一次指定了每个月的月底，第二次指定了周五的上午11:00，如下所示：

```
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() { ... }

```

你可以在可使用标准注解的任何地方使用可重复注解。例如，你有一个处理未授权访问异常的类。你为经理使用一个@Alert注解，为管理员使用另一个@Alert注解：

```
@Alert(role="Manager")
@Alert(role="Administrator")
public class UnauthorizedAccessException extends SecurityException { ... }

```

出于兼容性的原因，可重复注解存储在一个container annotation中，这个container annotation是由编译器自动生成的。为了帮助编译器自动生成container annotation，你需要在你的代码中做两个声明。

* Step1. 声明一个可重复的注解类型：

注解必须用@Repeatable元注解进行标注，例如：

```
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
public @interface Schedule {
  String dayOfMonth() default "first";
  String dayOfWeek() default "Mon";
  int hour() default 12;
}

```

 @Repeatable 元注解的值，既括号里的元素，是java编译器会为可重复注解自动生成的container annotation的类型。在上述示例中，container annotation类型是Schedules，因此，可重复的注解 @Schedule将会在 @Schedules注解中存储。
 
 * Step2：声明Container Annotation Type
 
 containing annotation类必须有一个value元素，它的类型是一个数组。数组存储的类型是可重复注解的类型，示例如下：
 
 ```
 public @interface Schedules {
    Schedule[] value();
}
 
 ```


#### 检索注解

在java 反射API中，有一些方法可以用来检索注解。有些返回一个单独注解类型的方法，例如AnnotatedElement.getAnnotationByType(Class<T>)，在可重复注解只出现一次的情况下，仍然会返回一个单独的注解。如果一个声明或type use被多于一个可重复注解标注，你可以首先得到它们的container annotation。这样的话，以前的代码仍然可以正常运行。java8引入了一些其他方法，这些方法可以扫描 container annotation，并一次性返回其中包含的多个注解，例如： AnnotatedElement.getAnnotations(Class<T>)，对于方法的更详细的描述，可以参考AnnotatedElement 类的定义。

#### 设计上的考虑

当设计一个注解类型时，你必须考虑这个注解的基数。因为，现在你可以不使用、使用一次或者多次使用一个注解。你也可以使用@Target元注解限制注解可以应用的地方。例如，你可以创建一个可重复的注解，这个注解仅能用于标注方法和域。仔细设计你的注解是很重要的，这可以保证程序员在使用注解时，发现这个注解既灵活又功能强大。















