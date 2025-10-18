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