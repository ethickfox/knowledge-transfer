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

  