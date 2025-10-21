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
