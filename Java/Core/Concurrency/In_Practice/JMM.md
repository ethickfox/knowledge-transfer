# Java Memory Model
```
aVariable = 3; 
```
A memory model addresses the question “Under what conditions does a  thread that reads aVariable see the value 3?”
The Java Language Specification requires the JVM to maintain withinthread as-if-serial semantics: as long as the program has the same result as if it  were executed in program order in a strictly sequential environment, all these  games are permissible.
The Java Memory Model is specified in terms of actions, which include reads and  writes to variables, locks and unlocks of monitors, and starting and joining with threads.

The JMM defines a partial ordering2 called happens-before on all actions  within the program. To guarantee that the thread executing action B can see the  results of action A (whether or not A and B occur in different threads), there must  be a happens-before relationship between A and B. In the absence of a happens-before  ordering between two operations, the JVM is free to reorder them as it pleases. 


### Platform memory models 
memory. Processor architectures provide  varying degrees of cache coherence; some provide minimal guarantees that allow  different processors to see different values for the same memory location at virtually any time. The operating system, compiler, and runtime (and sometimes, the  program, too) must make up the difference between what the hardware provides  and what thread safety requires. 
An  architecture’s memory model tells programs what guarantees they can expect from  the memory system, and specifies the special instructions required (called memory  barriers or fences) to get the additional memory coordination guarantees required  when sharing data.

One convenient mental model for program execution is to imagine that there  is a single order in which the operations happen in a program, regardless of  what processor they execute on, and that each read of a variable will see the  last write in the execution order to that variable by any processor. This happy,  if unrealistic, model is called sequential consistency.
### Reordering 
the JMM can permit actions to appear to execute in different orders from the perspective of different threads, making reasoning about  ordering in the absence of synchronization even more complicated. The various  reasons why operations might be delayed or appear to execute out of order can  all be grouped into the general category of reordering. 
The actions  in each thread have no dataflow dependence on each other, and accordingly can  be executed out of order. (Even if they are executed in order, the timing by which  caches are flushed to main memory can make it appear, from the perspective of  B, that the assignments in A occurred in the opposite order.)

It is prohibitively difficult to reason about ordering  in the absence of synchronization; it is much easier to ensure that your program  uses synchronization appropriately. Synchronization inhibits the compiler, runtime, and hardware from reordering memory operations in ways that would violate the visibility guarantees provided by the JMM.1 

