# Explicit Locks 
Unlike intrinsic locking, Lock offers a choice of unconditional,  polled, timed, and interruptible lock acquisition, and all lock and unlock operations are explicit. Lock implementations must provide the same memory-visibility  semantics as intrinsic locks, but can differ in their locking semantics, scheduling  algorithms, ordering guarantees, and performance characteristics.

ReentrantLock implements Lock, providing the same mutual exclusion and  memory-visibility guarantees as synchronized. Acquiring a ReentrantLock has  the same memory semantics as entering a synchronized block, and releasing a  ReentrantLock has the same memory semantics as exiting a synchronized block. 
## Polled and timed lock acquisition 
The timed and polled lock-acqusition modes provided by tryLock allow more  sophisticated error recovery than unconditional acquisition. With intrinsic locks,  a deadlock is fatal—the only way to recover is to restart the application, and the  only defense is to construct your program so that inconsistent lock ordering is impossible. Timed and polled locking offer another option: probabalistic deadlock  avoidance. 

Using timed or polled lock acquisition (tryLock) lets you regain control if you  cannot acquire all the required locks, release the ones you did acquire, and try  again (or at least log the failure and do something else).

The lockInterruptibly method allows you to try to acquire a lock while  remaining responsive to interruption, and its inclusion in Lock avoids creating  another category of non-interruptible blocking mechanisms. 

# Fairness 
The ReentrantLock constructor offers a choice of two fairness options: create a  nonfair lock (the default) or a fair lock. Threads acquire a fair lock in the order in  which they requested it, whereas a nonfair lock permits barging: threads requesting a lock can jump ahead of the queue of waiting threads if the lock happens  to be available when it is requested.
Some algorithms rely on fair queueing to ensure their correctness, but these are unusual. In most cases, the performance benefits of  nonfair locks outweigh the benefits of fair queueing. 
Don’t pay for fairness if you don’t need it. 

Fair locks tend to work best when they are held for a relatively long time or  when the mean time between lock requests is relatively long. In these cases, the  condition under which barging provides a throughput advantage—when the lock  is unheld but a thread is currently waking up to claim it—is less likely to hold. 
Like the default ReentrantLock, intrinsic locking offers no deterministic fairness guarantees, but the statistical fairness guarantees of most locking implementations are good enough for almost all situations.

# Choosing between synchronized and ReentrantLock 
Intrinsic locks still have significant advantages over explicit locks. The notation is familiar and compact, and many existing programs already use intrinsic  locking—and mixing the two could be confusing and error-prone. ReentrantLock is definitely a more dangerous tool than synchronization; if you forget to  wrap the unlock call in a finally block, your code will probably appear to run  properly, but you’ve created a time bomb that may well hurt innocent bystanders. Save ReentrantLock for situations in which you need something ReentrantLock  provides that intrinsic locking doesn’t. 
ReentrantLock is an advanced tool for situations where intrinsic locking  is not practical. Use it if you need its advanced features: timed, polled,  or interruptible lock acquisition, fair queueing, or non-block-structured  locking. Otherwise, prefer synchronized. 

ReadWriteLock, exposes two Lock objects—one for  reading and one for writing. To read data guarded by a ReadWriteLock you  must first acquire the read lock, and to modify data guarded by a ReadWriteLock  you must first acquire the write lock. While there may appear to be two separate  locks, the read lock and write lock are simply different views of an integrated  read-write lock object. 
In practice, read-write locks can improve performance for frequently accessed read-mostly data structures on multiprocessor  systems; under other conditions they perform slightly worse than exclusive locks due to their greater complexity.

ReentrantReadWriteLock provides reentrant locking semantics for both locks.  Like ReentrantLock, a ReentrantReadWriteLock can be constructed as nonfair  (the default) or fair. With a fair lock, preference is given to the thread that has  been waiting the longest; if the lock is held by readers and a thread requests the  write lock, no more readers are allowed to acquire the read lock until the writer  has been serviced and releases the write lock. With a nonfair lock, the order in  which threads are granted access is unspecified. Downgrading from writer to  reader is permitted; upgrading from reader to writer is not (attempting to do so  results in deadlock). 

ReadWriteMap uses a ReentrantReadWriteLock to wrap a Map so  that it can be shared safely by multiple readers and still prevent reader-writer or  writer-writer conflicts.7 In reality, ConcurrentHashMap’s performance is so good  that you would probably use it rather than this approach if all you needed was  a concurrent hash-based map, but this technique would be useful if you want  to provide more concurrent access to an alternate Map implementation such as  LinkedHashMap. 

# Building Custom Synchronizers 
State-dependent operations that block until the operation can proceed are more  convenient and less error-prone than those that simply fail. The built-in condition  queue mechanism enables threads to block until an object has entered a state that  allows progress and to wake blocked threads when they may be able to make  further progress.

## Condition queues
A condition queue gets its name because it gives a group of threads—called the  wait set—a way to wait for a specific condition to become true. Unlike typical  queues in which the elements are data items, the elements of a condition queue  are the threads waiting for the condition. 

Just as each Java object can act as a lock, each object can also act as a condition  queue, and the wait, notify, and notifyAll methods in Object constitute the  API for intrinsic condition queues.

``` java
@ThreadSafe 
public class BoundedBuffer<V> extends BaseBoundedBuffer<V> {
  // CONDITION PREDICATE: not-full (!isFull())
  // CONDITION PREDICATE: not-empty (!isEmpty())

  public BoundedBuffer(int size) {super(size);}

  // BLOCKS-UNTIL: not-full
  public synchronized void put(V v) throws InterruptedException {
    while (isFull())
      wait();
    doPut(v);
    notifyAll();
  }
// BLOCKS-UNTIL: not-empty
  public synchronized V take() throws InterruptedException {
    while (isEmpty())
      wait();
    V v = doTake();
    notifyAll();
    return v;
  }
} 
```
This is simpler than the sleeping version, and is both more efficient  (waking up less frequently if the buffer state does not change) and more responsive (waking up promptly when an interesting state change happens). This is a big  improvement, but note that the introduction of condition queues didn’t change  the semantics compared to the sleeping version. It is simply an optimization  in several dimensions: CPU efficiency, context-switch overhead, and responsiveness.

A production version should also include timed versions of put and take, so that blocking operations can time out if they cannot  complete within a time budget. The timed version of Object.wait makes this  easy to implement. 

The key to using condition queues correctly is identifying the condition predicates  that the object may wait for. It is the condition predicate that causes much of the  confusion surrounding wait and notify, because it has no instantiation in the  API and nothing in either the language specification or the JVM implementation  ensures its correct use.
The condition predicate is the precondition that makes an operation state-dependent  in the first place. In a bounded buffer, take can proceed only if the buffer is not  empty; otherwise it must wait. For take, the condition predicate is “the buffer is  not empty”,

There is an important three-way relationship in a condition wait involving  locking, the wait method, and a condition predicate. The condition predicate  involves state variables, and the state variables are guarded by a lock, so before  testing the condition predicate, we must hold that lock. The lock object and the  condition queue object (the object on which wait and notify are invoked) must  also be the same object. 

A single intrinsic condition queue may be used with more than one condition predicate. When your thread is awakened because someone called notifyAll, that doesn’t  mean that the condition predicate you were waiting for is now true. (This is like  having your toaster and coffee maker share a single bell; when it rings, you still  have to look to see which device raised the signal.)7 Additionally, wait is even  allowed to return “spuriously”—not in response to any thread calling notify. Maybe. It  might have been true at the time the notifying thread called notifyAll, but could  have become false again by the time you reacquire the lock. Other threads may  have acquired the lock and changed the object’s state between when your thread  was awakened and when wait reacquired the lock.

For all these reasons, when you wake up from wait you must test the condition  predicate again, and go back to waiting (or fail) if it is not yet true. Since you  can wake up repeatedly without your condition predicate being true, you must  therefore always call wait from within a loop, testing the condition predicate in  each iteration. The
``` java
void stateDependentMethod() throws InterruptedException {
  // condition predicate must be guarded by lock
  synchronized(lock) {
    while (!conditionPredicate())
      lock.wait();  // object is now in desired state
  }
} 
```
When using condition waits (Object.wait or Condition.await):  
- Always have a condition predicate—some test of object state that  must hold before proceeding;
- Always test the condition predicate before calling wait, and again  after returning from wait;
- Always call wait in a loop;
- Ensure that the state variables making up the condition predicate are  guarded by the lock associated with the condition queue;
- Hold the lock associated with the the condition queue when calling  wait, notify, or notifyAll; and
- Do not release the lock after checking the condition predicate but  before acting on it.

Another  form of liveness failure is missed signals. A missed signal occurs when a thread  must wait for a specific condition that is already true, but fails to check the condition predicate before waiting. Now the thread is waiting to be notified of an event  that has already occurred.
Missed signals are the result of coding errors like those  warned against in the list above, such as failing to test the condition predicate  before calling wait.

In order for take to unblock when the buffer becomes nonempty,  we must ensure that every code path in which the buffer could become nonempty  performs a notification.

There are two notification methods in the condition queue API—notify and  notifyAll. To call either, you must hold the lock associated with the condition  queue object. Calling notify causes the JVM to select one thread waiting on that  condition queue to wake up; calling notifyAll wakes up all the threads waiting  on that condition queue. Because you must hold the lock on the condition queue  object when calling notify or notifyAll, and waiting threads cannot return from  wait without reacquiring the lock, the notifying thread should release the lock  quickly to ensure that the waiting threads are unblocked as soon as possible. 

using notify instead of notifyAll can be dangerous, primarily because single notification is prone to a problem akin to missed  signals.  notifyAll should be preferred to single notify in most cases
Single notify can be used instead of notifyAll only when both of the  following conditions hold:  
- Uniform waiters. Only one condition predicate is associated with the  condition queue, and each thread executes the same logic upon returning from wait; 
- One-in, one-out. A notification on the condition variable enables at most  one thread to proceed. 

The notification done by put and take in BoundedBuffer is conservative: a  notification is performed every time an object is put into or removed from the  buffer. This could be optimized by observing that a thread can be released from a  wait only if the buffer goes from empty to not empty or from full to not full, and  notifying only if a put or take effected one of these state transitions. This is called  conditional notification. While conditional notification can improve performance, it  is tricky to get right (and also complicates the implementation of subclasses) and  so should be used carefully.

``` java
public synchronized void put(V v) throws InterruptedException {
  while (isFull())
    wait();
  boolean wasEmpty = isEmpty();
  doPut(v);
  if (wasEmpty)
    notifyAll();
} 
```

The condition predicate used by await is more complicated than simply testing isOpen. This is needed because if N threads are waiting at the gate at the time  it is opened, they should all be allowed to proceed. But, if the gate is opened and  closed in rapid succession, all threads might not be released if await examines  only isOpen: by the time all the threads receive the notification, reacquire the  lock, and emerge from wait, the gate may have closed again. So ThreadGate uses  a somewhat more complicated condition predicate: every time the gate is closed,  a “generation” counter is incremented, and a thread may pass await if the gate is  open now or if the gate has opened since this thread arrived at the gate. 
``` java
public class ThreadGate {
// CONDITION-PREDICATE: opened-since(n) (isOpen || generation>n)  
@GuardedBy("this")
private boolean isOpen;  
@GuardedBy("this") private int generation;  

public synchronized void close() {
  isOpen = false;  
}  

public synchronized void open() {
  ++generation;
  isOpen = true;
  notifyAll();
}
// BLOCKS-UNTIL: opened-since(generation on entry)  
public synchronized void await() throws InterruptedException {  
	int arrivalGeneration = generation;
	while (!isOpen && arrivalGeneration == generation)  
		wait();  
	}  
}ß
```
A state-dependent class should either fully expose (and document) its waiting and notification protocols to subclasses, or prevent subclasses from participating in them at all. 

One option for doing this is to effectively prohibit subclassing, either by making the class final or by hiding the condition queues, locks, and state variables  from subclasses.
Otherwise, if the subclass does something to undermine the  way the base class uses notify, it needs to be able to repair the damage.

It is generally best to encapsulate the condition queue so that it is not accessible outside the class hierarchy in which it is used. Otherwise, callers might be  tempted to think they understand your protocols for waiting and notification and  use them in a manner inconsistent with your design.
Unfortunately, this advice—to encapsulate objects used as condition queues—  is not consistent with the most common design pattern for thread-safe classes, in  which an object’s intrinsic lock is used to guard its state

Wellings (Wellings, 2004) characterizes the proper use of wait and notify in terms  of entry and exit protocols. For each state-dependent operation and for each operation that modifies state on which another operation has a state dependency,  you should define and document an entry and exit protocol. The entry protocol  is the operation’s condition predicate; the exit protocol involves examining any  state variables that have been changed by the operation to see if they might have  caused some other condition predicate to become true, and if so, notifying on the  associated condition queue. 

## Explicit condition objects 
A Condition is associated with a single Lock, just as a condition queue is  associated with a single intrinsic lock; to create a Condition, call Lock.newCondition on the associated lock. And just as Lock offers a richer feature set than  intrinsic locking, Condition offers a richer feature set than intrinsic condition  queues: multiple wait sets per lock, interruptible and uninterruptible condition  waits, deadline-based waiting, and a choice of fair or nonfair queueing. 
Unlike intrinsic condition queues, you can have as many Condition objects per  Lock as you want.

Hazard warning: The equivalents of wait, notify, and notifyAll for  Condition objects are await, signal, and signalAll. However, Condition extends Object, which means that it also has wait and notify methods. Be sure to use the proper versions—await and signal—  instead! 
``` java
@ThreadSafe  
public class ConditionBoundedBuffer<T> {  
	protected final Lock lock = new ReentrantLock();  // CONDITION PREDICATE: notFull (count < items.length)  
	private final Condition notFull = lock.newCondition();  // CONDITION PREDICATE: notEmpty (count > 0) 
	 private final Condition notEmpty = lock.newCondition();  
	@GuardedBy("lock")  private final T[] items = (T[]) new Object[BUFFER_SIZE];  
	@GuardedBy("lock") private int tail, head, count;  // BLOCKS-UNTIL: notFull 
	public void put(T x) throws InterruptedException {  
		lock.lock(); 
		try {  
			while (count == items.length)  
				notFull.await();  
			items[tail] = x;  
			
			if (++tail == items.length)  
				tail = 0;  
			++count;  
			notEmpty.signal();  
		} finally {  
			lock.unlock();  
		} 
	}  // BLOCKS-UNTIL: notEmpty  
	public T take() throws InterruptedException {  
		lock.lock();  
		try { 
			while (count == 0)  
				notEmpty.await();  
			T x = items[head];  
			items[head] = null;  
			
			if (++head == items.length)  
				head = 0;  
			--count;  
			notFull.signal();  
			return x;  
		} finally {  
			lock.unlock();  
		}  
	}  
} 
```
As with built-in locks and condition queues, the three-way relationship  among the lock, the condition predicate, and the condition variable must also  hold when using explicit Locks and Conditions. The variables involved in the  condition predicate must be guarded by the Lock, and the Lock must be held  when testing the condition predicate and when calling await and signal.
Choose between using explicit Conditions and intrinsic condition queues in  the same way as you would choose between ReentrantLock and synchronized:  use Condition if you need its advanced features such as fair queueing or multiple  wait sets per lock, and otherwise prefer intrinsic condition queues.

## Anatomy of a synchronizer 
implemented using a common base class, AbstractQueuedSynchronizer (AQS)—as are many other synchronizers. AQS is a framework for building locks and synchronizers, and a surprisingly broad range of  synchronizers can be built easily and efficiently using it. Not only are ReentrantLock and Semaphore built using AQS, but so are CountDownLatch, ReentrantReadWriteLock, SynchronousQueue, 12 and FutureTask. 
## AbstractQueuedSynchronizer 
The basic operations that an AQS-based synchronizer performs are some  variants of acquire and release. Acquisition is the state-dependent operation  and can always block. With a lock or semaphore, the meaning of acquire is  straightforward—acquire the lock or a permit—and the caller may have to wait  until the synchronizer is in a state where that can happen.

AQS takes on the  task of managing some of the state for the synchronizer class: it manages a single integer of state information that can be manipulated through the protected  getState, setState, and compareAndSetState methods. This can be used to represent arbitrary state; for example, ReentrantLock uses it to represent the count  of times the owning thread has acquired the lock, Semaphore uses it to represent  the number of permits remaining, and FutureTask uses it to represent the state  of the task (not yet started, running, completed, cancelled).

``` java
@ThreadSafe  
public class OneShotLatch {
  private final Sync sync = new Sync();
  
  public void signal() {
	  sync.releaseShared(0);
  }  
  
  public void await() throws InterruptedException { 
	  sync.acquireSharedInterruptibly(0);
  }  
  
  private class Sync extends AbstractQueuedSynchronizer {  
	  protected int tryAcquireShared(int ignored) {  // Succeed if latch is open (state == 1), else fail  
		  return (getState() == 1)? 1: -1;  
	  }  
	  
  protected boolean tryReleaseShared(int ignored) {  
		  setState(1);// Latch is now open  return true;// Other threads may now be able to acquire  
	}
}
```
OneShotLatch could have been implemented by extending AQS rather than  delegating to it, but this is undesirable for several reasons [EJ Item 14]. Doing so  would undermine the simple (two-method) interface of OneShotLatch, and while  the public methods of AQS won’t allow callers to corrupt the latch state, callers  could easily use them incorrectly. None of the synchronizers in java.util.concurrent extends AQS directly—they all delegate to private inner subclasses of  AQS instead. 

 FutureTask uses the AQS synchronization state to hold the task status—  running, completed, or cancelled. It also maintains additional state variables to  hold the result of the computation or the exception it threw. It further maintains  a reference to the thread that is running the computation (if it is currently in the  running state), so that it can be interrupted if the task is cancelled. 

# Atomic Variables and Nonblocking  Synchronization 
of the classes in java.util.concurrent, such as Semaphore and ConcurrentLinkedQueue, provide better performance and scalability than alternatives  using synchronized.
Atomic variables can also be used as “better volatile variables” even if you are  not developing nonblocking algorithms. Atomic variables offer the same memory  semantics as volatile variables, but with additional support for atomic updates—  making them ideal for counters, sequence generators, and statistics gathering  while offering better scalability than lock-based alternatives. 

Volatile variables are a lighter-weight synchronization mechanism than locking because they do not involve context switches or thread scheduling. However,  volatile variables have some limitations compared to locking: while they provide  similar visibility guarantees, they cannot be used to construct atomic compound  actions. This means that volatile variables cannot be used when one variable depends on another, or when the new value of a variable depends on its old value. 

## Compare and swap 
CAS has three operands—a memory location V on  which to operate, the expected old value A, and the new value B. CAS atomically  updates V to the new value B, but only if the value in V matches the expected old  value A; otherwise it does nothing. In either case, it returns the value currently  in V.
The typical pattern for using CAS is first to read the value A from V, derive  the new value B from A, and then use CAS to atomically change V from A to  B so long as no other thread has changed V to another value in the meantime.  CAS addresses the problem of implementing atomic read-modify-write sequences  without locking, because it can detect interference from other threads. 

The language syntax for locking may be compact, but the work done by the  JVM and OS to manage locks is not. Locking entails traversing a relatively complicated code path in the JVM and may entail OS-level locking, thread suspension,  and context switches. In the best case, locking requires at least one CAS, so using  locks moves the CAS out of sight but doesn’t save any actual execution cost. On  the other hand, executing a CAS from within the program involves no JVM code,  system calls, or scheduling activity. What looks like a longer code path at the application level is in fact a much shorter code path when JVM and OS activity are taken into account. The primary disadvantage of CAS is that it forces the caller to  deal with contention (by retrying, backing off, or giving up), whereas locks deal  with contention automatically by blocking until the lock is available.

A good rule of thumb is that the cost of the “fast path” for uncontended  lock acquisition and release on most processors is approximately twice the cost  of a CAS. 
In  Java 5.0, low-level support was added to expose CAS operations on int, long,  and object references, and the JVM compiles these into the most efficient means  provided by the underlying hardware. On platforms supporting CAS, the runtime inlines them into the appropriate machine instruction(s); in the worst case,  if a CAS-like instruction is not available the JVM uses a spin lock. This low-level  JVM support is used by the atomic variable classes (AtomicXxx in java.util.concurrent.atomic) to provide an efficient CAS operation on numeric and reference  types; these atomic variable classes are used, directly or indirectly, to implement  most of the classes in java.util.concurrent. 

## Atomic variable classes 
fast (uncontended) path for updating an atomic variable is no slower than the fast path for acquiring a lock, and usually faster; the  slow path is definitely faster than the slow path for locks because it does not  involve suspending and rescheduling threads.
With algorithms based on atomic  variables instead of locks, threads are more likely to be able to proceed without  delay and have an easier time recovering if they do experience contention. 

The atomic variable classes provide a generalization of volatile variables to  support atomic conditional read-modify-write operations. AtomicInteger represents an int value, and provides get and set methods with the same memory semantics as reads and writes to a volatile int. 
There are twelve atomic variable classes, divided into four groups: scalars,  field updaters, arrays, and compound variables. The most commonly used atomic  variables are the scalars: AtomicInteger, AtomicLong, AtomicBoolean, and AtomicReference. All support CAS; the Integer and Long versions support arithmetic  as well.
The atomic array classes (available in Integer, Long, and Reference versions)  are arrays whose elements can be updated atomically. The atomic array classes  provide volatile access semantics to the elements of the array, a feature not available for ordinary arrays—a volatile array has volatile semantics only for the  array reference, not for its elements.
# Nonblocking algorithms 
An algorithm is called nonblocking  if failure or suspension of any thread cannot cause failure or suspension of another thread; an algorithm is called lock-free if, at each step, some thread can make  progress.
The key to creating nonblocking algorithms is figuring out how  to limit the scope of atomic changes to a single variable while maintaining data  consistency.
``` java
private class Node<E> {  private final E item;  private volatile Node<E> next;  public Node(E item) {  this.item = item;  }  }  private static AtomicReferenceFieldUpdater<Node, Node> nextUpdater  = AtomicReferenceFieldUpdater.newUpdater(  Node.class, Node.class, "next"); 
```
The atomic field updater classes (available in Integer, Long, and Reference  versions) represent a reflection-based “view” of an existing volatile field so that  CAS can be used on existing volatile fields.

The field updater classes are not tied to a specific instance;  one can be used to update the target field for any instance of the target class.  The atomicity guarantees for the updater classes are weaker than for the regular  atomic classes because you cannot guarantee that the underlying fields will not be  modified directly—the compareAndSet and arithmetic methods guarantee atomicity only with respect to other threads using the atomic field updater methods. 
ConcurrentLinkedQueue, updates to the next field of a Node are applied  using the compareAndSet method of nextUpdater. This somewhat circuitous approach is used entirely for performance reasons. For frequently allocated, shortlived objects like queue link nodes, eliminating the creation of an AtomicReference for each Node is significant enough to reduce the cost of insertion operations. 
However, in nearly all situations, ordinary atomic variables perform just fine—in  only a few cases will the atomic field updaters be needed. (The atomic field updaters are also useful when you want to perform atomic updates while preserving  the serialized form of an existing class.) 

The ABA problem is an anomaly that can arise from the naive use of compareand-swap in algorithms where nodes can be recycled

A CAS effectively asks “Is the value of V still A?”,  and proceeds with the update if so.
sometimes we really  want to ask “Has the value of V changed since I last observed it to be A?” For  some algorithms, changing V from A to B and then back to A still counts as a  change that requires us to retry some algorithmic step. 
This ABA problem can arise in algorithms that do their own memory management for link node objects. In this case, that the head of a list still refers to  a previously observed node is not enough to imply that the contents of the list  have not changed.

If you cannot avoid the ABA problem by letting the garbage  collector manage link nodes for you, there is still a relatively simple solution:  instead of updating the value of a reference, update a pair of values, a reference and a version number. Even if the value changes from A to B and back  to A, the version numbers will be different. AtomicStampedReference (and its  cousin AtomicMarkableReference) provide atomic conditional update on a pair  of variables. AtomicStampedReference updates an object reference-integer pair,  allowing “versioned” references that are immune8 to the ABA problem. Similarly,  AtomicMarkableReference updates an object reference-boolean pair that is used  by some algorithms to let a node remain in a list while being marked as deleted.
