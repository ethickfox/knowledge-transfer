
- JSR 335, JEP 126: Language-level support for [lambda expressions](https://en.wikipedia.org/wiki/Lambda_(programming)) (officially, lambda expressions; unofficially, [closures](https://en.wikipedia.org/wiki/Closure_(computer_programming))) under Project Lambda and default methods (virtual [extension methods](https://en.wikipedia.org/wiki/Extension_method)) which can be used to add methods to interfaces without breaking existing implementations. There was an ongoing debate in the Java community on whether to add support for lambda expressions. Sun later declared that lambda expressions would be included in Java and asked for community input to refine the feature. Supporting lambda expressions also enables [functional](https://en.wikipedia.org/wiki/Functional_programming)style operations on streams of elements, such as [MapReduce](https://en.wikipedia.org/wiki/MapReduce)inspired transformations on collections. Default methods can be used by an author of an API to add new methods to an interface without breaking the old code using it. Although it was not their primary intent, default methods can also be used for multiple inheritance of behavior (but not state)
- [JEP 104: Annotation on Java types](https://openjdk.java.net/jeps/104)
- Unsigned integer arithmetic
- [JEP 120: Repeating annotations](https://openjdk.java.net/jeps/120)
- [JEP 150: Date and time API](https://openjdk.java.net/jeps/150)
- [JEP 178: Statically-linked JNI libraries](https://openjdk.java.net/jeps/178)
- [JEP 122: Remove the permanent generation](https://openjdk.java.net/jeps/122)

### **Why are companies still stuck with Java 8?**

There’s a mix of different reasons some companies are still stuck with Java 8. To name a few:

- Build tools (Maven, Gradle etc.) and some libraries _initially_ had bugs with versions Java versions > 8 and needed updates. For example, certain build tools like Maven would print out ["reflective access"-warnings](https://issues.apache.org/jira/browse/GROOVY-8339) when building Java projects, which simply "felt not ready", even though the builds were fine.
- Up until Java 8 you were pretty much using Oracle’s JDK builds and you did not have to care about licensing. Oracle changed the [licensing scheme In 2019](https://www.oracle.com/technetwork/java/javase/overview/oracle-jdk-faqs.html), though, which led the internet go crazy with a ton of articles saying "Java is not free anymore" - and a fair amount of confusion followed. This is however not really an issue, which you’ll learn about in the [Java Distributions](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features#_java_distributions) section of this guide.
- Some companies have policies to only use LTS versions and rely on their OS vendors to provide them these builds, which takes time.

To sum up: you have a mix of practical issues (upgrading your tools, libraries, frameworks) and political issues.

## Lambdas

Before Java 8, whenever you wanted to instantiate, for example, a new Runnable, you had to write an anonymous inner class like so:

```Java
 Runnable runnable = new Runnable(){
       @Override
       public void run(){
         System.out.println("Hello world !");
       }
     };
```

With lambdas, the same code looks like this:

```Java
Runnable runnable = () -> System.out.println("Hello world two!");
```

You also got method references, repeating annotations, default methods for interfaces and a few other language features.

## **Collections & Streams**

In Java 8 you also got functional-style operations for collections, also known as the Stream API. A quick example:

```Java
List<String> list = Arrays.asList("franz", "ferdinand", "fiel", "vom", "pferd");
```

Now pre-Java 8 you basically had to write for-loops to do something with that list.

With the Streams API, you can do the following:

```Java
list.stream()
    .filter(name -> name.startsWith("f"))
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
```

## DateTime API

## Functional Interface

### Embeded Functional interfaces

  

default anD static methods in interfaces

## Optionals

Finally, let's see a tempting, however dangerous, way to use _Optional_s: passing an _Optional_ parameter to a method.

Imagine we have a list of _Person_ and we want a method to search through that list for people with a given name. Also, we would like that method to match entries with at least a certain age, if it's specified.

With this parameter being optional, we come with this method:

```Java
public static List<Person> search(List<Person> people, String name, Optional<Integer> age) {
    // Null checks for people and name
    return people.stream()
            .filter(p -> p.getName().equals(name))
            .filter(p -> p.getAge().get() >= age.orElse(0))
            .collect(Collectors.toList());
}
```

Then we release our method, and another developer tries to use it:

```Java
someObject.search(people, "Peter", null);
```

Now the developer executes its code and gets a _NullPointerException._ **There we are, having to null check our optional parameter, which defeats our initial purpose in wanting to avoid this kind of situation.**

Here are some possibilities we could have done to handle it better:

```Java
public static List<Person> search(List<Person> people, String name, Integer age) {
    // Null checks for people and name
    final Integer ageFilter = age != null ? age : 0;

    return people.stream()
            .filter(p -> p.getName().equals(name))
            .filter(p -> p.getAge().get() >= ageFilter)
            .collect(Collectors.toList());
}
```

There, the parameter's still optional, but we handle it in only one check.

Another possibility would have been to **create two overloaded methods**:

```Java
public static List<Person> search(List<Person> people, String name) {
    return doSearch(people, name, 0);
}

public static List<Person> search(List<Person> people, String name, int age) {
    return doSearch(people, name, age);
}

private static List<Person> doSearch(List<Person> people, String name, int age) {
    // Null checks for people and name
    return people.stream()
            .filter(p -> p.getName().equals(name))
            .filter(p -> p.getAge().get().intValue() >= age)
            .collect(Collectors.toList());
}
```

That way we offer a clear API with two methods doing different things (though they share the implementation).

So, there are solutions to avoid using _Optional_s as method parameters. **The intent of Java when releasing** _**Optional**_ **was to use it as a return type**, thus indicating that a method could return an empty value. As a matter of fact, the practice of using _Optional_ as a method parameter is even [discouraged by some code inspectors](https://rules.sonarsource.com/java/RSPEC-3553).

### Type annotations

Even though we had annotations available before, now we can use them wherever we use a type. This means that we can use them on:

- a local variable definition  
      
    

```Java
public static void main(String[] args) {
        @NotNull String userName = args[0];
}
```

- constructor calls
- type casting
- generics

```Java
public static void main(String[] args) {
        List<@Email String> emails;
}
```

- throw clauses and more

Tools like IDEs can then read these annotations and show warnings or errors based on the annotations.

### Repeating Annotations

Repeating annotations allows us to place multiple annotations on the same class.

```Java
public class RepeatingAnnotations {
    
    @Repeatable(Notifications.class)
    public @interface Notify {
        String email();
    }

    public @interface Notifications {
        Notify[] value();
    }
}
```

We create `@Notify` as a regular annotation, but we add the `@Repeatable` (meta-)annotation to it. Additionally, we have to create a “container” annotation `Notifications` that contains an array of `Notify` objects. An annotation processor can now get access to all repeating `Notify` annotations through the container annotation `Noifications`.

  

We can add a repating annotation multiple times to the same construct:

```Java
@Notify(email = "admin@company.com")
@Notify(email = "owner@company.com")
public class UserNotAllowedForThisActionException
        extends RuntimeException {
    final String user;

    public UserNotAllowedForThisActionException(String user) {
        this.user = user;

    }
}
```

  

## Unmodifiable Collections

# Versions

### Java 1

- [inner classes](https://en.wikipedia.org/wiki/Inner_class) added to the language
- [JavaBeans](https://en.wikipedia.org/wiki/JavaBeans)
- [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity)
- [RMI](https://en.wikipedia.org/wiki/Java_remote_method_invocation) and [serialization](https://en.wikipedia.org/wiki/Serialization)
- [reflection](https://en.wikipedia.org/wiki/Reflection_(computer_science)) which supported Introspection only, no modification at runtime was possible. (The ability to modify objects reflectively was added in J2SE 1.2, by introducing the [AccessibleObject](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/reflect/AccessibleObject.html) class and its subclasses such as the [Field](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/reflect/Field.html) class.)
- [JIT (Just In Time) compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation) on [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) platforms, produced for JavaSoft by Symantec
- [Internationalization](https://en.wikipedia.org/wiki/Internationalization_and_localization) and [Unicode](https://en.wikipedia.org/wiki/Unicode) support originating from [Taligent](https://en.wikipedia.org/wiki/Taligent)
    
    [[19]](https://en.wikipedia.org/wiki/Java_version_history\#cite_note-taligentau-19)
    

### Java 2

- [`strictfp`](https://en.wikipedia.org/wiki/Strictfp) keyword (by JVM 17 an obsolete keyword, should not be used in new code)
- The [Swing](https://en.wikipedia.org/wiki/Swing_(Java)) graphical API was integrated into the core classes.
- Sun's JVM was equipped with a [JIT compiler](https://en.wikipedia.org/wiki/Just-in-time_compilation) for the first time.
- [Java plug-in](https://en.wikipedia.org/wiki/Java_plug-in)
- [Java IDL](https://en.wikipedia.org/wiki/Java_IDL), an [IDL](https://en.wikipedia.org/wiki/Interface_description_language) implementation for [CORBA](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture) interoperability
- [Collections](https://en.wikipedia.org/wiki/Java_collections_framework) framework

### Java 3

- [HotSpot](https://en.wikipedia.org/wiki/HotSpot_(virtual_machine)) JVM included (the HotSpot JVM was first released in April 1999 for the J2SE 1.2 JVM)
- [RMI](https://en.wikipedia.org/wiki/Java_remote_method_invocation) was modified to support optional compatibility with [CORBA](https://en.wikipedia.org/wiki/CORBA).
- [Java Naming and Directory Interface](https://en.wikipedia.org/wiki/Java_Naming_and_Directory_Interface) (JNDI) included in core libraries (previously available as an extension)
- [Java Platform Debugger Architecture](https://en.wikipedia.org/wiki/Java_Platform_Debugger_Architecture) (JPDA)
- JavaSound
- Synthetic proxy classes

### Java 4

- Language changes
    - [`assert`](https://en.wikipedia.org/wiki/Assertion_(computing)) keyword (specified in [JSR 41](https://web.archive.org/web/20080616233205/http://www.jcp.org/en/jsr/detail?id=41))
- Library improvements
    - [Regular expressions](https://en.wikipedia.org/wiki/Regular_expressions) modeled after [Perl](https://en.wikipedia.org/wiki/Perl) regular expressions
    - [Exception chaining](https://en.wikipedia.org/wiki/Exception_chaining) allows an exception to encapsulate original lower-level exception
    - Internet Protocol version 6 ([IPv6](https://en.wikipedia.org/wiki/IPv6)) support
    - [Non-blocking I/O](https://en.wikipedia.org/wiki/Non-blocking_I/O_(Java)) (named NIO) (specified in [JSR 51](http://www.jcp.org/en/jsr/detail?id=51))
    - Logging API (specified in [JSR 47](https://www.jcp.org/en/jsr/detail?id=47))
    - Image I/O API for reading and writing images in formats like [JPEG](https://en.wikipedia.org/wiki/JPEG) and [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics)
    - Integrated [XML](https://en.wikipedia.org/wiki/XML) parser and [XSLT](https://en.wikipedia.org/wiki/XSLT) processor ([JAXP](https://en.wikipedia.org/wiki/JAXP)) (specified in [JSR 5](http://www.jcp.org/en/jsr/detail?id=5) and [JSR 63](https://www.jcp.org/en/jsr/detail?id=63))
    - Integrated security and cryptography extensions ([JCE](https://en.wikipedia.org/wiki/Java_Cryptography_Extension), [JSSE](https://en.wikipedia.org/wiki/JSSE), [JAAS](https://en.wikipedia.org/wiki/Java_Authentication_and_Authorization_Service))
    - [Java Web Start](https://en.wikipedia.org/wiki/Java_Web_Start) included (Java Web Start was first released in March 2001 for J2SE 1.3) (specified in [JSR 56](https://www.jcp.org/en/jsr/detail?id=56))
    - Preferences API (`java.util.prefs`)

### Java 5

- [Generics](https://en.wikipedia.org/wiki/Generics_in_Java): provides compile-time (static) [type safety](https://en.wikipedia.org/wiki/Type_safety) for collections and eliminates the need for most [typecasts (type conversion)](https://en.wikipedia.org/wiki/Type_conversion) (specified by [JSR 14](http://www.jcp.org/en/jsr/detail?id=14))
- [Metadata](https://en.wikipedia.org/wiki/Metadata_(computing)): also called [annotations](https://en.wikipedia.org/wiki/Java_annotation); allows language constructs such as classes and methods to be tagged with additional data, which can then be processed by metadata-aware utilities (specified by [JSR 175](http://www.jcp.org/en/jsr/detail?id=175))
- [Autoboxing](https://en.wikipedia.org/wiki/Autoboxing)/unboxing: automatic conversions between [primitive types](https://en.wikipedia.org/wiki/Primitive_type) (such as `int`) and [primitive wrapper classes](https://en.wikipedia.org/wiki/Primitive_wrapper_class) (such as [`Integer`](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Integer.html)) (specified by [JSR 201](http://www.jcp.org/en/jsr/detail?id=201))
- [Enumerations](https://en.wikipedia.org/wiki/Enumeration_(programming)): the `enum` keyword creates a [typesafe](https://en.wikipedia.org/wiki/Type_safety), ordered list of values (such as `Day.MONDAY`, `Day.TUESDAY`, etc.); previously this could only be achieved by non-typesafe constant integers or manually constructed classes (typesafe enum pattern) (specified by [JSR 201](http://www.jcp.org/en/jsr/detail?id=201))
- [Varargs](https://en.wikipedia.org/wiki/Java_syntax#Varargs): the last parameter of a method can now be declared using a type name followed by three dots (e.g. `void drawtext(String... lines)`); in the calling code any number of parameters of that type can be used and they are then placed in an array to be passed to the method, or alternatively the calling code can pass an array of that type
- Enhanced [`for each`](https://en.wikipedia.org/wiki/Foreach) loop: the `for` loop syntax is extended with special syntax for iterating over each member of either an array or any [`Iterable`](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Iterable.html), such as the standard [`Collection`](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/Collection.html) classes (specified by [JSR 201](http://www.jcp.org/en/jsr/detail?id=201))
- Improved semantics of execution for multi-threaded Java programs; the new [Java memory model](https://en.wikipedia.org/wiki/Java_memory_model) addresses issues of complexity, effectiveness, and performance of previous specifications
- [Static imports](https://en.wikipedia.org/wiki/Static_import)

### Java 6

- Scripting Language Support ([JSR 223](https://en.wikipedia.org/wiki/JSR_223)): Generic API for tight integration with scripting languages, and built-in [Mozilla](https://en.wikipedia.org/wiki/Mozilla) [JavaScript](https://en.wikipedia.org/wiki/JavaScript) [Rhino](https://en.wikipedia.org/wiki/Rhino_(JavaScript_engine)) integration.
- Dramatic performance improvements for the core platform, and [Swing](https://en.wikipedia.org/wiki/Swing_(Java)).
- Improved Web Service support through [JAX-WS](https://en.wikipedia.org/wiki/JAX-WS) ([JSR 224](https://en.wikipedia.org/wiki/JSR_224)).
- [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) 4.0 support ([JSR 221](https://en.wikipedia.org/wiki/JSR_221)).
- Java Compiler API ([JSR 199](https://en.wikipedia.org/wiki/JSR_199)): an API allowing a Java program to select and invoke a Java Compiler programmatically.
- Upgrade of [JAXB](https://en.wikipedia.org/wiki/JAXB) to version 2.0: Including integration of a [StAX](https://en.wikipedia.org/wiki/StAX) parser.
- [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine) improvements include: [synchronization](https://en.wikipedia.org/wiki/Data_synchronization) and [compiler](https://en.wikipedia.org/wiki/Compiler) performance optimizations, new algorithms and upgrades to existing [garbage collection algorithms](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)), and application start-up performance.

### Java 7

- [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine) support for [dynamic languages](https://en.wikipedia.org/wiki/Dynamic_programming_language), with the new `invokedynamic` bytecode under JSR-292, following the prototyping work currently done on the [Multi Language Virtual Machine](https://en.wikipedia.org/wiki/Da_Vinci_Machine)
- Compressed 64-bit pointers (available in Java 6 with `XX:+UseCompressedOops`)
- These small language changes (grouped under a project named Coin):

- Strings in switch
- Automatic resource management in try-statement aka _try-with-resources statement
- Improved [type inference](https://en.wikipedia.org/wiki/Type_inference) for generic instance creation, aka _the diamond operator_ `<>`
- Simplified varargs method declaration
- Binary integer literals
- Allowing underscores in numeric literals
- Catching multiple exception types and rethrowing exceptions with improved type checking
- Concurrency utilities under JSR 166
- New file [I/O](https://en.wikipedia.org/wiki/I/O) library (defined by JSR 203) adding support for multiple file systems, file metadata and symbolic links. The new packages are `java.nio.file`, `java.nio.file.attribute` and `java.nio.file.spi`
- [Timsort](https://en.wikipedia.org/wiki/Timsort) is used to sort collections and arrays of objects instead of [merge sort](https://en.wikipedia.org/wiki/Merge_sort)
- Library-level support for [elliptic curve cryptography](https://en.wikipedia.org/wiki/Elliptic_curve_cryptography) algorithms
- Enhanced library-level support for new network protocols, including [SCTP](https://en.wikipedia.org/wiki/Stream_Control_Transmission_Protocol) and [Sockets Direct Protocol](https://en.wikipedia.org/wiki/Sockets_Direct_Protocol)
- [Upstream](https://en.wikipedia.org/wiki/Upstream_(software_development)) updates to [XML](https://en.wikipedia.org/wiki/XML) and [Unicode](https://en.wikipedia.org/wiki/Unicode)
- Java deployment rule sets