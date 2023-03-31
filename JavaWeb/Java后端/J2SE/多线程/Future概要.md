# 一、Future未来任务

## 1、基本概念

- Future接口是Java5新增的接口，提供了一种异步并行计算的功能。
- Future接口(FutureTask实现类)定义了操作异步任务执行一些方法，如获取异步任务的执行结果、取消任务的执行、判断任务是否被取消、判断任务执行是否完毕等。即Future接口可以为主线程开一个分支任务，专门为主线程处理耗时费力的复杂业务。
- 比如主线程让一个子线程去执行任务，子线程可能比较耗时，启动子线程开始执行任务后，主线程就去做其他事情了，忙其它事情或者先执行完，过了一会才去获取子任务的执行结果或变更的任务状态。

```java
// Future接口方法
boolean isCancelled();  // get线程是否被取消
boolean cancel(boolean);  // set线程是否取消
V get();  // 阻塞获取线程的返回值
V get(long, TimeUnit);  // 阻塞指定时间获取线程的返回值
boolean isDone();  // 线程是否执行完毕
```

## 2、Callable接口

- 创建线程的第三种方式（Callable）

    - 创建线程一共有4种方式：通过实现Runnable接口，继承Thread类，实现Callable接口，通过线程池获取。
    - 其中通过实现Callable接口，可以创建一个线程，该线程需要指定一个泛型，用于表示当前线程的返回值类型。

    ```java
    // 实现 Callable 接口，指定返回值为 String类型
    public class MyCallable implements Callable<String> {
    	// 实现Callable接口的call方法，与Runnable种的run方法类似
        @Override
        public String call() throws Exception {
            // ...异步任务
            // 与Runnable不同，Callable接口实现的线程拥有返回值
            return "异步任务返回值";
        }
    }
    ```

- 启动Callable接口线程
    - Callable接口不同于通过传统的Thread类构造启动，需要使用Future接口的实现类来进行异步任务的开启。
    
    ```java
    // Thread 构造方法 只支持 Runnable 接口实例对象参数
    // 不支持 Callable 接口实例对象
    Thread thread = new Thread(Runnable target);
    ```

## 3、FutureTask子类

## （1）FutureTask的实现接口

- FutureTask类可以用于包装一个实现Runnable和Callable接口的线程对象（Runnable或Callable定义线程执行方法，FutureTask将其包装成一个异步任务），他的本质是一个<font color="red">包装对象</font>。并且FutureTask对象可以实时的监控到线程的执行，如线程是否执行完毕，线程执行完毕的返回值，是否取消线程等。
- FutureTask类图：实现了Runnable、Future接口。此时通过包装的Callable对象就可以传入Thread类中进行线程的开启。

<img src="Future%E6%A6%82%E8%A6%81.assets/image-20230324201659568.png" alt="image-20230324201659568" style="zoom: 50%;" />

## （2）开启一个线程（异步任务）

- 定义一个异步任务（包装线程执行方法对象）
    - ``FutureTask<T>()``构造方法支持传入 Runnable 和 Callable 对象。并返回一个FutureTask对象。
    - 其中泛型用于指定异步任务的返回值

```java
// 1 传入Runnable对象 需要传递第二个参数，即线程完成后的返回值
FutureTask<String> future = new FutureTask<>(new Runnable() {
    @Override
    public void run() {
        System.out.println("runnable");
    }
}, "runnable线程返回值");

// 2 传入Callable对象
FutureTask<String> future1 = new FutureTask<>(new Callable<String>() {
    @Override
    public String call() throws Exception {
        System.out.println("callable");
        return "callable线程返回值";
    }
});
```

- 执行异步任务：当FutureTask对象创建后，再使用Thread构造FutureTask对象，即可开启一个新的线程。此时可以调用FutureTask对象的方法，对当前对象构造方法内包装的线程进行操作。

```java
// 1 通过 Thread 开启线程
public static void main(String[] args) {
    
    // 定义一个异步任务
    FutureTask<String> future = new FutureTask<>(() -> {
        System.out.println("callable");
        return "callable线程返回值";
	});
    
    // 开启线程
    Thread thread = new Thread(future);
    thread.start();
    
    // 阻塞直到future异步任务执行完毕,获取返回值
	String res = future.get();
}

// 2 通过线程池开启线程
public static void main(String[] args) {
    
    // 定义一个线程池
    ExecutorService executorService = Executors.newCachedThreadPool();
    
    // 定义一个异步任务
    FutureTask<String> future = new FutureTask<>(() -> {
        System.out.println("callable");
        return "callable线程返回值";
	});
    
    // 将异步任务提交到线程池中执行
    // 线程池submit支持传入 Runnable、Callable、带返回值的Runnable三种实现类开启线程
    executorService.submit(future);
}
```

## （3）FutureTask优缺点

- 优点：
    - future和线程池异步多线程任务配合，能显著提高程序的执行效率。只需要在程序执行过程中开启多个异步任务，然后在后面使用get方法拿到所有异步任务的执行结果，再进行汇总即可。
- 缺点：
    - get阻塞：一旦调用了get方法求结果，如果任务没有完成，则会容易造成程序阻塞。
    - isDone轮询：采用isDone轮询结果，则会消耗无谓的CPU资源，而且也不一定能及时的获取到任务的结果。
    - 如果想要异步获取结果，通常都采用轮询的方式获取结果，尽量不要直接的阻塞程序。

```java
// 轮询等待异步任务结果示例
public static void main(String[] args) {
    FutureTask<String> future = new FutureTask<>(() -> {
        // 执行长时间任务
	});
    // 开启线程
    new Thread(future).start();
	
    // 1 get方法直接阻塞获取结果 - 如果没有计算完成则一直阻塞
    future.get();
    
    // 2 isDone轮询获取结果
    while (true) {
        if (future.isDone()) {  // 判断任务是否已经完成
           future.get();
        } else {
            // 若任务没有完成 则休眠一会再询问
            TimeUnit.MILLISECONDS.sleep(500);
        }
    }
}
```

# 二、CompletableFuture

## 1、基本概念

- 在Java8中，CompletableFuture提供了非常强大的Future的扩展功能，可以帮助我们简化异步编程的复杂性，并且提供了函数式编程的能力，可以通过回调的方式处理计算结果，也提供了转换和组合CompletableFuture 的方法。它可能代表一个明确完成的Future，也有可能代表一个完成阶段（CompletionStage )，它支持在计算完成以后触发一些函数或执行某些动作。
- 解决的痛点：
    - 在传统的FutureTask异步编程下，容易造成线程的阻塞。阻塞的方式和异步编程的设计理念相违背。所以对于真正的异步处理我们希望是可以通过传入回调函数，在Future结束时自动调用该回调函数，这样，我们就不用等待结果。（与JavaScript的Promise对象概念一致）
    - 其中CompletableFuture提供了一种观察者模式类似的机制，可以让任务执待完成后通知监听的一方。
    - 当异步任务结束后，会自动调用某个对象的回调方法；主线程设置好回调后，不再关心异步任务的执行，异步任务之间可以顺序执行；异步任务出错时，也会自动回调某个对象的方法。
- CompletableFuture架构图：实现了Future接口（即与Future可以兼容使用），实现了更加强大的CompletionStage接口。

<img src="Future%E6%A6%82%E8%A6%81.assets/image-20230325191852427.png" alt="image-20230325191852427" style="zoom: 67%;" />

## 2、CompletionStage接口

- CompletionStage代表异步计算过程中的某一个阶段，一个阶段完成以后可能会触发另外一个阶段
- 其中CompletionStageAPI均为Lambda表达式参数传递，用户可以通过传递Lambda表达式更加方便的传递回调函数（像JavaScript一样）。
- 一个阶段的执行可能是被单个阶段的完成触发，也可能是由多个阶段一起触发（即可以同步，也可以异步，即可以合作完成，也可以单独完成）。
- 接口实现类的基本使用：

```java
public static void mian(String[] args) {
    // 1 不推荐使用构造方法直接创建CompletableFuture对象
    // CompletableFuture<T> future = new CompletableFuture<>();
    
    // 2 直接开启一个异步任务（CompletableFuture会使用默认线程池自动开启线程）
    // 推荐直接使用静态方法调用，此处调用runAsync方法（类似Runnable），返回值为Void对象
    CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            System.out.println("异步任务执行");
    });
    future.get();  // runAsync的Void对象返回 null
}
```

## 3、基本静态方法

### （1）runAsync

- 执行异步任务，无返回值（Runnable）。

```java
public static void mian(String[] args) {
    // 传入一个Runnable接口实现类，使用系统默认线程池（ForkJoinPool）执行
    CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
        System.out.println("future");
    });
   
    // 使用自定义线程池执行异步任务
    ExecutorService executors = Executors.newCachedThreadPool();
    CompletableFuture<Void> future1 = CompletableFuture.runAsync(() -> {
        System.out.println("future1, executor");
    }, executors);
}
```

### （2）supplyAsync

- 执行异步任务，有返回值（Callable），传入的为``Supplier<U> supplier``函数式接口。

```java
public static void mian(String[] args) {
    // 指定泛型为返回值的类型，传入一个异步任务方法并执行
    CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
        System.out.println("future2");
        return "hello";
	});
    
    // 线程池参数使用与 runAsync() 方法一致
    CompletableFuture<String> future3 = CompletableFuture.supplyAsync(() -> {
        System.out.println("future3, executor");
        return "hello";
    }, executors);
}
```

### （3）whenComplete

- 使用whenComplete可以接收异步任务执行结束后的返回值，回调函数并处理。
- 传入一个BiConsumer函数式接口
    - 第一个参数为上一步的返回值， 第二个参数为上一步抛出的异常（Throwable）

```java
public static void mian(String[] args) {
    CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
        return "hello";
    }).whenComplete((v, e) -> {
        // v = hello
        // e = null 无异常
    });
}
```

### （4）exceptionally

- 异步捕获链式调用上一步中抛出的异常，参数e为异常（Throwable），并提供捕获异常异步任务的返回值。

```java
.exceptionally(e -> {
    System.out.println("exception: " + e);
    return "exception";
});
```

## 4、常用回调方法

### （1）获取结果

- 可以通过get、join、getNow三种方法获取异步任务执行的结果。

```java
public static void mian(String[] args) {
    CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            System.out.println("异步任务执行");
    });
    
    // 1 阻塞当前线程获取执行结果，会抛出检查异常
    try {
        // 同FutureTask中get(long timeout, TimeUnit unit)可以指定阻塞时间
        future.get();
    } catch(Exception e) { ... }
    
    // 2 与 get() 方法一致，但是在编译期间不会抛出异常
    future.join();
    
    // 3 getNow() 方法支持判断是否计算完毕并返回一个默认值（立刻获取结果不阻塞）
    // 当程序执行到getNow时将直接判断当前线程是否已经完成
    // 如果完成则返回线程的返回值，否则直接返回当前方法传递的默认值参数
    future.getNow(<返回值类型T>);
}
```

### （2）打断线程（触发计算）

- 当异步任务还在执行的时候，可以使用complete方法打断线程的执行，此时线程将直接停止，并且直接返回complete方法中传递的参数值。

```java
public static void mian(String[] args) {
    CompletableFuture<String> future = CompletableFuture.runAsync(() -> {
        TimeUnit.SECONDS.sleep(2);
        return "result";
    });
    // 此时future线程显然还在运行
    TimeUnit.SECONDS.sleep(1);
    
    // 直接打断future线程，让其直接返回一个值（"complete"）
    future.complete("complete");
    future.join();  // 获取线程的返回值，即"complete" 
}
```

### （3）处理结果

- 当计算结果存在依赖关系，可以将这两个线程<font color="red">串行化</font>，即其中一个线程的执行必须等待另一个线程执行的结果时，可以将其链式串行。
- thenApply
    - thenApply支持将上一个线程的执行结果传递到下一个线程中。
    - 异常处理：由于存在依赖关系，当前步骤如果有异常，整个链式调用都停止（当前步错，不走下一步）。


```java
public static void mian(String[] args) {
    CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
        return 1;
    }).thenApply(f -> {  // 当上一步执行完毕，将开启此处异步任务
        // f = 1
        return f + 2;  // future异步任务新的返回值
    });
    // ...同理可以连续串行其他
    
    future.get();  // 异步任务结果为 3
}
```

- handle
    - handle使用与thenApply一致，但其回调方法传入两个参数，第一个为上一个线程中的执行结果，第二个为上一个线程中抛出的异常。
    - 异常处理：当出现异常时，只会停止当前步骤，不会影响其他步骤。

```java
public static void mian(String[] args) {
    CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
        return "result1";
    }).handle((v, e) -> {  // v为上一步执行结果，e为上一步出现的异常
        // f = result1 ；e = null
        return "result2";
    });
    
    future.get();  // "result2"
}
```

### （4）消费结果

- 可以对线程执行后的值进行获取并传入回调函数中消费。可以用于代替获取结果的方法（get、join），使用回调函数对线程执行后的结果进行处理。
- thenAccept
    - thenAccept方法会传递上一步的返回值，即可以获取到上一步的返回值。

```java
public static void mian(String[] args) {
    CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
        return "result1";
    }).thenAccept(r -> {
        // r = "result1"
    });
    // 或者使用方法引用 .thenAccept(System.out::println)
}
```

- thenRun
    - thenRun方法使用与thenAccept一致，其区别就是thenRun方法不会获取到上一步的返回值（我不需要）。

```java
public static void mian(String[] args) {
    CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
        return "result1";
    }).thenRun(() -> {
        // ...回调处理
    });
}
```

### （5）任务执行与线程池的选择

- 对于上述thenRun、thenAccept、thenApply方法，均是对线程执行顺序的编排。
    - thenRun：任务A执行完执行B，并且B不需要A的结果。
    - thenAccept：任务A执行完执行B，B需要A的结果，但是任务B<font color="red">无返回值</font>。
    - thenApply：任务A执行完执行B，B需要A的结果，同时任务B<font color="red">有返回值</font>。
- <font color="red">XXXAsync方法详解</font>：（线程池的选择）
    - 在上述方法中，都有对应的thenRunAsync、thenAcceptAsync、thenApplyAsync方法，它们可以对工作的线程池进行自由的选择，可以选择用户自己创建的线程池，也可以选则使用默认的线程池。其可以传递第二个指定线程池的参数（即类似最初runAsync、supplyAsync的第二个参数）。
    - 这里以thenRun举例，不同的线程池工作顺序，其他同理。

```java
// 1 最初使用自定义线程池
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
    // 用户自定义线程池
    TimeUnit.SECONDS.sleep(5);
}, executorService).thenRun(() -> {
    // thenRun 表示跟随最初
    // 如果最初是自定义线程池，则就用自定义线程池；如果最初是默认线程池，则用默认线程池
    // *备注：系统会自动优化线程的切换，如果线程之间处理的太快，则直接使用main线程处理
    TimeUnit.SECONDS.sleep(5);
});

// 2.1 使用thenRunAsync指定新的线程池
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
	// 未指定，默认ForkJoinPool线程池
}).thenRunAsync(() -> {
    // thenRunAsync也未指定，默认ForkJoinPool线程池
});
// 2.2
CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
	// 指定自定义线程池，自定义线程池
}, executorService).thenRunAsync(() -> {
    // thenRunAsync未指定（不跟随），默认ForkJoinPool线程池
    // 若指定其他用户自定义的线程池，则使用其他用户自定义的线程池
});
```

### （6）执行速度选用

- 可以同时开启两个异步任务，然后CompetableFuture提供判断两个异步任务谁先返回（谁先完成任务），然后直接返回最快完成的异步任务返回值。

```java
public static void mian(String[] args) {
    // 异步任务1 执行2秒
    CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
        TimeUnit.SECONDS.sleep(2);
        return "future1 win";
    });
    // 异步任务2 执行4秒
    CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
        TimeUnit.SECONDS.sleep(4);
        return "future2 win";
    });

    // 使用future1对象与future2判断，两者谁更快完成
    // 最快完成的结果传递到参数s中
    future1.applyToEither(future2, s -> {
        // s = "future1 win"
        return s;  // 属于future1对象，可以继续链式调用
    });
}
```

### （7）结果合并

- 当两个异步任务都完成后，最终可以把两个任务的结果一起交给thenCombine处理。其中一个任务如果已经完成，则需要等待另一个任务一并完成。

```java
public static void mian(String[] args) {
    // 异步任务1
    CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {
        return 1;
    });
    // 异步任务2
    CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> {
        return 2;
    });

	// 使用异步任务1 与 异步任务2 合并结果
    future1.thenCombine(future2, (x, y) -> {
        // x为future1的返回值，y为future2的返回值
        return x + y;  // 结果为 3
    });
}
```

## 5、最佳实践

- 模拟查询一个商品在不同商店里的价格，假设查询每家需要1秒钟。

```java
class NetMall {
    private String name;
    public Double calePrice() {
        // 业务模拟查询实践1秒
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 模拟返回价格随机值
        return ThreadLocalRandom.current().nextDouble() * 100;
    }
}
```

- 普通查询方式（一家一家的查询）。

```java
public void oldQuery() {
    // 分别查询 然后汇总返回 3秒
    List<String> res = list.stream()
            .map(item -> item.getName() + item.calePrice())
            .collect(Collectors.toList());
    System.out.println(res);
}
```

- 使用stream并行流查询

```java
public void newQueryByParallelStream() {
    // 并行查询 1秒
    List<String> res = list.parallelStream()
            .map(item -> item.getName() + item.calePrice())
            .collect(Collectors.toList());
    System.out.println(res);
}
```

- 使用CompletableFuture查询

```java
public void newQueryByCompletableFuture() {
    // 开启多个异步任务查询 得到对应的任务对象集合 1秒
    List<CompletableFuture<String>> completableFutureList = list.parallelStream()
            .map(item -> {
                return CompletableFuture.supplyAsync(() -> {
                    return item.getName() + item.calePrice();
                });
            })
            .collect(Collectors.toList());
    // 二次映射汇总结果
    List<String> res = completableFutureList.stream()
        .map(CompletableFuture::join)
        .collect(Collectors.toList());
    System.out.println(res);
}
```





