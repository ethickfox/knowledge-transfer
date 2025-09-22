# Task Execution
## Executing tasks in threads
- Executing tasks sequentially - Single thread
- Explicitly creating threads for tasks - create a new thread for servicing each request
  - Disadvantages of unbounded thread creation
    - Thread lifecycle overhead - If requests are frequent and lightweight, as in most server applications, creating a new thread for each request can consume signiﬁcant computing resources.
    - Resource consumption. Active threads consume system resources, especially memory. When there are more runnable threads than available processors, threads sit idle.Having many idle threads can tie up a lot of memory, putting pressure on the garbage collector,
    - Stability. There is a limit on how many threads can be created. The limit varies by platform and is affected by factors including JVM invocation parameters, the requested stack size in the Thread constructor, and limits on threads placed by the underlying operating system.2 When you hit this limit, the most likely result is an OutOfMemoryError.
# The Executor framework
**Executor** - based on the producer-consumer pattern, where activities that submit tasks are the producers (producing units of work to be done) and the threads that execute tasks are the consumers (consuming those units of work). Using an Executor is usually the easiest path to implementing a producer-consumer design in your application.
Whenever you see code of the form: `new Thread(runnable).start()`
and you think you might at some point want a more flexible execution
policy, seriously consider replacing it with the use of an Executor.

**Thread pool** - manages a homogeneous pool of worker threads. A thread pool is tightly bound to a work queue holding tasks waiting to be executed. Worker threads have a simple life: request the next task from the work queue, execute it, and go back to waiting for another task. Reusing an existing thread instead of creating a new one amortizes thread creation and teardown costs over multiple requests.
- newFixedThreadPool  - A fixed-size thread pool creates threads as tasks are submitted, up to the maximum pool size, and then attempts to keep the pool size constant (adding new threads if a thread dies due to an unexpected Exception).
- newCachedThreadPool - A cached thread pool has more flexibility to reap idle threads when the current size of the pool exceeds the demand for processing, and to add new threads when demand increases, but places no bounds on the size of the pool. 
- newSingleThreadExecutor - A single-threaded executor creates a single worker thread to process tasks, replacing it if it dies unexpectedly. Tasks are guaranteed to be processed sequentially according to the order imposed by the task queue (FIFO, LIFO, priority order).
- newScheduledThreadPool - A fixed-size thread pool that supports delayed and periodic task execution, similar to Timer.

An Executor implementation is likely to create threads for processing tasks. But the JVM can’t exit until all the (nondaemon) threads have terminated, so failing to shut down an Executor could prevent the JVM from exiting.
The lifecycle implied by ExecutorService has three states—running, shutting down,and terminated. ExecutorServices are initially created in the running state. 
- The shutdown method initiates a graceful shutdown: no new tasks are accepted but previously submitted tasks are allowed to complete—including those that have not yet begun execution. 
- The shutdownNow method initiates an abrupt shutdown: it attempts to cancel outstanding tasks and does not start any tasks that are queued but not begun.
RejectedExecutionException. Once all tasks have completed, the ExecutorService transitions to the terminated state.

Scheduled thread pools  letting you provide multiple threads for executing deferred and periodic tasks.

# Finding exploitable parallelism
``` java
Executionpublic interface Callable<V> {
  V call() throws Exception;
}

public interface Future<V> {
  boolean cancel(boolean mayInterruptIfRunning); boolean isCancelled();
  boolean isDone();
  V get() throws InterruptedException, ExecutionException, CancellationException;
  V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, CancellationException, TimeoutException;
}
```
CompletionService combines the functionality of an Executor and a BlockingQueue.You can submit Callable tasks to it for execution and use the queuelike methods take and poll to retrieve completed results,
- The primary challenge in executing tasks within a time budget is making sure that you don’t wait longer than the time budget to get an answer or find out that one is not forthcoming. The timed version of Future.get supports this requirement: it returns as soon as the result is ready, but throws TimeoutException if the result is not ready within the timeout period.
- A secondary problem when using timed tasks is to stop them when they run out of time, so they do not waste computing resources by continuing to compute a result that will not be used.


