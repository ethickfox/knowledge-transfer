# Java Core
# Object Oriented Programming
**OOP** - a programming methodology based on representing a program as a set of objects, each of which is an instance of a certain class, and the classes form an inheritance hierarchy.
What are the limitations of OOP?
- The length of the programms developed using OOP language is much larger than the procedural approach. Since the programme becomes larger in size, it requires more time to be executed that leads to slower execution of the programme.
- We can not apply OOP everywhere as it is not a universal language. It is applied only when it is required. It is not suitable for all types of problems.
- Programmers need to have brilliant designing skill and programming skill along with proper planning because using OOP is little bit tricky.
- OOPs take time to get used to it. The thought process involved in object-oriented programming may not be natural for some people.
- Everything is treated as object in OOP so before applying it we need to have excellent thinking in terms of objects.
- This style of developing software also necessitates a significant amount of preparation and forethought. The OOP code might be hard to comprehend if you don't have the relevant class documentation.
## Encapsulation
Hiding the state and implementation of the class and providing a public API for interacting with the class and its instances. 
- Data Encapsulation is an Object-Oriented Programming concept of hiding the data attributes and their behaviours in a single unit.
- It helps developers to follow modularity while developing software by ensuring that each object is independent of other objects by having its own methods, attributes, and functionalities.
- It is used for the security of the private properties of an object and hence serves the purpose of data hiding.
Example: a class with private fields and access via getters and setters
- public - все
- protected - наследник и пакет
- default - пакет
- private - только сам класс
## Inheritance
a mechanism that allows you to build some classes on the basis of others (children based on parent classes), in which the child class inherits the behavior and state of the parent.
### Multi-Inheritance
Originally, in Java we didn't have multi-inheritance so we didn’t face the diamond problem. With the emergence of default methods, the diamond problem may arise. Candidates may be asked what will happen if we have two unrelated interfaces that have a default method with the same signature and different implementation, and we want to implement these two interfaces in a class. the problem will arise at the compilation level, and will return a compilation error. One solution is to override this method in the class. Another option is to use composition: if you have one class that implements one interface, you can try to represent it as an inner class inside of another class.

Java does not support the multiple inheritance for classes, which means that a class can only inherit from a single superclass.

But you can implement multiple interfaces with a single class, and some of the methods of those interfaces may be defined as _default_ and have an implementation. This allows you to have a safer way of mixing different functionality in a single class.
## Polymorphism 
the ability to identically use objects with the same interface without information about the specific type of this object
- Static(раннее связывание) - behavior depends on the type of transmitted data (**overloading**)
        - Возвращаемый тип не имеет значения
        - Влияет только разное количество или тип параметров
- Dynamic (позднее связывание) - behavior depends on the type of object on which the method is called(**overriding**/переопределение).
        - Те же аргументы
        - Тот же или более узкий тип возвращаемых данных
        - Расширение модификаторов доступ
        - Более узкие ошибки/не указывать их или unchecked
## Abstraction
highlighting common characteristics and functionality of objects or systems, ignoring unnecessary details.
Пример: Банковское приложение/автомобиль
## Aggregation
the inner class can exist separately from the outer class.
- Static Inner class (Lazily initialized)  `static` inner class does not have access to members of the outer class
## Composition
внутренний класс полностью инкапсулирован внешним классом. Внешний мир не может получить ссылку на внутренний класс отдельно от внешнего. Он живет и умирает вместе с внешним
    - Inner Class

### Advantages of composition before inheritance.
Inheritance lags behind composition in the following scenarios:
- Multiple-inheritance is not possible in Java. Classes can only extend from one superclass. In cases where multiple functionalities are required, for example - to read and write information into the file, the pattern of composition is preferred. The writer, as well as reader functionalities, can be made use of by considering them as the private members.
- Composition assists in attaining high flexibility and prevents breaking of encapsulation.
- Unit testing is possible with composition and not inheritance. When a developer wants to test a class composing a different class, then Mock Object can be created for signifying the composed class to facilitate testing. This technique is not possible with the help of inheritance as the derived class cannot be tested without the help of the superclass in inheritance.
- The loosely coupled nature of composition is preferable over the tightly coupled nature of inheritance.
## Delegation
перепоручение задачи от внешнего объекта внутреннему
- Внутренние классы
- Анонимные классы
- Локальные классы
## Object
Класс, от которого неявно наследуются все объекты в Java
### Methods
- Empty Constructor
- getClass
- hashCode
- equals
- clone
- toString
- finalize
- notify
- notifyAll
- wait
- wait with params
### equals & hashCode
**hashCode** - метод для вычисления хэш функции. Дефолтная реализация зависит от jvm, может основываться на адресе объекта в памяти. При переопределении следует учитывать следующие правила:
- для одного и того-же объекта, хеш-код всегда будет одинаковым;
- если объекты одинаковые, то и хеш-коды одинаковые
- если хеш-коды равны, то входные объекты не всегда равны(коллизия)
- если хеш-коды разные, то и объекты гарантировано разные;
**equals** - сравнивает объекты по содержанию
- a == a
- if а == b -> b == a
- if а == b && а == c -> b == c
- постоянность - результат меняется только при изменении входящих в него полей
- != null
```java
@Override
public boolean equals(Object o) {
    if (o == this)
        return true;
    if (!(o instanceof Money))
        return false;
    Money other = (Money)o;
    boolean currencyCodeEquals = (this.currencyCode == null && other.currencyCode == null)
      || (this.currencyCode != null && this.currencyCode.equals(other.currencyCode));
    return this.amount == other.amount && currencyCodeEquals;
}
```
### static
Статические поля или переменные инициализируются после загрузки класса в память. 
Статичный блок НЕ может пробросить перехваченные исключения, но может выбросить не перехваченные. В таком случае возникнет «Exception Initializer Error». 
На практике, любое исключение возникшее во время выполнения и инициализации статических полей, будет завёрнуто Java в эту ошибку. Это также самая частая причина ошибки «No Class Def Found Error», т.к. класс не находился в памяти во время обращения к нему.
## Interface
### Маркерный Interface
Интерфейс без методов, с помощью которого можно помечать другие классы, тем самым предоставляя доп информацию о классе. Например `Serializible`, `Clonable`
### Функциональный Interface
Интерфейс с одним не реализованным методом. За исключением default и методов Object’a. Используется для лямбд.

# Immutable objects
Immutable objects are those objects whose state can not be changed once created. Class whose objects possess this characteristic can be termed as immutable class.

### Steps for creating an immutable class
- Declare all instance variable with private and final
- Remove setter methods
- Initialize all variables in constructor
- Perform cloning of mutable objects while returning from getter method
### Immutable classes thread safe
Immutable classes are thread safe because you can not change state of immutable objects, so even if two thread access immutable object in parallel, it won’t create any issue.
### **Immutable collections**
Collections.unmodifiable* - wraps collection into unmodifiable, returns UnsupportedOperationException for editing operations and returns object itself in case of getting operations. So objects should be immutable too.

# Типизация
- **Статическая‌** ‌- проверка типов на стадии компиляции
- Динамическая ‌- проверка типов на стадии исполнения
- **Сильная‌** ‌- нельзя смешивать разные типы(к числу нельзя добавить строку)
- слабая - как в js, везде язык преобразовывает к разным типам
- **Явная‌** все типы нужно указывать самому
- неявная - типы не нужно указывать

# Exceptions
[![](https://lh5.googleusercontent.com/Jp6mEK6hNZfF9Ap8Zp6yGmJqlFGB55hg-nVdJJV0Mf7KFsI2NEK6QWQIY8a8FIotOQjmc0waMza-bwcbiyQf2DQIIsMGs59ADipZ3fnuib-OB44bsEUf9ARJmNBsBDYsxMGsTQRurnoKC7TsPhQZPODfO-tNVUuF7RSpKK7fxfXfwPWi0byUhZT4zbr9)](https://lh5.googleusercontent.com/Jp6mEK6hNZfF9Ap8Zp6yGmJqlFGB55hg-nVdJJV0Mf7KFsI2NEK6QWQIY8a8FIotOQjmc0waMza-bwcbiyQf2DQIIsMGs59ADipZ3fnuib-OB44bsEUf9ARJmNBsBDYsxMGsTQRurnoKC7TsPhQZPODfO-tNVUuF7RSpKK7fxfXfwPWi0byUhZT4zbr9)
## Типы исключений
- **checked** - Исключения, которые необходимо проверять, без этого не пройдет компилци
- **unchecked** - Исключения, которые можно не проверять
### Обработка
Поймать исключение можно с помощью try-catch-finally. В случе если было выброшено исключение, поток, в котором оно выброшено завершает работу.
### **throws**
Указывает, что метод может выбросить исключение. При наследовании можно добавлять uncheked, не указывать, либо сужать
### try with resources
Новый вид try catch, который сам может закрывать ресурсы(если они наследуются от AutoCloseable), с которыми работал, вне зависимости от того, было ли исключение
### finally
Блок выполняется всегда(кроме System.exit()).




# Questions

### OOP


Composition, and Aggregation help to build (Has - A - Relationship) between classes and objects. But both are not the same in the end. 

- if the container object destroys then the contained objects will also get destroyed automatically. So here we can say that there is a strong association between the objects. So this Strong Association is called **Composition**.
- if the class is destroyed then the inner object will become free to bind with other objects. Because container objects only hold the references of contained objects. So here is the weak association between the objects. And this weak association is called **Aggregation**.
- Describe the Difference Between equals() and ==
    
    The == operator allows you to compare two objects for “sameness” (i.e. that both variables refer to the same object in memory). It is important to remember that the _new_ keyword always creates a new object which will not pass the _==_ equality with any other object, even if they seem to have the same value:
    
    The _equals()_ method is defined in the _java.lang.Object_ class and is, therefore, available for any reference type. By default, it simply checks that the object is the same via the == operator. But it is usually overridden in subclasses to provide the specific semantics of comparison for a class.
    
      
    
- How Do You Compare Two Enum Values: With _equals()_ or With ==?
    
    Actually, you can use both. The _enum_ values are objects, so they can be compared with _equals()_, but they are also implemented as _static_ constants under the hood, so you might as well compare them with _==_. This is mostly a matter of code style, but if you want to save character space (and possibly skip an unneeded method call), you should compare enums with _==_.
    
- equals and hashCode contract
    
    **hashCode** - метод для вычисления хэш функции. Дефолтная реализация зависит от jvm, может основываться на адресе объекта в памяти. При переопределении следует учитывать следующие правила:
    
    - для одного и того-же объекта, хеш-код всегда будет одинаковым;
    - если объекты одинаковые, то и хеш-коды одинаковые
    - если хеш-коды равны, то входные объекты не всегда равны(коллизия)
    - если хеш-коды разные, то и объекты гарантировано разные;
    
    **equals** - сравнивает объекты по содержанию
    
    - объект равен самому себе
    - если а == b, то b==a
    - если а==b и а==c, то b==c
    - постоянность - результат меняется только при изменении входящих в него полей
    - не равен null
- Can you call a constructor of a class inside the another constructor?
    
    Yes, the concept can be termed as constructor chaining and can be achieved using `this()`.
    
    Constructor chaining is a way of simplifying object construction by providing multiple constructors that call each other in sequence.
    
    The most specific constructor may take all possible arguments and may be used for the most detailed object configuration. A less specific constructor may call the more specific constructor by providing some of its arguments with default values. At the top of the chain, a no-argument constructor could instantiate an object with default values.
    
- Why is Java not a pure object oriented language?
    
    Java supports primitive data types - byte, boolean, char, short, int, float, long, and double and hence it is not a pure [**object oriented language**](https://www.interviewbit.com/oops-interview-questions/).
    
- Java works as “pass by value” or “pass by reference” phenomenon?
    
    Java always works as a “pass by value”. There is nothing called a “pass by reference” in Java. However, when the object is passed in any method, the address of the value is passed due to the nature of object handling in Java. When an object is passed, a copy of the reference is created by Java and that is passed to the method. The objects point to the same memory location. 2 cases might happen inside the method:
    
    - **Case 1:** When the object is pointed to another location: In this case, the changes made to that object do not get reflected the original object before it was passed to the method as the reference points to another location.
    - **Case 2:** When object references are not modified: In this case, since we have the copy of reference the main object pointing to the same memory location, any changes in the content of the object get reflected in the original object.
- Explain the concept of object-oriented programming (OOP)
    
    Object-oriented programming is a model that centers on data fields with distinct behaviors and attributes — referred to as objects — instead of logic or functions. Developers focus on the objects that need to be manipulated instead of the processes required to manipulate them.
    
- Describe the Place of the Object Class in the Type Hierarchy. Which Types Inherit from Object, and Which Don’t? Do Arrays Inherit from Object? Can a Lambda Expression Be Assigned to an Object Variable?
    
    The _java.lang.Object_ is at the top of the class hierarchy in Java. All classes inherit from it, either explicitly, implicitly (when the _extends_ keyword is omitted from the class definition) or transitively via the chain of inheritance.
    
    However, there are eight primitive types that do not inherit from _Object_, namely _boolean_, _byte_, _short_, _char_, _int_, _float_, _long_ and _double_.
    
    According to the Java Language Specification, arrays are objects too. They can be assigned to an _Object_ reference, and all _Object_ methods may be called on them.
    
    Lambda expressions can’t be assigned directly to an _Object_ variable because _Object_ is not a functional interface. But you can assign a lambda to a functional interface variable and then assign it to an _Object_ variable(or simply assign it to an Object variable by casting it to a functional interface at the same time).
    
- Explain the Difference Between Primitive and Reference Types.
    
    Reference types inherit from the top _java.lang.Object_ class and are themselves inheritable (except _final_ classes). Primitive types do not inherit and cannot be subclassed.
    
    Primitively typed argument values are always passed via the stack, which means they are passed by value, not by reference. This has the following implication: changes made to a primitive argument value inside the method do not propagate to the actual argument value.
    
- What Is a Default Method?
    
    Prior to Java 8, interfaces could only have abstract methods, i.e. methods without a body. Starting with Java 8, interface methods can have a default implementation. If an implementing class does not override this method, then the default implementation is used. Such methods are suitably marked with a _default_ keyword.
    
    One of the prominent use cases of a _default_ method is adding a method to an existing interface. If you don’t mark such interface method as _default_, then all existing implementations of this interface will break. Adding a method with a _default_ implementation ensures binary compatibility of legacy code with the new version of this interface.
    
    A good example of this is the _Iterator_ interface which allows a class to be a target of the for-each loop. This interface first appeared in Java 5, but in Java 8 it received two additional methods, _forEach_, and _spliterator_. They are defined as default methods with implementations and thus do not break backward compatibility:
    
- What Are Static Class Members?
    
    _Static_ fields and methods of a class are not bound to a specific instance of a class. Instead, they are bound to the class object itself. The call of a _static_ method or addressing a _static_ field is resolved at compile time because, contrary to instance methods and fields, we don’t need to walk the reference and determine an actual object we’re referring to.
    
- What is the ‘IS-A ‘ relationship in OOPs java?
    
    ‘IS-A’ relationship is another name for inheritance. When we inherit the base class from the derived class, then it forms a relationship between the classes. So that relationship is termed an ‘IS-A’ Relationship.
    
    **Example** - Consider a Television (Typical CRT TV). Now another Smart TV  that is inherited from television class. So we can say that the Smart iv is also a TV. Because CRT TV things can also be done in the Smart TV.
    
- When can you use super keyword?
    - The super keyword is used to access hidden fields and overridden methods or attributes of the parent class.
    - Following are the cases when this keyword can be used:
        - Accessing data members of parent class when the member names of the class and its child subclasses are same.
        - To call the default and parameterized constructor of the parent class inside the child class.
        - Accessing the parent class methods when the child classes have overridden them.
- What are shallow copy and deep copy in java?
    
    To copy the object's data, we have several methods like deep copy and shallow copy.
    
    Object for this Rectangle class - **Rectangle obj1 = new Rectangle();**
    
    - **Shallow copy** - The shallow copy only creates a new reference and points to the same object.
    
    Now by doing this what will happen is the new reference is created with the name obj2 and that will point to the same memory location.
    
    - **Deep Copy** - In a deep copy, we create a new object and copy the old object value to the new object.
- Apart from the security aspect, what are the reasons behind making strings immutable in Java?
    
    A String is made immutable due to the following reasons:
    
    - **String Pool:** Designers of Java were aware of the fact that String data type is going to be majorly used by the programmers and developers. Thus, they wanted optimization from the beginning. They came up with the notion of using the String pool (a storage area in Java heap) to store the String literals. They intended to decrease the temporary String object with the help of sharing. An immutable class is needed to facilitate sharing. The sharing of the mutable structures between two unknown parties is not possible. Thus, immutable Java String helps in executing the concept of String Pool.
    - [**Multithreading**](https://www.interviewbit.com/multithreading-interview-questions/)**:** The safety of threads regarding the String objects is an important aspect in Java. No external synchronization is required if the String objects are immutable. Thus, a cleaner code can be written for sharing the String objects across different threads. The complex process of concurrency is facilitated by this method.
    - [**Collections**](https://www.interviewbit.com/java-collections-interview-questions/)**:** In the case of Hashtables and HashMaps, keys are String objects. If the String objects are not immutable, then it can get modified during the period when it resides in the HashMaps. Consequently, the retrieval of the desired data is not possible. Such changing states pose a lot of risks. Therefore, it is quite safe to make the string immutable.
- May a Class Be Declared Abstract If It Does Not Have Any Abstract Members? What Could Be the Purpose of Such Class?
    
    Yes, a class can be declared _abstract_ even if it does not contain any _abstract_ members. As an abstract class, it cannot be instantiated, but it can serve as a root object of some hierarchy, providing methods that can be useful to its implementations.
    
- What Is an Anonymous Class? Describe Its Use Case.
    
    Anonymous class is a one-shot class that is defined in the same place where its instance is needed. This class is defined and instantiated in the same place, thus it does not need a name.
    
    Before Java 8, you would often use an anonymous class to define the implementation of a single method interface, like _Runnable_. In Java 8, lambdas are used instead of single abstract method interfaces. But anonymous classes still have use cases, for example, when you need an instance of an interface with multiple methods or an instance of a class with some added features.
    
- Can default methods override an Object method?
    
    Default methods of the interface can’t override some methods of the Object class
    
    If we create a default method with the same signature as, for example, equals in the Object class, then we'll change the class's behavior, which is inherited from another class. This violates the basic principle that distinguishes interfaces and classes.
    
- What Is the Difference Between an Abstract Class and an Interface? What Are the Use Cases of One and the Other?
    
    - **Availability of methods:** Only abstract methods are available in interfaces, whereas non-abstract methods can be present along with abstract methods in abstract classes.
    - **Variable types**: Static and final variables can only be declared in the case of interfaces, whereas abstract classes can also have non-static and non-final variables.
    - **Inheritance:** Multiple inheritances are facilitated by interfaces, whereas abstract classes do not promote multiple inheritances.
    - **Data member accessibility:** By default, the class data members of interfaces are of the public- type. Conversely, the class members for an abstract class can be protected or private also.
    - **Implementation:** With the help of an abstract class, the implementation of an interface is easily possible. However, the converse is not true;
    
    An abstract class is a _class_ with the _abstract_ modifier in its definition. It can’t be instantiated, but it can be subclassed. The interface is a type described with _interface_ keyword. It also cannot be instantiated, but it can be implemented.
    
    The main difference between an abstract class and an interface is that a class can implement multiple interfaces, but extend only one abstract class.
    
    An _abstract_ class is usually used as a base type in some class hierarchy, and it signifies the main intention of all classes that inherit from it.
    
    An _abstract_ class could also implement some basic methods needed in all subclasses. For instance, most map collections in JDK inherit from the _AbstractMap_ class which implements many methods used by subclasses (such as the _equals_ method).
    
    An interface specifies some contract that the class agrees to. An implemented interface may signify not only the main intention of the class but also some additional contracts.
    
- **Why is the character array preferred over string for storing confidential information?**
    
    In Java, a string is basically immutable i.e. it cannot be modified. After its declaration, it continues to stay in the string pool as long as it is not removed in the form of garbage. In other words, a string resides in the heap section of the memory for an unregulated and unspecified time interval after string value processing is executed.
    
    As a result, vital information can be stolen for pursuing harmful activities by hackers if a memory dump is illegally accessed by them. Such risks can be eliminated by using mutable objects or structures like character arrays for storing any variable. After the work of the character array variable is done, the variable can be configured to blank at the same instant. Consequently, it helps in saving heap memory and also gives no chance to the hackers to extract vital data.
    
- What Are the Restrictions on the Members (Fields and Methods) of an Interface Type?
    
    An interface can declare fields, but they are implicitly declared as _public_, _static_ and _final_, even if you don’t specify those modifiers. Consequently, you can’t explicitly define an interface field as _private_. In essence, an interface may only have constant fields, not instance fields.
    
    All methods of an interface are also implicitly _public_. They also can be either (implicitly) _abstract_, or _default_.
    
    Since java 9 there can be private methods, they can be both static and non static
    
      
    
- What is Interface?
    
    An `interface` is a completely "**abstract class**" that is used to group related methods with empty bodies:
    
- What Is the Difference Between an Inner Class and a Static Nested Class?
    
    Nested class is basically a class defined inside another class.
    
    Nested classes fall into two categories with very different properties. An inner class is a class that can’t be instantiated without instantiating the enclosing class first, i.e. any instance of an inner class is implicitly bound to some instance of the enclosing class.
    
- What Are the Wrapper Classes? What Is Autoboxing?
    
    For each of the eight primitive types in Java, there is a wrapper class that can be used to wrap a primitive value and use it like an object. Those classes are, correspondingly, _Boolean_, _Byte_, _Short_, _Character_, _Integer_, _Float_, _Long_, and _Double_. These wrappers can be useful, for instance, when you need to put a primitive value into a generic collection, which only accepts reference objects.
    
    To save the trouble of manually converting primitives back and forth, an automatic conversion known as autoboxing/auto unboxing is provided by the Java compiler.
    
    ```Java
    List<Integer> list = new ArrayList<>();
    list.add(5);
    int value = list.get(0);
    ```
    
- Suppose You Have a Variable That References an Instance of a Class Type. How Do You Check That an Object Is an Instance of This Class?
    
    You cannot use _instanceof_ keyword in this case because it only works if you provide the actual class name as a literal.
    
    Thankfully, the _Class_ class has a method _isInstance_ that allows checking if an object is an instance of this class:
    
    ```Java
    Class<?> integerClass = new Integer(5).getClass();
    assertTrue(integerClass.isInstance(new Integer(4)));
    ```
    
- What is the value object? Why should you use them?
    
    > [!info] Entity vs Value Object: полный список отличий  
    > Тема отличий таких понятий как Entity (Сущность) и Value Object (Объект-Значение) из Domain-Driven Design не нова.  
    > [https://habr.com/ru/articles/275599/](https://habr.com/ru/articles/275599/)  
    
- Describe the Meaning of the Final Keyword When Applied to a Class, Method, Field or a Local Variable.
    - A _final_ class is a class that cannot be subclassed
    - A _final_ method is a method that cannot be overridden in subclasses
    - A _final_ field is a field that has to be initialized in the constructor or initializer block and cannot be modified after that
    - A _final_ variable is a variable that may be assigned (and has to be assigned) only once and is never modified after that
- What Is Overriding and Overloading of Methods? How Are They Different?
    - Overriding of a method is done in a subclass when you define a method with the same signature as in superclass. This allows the runtime to pick a method depending on the actual object type that you call the method on. Methods _toString_, _equals_, and _hashCode_ are overridden quite often in subclasses.
    - Overloading of a method happens in the same class. Overloading occurs when you create a method with the same name but with different types or number of arguments. This allows you to execute a certain code depending on the types of arguments you provide, while the name of the method remains the same.
- Can You Override a Static Method?
    
    No, you can’t. By definition, you can only override a method if its implementation is determined at runtime by the type of the actual instance (a process known as the dynamic method lookup). The _static_ method implementation is determined at compile time using the type of the reference, so overriding would not make much sense anyway. Although you can add to subclass a _static_ method with the exact same signature as in superclass, this is not technically overriding.
    
- What Is an Immutable Class, and How Can You Create One?
    
    An instance of an immutable class cannot be changed after it’s created. By changing we mean mutating the state by modifying the values of the fields of the instance. Immutable classes have many advantages: they are thread-safe, and it is much easier to reason about them when you have no mutable state to consider.
    
    To make a class immutable, you should ensure the following:
    
    - All fields should be declared _private_ and _final_; this infers that they should be initialized in the constructor and not changed ever since;
    - The class should have no setters or other methods that mutate the values of the fields;
    - All fields of the class that were passed via constructor should either be also immutable, or their values should be copied before field initialization (or else we could change the state of this class by holding on to these values and modifying them);
    - The methods of the class should not be overridable; either all methods should be _final_, or the constructor should be _private_ and only invoked via _static_ factory method.
    - If adding getters, the should return copy of the object
- What Is an Initializer Block? What Is a Static Initializer Block?
    
    An initializer block is a curly-braced block of code in the class scope which is executed during the instance creation. You can use it to initialize fields with something more complex than in-place initialization one-liners.
    
    Actually, the compiler just copies this block inside every constructor, so it is a nice way to extract common code from all constructors.
    
    A static initializer block is a curly-braced block of code with the _static_ modifier in front of it. It is executed once during the class loading and can be used for initializing static fields or for some side effects.
    
- What Is a Marker Interface? What Are the Notable Examples of Marker Interfaces in Java?
    
    A marker interface is an interface without any methods. It is usually implemented by a class or extended by another interface to signify a certain property. The most widely known marker interfaces in standard Java library are the following:
    
    - _Serializable_ is used to explicitly express that this class can be serialized;
    - _Cloneable_ allows cloning objects using the _clone_ method (without _Cloneable_ interface in place, this method throws a _CloneNotSupportedException_)
- What Is a Var-Arg? What Are the Restrictions on a Var-Arg? How Can You Use It Inside the Method Body?
    
    Var-arg is a variable-length argument for a method. A method may have only one var-arg, and it has to come last in the list of arguments. It is specified as a type name followed by an ellipsis and an argument name. Inside the method body, a var-arg is used as an array of specified type.
    
- Can You Access an Overridden Method of a Superclass? Can You Access an Overridden Method of a Super-Superclass in a Similar Way?
    
    To access an overridden method of a superclass, you can use the _super_ keyword. But you don’t have a similar way of accessing the overridden method of a super-superclass.
    
- **How to not allow serialization of attributes of a class in Java?**
    
    • In order to achieve this, the attribute can be declared along with the usage of `transient` keyword as shown below:
    
- **What do you understand by Object Cloning and how do you achieve it in Java?**
    
    • It is the process of creating an exact copy of any object. In order to support this, a java class has to implement the Cloneable interface of java.lang package and override the clone() method provided by the Object class the syntax of which is:
    
    • In case the Cloneable interface is not implemented and just the method is overridden, it results in CloneNotSupportedException in Java.
    
- **Is it possible to import the same class or package twice in Java and what happens to it during runtime?**
    
    It is possible to import a class or package more than once, however, it is redundant because the JVM internally loads the package or class only once.
    
- **In case a package has sub packages, will it suffice to import only the main package? e.g. Does importing of com.myMainPackage.* also import com.myMainPackage.mySubPackage.*?**
    
    This is a big NO. We need to understand that the importing of the sub-packages of a package needs to be done explicitly. Importing the parent package only results in the import of the classes within it and not the contents of its child/sub-packages.
    
- **Why is it said that the length() method of String class doesn't return accurate results?**
    
    - The length method returns the number of Unicode units of the String. Let's understand what Unicode units are and what is the confusion below.
    - We know that Java uses UTF-16 for String representation. With this Unicode, we need to understand the below two Unicode related terms:
        - Code Point: This represents an integer denoting a character in the code space.
        - Code Unit: This is a bit sequence used for encoding the code points. In order to do this, one or more units might be required for representing a code point.
    - Under the UTF-16 scheme, the code points were divided logically into 17 planes and the first plane was called the Basic Multilingual Plane (BMP). The BMP has classic characters - U+0000 to U+FFFF. The rest of the characters- U+10000 to U+10FFFF were termed as the supplementary characters as they were contained in the remaining planes.
        - The code points from the first plane are encoded using **one** 16-bit code unit
        - The code points from the remaining planes are encoded using **two** code units.
    
    Now if a string contained supplementary characters, the length function would count that as 2 units and the result of the length() function would not be as per what is expected.
    
    In other words, if there is 1 supplementary character of 2 units, the length of that SINGLE character is considered to be TWO - Notice the inaccuracy here? As per the java documentation, it is expected, but as per the real logic, it is inaccurate.
    
- **What is a Memory Leak?**
    
    The Java Garbage Collector (GC) typically removes unused objects when they are no longer required, but when they are still referenced, the unused objects cannot be removed. So this causes the memory leak problem. **Example** - Consider a linked list like the structure below -
    
    ![Untitled 56.png](Untitled%2056.png)
    
    In the above image, there are unused objects that are not referenced. But then also Garbage collection will not free it. Because it is referencing some existing referenced object. So this can be the situation of memory leak.
    
    **Some common causes of Memory leaks are** -
    
    - When there are Unbounded caches.
    - Excessive page swapping is done by the operating system.
    - Improper written custom data structures.
    - Inserting into a collection object without first deleting it.etc.
- **What Is the Difference Between an Unlabeled and a Labeled** _**break**_ **Statement?**
    
    An unlabeled _break_ statement terminates the innermost _switch_, _for_, _while_ or _do-while_ statement, whereas a labeled _break_ ends the execution of an outer statement.
    
    ```Java
    outer: for (int[] rows : table) {
        for (int row : rows) {
            loopCycles++;
            if (row == 37) {
                found = true;
                break outer;
            }
        }
    }
    ```
    
- **Why would it be more secure to store sensitive data (such as a password, social security number, etc.) in a character array rather than in a String?**
    
    In Java, Strings are [immutable](http://docs.oracle.com/javase/tutorial/essential/concurrency/immutable.html) and are stored in the String pool. What this means is that, once a String is created, it stays in the pool in memory until being garbage collected. Therefore, even after you’re done processing the string value (e.g., the password), it remains available in memory for an indeterminate period of time thereafter (again, until being garbage collected) which you have no real control over. Therefore, anyone having access to a memory dump can potentially extract the sensitive data and exploit it.
    
    In contrast, if you use a mutable object like a character array, for example, to store the value, you can set it to blank once you are done with it with confidence that it will no longer be retained in memory.
    
- **When designing an abstract class, why should you avoid calling abstract methods inside its constructor?**
    
    This is a problem of initialization order. The subclass constructor will not have had a chance to run yet and there is no way to force it to run it before the parent class.
    

> [!info] 11.7 Implementing the 'equals()', 'hashCode()', and 'compareTo()' Methods :: Chapter 11. Collections and Maps :: A programmer's guide to java certification :: Certification :: eTutorials.org  
> The majority of the non-final methods of the Object class are meant to be overridden.  
> [http://etutorials.org/cert/java+certification/Chapter+11.+Collections+and+Maps/11.7+Implementing+the+equals+hashCode+and+compareTo+Methods/](http://etutorials.org/cert/java+certification/Chapter+11.+Collections+and+Maps/11.7+Implementing+the+equals+hashCode+and+compareTo+Methods/)  

### Exceptions

- What is the difference between the ‘throw’ and ‘throws’ keyword in java?
    - The ‘**throw**’ keyword is used to manually throw the exception to the calling method.
    - And the ‘**throws**’ keyword is used in the function definition to inform the calling method that this method throws the exception. So if you are calling, then you have to handle the exception.
    - What is the difference between an exception and a validation error? When an exception should be thrown and when you should catch an exception?
- Exceptions hierarchy
    
    [![](https://lh5.googleusercontent.com/Jp6mEK6hNZfF9Ap8Zp6yGmJqlFGB55hg-nVdJJV0Mf7KFsI2NEK6QWQIY8a8FIotOQjmc0waMza-bwcbiyQf2DQIIsMGs59ADipZ3fnuib-OB44bsEUf9ARJmNBsBDYsxMGsTQRurnoKC7TsPhQZPODfO-tNVUuF7RSpKK7fxfXfwPWi0byUhZT4zbr9)](https://lh5.googleusercontent.com/Jp6mEK6hNZfF9Ap8Zp6yGmJqlFGB55hg-nVdJJV0Mf7KFsI2NEK6QWQIY8a8FIotOQjmc0waMza-bwcbiyQf2DQIIsMGs59ADipZ3fnuib-OB44bsEUf9ARJmNBsBDYsxMGsTQRurnoKC7TsPhQZPODfO-tNVUuF7RSpKK7fxfXfwPWi0byUhZT4zbr9)
    
- What Is an Exception?
    
    An exception is an abnormal event that occurs during the execution of a program and disrupts the normal flow of the program’s instructions.
    
- How Can You Catch Multiple Exceptions?
    
    There are three ways of handling multiple exceptions in a block of code.
    
    The first is to use a _catch_ block that can handle all exception types being thrown:
    
    ```Java
    try {
        // ...
    } catch (Exception ex) {
        // ...
    }
    ```
    
    The second way is implementing multiple catch blocks:
    
    ```Java
    try {
        // ...
    } catch (FileNotFoundException ex) {
        // ...
    } catch (EOFException ex) {
        // ...
    }
    ```
    
    Note that, if the exceptions have an inheritance relationship; the child type must come first and the parent type later. If we fail to do this, it will result in a compilation error.
    
    The third is to use a multi-catch block:
    
    ```Java
    try {
        // ...
    } catch (FileNotFoundException | EOFException ex) {
        // ...
    }
    ```
    
- Exceptions types
    
    ### **checked**
    
    Исключения, которые необходимо проверять, без этого не пройдет компилци
    
    ### **unchecked**
    
    Исключения, которые можно не проверять
    
- How does an exception propagate in the code?
    
    When an exception occurs, first it searches to locate the matching catch block. In case, the matching catch block is located, then that block would be executed. Else, the exception propagates through the method call stack and goes into the caller method where the process of matching the catch block is performed. This propagation happens until the matching catch block is found. If the match is not found, then the program gets terminated in the main method.
    
- Is it mandatory for a catch block to be followed after a try block?
    
    No, it is not necessary for a catch block to be present after a try block. - A try block should be followed either by a catch block or by a finally block. If the exceptions likelihood is more, then they should be declared using the throws clause of the method.
    
- How do exceptions affect the program if it doesn't handle them?
    
      
    
- try with resources
    
    Новый вид try catch, который сам может закрывать ресурсы(если они наследуются от AutoCloseable), с которыми работал, вне зависимости от того, было ли исключение
    
- What Is the Difference Between a Checked and an Unchecked Exception?
    
    A checked exception must be handled within a _try-catch_ block or declared in a _throws_ clause; whereas an unchecked exception is not required to be handled nor declared.
    
    Checked and unchecked exceptions are also known as compile-time and runtime exceptions respectively.
    
    All exceptions are checked exceptions, except those indicated by _Error_, _RuntimeException_, and their subclasses.
    
- What Is the Difference Between an Exception and Error?
    
    An exception is an event that represents a condition from which is possible to recover, whereas error represents an external situation usually impossible to recover from.
    
    All errors thrown by the JVM are instances of _Error_ or one of its subclasses, the more common ones include but are not limited to:
    
    - _OutOfMemoryError_ – thrown when the JVM cannot allocate more objects because it is out memory, and the garbage collector was unable to make more available
    - _StackOverflowError_ – occurs when the stack space for a thread has run out, typically because an application recurses too deeply
    - _ExceptionInInitializerError_ – signals that an unexpected exception occurred during the evaluation of a static initializer
    - _NoClassDefFoundError_ – is thrown when the classloader tries to load the definition of a class and couldn’t find it, usually because the required _class_ files were not found in the classpath
    - _UnsupportedClassVersionError_ – occurs when the JVM attempts to read a _class_ file and determines that the version in the file is not supported, normally because the file was generated with a newer version of Java
    
    Although an error can be handled with a _try_ statement, this is not a recommended practice since there is no guarantee that the program will be able to do anything reliably after the error was thrown.
    
- What Is Exception Chaining?
    
    Occurs when an exception is thrown in response to another exception. This allows us to discover the complete history of our raised problem:
    
    ```Java
    try {
        task.readConfigFile();
    } catch (FileNotFoundException ex) {
        throw new TaskException("Could not perform task", ex);
    }
    ```
    
- Can You Throw Any Exception Inside a Lambda Expression’s Body?
    
    When using a standard functional interface already provided by Java, you can only throw unchecked exceptions because standard functional interfaces do not have a “throws” clause in method signatures
    
    However, if you are using a custom functional interface, throwing checked exceptions is possible:
    
- What Are the Rules We Need to Follow When Overriding a Method That Throws an Exception?
    - When the parent class method doesn’t throw any exceptions, the child class method can’t throw any checked exception, but it may throw any unchecked.
    - When the parent class method throws one or more checked exceptions, the child class method can throw any unchecked exception; all, none or a subset of the declared checked exceptions, and even a greater number of these as long as they have the same scope or narrower.
    - When the parent class method has a throws clause with an unchecked exception, the child class method can throw none or any number of unchecked exceptions, even though they are not related.
- Is There Any Way of Throwing a Checked Exception from a Method That Does Not Have a Throws Clause?
    
    Yes. We can take advantage of the type erasure performed by the compiler and make it think we are throwing an unchecked exception, when, in fact; we’re throwing a checked exception:
    
    ```Java
    public <T extends Throwable> T sneakyThrow(Throwable ex) throws T {
        throw (T) ex;
    }
    
    public void methodWithoutThrows() {
        this.<RuntimeException>sneakyThrow(new Exception("Checked Exception"));
    }
    ```
    
- In Which Situations _try-finally_ Block Might Be Used Even When Exceptions Might Not Be Thrown?
    
    This block is useful when we want to ensure we don’t accidentally bypass the clean up of resources used in the code by encountering a _break_, _continue_ or _return_ statement:
    
    Also, we may face situations in which we can’t locally handle the exception being thrown, or we want the current method to throw the exception still while allowing us to free up resources:
    
- How Does _try-with-resources_ Work?
    
    he _try-with-resources_ statement declares and initializes one or more resources before executing the _try_ block and closes them automatically at the end of the statement regardless of whether the block completed normally or abruptly. Any object implementing _AutoCloseable_ or _Closeable_ interfaces can be used as a resource:
    
      
    
- **How can you catch an exception thrown by another thread in Java?**
    
    This can be done using [Thread.UncaughtExceptionHandler](http://docs.oracle.com/javase/7/docs/api/java/lang/Thread.UncaughtExceptionHandler.html).
    
    Here’s a simple example:
    
    ```Plain
    //create our uncaughtexceptionhandler
    Thread.UncaughtExceptionHandlerhandler = new Thread.UncaughtExceptionHandler() {
        public void uncaughtException(Thread th, Throwable ex) {
    System.out.println("Uncaught exception: " + ex);
        }
    };
    
    //create another thread
    Thread otherThread = new Thread() {
        public void run() {
    System.out.println("Sleeping ...");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
    System.out.println("Interrupted.");
            }
    System.out.println("Throwing exception ...");
            throw new RuntimeException();
        }
    };
    
    //set our uncaughtexceptionhandleras the oneto be usedwhen the new thread
    // throws an uncaughtexception
    otherThread.setUncaughtExceptionHandler(handler);
    
    //start the other thread - our uncaughtexceptionhandler will be invokedwhen
    // the other thread throws an uncaughtexception
    otherThread.start();
    ```
    

### Annotations

- **What Are Annotations? What Are Their Typical Use Cases?**
    
    Annotations have been around since Java 5, they are metadata bound to elements of the source code of a program and have no effect on the operation of the code they operate.
    
    Their typical uses cases are:
    
    - **Information for the compiler** – with annotations, the compiler can detect errors or suppress warnings
    - **Compile-time and deployment-time processing** – software tools can process annotations and generate code, configuration files, etc.
    - **Runtime processing** – annotations can be examined at runtime to customize the behavior of a program
- **Describe Some Useful Annotations from the Standard Library**
    
    There are several annotations in the _java.lang_ and _java.lang.annotation_ packages, the more common ones include but not limited to:
    
    - _@Override –_ marks that a method is meant to override an element declared in a superclass. If it fails to override the method correctly, the compiler will issue an error
    - _@Deprecated_ – indicates that element is deprecated and should not be used. The compiler will issue a warning if the program uses a method, class, or field marked with this annotation
    - _@SuppressWarnings_ – tells the compiler to suppress specific warnings. Most commonly used when interfacing with legacy code written before generics appeared
    - _@FunctionalInterface_ – introduced in Java 8, indicates that the type declaration is a functional interface and whose implementation can be provided using a Lambda Expression
- How to create Annotation
    
    Annotations are a form of an interface where the keyword _interface_ is preceded by _@,_ and whose body contains _annotation type element_ declarations that look very similar to methods:
    
    ```Java
    public @interface SimpleAnnotation {
        String value();
    
        int[] types();
    }
    ```
    
    After the annotation is defined, yon can start using it in through your code:
    
    ```Java
    @SimpleAnnotation(value = "an element", types = 1)
    public class Element {
        @SimpleAnnotation(value = "an attribute", types = { 1, 2 })
        public Element nextElement;
    }
    ```
    
    Optionally, default values can be provided as long as they are constant expressions to the compiler:
    
    ```Java
    public @interface SimpleAnnotation {
        String value() default "This is an element";
    
        int[] types() default { 1, 2, 3 };
    }
    ```
    
      
    
      
    
- **What Object Types Can Be Returned from an Annotation Method Declaration?**
    
    The return type must be a primitive, _String_, _Class_, _Enum_, or an array of one of the previous types. Otherwise, the compiler will throw an error.
    
    Here’s an example code that successfully follows this principle:
    
    ```Java
    enum Complexity {
        LOW, HIGH
    }
    
    public @interface ComplexAnnotation {
        Class<? extends Object> value();
    
        int[] types();
    
        Complexity complexity();
    }
    ```
    
    The next example will fail to compile since _Object_ is not a valid return type:
    
    ```Java
    public @interface FailingAnnotation {
        Object complexity();
    }
    ```
    
      
    
- **Which Program Elements Can Be Annotated?**
    
    Annotations can be applied in several places throughout the source code. They can be applied to
    
    - declarations of classes, constructors, and fields:
    - Methods and their parameters:
    - Local variables, including a loop and resource variables:
    - Other annotation types:
    - And even packages, through the _package-info.java_ file
    - As of Java 8, they can also be applied to the _use_ of types. For this to work, the annotation must specify an _@Target_ annotation with a value of _ElementType.USE_
    - Now, the annotation can be applied to class instance creation:
        
        ```Java
        new @SimpleAnnotation Apply();
        
        //Type casts:
        aString = (@SimpleAnnotation String) something;
        
        //Implements clause
        public class SimpleList<T>
          implements @SimpleAnnotation List<@SimpleAnnotation T> {
            // ...
        }
        
        //And throws clause:
        void aMethod() throws @SimpleAnnotation Exception {
            // ...
        }
        ```
        
- **Is There a Way to Limit the Elements in Which an Annotation Can Be Applied?**
    
    Yes, the _@Target_ annotation can be used for this purpose. If we try to use an annotation in a context where it is not applicable, the compiler will issue an error.
    
    We can pass multiple constants if we want to make it applicable in more contexts
    
    We can even make an annotation so it cannot be used to annotate anything. This may come in handy when the declared types are intended solely for use as a member type in complex annotations:
    
- **What Are Meta-Annotations?**
    
    Are annotations that apply to other annotations.
    
    All annotations that aren’t marked with _@Target,_ or are marked with it but include _ANNOTATION_TYPE_ constant are also meta-annotations:
    
- **What Are Repeating Annotations?**
    
    These are annotations that can be applied more than once to the same element declaration.
    
    For compatibility reasons, since this feature was introduced in Java 8, repeating annotations are stored in a _container annotation_ that is automatically generated by the Java compiler. For the compiler to do this, there are two steps to declared them.
    
    First, we need to declare a repeatable annotation:
    
    ```Java
    @Repeatable(Schedules.class)
    public @interface Schedule {
        String time() default "morning";
    }
    ```
    
    Then, we define the containing annotation with a mandatory _value_ element, and whose type must be an array of the repeatable annotation type:
    
    ```Java
    public @interface Schedules {
        Schedule[] value();
    }
    ```
    
    Now, we can use @Schedule multiple times:
    
    ```Java
    @Schedule
    @Schedule(time = "afternoon")
    @Schedule(time = "night")
    void scheduledMethod() {
        // ...
    }
    ```
    
- **How Can You Retrieve Annotations? How Does This Relate to Its Retention Policy?**
    
    You can use the Reflection API or an annotation processor to retrieve annotations.
    
    The _@Retention_ annotation and its _RetentionPolicy_ parameter affect how you can retrieve them. There are three constants in _RetentionPolicy_ enum:
    
    - _RetentionPolicy.SOURCE_ – makes the annotation to be discarded by the compiler but annotation processors can read them
    - _RetentionPolicy.CLASS_ – indicates that the annotation is added to the class file but not accessible through reflection
    - _RetentionPolicy.RUNTIME_ –Annotations are recorded in the class file by the compiler and retained by the JVM at runtime so that they can be read reflectively
- **Is It Possible to Extend Annotations?**
    
    No. Annotations always extend _java.lang.annotation.Annotation,_ as stated in the [Java Language Specification](http://docs.oracle.com/javase/specs/jls/se7/html/jls-9.html#jls-9.6).