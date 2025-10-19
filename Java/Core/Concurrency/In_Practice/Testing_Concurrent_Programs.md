# Testing Concurrent Programs 
Most tests of concurrent classes fall into one or both of the classic categories of  safety and liveness.

we defined safety as “nothing bad ever happens”  and liveness as “something good eventually happens”. 
Related to liveness tests are performance tests. Performance can be measured  in a number of ways, including:  Throughput: the rate at which a set of concurrent tasks is completed;  Responsiveness: the delay between a request for and completion of some action  (also called latency); or  Scalability: the improvement in throughput (or lack thereof) as more resources  (usually CPUs) are made available. 
## Testing for correctness 
Tests of essential concurrency properties require introducing more than one  thread. Most testing frameworks are not very concurrency-friendly: they rarely  include facilities to create threads or monitor them to ensure that they do not die  unexpectedly. If a helper thread created by a test case discovers a failure, the  framework usually does not know with which test the thread is associated, so  some work may be required to relay success or failure information back to the  main test runner thread so it can be reported. 
If a method is supposed to block under certain conditions, then a test for  that behavior should succeed only if the thread does not proceed. Testing that  a method blocks is similar to testing that a method throws an exception; if the  method returns normally, the test has failed. 
``` java
void testTakeBlocksWhenEmpty() {
	final BoundedBuffer<Integer> bb = new BoundedBuffer<Integer>(10);
	  Thread taker = () => { 
		  try {  
			  int unused = bb.take();  
			  fail(); // if we get here, it’s an error
		  } catch (InterruptedException success) {}  }};  
		
			try {  taker.start(); 
				Thread.sleep(LOCKUP_DETECT_TIMEOUT); 
				taker.interrupt(); 
				taker.join(LOCKUP_DETECT_TIMEOUT); 
				assertFalse(taker.isAlive());  
			} catch (Exception unexpected) {  
				fail();
			}
		}
```

The challenge to constructing effective safety tests for concurrent classes  is identifying easily checked properties that will, with high probability, fail  if something goes wrong, while at the same time not letting the failureauditing code limit concurrency artificially. It is best if checking the test property does not require any synchronization. 
Tests should be run on multiprocessor systems to increase the diversity  of potential interleavings. However, having more than a few CPUs does  not necessarily make tests more effective. To maximize the chance of  detecting timing-sensitive data races, there should be more active threads  than CPUs, so that at any given time some threads are running and some  are switched out, thus reducing the predicatability of interactions between  threads. 
sometimes yielding in the middle  of an operation, you may activate timing-sensitive bugs in code that does not use  adequate synchronization to access state. The inconvenience of adding these calls  for testing and removing them for production can be reduced by adding them  using aspect-oriented programming (AOP) tools. 

# Testing for performance 
Histograms of task completion times are normally the best way to visualize  variance in service time. Variances are only slightly more difficult to measure  than averages—you need to keep track of per-task completion times in addition  to aggregate completion time. Since timer granularity can be a factor in measuring individual task time (an individual task may take less than or close to the  smallest “timer tick”, which would distort measurements of task duration), to  avoid measurement artifacts we can measure the run time of small batches of put  and take operations instead. 
unless threads are continually blocking anyway because of tight synchronization requirements, nonfair semaphores provide much better throughput and  fair semaphores provides lower variance. Because the results are so dramatically  different, Semaphore forces its clients to decide which of the two factors to optimize for. 
# Garbage collection 
The timing of garbage collection is unpredictable, so there is always the possibility  that the garbage collector will run during a measured test run. If a test program  performs N iterations and triggers no garbage collection but iteration N +1 would  trigger a garbage collection, a small variation in the size of the run could have a  big (but spurious) effect on the measured time per iteration. 

There are two strategies for preventing garbage collection from biasing your  results. One is to ensure that garbage collection does not run at all during your  test (you can invoke the JVM with -verbose:gc to find out); alternatively, you  can make sure that the garbage collector runs a number of times during your run  so that the test program adequately reflects the cost of ongoing allocation and  garbage collection. The latter strategy is often better—it requires a longer test and  is more likely to reflect real-world performance. 

## Dynamic compilation 
One way to to prevent compilation from biasing your results is to run your  program for a long time (at least several minutes) so that compilation and interpreted execution represent a small fraction of the total run time. Another  approach is to use an unmeasured “warm-up” run, in which your code is executed enough to be fully compiled when you actually start timing.

Running the same test several times in the same JVM instance can be used to  validate the testing methodology. The first group of results should be discarded  as warm-up; seeing inconsistent results in the remaining groups suggests that  the test should be examined further to determine why the timing results are not  repeatable. 

The JVM uses various background threads for housekeeping tasks. When  measuring multiple unrelated computationally intensive activities in a single run,  it is a good idea to place explicit pauses between the measured trials to give the  JVM a chance to catch up with background tasks with minimal interference from  measured tasks. (When measuring multiple related activities, however, such as  multiple runs of the same test, excluding JVM background tasks in this way may  give unrealistically optimistic results.) 

## Dead code elimination 
Writing effective performance tests requires tricking the optimizer into  not optimizing away your benchmark as dead code. This requires every  computed result to be used somehow by your program—in a way that  does not require synchronization or substantial computation. 
Not only should every computed result be used, but results should also be  unguessable. Otherwise, a smart dynamic optimizing compiler is allowed to replace actions with precomputed results.

## Testing approaches
### Static analysis tools 
FindBugs includes detectors for the following concurrencyrelated bug patterns, and more are being added all the time: 

**Inconsistent synchronization** - Many objects follow the synchronization policy of  guarding all variables with the object’s intrinsic lock. If a field is accessed  frequently but not always with the this lock held, this may indicate that the  synchronization policy is not being consistently followed. 

**Invoking Thread.run.** Thread implements Runnable and therefore has a run  method. However, it is almost always a mistake to call Thread.run directly;  usually the programmer meant to call Thread.start. 

**Unreleased lock.** Unlike intrinsic locks, explicit locks (see Chapter 13) are not  automatically released when control exits the scope in which they were acquired. The standard idiom is to release the lock from a finally block;  otherwise the lock can remain unreleased in the event of an Exception. 

**Double-checked locking**. Double-checked locking is a broken idiom for reducing synchronization overhead in lazy initialization (see Section 16.2.4) that  involves reading a shared mutable field without appropriate synchronization. 

**Starting a thread from a constructor.** Starting a thread from a constructor introduces the risk of subclassing problems, and can allow the this reference to  escape the constructor. 