# Clean Code
## Variables Naming
### Use Intention-Revealing Names
The problem isn’t the simplicity of the code but the implicity of the code (to coin a phrase): the degree to which the context is not explicit in the code itself.
### Avoid Disinformation
- Programmers must avoid leaving false clues that obscure the meaning of code. We should avoid words whose entrenched meanings vary from our intended meaning.
- Beware of using names which vary in small ways.
- Spelling similar concepts similarly is information. Using inconsistent spellings is disinformation.
### Make Meaningful Distinctions
- If names must be different, then they should also mean something different.
- Noise words are another meaningless distinction. Imagine that you have a Product class. If you have another called ProductInfo or ProductData, you have made the names different without making them mean anything different.
- Noise words are redundant. The word variable should never appear in a variable name. The word table should never appear in a table name.
- Use Pronounceable Names
- Use Searchable Names
- The length of a name should correspond to the size of its scope
- Avoid Encodings - Encoding type or scope information into names simply adds an extra burden of deciphering
- Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have fetch, retrieve, and get as equivalent methods of different classes.
- Avoid using the same word for two purposes. you need to place names in context for your reader by enclosing them in well-named classes, functions, or namespaces.
## Functions
Functions should not be 100 lines long. Functions should hardly ever be 20 lines long.
- blocks within if statements, else statements, while statements, and so on should be one line long. Probably that line should be a function call.
- functions should not be large enough to hold nested structures. Therefore, the indent level of a function should not be greater than one or two.
### One Responsibility
FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY.
- another way to know that a function is doing more than “one thing” is if you can extract another function from it with a name that is not merely a restatement of its implementation
- statements within function should all be at the same level of abstraction
- The Stepdown Rule: We want every function to be followed by those at the next level of abstraction so that we can read the program, descending one level of abstraction at a time as we read down the list of functions
- general rule for switch statements is that they can be tolerated if they appear only once, are used to create polymorphic objects, and are hidden behind an inheritance
- The ideal number of arguments for a function 
	- is zero (niladic).
	- Next comes one (monadic), 
	- followed closely by two (dyadic). 
	- Three arguments (triadic) should be avoided where possible. 
	- More than three (polyadic) requires very special justification—and then shouldn’t be used anyway
- Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!
- Reducing the number of arguments by creating objects out of them may seem like cheating, but it’s not. When groups of variables are passed together,  they are likely part of a concept that deserves a name of its own.
- function and argument should form a very nice verb/noun pair. For example, write(name) is very evocative. Whatever this “name” thing is, it is being “written.” An even better name might be writeField(name), which tells us that the “name” thing is a “field.” This last is an example of the keyword form of a function name.
- In the days before object oriented programming it was sometimes necessary to have output arguments. However, much of the need for output arguments disappears in OO languages because this is intended to act as an output argument. In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.

* Functions should either do something or answer something, but not both. Either your function should 
	* change the state of an object
	* or it should return some information about that object. 

* Try/catch blocks are ugly in their own right. They confuse the structure of the code and mix error processing with normal processing. So it is better to extract the bodies of the try and catch blocks out into functions of their own.
### Formatting
- if one function calls another, they should be vertically close, and the caller should be above the callee, if at all possible.
## Comments
- Sometimes a comment goes beyond just useful information about the implementation and provides the intent behind a decision. In the following case we see an interesting decision documented by a comment. 

* Sometimes it is useful to warn other programmers about certain consequences

* A comment may be used to amplify the importance of something that may otherwise seem inconsequential.

* As useful as javadocs are for public APIs, they are anathema to code that is not intended for public consumption.
## Objects and Data Structures
- We do not want to expose the details of our data. Rather we want to express our data in abstract terms. This is not merely accomplished by using interfaces and/or getters and setters. Serious thought needs to be put into the best way to represent the data that an object contains. The worst option is to blithely add getters and setters
	- **Objects** hide their data behind abstractions and expose functions that operate on that data
	- **Data structure** expose their data and have no meaningful functions.
- **Procedural code** (code using data structures) makes it easy to add new functions without changing the existing data structures. Procedural code makes it hard to add new data structures because all the functions must change.
``` java
public class Circle {
	public Point center;
	public double radius;
}

public class Geometry {
	public final double PI = 3.141592653589793;
	
	public double area(Object shape) throws NoSuchShapeException {
		if (shape instanceof Square) {
			Square s = (Square)shape;
			return s.side * s.side;
		} else if (shape instanceof Rectangle) {
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		}
		throw new NoSuchShapeException();
	}
}

```
- **Object Oriented code** makes it easy to add new classes without changing existing functions. OO code makes it hard to add new functions because all the classes must change.
```java
public class Square implements Shape {
	private Point topLeft;
	private double side;
	
	public double area() {
	return side*side;
	}
}

public class Rectangle implements Shape {
	private Point topLeft;
	private double height;
	private double width;
	
	public double area() {
		return height * width;
	}
```

**Law of Demeter** says a module should not know about the innards of the objects it manipulates. Method f of a class C should only call the methods of these:
- C
- An object created by f
- An object passed as an argument to f
- An object held in an instance variable of C
The following code appears to violate the Law of Demeter
```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```
This kind of code is often called a train wreck.  It is usually best to split them up as follows:
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
```

**Active Records** are special forms of DTOs. They are data structures with public (or bean-accessed) variables; but they typically have navigational methods like save and find. Typically these Active Records are direct translations from database tables, or other data sources.

## Exceptions
- In a way, try blocks are like transactions. Your catch has to leave your program in a consistent state, no matter what happens in the try. For this reason it is good practice to start with a try-catch-finally statement when you are writing code that could throw exceptions. This helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the try.
- Try to write tests that force exceptions, and then add behavior to your handler to satisfy your tests. This will cause you to build the transaction scope of the try block first and will help you maintain the transaction nature of that scope.
- The price of **checked exceptions** is an Open/Closed Principle1 violation. If you throw a checked exception from a method in your code and the catch is three levels above, you must declare that exception in the signature of each method between you and the _catch_. This means that a change at a low level of the software can force signature changes on many higher levels. Encapsulation is broken because all functions in the path of a throw must know about details of that low-level exception.
- Each exception that you throw should provide enough context to determine the source and location of an error.
- wrapping third-party APIs is a best practice. When you wrap a third-party API, you minimize your dependencies upon it: You can choose to move to a different library in the future without much penalty. Wrapping also makes it easier to mock out third-party calls when you are testing your own code.
- This is called the SPECIAL CASE PATTERN. You create a class or configure an object so that it handles a special case for you.
```java
try {
	MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
	m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
	m_total += getMealPerDiem();
}

// turns into

MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();

public class PerDiemMealExpenses implements MealExpenses {
	public int getTotal() {
	// return the per diem default
	}
}
```
- Never return null. When we return null, we are essentially creating work for ourselves and foisting problems upon our callers. All it takes is one missing null check to send an application spinning out of control.
- Unless you are working with an API which expects you to pass null, you should avoid passing null in your code whenever possible.
### Boundaries
We could create an adapters/separators to have an additional layer between using third party api. This will help in cases if api is changed - we will have a single place to fix everything. We manage third-party boundaries by having very few places in the code that refer to them. We may wrap them, or we may use an ADAPTER to convert from our perfect interface to the provided interface.
### Unit Tests
The Three Laws of TDD:
1. You may not write production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
3. You may not write more production code than is sufficient to pass the currently failing test.
- It is unit tests that keep our code flexible, maintainable, and reusable. If you have tests, you do not fear making changes to the code! Without tests every change is a possible bug.
-  tests enable change
#### Clean tests
- Readability
- The BUILD-OPERATE-CHECK pattern is made obvious by the structure of these tests.Each of the tests is clearly split into three parts. The first part builds up the test data, the second part operates on that test data, and the third part checks that the operation yielded the expected results.
-  test a single concept in each test function.
#### FIRST
- **Fast** Tests should be fast. They should run quickly. When tests run slow, you won’t want to run them frequently.
- **Independent** Tests should not depend on each other. One test should not set up the conditions for the next test. You should be able to run each test independently and run the tests in any order you like.
- **Repeatable** Tests should be repeatable in any environment. You should be able to run the tests in the production environment, in the QA environment
- **Self-Validating** The tests should have a boolean output. Either they pass or fail. You should not have to read through a log file to tell whether the tests pass.
- **Timely** The tests need to be written in a timely fashion. Unit tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may find the production code to be hard to test

Classes
- We like to keep our variables and utility functions private, but we’re not fanatic about it. Sometimes we need to make a variable or utility function protected so that it can be accessed by a test. For us, tests rule. If a test in the same package needs to call a function or access a variable, we’ll make it protected or package scope. However, we’ll first look for a way to maintain privacy.
- The first rule of classes is that they should be small. With classes we count responsibilities.
* The Single Responsibility Principle (SRP) states that a class or module should have one, and only one, reason to change. Trying to identify responsibilities (reasons to change) often helps us recognize and create better abstractions in our code. We want our systems to be composed of many small classes, not a few large ones. Each small class encapsulates a single responsibility, has a single reason to change, and collaborates with a few others to achieve the desired system behaviors.
- Classes should have a small number of instance variables. Each of the methods of a class should manipulate one or more of those variables. In general the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.
- you want to extract one small part of that function into a separate function. However, the code you want to extract uses four of the variables declared in the function. Must you pass all four of those variables into the new function as arguments? Not at all! If we promoted those four variables to instance variables of the class, then we could extract the code without passing any variables at all. It would be easy to break the function up into small pieces.
- breaking a large function into many smaller functions often gives us the opportunity to split several smaller classes out as well. This gives our program a much better organization and a more transparent structure.
- By minimizing coupling in this way, our classes adhere to another class design principle known as the Dependency Inversion Principle (DIP). In essence, the DIP says that our classes should depend upon abstractions, not on concrete details.
## Systems
- Software systems should separate the startup process, when the application objects are constructed and the dependencies are “wired” together, from the runtime logic that takes over after startup. The startup process is a concern that any application must address. The separation of concerns is one of the oldest and most important design techniqu
- The code for the startup process is ad hoc and it is mixed in with the runtime logic. Here is a typical example:

``` java

public Service getService() {
    if (service == null){
	    service = new MyServiceImpl(...); // Good enough default for most cases?
	}
	return service;
}

```

- This is the LAZY INITIALIZATION/EVALUATION idiom, and it has several merits. We don’t incur the overhead of construction unless we actually use the object, and our startup times can be faster as a result. We also ensure that null is never returned.

- we now have a hard-coded dependency on MyServiceImpl and everything its constructor requires
- If MyServiceImpl is a heavyweight object, we will need to make sure that an appropriate TEST DOUBLE1 or MOCK OBJECT gets assigned to the service field before this method is called during unit testing. Because we have construction logic mixed in with normal runtime processing, we should test all execution paths (for example, the null test and its block). Having both of these responsibilities means that the method is doing more than one thing, so we are breaking the Single Responsibility Principle in a small way.
- Perhaps worst of all, we do not know whether MyServiceImpl is the right object in all cases.
- convenient idioms lead to modularity breakdown. The startup process of object construction and wiring is no exception. We should modularize this process separately from the normal runtime logic and we should make sure that we have a global, consistent strategy for resolving our major dependencies.
- Inversion of Control moves secondary responsibilities from an object to other objects that are dedicated to the purpose, thereby supporting the Single Responsibility Principle. In the context of dependency management, an object should not take responsibility for instantiating dependencies itself. Instead, it should pass this responsibility to another “authoritative” mechanism

- An optimal system architecture consists of modularized domains of concern, each of which is implemented with Plain Old Java (or other) Objects. The different domains are integrated together with minimally invasive Aspects or Aspect-like tools. This architecture can be test-driven, just like the code.
- The agility provided by a POJO system with modularized concerns allows us to make optimal, just-in-time decisions, based on the most recent knowledge. The complexity of these decisions is also reduced.

- Standards make it easier to reuse ideas and components, recruit people with relevant experience, encapsulate good ideas, and wire components together. However, the process of creating standards can sometimes take too long for industry to wait, and some standards lose touch with the real needs of the adopters they are intended to serve.
- DSLs, when used effectively, raise the abstraction level above code idioms and design patterns. They allow the developer to reveal the intent of the code at the appropriate level of abstraction.
- Domain-Specific Languages allow all levels of abstraction and all domains in the application to be expressed as POJOs, from high-level policy to low-level details.

Emergence

Emergent Design

- Runs all the tests
  A system that is comprehensively tested and passes all of its tests all of the time is a verifiiable system. making our systems testable pushes us toward a design where our classes are small and single purpose. Tight coupling makes it difficult to write tests.
* Contains no duplication
  For each few lines of code we add, we pause and reflect on the new design. Did we just degrade it? If so, we clean it up and run our tests to demonstrate that we haven’t broken anything. The fact that we have these tests eliminates the fear that cleaning up the code will break it! This is also where we apply the final three rules of simple design: Eliminate duplication, ensure expressiveness, and minimize the number of classes and methods
* Expresses the intent of the programmer
  The clearer the author can make the code, the less time others will have to spend understanding it. Thiswill reduce defects and shrink the cost of maintenance.You can express yourself by choosing good names. We want to be able to hear a class or function name and not be surprised when we discover its responsibilities You can also express yourself by keeping your functions and classes small. Small classes and functions are usually easy to name, easy to write, and easy to understand. You can also express yourself by using standard nomenclature. Design patterns, for example, are largely about communication and expressiveness. Well-written unit tests are also expressive. A primary goal of tests is to act as documentation by example. Someone reading our tests should be able to get a quick understanding of what a class is all about.
* Minimizes the number of classes and methods
  In an effort to make our classes and methods small, we might create too many tiny classes and methods. So this rule suggests that we also keep our function and class counts low.

### Concurrency
- Concurrency can sometimes improve performance, but only when there is a lot of wait time that can be shared between multiple threads or multiple processors. Neither situation is trivial.
- Design of a concurrent algorithm can be remarkably different from the design of a single-threaded system. The decoupling of what from when usually has a huge effect on the structure of the system
Defense principles
- SRP for concurrency
* Concurrency-related code has its own life cycle of development, change, and tuning.
* Concurrency-related code has its own challenges, which are different from and often more difficult than non concurrency-related code.
* The number of ways in which miswritten c^oncurrency-based code can fail makes it challenging enough without the added burden of surrounding application code.
* **Keep your concurrency-related code separate from other code.**
* Limit the Scope of Data
* One solution is to use the synchronized keyword to protect a critical section in the code that uses the shared object
* Take data encapsulation to heart; severely limit the access of any data that may be shared.
* Use Copies
* In some situations it is possible to copy objects and treat them as read-only. In other cases it might be possible to copy objects, collect results from multiple threads in these copies and then merge the results in a single thread.
* Threads Should Be as Independent as Possible
* Consider writing your threaded code such that each thread exists in its own world, sharing no data with any other thread.
	* For example: classes that subclass from HttpServlet receive all of their information as parameters passed in to the doGet and doPost methods. This makes each Servlet act as if it has its own machine. So long as the code in the Servlet uses only local variables, there is no chance that the Servlet will cause synchronization problems
- There are several things to consider when writing threaded code in Java 5:
	*  Use the provided thread-safe collections.
	*  Use the executor framework for executing unrelated tasks.
	*  Use nonblocking solutions when possible.
	* Several library classes are not thread safe.
* There are several different ways to partition behavior in a concurrent application.
	* Bound Resources  - Resources of a fixed size or number used in a concurrent environ-ment. Example: database connections and fixed-size read/write buffers.
	* Mutual Exclusion - Only one thread can access shared data or a shared resource at a time.
	* Starvation - One thread or a group of threads is prohibited from proceeding for an excessively long time or forever. For example, always letting fast-running threads through first could starve out longer running threads if there is no end to the fast-running threads.
	* Deadlock - Two or more threads waiting for each other to finish. Each thread has a resource that the other thread requires and neither can finish until it gets the other resource.
	* Livelock - Threads in lockstep, each trying to do work but finding another “in the way.” Due to resonance, threads continue trying to make progress but are unable to for an excessively long time—or forever.

Producer-Consumer
* One or more producer threads create some work and place it in a buffer or queue
* One or more consumer threads acquire that work from the queue and complete it.
The queue between the producers and consumers is a bound resource. This means producers must wait for free space in the queue before writing and consumers must wait until there is something in the queue to consume. 
* Coordination between the producers and consumers via the queue involves producers and consumers signaling each other. The producers write to the queue and signal that the queue is no longer empty. Consumers read from the queue and signal that the queue is no longer full. Both potentially wait to be notified when they can continue.
Readers-writers
* Writers tend to block many readers for a long period of time, thus causing throughput issues.
* The challenge is to balance the needs of both readers and writers to satisfy correct operation, provide reasonable throughput and avoiding starvation. A simple strategy makes writers wait until there are no readers before allowing the writer to perform an update.
* On the other hand, if there are frequent writers and they are given priority, throughput will suffer. Finding that balance and avoiding concurrent update issues is what the problem addresses.
* Unless carefully designed, systems that compete in this way can experience deadlock, livelock, throughput, and efficiency degradation. Most concurrent problems you will likely encounter will be some variation of these three problems. 
- Beware Dependencies Between Synchronized Methods
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
## Testing Threaded Code
- Write tests that have the potential to expose problems and then run them frequently, with different programatic configurations and system configurations and load. If tests ever fail, track down the failure. Don’t ignore a failure just because the tests pass on a subsequent run.
* Treat spurious failures as candidate threading issues.
* Bugs in threaded code might exhibit their symptoms once in a thousand, or a million, executions. Attempts to repeat the systems can be frustratingly.
* The longer these “one-offs” are ignored, the more code is built on top of a potentially faulty approach. Do not ignore system failures as one-offs.
* Get your nonthreaded code working first.
* Make sure code works outside of its use in threads. Generally, this means creating POJOs that are called by your threads. The POJOs are not thread aware, and can therefore be tested outside of the threaded environment. Do not try to chase down nonthreading bugs and threading bugs at the same time. Make sure your code works outside of threads.
* Make your threaded code pluggable. Write the concurrency-supporting code such that it can be run in several configurations:
	*  One thread, several threads, varied as it executes
	* Threaded code interacts with something that can be both real or a test double.
	* Execute with test doubles that run quickly, slowly, variable.
	* Configure tests so they can run for a number of iterations.
	* Make your thread-based code especially pluggable so that you can run it in various configurations.
* Make your threaded code tunable.
	* Getting the right balance of threads typically requires trial an error. Early on, find ways to time the performance of your system under different configurations. Allow the number of threads to be easily tuned. Consider allowing it to change while the system is running. Consider allowing self-tuning based on throughput and system utilization.
	* Run with more threads than processors - Things happen when the system switches between tasks. To encourage task swapping, run with more threads than processors or cores. The more frequently your tasks swap, the more likely you’ll encounter code that is missing a critical section or causes deadlock.
* Run on different platforms.
## **Instrument Your Code to Try and Force Failures**
To increase your chances of catching such rare occurrences you can instrument your code and force it to run in different orderings by adding calls to methods like Object.wait(), Object.sleep(), Object.yield() and Object.priority().
Each of these methods can affect the order of execution, thereby increasing the odds
of detecting a flaw. It’s better when broken code fails as early and as often as possible.
### Hand-coded
You can insert calls to wait(), sleep(), yield(), and priority() in your code by hand. It might be just the thing to do when you’re testing a particularly thorny piece of code.

```java
public synchronized String nextUrlOrNull() {
	if(hasNext()) {
		String url = urlGenerator.next();
		Thread.yield(); // inserted for testing.
		updateHasNext();
		return url;
	}
	return null;
}
```
The inserted call to yield() will change the execution pathways taken by the code and
possibly cause the code to fail where it did not fail before. If the code does break, it was
not because you added a call to yield(). Rather, your code was broken and this simply made the failure evident.
### Automated
You could use tools like an Aspect-Oriented Framework, CGLIB, or ASM to programmatically instrument your code. 
- Use a class with a single method add calls to this in various places within your code:
* use a simple aspect that randomly selects among doing nothing, sleeping, or yielding.
* Make sure when you are testing your thread-aware code, you are only testing it and nothing else. This suggests that your thread-aware code should be small and focused.

To keep concurrent systems clean, thread management should be kept to a few,
well-controlled places. What’s more, any code that manages threads should do nothing
other than thread management. Why? If for no other reason than that tracking down concurrency issues is hard enough without having to unwind other nonconcurrency issues at the same time.
- according to the Java Memory model, assignment to a 32-bit value is uninterruptable.
- according to the JVM specification, assignment to any 64-bit value requires two 32-bit assignments.

- Frame—Every method invocation requires a frame. The frame includes the return address, any parameters passed into the method and the local variables defined in the method. This is a standard technique used to define a call stack, which is used by modern languages to allow for basic function/method invocation and to allow for recursive invocation.
- Local variable—Any variables defined in the scope of the method. All nonstatic methods have at least one variable, **this**, which represents the current object, the object that received the most recent message (in the current thread), which caused the method invocation.
- Operand stack—Many of the instructions in the Java Virtual Machine take parameters. The operand stack is where those parameters are put. The stack is a standard last-in, first-out (LIFO) data structure.
**Compare and Swap (CAS).** 
This operation is analogous to optimistic locking in databases, whereas
the synchronized version is analogous to pessimistic locking.The synchronized keyword always acquires a lock, even when a second thread is not
trying to update the same value. Even though the performance of intrinsic locks has
improved from version to version, they are still costly. The nonblocking version starts with the assumption that multiple threads generally do
not modify the same value often enough that a problem will arise. Instead, it efficiently
detects whether such a situation has occurred and retries until the update happens success-fully. This detection is almost always less costly than acquiring a lock, even in moderate to high contention situations
When a method attempts to update a shared variable, the CAS operation verifies that
the variable getting set still has the last known value. If so, then the variable is changed. If not, then the variable is not set because another thread managed to get in the way. The method making the attempt (using the CAS operation) sees that the change was not made and retries
There are some classes that are inherently not thread safe. Here are a few examples:
- SimpleDateForma
- Database connections
- Containers in java.util
- Servlets
## Dependencies Between Methods Can Break Concurrent Code
You have three options:
* Tolerate the failure - Sometimes you can set things up such that the failure causes no harm. For example, the above client could catch the exception and clean up. Frankly, this is a bit sloppy. It’s rather like cleaning up memory leaks by rebooting at midnight.
* Solve the problem by changing the client: client-based locking third-party tools. This strategy is risky because all programmers who use the server must remember to lock it before using it and unlock it when done. 
	// Client-based locking really blows.
* Solve the problem by changing the server, which additionally changes the client: server-based locking
* In general you should prefer server-based locking for these reasons:
	- It reduces repeated code
	- It allows for better performance—You can swap out a thread-safe server for a non-thread safe one in the case of single-threaded deployment, thereby avoiding all overhead.
	- It reduces the possibility of error—All it takes is for one programmer to forget to lock properly.
	- It enforces a single policy
	- It reduces the scope of the shared variables
What if you do not own the server code?
- Use an ADAPTER to change the API and add locking
- OR better yet, use the thread-safe collections with extended interfaces

It is always better to synchronize as little as possible as opposed to synchronizing as much as possible.

### Deadlock
All four of these conditions must hold for deadlock to be possible. Break any one of these conditions and deadlock is not possible.
To really solve the problem of deadlock, we need to understand what causes it. There are four conditions required for deadlock to occur:
- Mutual exclusion
Mutual exclusion occurs when multiple threads need to use the same resources and those resource. 
Cannot be used by multiple threads at the same time.
Are limited in number.
**Breaking**
sidestep the mutual exclusion condition. You might be able to do this by
- Using resources that allow simultaneous use, for example, AtomicInteger.
- Increasing the number of resources such that it equals or exceeds the number of competing threads
- Checking that all your resources are free before seizing any.

- Lock & wait - Once a thread acquires a resource, it will not release the resource until it has acquired allof the other resources it requires and has completed its work.
- **Breaking** if you refuse to wait. Check each resource before youseize it, and release all resources and start over if you run into one that’s busy. 
  This approach introduces several potential problems:
	• Starvation—One thread keeps being unable to acquire the resources it needs (maybe it has a unique combination of resources that seldom all become available).
	• Livelock—Several threads might get into lockstep and all acquire one resource and then release one resource, over and over again. This is especially likely with simplistic CPU scheduling algorithms (think embedded devices or simplistic hand-written thread balancing algorithms).Both of these can cause poor throughput. The first results in low CPU utilization, whereas the second results in high and useless CPU utilization. As inefficient as this strategy sounds, it’s better than nothing. It has the benefit that it can almost always be implemented if all else fails.
* No preemption - One thread cannot take resources away from another thread. Once a thread holds a resource, the only way for another thread to get it is for the holding thread to release it.
Breaking
allow threads to take resources away from
other threads. This is usually done through a simple request mechanism. When a thread
discovers that a resource is busy, it asks the owner to release it. If the owner is also waiting
for some other resource, it releases them all and starts over. 
This is similar to the previous approach but has the benefit that a thread is allowed to
wait for a resource. This decreases the number of startovers. Be warned, however, that
managing all those requests can be tricky.

• Circular wait - This is also referred to as the deadly embrace. Imagine two threads, T1 and T2, and two
resources, R1 and R2. T1 has R1, T2 has R2. T1 also requires R2, and T2 also requires R1.
Breaking
if all threads can agree on a global ordering of resources and if they
all allocate resources in that order, then deadlock is impossible.
For most systems it requires
no more than a simple convention agreed to by all parties.
##### Testing Multithreaded Code
how can we write tests that will demonstrate failures in more complex code? How
will we be able to discover if our code has failures when we do not know where to look?
Here are a few ideas:
• Monte Carlo Testing. Make tests flexible, so they can be tuned. Then run the test over 
and over—say on a test server—randomly changing the tuning values. If the tests ever 
fail, the code is broken. Make sure to start writing those tests early so a continuous 
integration server starts running them soon. By the way, make sure you carefully log 
the conditions under which the test failed.
• Run the test on every one of the target deployment platforms. Repeatedly. Continu-ously. The longer the tests run without failure, the more likely that
– The production code is correct or
– The tests aren’t adequate to expose problems.
• Run the tests on a machine with varying loads. If you can simulate loads close to a 
production environment, do so.
# Smells and Heuristics
### Functions
- Functions should have a small number of arguments. No argument is best, followed by one, two, and three.
- Output arguments are counterintuitive. Readers expect arguments to be inputs, not outputs. If your function must change the state of something, have it change the state of the object it is called on.
- Boolean(Flag) arguments loudly declare that the function does more than one thing. They are confusing and should be eliminated.
- Following “The Principle of Least Surprise,”2 any function or class should implement the behaviors that another programmer could reasonably expect
### General
- Every boundary condition, every corner case, every quirk and exception represents something that can confound an elegant and intuitive algorithm. Don’t rely on your intuition. Look for every boundary condition and write a test for it.
- Every time you see duplication in the code, it represents a missed opportunity for abstraction. That duplication could probably become a subroutine or perhaps another class outright.
	- A more subtle form is the switch/case or if/else chain that appears again and again in various modules, always testing for the same set of conditions. These should be replaced with polymorphism.
	- the modules that have similar algorithms, but that don’t share similar lines of code. This is still duplication and should be addressed by using the    TEMPLATE METHOD, or STRATEGY pattern.
- It is important to create abstractions that separate higher level general concepts from lower level detailed concepts. Sometimes we do this by creating abstract classes to hold the higher level concepts
	- We want all the lower level concepts to be in the derivatives and all the higher level concepts to be in the base class.
- The most common reason for partitioning concepts into base and derivative classes is so that the higher level base class concepts can be independent of the lower level derivative class concepts. Therefore, when we see base classes mentioning the names of their derivatives, we suspect a problem. In general, base classes should know nothing about their derivatives.
- Well-defined modules have very small interfaces that allow you to do a lot with a little. 
	- Poorly defined modules have wide and deep interfaces that force you to use many different gestures to get simple things done.
	- A poorly defined interface provides lots of functions that you must call, so coupling is high.
	- learn to limit what they expose at the interfaces of their classes and modules. The fewer methods a class has, the better. The fewer variables a function knows about, the better. The fewer instance variables a class has, the better.
	- Hide your data. Hide your utility functions. Hide your constants and your temporaries. Don’t create classes with lots of methods or lots of instance variables. Don’t create lots of protected variables and functions for your subclasses. Concentrate on keeping interfaces very tight and very small. Help keep coupling low by limiting information.
- Variables and function should be defined close to where they are used.
	- Local variables should be declared just above their first usage and should have a small vertical scope.
	- Private functions should be defined just below their first usage. Private functions belong to the scope of the whole class, but we’d still like to limit the vertical distance between the invocations and definitions. Finding a private function should just be a matter of scanning downward from the first usage.
- If you do something a certain way, do all similar things in the same way. This goes back to the principle of least surprise. Be careful with the conventions you choose, and once chosen, be careful to continue to follow them.
- Variables that aren’t used, functions that are never called, comments that add no information, and so forth. All these things are clutter and should be removed. Keep your source files clean, well organized, and free of clutter.
- Things that don’t depend upon each other should not be artificially coupled.In general an artificial coupling is a coupling between two modules that serves no direct purpose. It is a result of putting a variable, constant, or function in a temporarily convenient, though inappropriate, location.
- The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes. When a method uses accessors and mutators of some other object to manipulate the data within that object, then it envies the scope of the class of that other object.
```java
public class HourlyPayCalculator {
	public Money calculateWeeklyPay(HourlyEmployee e) {
		int tenthRate = e.getTenthRate().getPennies();
		int tenthsWorked = e.getTenthsWorked();
		int straightTime = Math.min(400, tenthsWorked);
		int overTime = Math.max(0, tenthsWorked - straightTime);
		int straightPay = straightTime * tenthRate;
		int overtimePay = (int)Math.round(overTime*tenthRate*1.5);
		return new Money(straightPay + overtimePay);
	}
}
```
The calculateWeeklyPay method reaches into the HourlyEmployee object to get the data on which it operates. The calculateWeeklyPay method envies the scope of HourlyEmployee. It “wishes” that it could be inside HourlyEmployee.

 Sometimes, however, Feature Envy is a necessary evil
 ```java
public class HourlyEmployeeReport {
	private HourlyEmployee employee ;

	public HourlyEmployeeReport(HourlyEmployee e) {	
		this.employee = e;
	}
	
	String reportHours() {
		return String.format(
			"Name: %s\tHours:%d.%1d\n",
			employee.getName(),
			employee.getTenthsWorked()/10,
			employee.getTenthsWorked()%10);
	}
}
```
Clearly, the reportHours method envies the HourlyEmployee class. On the other hand, we don’t want HourlyEmployee to have to know about the format of the report.
- There is hardly anything more abominable than a dangling false argument at the end of a function call. Not only is the purpose of a selector argument difficult to remember, each selector argument combines many functions into one. Selector arguments are just a lazy way to avoid splitting a large function into several smaller functions.Of course, selectors need not be boolean. They can be enums, integers, or any other type of argument that is used to select the behavior of the function. In general it is better to have many functions than to pass some code into a function to select the behavior.
- Code should be placed where a reader would naturally expect it to be. The PI constant should go where the trig functions are declared. The OVERTIME_RATE constant should be declared in the HourlyPayCalculator class.
- Sometimes, however, we write static functions that should not be static. For example, consider:
`HourlyPayCalculator.calculatePay(employee, overtimeRate).`
Again, this seems like a reasonable static function. It doesn’t operate on any particular object and gets all it’s data from it’s arguments. However, there is a reasonable chance that we’ll want this function to be polymorphic. We may wish to implement several different algorithms for calculating hourly pay. So in this case the function should not be static. It should be a nonstatic member function of Employee. In general you should prefer nonstatic methods to static methods. When in doubt, make the function nonstatic. If you really want a function to be static, make sure that there is no chance that you’ll want it to behave polymorphically.
- It is hard to overdo this. More explanatory variables are generally better than fewer. It is remarkable how an opaque module can suddenly become transparent simply by breaking the calculations up into well-named intermediate values.
- If one module depends upon another, that dependency should be physical, not just logical. The dependent module should not make assumptions (in other words, logical dependencies) about the module it depends upon. Rather it should explicitly ask that module for all the information it depends upon.
- Prefer Polymorphism to If/Else or Switch/Case. switch statements are probably appropriate in the parts of the system where adding new functions is more likely than adding new types.cases where functions are more volatile than types are relatively rare. So every switch statement should be suspect. There may be no more than one switch statement for a given type of selection. The cases in that switch statement must create polymorphic objects that take the place of other such switch statements in the rest of the system.
- Replace Magic Numbers with Named Constants 
- Enforce design decisions with structure over convention. Naming conventions are good, but they are inferior to structures that force compliance. No one is forced to implement the switch/case statement the same way each time; but the base classes do enforce that concrete classes have all abstract methods implemented.
- Encapsulate Conditionals Boolean logic is hard enough to understand without having to see it in the context of an if or while statement. Extract functions that explain the intent of the conditional. For example: `if (shouldBeDeleted(timer))` is preferable to `if (timer.hasExpired() && !timer.isRecurrent())`
	- Negatives are just a bit harder to understand than positives. So, when possible, conditionals should be expressed as positives.
- It is often tempting to create functions that have multiple sections that perform a series of operations. Functions of this kind do more than one thing, and should be converted into many smaller functions, each of which does one thing.
- Temporal couplings are often necessary, but you should not hide the coupling. Structure the arguments of your functions such that the order in which they should be called is obvious. You could expose the temporal coupling by creating a bucket brigade. Each function produces a result that the next function needs, so there is no reasonable way to call them out of order.
- The statements within a function should all be written at the same level of abstraction, which should be one level below the operation described by the name of the function.
- Keep Configurable Data at High Levels. If you have a constant such as a default or configuration value that is known and expected at a high level of abstraction, do not bury it in a low-level function.
- Avoid Transitive Navigation In general we don’t want a single module to know much about its collaborators. More specifically, if A collaborates with B, and B collaborates with C, we don’t want modules that use A to know about C. (For example, we don’t want a.getB().getC().doSomething();.)
### Names
- Choose Names at the Appropriate Level of Abstraction Don’t pick names that communicate implementation; choose names the reflect the level of abstraction of the class or function you are working in. 
- Use Standard Nomenclature Where PossibleNames are easier to understand if they are based on existing convention or usage. For example, if you are using the DECORATOR pattern, you should use the word Decorator in the names of the decorating classes.
* the longer the scope of the name, the longer and more precise the name should be.
* Names should not be encoded with type or scope information.
* Names should describe everything that a function, variable, or class is or does. Don’t hide side effects with a name
### Tests
- A test suite should test everything that could possibly break. The tests are insufficient so long as there are conditions that have not been explored by the tests 
- Coverage tools reports gaps in your testing strategy. They make it easy to find modules, classes, and functions that are insufficiently tested.
* Take special care to test boundary conditions. 
* Bugs tend to congregate. When you find a bug in a function, it is wise to do an exhaustive test of that function. You’ll probably find that the bug was not alone.
* A slow test is a test that won’t get run. When things get tight, it’s the slow tests that will be dropped from the suite. So do what you must to keep your tests fast.


