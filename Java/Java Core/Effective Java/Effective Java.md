---
Interview graded: false
Topic:
  - "[[Java Core\\|Java Core]]"
---
# [[Creating and Destroying Objects]]
- Consider static factory methods instead of constructors
- Consider a builder when faced with many constructor parameters 
- Enforce the singleton property with a private constructor or an enum type
- Enforce noninstantiability with a private constructor
- Prefer dependency injection to hardwiring resources
- Avoid creating unnecessary objects
- Eliminate obsolete object references
- Avoid finalizers and cleaners
- Prefer try-with-resources to try-finally
# Methods Common to All Objects
- Obey the general contract when overriding equals
- Always override hashCode when you override equals
- Always override toString
- Override clone judiciously
- Consider implementing Comparable
# Classes and Interfaces
- Minimize the accessibility of classes and members
- In public classes, use accessor methods, not public fields
- Minimize mutability
- Favor composition over inheritance
- Design and document for inheritance or else prohibit it
- Prefer interfaces to abstract classes
- Design interfaces for posterity
- Use interfaces only to define types
- Prefer class hierarchies to tagged classes
- Favor static member classes over nonstatic
- Limit source files to a single top-level class
# Generics
# Enums and Annotations
# Lambdas and Streams 
# Methods 
# General Programming
# Exceptions 
# Concurrency
# Serialization