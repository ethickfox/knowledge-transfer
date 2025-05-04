### Intro

**Fairness.** Multiple users and programs may have equal claims on the machine’s  
resources. It is preferable to let them share the computer via finer-grained  
time slicing than to let one program run to completion and then start another  

## Fundamentals

### Thread Safety

Race Condition

```JavaScript
@NotThreadSafe
public class UnsafeSequence {
	private int value;
	/** Returns a unique value. */
	public int getNext() {
		return value++;
	}
}
```

![[Untitled 43.png|Untitled 43.png]]

```JavaScript
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

![[Untitled 1 8.png|Untitled 1 8.png]]

![[Untitled 2 5.png|Untitled 2 5.png]]

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

  

### Sharing objects

Visibility

There is no guarantee that reading thread will see variable written by another thread on a timely basis or even at all. In order to ensure visibility of memory writes across threads, you must use synchronisation

In this case program might output 42, 0 or stuck

![[_img/Untitled 3 5.png|Untitled 3 5.png]]

This might happen due to **reordering.** In order to have a better performance compiler might cache and reorder operations. When the main thread writes first to number and then to ready without synchronisation, the reader thread could see these operations in different order or not at all.

Always use propper synchronisation, when data accessed from different threads

Als e there might be a stale data(outdated), unless synchronization used every time a variable is accessed

64 bit variables(double, long) that are not marked as volatile are read and written in two 32 bit operations.

Locking is also about memory visibility

volatile - means the variable is shared by different threads and operations on that variable shouldn’t be reordered and cached. From a memory perspective writing volatile variable is like exiting synchronization block and reading - is entering.

Use volatile variables only when they simplify implementing and verifying your synchronization policy. Good use is checking a status flag to determine when to exit the loop.

Semantics of volatile are not strong enough to perform increment operation(count++) atomic,

Locking can guarantee both - visibility and atomacity, while volatile only visibility

You can use volatile variable only when all criteria are met:

1. Writes to the variable don’t depend on it’s current state(count++), or you can ensure that only a single thread ever update the value
2. The variable doesn’t participate in invariants with other state variables
3. Locking is not required for any other reason while the variable is being accessed

  

### Composing Objects

### Building Blocks

## Structuring Concurrent Applications

### Task Execution

### Cancellation and Shutdown

### Applying ThreadPool

## Liveness, Performance, Testing

### Avoiding Liveness Hazards

### Performance and Scalability

### Testing Concurrent Programs

## Advanced Topics

### Explicit Locks

### Building Custom Synchronizers

### Atomic Variables And NonBlocking Synchronizations

### Java Memory Model

### Annotations For Concurrency