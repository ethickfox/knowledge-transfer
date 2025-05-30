# Clean Code

# Variables
- Use Intention-Revealing Names
	- Programmers must avoid leaving false clues that obscure the meaning of code.
	- Beware of using names which vary in small ways.
	- Spelling similar concepts similarly is information. Using inconsistent spellings is disinformation.
	- Make Meaningful Distinctions
		- Noise words are redundant. 
		- Use Pronounceable Names
		- Use Searchable Names
- Names should not be encoded with type or scope information.
- Names should describe everything that a function, variable, or class is or does. Don’t hide side effects with a name
---
# Functions
Functions should do only ONE thing.
- way to know that a function is doing more than “one thing” is if you can extract another function from it with a name that is not merely a restatement of its implementation
- statements within function should all be at the same level of abstraction
	- We want every function to be followed by those at the next level of abstraction so that we can read the program, descending one level of abstraction at a time as we read down the list of functions
- Flag arguments are ugly. this function does more than one thing. It does one thing if the flag is true and another if the flag is false
-  In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.
- Functions should either do something or answer something, but not both. Either your function should 
	* change the state of an object
	* or it should return some information about that object.
---
# Functions
-  Functions should not be 100 lines long. Functions should hardly ever be 20 lines long.
*  Never return null. When we return null, we are essentially creating work for ourselves and foisting problems upon our callers. All it takes is one missing null check to send an application spinning out of control.
- Unless you are working with an API which expects you to pass null, you should avoid passing null in your code whenever possible.
- Functions should have a small number of arguments. No argument is best, followed by one, two, and three.
- Output arguments are counterintuitive. Readers expect arguments to be inputs, not outputs. If your function must change the state of something, have it change the state of the object it is called on.
---
# Comments
- Sometimes it is useful to warn other programmers about certain consequences
- A comment may be used to amplify the importance of something that may otherwise seem inconsequential.
---
# Objects and Data Structures
- We do not want to expose the details of our data. Rather we want to express our data in abstract terms. 
- A poorly defined interface provides lots of functions that you must call, so coupling is high.
- If you do something a certain way, do all similar things in the same way. This goes back to the principle of least surprise. Be careful with the conventions you choose, and once chosen, be careful to continue to follow them.
- The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes.
- In general you should prefer nonstatic methods to static methods. When in doubt, make the function nonstatic. If you really want a function to be static, make sure that there is no chance that you’ll want it to behave polymorphically.
- Avoid Transitive Navigation In general we don’t want a single module to know much about its collaborators. More specifically, if A collaborates with B, and B collaborates with C, we don’t want modules that use A to know about C. (For example, we don’t want a.getB().getC().doSomething();.)
---
# Objects and Data Structures
**Law of Demeter** says a module should not know about the innards of the objects it manipulates. Method f of a class C should only call the methods of these:
- C
- An object created by f
- An object passed as an argument to f
- An object held in an instance variable of C
The following code appears to violate the Law of Demeter
``` java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
---
# Exceptions
- Your catch has to leave your program in a consistent state, no matter what happens in the try.
- Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests. 
- The price of **checked exceptions** is an Open/Closed Principle violation. 
	- If you throw a checked exception from a method in your code and the catch is three levels above, you must declare that exception in the signature of each method between you and the _catch_. 
	- This means that a change at a low level of the software can force signature changes on many higher levels. Encapsulation is broken because all functions in the path of a throw must know about details of that low-level exception.
- wrapping third-party APIs is a best practice. When you wrap a third-party API, you minimize your dependencies upon it: You can choose to move to a different library in the future without much penalty. Wrapping also makes it easier to mock out third-party calls when you are testing your own code.
---
# Unit Tests
It is unit tests that keep our code flexible, maintainable, and reusable. If you have tests, you do not fear making changes to the code! Without tests every change is a possible bug.
Tests should be 
- clean
- Readable
- The BUILD-OPERATE-CHECK pattern applied.
	- The first part builds up the test data
	- the second part operates on that test data
	- the third part checks that the operation yielded the expected results.
-  test a single concept in each test function.
- Apply FIRST rule
	- Fast
	- Independent
	- Repeatable
	- Self-Validating
	- Timely
---
# Unit Tests
- A test suite should test everything that could possibly break. The tests are insufficient so long as there are conditions that have not been explored by the tests 
- Coverage tools reports gaps in your testing strategy. They make it easy to find modules, classes, and functions that are insufficiently tested.
- Bugs tend to congregate. When you find a bug in a function, it is wise to do an exhaustive test of that function. You’ll probably find that the bug was not alone.
- A slow test is a test that won’t get run. When things get tight, it’s the slow tests that will be dropped from the suite. So do what you must to keep your tests fast.
---
# Classes
-  Classes should be small. With classes we count responsibilities.(SRP)
	-  Trying to identify responsibilities (reasons to change) often helps us recognize and create better abstractions in our code. 
	-  We want our systems to be composed of many small classes, not a few large ones. 
	- Each small class encapsulates a single responsibility, has a single reason to change
- Classes should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables. 
- By minimizing coupling in this way, our classes adhere to another class design principle known as the Dependency Inversion Principle
---
# Systems
- Software systems should separate the startup process, when the application objects are constructed and the dependencies are “wired” together, from the runtime logic that takes over after startup. 
- DSLs, when used effectively, raise the abstraction level above code idioms and design patterns. They allow the developer to reveal the intent of the code at the appropriate level of abstraction.
- We could create an adapters/separators to have an additional layer between using third party api. This will help in cases if api is changed - we will have a single place to fix everything. We manage third-party boundaries by having very few places in the code that refer to them. We may wrap them, or we may use an ADAPTER to convert from our perfect interface to the provided interface.
---
# Systems - Emergent Design
1. Runs all the tests
	1. A system that is comprehensively tested and passes all of its tests all of the time is a verifiable system.
	2. Making our systems testable pushes us toward a design where our classes are small and single purpose.
	3. Tight coupling makes it difficult to write tests
2. Contains no duplication
	1. For each few lines of code we add, we pause and reflect on the new design. Did we just degrade it?
	2. The fact that we have tests eliminates the fear that cleaning up the code will break it! 
3. Expresses the intent of the programmer
	1.  The clearer the author can make the code, the less time others will have to spend understanding it.
	2. We want to be able to hear a class or function name and not be surprised when we discover its responsibilities 
	3. Well-written unit tests are also expressive. 
4. Minimizes the number of classes and methods
---
# Concurrency - Defense principles
- SRP for concurrency
- Keep your concurrency-related code separate from other code.
- Limit the Scope of Data
- Threads Should Be as Independent as Possible
- Use Copies
- Use Language features
	- Use the provided thread-safe collections.
	*  Use the executor framework for executing unrelated tasks.
	*  Use nonblocking solutions when possible.
- Consider writing your threaded code such that each thread exists in its own world, sharing no data with any other thread.
- There are several different ways to partition behavior in a concurrent application.
	- Mutual Exclusion - Only one thread can access shared data or a shared resource at a time

__ Review__
- Possible issues
	* Starvation - One thread or a group of threads is prohibited from proceeding for an excessively long time or forever. For example, always letting fast-running threads through first could starve out longer running threads if there is no end to the fast-running threads.
	* Deadlock - Two or more threads waiting for each other to finish. Each thread has a resource that the other thread requires and neither can finish until it gets the other resource. All four of these conditions must hold for deadlock to be possible. Break any one of these conditions and deadlock is not possible.
		* Mutual exclusion
		* Lock & wait
		* No preemption
		* Circular wait
	* Livelock - Threads in lockstep, each trying to do work but finding another “in the way.” Due to resonance, threads continue trying to make progress but are unable to for an excessively long time—or forever.
* Popular patterns issues
	* Producer-Consumer
		* One or more producer threads create some work and place it in a buffer or queue
		* One or more consumer threads acquire that work from the queue and complete it.
	* Readers-writers
		* Writers tend to block many readers for a long period of time, thus causing throughput issues.
		* The challenge is to balance the needs of both readers and writers to satisfy correct operation, provide reasonable throughput and avoiding starvation. A simple strategy makes writers wait until there are no readers before allowing the writer to perform an update.
* Possible solutions
	* Beware Dependencies Between Synchronized Method. You have three options:
		* Tolerate the failure 
		* Solve the problem by changing the client: client-based locking. This strategy is risky because all programmers who use the server must remember to lock it before using it and unlock it when done. 
		* Solve the problem by changing the server, which additionally changes the client: server-based locking
		* In general you should prefer server-based locking for these reasons:
			- It reduces repeated code
			- It allows for better performance—You can swap out a thread-safe server for a non-thread safe one in the case of single-threaded deployment, thereby avoiding all overhead.
			- It reduces the possibility of error—All it takes is for one programmer to forget to lock properly.
			- It enforces a single policy
			- It reduces the scope of the shared variables
	* Avoid using more than one method on a shared object.
	* Otherwise use
		* Client-Based Locking—Have the client lock the server before calling the first method and make sure the lock’s extent includes code calling the last method.
		* Server-Based Locking—Within the server create a method that locks the server, calls all the methods, and then unlocks. Have the client call the new method.
		* Adapted Server—create an intermediary that performs the locking. This is an example of server-based locking, where the original server cannot be changed.
	- Keep Synchronized Sections Small
	- Locks are expensive because they create delays and add overhead. So we don’t want to litter our code with synchronized statements. On the other hand, critical sections must be guarded. So we want to design our code with as few critical sections as possible.
	* extending synchronization beyond the minimal critical section increases contention and degrades performance. Keep your synchronized sections as small as possible.
	* Writing Correct Shut-Down Code Is Hard
		* Graceful shutdown can be hard to get correct. Common problems involve deadlock, with threads waiting for a signal to continue that never comes.
		* So if you must write concurrent code that involves shutting down gracefully, expect to spend much of your time getting the shutdown to happen correctly. Think about shut-down early and get it working early. It’s going to take longer than you expect. Review existing algorithms because this is probably harder than you think.
---
# Testing Threaded Code
- Write tests that have the potential to expose problems and then run them frequently, with different programatic configurations and system configurations and load. If tests ever fail, track down the failure. Don’t ignore a failure just because the tests pass on a subsequent run.
- Treat spurious failures as candidate threading issues.
	- Bugs in threaded code might exhibit their symptoms once in a thousand, or a million, executions. Attempts to repeat the systems can be frustratingly.
	* The longer these “one-offs” are ignored, the more code is built on top of a potentially faulty approach. Do not ignore system failures as one-offs.
* Write the concurrency-supporting code such that it can be run in several configurations:
	* One thread, several threads, varied as it executes
	* Execute with test doubles that run quickly, slowly, variable.
	* Make your thread-based code especially pluggable so that you can run it in various configurations.
* Make your threaded code tunable.
	* Getting the right balance of threads typically requires trial an error. Early on, find ways to time the performance of your system under different configurations.
	* Run with more threads than processors.
	* Run on different platforms.
To increase your chances of catching such rare occurrences you can instrument your code and force it to run in different orderings by adding calls to methods like Object.wait(), Object.sleep(), Object.yield() and Object.priority().
Each of these methods can affect the order of execution, thereby increasing the odds
of detecting a flaw. It’s better when broken code fails as early and as often as possible.
- You can insert calls to wait(), sleep(), yield(), and priority() in your code by hand. It might be just the thing to do when you’re testing a particularly thorny piece of code.
- You could use tools like an Aspect-Oriented Framework, CGLIB, or ASM to programmatically instrument your code. 
	- Use a class with a single method add calls to this in various places within your code:
	* use a simple aspect that randomly selects among doing nothing, sleeping, or yielding.
	* Make sure when you are testing your thread-aware code, you are only testing it and nothing else. This suggests that your thread-aware code should be small and focused.
---