### Lock Objects

同步代码依赖一种简单的可重入锁。这种锁很容易使用，但是有一些限制。java.util.concurrent.locks包中提供了更高级的锁语法。我们不会深入的探究这个包中的细节，本节只讨论包中的基本接口--Lock。

Lock对象跟synchronized代码需要使用的隐式锁的工作方式类似。就像隐式锁一样，同一时间仅能有一个线程可以拥有一个Lock对象，锁对象同样支持wait/notify机制，是通过与它们关联的Condition对象来实现这个机制的。


锁对象相比与隐式锁，其最大的优势在于当它试图获取一把锁但是没有成功时，它可以退出。当调用tryLock方法时，如果此时锁无法获取，则方法立刻返回或者等待一个指定的时间返回。在锁被获取之前，如果另一个线程发送了一个中断，lockInterruptibly方法可以返回。


让我们使用锁对象来解决在活跃性一节中碰到的死锁问题。Alphonse和Gaston已经被训练得到新技能--他们可以知道对方是否将要鞠躬。在Alphonse和Gaston向对方鞠躬之前，他们必须同时获取参与者的所有锁，此处即获取两把锁。下面是改进后的代码，Safelock。为了阐述lock语法的用途，我们假设Alphonse和Gaston非常迷恋他们新发现的技能(鞠躬时再也不会发生死锁)，所以不断的向对方鞠躬。

个人感觉此程序在语言情景上表述不通，但是不影响展示锁的使用方法。


```
public class Safelock {
	static class Friend {
		private final String name;
		private final Lock lock = new ReentrantLock();
		
		public Friend(String name) {
			this.name = name;
		}
		
		public String getName() {
			return name;
		}
		
		public boolean impendingBow(Friend bower) {
			boolean myLock = false;
			boolean yourLock = false;
			
			try {
				myLock = lock.tryLock();
				yourLock = bower.lock.tryLock();
			} finally {
				if (! (myLock && yourLock)) {
					if (myLock) {
						lock.unlock();
					}
					if (yourLock) {
						bower.lock.unlock();
					}
				}
			}
			return myLock && yourLock;
		}
		
		public void bow(Friend bower) {
			if (impendingBow(bower)) {
				try {
					System.out.format("%s: %s has" + " bowed to me!%n", this.name, bower.getName());
					bower.bowBack(this);
				} finally {
					lock.unlock();
					bower.lock.unlock();
				}
			} else {
				System.out.format("%s: %s started to bow to me, but saw that I was already bowing to him.%n", this.name, bower.getName());
			}
		}
		
		public void bowBack(Friend bower) {
			System.out.format("%s: %s has bowed back to me!%n", this.name, bower.getName());
		}
	}
	
	static class BowLoop implements Runnable {
		private Friend bower;
		private Friend bowee;
		
		public BowLoop(Friend bower, Friend bowee) {
			this.bower = bower;
			this.bowee = bowee;
		}
		
		public void run() {
			Random random = new Random();
			for(;;) {
				try {
					Thread.sleep(random.nextInt(10));
				} catch (InterruptedException e) {}
				bowee.bow(bower);
			}
		}
	}
	
	public static void main(String[] args) {
		final Friend alphonse = new Friend("Alphonse");
		final Friend gaston = new Friend("Gaston");
		new Thread(new BowLoop(alphonse, gaston)).start();
		new Thread(new BowLoop(gaston, alphonse)).start();
	}
}

```























