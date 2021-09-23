# Thread status

Defined by java.lang.Thread.State: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED.

# Create new thread

To create a thread:

``` java
new Thread(new Runnable(){
  @Override
  public void run(){
    ...
  }
})
```

The performance of creation is low and the creation is unlimited. So, thread pool is recommended because of:

- better performance of creation
- control of concorrent threads
- have more functions like timer and queue

# Create thread pool

##  via executors factory

``` java
// unlimited
ExecutorService executorService = Executors.newCachedThreadPool();
// ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>())
// fixed
ExecutorService executorService = Executors.newFixedThreadPool(3);
// ThreadPoolExecutor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>())
// single
ExecutorService executorService = Executors.newSingleThreadExecutor();
// ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>())
// usage
executorService.execute(()->{});
```

Because of the length of workQueue or maximunPoolSize is Integer.MAX_VALUE, can cause OOM, so that it's not recommended to create in this way.

## via ThreadPoolExecutor

``` java
// defination
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler) {}
```

- corePoolSize: number of active threads
- maximumPoolSize: number of allowed threads
- keepAliveTime: time for free non-core threads to keep alive
- workQueue: job queues, often use LinkedBlockingQueue and Synchronous
- handler: triggered when submit more than `maximumPoolSize + workQueue size`; default AbortPolicy; have CallerRunsPolicy, DiscardOldestPolicy and DiscardPolicy

