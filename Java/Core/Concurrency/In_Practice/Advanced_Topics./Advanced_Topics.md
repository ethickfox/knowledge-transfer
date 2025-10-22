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
// CONDITION-PREDICATE: opened-since(n) (isOpen || generation>n)  @GuardedBy("this") private boolean isOpen;  @GuardedBy("this") private int generation;  public synchronized void close() {  isOpen = false;  }  public synchronized void open() {  ++generation;  isOpen = true;  notifyAll();  }  // BLOCKS-UNTIL: opened-since(generation on entry)  public synchronized void await() throws InterruptedException {  int arrivalGeneration = generation;  while (!isOpen && arrivalGeneration == generation)  wait();  }  } 

```
A state-dependent class should either fully expose (and document) its waiting and notification protocols to subclasses, or prevent subclasses from participating in them at all. 

One option for doing this is to effectively prohibit subclassing, either by making the class final or by hiding the condition queues, locks, and state variables  from subclasses.
Otherwise, if the subclass does something to undermine the  way the base class uses notify, it needs to be able to repair the damage.

It is generally best to encapsulate the condition queue so that it is not accessible outside the class hierarchy in which it is used. Otherwise, callers might be  tempted to think they understand your protocols for waiting and notification and  use them in a manner inconsistent with your design.
Unfortunately, this advice—to encapsulate objects used as condition queues—  is not consistent with the most common design pattern for thread-safe classes, in  which an object’s intrinsic lock is used to guard its state

Wellings (Wellings, 2004) characterizes the proper use of wait and notify in terms  of entry and exit protocols. For each state-dependent operation and for each operation that modifies state on which another operation has a state dependency,  you should define and document an entry and exit protocol. The entry protocol  is the operation’s condition predicate; the exit protocol involves examining any  state variables that have been changed by the operation to see if they might have  caused some other condition predicate to become true, and if so, notifying on the  associated condition queue. 
