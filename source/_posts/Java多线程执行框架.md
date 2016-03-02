title: Java多线程执行框架
date: 2016-03-02 21:25:06
tags: [java多线程,Executor]
------------------------

# 为什么需要执行框架呢？

使用一般的new方法来创建线程有什么问题呢？一般的new线程的方式一般要给出一个实现了Runnable接口的执行类，在其中重写run()方法，然后再在将这个执行类的对象传给线程以完成初始化，这个过程中线程的定义和执行过程其实是杂糅在一起了，而且每次new一个新的线程出来在资源上很有可能会产生不必要的消耗，因此我们通过多线程执行框架来解决这两个问题，其一可以分离线程的定义和执行过程，其二可以通过线程池来动态地管理线程以减小不必要的资源开销。

#  线程执行框架启动线程

将要多线程执行的任务封装为一个Runnable对象,将其传给一个执行框架Executor对象, Executor从线程池中选择线程执行工作任务。

创建多线程框架对象调用线程执行任务
我们通常通过Executors类的一些静态方法来实例化Executor或ThreadPoolExecutor对象：

比如Executor对象来执行：

```java
public class ThreadTest {
    public static void main(String[] args) {
        Executor executor = Executors.newSingleThreadExecutor();
        executor.execute(new MyRunnable());
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("running");
    }
}
```

比如线程池的Executor对象来执行：

```java
public class ThreadTest {
    public static void main(String[] args) {
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors
                .newFixedThreadPool(3);
        executor.execute(new MyRunnable());
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("running");
    }
}
```

* Executors. newSingleThreadExecutor()：一 个线程死掉后，自动重新创建后一个新的线程，所以没有线程池的概念，不能被ThreadPoolExecutor接收；
* Executors. newFixedThreadPool()：固定数目的线程池；
* Executors. newCachedThreadPool()：动态地增加和减少线程数；


# 多线程框架对象调用线程执行任务取回结果

实现了Runnable接口的执行类虽然可以在run()方法里写入执行体，但是无法返回结果值，因为run()方法是void型的，而Callable接口解决了这个问题，在继承了Callable接口的执行类中重写call()方法可以设置返回值，当Executor对象使用submit()函数提交执行类的时候会由线程池里的线程来运行，运行得到的返回值可以使用Future<V>接口来接，取得的返回值类型由V决定，Future<V>接口表示可能会得到的返回值，但是有可能报异常，因此要抛出这些异常，然后可以取得这些返回值。

1.invokeAny():

```java
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException, ExecutionException {
ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors.newFixedThreadPool(5);
List<MyCallable> callables = new ArrayList<>();
for(int i=0;i<10;i++){
MyCallable myCallable = new MyCallable(i);
callables.add(myCallable);
}
Integer res = executor.invokeAny(callables);
System.out.println(res);

}
}

class MyCallable implements Callable<Integer> {
    private int num;

    public MyCallable(int num) {
        this.num = num;
    }

    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + " is running");
        return num * 2;
    }
}
```

2.invokeAll():

```java
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException,
            ExecutionException {
        ThreadPoolExecutor executor = (ThreadPoolExecutor) Executors
                .newFixedThreadPool(5);
        List<MyCallable> callables = new ArrayList<MyCallable>();
        for (int i = 0; i < 10; i++) {
            MyCallable myCallable = new MyCallable(i);
            callables.add(myCallable);
        }
        List<Future<Integer>> res = executor.invokeAll(callables);
        for (Future<Integer> future : res) {
            System.out.println(future.get());
        }
    }
}

class MyCallable implements Callable<Integer> {
    private int num;

    public MyCallable(int num) {
        this.num = num;
    }

    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + " is running");
        return num * 2;
    }
}
```

# 多线程框架对象执行定时任务

使用Executor的schedule()函数族来调度线程池中的线程来执行callable执行类对象中的call()定时任务:

```java
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException,
            ExecutionException {
        ScheduledExecutorService executorService = Executors
                .newScheduledThreadPool(2);
        MyCallable callable = new MyCallable(2);
        executorService.schedule(callable, 10, TimeUnit.SECONDS);
        executorService.shutdown();
    }
}

class MyCallable implements Callable<Integer> {
    private int num;

    public MyCallable(int num) {
        this.num = num;
    }

    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + " is running");
        return num * 2;
    }
}
```