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


