今天这篇文章给大家介绍几个 Java 并发包中很重要的几个同步类，只要你是学 Java 的，我觉得你至少得会使用，主要介绍以下四个同步类的使用，以及一些原理。

1. ReentrantLock
2. CountDownLatch
3. CycliBarrier
4. Semaphore

#### 1、ReenTrantLock

reenTrantLock 是可重入锁的意思

> 可重入锁：就是一个线程获取的锁之后，这个现在在没有释放该锁之前，可以再次进入获取该锁，这像递归调用，同一个线程多次进入同一个锁。

在 ReenTrantLock 中，通过调用 lock() 方法来获取锁，调用 unlock() 方法来释放锁的机制进行代码块的同步。我们先来看下怎么使用：假如有 10 个人要去银行办理事情，但银行只有一个窗口，每次只能有一个人进去办理事情，其他人得等待。

```java
public class ReentrantLockTest {
    static ReentrantLock reen = new ReentrantLock();

    static class MyThread implements Runnable{
        @Override
        public void run() {
            try {
                reen.lock();//尝试获取锁
                System.out.println(Thread.currentThread() + "拿到锁了，窗口办理事情中");
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }finally {
                // 释放持有的许可证
                reen.unlock();
            }
        }
    }
    // 开始测试
    public static void main(String[] args) {
        // 10个人来银行办理，不过每次只能一个人进去办理办理
        for (int i = 0; i < 10; i++) {
            Thread t = new Thread(new MyThread());
            t.start();
        }
    }
}
```
在使用上，还是非常简单的，ReentrantLock 的实现依赖于同步器框架 AbstranctQueuedSynchronizer。

> AbstrancQueuedSynchronizer，这个类也简称为 AQS。AQS 使用一个整型的 volatile 变量(名为 state)来维护同步状态，而这个变量的操作是靠 CAS 机制来保证他的原子性。对于 AQS 和 CAS 具体是如何的，在后续的文章我再来跟大家介绍。


默认情况下，ReenTrantLock 使用的是**非公平锁**，我们也可以通过构造器指定是否要公平锁。


> 公平锁：做个比喻就是在银行门口等待的人，先来的，等下可以先获取到锁来办理事情。即在锁的获取上，是按照时间的顺序公平获取的。

> 非公平锁：和公平锁相反，慢来的也有可能先获取到锁。
```java
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

#### 2、CountDownLatch

这是一个计数同步类，他可以让当前线程处于等待状态，直到计数为0时才能继续往下执行。

主要的一些方法

```java
// 构造器，必须制定计数次数
public CountDownLatch(int count) {
    if (count < 0) throw new IllegalArgumentException("count < 0");
    this.sync = new Sync(count);
}

// 进入等待状态，直到计数数为 0 时，才可以继续往下执行，如果该线程被打断的话，则会抛出 InterruptedException 异常
public void await() throws InterruptedException {
    sync.acquireSharedInterruptibly(1);
}
    
// 计数减一
public void countDown() {
sync.releaseShared(1);
}
```
先来展示里例子吧：假如我们用一个线程来计算 1-100 的和，一个线程来计算 101-200 的和，最后用一个线程来计算这两个线程的结果。代码如下：
```java
public class CountDownLatchTest {
    static CountDownLatch count = new CountDownLatch(2);
    static int sum1 = 0; // 1 到 100 相加
    static int sum2 = 0; // 101 到 200 相加

    public static void main (String[] args) {
        // 线程 1
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 执行 1 到 10 相加
                for (int i = 1; i <= 100; i++) {
                    sum1 += i;
                }
                count.countDown();
            }
        }).start();
        // 线程2
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 执行 101 到 200 相加
                for (int i = 101; i <= 200; i++) {
                    // 假设每次执行相加都会花一些时间
                    sum1 += i;
                }
                count.countDown();
            }
        }).start();
        // 等到两个线程都执行完，在把他们相加
        try {
            count.wait();
            System.out.println("最终的结果：" + (sum1 + sum2));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

```
值得提醒的是，该类没有继承、实现任何接口，不过内部维护着一个 Sync 类，该内部类继承了同步阻塞队列。

```java
 private static final class Sync extends AbstractQueuedSynchronizer
```
实际上，CountDownLatch 可以说是只是一个包装，他的所有方法都是通过调用 Sync 方法来解决的，而 Sync 方法的运算操作都是基于 CAS 机制。

例如我们调用 countDown() 方法，他的实际调用过程是这样的：

```java
public void countDown() {
        sync.releaseShared(1);
}
```
接着releaseShared(1)内部实现逻辑如下：
```java
public final boolean releaseShared(int arg) {
    if (tryReleaseShared(arg)) {
        doReleaseShared();
        return true;
    }
    return false;
}
```
也就是说，真正执行减 1 的是 doReleaseShared() 方法，该方法的内部实现逻辑主要是依靠 CAS 机制来实现计数减一

#### 3、CyclicBarrier 同步类

Barrier 是阻拦的意思，他和 CountDownLatch 有点类似，当指定的线程数都执行到某个位置的时候，他才会继续往下执行。主要特点是可以让几个线程相互等待，就像被一道围栏给阻塞了一样，等到人齐了，在一同往下执行。

该类主要的几个方法(注意，我这里只是列出几个主要方法，实现代码全部省略)

```java
// 类的声明
public class CyclicBarrier{
    // 构造器，指定线程个数
    public CyclicBarrier(int parties){}；
    
    // 调用该方法，进入等待状态，只要当指定线程数都到达这个位置，所有等待的线程才会继续往下执行。 
    public int await() throws InterruptedException, BrokenBarrierException{};
    
    // 和没参数的相似，只是我们可以指定一个超时时间，也就是说，等待超过了这个时间线程就会网线执行（抛出的异常也被我省略了）
    public int await(long timeout, TimeUnit unit){};
    
    // 从新设置状态，也就是说，重新初始化了这个类。
    public void reset() {}；
    
    // 获取目前有多少个正在等待的线程
    public int getNumberWaiting(){}；
```

** 例子展示**

和上个例子一样，也是让一个线程执行 1-100相加，另外一个执行 101 -200 相加，最后让第三个线程吧结果进行汇总。代码如下：

```java
public class CyclicBarrierTest {
    static CyclicBarrier barrier = new CyclicBarrier(3);
    static int sum1 = 0; // 1 到 100 相加
    static int sum2 = 0; // 101 到 200 相加
    public static void main (String[] args) {
        // 线程 1
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 执行 1 到 10 相加
                for (int i = 1; i <= 100; i++) {
                    sum1 += i;
                }
                // 进入等待
                try {
                    barrier.await();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
        // 线程2
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 执行 101 到 200 相加
                for (int i = 101; i <= 200; i++) {
                    // 假设每次执行相加都会花一些时间
                    sum1 += i;
                }
                // 进入等待
                try {
                    barrier.await();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
        // 等到两个线程都执行完，在把他们相加
        try {
            barrier.await();
            System.out.println("最终的结果：" + (sum1 + sum2));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

通过这个类的声明我们知道，这个类没有继承任何同步类，也没有像 CountDownLatch 维护着一个内部同步类，那他是如何保证线程安全的呢？我们可以看下 await() 方法内存都实现了啥。


```java
    public int await() throws InterruptedException, BrokenBarrierException {
        try {
            return dowait(false, 0L);
        } catch (TimeoutException toe) {
            throw new Error(toe); // cannot happen
        }
    }
```
主要逻辑就是调用了 dowait(false, 0L) 这个方法，我们在看一下这个方法的执行逻辑，这个方法里面的代码有点多，我就挑出重要的几行代码

```java
    private int dowait(boolean timed, long nanos)
        throws InterruptedException, BrokenBarrierException,
               TimeoutException {
        // 从这里可以看出，
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            // count 的值就是我们在构造器传入的值
            int index = --count;
            // 此处省略几千行代码
        }finally{
            lock.unlock();
        }
```
通过跟踪方法的实现逻辑可以知道，CycliBarrier 主要是依赖于 ReenTrantLock 来实现线程同步安全的,不新我们在看另外的方法

```java
    public void reset() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            breakBarrier();   // break the current generation
            nextGeneration(); // start a new generation
        } finally {
            lock.unlock();
        }
    }
```

说实话，CountDownLatch 与 CyclicBarrier 还是有点类似的，都可以指定一定的线程，让他们都实现到某个位置，在继续执行下去。下面我们来说说 CountDownLatch 与 CyclicBarrier 的一些不同点。

相比与 CountDownLatch，CyclicBarrier 实例可以重复利用，我们可以通过重置方法 reset() 实现重复利用。而且，CycliBarrier 是**多个线程互相等待**，而 CountDownLatch 是**一个线程等待其他线程执行完毕**。还有在同步锁上也有些不同，一个是依赖于 ReentrantLoc，一个是依赖于 CAS（虽然 ReentrantLock的底层也是依赖于 CAS 机制）。

####4、 semaphore

semaphore 是信号量的意思，通过这个类，可以控制控制某个资源最多可以被几个线程所持有。例如我们平时去银行办理一些事情，银行只有 3 个窗口，那么最多可以有 3 个人在办理事情了，其他人只能等待别人办理好才能上去办理。对于这种需求，就可以使用这个 semaphore 线程类了。

我们先来看看这个类的定义

```java
// 注意，该累本身没有实现任何同步接口
public class Semaphore implements java.io.Serializable {}

```
该累本身没有实现任何同步接口，不过他和CountDownLatch一样，有一个实现了同步队列接口的内部类。

```java
 abstract static class Sync extends AbstractQueuedSynchronizer {)
```
我们来看看Semaphore 两个主要的构造器

```java
// 构造器，指定总共有多少个许可
public Semaphore(int permits) {
        sync = new NonfairSync(permits);
    }
// 构造器，可以通过制定参数 fair 来决定是要创建公平锁还是非公平锁
 public Semaphore(int permits, boolean fair) {
        sync = fair ? new FairSync(permits) 
        : new NonfairSync(permits);
    }
```
接着看下几个主要的方法

```java
// 如果还有许可，则继续执行，否则将进入等待状态，知道有许可
public void acquire() throws InterruptedException {
    sync.acquireSharedInterruptibly(1);
}

// 是否持有许可
public void release() {
    sync.releaseShared(1);
}
```
从方法中可以看的出，Semaphore 相当于是一层包装，实际调用的方法都是内部类 Sync 里面的方法。

下面写一个例子：假如 有10 个人要去银行办事，单银行只有 4 个窗口，也就是说最多只有 4 个人同时在办理，其他人只能等等。

```java
public class SemaphoreTest {

    // 假设有两个许可证，每次打饭都需要一个许可证
    static Semaphore semaphore = new Semaphore(4);

    static class EatingThread implements Runnable{
        @Override
        public void run() {
            // 尝试是否能够拿到可用的许可证
            try {
                semaphore.acquire();
                System.out.println(Thread.currentThread() + "拿到许可证了，窗口办理事情中");
                Thread.sleep(10000);
            } catch (Exception e) {
                e.printStackTrace();
            }finally {
                // 释放持有的许可证
                semaphore.release();
            }

        }
    }

    // 开始测试
    public static void main(String[] args) {
        // 10个人来银行办理，不过总共只有3个窗口，每次只能有三个人同时在办理
        for (int i = 0; i < 10; i++) {
            Thread t = new Thread(new EatingThread());
            t.start();
        }
    }
}

```
这个类的主要特点是，可以对资源的同时访问人数进行控制。

该类还有如下几个重要的方法

```java
// 尝试是否能拿到一个许可证，无论是否拿到，都会继续往下执行，只是拿到了就返回 true，否则返回 false
public boolean tryAcquire() {
    return sync.nonfairTryAcquireShared(1) >= 0;
}
// 延迟一定的时间再去获取许可证
public boolean tryAcquire(long timeout, TimeUnit unit)
    throws InterruptedException {
    return sync.tryAcquireSharedNanos(1, unit.toNanos(timeout));
}
// 一次性获取指定数量的许可证
public void acquire(int permits) throws InterruptedException {
    if (permits < 0) throw new IllegalArgumentException();
    sync.acquireSharedInterruptibly(permits);
}
// 判断是否还有可用的许可证
 public int availablePermits() {
    return sync.getPermits();
}
```

#### 总结

今天给大家介绍了并发包中几个重要、常用的几个类，通过这些类我们可以发现，他们都是基于 AQS、CAS 来实现的，当然，AQS 也是依赖于 CAS，所以，想要搞懂这些类，我们还得苦下工夫看看 AQS 是如何实现的，

