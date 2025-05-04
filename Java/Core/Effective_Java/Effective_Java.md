# Creating and Destroying Objects
Static factory method(not the same as Factory method pattern)\]
``` Java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE
}
```
###### Pros
1.  Static method has name, constructor - don't
	1. Easier to express what we are creating
	2. Ability to create constructors with the same signature
2.  They are not required to create new object on invocation
	1. Immutable classes could use preconstructed instances, even avoid creating new objects(this is used by [[../../../Software_Architecture/Patterns/Flyweight]] pattern)
	3. Creation of instance-controlled classes
3. They could return any subtype of this class
	1. Api could return objects without making their classes public(interface based frameworks)
	2. We could create companion classes to group and avoid exposing api(like Collections)
4. Class of returning parameter may vary from call to call because of different parameters
5. Class of returning object may not exist at the moment of writing method
	1. We could plug implementation later like JDBC drivers
	2. Such methods form basis of [[../../../Software_Architecture/Service provider framework]]

- Item 1: Consider static factory methods instead of constructors
	Pros
	Cons
	1. Classes without public on protected constructors can't be subclassed
	2. Harden to find than constructors
	
	**Use builder when face many constructor parameters**
	
	Covariant return typing - technique wherein a subclas method is declared to return a subtype of the return type declared in the super class.
	```Java
	Class builder <t extends builder >{
	
	Public T get vall (){
	
	Return self ();
	
	}
	
	Abstract T self ();
	
	}
	```
	
	Always Prefer to use singleton created with enum it is the safiest ones but you cannt use extends, only implements.
	Hatch out for unintentional unboxing as it creates redundant objects
- Item 2: Consider a builder when faced with many constructor parameters
- Item 3: Enforce the singleton property with a private constructor or an enum type
- Item 4: Enforce noninstantiability with a private constructor
- Item 5: Prefer dependency injection to hardwiring resources
- Item 6: Avoid creating unnecessary objects
- Item 7: Eliminate obsolete object references
	It stack grows and then shrinks the objects that were popped off the stack will not be garbage collected this is because stack maintains **obsolete references** to these objects. An obsolete reference-is the reference that will never be dereferenced
	Fullyfy the references to get rid of these objects
	
	The other common source of memory leak is listeners and callbacks if you register callback but don't dragster them they will accumulate• this could be found using heap profiler

- Item 8: Avoid finalizers and cleaners
- Item 9: Prefer try-with-resources to try-finally
# Methods Common to All Objects
- Item 10: Obey the general contract when overriding equals
- Item 11: Always override hashCode when you override equals
	Overneding equals
	
	Always pay attention when overriding equals for subclass,you May Break transitivity and symmetrg Rules Because if you leave old method as ist will start to work falsely with new subclass.
	
	Favor composition over inheritance.
	
	In Most cases it would be easier to extend and maintain classes with composition
	
	Instance of is specified to return false if first argument is null
	
	How to implement equals,
	
	1. Use == to check of the object is the same
	2. Use instance of to check if object has connect type
	3. Last the arguments to a correct type
	4. For each significant field check is that field of the argument matches to the corresponding of this object
	
	1. For the primitive fields except float and double use==
	2. For float and double use float.compare and double.compare
	3. For nullable objects use objects.equals
	4. Compare first fields that are more likely to differ - to improve performance
	5. You may use derived vales to speed up comparation
	
	6. After finishing check that method
	
	1. Symmetric
	2. Transitive
	3. Consistent
	
	8. Override hash code
	
	Overriding hash code
	
	Use the same fields as in equals ; multiplication will make order of the fields important
	
	For immutable classes, if computation is hard, you may cachethe value
- Item 12: Always override toString
- Item 13: Override clone judiciously
	1. Call super.clone
	2. If object has only primitive and immutable fields - that is it
	3. You could return actual type instead of object as Java supports covariant return types
	4. Never execute overrode field in clone under construction - it may lead to corruption of original
	Immutable classes should never provide a clone method - it would encourage wasteful copying
	
	The clone method acts as the constructor; ensure that it won't harm initial object.
	
	A better alternative to copy method - **сору constructor or copy factory**
	
- Item 14: Consider implementing Comparable
	If you are creating a value class with natural ordering consider implementing comparable. Implementation must ensure;
	1. Sign of x.CompareTo(y)== -y.compareTo(x)
	2. Transitivity
	3. X.compareTo(y) == 0 implies that x.compareTo(z) == y.compareTo(z)
	4. Recommended but not required (x.compareTo(y) == 0) == (x.equals(y))
	Consider using comparator construction methods instead of self written 
	
# Classes and Interfaces

- Item 15: Minimize the accessibility of classes and members
- Item 16: In public classes, use accessor methods, not public fields
	Java g modules
	
	If you place a modules jar file on app class path instead of module's the package in the module will revert to it's non modular behaviour
	
	It is normal to expose public fields for the package private and private nested classes
- Item 17: Minimize mutability
	How to create immutable class
	1. Don't provide methods that modify object's state
	2. Ensure that class can't be extended
	3. Make all fields final
	4. Make all fields private
	5. Ensure exclusive access to any mutable field mare defensive copies in constructions. Accessors and readobject methods
- Item 18: Favor composition over inheritance
	When designing class for inheritance make sure that the class never invokes its overridable methods.
	Eliminate classes self use of overridable methods. If it’s not possible you must document all of its self use.
- Item 20: Prefer interfaces to abstract classes
	Existing classes can easily be retrofitted to implement a new interface
	Interfaces are ideal for defining mixins
	Mixin - type that class can implement in addition to its primary type  to declare that it provides some optional behavior. Ex. Comparable - is a mixin. It’s called mixin as it allows optional functionality to be mixed in to the type’s primary functionality
	If you export a nontrivial interface, you should consider writing a skeleton implementation to go with it.
	Skeleton implementation - abstract class implementation for an interface and designed for inheritance , it’s simplest possible working implementation
- Item 21: Design interfaces for posterity
- Item 22: Use interfaces only to define types
- Item 23: Prefer class hierarchies to tagged classes
- Item 24: Favor static member classes over nonstatic
  One of common use of nonstatic is to define an Adapter that allows an instance of the outer class to be viewed as an instance of some unrelated class, like keySet or entrySet or iterators in List and Set.
  If you create a member class that doesn't require access to an enclosing instance - always use static nested class. As otherwise they will require additional memory to store them and may lead to memory leak.
  anonymous classes could be used for implementation of static factory methods
- Item 25: Limit source files to a single top-level class
# Generics
- Item 26: Don’t use raw types
	You loose type safety if you use raw types such as List, but not if you use a parametrized type, such as List\<Object\>. Or use wildcard if it's possible - it would be safe
	You could use instanceof only with unbounded wildcards
	```Java
	if(o instanceof Set){
	    Set<?> s = (Set<?>)o;
	}
	```

	```Java
	Set<Object> // parametrized type representing a set that can contain objects of any type
	Set<?> // parametrized type representing a set that can contain objects of some unknown type
	<E extends Number> //Bounded type parameter
	List<? extends Number> // Bounded wildcard type 
	<?> //Unbounded wildcard type
	```
	It is permissible, though relatively rare, for a type parameter to be bounded by some expression involving that type parameter itself. This is what’s known as a recursive type bound.   A common use of recursive type bounds is in connection with the Comparable interface,
	```Java
			<T extends Comparable<T>> //Recursive type bound
	```
	The type parameter T defines the type to which elements of the type implementing `Comparable<T> `can be compared. In practice, nearly all types can be compared only to elements of their own type. So, for example, `String` implements `Comparable<String>`, Integer implements `Comparable<Integer>`, and so on.
	
	The type bound `<E extends Comparable<E>> `may be read as “any type E that can be compared to itself,” which corresponds more or less precisely to the notion of mutual comparability.
- Item 27: Eliminate unchecked warnings
- Item 28: Prefer lists to arrays
- Item 29: Favor generic types
- Item 30: Favor generic methods
- Item 31: Use bounded wildcards to increase API flexibility
  The type of the input parameter to pushAll should not be “Iterable of E” but “Iterable of some subtype of E,” and there is a wildcard type that means precisely that: `Iterable<? extends E>.` 
  ```Java
	// Wildcard type for a parameter that serves as an E producer
	public void pushAll(Iterable<? extends E> src) {
	    for (E e : src)
	        push(e)
	}
	```
	The type of the input parameter to popAll should not be “collection of E” but “collection of some supertype of E”. Again, there is a wildcard type that means precisely that:` Collection<? super E>`. 
	```Java
	// Wildcard type for parameter that serves as an E consumer
	public void popAll(Collection<? super E> dst) {
	    while (!isEmpty())
	        dst.add(pop());
	}
	```
	For maximum flexibility, use wildcard types on input parameters that represent producers or consumers. If an input parameter is both a producer and a consumer, then wildcard types will do you no good: you need an exact type match, which is what you get without any wildcards.
	>**PECS stands for producer-extends, consumer-super.*** In other words, if a parameterized type represents a T producer, use `<? extends T>`; if it represents a T consumer, use` <? super T>`

	The Chooser constructor in Item 28 has this declaration:
	`public Chooser(Collection<T> choices)`
	This constructor uses the collection choices only to produce values of type T (and stores them for later use), so its declaration should use a wildcard type that extends T. Here’s the resulting constructor declaration:
	```Java
	// Wildcard type for parameter that serves as a T producer
	public Chooser(Collection<? extends T> choices)
	```
	Do not use bounded wildcard types as return types:
	Rather than providing additional flexibility for your users, it would force them to use wildcard types in client code.

	Here are two possible declarations for a static method to swap two indexed items in a list. The first uses an unbounded type parameter (Item 30) and the second an unbounded wildcard:
	```Java
	// Two possible declarations for the swap method
	public static <E> void swap(List<E> list, int i, int j);
	public static void swap(List<?> list, int i, int j);
	```
	In a public API, the second is better because it’s simpler. You pass in a list—any list—and the method swaps the indexed elements. There is no type parameter to worry about. As a rule, if a type parameter appears only once in a method declaration, replace it with a wildcard
	
	The problem is that the type of list is `List<?>`, and you can’t put any value except null into a `List<?>`. Fortunately, there is a way to implement this method without resorting to an unsafe cast or a raw type. The idea is to write a private helper method to capture the wildcard type. The helper method must be a generic method in order to capture the type. Here’s how it looks:
	```Java
	public static void swap(List<?> list, int i, int j) {
	    swapHelper(list, i, j);
	}
	
	// Private helper method for wildcard capture
	private static <E> void swapHelper(List<E> list, int i, int j) {
	    list.set(i, list.set(j, list.get(i)));
	}
	```
- Item 32: Combine generics and varargs judiciously
  Combining Generics and varargs may lead to a Heap pollution due to varargs being array
   Heap pollution occurs when a variable of a parameterized type refers to an object that is not of that type. It can cause the compiler’s automatically generated casts to fail, violating the fundamental guarantee of the generic type system.
   The SafeVarargs annotation constitutes a promise by the author of a method that it is typesafe.
   If the varargs parameter array is used only to transmit a variable number of arguments from the caller to the method—which is, after all, the purpose of varargs—then the method is safe.
   ```Java
	// UNSAFE - Exposes a reference to its generic parameter array!
	static <T> T[] toArray(T... args) {
	    return args;
	}
	```
	It is unsafe to give another method access to a generic varargs parameter array
	```Java
	// Safe method with a generic varargs parameter
	@SafeVarargs
	static <T> List<T> flatten(List<? extends T>... lists) {
	List<T> result = new ArrayList<>();
	
	for (List<? extends T> list : lists)
		result.addAll(list);
		return result;
	}
	```
	Use @SafeVarargs on every method with a varargs parameter of a generic or parameterized type
	Generic varargs methods is safe if:
	1. it doesn’t store anything in the varargs parameter array
	2. it doesn’t make the array (or a clone) visible to untrusted code
- Item 33: Consider typesafe heterogeneous containers
	The normal use of generics, exemplified by the collections APIs, restricts you to a fixed number of type parameters per container. You can get around this restriction by placing the type parameter on the key rather than the container. You can use Class objects as keys for such typesafe heterogeneous containers. A Class object used in this fashion is called a type token. You can also use a custom key type.
	```Java
	// Typesafe heterogeneous container pattern - implementation
	public class Favorites {
		private Map<Class<?>, Object> favorites = new HashMap<>();
		
		public <T> void putFavorite(Class<T> type, T instance) {
			favorites.put(Objects.requireNonNull(type), instance);
		}
		
		public <T> T getFavorite(Class<T> type) {
			return type.cast(favorites.get(type));
		}
	}
	```
	
# Enums and Annotations
- Item 34: Use enums instead of int constants
  Enum types have an automatically generated valueOf(String) method that translates a constant’s name into the constant itself. If you override the toString method in an enum type, consider writing a fromString method to translate the custom string representation back to the corresponding enum.
  
  To be forced to to implement logic each time you add an enum constant, you could move the overtime pay computation into a nested enum, and to pass an instance of strategy enum to the constructor for the initial enum. The initial enum then delegates the calculation to the strategy enum, eliminating the need for a switch statement or constant-specific method implementation. While this pattern is less concise than the switch statement, it is safer and more flexible:
  ```Java
	 // The strategy enum pattern
	 enum PayrollDay {
		 MONDAY(WEEKDAY), TUESDAY(WEEKDAY), WEDNESDAY(WEEKDAY),
		 THURSDAY(WEEKDAY), FRIDAY(WEEKDAY),
		 SATURDAY(WEEKEND), SUNDAY(WEEKEND);
		 
		 private final PayType payType;

		PayrollDay(PayType payType) { this.payType = payType; }
		
		int pay(int minutesWorked, int payRate) {
			return payType.pay(minutesWorked, payRate);
		}
		// The strategy enum type

>     private enum PayType {

>         WEEKDAY {

>             int overtimePay(int minsWorked, int payRate) {

>                 return minsWorked <= MINS_PER_SHIFT ? 0 :

>                   (minsWorked - MINS_PER_SHIFT) * payRate / 2;

>             }

>         },

>         WEEKEND {

>             int overtimePay(int minsWorked, int payRate) {

>                 return minsWorked * payRate / 2;

>             }

>         };

> 

>         abstract int overtimePay(int mins, int payRate);

>         private static final int MINS_PER_SHIFT = 8 * 60;

> 

>         int pay(int minsWorked, int payRate) {

>             int basePay = minsWorked * payRate;

>             return basePay + overtimePay(minsWorked, payRate);

>         }

>     }

> }
	```



### **Strategy enum pattern** 

If switch statements on enums are not a good choice for implementing constant-specific behavior on enums, what are they good for? Switches on enums are good for augmenting enum types with constant-specific behavior. For example, suppose the Operation enum is not under your control and you wish it had an instance method to return the inverse of each operation. You could simulate the effect with the following static method:
```Java
> // Switch on an enum to simulate a missing method

> public static Operation inverse(Operation op) {

>     switch(op) {

>         case PLUS:   return Operation.MINUS;

>         case MINUS:  return Operation.PLUS;

>         case TIMES:  return Operation.DIVIDE;

>         case DIVIDE: return Operation.TIMES;

> 

>         default:  throw new AssertionError("Unknown op: " + op);

>     }

> }
> 
```

- Item 35: Use instance fields instead of ordinals
  Never derive a value associated with an enum from its ordinal; store it in an instance field instead
- Item 36: Use EnumSet instead of bit fields
  Just because an enumerated type will be used in sets, there is no reason to represent it with bit fields, you could use EnumSet
  it is rarely appropriate to use ordinals to index into arrays: use EnumMap instead.
- Item 37: Use EnumMap instead of ordinal indexing
  While you cannot write an extensible enum type, you can emulate it by writing an interface to accompany a basic enum type that implements the interface. This allows clients to write their own enums (or other types) that implement the interface.
  ```
	// Annotation type with an array parameter 
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
	public @interface ExceptionTest {
		Class<? extends Exception>[] value();
	}
	```
	The syntax for array parameters in annotations is flexible. It is optimized for single-element arrays. All of the previous ExceptionTest annotations are still valid with the new array-parameter version of ExceptionTest and result in single-element arrays.
- Item 38: Emulate extensible enums with interfaces
- Item 39: Prefer annotations to naming patterns
  As of Java 8, there is another way to do multivalued annotations. Instead of declaring an annotation type with an array parameter, you can annotate the declaration of an annotation with the @Repeatable meta-annotation, to indicate that the annotation may be applied repeatedly to a single element. 
```Java
 // Repeatable annotation type

 @Retention(RetentionPolicy.RUNTIME)

 @Target(ElementType.METHOD)
 @Repeatable(ExceptionTestContainer.class)

 public @interface ExceptionTest {
     Class<? extends Exception> value();
 }
 @Retention(RetentionPolicy.RUNTIME)
 @Target(ElementType.METHOD)

public @interface ExceptionTestContainer {
	ExceptionTest[] value();
}
```
The getAnnotationsByType method glosses over this fact, and can be used to access both repeated and non-repeated annotations of a repeatable annotation type. But isAnnotationPresent makes it explicit that repeated annotations are not of the annotation type, but of the containing annotation type. 
``` Java
// Processing repeatable annotations
if (m.isAnnotationPresent(ExceptionTest.class)
	|| m.isAnnotationPresent(ExceptionTestContainer.class)) {
ExceptionTest[] excTests = m.getAnnotationsByType(ExceptionTest.class);
	for (ExceptionTest excTest : excTests) {
		if (excTest.value().isInstance(exc)) {
```

- isAnnotationPresent for ExceptionTest  won't work in case of multiple annotations of this type

* isAnnotationPresent for ExceptionTestContainer - will work only with multiple, but not with single
- Item 40: Consistently use the Override annotation
- Item 41: Use marker interfaces to define types
	  - marker interfaces define a type that is implemented by instances of the marked class; marker annotations do not.
	
	* Another advantage of marker interfaces over marker annotations is that they can be targeted more precisely
	
	If an annotation type is declared with target ElementType.TYPE, it can be applied to any class or interface. Suppose you have a marker that is applicable only to implementations of a particular interface. If you define it as a marker interface, you can have it extend the sole interface to which it is applicable, guaranteeing that all marked types are also subtypes of the sole interface to which it is applicable.
	* The chief advantage of marker annotations over marker interfaces is that they are part of the larger annotation facility.
	
	When to use what?
	- Clearly you must use an annotation if the marker applies to any program element other than a class or interface
	* If the marker applies only to classes and interfaces, ask yourself the question “Might I want to write one or more methods that accept only objects that have this marking?” If so, you should use a marker interface in preference to an annotation. This will make it possible for you to use the interface as a parameter type for the methods in question, which will result in the benefit of compile-time type checking
	Omit the types of all lambda parameters unless their presence makes your program clearer. If the compiler generates an error telling you it can’t infer the type of a lambda parameter, then specify it.
	Unlike methods and classes, lambdas lack names and documentation; if a computation isn’t self-explanatory, or exceeds a few lines, don’t put it in a lambda. One line is ideal for a lambda, and three lines is a reasonable maximum.
# Lambdas and Streams
- Item 42: Prefer lambdas to anonymous classes
	Note that this call uses an anonymous class instance in place of the lambda used in the previous call. That is because the function object needs to pass itself to s.removeObserver, and lambdas cannot access themselves
	Lambdas share with anonymous classes the property that you can’t reliably serialize and deserialize them across implementations. Therefore, you should rarely, if ever, serialize a lambda (or an anonymous class instance). If you have a function object that you want to make serializable, such as a Comparator, use an instance of a private static nested class
- Item 43: Prefer method references to lambdas
- Item 44: Favor the use of standard functional interfaces
  ![](Pasted%20image%2020250202142014.png)
	  If one of the standard functional interfaces does the job, you should generally use it in preference to a purpose-built functional interface.
	Don’t be tempted to use basic functional interfaces with boxed primitives instead of primitive functional interfaces. While it works, it violates the advice of Item 61, “prefer primitive types to boxed primitives.” The performance consequences of using boxed primitives for bulk operations can be deadly.
	You should seriously consider writing a purpose-built functional interface in preference to using a standard one if you need a functional interface that shares one or more of the following characteristics with Comparator:
	
	• It will be commonly used and could benefit from a descriptive name.
	
	• It has a strong contract associated with it.
	
	• It would benefit from custom default methods.
	  
- Item 45: Use streams judiciously
	    
	
	The most important part of the streams paradigm is to structure your computation as a sequence of transformations where the result of each stage is as close as possible to a pure function of the result of the previous stage. A pure function is one whose result depends only on its input: it does not depend on any mutable state, nor does it update any state. 
	
	  
	
	A forEach operation that does anything more than present the result of the computation performed by a stream is a “bad smell in code,” as is a lambda that mutates state.
	
	  
	
	The forEach operation should be used only to report the result of a stream computation, not to perform the computation.  
	
	The most important part of the streams paradigm is to structure your computation as a sequence of transformations where the result of each stage is as close as possible to a pure function of the result of the previous stage. A pure function is one whose result depends only on its input: it does not depend on any mutable state, nor does it update any state. 
	
	  
	
	A forEach operation that does anything more than present the result of the computation performed by a stream is a “bad smell in code,” as is a lambda that mutates state.
	
	  
	
	The forEach operation should be used only to report the result of a stream computation, not to perform the computation.
- Item 46: Prefer side-effect-free functions in streams
- Item 47: Prefer Collection to Stream as a return type
- Item 48: Use caution when making streams parallel
	Parallelizing a pipeline is unlikely to increase its performance if the source is from `Stream.iterate`, or the intermediate operation `limit` is used
	Do not parallelize stream pipelines indiscriminately.
	Performance gains from parallelism are best on streams over `ArrayList`, `HashMap`, `HashSet`, and `ConcurrentHashMap` instances; arrays; `int` ranges; and `long` ranges.
	
	The best terminal operations for parallelism are _reductions_, where all of the elements emerging from the pipeline are combined using one of `Stream`’s `reduce` methods, or prepackaged reductions such as `min`, `max`, `count`, and `sum`. The _short-circuiting_ operations `anyMatch`, `allMatch`, and `noneMatch` are also amenable to parallelism. The operations performed by `Stream`’s `collect` method, which are known as _mutable reductions_, are not good candidates for parallelism because the overhead of combining collections is costly.
	
	Not only can parallelizing a stream lead to poor performance, including liveness failures; it can lead to incorrect results and unpredictable behavior (safety failures)
	
	You won’t get a good speedup from parallelization unless the pipeline is doing enough real work to offset the costs associated with parallelism. As a _very_ rough estimate, the number of elements in the stream times the number of lines of code executed per element should be at least a hundred thousand
	
# Methods 
- Item 49: Check parameters for validity
- Item 50: Make defensive copies when needed
	    
	
	It is essential to make a defensive copy of each mutable parameter to the constructor 
	
	public Period(Date start, Date end) {
	
	    this.start = new Date(start.getTime());
	
	    this.end   = new Date(end.getTime());
	
	  
	
	    if (this.start.compareTo(this.end) > 0)
	
	      throw new IllegalArgumentException(
	
	          this.start + " after " + this.end);
	
	}
	
	do not use the clone method to make a defensive copy of a parameter whose type is subclassable by untrusted parties.
	
	  
	
	// Second attack on the internals of a Period instance
	
	Date start = new Date();
	
	Date end = new Date();
	
	Period p = new Period(start, end);
	
	p.end().setYear(78);  // Modifies internals of p!
	
	  
	
	To defend against the second attack, merely modify the accessors to return defensive copies of mutable internal fields:
	
	// Repaired accessors - make defensive copies of internal fields
	
	public Date start() {
	
	    return new Date(start.getTime());
	
	}
	
	  
	
	public Date end() {
	
	    return new Date(end.getTime());
	
	}
	
	In the accessors, unlike the constructor, it would be permissible to use the clone method to make the defensive copies. This is so because we know that the class of Period’s internal Date objects is java.util.Date, and not some untrusted subclass.
	
	  
	
	You should, where possible, use immutable objects as components of your objects so that you don’t have to worry about defensive copying
	
	  
	
	If the cost of the copy would be prohibitive and the class trusts its clients not to modify the components inappropriately, then the defensive copy may be replaced by documentation outlining the client’s responsibility not to modify the affected components.
- Item 51: Design method signatures carefully
- Item 52: Use overloading judiciously
		  Choice of which overloading to invoke is made at compile time. 
	
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
	
	            System.out.println(classify(c));
	
	    }
	
	}
	
	It prints Unknown Collection three times.
	
	selection among overloaded methods is static, while selection among overridden methods is dynamic.
	
	  
	
	A safe, conservative policy is never to export two overloadings with the same number of parameters
	
	  
	
	do not overload methods to take different functional interfaces in the same argument position.
- Item 53: Use varargs judiciously
	  // The right way to use varargs to pass one or more arguments
	
	static int min(int firstArg, int... remainingArgs) {
	
	  
	
	Exercise care when using varargs in performance-critical situations. Every invocation of a varargs method causes an array allocation and initialization.
	
	public void foo() { }
	
	public void foo(int a1) { }
	
	public void foo(int a1, int a2) { }
	
	public void foo(int a1, int a2, int a3) { }
	
	public void foo(int a1, int a2, int a3, int... rest) { }
	
	Now you know that you’ll pay the cost of the array creation only in the 5 percent of all invocations where the number of parameters exceeds three.
	
- Item 54: Return empty collections or arrays, not nulls
- Item 55: Return optionals judiciously
- Item 56: Write doc comments for all exposed API elements
  @implSpec comments, by contrast, describe the contract between a method and its subclass, allowing subclasses to rely on implementation behavior if they inherit the method or call it via super

	Occasionally you may wish to index additional terms that are important to your API. The {@index} tag was added for this purpose.
# General Programming
- Item 57: Minimize the scope of local variables
	The most powerful technique for minimizing the scope of a local variable is to declare it where it is first used.
- Item 58: Prefer for-each loops to traditional for loops
- Item 59: Know and use the libraries
	  the random number generator of choice is now ThreadLocalRandom.
- Item 60: Avoid float and double if exact answers are required
	The float and double types are particularly ill-suited for monetary calculations because it is impossible to represent 0.1 (or any other negative power of ten) as a float or double exactly.
	
	The right way to solve this problem is to use BigDecimal, int, or long for monetary calculations.
- Item 61: Prefer primitive types to boxed primitives
- Item 62: Avoid strings where other types are more appropriate
- Item 63: Beware the performance of string concatenation
- Item 64: Refer to objects by their interfaces
- Item 65: Prefer interfaces to reflection
  You can obtain many of the benefits of reflection while incurring few of its costs by using it only in a very limited form. For many programs that must use a class that is unavailable at compile time, there exists at compile time an appropriate interface or superclass by which to refer to the class (Item 64). If this is the case, you can create instances reflectively and access them normally via their interface or superclass.
	An alternative to providing a separate state-testing method is to have the state-dependent method return an empty optional (Item 55) or a distinguished value such as null if it cannot perform the desired computation.
- Item 66: Use native methods judiciously
- Item 67: Optimize judiciously
- Item 68: Adhere to generally accepted naming conventions
# Exceptions 
- Item 69: Use exceptions only for exceptional conditions
	Here are some guidelines to help you choose between a state-testing method and an optional or distinguished return value. If an object is to be accessed concurrently without external synchronization or is subject to externally induced state transitions, you must use an optional or distinguished return value, as the object’s state could change in the interval between the invocation of a state-testing method and its state-dependent method. Performance concerns may dictate that an optional or distinguished return value be used if a separate state-testing method would duplicate the work of the state-dependent method. All other things being equal, a state-testing method is mildly preferable to a distinguished return value. It offers slightly better readability, and incorrect use may be easier to detect: if you forget to call a state-testing method, the state-dependent method will throw an exception, making the bug obvious; if you forget to check for a distinguished return value, the bug may be subtle. This is not an issue for optional return values.
	
	if an object is to be accessed concurrently without external synchronization or it is subject to externally induced state transitions, this refactoring is inappropriate because the object’s state may change between the calls to actionPermitted and action. If a separate actionPermitted method would duplicate the work of the action method, the refactoring may be ruled out on performance grounds.
- Item 70: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors
	    
	
	- use checked exceptions for conditions from which the caller can reasonably be expected to recover
	
	* Use runtime exceptions to indicate programming errors. The great majority of runtime exceptions indicate precondition violations. A precondition violation is simply a failure by the client of an API to adhere to the contract established by the API specification
	
	* all of the unchecked throwables you implement should subclass RuntimeException
- Item 71: Avoid unnecessary use of checked exceptions
  In summary, when used sparingly, checked exceptions can increase the reliability of programs; when overused, they make APIs painful to use. If callers won’t be able to recover from failures, throw unchecked exceptions. If recovery may be possible and you want to force callers to handle exceptional conditions, first consider returning an optional. Only if this would provide insufficient information in the case of failure should you throw a checked exception.
- Item 72: Favor the use of standard exceptions
	- ****IllegalArgumentException**** - Non-null parameter value is inappropriate
	
	* ****IllegalStateException**** - Object state is inappropriate for method invocation
	
	* ****NullPointerException**** - Parameter value is null where prohibited
	
	* ****IndexOutOfBoundsException**** -Index parameter value is out of range
	
	* ****ConcurrentModificationException**** - Concurrent modification of an object has been detected where it is prohibited
	
	* ****UnsupportedOperationException**** - Object does not support method
- Item 73: Throw exceptions appropriate to the abstraction  
	Higher layers should catch lower-level exceptions and, in their place, throw exceptions that can be explained in terms of the higher-level abstraction.

	Where possible, the best way to deal with exceptions from lower layers is to avoid them, by ensuring that lower-level methods succeed. Sometimes you can do this by checking the validity of the higher-level method’s parameters before passing them on to lower layers
	
	If it is impossible to prevent exceptions from lower layers, the next best thing is to have the higher layer silently work around these exceptions, insulating the caller of the higher-level method from lower-level problems. Under these circumstances, it may be appropriate to log the exception using some appropriate logging facility such as java.util.logging. This allows programmers to investigate the problem, while insulating client code and the users from it.
- Item 74: Document all exceptions thrown by each method
	Use the Javadoc @throws tag to document each exception that a method can throw, but do not use the throws keyword on unchecked exceptions. 
	
	If an exception is thrown by many methods in a class for the same reason, you can document the exception in the class’s documentation comment rather than documenting it individually for each method.
- Item 75: Include failure-capture information in detail messages
	To capture a failure, the detail message of an exception should contain the values of all parameters and fields that contributed to the exception. 
	One way to ensure that exceptions contain adequate failure-capture information in their detail messages is to require this information in their constructors instead of a string detail message. The detail message can then be generated automatically to include the information. For example, instead of a String constructor, IndexOutOfBoundsException could have had a constructor that looks like this:
	
	public IndexOutOfBoundsException(int lowerBound, int upperBound, int index);
- Item 76: Strive for failure atomicity
	  Generally speaking, a failed method invocation should leave the object in the state that it was in prior to the invocation.
	
	- The most common way to achieve failure atomicity is to check parameters for validity before performing the operation
	
	* A closely related approach to achieving failure atomicity is to order the computation so that any part that may fail takes place before any part that modifies the object.
	
	* A third approach to achieving failure atomicity is to perform the operation on a temporary copy of the object and to replace the contents of the object with the temporary copy once the operation is complete.
	
	- A last and far less common approach to achieving failure atomicity is to write recovery code that intercepts a failure that occurs in the midst of an operation
- Item 77: Don’t ignore exceptions
# Concurrency
Item 78: Synchronize access to shared mutable data
Many programmers think of synchronization solely as a means of mutual exclusion, to prevent an object from being seen in an inconsistent state by one thread while it’s being modified by another. 

Proper use of synchronization guarantees that no method will ever observe the object in an inconsistent state.

Not only does synchronization prevent threads from observing an object in an inconsistent state, but it ensures that each thread entering a synchronized method or block sees the effects of all previous modifications that were guarded by the same lock.

Synchronization is required for reliable communication between threads as well as for mutual exclusion. his is due to a part of the language specification known as the memory model, which specifies when and how changes made by one thread become visible to others

The language specification guarantees that reading or writing a variable is atomic unless the variable is of type long or double 

  

Do not use Thread.stop. A recommended way to stop one thread from another is to have the first thread poll a boolean field that is initially false but can be set to true by the second thread to indicate that the first thread is to stop itself. Because reading and writing a boolean field is atomic, some programmers dispense with synchronization when accessing the field:
  

  ```Java

 // Broken! - How long would you expect this program to run?

 public class StopThread {

     private static boolean stopRequested;


     public static void main(String[] args)
             throws InterruptedException {

         Thread backgroundThread = new Thread(() -> {

             int i = 0;

             while (!stopRequested)

                 i++;

         });

         backgroundThread.start();

 

         TimeUnit.SECONDS.sleep(1);

         stopRequested = true;

     }

 }
```
You might expect this program to run for about a second, after which the main thread sets stopRequested to true, causing the background thread’s loop to terminate. On my machine, however, the program never terminates: the background thread loops forever!

  

The problem is that in the absence of synchronization, there is no guarantee as to when, if ever, the background thread will see the change in the value of stopRequested made by the main thread. In the absence of synchronization, it’s quite acceptable for the virtual machine to transform this code:

```Java
     while (!stopRequested)

         i++;
```

into this code:

```Java
 if (!stopRequested)

     while (true)

         i++;
```

This optimization is known as hoisting, and it is precisely what the OpenJDK Server VM does. The result is a liveness failure: the program fails to make progress. One way to fix the problem is to synchronize access to the stopRequested field. This program terminates in about one second, as expected:
```Java
 // Properly synchronized cooperative thread termination

 public class StopThread {

     private static boolean stopRequested;

 

     private static synchronized void requestStop() {

         stopRequested = true;

     }

 

     private static synchronized boolean stopRequested() {

         return stopRequested;

     }

 

     public static void main(String[] args)

             throws InterruptedException {

         Thread backgroundThread = new Thread(() -> {

             int i = 0;

             while (!stopRequested())

                 i++;

         });

         backgroundThread.start();

         TimeUnit.SECONDS.sleep(1);

         requestStop();
     }
 }
```

Note that both the write method (requestStop) and the read method (stop-Requested) are synchronized. It is not sufficient to synchronize only the write method! Synchronization is not guaranteed to work unless both read and write operations are synchronized.

  

The actions of the synchronized methods in StopThread would be atomic even without synchronization. In other words, the synchronization on these methods is used solely for its communication effects, not for mutual exclusion. While the cost of synchronizing on each iteration of the loop is small, there is a correct alternative that is less verbose and whose performance is likely to be better. The locking in the second version of StopThread can be omitted if stopRequested is declared volatile. While the volatile modifier performs no mutual exclusion, it guarantees that any thread that reads the field will see the most recently written value.

You do have to be careful when using volatile. Consider the following method, which is supposed to generate serial numbers:
```Java
 // Broken - requires synchronization!
 private static volatile int nextSerialNumber = 0;
 public static int generateSerialNumber() {
     return nextSerialNumber++;
 }
```

The problem is that the increment operator (++) is not atomic. It performs two operations on the nextSerialNumber field: first it reads the value, and then it writes back a new value, equal to the old value plus one. If a second thread reads the field between the time a thread reads the old value and writes back a new one, the second thread will see the same value as the first and return the same serial number. This is a safety failure: the program computes the wrong results.

  

One way to fix generateSerialNumber is to add the synchronized modifier to its declaration. This ensures that multiple invocations won’t be interleaved and that each invocation of the method will see the effects of all previous invocations. Once you’ve done that, you can and should remove the volatile modifier from nextSerialNumber. To bulletproof the method, use long instead of int, or throw an exception if nextSerialNumber is about to wrap.

  

Better still, follow the advice in Item 59 and use the class AtomicLong, which is part of java.util.concurrent.atomic. This package provides primitives for lock-free, thread-safe programming on single variables. While volatile provides only the communication effects of synchronization, this package also provides atomicity. This is exactly what we want for generateSerialNumber, and it is likely to outperform the synchronized version:
```Java
 // Lock-free synchronization with java.util.concurrent.atomic
 private static final AtomicLong nextSerialNum = new AtomicLong();
 public static long generateSerialNumber() {
     return nextSerialNum.getAndIncrement();
 }
```

  

The best way to avoid the problems discussed in this item is not to share mutable data. Either share immutable data (Item 17) or don’t share at all. In other words, confine mutable data to a single thread. 

It is acceptable for one thread to modify a data object for a while and then to share it with other threads, synchronizing only the act of sharing the object reference. Other threads can then read the object without further synchronization, so long as it isn’t modified again. Such objects are said to be effectively immutable 

Transferring such an object reference from one thread to others is called safe publication [Goetz06, 3.5.3]. There are many ways to safely publish an object reference: you can store it in a static field as part of class initialization; you can store it in a volatile field, a final field, or a field that is accessed with normal locking; or you can put it into a concurrent collection (Item 81).

When multiple threads share mutable data, each thread that reads or writes the data must perform synchronization.
**Item 79: Avoid excessive synchronization**
To avoid liveness and safety failures, never cede control to the client within a synchronized method or block. 

In other words, inside a synchronized region, do not invoke a method that is designed to be overridden, or one provided by a client in the form of a function object

  ```
 // Alien method moved outside of synchronized block - open calls
 private void notifyElementAdded(E element) {
     synchronized(observers) {
         for (SetObserver<E> observer : observers)

             observer.added(this, element);

        }

    }
 }
```

Luckily, it is usually not too hard to fix this sort of problem by moving alien method invocations out of synchronized blocks. For the notifyElementAdded method, this involves taking a “snapshot” of the observers list that can then be safely traversed without a lock. With this change, both of the previous examples run without exception or deadlock:
```
 // Alien method moved outside of synchronized block - open calls
 private void notifyElementAdded(E element) {
     List<SetObserver<E>> snapshot = null;
     synchronized(observers) {
         snapshot = new ArrayList<>(observers);
     }
     for (SetObserver<E> observer : snapshot)
         observer.added(this, element);
 }
```
In fact, there’s a better way to move the alien method invocations out of the synchronized block. The libraries provide a concurrent collection (Item 81) known as CopyOnWriteArrayList that is tailor-made for this purpose. This List implementation is a variant of ArrayList in which all modification operations are implemented by making a fresh copy of the entire underlying array. Because the internal array is never modified, iteration requires no locking and is very fast. For most uses, the performance of CopyOnWriteArrayList would be atrocious, but it’s perfect for observer lists, which are rarely modified and often traversed.

  

As a rule, you should do as little work as possible inside synchronized regions. Obtain the lock, examine the shared data, transform it as necessary, and drop the lock. If you must perform some time-consuming activity, find a way to move it out of the synchronized region

  

  

If a method modifies a static field and there is any possibility that the method will be called from multiple threads, you must synchronize access to the field internally (unless the class can tolerate nondeterministic behavior). 

It is not possible for a multithreaded client to perform external synchronization on such a method, because unrelated clients can invoke the method without synchronization. 

The field is essentially a global variable even if it is private because it can be read and modified by unrelated clients. The nextSerialNumber field used by the method generateSerialNumber in Item 78 exemplifies this situation.

In summary, to avoid deadlock and data corruption, never call an alien method from within a synchronized region. More generally, keep the amount of work that you do from within synchronized regions to a minimum. When you are designing a mutable class, think about whether it should do its own synchronization. In the multicore era, it is more important than ever not to oversynchronize. Synchronize your class internally only if there is a good reason to do so, and document your decision clearly
**Item 80: Prefer executors, tasks, and streams to threads**
ExecutorService exec = Executors.newSingleThreadExecutor();

exec.execute(runnable);

exec.shutdown();

- ExecutorCompletionService

* ScheduledThreadPoolExecutor

* ThreadPoolExecutor

* Executors.newFixedThreadPool

* Choosing the executor service for a particular application can be tricky. For a small program, or a lightly loaded server, ****Executors.newCachedThreadPool**** is generally a good choice because it demands no configuration and generally “does the right thing.” But a cached thread pool is not a good choice for a heavily loaded production server! In a cached thread pool, submitted tasks are not queued but immediately handed off to a thread for execution. If no threads are available, a new one is created. If a server is so heavily loaded that all of its CPUs are fully utilized and more tasks arrive, more threads will be created, which will only make matters worse. 

* Therefore, in a heavily loaded production server, you are much better off using Executors.newFixedThreadPool, which gives you a pool with a fixed number of threads, or using the ThreadPoolExecutor class directly, for maximum control.

* A fork-join task, represented by a ForkJoinTask instance, may be split up into smaller subtasks, and the threads comprising a ForkJoinPool not only process these tasks but “steal” tasks from one another to ensure that all threads remain busy, resulting in higher CPU utilization, higher throughput, and lower latency. Writing and tuning fork-join tasks is tricky. Parallel streams (Item 48) are written atop fork join pools and allow you to take advantage of their performance benefits with little effort, assuming they are appropriate for the task at hand.****
Item 81: Prefer concurrency utilities to wait and notify

Given the difficulty of using wait and notify correctly, you should use the higher-level concurrency utilities instead.

it is impossible to exclude concurrent activity from a concurrent collection; locking it will only slow the program.

concurrent collection interfaces were outfitted with state-dependent modify operations, which combine several primitives into a single atomic operation. These operations proved sufficiently useful on concurrent collections that they were added to the corresponding collection interfaces in Java 8, using default methods (Item 21).

  

For example, Map’s putIfAbsent(key, value) method inserts a mapping for a key if none was present and returns the previous value associated with the key, or null if there was none. This makes it easy to implement thread-safe canonicalizing maps. 

ConcurrentHashMap is optimized for retrieval operations, such as get. Therefore, it is worth invoking get initially and calling putIfAbsent only if get indicates that it is necessary:
```
 // Concurrent canonicalizing map atop ConcurrentMap - faster!
 public static String intern(String s) {
     String result = map.get(s);
     if (result == null) {
         result = map.putIfAbsent(s, s);
         if (result == null)
             result = s;
     }
     return result;
 }
```

use ConcurrentHashMap in preference to Collections.synchronizedMap. Simply replacing synchronized maps with concurrent maps can dramatically increase the performance of concurrent applications.

BlockingQueue extends Queue and adds several methods, including take, which removes and returns the head element from the queue, waiting if the queue is empty. This allows blocking queues to be used for work queues (also known as producer-consumer queues)

  

The executor passed to the time method must allow for the creation of at least as many threads as the given concurrency level, or the test will never complete. This is known as a thread starvation deadlock [Goetz06, 8.1.1]. If a worker thread catches an InterruptedException, it reasserts the interrupt using the idiom Thread.currentThread().interrupt() and returns from its run method. This allows the executor to deal with the interrupt as it sees fit. 

  

The wait method is used to make a thread wait for some condition. It must be invoked inside a synchronized region that locks the object on which it is invoked. Here is the standard idiom for using the wait method:
```
   // The standard idiom for using the wait method
 synchronized (obj) {
     while (<condition does not hold>)
         obj.wait(); // (Releases lock, and reacquires on wakeup)
     ... // Perform action appropriate to condition
 }
```

  

Always use the wait loop idiom to invoke the wait method; never invoke it outside of a loop. The loop serves to test the condition before and after waiting.

  

Testing the condition before waiting and skipping the wait if the condition already holds are necessary to ensure liveness. If the condition already holds and the notify (or notifyAll) method has already been invoked before a thread waits, there is no guarantee that the thread will ever wake from the wait.

Testing the condition after waiting and waiting again if the condition does not hold are necessary to ensure safety. If the thread proceeds with the action when the condition does not hold, it can destroy the invariant guarded by the lock. There are several reasons a thread might wake up when the condition does not hold:

  

- Another thread could have obtained the lock and changed the guarded state between the time a thread invoked notify and the waiting thread woke up.

*  Another thread could have invoked notify accidentally or maliciously when the condition did not hold. Classes expose themselves to this sort of mischief by waiting on publicly accessible objects. Any wait in a synchronized method of a publicly accessible object is susceptible to this problem.

* The notifying thread could be overly “generous” in waking waiting threads. For example, the notifying thread might invoke notifyAll even if only some of the waiting threads have their condition satisfied.

* The waiting thread could (rarely) wake up in the absence of a notify. This is known as a spurious wakeup 

A related issue is whether to use notify or notifyAll to wake waiting threads. (Recall that notify wakes a single waiting thread, assuming such a thread exists, and notifyAll wakes all waiting threads.) It is sometimes said that you should always use notifyAll. This is reasonable, conservative advice. It will always yield correct results because it guarantees that you’ll wake the threads that need to be awakened. You may wake some other threads, too, but this won’t affect the correctness of your program. These threads will check the condition for which they’re waiting and, finding it false, will continue waiting.

As an optimization, you may choose to invoke notify instead of notifyAll if all threads that could be in the wait-set are waiting for the same condition and only one thread at a time can benefit from the condition becoming true.
**Item 82: Document thread safety**
There are several levels of thread safety. To enable safe concurrent use, a class must clearly document what level of thread safety it supports. The following list summarizes levels of thread safety. It is not exhaustive but covers the common cases:

1. Immutable—Instances of this class appear constant. No external synchronization is necessary. Examples include String, Long, and BigInteger

2. Unconditionally thread-safe—Instances of this class are mutable, but the class has sufficient internal synchronization that its instances can be used concurrently without the need for any external synchronization. Examples include AtomicLong and ConcurrentHashMap.

3. Conditionally thread-safe—Like unconditionally thread-safe, except that some methods require external synchronization for safe concurrent use. Examples include the collections returned by the Collections.synchronized wrappers, whose iterators require external synchronization.

4. Not thread-safe—Instances of this class are mutable. To use them concurrently, clients must surround each method invocation (or invocation sequence) with external synchronization of the clients’ choosing. Examples include the general-purpose collection implementations, such as ArrayList and HashMap.

5.  Thread-hostile—This class is unsafe for concurrent use even if every method invocation is surrounded by external synchronization. Thread hostility usually results from modifying static data without synchronization. No one writes a thread-hostile class on purpose; such classes typically result from the failure to consider concurrency. When a class or method is found to be thread-hostile, it is typically fixed or deprecated.

Also, a client can mount a denial-of-service attack by holding the publicly accessible lock for a prolonged period. This can be done accidentally or intentionally.

  

To prevent this denial-of-service attack, you can use a private lock object instead of using synchronized methods (which imply a publicly accessible lock):
```
// Private lock object idiom - thwarts denial-of-service attack

private final Object lock = new Object();

  

public void foo() {

synchronized(lock) {

...

}

}
```
  

Note that the lock field is declared final. This prevents you from inadvertently changing its contents, which could result in catastrophic unsynchronized access (Item 78). We are applying the advice of Item 17, by minimizing the mutability of the lock field. Lock fields should always be declared final. This is true whether you use an ordinary monitor lock (as shown above) or a lock from the java.util.concurrent.locks package.

  

The private lock object idiom can be used only on unconditionally thread-safe classes. Conditionally thread-safe classes can’t use this idiom because they must document which lock their clients are to acquire when performing certain method invocation sequences
**Item 83: Use lazy initialization judiciously**
- If you need to use lazy initialization for performance on a static field, use the lazy initialization holder class idiom. This idiom exploits the guarantee that a class will not be initialized until it is used [JLS, 12.4.1]. Here’s how it looks:
```
// Lazy initialization holder class idiom for static fields

private static class FieldHolder {

static final FieldType field = computeFieldValue();

}

  

private static FieldType getField() { return FieldHolder.field; }
```
  

When getField is invoked for the first time, it reads FieldHolder.field for the first time, causing the initialization of the FieldHolder class. The beauty of this idiom is that the getField method is not synchronized and performs only a field access, so lazy initialization adds practically nothing to the cost of access. A typical VM will synchronize field access only to initialize the class. Once the class is initialized, the VM patches the code so that subsequent access to the field does not involve any testing or synchronization.

- If you need to use lazy initialization for performance on an instance field, use the double-check idiom. This idiom avoids the cost of locking when accessing the field after initialization (Item 79). The idea behind the idiom is to check the value of the field twice (hence the name double-check): once without locking and then, if the field appears to be uninitialized, a second time with locking.  it is critical that the field be declared volatile
```
// Double-check idiom for lazy initialization of instance fields

private volatile FieldType field;

  

private FieldType getField() {

FieldType result = field;

if (result == null) {  // First check (no locking)

synchronized(this) {

if (field == null)  // Second check (with locking)

field = result = computeFieldValue();

}

}

return result;

}
```
This code may appear a bit convoluted. In particular, the need for the local variable (result) may be unclear. What this variable does is to ensure that field is read only once in the common case where it’s already initialized.While not strictly necessary, this may improve performance
**Item 84: Don’t depend on the thread scheduler**
Any program that relies on the thread scheduler for correctness or performance is likely to be nonportable. The best way to write a robust, responsive, portable program is to ensure that the average number of runnable threads is not significantly greater than the number of processors. This leaves the thread scheduler with little choice: it simply runs the runnable threads till they’re no longer runnable. 

Threads should not run if they aren’t doing useful work. In terms of the Executor Framework (Item 80), this means sizing thread pools appropriately [Goetz06, 8.2] and keeping tasks short, but not too short, or dispatching overhead will harm performance.

Threads should not busy-wait, repeatedly checking a shared object waiting for its state to change.

When faced with a program that barely works because some threads aren’t getting enough CPU time relative to others, resist the temptation to “fix” the program by putting in calls to Thread.yield. You may succeed in getting the program to work after a fashion, but it will not be portable. The same yield invocations that improve performance on one JVM implementation might make it worse on a second and have no effect on a third. Thread.yield has no testable semantics. A better course of action is to restructure the application to reduce the number of concurrently runnable threads.

Thread priorities are among the least portable features of Java. It is not unreasonable to tune the responsiveness of an application by tweaking a few thread priorities, but it is rarely necessary and is not portable. It is unreasonable to attempt to solve a serious liveness problem by adjusting thread priorities.
# Serialization
**Item 85: Prefer alternatives to Java serialization**
A fundamental problem with serialization is that its attack surface is too big to protect, and constantly growing: Object graphs are deserialized by invoking the readObject method on an ObjectInputStream. This method is essentially a magic constructor that can be made to instantiate objects of almost any type on the class path, so long as the type implements the Serializable interface. In the process of deserializing a byte stream, this method can execute code from any of these types, so the code for all of these types is part of the attack surface.

The best way to avoid serialization exploits is never to deserialize anything. There is no reason to use Java serialization in any new system you write. 

If you can’t avoid Java serialization entirely, perhaps because you’re working in the context of a legacy system that requires it, your next best alternative is to never deserialize untrusted data. 

If you can’t avoid serialization and you aren’t absolutely certain of the safety of the data you’re deserializing, use the object deserialization filtering added in Java 9 and backported to earlier releases (java.io.ObjectInputFilter). This facility lets you specify a filter that is applied to data streams before they’re deserialized. It operates at the class granularity, letting you accept or reject certain classes.
**Item 86: Implement Serializable with great caution**
A major cost of implementing Serializable is that it decreases the flexibility to change a class’s implementation once it has been released.

f you accept the default serialized form and later change a class’s internal representation, an incompatible change in the serialized form will result. Clients attempting to serialize an instance using an old version of the class and deserialize it using the new one (or vice versa) will experience program failures.

- A simple example of the constraints on evolution imposed by serializability concerns stream unique identifiers, more commonly known as serial version UIDs. Every serializable class has a unique identification number associated with it. If you do not specify this number by declaring a static final long field named serialVersionUID, the system automatically generates it at runtime by applying a cryptographic hash function (SHA-1) to the structure of the class. This value is affected by the names of the class, the interfaces it implements, and most of its members, including synthetic members generated by the compiler. If you change any of these things, for example, by adding a convenience method, the generated serial version UID changes. If you fail to declare a serial version UID, compatibility will be broken, resulting in an InvalidClassException at runtime.

- A second cost of implementing Serializable is that it increases the likelihood of bugs and security holes (Item 85). Normally, objects are created with constructors; serialization is an extralinguistic mechanism for creating objects. Whether you accept the default behavior or override it, deserialization is a “hidden constructor” with all of the same issues as other constructors. Because there is no explicit constructor associated with deserialization, it is easy to forget that you must ensure that it guarantees all of the invariants established by the constructors and that it does not allow an attacker to gain access to the internals of the object under construction. Relying on the default deserialization mechanism can easily leave objects open to invariant corruption and illegal access

* A third cost of implementing Serializable is that it increases the testing burden associated with releasing a new version of a class. When a serializable class is revised, it is important to check that it is possible to serialize an instance in the new release and deserialize it in old releases, and vice versa. The amount of testing required is thus proportional to the product of the number of serializable classes and the number of releases, which can be large. You must ensure both that the serialization-deserialization process succeeds and that it results in a faithful replica of the original object.

* Implementing Serializable is not a decision to be undertaken lightly. It is essential if a class is to participate in a framework that relies on Java serialization for object transmission or persistence.

* Inner classes should not implement Serializable. They use compiler-generated synthetic fields to store references to enclosing instances and to store values of local variables from enclosing scopes. How these fields correspond to the class definition is unspecified, as are the names of anonymous and local classes. Therefore, the default serialized form of an inner class is ill-defined. A static member class can, however, implement Serializable.
**Item 87: Consider using a custom serialized form**
Do not accept the default serialized form without first considering whether it is appropriate. Accepting the default serialized form should be a conscious decision that this encoding is reasonable from the standpoint of flexibility, performance, and correctness. Generally speaking, you should accept the default serialized form only if it is largely identical to the encoding that you would choose if you were designing a custom serialized form.

The ideal serialized form of an object contains only the logical data represented by the object. It is independent of the physical representation.

Even if you decide that the default serialized form is appropriate, you often must provide a readObject method to ensure invariants and security.

Regardless of what serialized form you choose, declare an explicit serial version UID in every serializable class you write. 

  

Do not change the serial version UID unless you want to break compatibility with all existing serialized instances of a class.
**Item 88: Write readObject methods defensively**
readObject method is effectively another public constructor, and it demands all of the same care as any other constructor. Just as a constructor must check its arguments for validity (Item 49) and make defensive copies of parameters where appropriate (Item 50), so must a readObject method

would you feel comfortable adding a public constructor that took as parameters the values for each nontransient field in the object and stored the values in the fields with no validation whatsoever? If not, you must provide a readObject method, and it must perform all the validity checking and defensive copying that would be required of a constructor. 

Do not invoke any overridable methods in the class, directly or indirectly.
Item 89: For instance control, prefer enum types to readResolve

Item 90: Consider serialization proxies instead of serialized instances