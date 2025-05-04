#### Managing Objects
- Use builder when faced with many constructor parameters
- Always override together and follow hashCode and equals contract
- Always override toString for data objects to make debugging easier
- Prefer dependency injection to hardwiring resources
---
- Avoid creating unnecessary objects 
	- Don't use Strings for concatenation - use StringBuilder
	- Avoid redundant auto-boxing
	```
	private static long sum() {  
	    Long sum = 0L;  
	    for (long i = 0; i <= Integer.MAX_VALUE; i++)  
	        sum += i;  
	    return sum;  
	}
	```
---
- Override clone with caution - as you would implement constructor
	- Never execute overriden field in clone under construction - it may lead to corruption of original
	- Immutable classes should never provide a clone method - it would encourage wasteful copying
	- A better alternative to copy method - **сору constructor**
---
- Make mutability of the object as minimal as possible, 
	- Immutable objects are inherently thread-safe; they require no synchronization.
	- Immutable objects provide failure atomicity for free
- Favor composition over inheritance
	- Unlike method invocation, inheritance violates encapsulation
		- When designing class for inheritance make sure that the class never invokes its overridable methods.
		- Eliminate classes self use of overridable methods. If it’s not possible you must document all of its self use.
- Prefer interfaces to abstract classes
	- default methods allow backward compatibility
	- better suited for defining class design
	- allows multiple inheritance
- Avoid using raw types, as it will require manual cast and more error prone
---
#### Lambdas
- Use Lambdas in favor of anonymous classes, when possible use reference methods
	- They are cleaner, easier to read and write
	- Have less overhead than creating a new class instance for every anonymous class.
- Use Standard Functional interfaces when possible, otherwise define own to provide more readability
---
#### Streams
- Use streams for 
	- Processing Collections and Arrays
	- Transforming Data
- Don't use Streams
	- With Complex/Nested Logic
	- Mutating external state
- Prefer Collection to Stream as return type
- Use Parallel streams 
	-  On streams over `ArrayList`, `HashMap`, `HashSet`, and `ConcurrentHashMap` instances; arrays for most Performance gains.
	- Only on stateless non-interfering tasks, otherwise it won't improve performance and might give incorrect results
---
#### Methods
- Check parameters for validity as soon as possible otherwise it might be harder to find a root cause
- Return empty collections or arrays, not nulls
- Refer to objects by their interfaces as it allows more flexibility
---
Do not overload methods to take different interfaces of same hierarchy in the same argument position
Differently from overriding it would invoke method based on compile-time information
  ```
	public class CollectionClassifier {  
	    public static String classify(Set<?> s) {  
	        return "Set";  
	    }  
	  
	    public static String classify(List<?> lst) {  
	        return "List";  
	    }  
	  
	    public static String classify(Collection<?> c) {  
	        return "Unknown Collection";  
	    }  
	  
	    public static void main(String[] args) {  
	        Collection<?>[] collections = {  
	            new HashSet<String>(),  
	            new ArrayList<BigInteger>(),  
	            new HashMap<String, String>().values()  
	        };  
	  
	        for (Collection<?> c : collections)  
	         System.out.println(classify(c)); // `Unknown Collection` x3
	    }  
	}
``` 

---
Consider using a custom serialized form when object’s physical representation is not identical to its logical content.
``` 
		// Not suitable for default serialization
		public final class StringList implements Serializable {  
		    private int size = 0;  
		    private Entry head = null;  
		  
		    private static class Entry implements Serializable {  
		        String data;  
		        Entry  next;  
		        Entry  previous;  
		    }  

		    private void writeObject(ObjectOutputStream s) {  
		        s.writeInt(size);  
		        for (Entry e = head; e != null; e = e.next)  
		            s.writeObject(e.data);  
		    } 
		}
```
- Using the default serialized form when an object’s physical representation differs substantially from its logical data permanently ties the exported API to the current internal representation.
- Regardless of what serialized form you choose, declare an explicit serial version UID in every serializable class you write.
- Do not change the serial version UID unless you want to break compatibility with all existing serialized instances of a class.
---
#### Exceptions 
- Avoid unnecessary use of checked exceptions
	- The easiest way to eliminate a checked exception is to return an _optional_ of the desired result type
	- You can also turn a checked exception into an unchecked exception by breaking the method that throws the exception into two methods, the first of which returns a `boolean` indicating whether the exception would be thrown.
``` 
// Invocation with checked exception 
try {  
    obj.action(args);  
} catch (TheCheckedException e) {  
    ... // Handle exceptional condition  
}
```

```
if (obj.actionPermitted(args)) {  
    obj.action(args);  
} else {  
    ... // Handle exceptional condition  
}
```
---
- Strive for failure atomicity - a failed method invocation should leave the object in the state that it was in prior to the invocation.
```
public Object pop() {  
    //if (size == 0)
    //    throw new EmptyStackException();
    Object result = elements[--size];// ArrayIndexOutOfBoundsException 
    elements[size] = null; 
    return result;  
}
```
---
- Throw exceptions appropriate to the abstraction 
	- higher layers should catch lower-level exceptions
	- in their place, throw exceptions that can be explained in terms of the higher-level abstraction.
	-  Use _exception chaining_ to make debugging easier
```
public E get(int index) {  
    ListIterator<E> i = listIterator(index);  
    try {  
        return i.next();  
    } catch (NoSuchElementException e) {  
        throw new IndexOutOfBoundsException("Index: " + index);  
    }  
}
```
---
### Concurrency
 - Avoid excessive synchronization
 - Avoid using Thread.yield() as it's behavior is not guaranteed. It's a hint to the scheduler that the current thread is willing to yield its execution time.
 - Use `ConcurrentHashMap` in preference to `Collections.synchronizedMap`
---
- Synchronize access to shared mutable data
	- Synchronization is not guaranteed to work unless both read and write operations are synchronized.
	- Try to confine mutable data usage to a single thread
---
- Document thread safety with levels
	- **Immutable** — Instances of this class appear constant.
	- **Unconditionally thread-safe** — Instances of this class are mutable, but the class has sufficient internal synchronization that its instances can be used concurrently without the need for any external synchronization.
	- **Conditionally thread-safe** — Like unconditionally thread-safe, except that some methods require external synchronization for safe concurrent use.
	- **Not thread-safe** — Instances of this class are mutable. To use them concurrently, clients must surround each method invocation (or invocation sequence) with external synchronization
---
- Use lazy initialization judiciously
	- Under most circumstances, normal initialization is preferable to lazy initialization.
	- If you need to use lazy initialization for performance on a static field, use the lazy initialization holder class idiom
	  ``` 
		// Lazy initialization holder class idiom for static fields 
		
		private static class FieldHolder {  
		    static final FieldType field = computeFieldValue();  
		}  

		private static FieldType getField() { 
			return FieldHolder.field;
		}
		```
	- If you need to use lazy initialization for performance on an instance field, use the double-check idiom
	  ```
		// Double-check idiom for lazy initialization of instance fields
		
		private volatile FieldType field;  
		
		private FieldType getField() {  
		    FieldType result = field;  
			 if (result == null) {  // First check (no locking)  
		        synchronized(this) {  
		            if (field == null)  // Second check (with locking)  
		                field = result = computeFieldValue();  
		        }  
		    }  
		    return result;  
		}
		```
	- If an instance field that can tolerate repeated initialization. You could use ingle-check idiom
	  ```
		private volatile FieldType field;
		
		private FieldType getField() {
		    FieldType result = field;  
		    if (result == null)  
		        field = result = computeFieldValue();  
		    return result;  
		}
	    ```