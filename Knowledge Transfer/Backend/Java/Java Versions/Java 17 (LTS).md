Java 17 is the new long-term support (LTS) release of Java, after Java 11.

### Java 12

Java 12 got a couple [new features and clean-ups](https://www.oracle.com/technetwork/java/javase/12-relnote-issues-5211422.html), but the only ones worth mentioning here are Unicode 11 support and a preview of the new switch expression, which you will see covered in the next section.

- [JEP 230: Microbenchmark Suite](https://openjdk.java.net/jeps/230)
- [JEP 334: JVM Constants API](https://openjdk.java.net/jeps/334)
- [JEP 341: Default CDS Archives](https://openjdk.java.net/jeps/341)
- [JEP 344: Abortable Mixed Collections for G1](https://openjdk.java.net/jeps/344)
- [JEP 346: Promptly Return Unused Committed Memory from G1](https://openjdk.java.net/jeps/346)

### Java 13

- [JEP 350: Dynamic CDS Archives](https://openjdk.java.net/jeps/350)
- [JEP 351: ZGC: Uncommit Unused Memory](https://openjdk.java.net/jeps/351)
- [JEP 353: Reimplement the Legacy Socket API](https://openjdk.java.net/jeps/353)

Unicode 12.1 support

### **Switch Expression (Preview)**

Switch expressions can now return a value. And you can use a lambda-style syntax for your expressions, without the fall-through/break issues:

Old switch statements looked like this:

```Java
switch(status) {
  case SUBSCRIBER:
    // code blockbreak;
  case FREE_TRIAL:
    // code blockbreak;
  default:
    // code block}
```

Whereas with Java 13, switch statements can look like this:

```Java
boolean result = switch (status) {
    case SUBSCRIBER -> true;
    case FREE_TRIAL -> false;
    default -> throw new IllegalArgumentException("something is murky!");
};
```

### **Multiline Strings (Preview)**

You can _finally_ do this in Java:

```Java
String htmlBeforeJava13 = "<html>\n" +
              "    <body>\n" +
              "        <p>Hello, world</p>\n" +
              "    </body>\n" +
              "</html>\n";

String htmlWithJava13 = """
              <html>
                  <body>
                      <p>Hello, world</p>
                  </body>
              </html>
              """;
```

### Java 14

- [JEP 345: NUMA-Aware Memory Allocation for G1](https://openjdk.java.net/jeps/345)
- [JEP 349: JFR Event Streaming](https://openjdk.java.net/jeps/349)
- [JEP 352: Non-Volatile Mapped Byte Buffers](https://openjdk.java.net/jeps/352)
- [JEP 358: Helpful NullPointerExceptions](https://openjdk.java.net/jeps/358)
- [JEP 362: Deprecate the Solaris and SPARC Ports](https://openjdk.java.net/jeps/362)
- [JEP 363: Remove the Concurrent Mark Sweep (CMS) Garbage Collector](https://openjdk.java.net/jeps/363)
- [JEP 364: ZGC on macOS](https://openjdk.java.net/jeps/364)
- [JEP 365: ZGC on Windows](https://openjdk.java.net/jeps/365)
- [JEP 366: Deprecate the ParallelScavenge + SerialOld GC Combination](https://openjdk.java.net/jeps/366)
- [JEP 367: Remove the Pack200 Tools and API](https://openjdk.java.net/jeps/367)

### **Switch Expression (Standard)**

The switch expressions that were _preview_ in versions 12 and 13, are now standardised.

Keyword yeld

```Java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY                -> 7;
    default      -> {
      String s = day.toString();
      int result = s.length();
      yield result;
    }
};
```

### **Records (Preview)**

There are now record classes, which help alleviate the pain of writing a lot of boilerplate with Java.

Have a look at this pre Java 14 class, which only contains data, (potentially) getters/setters, equals/hashcode, toString.

```Java
final class Point {
    public final int x;
    public final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
    // state-based implementations of equals, hashCode, toString// nothing else
```

With records, it can now be written like this:

```Java
record Point(int x, int y) { }
```

Again, this is a preview feature and subject to change in future releases.

### **Helpful NullPointerExceptions**

Finally NullPointerExceptions describe _exactly_ which variable was null.

```Java
author.age = 35;
---
Exception in thread "main" java.lang.NullPointerException:
     Cannot assign field "age" because "author" is null
```

### **Pattern Matching For InstanceOf (Preview)**

Whereas previously you had to (cast) your objects inside an instanceof like this:

```Java
if (obj instanceof String) {
    String s = (String) obj;
    // use s
}
```

You can now do this, effectively dropping the cast.

```Java
if (obj instanceof String s) {
    System.out.println(s.contains("hello"));
}
```

### **Packaging Tool (Incubator)**

There’s an incubating _jpackage_ tool, which allows to package your Java application into platform-specific packages, including all necessary dependencies.

- Linux: deb and rpm
- macOS: pkg and dmg
- Windows: msi and exe

### **Garbage Collectors**

The Concurrent Mark Sweep (CMS) Garbage Collector has been removed, and the experimental Z Garbage Collector has been added.

### Java 15

- [JEP 339: Edwards-Curve Digital Signature Algorithm (EdDSA)](https://openjdk.java.net/jeps/339)
- [JEP 371: Hidden Classes](https://openjdk.java.net/jeps/371)
- [JEP 372: Remove the Nashorn JavaScript Engine](https://openjdk.java.net/jeps/372)
- [JEP 373: Reimplement the Legacy DatagramSocket API](https://openjdk.java.net/jeps/373)
- [JEP 374: Disable and Deprecate Biased Locking](https://openjdk.java.net/jeps/374)
- [JEP 377: ZGC: A Scalable Low-Latency Garbage Collector](https://openjdk.java.net/jeps/377)
- [JEP 378: Text Blocks](https://openjdk.java.net/jeps/378)
- [JEP 379: Shenandoah: A Low-Pause-Time Garbage Collector](https://openjdk.java.net/jeps/379)
- [JEP 381: Remove the Solaris and SPARC Ports](https://openjdk.java.net/jeps/381)
- [JEP 385: Deprecate RMI Activation for Removal](https://openjdk.java.net/jeps/385)

### **Text-Blocks / Multiline Strings**

Introduced as an experimental feature in Java 13 (see above), multiline strings are now production-ready.

```Java
String text = """
                Lorem ipsum dolor sit amet, consectetur adipiscing \
                elit, sed do eiusmod tempor incididunt ut labore \
                et dolore magna aliqua.\
                """;
```

### **Nashorn JavaScript Engine**

After having been deprecated in Java 11, the Nashorn Javascript Engine was now finally removed in JDK 15.

### **ZGC: Production Ready**

The [Z Garbage Collector](https://wiki.openjdk.java.net/display/zgc/Main) is not marked experimental anymore. It’s now production-ready.

### Java 16

- [JEP 347: Enable C++14 Language Features](https://openjdk.java.net/jeps/347)
- [JEP 357: Migrate from Mercurial to Git](https://openjdk.java.net/jeps/357)
- [JEP 369: Migrate to GitHub](https://openjdk.java.net/jeps/369)
- [JEP 376: ZGC: Concurrent Thread-Stack Processing](https://openjdk.java.net/jeps/376)
- [JEP 380: Unix-Domain Socket Channels](https://openjdk.java.net/jeps/380)
- [JEP 387: Elastic Metaspace](https://openjdk.java.net/jeps/387)
- [JEP 388: Windows/AArch64 Port](https://openjdk.java.net/jeps/388)
- [JEP 390: Warnings for Value-Based Classes](https://openjdk.java.net/jeps/390)
- [JEP 392: Packaging Tool](https://openjdk.java.net/jeps/392)
- [JEP 394: Pattern Matching for instanceof](https://openjdk.java.net/jeps/394)
- [JEP 395: Records](https://openjdk.java.net/jeps/395)
- [JEP 396: Strongly Encapsulate JDK Internals by Default](https://openjdk.java.net/jeps/396)
- [JEP 397: Sealed Classes (Second Preview)](https://openjdk.java.net/jeps/397)

### **Pattern Matching for instanceof**

Instead of:

```Java
if (obj instanceof String) {
    String s = (String) obj;
    // e.g. s.substring(1)}
```

You can now do this:

```Java
if (obj instanceof String s) {
    // Let pattern matching do the work!// ... s.substring(1)}
```

```Java
public class PatternMatching {
    public static double price(Vehicle v) {
        if (v instanceof Car c) {
            return 10000 - c.kilomenters * 0.01 -
                    (Calendar.getInstance().get(Calendar.YEAR) -
                            c.year) * 100;
        } else if (v instanceof Bicycle b) {
            return 1000 + b.wheelSize * 10;
        } else throw new IllegalArgumentException();
    }
}
```

### **Unix-Domain Socket Channels**

You can now connect to Unix domain sockets (also supported by macOS and Windows (10+).

```Java
 socket.connect(UnixDomainSocketAddress.of(
        "/var/run/postgresql/.s.PGSQL.5432"));
```

### **Foreign Linker API - Preview**

A planned replacement for JNI (Java Native Interface), allowing you to bind to native libraries (think C).

### **Records**

A record class is nothing more than regular POJO, for which most of the code is generated from the definition.

Let us look into the example of the POJO class before Java 16 introduced records:

```Java
public class Vehicle {
    String code;
    String engineType;

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getEngineType() {
        return engineType;
    }

    public void setEngineType(String engineType) {
        this.engineType = engineType;
    }

    public Vehicle(String code, String engineType) {
        this.code = code;
        this.engineType = engineType;
    }

    @Override
    public boolean equals(Object o) ...

    @Override
    public int hashCode() ...

    @Override
    public String toString() ...
}
```

Definition of a vehicle record, with the same two properties, can be done in just one line:

```Java
public record VehicleRecord(String code, String engineType) {}
```

One thing to note is that the record class is, by default, final, and we need to comply with that. That means we cannot extend a record class, but most other things are available for us. One of the main characteristics of ‘record’ is that it’s immutable. This means once a record is created, its state can’t be changed. If you try to modify a field of a record, you’ll get a compile-time error

### Java 17

- [JEP 306: Restore Always-Strict Floating-Point Semantics](https://openjdk.java.net/jeps/306)
- [JEP 356: Enhanced Pseudo-Random Number Generators](https://openjdk.java.net/jeps/356)
- [JEP 382: New macOS Rendering Pipeline](https://openjdk.java.net/jeps/382)
- [JEP 398: Deprecate the Applet API for Removal](https://openjdk.java.net/jeps/398)
- [JEP 403: Strongly Encapsulate JDK Internals](https://openjdk.java.net/jeps/403)
- [JEP 407: Remove RMI Activation](https://openjdk.java.net/jeps/407)
- [JEP 409: Sealed Classes](https://openjdk.java.net/jeps/409)
- [JEP 410: Remove the Experimental AOT and JIT Compiler](https://openjdk.java.net/jeps/410)
- [JEP 411: Deprecate the Security Manager for Removal](https://openjdk.java.net/jeps/411)
- [JEP 415: Context-Specific Deserialization Filters](https://openjdk.java.net/jeps/415)

### **Pattern Matching for switch (Preview)**

Already available in many other languages:

```Java
public String test(Object obj) {

    return switch(obj) {
    case Integer i -> "An integer";
    case String s -> "A string";
    case Cat c -> "A Cat";
    default -> "I don't know what it is";
    };

}
```

Now you can pass `_Objects_` to switch functions and check for a particular type.

### **Sealed Classes**

Recap: If you ever wanted to have an even closer grip on who is allowed to subclass your classes, there’s now the `_sealed_` feature.

```Java
public sealed class Vehicle permits Bicycle, Car {...}
```

This means that while the class is `_public_`, the only classes allowed to subclass Vehicle are Bicycle and Car.

```Java
public final class Bicycle extends Vehicle {...}
```

- Permitted subclasses must be accessible by the sealed class at compile time
- Permitted subclasses must directly extend the sealed class
- Permitted subclasses must have one of the following modifiers:
    - final
    - sealed
    - non-sealed
- Permitted subclasses must be in the same Java module

### **Foreign Function & Memory API (Incubator)**

A replacement for the Java Native Interface (JNI). Allows you to call native functions and access memory `_outside_` the JVM. Think C calls for now, but with plans for supporting additional languages (like C++, Fortran) over time.

### **Deprecating the Security Manager**

Since Java 1.0, there had been a Security Manager. It’s now deprecated and will be removed in a future version.