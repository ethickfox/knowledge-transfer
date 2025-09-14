# Synchronized collections
The synchronized collection classes include Vector and Hashtable, 1.2, the synchronized wrapper classes created by the Collections.synchronizedXxx factory methods.

The synchronized collections are thread-safe, but you may sometimes need to use additional client-side locking to guard compound actions.
Common compound actions on collections include iteration (repeatedly fetch elements until the collection is exhausted), navigation (ﬁnd the next element after this one according to some order), and conditional operations such as put-if-absent

An alternative to locking the collection during iteration is to clone the collection and iterate the copy instead. Since the clone is thread-conﬁned, no other thread can modify it during iteration, eliminating the possibility of ConcurrentModificationException.

## Hidden iterators
you have to remember to use locking everywhere a shared collection might be iterated.
The addTenThings method could throw ConcurrentModificationException, because the collection is being iterated by toString in the process of preparing the debugging message. Of course, the real problem is that HiddenIterator is not thread-safe; the HiddenIterator lock should be acquired before using set in the println call, but debugging and logging code commonly neglect to do this.
``` java
@GuardedBy("this")
 private final Set<Integer> set = new HashSet<Integer>();
 public synchronized void add(Integer i) { set.add(i); } public synchronized void remove(Integer i) { set.remove(i); }  public void addTenThings() {
Random r = new Random();
for (int i = 0; i < 10; i++) add(r.nextInt());
System.out.println("DEBUG: added ten elements to " + set); }
}
```
Iteration is also indirectly invoked by the collection’s hashCode and equals methods, which may be called if the collection is used as an element or key of another collection. Similarly, the containsAll, removeAll,and retainAll methods, as well as the constructors that take collections as arguments, also iterate the collection. All of these indirect uses of iteration can cause ConcurrentModificationException.
# Concurrent collections
Replacing synchronized collections with concurrent collections can oﬀer dramatic scalability improvements with little risk.
- BlockingQueue extends Queue to add blocking insertion and retrieval operations. If the queue is empty, a retrieval blocks until an element is available, and if the queue is full (for bounded queues) an insertion blocks until there is space available. Blocking queues are extremely useful in producer-consumer designs,
- ConcurrentHashMap is uses a finer-grained locking mechanism called lock striping. to allow a greater degree of shared access. Arbitrarily many reading threads can access the map concurrently, readers can access the map concurrently with writers, and a limited number of writers can modify the map concurrently. along with the other concurrent collections, further improve on the synchronized collection classes by providing iterators that do not throw ConcurrentModificationException, thus eliminating the need to lock the collection during iteration. The iterators returned by ConcurrentHashMap are weakly consistent instead of fail-fast. A weakly consistent iterator can tolerate concurrent modiﬁcation, traverses elements as they existed when the iterator was constructed, and may (but is not guaranteed to) reflect modiﬁcations to the collection after the construction of the iterator.
   - Since the result of size could be out of date by the time it is computed, it is really only an estimate, so size is allowed to return an approximation instead of an exact count. While at first this may seem disturbing, in reality methods like size and isEmpty are far less useful in concurrent environments because these quantities are moving targets. So the requirements for these operations were weakened to enable performance optimizations for the most important operations, primarily get, put, containsKey,and remove.
   - Only if your application needs to lock the map for exclusive access3 is ConcurrentHashMap not an appropriate drop-in replacement for synchronized Maps
- CopyOnWriteArrayList is a concurrent replacement for a synchronized List that offers better concurrency in some common situations and eliminates the need to lock or copy the collection during iteration.Iterators for the copy-on-write collections retain a reference to the backing array that was current at the start of iteration, and since this will never change, they need to synchronize only brieﬂy to ensure visibility of the array contents. As a result, multiple threads can iterate the collection without interference from one another or from threads wanting to modify the collection. large; the copy-on-write collections are reasonable to use only when iteration is far more common than modiﬁcation.
- Blocking queues provide blocking put and take methods as well as the timed equivalents offer and poll. If the queue is full, put blocks until space becomes available; if the queue is empty, take blocks until an element is available. Queues can be bounded or unbounded; unbounded queues are never full, so a put on an unbounded queue never blocks.One of the most common producer-consumer designs is a thread pool coupled with a work queue; this pattern is embodied in the Executor task execution framework. For mutable objects, producer-consumer designs and blocking queues facilitate serial thread confinement for handing off ownership of objects from producers to consumers. A thread-conﬁned object is owned exclusively by a single thread, but that ownership can be “transferred” by publishing it safely where only one other thread will gain access to it and ensuring that the publishing thread does not access it after the handoff.
- deques lend themselves to a related pattern called work stealing. A producerconsumer design has one shared work queue for all consumers; in a work stealing design, every consumer has its own deque. If a consumer exhausts the work in its own deque, it can steal work from the tail of someone else’s deque.

# Synchronizers
A synchronizer is any object that coordinates the control flow of threads based on its state.
## Latches
A latch is a synchronizer that can delay the progress of threads until it reaches its terminal state. A latch acts as a gate: until the latch reaches the terminal state the gate is closed and no thread can pass, and in the terminal state the gate opens, allowing all threads to pass. Once the latch reaches the terminal state, it cannot change state again, so it remains open forever. Latches can be used to ensure that certain activities do not proceed until other one-time activities complete,
CountDownLatch is a flexible latch implementation that can be used in any of these situations; it allows one or more threads to wait for a set of events to occur. The latch state consists of a counter initialized to a positive number, representing the number of events to wait for.
FutureTask also acts like a latch. A computation represented by a FutureTask is implemented with a Callable, the result-bearing equivalent of Runnable, and can be in one of three states: waiting to run, running, or completed. Completion subsumes all the ways a computation can complete, including normal completion, cancellation, and exception. Once a FutureTask enters the completed state, it stays in that state forever.

Counting semaphores are used to control the number of activities that can access a certain resource or perform a given action at the same time. Semaphore manages a set of virtual permits; the initial number of permits is passed to the Semaphore constructor.

Barriers
Barriers are similar to latches in that they block a group of threads until some event has occurred. The key difference is that with a barrier, all the threads must come together at a barrier point at the same time in order to proceed.
proceed. 
Latches are for waiting for events; barriers are for waiting for other threads.
CyclicBarrier allows a fixed number of parties to rendezvous repeatedly at a barrier point and is useful in parallel iterative algorithms that break down a problem into a fixed number of independent subproblems. Threads call await when they reach the barrier point, and await blocks until all the threads have reached the barrier point. If all threads meet at the barrier point, the barrier has been successfully passed, in which case all threads are released and the barrier is reset so it can be used again.
Another form of barrier is Exchanger, a two-party barrier in which the parties exchange data at the barrier point. Exchangers are useful when the parties perform asymmetric activities, for example when one thread fills a buffer with data and the other thread consumes the data from the buffer; these threads could use an Exchanger to meet and exchange a full buffer for an empty one. When two threads exchange objects via an Exchanger, the exchange constitutes a safe publication of both objects to the other party.

Building an efficient, scalable result cache
Example of cache
``` java
class Memoizer3<A, V> implements Computable<A, V> {  
    private final Map<A, Future<V>> cache = new ConcurrentHashMap<>();  
    private final Computable<A, V> c;  
  
    public Memoizer3(Computable<A, V> c) {  
        this.c = c;  
    }  
  
    public V compute(final A arg) throws ExecutionException, InterruptedException {  
        Future<V> f = cache.get(arg);  
        if (f == null) {  
            Callable<V> eval = () -> c.compute(arg);  
            FutureTask<V> ft = new FutureTask<>(eval);  
            f = ft;  
            cache.put(arg, ft);  
            ft.run(); // call to c.compute happens here  
        }  
        return f.get();  
    }  
}
```
Memoizer3 is vulnerable to this problem because a compound action (putif-absent) is performed on the backing map that cannot be made atomic using locking.

Caching a Future instead of a value creates the possibility of cache pollution: if a computation is cancelled or fails, future attempts to compute the result will also indicate cancellation or failure. To avoid this, Memoizer removes the Future from the cache if it detects that the computation was cancelled; it might also be desirable to remove the Future upon detecting a RuntimeException if the computation might succeed on a future attempt.
``` java
class Memoizer4<A, V> implements Computable<A, V> {  
    private final ConcurrentMap<A, Future<V>> cache = new ConcurrentHashMap<A, Future<V>>();  
    private final Computable<A, V> c;  
  
    public Memoizer4(Computable<A, V> c) {  
        this.c = c;  
    }  
  
    public V compute(final A arg) throws InterruptedException {  
        while (true) {  
            Future<V> f = cache.get(arg);  
            if (f == null) {  
                Callable<V> eval = () -> c.compute(arg);  
                FutureTask<V> ft = new FutureTask<>(eval);  
                f = cache.putIfAbsent(arg, ft);  
                if (f == null) {  
                    f = ft;  
                    ft.run();  
                }  
            }  
            try {  
                return f.get();  
            } catch (CancellationException e) {  
                cache.remove(arg, f);  
            } catch (ExecutionException e) {  
            }  
        }  
    }  
}
```


Summary
- Guard each mutable variable with a lock.
- Guard all variables in an invariant with the same lock.
- Hold locks for the duration of compound actions.
- A program that accesses a mutable variable from multiple threads without synchronization is a broken program.
- Don’t rely on clever reasoning about why you don’t need to synchronize.
- Include thread safety in the design process—or explicitly document that your class is not thread-safe.
- Document your synchronization policy.