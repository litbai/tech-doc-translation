### Concurrent Random Numbers


在JDK7中，java.util.concurrent包中包含一个方便使用的类，ThreadLocalRandom，用于多线程或者ForkJoin任务中获取随机数。


在多线程中访问一个随机数，可以使用ThreadLocalRandom取代Math.random()方法，可以避免过多的竞争条件，从而获得更好的性能。


你只需调用ThreadLocalRandom.current()方法，然后调用它的生成随机数的方法获得一个随机数：

```
int r = ThreadLocalRandom.current().nextInt(4, 77);

```