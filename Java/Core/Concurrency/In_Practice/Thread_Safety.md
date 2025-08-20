
# Thread Safety

Race Condition

```java
@NotThreadSafe
public class UnsafeSequence {
	private int value;
	/** Returns a unique value. */
	public int getNext() {
		return value++;
	}
}
```

![Untitled 43.png](Untitled%2043.png)

```java
@ThreadSafe
public class Sequence {
	@GuardedBy("this") private int value;
	public synchronized int getNext() {
		return value++;
	}
}
```

Liveness hazard of a thread - something good eventually happens

Performance hazard of a thread - performance may drop due to context switching

Context switching- schedulre suspends current active thread temporary, so the other thread may run

Shared - variable could be accessed by many different threads

Mutable - it’s value may change during it’s lifetime

thread safety is all about protecting data

If multiple threads access the same mutable variable without synchronization, the program is broken, to fixit:

- Don’t share state variable accross threads
- Make state variable immutable
- Use synchronization whenever accessing variable

A class is **thread-safe** if it behaves correctly when accessed from multiple threads, regardless of the scheduling, interleaving and with no additional synchronization or other coordination on the part of calling code.

Thread-safe classes encapsulate any needed synchronization so the clients need not to provide their own

data race - thread writes a variable that might next be read by another thread or reads the variable that might be read by another thread if both threads don’t use synchronization

**Race conditions** occurs when correctness of computation relies on the relative timing or interleaving of multiple threads by the runtime

- check-then-act race condition - you observe something is true(file doesn’t exist); take an action based on observation(create file); But at the moment of action observation might become invalid
- read-modify-write - increment

Example

Race condition in Lazy initialization - you check if object is instantiated, if not - instantiate it

  

**Atomic operations** - operation A and B are atomic with respect to each other if from perspective of thread executing A, when another thread executes A, all operations B are executed or not started

**Compound actions** - actions that consist from several operations, like read-modify-write or check-then-act. These operations must be executed atomically in order to remain thread

When multiple variables participate in an invariant, they are not independent, thus when you update one, you should update other in the same atomic operation

![3.png](../../_img/3.png)

![Untitled 2 5.png](Untitled%202%205.png)

Intrinsic locks in java act like mutex - at most one thread can own the lock

```Java
synchronized(object){
	...
}
```

Reentrancy

When thread requests lock that is already held by another thread, the requesting thread blocks.

But because intrinsic lock is reentrant, if thread tries to acquire lock that already holds, it will succeed. Reentrancy means that lock acquired on per thread, not per invocation basis. If it happens, lock count is increased and associated thread remains the same. Reentrancy facilitates encapsulation of locking - calling super.doSomething inside of this.doSomething (assuming the’re both marked synchronized) won’t lead to deadlock.

Serializing access - threads take turns accessing the object exclusively, rather then doing so concurrently.

  

It’s common mistake to assume that synchronization is needed only when writing to shared variables. For all the access to mutable variable must be performed with the same lock held.

  

Every shared mutable variable should be guarded by exactly one lock.

By shared, we mean that a variable could be accessed by multiple threads; by mutable, we mean that its value could change during its lifetime.

If multiple threads access the same mutable state variable without appropriate synchronization, your program is broken. There are three ways to fix it: • Don’t share the state variable across threads;
• Make the state variable immutable;or
• Use synchronization whenever accessing the state variable.

It is far easier to design a class to be thread-safe than to retrofit it for thread safety later.

A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.
No set of operations performed sequentially or concurrently on instances of a thread-safe class can cause an instance to be in an invalid state.

The most common type of race condition is check-then-act, where a potentially stale observation is used to make a decision on what to do next. check-then-act: you observe something to be true (ﬁle X doesn’t exist) and then take action based on that observation (create X); but in fact the observation could have become invalid between the time you observed it and the time you acted on it (someone else created X in the meantime), causing a problem (unexpected exception, overwritten data, file corruption).

Read-modify-write operations, like incrementing a counter, define a transformation of an object’s state in terms of its previous state. To increment a counter, you have to know its previous value and make sure no one else changes or uses that value while you are in mid-update.

Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or none of it has. An atomic operation is one that is atomic with respect to all operations, including itself, that operate on the same state.

To ensure thread safety, check-then-act operations (like lazy initialization) and read-modify-write operations (like increment) must always be atomic. We refer collectively to check-then-act and read-modify-write sequences as compound actions: sequences of operations that must be executed atomically in order to remain thread-safe.

# Locking
The definition of thread safety requires that invariants be preserved regardless of timing or interleaving of operations in multiple threads.
holds. 
When multiple variables participate in an invariant, they are not independent: the value of one constrains the allowed value(s) of the others. Thus when updating one, you must update the others in the same atomic operation.
Similarly, the two values cannot be fetched simultaneously: between the time when thread A fetches the two values, thread B could have changed them, and again A may observe that the invariant does not hold. To preserve state consistency, update related state variables in a single atomic operation.

Nikita. Java concurrency in practice (Function). Kindle Edition. 
