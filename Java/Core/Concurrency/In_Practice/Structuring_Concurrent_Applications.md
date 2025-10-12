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
# Cancellation and Shutdown
Java does not provide any mechanism for safely forcing a thread to stop what it is doing.1 Instead, it provides interruption, a cooperative mechanism that lets one thread ask another to stop what it is doing.
An activity is cancellable if external code can move it to completion before its normal completion.
One such cooperative mechanism is setting a “cancellation requested” flag that the task checks periodically; if it finds the flag set, the task terminates early.

A task that wants to be cancellable must have a cancellation policy that speciﬁes the “how”, “when”, and “what” of cancellation—how other code can request cancellation, when the task checks whether cancellation has been requested, and what actions the task takes in response to a cancellation request.

Just as tasks should have a cancellation policy, threads should have an interruption policy. An interruption policy determines how a thread interprets an interruption request—what it does (if anything) when one is detected, what units of work are considered atomic with respect to interruption, and how quickly it reacts to interruption.
Just as task code should not make assumptions about what interruption means to its executing thread, cancellation code should not make assumptions about the interruption policy of arbitrary threads. A thread should be interrupted only by its owner; the owner can encapsulate knowledge of the thread’s interruption policy in an appropriate cancellation mechanism such as a shutdown method.

ExecutorService.submit returns a Future describing the task. Future has a cancel method that takes a boolean argument, mayInterruptIfRunning,and returns a value indicating whether the cancellation attempt was successful. (This tells you only whether it was able to deliver the interruption, not whether the task detected and acted on it.)

# JVM shutdown
The JVM can shut down in either an orderly or abrupt manner. 
An orderly shutdown is initiated when
- the last “normal” (nondaemon) thread terminates
- someone calls System.exit
- by other platform-specific means (such as sending a SIGINT or hitting Ctrl-C).

While this is the standard and preferred way for the JVM to shut down, it can also be shut down abruptly by
- calling Runtime.halt
- by killing the JVM process through the operating system (such as sending a SIGKILL).
In an orderly shutdown, the JVM first starts all registered shutdown hooks. Shutdown hooks are unstarted threads that are registered with Runtime.addShutdownHook. The JVM makes no guarantees on the order in which shutdown hooks are started.
``` java
Runtime.getRuntime().addShutdownHook(new Thread() {
	public void run() {
		try {
			LogService.this.stop();
		} catch (InterruptedException ignored) {
		}
	}
});
```

Sometimes you want to create a thread that performs some helper function but you don’t want the existence of this thread to prevent the JVM from shutting down. This is what daemon threads are for.
shutdown. When the JVM halts, any remaining daemon threads are abandoned— finally blocks are not executed, stacks are not unwound—the JVM just exits.

# Applying Thread Pools 

Tasks that exploit thread confinement. Single-threaded executors make stronger  promises about concurrency than do arbitrary thread pools. They guarantee that tasks are not executed concurrently, which allows you to relax the  thread safety of task code. Objects can be confined to the task thread, thus  enabling tasks designed to run in that thread to access those objects without  synchronization, even if those resources are not thread-safe. This forms an  implicit coupling between the task and the execution policy.

ThreadLocal allows each thread to have its own private “version” of a variable. However, executors are free to reuse threads as  they see fit. The standard Executor implementations may reap idle threads  when demand is low and add new ones when demand is high, and also replace a worker thread with a fresh one if an unchecked exception is thrown  from a task. ThreadLocal makes sense to use in pool threads only if the  thread-local value has a lifetime that is bounded by that of a task; ThreadLocal should not be used in pool threads to communicate values between  tasks. 

Thread pools work best when tasks are homogeneous and independent. Mixing long-running and short-running tasks risks “clogging” the pool unless it is  very large; submitting tasks that depend on other tasks risks deadlock unless  the pool is unbounded.

## Thread starvation deadlock 
If tasks that depend on other tasks execute in a thread pool, they can deadlock. In  a single-threaded executor, a task that submits another task to the same executor  and waits for its result will always deadlock.

Threads are executing tasks that are blocked waiting for other  tasks still on the work queue. This is called thread starvation deadlock, and can occur  whenever a pool task initiates an unbounded blocking wait for some resource or  condition that can succeed only through the action of another pool task,

Whenever you submit to an Executor tasks that are not independent, be  aware of the possibility of thread starvation deadlock, and document any  pool sizing or configuration constraints in the code or configuration file  where the Executor is configured. 

## Sizing thread pools 
The ideal size for a thread pool depends on the types of tasks that will be submitted and the characteristics of the deployment system. Thread pool sizes should  rarely be hard-coded; instead pool sizes should be provided by a configuration  mechanism or computed dynamically by consulting Runtime.availableProcessors. 
For compute-intensive tasks, an Ncpu-processor system usually achieves optimum utilization with a thread pool of Ncpu + 1 threads. (Even compute-intensive  threads occasionally take a page fault or pause for some other reason, so an “extra” runnable thread prevents CPU cycles from going unused when this happens.) 

For tasks that also include I/O or other blocking operations, you want a larger  pool, since not all of the threads will be schedulable at all times. In order to size  the pool properly, you must estimate the ratio of waiting time to compute time  for your tasks; this estimate need not be precise and can be obtained through pro-  filing or instrumentation.

# ThreadPoolExecutor 
ThreadPoolExecutor allows you to supply a BlockingQueue to hold tasks  awaiting execution. There are three basic approaches to task queueing: unbounded queue, bounded queue, and synchronous handoff. The choice of queue  interacts with other configuration parameters such as pool size. 

The default for newFixedThreadPool and newSingleThreadExecutor is to use  an unbounded LinkedBlockingQueue. Tasks will queue up if all worker threads  are busy, but the queue could grow without bound if the tasks keep arriving faster  than they can be executed. 

A more stable resource management strategy is to use a bounded queue, such  as an ArrayBlockingQueue or a bounded LinkedBlockingQueue or PriorityBlockingQueue. Bounded queues help prevent resource exhaustion but introduce  the question of what to do with new tasks when the queue is full.

For very large or unbounded pools, you can also bypass queueing entirely and  instead hand off tasks directly from producers to worker threads using a SynchronousQueue. A SynchronousQueue is not really a queue at all, but a mechanism  for managing handoffs between threads. In order to put an element on a SynchronousQueue, another thread must already be waiting to accept the handoff. If  no thread is waiting but the current pool size is less than the maximum, ThreadPoolExecutor creates a new thread; otherwise the task is rejected according to  the saturation policy.

The newCachedThreadPool factory uses a SynchronousQueue. 
The newCachedThreadPool factory is a good default choice for an Executor, providing better queuing performance than a fixed thread pool.5  A fixed size thread pool is a good choice when you need to limit the  number of concurrent tasks for resource-management purposes, as in a  server application that accepts requests from network clients and would  otherwise be vulnerable to overload. 

## Saturation policies 
The  saturation policy for a ThreadPoolExecutor can be modified by calling setRejectedExecutionHandler. (The saturation policy is also used when a task is  submitted to an Executor that has been shut down.) Several implementations of  RejectedExecutionHandler are provided, each implementing a different saturation policy: AbortPolicy, CallerRunsPolicy, DiscardPolicy, and DiscardOldestPolicy.  The default
The caller-runs policy implements a form of throttling that neither discards  tasks nor throws an exception, but instead tries to slow down the flow of new  tasks by pushing some of the work back to the caller. It executes the newly  submitted task not in a pool thread, but in the thread that calls execute.

ThreadPoolExecutor was designed for extension, providing several “hooks” for  subclasses to override—beforeExecute, afterExecute, and terminated—that  can be used to extend the behavior of ThreadPoolExecutor. 

Sequential loop iterations are suitable for parallelization when each iteration is independent of the others and the work done in each iteration of  the loop body is significant enough to offset the cost of managing a new  task. 

Callers of parallelRecursive can wait for all the results by creating an Executor specific to the  traversal and using shutdown and awaitTermination,

``` java
	public<T> Collection<T> getParallelResults(List<Node<T>> nodes)  throws InterruptedException {
	ExecutorService exec = Executors.newCachedThreadPool();
	Queue<T> resultQueue = new ConcurrentLinkedQueue<T>();

	parallelRecursive(exec, nodes, resultQueue);  exec.shutdown();
	exec.awaitTermination(Long.MAX_VALUE, TimeUnit.SECONDS);

	return resultQueue;
} 
```

# Avoiding Liveness Hazards
indiscriminate use of locking can cause lock-ordering deadlocks.  Similarly, we use thread pools and semaphores to bound resource consumption,  but failure to understand the activities being bounded can cause resource deadlocks. 
## Dynamic lock order deadlocks 
It may appear as if all the threads acquire  their locks in the same order, but in fact the lock order depends on the order of  arguments passed to transferMoney, and these in turn might depend on external  inputs. Deadlock can occur if two threads call transferMoney at the same time, one transferring from X to Y, and the other doing the opposite:
```
A: transferMoney(myAccount, yourAccount, 10);  
B: transferMoney(yourAccount, myAccount, 20); 
```

``` java
public void transferMoney(Account fromAccount,
	Account toAccount,
	DollarAmount amount)  throws InsufficientFundsException {  
	synchronized (fromAccount) {
	    synchronized (toAccount) {  
		    if (fromAccount.getBalance().compareTo(amount) < 0) {
		      throw new InsufficientFundsException();  
		    } else {  
			    fromAccount.debit(amount);
			    toAccount.credit(amount);
			}
		}
	 }
}
```
Deadlocks like this one can be spotted — look  for nested lock acquisitions. Since the order of arguments is out of our control,  to fix the problem we must induce an ordering on the locks and acquire them  according to the induced ordering consistently throughout the application. 
If Account has a unique, immutable, comparable key such as an account number, inducing a lock ordering is even easier: order objects by their key and lock.
## Deadlocks between cooperating objects 
Invoking an alien method with a lock held is asking for liveness trouble.  The alien method might acquire other locks (risking deadlock) or block  for an unexpectedly long time, stalling other threads that need the lock  you hold. 
```java
class Taxi { 
	public synchronized void setLocation(Point location) {  
		this.location = location;  
		if (location.equals(destination))  
			dispatcher.notifyAvailable(this);  
		} 
	}
}

class Dispatcher { 
	public synchronized void notifyAvailable(Taxi taxi) {  
		availableTaxis.add(taxi);  
	} 
}
```
Because you don’t know what is happening on the other side of the call,  calling an alien method with a lock held is difficult to analyze and therefore risky. 
Calling a method with no locks held is called an open call, and  classes that rely on open calls are more well-behaved and composable than classes  that make calls with locks held.
## Deadlock analysis with thread dumps 
A thread dump includes a stack  trace for each running thread, similar to the stack trace that accompanies an exception. 
- Thread dumps also include locking information, such as which locks are  held by each thread, in which stack frame they were acquired, and which lock a  blocked thread is waiting to acquire.
- Before generating a thread dump, the JVM  searches the is-waiting-for graph for cycles to find deadlocks.
- If it finds one, it  includes deadlock information identifying which locks and threads are involved,  and where in the program the offending lock acquisitions are. 
## Starvation 
Starvation occurs when a thread is perpetually denied access to resources it needs  in order to make progress; the most commonly starved resource is CPU cycles.  Starvation in Java applications can be caused by inappropriate use of thread priorities. It can also be caused by executing nonterminating constructs (infinite loops  or resource waits that do not terminate) with a lock held, since other threads that  need that lock will never be able to acquire it. 
It is generally wise to resist the temptation to tweak thread priorities. As  soon as you start modifying priorities, the behavior of your application becomes  platform-specific and you introduce the risk of starvation. You can often spot a  program that is trying to recover from priority tweaking or other responsiveness  problems by the presence of Thread.sleep or Thread.yield calls in odd places,  in an attempt to give more time to lower-priority threads.
## Livelock 
Livelock is a form of liveness failure in which a thread, while not blocked, still  cannot make progress because it keeps retrying an operation that will always  fail. Livelock often occurs in transactional messaging applications, where the  messaging infrastructure rolls back a transaction if a message cannot be processed  successfully, and puts it back at the head of the queue.