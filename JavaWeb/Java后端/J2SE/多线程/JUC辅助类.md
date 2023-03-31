# 一、减少计数（CountDownLatch）

- 减少计数辅助类可以实现前驱任务状态，当一个子任务完成后可以将计数器+1，当计数器达到指定的值（说明子任务都已完成），即可以唤醒阻塞的线程。

```java
public static void main(String[] args) throws InterruptedException {

    // 定义一个计数器 传入一个计数器的初始值
    CountDownLatch countDownLatch = new CountDownLatch(16);
    
    for (int i = 0; i < 16; i++) {
        new Thread(() -> {
            // 计数器从0开始计数
            // 调用 countDown 方法，将计数器 +1
            countDownLatch.countDown();
            System.out.println(Thread.currentThread().getName() + "已调用countDown方法");
        }, "Thread-" + i).start();
    }

    // 阻塞当前线程，直到计数器不断累加到指定的初始值 16，此时将线程唤醒
    countDownLatch.await();
    System.out.println("countDownLatch阻塞结束");

}
```

# 二、循环栅栏（CyclicBarrier）

- 循环栅栏类似减少计数，与其不一样的是，循环栅栏调用await阻塞方法才会使计数器+1，当计数器达到指定的值时，唤醒的为循环栅栏中指定的回调方法。

```java
public static void main(String[] args) throws Exception {

    // 定义一个循环栅栏 传入一个初始值 和 一个Runnable回调方法
    CyclicBarrier cyclicBarrier = new CyclicBarrier(16, () -> {
        // 当计数器累加到指定的初始值 16 时，将会执行此回调方法
        System.out.println("cyclicBarrier Runnable方法执行");
    });

    for (int i = 0; i < 16; i++) {
        new Thread(() -> {
            try {
                // 调用await阻塞方法时，计数器 +1
                System.out.println("线程 " + Thread.currentThread().getName() + " 已阻塞");
                cyclicBarrier.await();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }, "Thread-" + i).start();
    }

}
```

# 三、信号灯（Semaphore）

- 信号灯可以用于完成停车场问题，如有6辆车和3个车位，用户可以获取对应的车位，此时车位-1，如果没有车位了则必须阻塞等待，也可以将车位归还。


```java
public static void main(String[] args) {

    // 定义一个信号灯 传入一个信号量初始值
    Semaphore semaphore = new Semaphore(3);

    for (int i = 0; i < 6; i++) {
        new Thread(() -> {
            try {
                // 获取一个信号量，若有可用的信号量（信号量>0）
                // 则获取成功（信号量-1），否则阻塞当前线程
                semaphore.acquire();
                System.out.println(Thread.currentThread().getName() + "已获取到信号量");
                TimeUnit.SECONDS.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                // 释放一个信号量，将信号量+1
                semaphore.release();
            }
        }, "Thread-" + i).start();
    }
}
```

# 四、阻塞队列（BlockingQueue）

## 1、概念

- 作用：即一个队列数据解构。当队列是空的，从队列中获取元素的操作将会被阻塞；当队列是满的，从队列中添加元素的操作将会被阻塞。试图从空的队列中获取元素的线程将会被阻塞，直到其他线程往空的队列插入新的元素。试图向已满的队列中添加新元素的线程将会被阻塞，直到其他线程从队列中移除一个或多个元素或者完全清空，使队列变得空闲起来并后续新增。
- 阻塞队列可以用来完成经典的“生产者”和“消费者”模型问题，通过阻塞队列可以很便利的实现两者之间的数据共享问题。

## 2、接口实现类

（1）BlockingQueue接口

```java
public interface BlockingQueue<E> extends Queue<E>
```

（2）实现类

- <font color="red">ArrayBlockingQueue</font>
    - 基于数组数据结构的阻塞队列实现，其中生产者和消费者共用同一个锁对象。
- <font color="red">LinkedBlockingQueue</font>
    - 基于链表数据结构的阻塞队列实现，其中生产者和消费者分别采用独立的锁来控制数据同步。默认队列大小值为Integer类型的最大值。
- DelayQueue
    - 基于优先队列实现，其中元素只有当其指定的延迟时间到了，才能够从队列中获取到该元素，其中DelayQueue是一个不限制大小的队列，因此往队列中插入数据永远不会被阻塞，只有获取数据才会阻塞。
- PriorityBlockingQueue
    - 基于优先队列实现，类似DelayQueue不限制大小。并且PriorityBlockingQueue支持通过重写Compator方法判断对象的优先级。内部控制线程同步采用公平锁。
- SynchronousQueue
    - 一个无缓冲的等待队列，不存储元素的阻塞队列，即单个元素的队列。整个队列大小就为1。
- LinkedTransferQueue
    - 基于链表实现的无界阻塞队列，LinkedTransferQueue 采用一种预占模式。意思就是消费者线程取元素时，如果队列不为空，则直接取走数据，若队列为空，那就生成一个节点（节点元素为null）入队，然后消费者线程被等待在这个节点上，后面生产者线程入队时发现有一个元素为null的节点，生产者线程就不入队了，直接就将元素填充到该节点，并唤醒该节点等待的线程，被唤醒的消费者线程取走元素，从调用的方法返回。
- LinkedBlockingDueue
    - 基于链表实现的双向阻塞队列，可以从队头或者队尾插入或移除元素。

## 3、常用方法

- BlockingQueue接口常用方法

| 方法类型 | 抛出异常  | 返回布尔值 |  阻塞  |       阻塞超时       |
| :------: | :-------: | :--------: | :----: | :------------------: |
|   插入   |  add(e)   |  offer(e)  | put(e) | offer(e, time, unit) |
| 移除队头 | remove()  |   poll()   | take() |   poll(time, unit)   |
| 检查队头 | element() |   peek()   |   无   |          无          |

- 使用实例：

```java
public static void main(String[] args) {
    // 创建一个阻塞队列，指定长度为2
    BlockingQueue<String> queue = new ArrayBlockingQueue<>(2);
    queue.add("a");
    queue.add("b");
    // add方法入队第3个元素，抛出异常
    queue.add("c");
}
```





