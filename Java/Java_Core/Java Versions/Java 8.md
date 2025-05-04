- [Lambdas](#Lambdas) − a new language feature allowing us to treat actions as objects
- [Method Reference](#Method%20Reference) − enable us to define Lambda Expressions by referring to methods directly using their names
- [**Optional**](#**Optional**) − special wrapper class used for expressing optionality
- **Functional Interface** – an interface with maximum one abstract method; implementation can be provided using a Lambda Expression
- [Default Methods](#Default%20Methods) − give us the ability to add full implementations in interfaces besides abstract methods
- [Stream API](#Stream%20API) − a special iterator class that allows us to process collections of objects in a functional manner
- [Date Time Api](#Date%20Time%20Api) − an improved, immutable JodaTime-inspired Date API
### Stream API
The mechanism that allows you to work with data in a functional style. Stream is an iterator whose role is to accept a set of actions to apply on each of the elements it contains.  It can be used to process the data flow. The _stream_ represents a sequence of objects from a source such as a collection, which supports aggregate operations. They were designed to make collection processing simple and concise.

Another important distinction from collections is that streams are inherently lazily loaded and processed.
#### Conveyor(intermediate) Operations
Operations that can be performed any number of times as they return stream

| Method stream                         | Description                                                                                   |
| ------------------------------------- | --------------------------------------------------------------------------------------------- |
| filter                                | Filters records, returns only records that match the condition                                |
| skip                                  | Skips N first elements                                                                        |
| distinct                              | Returns stream without duplicates (for the equals method)                                     |
| map                                   | Convert each stream element                                                                   |
| peek                                  | Returns the same stream, but applies the function to each element of the stream               |
| limit                                 | Allows you to limit the sample to a certain number of first elements                          |
| sorted                                | Allows you to sort the values either in natural order or by setting Comparator                |
| mapToInt,mapToDouble, mapToLong       | Analog map, but returns a primitives stream (i.e., a stream from numeric primitives)          |
| flatMap, flatMapToInt,flatMapToDouble | Similar to map, but can convert stream of Collections to stream of items of these collections |

#### Terminal
Operations that return the resulting value

| **Method stream**                  | **Description**                                                                                                                                                                                                                                                      |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| findFirst                          | Returns the first element of a stream (returns Optional)                                                                                                                                                                                                             |
| findAny                            | Returns any matching element from the stream (returns Optional)                                                                                                                                                                                                      |
| collect                            | Presentation of results as collections and other data structures                                                                                                                                                                                                     |
| count                              | Returns the number of elements in a stream                                                                                                                                                                                                                           |
| anyMatch                           | Returns true if the condition is fulfilled for at least one element                                                                                                                                                                                                  |
| noneMatch                          | Returns true if none of the elements are true                                                                                                                                                                                                                        |
| allMatch                           | Returns true if the condition is met for all elements                                                                                                                                                                                                                |
| min                                | Returns the minimum element, the condition is used by the comparator                                                                                                                                                                                                 |
| max                                | Returns the maximum element, the condition is a comparator                                                                                                                                                                                                           |
| forEach                            | Applies function to each stream object, order in parallel execution is not guaranteed                                                                                                                                                                                |
| forEachOrdered                     | Applies the function to each stream object, keeping the order of the elements ensures                                                                                                                                                                                |
| reduce                             | Allows you to perform aggregate functions on the entire collection and return one result                                                                                                                                                                             |
| toArray                            | Returns a string of values                                                                                                                                                                                                                                           |
|                                    |                                                                                                                                                                                                                                                                      |
| *Aggregating*                      | Terminal operations that allow the collection of the resulting data, after the impact on them stream. <br>- Run with collect, it can be implemented manually, or with the Collector class. <br>- Which accepts the resulting flow type, intermediate type, and type. |
| toList, toMap, toSet, toCollection |                                                                                                                                                                                                                                                                      |
| joining                            | соединяет элементы стрима в CharSequence                                                                                                                                                                                                                             |
| counting                           | возвращает к-во элементов                                                                                                                                                                                                                                            |
| groupingBy                         | группирует объекты по свойству и складывает их в map. В отличие от toMap принимает вторым параметром коллектор                                                                                                                                                       |
| partitioningBy                     | тот же groupingBy, но разделяет коллекцию по предикату на true и false                                                                                                                                                                                               |
#### Creation
- Stream.of(o1, o2, o3)
- Сollection.stream()
- Arrays.stream()
#### Stream vs loop

| Streams                                                                                     | Loops                          |
| ------------------------------------------------------------------------------------------- | ------------------------------ |
| Declarative                                                                                 | Imperative                     |
| Easier to Maintain                                                                          | Better raw performance         |
| Better performance for huge lists by using parallel streams                                 | Fully controlled by developer  |
| Functional in nature                                                                        | More readable for nested logic |
| Operations performed on a stream do not modify the source.                                  |                                |
| Stream is lazy and evaluates code only when required.                                       |                                |
| The elements of a stream only visited once during the life of a stream.                     |                                |
| Like an Iterator, a new stream must be generated to revisit the same elements of the source |                                |
| Uses extra memory for intermediate operations                                               |                                |
#### Parallel Streams
In case of Parallel stream, several threads are spawned simultaneously and it internally using Fork and Join pool to create and manage threads.Parallel streams create `ForkJoinPool` instance via static `ForkJoinPool.commonPool()` method.

```Java
IntStream intParallelStream = Arrays.stream(array).parallel();
intParallelStream.forEach(s ->
        System.out.println(s + " " + Thread.currentThread().getName())
);
// =================================
// 2 main
// 1 ForkJoinPool.commonPool-worker-1
// 3 ForkJoinPool.commonPool-worker-2
// =================================
```

Parallel Stream takes the benefits of all available `CPU cores` and processes the tasks in parallel. If number of tasks exceeds the number of cores, then remaining tasks wait for currently running task to complete.

You need to consider parallel Stream if and only if:
- You have large dataset to process.
- ForkJoinPool forks sources stream and submit for execution, so your source stream should be splittable. 
	- ArrayList is very easy to split, as we can find a middle element by its index and split it
	- LinkedList is very hard to split and does not perform very well in most of the cases.
- You are actually suffering from performance issues.
- You need to make sure that all the shared resources between threads need to be synchronized properly otherwise it might produce unexpected results.

You could use custom fork join pool

```Java
ForkJoinPool fjp1 = new ForkJoinPool(5);
Callable<Integer> callable1 = () -> data.parallelStream()
  .map(i -> (int) Math.sqrt(i))
  .map(number -> performComputation(number))
  .peek(i -> {
    System.out.println("Processing with "+Thread.currentThread()+" "+ i);})
  .reduce(0, Integer::sum);

try {
    sumFJ1 = fjp1.submit(callable1).get();
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

#### Default Methods
A default method is a method with an implementation, which can be found in an interface.
We can use a default method to add a new functionality to an interface, while maintaining backward compatibility with classes that are already implementing the interface.
When a class implements several interfaces that define the same _default_ methods, the code simply won't compile, as there's a conflict caused by multiple interface inheritance (a.k.a the Diamond Problem). To solve this ambiguity, we must explicitly provide an implementation methods.
#### Static methods in interfaces
- Since _static_ methods don't belong to a particular object, they're not part of the API of the classes implementing the interface; therefore, they have to be **called by using the interface name preceding the method name**. 
- **Defining a** _**static**_ **method within an interface is identical to defining one in a class.** Moreover, a _static_ method can be invoked within other _static_ and _default_ methods. 
- The idea behind _static_ interface methods is to provide a simple mechanism that allows us to **increase the degree of** cohesion of a design by putting together related methods in one single place without having to create an object.

### Lambdas
A function that we can reference and pass around as an object. lambda expressions are a natural replacement for anonymous classes such as method arguments. One of their main uses is to define inline implementations of functional interfaces. 

 - Лямбда может работать с final и effectively final переменными, которые находятся в том же методе (Анонимные классы тоже, но еще и с дефолтными методами), полями класса и статическими переменными. Для создания лямбды компилятор генерирует синтетический метод (неявный).

### Method Reference
A method reference is a Java 8 construct that can be used for referencing a method without invoking it. It’s used for treating methods as Lambda Expressions. They only work as syntactic sugar to reduce the verbosity of some lambdas.

- Constructor reference:  
  `String::new;`
- Static method reference:  
  `String::valueOf;`
- Bound instance method reference:  
  `str::toString;`
- Unbound instance method reference:  
  `String::toString;`
### **Функциональные интерфейсы**
A functional interface is an interface with one single abstract method (_default_ methods do not count), no more, no less.

Where an instance of such an interface is required, a Lambda Expression can be used instead. More formally put: _Functional interfaces_ provide target types for lambda expressions and method references.

The arguments and return type of such an expression directly match those of the single abstract method.

Functional interfaces are usually annotated with the _@FunctionalInterface_ annotation, which is informative and doesn’t affect the semantics.

|Interface|Parameters|Returns|
|---|---|---|
|Supplier|()|T|
|Consumer|T|void|
|Predicate|T|Boolean|
|Function|T|R|
|Operator|T|T|
## **Optional**
Опциональные значения (optionals) являются удобным средством обертывания результата операции для предотвращения NullPointerException. 
- Они позволяют показать пользователю метода, что результат может оказаться пустым. 
- Если передается null
	- в Optional.of будет исключение, 
	- в Optional.ofNullable - Optional.empty
- It’s not recommended that we use it as a field value in entity classes, which is clearly indicated by it not implementing the _Serializable_ interface.

### **Monads**
In functional programming, a monad is a software design pattern with a structure that combines program fragments (functions) and wraps their return values in a type with additional computation. It encapsulates an effect — computation that may either result in an exception or return a successfully computed value. This is what Monad does — wraps a value and gives it some effect. That’s it. You just take a value, put it into a monad and get back an amplified value. An “amplified” means that the power of value’s type is increased. All it takes be a Monad is to provide two functions which conform to three laws.
1. Place a value into monadic contextOptional.of
2. Apply a function in monadic context. `flatMap(n -> Optional.of(n + 1))`

Laws
1. left-identity
2. right-identity
3. associative

Left-identity and right-identity laws basically say that Optional.of function doesn’t change the value in a monad:

`Optional.of(x).flatMap(f) == f(x)monad.flatMap(val -> Optional.of(val)) == monad`

Associativity law says that when we have a chain of flatMap functions it shouldn’t matter how they’re nested:

`monad.flatMap(f).flatMap(g) == monad.flatMap(x ⇒ f(x).flatMap(g))`

May me used in some data pipelines for ex streams to prevent failures due to exceptions.

### Date Time Api
//todo add