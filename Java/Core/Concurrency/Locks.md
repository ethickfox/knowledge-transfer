# **Locks**
Lock is a more flexible and sophisticated thread synchronization mechanism than the standard synchronized block.
Using synchronized:
- It is not possible to interrupt a thread waiting to acquire a lock (lock Interruptibly).
- It is not possible to attempt to acquire a lock without being willing to wait for it forever (try lock).
- Cannot implement non-block-structured locking disciplines, as intrinsic locks must be released in the same block in which they are acquired.
- Faireless locking
# Lock interface
Один из базовых механизмов синхронизации в Java - интерфейс Lock и реализующие его классы. Любой объект в Java может выступать в качестве lock для целей синхронизации. Такие встроенные локи называются intrinsic locks или monitor locks. Исполняющий поток автоматически занимает лок перед тем, как войти в код внутри синхронизированного блока, и автоматически освобождает его, когда выходит из этого блока (через нормальное исполнение кода или выброс исключения).

Базовая реализация - ReentrantLock. Поток, который захватил, но не освободил ReentrantLock, владеет этим локом. Как и intrinsic лок, данный лок является reentrant. В общем случае, если поток запрашивает лок, который удерживается другим потоком, запрашивающий поток блокируется до освобождения лока. Однако если поток запрашивает лок, которым он уже владеет в данный момент, запрос удовлетворяется.

Таким образом, reentrancy означает, что локи захватываются на основе per thread, а не per invocation. Реализуется ассоциацией с каждым локом счётчика захватов. Когда счётчик равен нулю, лок считается незанятым. Когда поток захватывает прежде не занятый лок, JVM записывает владельца лока и увеличивает счётчик на единицу. Если тот же поток захватывает лок снова, счётчик опять увеличивается, а когда освобождает - уменьшается. Когда счётчик становится равным нулю, лок освобождается.

Let's take a look at the methods in the Lock interface:

**void lock()** – acquire the lock if it's available; if the lock isn't available a thread gets blocked until the lock is released

**void lockInterruptibly()** – this is similar to the lock(), but it allows the blocked thread to be interrupted and resume the execution through a thrown java.lang.InterruptedException

**boolean tryLock()** – this is a non-blocking version of lock() method; it attempts to acquire the lock immediately, return true if locking succeeds

**boolean tryLock(long timeout, TimeUnit timeUnit)** – this is similar to tryLock(), except it waits up the given timeout before giving up trying to acquire the Lock

**void unlock()** – unlocks the Lock instance

A locked instance should always be unlocked to avoid deadlock condition. A recommended code block to use the lock should contain a try/catch and finally block:

# ReentrantLock
ReentrantLock class implements the Lock interface. It offers the same concurrency and memory semantics, as the implicit monitor lock accessed using synchronized methods and statements, with extended capabilities.

We need to make sure that we are wrapping the lock() and the unlock() calls in the try-finally block to avoid the deadlock situations.

Let's see how the tryLock() works:

```Java
public void performTryLock(){
	//...
	boolean isLockAcquired = lock.tryLock(1, TimeUnit.SECONDS);
	
	if(isLockAcquired) {
		try {
			//Critical section here
		} finally {
			lock.unlock();
		}
	}
	//...
}
```
In this case, the thread calling tryLock(), will wait for one second and will give up waiting if the lock isn't available.
![](_img/Pasted%20image%2020250609172335.png)
# ReadWriteLock
поддерживает пару связанных блокировок: одну для операций только для чтения, а другую - для записи. Блокировка чтения может удерживаться одновременно несколькими потоками чтения, пока нет писателей. Блокировка записи является исключительной.
- Useful in consumer/producer scenarios. many readers and few writers(Ratio 1:100)
- Single writer completely blocks everyone else
- When read/write ratio increase - can be slow

**Read Lock** – multiple threads can acquire the read lock if
- no thread acquired or requested the write lock 
**Write Lock** – only one thread can acquire the write lock if
- no threads are reading or writing 
```Java
Map<String,String> syncHashMap = new HashMap<>();
ReadWriteLock lock = new ReentrantReadWriteLock();
// ...
Lock writeLock = lock.writeLock();

public void put(String key, String value) {
	try {
		writeLock.lock();
		syncHashMap.put(key, value);
	} finally {
		writeLock.unlock();
	}
}

public String remove(String key){
	try {
		writeLock.lock();
		return syncHashMap.remove(key);
	} finally {
		writeLock.unlock();
	}
}
```
# StampedLock
StampedLock is introduced in Java 8. It also supports both read and write locks. However, lock acquisition methods return a stamp that is used to release a lock or to check if the lock is still valid:
![](_img/Pasted%20image%2020250609172412.png)
```Java
Map<String,String> map = new HashMap<>();
private StampedLock lock = new StampedLock();

public void put(String key, String value){
long stamp = lock.writeLock();
try {
map.put(key, value);
} finally {
lock.unlockWrite(stamp);
}
}

public String get(String key) throws InterruptedException {
long stamp = lock.readLock();
try {
return map.get(key);
} finally {
lock.unlockRead(stamp);
}
}
```
Another feature provided by StampedLock is optimistic locking. Most of the time read operations don't need to wait for write operation completion and as a result of this, the full-fledged read lock isn't required.
Instead, we can upgrade to read lock:
```Java
public String readWithOptimisticLock(String key) {

long stamp = lock.tryOptimisticRead();

String value = map.get(key);

if(!lock.validate(stamp)) {

stamp = lock.readLock();

try {

return map.get(key);

} finally {

lock.unlock(stamp);

}

}

return value;

}
```
# Condition
The Condition class provides the ability for a thread to wait for some condition to occur while executing the critical section.

This can occur when a thread acquires the access to the critical section but doesn't have the necessary condition to perform its operation. For example, a reader thread can get access to the lock of a shared queue, which still doesn't have any data to consume.

Traditionally Java provides wait(), notify() and notifyAll() methods for thread intercommunication. Conditions have similar mechanisms, but in addition, we can specify multiple conditions:

```Java
Stack<String> stack = new Stack<>();
int CAPACITY = 5;
ReentrantLock lock = new ReentrantLock();
Condition stackEmptyCondition = lock.newCondition();
Condition stackFullCondition = lock.newCondition();
public void pushToStack(String item){
try {
lock.lock();

while(stack.size() == CAPACITY) {

stackFullCondition.await();

}

stack.push(item);

stackEmptyCondition.signalAll();

} finally {

lock.unlock();

}

}

public String popFromStack() {

try {

lock.lock();

while(stack.size() == 0) {

stackEmptyCondition.await();

}

return stack.pop();

} finally {

stackFullCondition.signalAll();

lock.unlock();

}

}
```

# Lock vs Synchronized Block
There are few differences between the use of synchronized block and using Lock API's:
- A synchronized block is fully contained within a method – we can have Lock API's lock() and unlock() operation in separate methods
- A synchronized block doesn't support the fairness, any thread can acquire the lock once released, no preference can be specified. We can achieve fairness within the Lock APIs by specifying the fairness property. It makes sure that longest waiting thread is given access to the lock
- A thread gets blocked if it can't get an access to the synchronized block. The Lock API provides tryLock() method. The thread acquires lock only if it's available and not held by any other thread. This reduces blocking time of thread waiting for the lock
- A thread which is in “waiting” state to acquire the access to synchronized block, can't be interrupted. The Lock API provides a method lockInterruptibly() which can be used to interrupt the thread when it's waiting for the lock
# Issues
## Deadlock
Состояние, когда два потока заняли ресурсы, к которым хочет обратиться противоположный поток.
![](_img/Pasted%20image%2020250609173123.png)
### Prevention
- **Lock Ordering -** **If you make sure that all locks are always taken in the same order by any thread, deadlocks cannot occur.**
- **Lock Timeout** - put a timeout on lock attempts meaning a thread trying to obtain a lock will only try for so long before giving up. If a thread does not succeed in taking all necessary locks within the given timeout, it will backup, free all locks taken, wait for a random amount of time and then retry.
- **Interruptible lock** acquisition allows locking to be used within cancellable activities.The lockInterruptibly method allows us to try and acquire a lock while being available for interruption.
![](_img/Pasted%20image%2020250609173202.png)
**Анализ объектов в дедлоке**
JVM allows you to diagnose deadlocks by displaying them in thread dumps. Such dumps include information on the state of the flow. If it is locked, the dump contains information about the monitor the stream is waiting to release. JVM looks at a graph of expected (busy) monitors before output of the dump threads, and if it finds a loop, it adds information about mutual locking, specifying the monitors and threads involved.
![](_img/Pasted%20image%2020250609173233.png)

