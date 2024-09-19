### Java 18

- [JEP 400: UTF-8 by Default](https://openjdk.java.net/jeps/400)
- [JEP 408: Simple Web Server](https://openjdk.java.net/jeps/408)
- [JEP 413: Code Snippets in Java API Documentation](https://openjdk.java.net/jeps/413)
- [JEP 416: Reimplement Core Reflection with Method Handles](https://openjdk.java.net/jeps/416)
- [JEP 418: Internet-Address Resolution SPI](https://openjdk.java.net/jeps/418)
- [JEP 421: Deprecate Finalization for Removal](https://openjdk.java.net/jeps/421)

### **UTF-8 by Default**

If you tried, e.g. reading in files without specifying an explicit character ending, the operating system encoding was used in previous Java versions (e.g. UTF-8 on Linux and macOS, and Windows-1252 on Windows). With Java 18 this changed to UTF-8 by default.

### **Simple Web Server**

Java 18 now comes with a rudimentary HTTP server, that you can start with:

`jwebserver`

Learn more about its features [here](https://openjdk.org/jeps/408).

### Java 19

All JEPs were either incubators or previews.

### Java 20

All JEPs were either incubators or previews

### Java 21

- [JEP 430: String Templates (Preview)](https://openjdk.java.net/jeps/430)
- [JEP 431: Sequenced Collections](https://openjdk.java.net/jeps/431)
- [JEP 439: Generational ZGC](https://openjdk.java.net/jeps/439)
- [JEP 440: Record Patterns](https://openjdk.java.net/jeps/440)
- [JEP 441: Pattern Matching for switch](https://openjdk.java.net/jeps/441)
- [JEP 442: Foreign Function & Memory API (Third Preview)](https://openjdk.java.net/jeps/442)
- [JEP 443: Unnamed Patterns and Variables (Preview)](https://openjdk.java.net/jeps/443)
- [JEP 444: Virtual Threads](https://openjdk.java.net/jeps/444)
- [JEP 445: Unnamed Classes and Instance Main Methods (Preview)](https://openjdk.java.net/jeps/445)
- [JEP 446: Scoped Values (Preview)](https://openjdk.java.net/jeps/446)
- [JEP 448: Vector API (Sixth Incubator)](https://openjdk.java.net/jeps/448)
- [JEP 449: Deprecate the Windows 32-bit x86 Port for Removal](https://openjdk.java.net/jeps/449)
- [JEP 451: Prepare to Disallow the Dynamic Loading of Agents](https://openjdk.java.net/jeps/451)
- [JEP 452: Key Encapsulation Mechanism API](https://openjdk.java.net/jeps/452)
- [JEP 453: Structured Concurrency (Preview)](https://openjdk.java.net/jeps/453)

## **Pattern Matching for switch (Third Preview) – JEP 427**

n Java 19, [JDK Enhancement Proposal 427](https://openjdk.org/jeps/427) changed the syntax of the so-called "Guarded Pattern" (in the example above "`String s && s.length() > 5`"). Instead of `&&`, we now have to use the new keyword `when`.

The example from above is notated in Java 19 as follows:

```Plain
switch (obj) {
  case String s when s.length() > 5 -> System.out.println(s.toUpperCase());
  case String s                     -> System.out.println(s.toLowerCase());

  case Integer i                    -> System.out.println(i * i);

  default -> {}
}Code language: Java (java)
```

`when` is a so-called "contextual keyword" and therefore only has a meaning within a `case` label. If you have variables or methods with the name "when" in your code, you don't need to change them.

# R**ecord Patterns (Preview) – JEP 405**

We stay with the topic "pattern matching" and come to "record patterns". If the subject "records" is new to you, I recommend reading the article "[Records in Java](https://www.happycoders.eu/java/records/)" first.

I'll best explain what a record pattern is with an example. Let's assume we have defined the following record:

```Plain
public record Position(int x, int y) {}Code language: Java (java)
```

We also have a `print()` method that can print any object, including positions:

```Plain
private void print(Object object) {
  if (object instanceof Position position) {
    System.out.println("object is a position, x = " + position.x()
                                         + ", y = " + position.y());
  }
  // else ...
}
```

As of Java 19, [JDK Enhancement Proposal 405](https://openjdk.org/jeps/405) allows us to use a so-called "record pattern". This allows us to write the code as follows:

```Plain
private void print(Object object) {
  if (object instanceof Position(int x, int y)) {
    System.out.println("object is a position, x = " + x + ", y = " + y);
  }
  // else ...
}Code language: Java (java)
```

Instead of matching on "`Position position`" and accessing `position` in the following code, we now match on "`Position(int x, int y)`" and can then access `x` and `y` directly.

# **Virtual Threads (Preview) – JEP 425**

The most exciting innovation in Java 19 for me is "Virtual Threads". Virtual threads have been developed in [Project Loom](https://openjdk.org/projects/loom/) for several years and could only be tested with a self-compiled JDK so far.

With [JDK Enhancement Proposal 425](https://openjdk.org/jeps/425), virtual threads finally make their way into the official JDK – and they do so directly in the preview stage, so no more significant changes to the API are expected.

To find out why we need virtual threads, what they are, how they work, and how to use them, check out the [main article on virtual threads](https://www.happycoders.eu/java/virtual-threads/). You definitely shouldn't miss it.

# **Structured Concurrency (Incubator) – JEP 428**

Also developed in Project Loom and initially released as an incubator feature in Java 19 with [JDK Enhancement Proposal 428](https://openjdk.org/jeps/428) is the so-called "Structured Concurrency."

When a task consists of several subtasks that can be processed in parallel, Structured Concurrency allows us to implement this in a particularly readable and maintainable way.

You can learn more about how this works in the [main article about Structured Concurrency](https://www.happycoders.eu/java/structured-concurrency-structuredtaskscope/).

# **Foreign Function & Memory API (Preview) – JEP 424**

In [Project Panama](https://openjdk.org/projects/panama/), a replacement for the cumbersome, error-prone, and slow Java Native Interface (JNI) has been in the works for a long time.

The "Foreign Memory Access API" and the "Foreign Linker API" were already introduced in [Java 14](https://www.happycoders.eu/java/java-14-features/#Foreign-Memory_Access_API_Incubator)and [Java 16](https://www.happycoders.eu/java/java-16-features/#Foreign_Linker_API_Incubator_Foreign-Memory_Access_API_Third_Incubator) – both initially individually in the incubator stage. In [Java 17](https://www.happycoders.eu/java/java-17-features/#Foreign_Function_Memory_API_Incubator), these APIs were combined to form the "Foreign Function & Memory API" (FFM API), which remained in the incubator stage until [Java 18](https://www.happycoders.eu/java/java-18-features/#Foreign_Function_Memory_API_Second_Incubator).

In Java 19, [JDK Enhancement Proposal 424](https://openjdk.org/jeps/424) finally promoted the new API to the preview stage, which means that only minor changes and bug fixes will be made. So it's time to introduce the new API!

The Foreign Function & Memory API enables access to native memory (i.e., memory outside the Java heap) and access to native code (e.g., C libraries) directly from Java.

I will show how this works with an example. However, I won't go too deep into the topic here since most Java developers rarely (or never) need to access native memory and code.

Here is a simple example that stores a string in off-heap memory and calls the "strlen" function of the C standard library on it:

```Plain
public class FFMTest {
  public static void main(String[] args) throws Throwable {
    // 1. Get a lookup object for commonly used libraries
    SymbolLookup stdlib = Linker.nativeLinker().defaultLookup();

    // 2. Get a handle to the "strlen" function in the C standard library
    MethodHandle strlen = Linker.nativeLinker().downcallHandle(
        stdlib.lookup("strlen").orElseThrow(),
        FunctionDescriptor.of(JAVA_LONG, ADDRESS));

    // 3. Convert Java String to C string and store it in off-heap memory
    MemorySegment str = implicitAllocator().allocateUtf8String("Happy Coding!");

    // 4. Invoke the foreign function
    long len = (long) strlen.invoke(str);

    System.out.println("len = " + len);
  }
}
Code language: Java (java)
```

Interesting is the `FunctionDescriptor` in line 9: it expects as the first parameter the return type of the function and as additional parameters the function's arguments. The `FunctionDescriptor`ensures that all Java types are adequately converted to C types and vice versa.

Since the FFM API is still in the preview stage, we must specify a few additional parameters to compile and start it:

```Plain
$ javac --enable-preview -source 19 FFMTest.java
$ java --enable-preview FFMTestCode language: plaintext (plaintext)
```

Anyone who has worked with JNI – and remembers how much Java and C boilerplate code you had to write and keep in sync – will realize that the effort required to call the native function has been reduced by orders of magnitude.

If you want to delve deeper into the matter: you can find more complex examples in the [JEP](https://openjdk.org/jeps/424).

# **Vector API (Fourth Incubator) – JEP 426**

The new Vector API has nothing to do with the `java.util.Vector` class. In fact, it is about a new API for mathematical vector computation and its mapping to modern SIMD (Single-Instruction-Multiple-Data) CPUs.

The Vector API has been part of the JDK since [Java 16](https://www.happycoders.eu/java/java-16-features/#Vector_API_Incubator) as an incubator and was further developed in [Java 17](https://www.happycoders.eu/java/java-17-features/#Vector_API_Second_Incubator) and [Java 18](https://www.happycoders.eu/java/java-18-features/#Vector_API_Third_Incubator).

With [JDK Enhancement Proposal 426](https://openjdk.org/jeps/426), Java 19 delivers the fourth iteration in which the API has been extended to include new vector operations – as well as the ability to store vectors in and read them from memory segments (a feature of the [Foreign Function & Memory API](https://www.happycoders.eu/java/java-19-features/#Foreign_Function_Memory_API_Preview_-_JEP_424)).

Incubator features may still be subject to significant changes, so that I won't present the API in detail here. I will do that as soon as the Vector API has moved to the preview stage.