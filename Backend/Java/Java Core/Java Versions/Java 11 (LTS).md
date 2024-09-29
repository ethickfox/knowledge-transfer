  

### Java 9

- JSR 376: Modularization of the JDK under Project Jigsaw ([Java Platform Module System](https://en.wikipedia.org/wiki/Java_Platform_Module_System))
- [JavaDB](https://en.wikipedia.org/wiki/JavaDB) was removed from JDK
- [JEP 193: Variable handles](https://openjdk.java.net/jeps/193), define a standard means to invoke the equivalents of various `java.util.concurrent.atomic` and `sun.misc.Unsafe` operations
- [JEP 213: Milling Project Coin](https://openjdk.java.net/jeps/213), allow @SafeVarargs on private instance methods; Allow effectively-final variables to be used as resources in the try-with-resources statement; Allow diamond with anonymous classes if the argument type of the inferred type is denotable; Complete the removal, begun in Java SE 8, of underscore from the set of legal identifier names; Support for private methods in interfaces
- [JEP 222: jshell: The Java Shell (Read-Eval-Print Loop)](https://openjdk.java.net/jeps/222): [JShell](https://en.wikipedia.org/wiki/JShell) is a [REPL](https://en.wikipedia.org/wiki/REPL) command-line interface for the Java language.
- [JEP 254: Compact Strings](https://openjdk.java.net/jeps/254)
- [JEP 266: More concurrency updates](https://openjdk.java.net/jeps/266), it includes a Java implementation of [Reactive Streams](https://en.wikipedia.org/wiki/Reactive_Streams), including a new `Flow` class that included the interfaces previously provided by Reactive Streams
- [JEP 268: XML catalogs](https://openjdk.java.net/jeps/268)
- [JEP 282: jlink: The Java Linker](https://openjdk.java.net/jeps/282), create a tool that can assemble and optimize a set of modules and their dependencies into a custom run-time image. It effectively allows to produce a fully usable executable including the JVM to run it
- [JEP 295: Ahead-of-Time Compilation](https://openjdk.java.net/jeps/295), [ahead-of-time compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) provided by [GraalVM](https://en.wikipedia.org/wiki/GraalVM)

### **Java Modules**

Java 9 got the [Jigsaw Module System](https://www.oracle.com/corporate/features/understanding-java-9-modules.html), which somewhat resembles the good old [OSGI specification](https://en.wikipedia.org/wiki/OSGi). It is not in the scope of this guide to go into full detail on Jigsaw, but have a look at the previous links to learn more.

When creating the new module, we need to provide several attributes:

- Name
- Dependencies
- Public Packages - by default, all packages are module private
- Services Offered
- Services Consumed
- Reflection Permissions

```Java
module world.module {
    exports com.reflectoring.io.app.world;
}
```

```Java
module hello.module {
    requires world.module;
}
```

### **Multi-Release Jar Files**

Multi-Release .jar files made it possible to have one .jar file which contains different classes for different JVM versions. So your program can behave differently/have different classes used when run on Java 8 vs. Java 10, for example.

### Changed versioning 1.8 -> 9

### JDK License

### **Collections**

Collections got a couple of new helper methods, to easily construct Lists, Sets and Maps.

```Java
List<String> list = List.of("one", "two", "three");
Set<String> set = Set.of("one", "two", "three");
Map<String, String> map = Map.of("foo", "one", "bar", "two");
```

### **JShell**

Finally, Java got a shell where you can try out simple commands and get immediate results.

```Java
% jshell
|  Welcome to JShell -- Version 9
|  For an introduction type: /help intro

jshell> int x = 10
x ==> 10
```

### **Optionals**

Optionals got the sorely missed ifPresentOrElse method.

```Java
user.ifPresentOrElse(this::displayAccount, this::displayLogin);
```

### **Streams**

Streams got a couple of additions, in the form of takeWhile,dropWhile,iterate methods.

```Java
Stream<String> stream = Stream.iterate("", s -> s + "s")
  .takeWhile(s -> s.length() < 10);
```

### **Interfaces**

Interfaces got private methods:

```Java
public interface MyInterface {

    private static void myPrivateMethod(){
        System.out.println("Yay, I am private!");
    }
}
```

### Try-with-resources final

As of Java 9 and as part of [JEP 213](https://openjdk.java.net/jeps/213), **we can now use** [_**final**_](https://www.baeldung.com/java-effectively-final) [**or even effectively final**](https://www.baeldung.com/java-effectively-final) **variables inside a** _**try-with-resources**_ **block**:

```Java
final Scanner scanner = new Scanner(new File("testRead.txt"));
PrintWriter writer = new PrintWriter(new File("testWrite.txt"))
try (scanner;writer) { 
    // omitted
}
```

Put simply, a variable is effectively final if it doesn't change after the first assignment, even though it's not explicitly marked as _final_.

### **Diamond Syntax with Inner Anonymous Classes**

## **HTTPClient**

Java 9 brought the initial preview version of a new HttpClient. Up until then, Java’s built-in Http support was rather low-level, and you had to fall back on using third-party libraries like Apache HttpClient or OkHttp (which are great libraries, btw!).

With Java 9, Java got its own, modern client - although in preview mode, which means subject to change in later Java versions.

### Java 10

- [JEP 286: Local-Variable Type Inference](https://openjdk.java.net/jeps/286)
- [JEP 296: Consolidate the JDK Forest into a Single Repository](https://openjdk.java.net/jeps/296)
- [JEP 304: Garbage-Collector Interface](https://openjdk.java.net/jeps/304)
- [JEP 307: Parallel Full GC for G1](https://openjdk.java.net/jeps/307)
- [JEP 310: Application Class-Data Sharing](https://openjdk.java.net/jeps/310)
- [JEP 312: Thread-Local Handshakes](https://openjdk.java.net/jeps/312)
- [JEP 313: Remove the Native-Header Generation Tool (javah)](https://openjdk.java.net/jeps/313)
- [JEP 314: Additional Unicode Language-Tag Extensions](https://openjdk.java.net/jeps/314)
- [JEP 316: Heap Allocation on Alternative Memory Devices](https://openjdk.java.net/jeps/316)
- [JEP 317: Experimental Java-Based JIT Compiler](https://openjdk.java.net/jeps/317)
- [JEP 319: Root Certificates](https://openjdk.java.net/jeps/319)
- [JEP 322: Time-Based Release Versioning](https://openjdk.java.net/jeps/322)

There have been a few changes to Java 10, like Garbage Collection etc. But the only real change you as a developer will likely see is the introduction of the "var"-keyword, also called local-variable type inference.

## **Local-Variable Type Inference: var-keyword**

### **Local-Variable Type Inference: var-keyword**

```Java
// Pre-Java 10
String myName = "Marco";

// With Java 10
var myName = "Marco"
```

Feels Javascript-y, doesn’t it? It is still strongly typed, though, and only applies to variables _inside methods_

Unmodifiable collecions - new methods

JVMs aware of being run in a Docker container

Parallel Full GC for G1

Java 10 was the last free Oracle JDK release that we could use commercially without a license

### Java 11

- [JEP 181: Nest-Based Access Control](https://openjdk.java.net/jeps/181)
- [JEP 309: Dynamic Class-File Constants](https://openjdk.java.net/jeps/309)
- [JEP 315: Improve Aarch64 Intrinsics](https://openjdk.java.net/jeps/315)
- [JEP 318: Epsilon: A No-Op Garbage Collector](https://openjdk.java.net/jeps/318)
- [JEP 320: Remove the Java EE and CORBA Modules](https://openjdk.java.net/jeps/320)
- [JEP 321: HTTP Client (Standard)](https://openjdk.java.net/jeps/321)
- [JEP 323: Local-Variable Syntax for Lambda Parameters](https://openjdk.java.net/jeps/323)
- [JEP 324: Key Agreement with Curve25519 and Curve448](https://openjdk.java.net/jeps/324)
- [JEP 327: Unicode 10](https://openjdk.java.net/jeps/327)
- [JEP 328: Flight Recorder](https://openjdk.java.net/jeps/328)
- [JEP 329: ChaCha20 and Poly1305 Cryptographic Algorithms](https://openjdk.java.net/jeps/329)
- [JEP 330: Launch Single-File Source-Code Programs](https://openjdk.java.net/jeps/330)
- [JEP 331: Low-Overhead Heap Profiling](https://openjdk.java.net/jeps/331)
- [JEP 332: Transport Layer Security (TLS) 1.3](https://openjdk.java.net/jeps/332)
- [JEP 335: Deprecate the Nashorn JavaScript Engine](https://openjdk.java.net/jeps/335)
- [JEP 336: Deprecate the Pack200 Tools and API](https://openjdk.java.net/jeps/336)

## **Strings & Files**

Strings and Files got a couple new methods (not all listed here):

```Java
"Marco".isBlank();
"Mar\nco".lines();
"Marco  ".strip();

Path path = Files.writeString(Files.createTempFile("helloworld", ".txt"), "Hi, my name is!");
String s = Files.readString(path);
```

**Local-Variable Type Inference (var) for lambda parameters**

```Java
(var firstName, var lastName) -> firstName + lastName
```

## **Run Source Files**

Starting with Java 10, you can run Java source files _without_ having to compile them first. A step towards scripting.

## Other

Java Flight Recorder (JFR) is now open-source in Open JDK;