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

# Performance versus scalability 
Scalability describes the ability to improve throughput or capacity when  additional computing resources (such as additional CPUs, memory, storage, or I/O bandwidth) are added. 

Two aspects of performance—how fast and how much—are completely  separate, and sometimes even at odds with each other. In order to achieve higher  scalability or better hardware utilization, we often end up increasing the amount  of work done to process each individual task, such as when we divide tasks into  multiple “pipelined” subtasks.

Avoid premature optimization. First make it right, then make it fast—if it  is not already fast enough.

Most performance decisions involve multiple variables and are highly situational. Before deciding that one approach is “faster” than another, ask yourself  some questions: 
- What do you mean by “faster”?
- Under what conditions will this approach actually be faster?
	- Under light or  heavy load? With large or small data sets?
 	- Can you support your answer  with measurements?
 - How often are these conditions likely to arise in your situation?
	 - Can you  support your answer with measurements?
- Is this code likely to be used in other situations where the conditions may  be different?
- What hidden costs, such as increased development or maintenance risk, are  you trading for this improved performance? Is this a good tradeoff?

All concurrent applications have some sources of serialization; if you think  yours does not, look again. 

## Context switching 
hand, if there are more runnable threads than CPUs,  eventually the OS will preempt one thread so that another can use the CPU. This  causes a context switch, which requires saving the execution context of the currently running thread and restoring the execution context of the newly scheduled  thread. 

When a new thread is switched in, the data it needs is unlikely to be in the local  processor cache, so a context switch causes a flurry of cache misses, and thus  threads run a little more slowly when they are first scheduled. 

When a thread blocks because it is waiting for a contended lock, the JVM  usually suspends the thread and allows it to be switched out. If threads block  frequently, they will be unable to use their full scheduling quantum. A program  that does more blocking (blocking I/O, waiting for contended locks, or waiting  on condition variables) incurs more context switches than one that is CPU-bound,  increasing scheduling overhead and reducing throughput.

The actual cost of context switching varies across platforms, but a good rule of  thumb is that a context switch costs the equivalent of 5,000 to 10,000 clock cycles,  or several microseconds on most current processors. 

The vmstat command on Unix systems and the perfmon tool on Windows  systems report the number of context switches and the percentage of time spent  in the kernel. High kernel usage (over 10%) often indicates heavy scheduling  activity, which may be caused by blocking due to I/O or lock contention. 

The performance cost of synchronization comes from several sources. The visibility guarantees provided by synchronized and volatile may entail using special  instructions called memory barriers that can flush or invalidate caches, flush hardware write buffers, and stall execution pipelines. Memory barriers may also have  indirect performance consequences because they inhibit other compiler optimizations; most operations cannot be reordered with memory barriers. 

When assessing the performance impact of synchronization, it is important  to distinguish between contended and uncontended synchronization. The synchronized mechanism is optimized for the uncontended case (volatile is always  uncontended), and at this writing, the performance cost of a “fast-path” uncontended synchronization ranges from 20 to 250 clock cycles for most systems.  While this is certainly not zero, the effect of needed, uncontended synchronization is rarely significant in overall application performance
Modern JVMs can reduce the cost of incidental synchronization by optimizing  away locking that can be proven never to contend. If a lock object is accessible  only to the current thread, the JVM is permitted to optimize away a lock acquisition because there is no way another thread could synchronize on the same lock. 
More sophisticated JVMs can use escape analysis to identify when a local object  reference is never published to the heap and is therefore thread-local.
Even without escape analysis, compilers can also perform lock coarsening, the  merging of adjacent synchronized blocks using the same lock.
Don’t worry excessively about the cost of uncontended synchronization.  The basic mechanism is already quite fast, and JVMs can perform additional optimizations that further reduce or eliminate the cost. Instead,  focus optimization efforts on areas where lock contention actually occurs. 

Synchronization by one thread can also affect the performance of other  threads. Synchronization creates traffic on the shared memory bus; this bus  has a limited bandwidth and is shared across all processors. If threads must  compete for synchronization bandwidth, all threads using synchronization will  suffer.

## Blocking 
When locking is contended, the losing thread(s) must block. The JVM  can implement blocking either via spin-waiting (repeatedly trying to acquire the  lock until it succeeds) or by suspending the blocked thread through the operating  system.

# Reducing lock contention 
The principal threat to scalability in concurrent applications is the exclusive resource lock. 
Two factors influence the likelihood of contention for a lock: how often that  lock is requested and how long it is held once acquired. If the product of these  factors is sufficiently small, then most attempts to acquire the lock will be uncontended, and lock contention will not pose a significant scalability impediment. If,  however, the lock is in sufficiently high demand, threads will block waiting for it;  in the extreme case, processors will sit idle even though there is plenty of work  to do. 

There are three ways to reduce lock contention:  
• Reduce the duration for which locks are held
This can be done by moving code that doesn’t require the lock out of  synchronized blocks, especially for expensive operations and potentially blocking operations such as I/O. 
• Reduce the frequency with which locks are requested; or 
• Replace exclusive locks with coordination mechanisms that permit  greater concurrency. 

Because AttributeStore has only one state variable, attributes, we can improve it further by the technique of delegating thread safety (Section 4.3). By replacing attributes with a thread-safe Map (a Hashtable, synchronizedMap, or ConcurrentHashMap), AttributeStore can delegate all its thread safety obligations  to the underlying thread-safe collection.

because the cost of synchronization is nonzero, breaking one  synchronized block into multiple synchronized blocks (correctness permitting)  at some point becomes counterproductive in terms of performance.9 The ideal  balance is of course platform-dependent, but in practice it makes sense to worry  about the size of a synchronized block only when you can move “substantial”  computation or blocking operations out of it. 

## Reducing lock granularity 
This can be accomplished by lock splitting and lock striping, which involve using  separate locks to guard multiple independent state variables previously guarded  by a single lock. These techniques reduce the granularity at which locking occurs,  potentially allowing greater scalability—but using more locks also increases the  risk of deadlock. 

If a lock guards more than one independent state variable, you may be able  to improve scalability by splitting it into multiple locks that each guard different  variables. This results in each lock being requested less often. 

Splitting a lock into two offers the greatest possibility for improvement when  the lock is experiencing moderate but not heavy contention. Splitting locks that  are experiencing little contention yields little net improvement in performance or  throughput, although it might increase the load threshold at which performance  starts to degrade due to contention.

## Lock striping 
Splitting a heavily contended lock into two is likely to result in two heavily contended locks. While this will produce a small scalability improvement by enabling  two threads to execute concurrently instead of one, it still does not dramatically  improve prospects for concurrency on a system with many processors.

Lock splitting can sometimes be extended to partition locking on a variablesized set of independent objects, in which case it is called lock striping. For example, the implementation of ConcurrentHashMap uses an array of 16 locks, each of  which guards 1/16 of the hash buckets;

One of the downsides of lock striping is that locking the collection for exclusive access is more difficult and costly than with a single lock. Usually an  operation can be performed by acquiring at most one lock, but occasionally you  need to lock the entire collection, as when ConcurrentHashMap needs to expand  the map and rehash the values into a larger set of buckets. This is typically done  by acquiring all of the locks in the stripe set.

Keeping a separate count to speed up operations like size and isEmpty works  fine for a single-threaded or fully synchronized implementation, but makes it  much harder to improve the scalability of the implementation because every operation that modifies the map must now update the shared counter. Even if you  use lock striping for the hash chains, synchronizing access to the counter reintroduces the scalability problems of exclusive locking. What looked like a performance optimization—caching the results of the size operation—has turned into  a scalability liability. In this case, the counter is called a hot field because every  mutative operation needs to access it.  ConcurrentHashMap avoids this problem by having size enumerate the stripes  and add up the number of elements in each stripe, instead of maintaining a global  count. To avoid enumerating every element, ConcurrentHashMap maintains a  separate count field for each stripe, also guarded by the stripe lock.

## Alternatives to exclusive locks 
A third technique for mitigating the effect of lock contention is to forego the use  of exclusive locks in favor of a more concurrency-friendly means of managing  shared state. These include using the concurrent collections, read-write locks,  immutable objects and atomic variables. 
- ReadWriteLock (see Chapter 13) enforces a multiple-reader, single-writer locking discipline: more than one reader can access the shared resource concurrently  so long as none of them wants to modify it, but writers must acquire the lock  excusively.
- Atomic variables (see Chapter 15) offer a means of reducing the cost of updating “hot fields” such as statistics counters, sequence generators, or the reference to the first node in a linked data structure.The atomic variable classes provide very fine-grained (and therefore more scalable) atomic operations on integers  or object references, and are implemented using low-level concurrency primitives  (such as compare-and-swap) provided by most modern processors
## Monitoring CPU utilization 
When testing for scalability, the goal is usually to keep the processors fully utilized. Tools like vmstat and mpstat on Unix systems or perfmon on Windows  systems can tell you just how “hot” the processors are running. 
Asymmetric utilization indicates that most of the computation is going on  in a small set of threads, and your application will not be able to take advantage  of additional processors. 
If the CPUs are not fully utilized, you need to figure out why. There are several  likely causes: 
- Insufficent load. It may be that the application being tested is just not subjected  to enough load. You can test for this by increasing the load and measuring  changes in utilization, response time, or service time.
- I/O-bound. You can determine whether an application is disk-bound using  iostat or perfmon, and whether it is bandwidth-limited by monitoring  traffic levels on your network.
- Externally bound. If your application depends on external services such as a  database or web service, the bottleneck may not be in your code.
- Lock contention. Profiling tools can tell you how much lock contention your application is experiencing and which locks are “hot”. You can often get the  same information without a profiler through random sampling, triggering a  few thread dumps and looking for threads contending for locks. If a thread  is blocked waiting for a lock, the appropriate stack frame in the thread dump  indicates “waiting to lock monitor ...” Locks that are mostly uncontended  rarely show up in a thread dump; a heavily contended lock will almost always have at least one thread waiting to acquire it and so will frequently  appear in thread dumps.

## Just say no to object pooling 
allocation in  Java is now faster than malloc is in C: the common code path for new Object in  HotSpot 1.4.x and 5.0 is approximately ten machine instructions. 
To work around “slow” object lifecycles, many developers turned to object  pooling, where objects are recycled instead of being garbage collected and allocated anew when needed. Even taking into account its reduced garbage collection  overhead, object pooling has been shown to be a performance loss14 for all but the  most expensive objects (and a serious loss for light- and medium-weight objects)  in single-threaded programs
In concurrent applications, pooling fares even worse. When threads allocate  new objects, very little inter-thread coordination is required, as allocators typically  use thread-local allocation blocks to eliminate most synchronization on heap data  structures. But if those threads instead request an object from a pool, some synchronization is necessary to coordinate access to the pool data structure, creating  the possibility that a thread will block. Because blocking a thread due to lock  contention is hundreds of times more expensive than an allocation, even a small  amount of pool-induced contention would be a scalability bottleneck.(Even an  uncontended synchronization is usually more expensive than allocating an object.)



