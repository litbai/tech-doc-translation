### DeadLock

死锁描述了这样一种场景，两个或多个线程由于相互等待而被永久的阻塞，如果没有外力的作用，它们都将无法继续执行，下面是一个示例：

Alphonse和Gaston是好朋友，而且他们都非常懂礼节。一个非常严格的礼节是当你向一个朋友鞠躬时，你必须始终保持鞠躬状态，直到你的朋友向你回鞠躬礼。但是，不幸的是，它没有考虑到一种情况：当你和你的朋友同时向对方鞠躬时，怎么办？下面是示例程序，DeadLock，对这种可能做了建模：

```
public class DeadLock {
	static class Friend {
		private String name;
		
		public Friend(String name) {
			this.name = name;
		}
		
		public String getName() {
			return name;
		}
		
		public synchronized void bow(Friend other) {
			 System.out.format("%s: %s"
                + "  has bowed to me!%n", 
                this.name, other.getName());
			other.bowBack(this);
		}
		
		public synchronized void bowBack(Friend other) {
			System.out.println(name + " is bowing back to " + other.getName());
		}
	}
	
	public static void main(String[] args) {
		Friend alphonse = new Friend("Alphonse");
		Friend gaston = new Friend("gaston");
		new Thread(new Runnable() {
			@Override
			public void run() {
				alphonse.bow(gaston);
			}
		}).start();
		new Thread(new Runnable() {
			@Override
			public void run() {
				gaston.bow(alphonse);
			}
		}).start();
	}
}

```


当运行上面的main方法时，非常可能发生的一种情况是，两个线程试图调用bowBack的时候，都将被阻塞。任何一个线程都将无法继续执行，因为它们在互相等待对方执行完bow方法。










