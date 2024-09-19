---
Interview graded: true
Last edited time: 2024-04-03T18:07
Last recall: 2024-03-29
Needs Rework: false
Status: Not started
Topic:
  - "[[Knowledge Transfer/Backend/Java/Java Core/Java Core\\|Java Core]]"
---
Stream

Механизм, позволяющий выполнять работу с данными в функциональном стиле. С его помощью можно выполнять обработку потока данных

There are 2 types of operations:

- **conveyor**(intermediate) - operations that can be performed any number of times as they return stream

|   |   |
|---|---|
|Метод stream|Описание|
|filter|Отфильтровывает записи, возвращает только записи, соответствующие условию|
|skip|Позволяет пропустить N первых элементов|
|distinct|Возвращает стрим без дубликатов (для метода equals)|
|map|Преобразует каждый элемент стрима|
|peek|Возвращает тот же стрим, но применяет функцию к каждому элементу стрима|
|limit|Позволяет ограничить выборку определенным количеством первых элементов|
|sorted|Позволяет сортировать значения либо в натуральном порядке, либо задавая Comparator|
|mapToInt,mapToDouble, mapToLong|Аналог map, но возвращает числовой стрим (то есть стрим из числовых примитивов)|
|flatMap, flatMapToInt,flatMapToDouble|Похоже на map, но может создавать из одного элемента несколько|

- **terminal** operations that return the resulting value

|   |   |
|---|---|
|**Метод stream**|**Описание**|
|findFirst|Возвращает первый элемент из стрима (возвращает Optional)|
|findAny|Возвращает любой подходящий элемент из стрима (возвращает Optional)|
|collect|Представление результатов в виде коллекций и других структур данных|
|count|Возвращает количество элементов в стриме|
|anyMatch|Возвращает true, если условие выполняется хотя бы для одного элемента|
|noneMatch|Возвращает true, если условие не выполняется ни для одного элемента|
|allMatch|Возвращает true, если условие выполняется для всех элементов|
|min|Возвращает минимальный элемент, в качестве условия использует компаратор|
|max|Возвращает максимальный элемент, в качестве условия использует компаратор|
|forEach|Применяет функцию к каждому объекту стрима, порядок при параллельном выполнении не гарантируется|
|forEachOrdered|Применяет функцию к каждому объекту стрима, сохранение порядка элементов гарантирует|
|reduce|Позволяет выполнять агрегатные функции на всей коллекцией и возвращать один результат|
|toArray|Возвращает массив значений стрима|

### **Создание**

- Stream.of
- Сollection.stream
- Arrays.stream

### **Aggregating operations**

Terminal operations that allow the collection of the resulting data, after the impact on them stream. Run with collect, it can be implemented manually, or with the Collector class. Which accepts the resulting flow type, intermediate type, and type. Contains

- toList, toMap, toSet, toCollection
- joining - соединяет элементы стрима в CharSequence
- counting - возвращает к-во элементов
- groupingBy - группирует объекты по свойству и складывает их в map. В отличие от toMap принимает вторым параметром коллектор
- partitioningBy - тот же groupingBy, но разделяет коллекцию по предикату на true и false

### **Область видимости**

Лямбда может работать с final переменными, которые находятся в том же методе

Анонимные тоже, но еще и с дефолтными методами

**Stream vs loop**

**Streams**

- Easier to Maintain
- Better performance for huge lists by using parallel streams
- Functional in nature
- Operations performed on a stream do not modify the source.
- Stream is lazy and evaluates code only when required.
- The elements of a stream only visited once during the life of a stream.
- Like an Iterator, a new stream must be generated to revisit the same elements of the source

**Loops**

- Better raw performance

## Parallel Streams

In case of Parallel stream,2 threads are spawned simultaneously and it internally using Fork and Join pool to create and manage threads.Parallel streams create `ForkJoinPool` instance via static `ForkJoinPool.commonPool()` method.

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
- As you know that Java uses `ForkJoinPool` to achieve parallelism, ForkJoinPool forks sources stream and submit for execution, so your source stream should be splittable. For example: [ArrayList](https://java2blog.com/arraylist-in-java-with-example/) is very easy to split, as we can find a middle element by its index and split it but LinkedList is very hard to split and does not perform very well in most of the cases.
- You are actually suffering from performance issues.
- You need to make sure that all the shared resources between threads need to be synchronized properly otherwise it might produce unexpected results.

You could use custom fork join pool

```Java
ForkJoinPool fjp1 = new ForkJoinPool(5);
Callable<Integer> callable1 = () -> data.parallelStream()
  .map(i -> (int) Math.sqrt(i))
  .map(number -> performComputation(number))
  .peek( (i) -> {
    System.out.println("Processing with "+Thread.currentThread()+" "+ i);
  })
  .reduce(0, Integer::sum);

try {
    sumFJ1 = fjp1.submit(callable1).get();
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}
```

**Default Methods**

When a class implements several interfaces that define the same _default_ methods, the code simply won't compile, as there's a conflict caused by multiple interface inheritance (a.k.a the [Diamond Problem](https://en.wikipedia.org/wiki/Multiple_inheritance)). To solve this ambiguity, we must explicitly provide an implementation for the methods:

**Static methods in interfaces**

Since _static_ methods don't belong to a particular object, they're not part of the API of the classes implementing the interface; therefore, they have to be **called by using the interface name preceding the method name**. **Defining a** _**static**_ **method within an interface is identical to defining one in a class.** Moreover, a _static_ method can be invoked within other _static_ and _default_ methods. The idea behind _static_ interface methods is to provide a simple mechanism that allows us to **increase the degree of** [**cohesion**](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) of a design by putting together related methods in one single place without having to create an object.

  

### **Лямбды**

Объект - функция(анонимная функция), основанная на функциональном интерфейсе.Может ссылаться на переменные, объявленные как final, на экземплярные поля класса и статические переменные. Для создания лямбды компилятор генерирует синтетический метод(неявный).

Анонимный класс в смысле языка Java — это совсем не то, что анонимный класс в смысле JVM (говорю о HotSpot). Джавовый анонимный класс для JVM ничем не отличается от обычного. Но в JVM есть именно анонимные классы (создаются через Unsafe.defineAnonymousClass), которые действительно легковеснее обычных. К примеру, они не привязаны к класс-лоадеру. И лямбды (в отличие от анонимных классов Java) материализуются как раз через defineAnonymousClass.

### **Ссылки на методы**

### **Функциональные интерфейсы**

Интерфейсы, которые имеют только один нереализованный метод

**Встроенные:**

|Interface|Parameters|Returns|
|---|---|---|
|Supplier|()|T|
|Consumer|T|void|
|Predicate|T|Boolean|
|Function|T|R|
|Operator|T|T|

### **DateTimeApi**

## **Optional**

Опциональные значения (optionals) являются удобным средством обертывания результата операции для предотвращения NullPointerException. Они позволяют показать пользователю метода, что результат может оказаться пустым. Если передается null, в случае Optional.of будет исключение, в Optional.ofNullable - Optional.empty

### **Monads**

In functional programming, a monad is a software design pattern with a structure that combines program fragments (functions) and wraps their return values in a type with additional computation. It encapsulates an effect — computation that may either result in an exception or return a successfully computed value. This is what Monad does — wraps a value and gives it some effect. That’s it. You just take a value, put it into a monad and get back an amplified value. An “amplified” means that the power of value’s type is increased. All it takes be a Monad is to provide two functions which conform to three laws.

1. Place a value into monadic contextOptional.of
2. Apply a function in monadic context.flatMap(n -> Optional.of(n + 1))

Laws

1. left-identity
2. right-identity
3. associative

Left-identity and right-identity laws basically say that Optional.of function doesn’t change the value in a monad:

Optional.of(x).flatMap(f) == f(x)monad.flatMap(val -> Optional.of(val)) == monad

Associativity law says that when we have a chain of flatMap functions it shouldn’t matter how they’re nested:

monad.flatMap(f).flatMap(g) == monad.flatMap(x ⇒ f(x).flatMap(g))

May me used in some data pipelines for ex streams to prevent failures due to exceptions.

### Questions

- What New Features Were Added in Java 8?
    - **Lambda Expressions** − a new language feature allowing us to treat actions as objects
    - **Method References** − enable us to define Lambda Expressions by referring to methods directly using their names
    - _**Optional**_ − special wrapper class used for expressing optionality
    - **Functional Interface** – an interface with maximum one abstract method; implementation can be provided using a Lambda Expression
    - **Default methods** − give us the ability to add full implementations in interfaces besides abstract methods
    - _**Stream**_ **API** − a special iterator class that allows us to process collections of objects in a functional manner
    - **Date API** − an improved, immutable JodaTime-inspired Date API
- What Is a Method Reference?
    
    A method reference is a Java 8 construct that can be used for referencing a method without invoking it. It’s used for treating methods as Lambda Expressions. They only work as syntactic sugar to reduce the verbosity of some lambdas.
    
    - Constructor reference:  
          
        `String::``**new**``;`
    - Static method reference:  
          
        `String::valueOf;`
    - Bound instance method reference:  
          
        `str::toString;`
    - Unbound instance method reference:  
          
        `String::toString;`
- Default Methods
    
    A default method is a method with an implementation, which can be found in an interface.
    
    We can use a default method to add a new functionality to an interface, while maintaining backward compatibility with classes that are already implementing the interface.
    
    When a class implements several interfaces that define the same _default_ methods, the code simply won't compile, as there's a conflict caused by multiple interface inheritance (a.k.a the [Diamond Problem](https://en.wikipedia.org/wiki/Multiple_inheritance)). To solve this ambiguity, we must explicitly provide an implementation for the methods
    
- What Is a Lambda Expression and What Is It Used For?
    
    A function that we can reference and pass around as an object. lambda expressions are a natural replacement for anonymous classes such as method arguments. One of their main uses is to define inline implementations of functional interfaces. Может ссылаться на переменные, объявленные как final, на экземплярные поля класса и статические переменные. Для создания лямбды компилятор генерирует синтетический метод(неявный).
    
- Lambda visibility
    
    Лямбда может работать с final переменными, которые находятся в том же методе
    
    Анонимные тоже, но еще и с дефолтными методами
    
- What Is a Functional Interface? What Are the Rules of Defining a Functional Interface?
    
    A functional interface is an interface with one single abstract method (_default_ methods do not count), no more, no less.
    
    Where an instance of such an interface is required, a Lambda Expression can be used instead. More formally put: _Functional interfaces_ provide target types for lambda expressions and method references.
    
    The arguments and return type of such an expression directly match those of the single abstract method.
    
    Functional interfaces are usually annotated with the _@FunctionalInterface_ annotation, which is informative and doesn’t affect the semantics.
    
- Describe Some of the Functional Interfaces in the Standard Library
    
    |Interface|Parameters|Returns|
    |---|---|---|
    |Supplier|()|T|
    |Consumer|T|void|
    |Predicate|T|Boolean|
    |Function|T|R|
    |Operator|T|T|
    
- What Is _Optional_? How Can It Be Used?
    
    Optional values (optionals) are convenient for wrapping the operation result to prevent NullPointerException. They show the user of the method that the result may be empty. If passed null, in the case of Optional.of there is an exception, in Optional.ofNullable - Optional.empty
    
    It’s not recommended that we use it as a field value in entity classes, which is clearly indicated by it not implementing the _Serializable_ interface.
    
- What Is a Stream? How Does It Differ From a Collection?
    
    The mechanism that allows you to work with data in a functional style. Stream is an iterator whose role is to accept a set of actions to apply on each of the elements it contains. The _stream_ represents a sequence of objects from a source such as a collection, which supports aggregate operations. They were designed to make collection processing simple and concise.
    
    Another important distinction from collections is that streams are inherently lazily loaded and processed.
    
- What types of operations do Streams have?
    
    There are 2 types of operations:
    
    - **conveyor**(intermediate) - Intermediate operations are those operations that return _Stream_ itself, allowing for further operations on a stream. These operations are always lazy, i.e. they do not process the stream at the call site. An intermediate operation can only process data when there is a terminal operation.
        - filter
        - skip
        - distinct
        - map - wraps its return value inside its ordinal type
        - peek
        - limit
        - sorted
        - flatMap - operation “flattens” the streams into one.
    - **terminal** In contrast, terminal operations terminate the pipeline and initiate stream processing. The stream is passed through all intermediate operations during terminal operation call.
        - findFirst
        - findAny
        - collect
        - count
        - anyMatch
        - noneMatch
        - allMatch
        - min
        - max
        - forEach
        - forEachOrdered
        - reduce
        - toArray
        - **Aggregating operations -**Terminal operations that allow the collection of the resulting data, after the impact on them stream. Run with collect, it can be implemented manually, or with the Collector class. Which accepts the resulting flow type, intermediate type, and type.
            - toList, toMap, toSet, toCollection
            - joining - соединяет элементы стрима в CharSequence
            - counting - возвращает к-во элементов
            - groupingBy - группирует объекты по свойству и складывает их в map. В отличие от toMap принимает вторым параметром коллектор
            - partitioningBy
- Stream vs loop
    
    **Streams**
    
    - Easier to Maintain
    - Better performance for huge lists by using parallel streams
    - Functional in nature
    - Operations performed on a stream do not modify the source.
    - Stream is lazy and evaluates code only when required.
    - The elements of a stream only visited once during the life of a stream.
    - Like an Iterator, a new stream must be generated to revisit the same elements of the source
    
    **Loops**
    
    - Better raw performance
    - Sometimes mor readable
- Parallel Streams
    
    In case of Parallel stream,2 threads are spawned simultaneously and it internally using Fork and Join pool to create and manage threads.Parallel streams create `ForkJoinPool` instance via static `ForkJoinPool.commonPool()` method.
    
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
    - As you know that Java uses `ForkJoinPool` to achieve parallelism, ForkJoinPool forks sources stream and submit for execution, so your source stream should be splittable.For example:[ArrayList](https://java2blog.com/arraylist-in-java-with-example/) is very easy to split, as we can find a middle element by its index and split it but LinkedList is very hard to split and does not perform very well in most of the cases.
    - You are actually suffering from performance issues.
    - You need to make sure that all the shared resources between threads need to be synchronized properly otherwise it might produce unexpected results.
    
    You could use custom fork join pool