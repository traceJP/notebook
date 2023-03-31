# 一、JUC基本概念

## 1、基本概念

- 在Java中，线程部分是一个重点，本篇文章说的JUC也是关于线程的。JUC就是<font color="red">java.util.concurrent</font>工具包的简称。这是一个处理线程的工具包，JDK1.5开始出现的。

## 2、进程与线程

- 进程与线程：
    - 进程︰指在系统中正在运行的一个应用程序，程序一旦运行就是进程;进程是资源分配的最小单位。
    - 线程∶系统分配处理器时间资源的基本单元，或者说进程之内独立执行的一个单元执行流。线程是程序执行的最小单位。
    - 进程：进程就是包换上下文切换的程序执行时间总和 = CPU加载上下文+CPU执行+CPU保存上下文可以说是一整个程序运行时刻的动态过程。
    - 线程：进程的颗粒度太大，每次都要有上下的调入，保存，调出。如果我们把进程比喻为一个运行在电脑上的软件，那么一个软件的执行不可能是一条逻辑执行的，必定有多个分支和多个程序段，就好比要实现程序A，实际分成 a，b，c等多个块组合而成。那么这里具体的执行就可能变成：程序A得到CPU -> CPU加载上下文，开始执行程序A的a小段，然后执行A的b小段，然后再执行A的c小段，最后CPU保存A的上下文。这里a，b，c的执行是共享了A的上下文，CPU在执行的时候没有进行上下文切换的。这里的a，b，c就是线程，也就是说线程是共享了进程的上下文环境，的更为细小的CPU时间段。
- 线程的状态枚举

```java
public enum State {
    NEW,  // 新建状态
    RUNNABLE,  // 就绪状态
    BLOCKED,  // 阻塞状态
    WAITING,  // 持续等待状态
    TIMED_WAITING,  // 短暂等待状态
    TERMINATED;  // 结束状态
}
```

## 3、概念区分

### （1）Wait和Sleep的区别

- sleep是Thread的静态方法，sleep不会释放锁，它也不需要占用锁。
- wait是Object对象的方法，任何对象实例都能调用。wait会释放锁，但调用它的前提是当前线程占有锁（即代码要在synchronized中）。
- 它们都可以被interrupted方法中断。

### （2）并发和并行

- 并发：同一时刻多个线程在访问同一个资源，多个线程对一个点。
- 并行：多项工作一起执行，之后再汇总。

### （3）用户线程和守护线程

- 用户线程：即用户自定义的线程

```java
// 现象：当main函数所在线程执行结束后，用户线程依然继续执行，JVM继续运行
public static void main(String[] args) {
    // 开启一个用户线程
    new Thread(() -> {
        while (true) {}  // 死循环执行
    }).start();
}
```

- 守护线程：如GC线程等线程

```java
// 现象：主线程结束后，守护线程将直接终止，JVM运行结束
public static void main(String[] args) {
    // 开启一个守护线程
    Thread thread = new Thread(() -> {
        while (true) {}
    });
    // 设置该线程为一个守护线程
    thread.setDaemon(true);
    
    thread.start();
}
```

## 4、管程

- 管程、Monitor监视器，即Java中所说的锁。
- 是一种同步机制，保证同一个时间，只有一个线程访问被保护数据或者代码。
- jvm同步基于进入和退出，使用管程对象实现的。

## 5、Synchronized关键字

- Synchronized作用可以将一个对象进行普通上锁操作。
- 采用synchronized不需要用户去手动释放锁，当synchronized方法或synchronized代码块执行完之后，或者发生异常的时候，系统会自动让线程释放对锁的占用。
- 作用范围：

```java
// 1 锁对象
synchronized(<对象>) { ... }
// 2 锁当前调用方法的对象
public synchronized void lock() { ... }
```

- synchronized实现同步的基础：Java中的每一个对象都可以作为锁。具体表现为以下3种形式。
    - 对于普通同步方法，锁是当前实例对象。
    - 对于静态同步方法（static），锁是当前整个类（Class字节码对象）。
    - 对于同步方法块，锁是synchonized括号里配置的对象

## 6、Lock类

- Lock实现提供比使用synchronized方法和语句可以获得的更广泛的锁定操作。它们允许更灵活的结构化，可能具有完全不同的属性，并且可以支持多个相关。<font color="red">即Lock提供了比synchoronized更多的功能</font>。
- Lock是一个类，通过这个类可以实现同步访问。并且Lock则必须要用户去手动释放锁，如果没有主动释放锁，就有可能导致出现死锁现象。
- 优点：Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized 时，等待的线程会一直等待下去，不能够响应中断。通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。
- Lock接口实现类包括
    - ReentrantLock(boolean fair = false)（可重入锁）
        - 构造参数可以传入一个boolean参数，用于指定该锁是否是公平锁，公平锁即每个线程都有相同的机会抢占锁，效率相对较低。非公平锁可能会出现线程饿死问题，即其他线程很难抢到锁进行业务执行，相对效率较高。
    - ReentrantReadWriteLock.ReadLock（读锁）实现ReadWriteLock读写锁接口
    - ReentrantReadWriteLock.WriteLock （写锁）实现ReadWriteLock读写锁接口


```java
// 定义一把锁
final Lock lock = new ReentrantLock();
// final Lock lock = new ReentrantLock(true); 定义一把公平锁
lock.lock();  // 上锁
try {
    ...
} finally {
    lock.unlock();  // 解锁
}
```

# 二、线程间通信

- 固定写法：调用、执行（判断阻塞当前线程、执行业务、唤醒其他线程）

## 1、线程通信实例（Synchronized实现）

- 实例：实现多个线程对单个资源进行操作+1和-1，当资源为0时+1，当资源为1时-1。
- 创建两个线程，分别执行incr和desc方法，可得1010交替现象。

```java
public synchronized void incr() throws InterruptedException {
    // 1 不符合操作要求 阻塞当前线程 直到被其他线程调用Object.notify方法
    // 阻塞 释放锁
    if (number != 0) {  // while防止虚假唤醒
        this.wait();
    }
    // 2 符合当前要求 完成当前任务
    number++;
    System.out.println(Thread.currentThread().getName() + " : " + number);
    
    // 3 当前任务完成 唤醒其他Object.wait线程继续执行任务
    this.notifyAll();
}
```

## 2、Wait的虚假唤醒问题

- 如果在一个条件判断是否应该wait的情况下，将可能产生中断和虚假唤醒

```java
public synchronized void desc() throws InterruptedException {
    // 1 因不符合条件 有多个线程在此处阻塞
    if (number != 1) {
        // 3 此时如果又被desc方法所在线程抢占到锁 则会在原地继续唤醒
        // 哪里等待阻塞 被唤醒就在哪里继续执行
        this.wait();
    }
    // 4 为0的值未经过 number!=1 的判读，将再此被-1 出现错误
    number--;
    System.out.println(Thread.currentThread().getName() + " : " + number);
    // 2 首个线程任务完成 通知唤醒所有线程
    this.notifyAll();
}
```

- 解决方案：通过循环进行判断，当唤醒的时候，从当前执行点再此进行判断。

```java
public synchronized void desc() throws InterruptedException {
    // 循环判断
    while (number != 1) {
        this.wait();  // 唤醒后不立刻执行业务逻辑，而是再此判断是否符合操作要求
    }
}
```

## 3、线程通信实例（Lock实现）

```java
public class LockInstance {

    private Integer number = 0;
	
    // 两个方法操作同一把锁
    private Lock lock = new ReentrantLock();
    // 锁的条件 用于线程的等待和唤醒
    private Condition condition = lock.newCondition();

    public void incr() throws InterruptedException {
        lock.lock();  // 1 上锁
        try {
            while (number != 0) {  // 2 判断是否符合业务条件
                condition.await();  // 2.1 阻塞等待锁
            }
            number++;
            condition.signalAll();  // 3 通知唤醒其他等待锁的线程
        } finally {
            lock.unlock();  // 4 解锁
        }
    }

    // 注意：这里不能加 synchronized 关键字
    // 否则在condition.signalAll()的瞬间，其他线程会给此方法上锁 从而造成死锁
    public void desc() throws InterruptedException {
        lock.lock();
        try {
            while (number != 1) {
                condition.await();
            }
            number--;
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }
}
```

# 三、线程的顺序通信

- 想要让不同的线程按指定的顺序执行，可以为每个线程定义一个标志位和对应的锁。
- 因此，当前线程需要先判断标志位是否是自己（不是自己则阻塞等待），否则可以执行业务，然后再将标志位赋值为其他线程标志位并唤醒其他阻塞线程，其他线程被唤醒后再此判读标志位。以达到执行线程的顺序通信的效果。

- 以下为3个线程分别按顺序执行三个方法

```java
// 标志位
private Integer flag = 1;

// 3把锁 对应 3个标志位 3个线程
private final Lock lock = new ReentrantLock();
private final Condition condition1 = lock.newCondition();
private final Condition condition2 = lock.newCondition();
private final Condition condition3 = lock.newCondition();

public void run1() throws InterruptedException {
    lock.lock();
    try {
        // 若当前标志位为自己 则 被唤醒后 可以执行业务
        if (flag != 1) {
            condition1.await();
        }
        System.out.println("方法1");
        // 唤醒第二个线程
        flag = 2;
        condition2.signal();
    } finally {
        lock.unlock();
    }
}

public void run2() throws InterruptedException {
    lock.lock();
    try {
        if (flag != 2) {
            condition2.await();
        }
        System.out.println("方法2");
        flag = 3;
        condition3.signal();
    } finally {
        lock.unlock();
    }
}

public void run3() throws InterruptedException {
    lock.lock();
    try {
        if (flag != 3) {
            condition3.await();
        }
        System.out.println("方法3");
        flag = 1;
        condition1.signal();
    } finally {
        lock.unlock();
    }
}
```

# 四、集合的线程安全

## 1、典型的并发方法抛出的并发修改异常

```java
List<T> strings = new LinkedList<>();
for (int i = 0; i < 100; i++) {
    strings.add("String-" + i);
}

// forEach相当于开启了一个线程遍历
// 同理也可以开启多个线程对同一个并发不安全集合类进行读写测试
strings.forEach(item -> {
    System.out.println(item);
    // 一边遍历 一边修改元素会出现异常
    // 这里会抛出 ConcurrentModificationException 异常
    strings.remove(item);
});
```

## 2、解决方案

- 使用Vector集合类进行遍历
    - Vector集合类对遍历使用的常用API做了Synchronized加锁处理
- 使用Collections工具类包装处理

```java
// list为线程安全集合
List<T> list = Collections.synchronizedList(new ArrayList<>());
```

- 使用CopyOnWriteArrayList类（写时复制技术）
    - 当该集合写入时，内部会将原集合复制一份再进行操作，操作完毕后将新集合覆盖原集合，读取再进行。

```java
List<T> list = new CopyOnWriteArrayList<>();
Set<T> set = new CopyOnWriteArraySet<>();
Map<T1, T2> map = new ConcurrentHashMap<>();
```

# 五、死锁检查

- 死锁造成原因：两个或者两个以上进程在执行过程中，因为争夺资源而造成一种互相等待的现象，如果没有外力干涉，他们无法再执行下去。
- 如何检查Java程序中是否存在死锁：
    - 可以通过JDK中自带的检测命令进行检测。

```shell
# jps命令可以列出当前正在运行的所有线程PID
# 也可以使用Idea自带的分析器选项卡查看进程PID
> jsp

# jstack命令可以查看一个线程是否存在死锁
# 若存在死锁 则会打印 Found X deadlock 并判断哪些线程导致的死锁原因
> jstack <PID>
```



