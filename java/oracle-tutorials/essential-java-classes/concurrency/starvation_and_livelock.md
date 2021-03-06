### Starvation and Livelock

饥饿和活锁问题相对死锁来说较为少见，但是程序员在编写并发程序时仍然可能遇到这两个问题。

#### Starvation

饥饿描述了这样一种场景 -- 一个线程长时间不能够获得共享资源的访问权限而导致它长时间不能推进。当共享资源被“贪婪”的线程长时间所占有时，可能会发生这种情况。例如，假设一个对象提供了一个同步方法，这个同步方法比较复杂，运行期比较长。如果一个线程在非常频繁的调用这个方法，那其他也需要同步访问这个对象的线程将经常被阻塞。

#### Livelock

一个线程通常会对其他线程的动作做出回应。如果这时A线程对B线程的一个动作做出回应，同时B线程也向A线程的动作作出回应，那么就可能发生活锁问题。跟死锁一样，产生活锁问题的线程也将无法继续执行。但是，这些线程并没有处于阻塞状态--它们只是忙于互相回应（瞎忙）。这就类似于，两个人在一条走廊相遇，A把右手边的空间让给B过，而同时B把左手边的空间让给A过，依次循环，那么此时二人还将多次面对面无法通行。。。































