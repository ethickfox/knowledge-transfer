---
Interview graded: true
Last edited time: 2024-02-27T13:14
Last recall: 2023-07-11
Needs Rework: false
Status: Not started
Topic:
  - "[[Program Structure Architecture]]"
---
# Creational patterns

## Factory Method

**Also known as:** Virtual Constructor

Provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

![[/Untitled 63.png|Untitled 63.png]]

### Implementation

```Java
public interface Button {
    void render();
    void onClick();
}
```

```Java
public class HtmlButton implements Button {

    public void render() {
        System.out.println("<button>Test Button</button>");
        onClick();
    }

    public void onClick() {
        System.out.println("Click! Button says - 'Hello World!'");
    }
}
```

```Java
public class WindowsButton implements Button {
    JPanel panel = new JPanel();
    JFrame frame = new JFrame();
    JButton button;

    public void render() {
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        JLabel label = new JLabel("Hello World!");
				...
        onClick();
    }

    public void onClick() {
        button = new JButton("Exit");
        button.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                frame.setVisible(false);
                System.exit(0);
            }
        });
    }
}
```

```Java
/**
 * Base factory class. Note that "factory" is merely a role for the class. It
 * should have some core business logic which needs different products to be
 * created.
 */
public abstract class Dialog {

    public void renderWindow() {
        // ... other code ...

        Button okButton = createButton();
        okButton.render();
    }

    /**
     * Subclasses will override this method in order to create specific button
     * objects.
     */
    public abstract Button createButton();
}
```

```Java
/**
 * HTML Dialog will produce HTML buttons.
 */
public class HtmlDialog extends Dialog {

    @Override
    public Button createButton() {
        return new HtmlButton();
    }
}
```

```Java
public class WindowsDialog extends Dialog {

    @Override
    public Button createButton() {
        return new WindowsButton();
    }
}
```

### **Applicability**

- Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.
- Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.
- Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.

### Pros

- You avoid tight coupling between the creator and the concrete products.
- _Single Responsibility Principle_. You can move the product creation code into one place in the program, making the code easier to support.
- _Open/Closed Principle_. You can introduce new types of products into the program without breaking existing client code.

### Cons

- The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.

### **Relations with Other Patterns**

- Many designs start by using [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customizable via subclasses) and evolve toward [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory), [**Prototype**](https://refactoring.guru/design-patterns/prototype), or [**Builder**](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
- [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) classes are often based on a set of [**Factory Methods**](https://refactoring.guru/design-patterns/factory-method), but you can also use [**Prototype**](https://refactoring.guru/design-patterns/prototype) to compose the methods on these classes.
- You can use [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) along with [**Iterator**](https://refactoring.guru/design-patterns/iterator) to let collection subclasses return different types of iterators that are compatible with the collections.
- [**Prototype**](https://refactoring.guru/design-patterns/prototype) isn’t based on inheritance, so it doesn’t have its drawbacks. On the other hand, _Prototype_ requires a complicated initialization of the cloned object. [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) is based on inheritance but doesn’t require an initialization step.
- [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) is a specialization of [**Template Method**](https://refactoring.guru/design-patterns/template-method). At the same time, a _Factory Method_may serve as a step in a large _Template Method_.

### **Usage examples**

The Factory Method pattern is widely used in Java code. It’s very useful when you need to provide a high level of flexibility for your code.

The pattern is present in core Java libraries:

- [`**java.util.Calendar#getInstance()**`](http://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html#getInstance--)
- [`**java.util.ResourceBundle#getBundle()**`](http://docs.oracle.com/javase/8/docs/api/java/util/ResourceBundle.html#getBundle-java.lang.String-)
- [`**java.text.NumberFormat#getInstance()**`](http://docs.oracle.com/javase/8/docs/api/java/text/NumberFormat.html#getInstance--)
- [`**java.nio.charset.Charset#forName()**`](http://docs.oracle.com/javase/8/docs/api/java/nio/charset/Charset.html#forName-java.lang.String-)
- [`**java.net.URLStreamHandlerFactory#createURLStreamHandler(String)**`](http://docs.oracle.com/javase/8/docs/api/java/net/URLStreamHandlerFactory.html) (Returns different singleton objects, depending on a protocol)
- [`**java.util.EnumSet#of()**`](https://docs.oracle.com/javase/8/docs/api/java/util/EnumSet.html#of(E))
- [`**javax.xml.bind.JAXBContext#createMarshaller()**`](https://docs.oracle.com/javase/8/docs/api/javax/xml/bind/JAXBContext.html#createMarshaller--) and other similar methods.

## Abstract Factory

[https://stackoverflow.com/questions/5739611/what-are-the-differences-between-abstract-factory-and-factory-design-patterns](https://stackoverflow.com/questions/5739611/what-are-the-differences-between-abstract-factory-and-factory-design-patterns)

Lets you produce families of related objects without specifying their concrete classes.

The first thing the Abstract Factory pattern suggests is to explicitly declare interfaces for each distinct product of the product family (e.g., chair, sofa or coffee table). Then you can make all variants of products follow those interfaces. For example, all chair variants can implement the `Chair` interface; all coffee table variants can implement the `CoffeeTable` interface, and so on.

The next move is to declare the _Abstract Factory_—an interface with a list of creation methods for all products that are part of the product family (for example, `createChair`, `createSofa` and `createCoffeeTable`). These methods must return **abstract** product types represented by the interfaces we extracted previously: `Chair`, `Sofa`, `CoffeeTable` and so on.

Now, how about the product variants? For each variant of a product family, we create a separate factory class based on the `AbstractFactory` interface. A factory is a class that returns products of a particular kind. For example, the `ModernFurnitureFactory` can only create `ModernChair`, `ModernSofa` and `ModernCoffeeTable` objects.

The client code has to work with both factories and products via their respective abstract interfaces. This lets you change the type of a factory that you pass to the client code, as well as the product variant that the client code receives, without breaking the actual client code.

![[/Untitled 1 20.png|Untitled 1 20.png]]

### Implementation

```Java
public interface Button {
    void paint();
}

public class MacOSButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created MacOSButton.");
    }
}

public class WindowsButton implements Button {

    @Override
    public void paint() {
        System.out.println("You have created WindowsButton.");
    }
}
```

```Java
public interface Checkbox {
    void paint();
}

public class MacOSCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created MacOSCheckbox.");
    }
}

public class WindowsCheckbox implements Checkbox {

    @Override
    public void paint() {
        System.out.println("You have created WindowsCheckbox.");
    }
}
```

```Java
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

public class MacOSFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new MacOSButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new MacOSCheckbox();
    }
}

public class WindowsFactory implements GUIFactory {

    @Override
    public Button createButton() {
        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}
```

### Applicability

- Use the Abstract Factory when your code needs to work with various families of related products, but you don’t want it to depend on the concrete classes of those products—they might be unknown beforehand or you simply want to allow for future extensibility.
- Consider implementing the Abstract Factory when you have a class with a set of [Factory Methods](https://refactoring.guru/design-patterns/factory-method) that blur its primary responsibility.

### Pros

- You can be sure that the products you’re getting from a factory are compatible with each other.
- You avoid tight coupling between concrete products and client code.
- _Single Responsibility Principle_. You can extract the product creation code into one place, making the code easier to support.
- _Open/Closed Principle_. You can introduce new variants of products without breaking existing client code.

### Cons

- The code may become more complicated than it should be, since a lot of new interfaces and classes are introduced along with the pattern.

### **Relations with Other Patterns**

- Many designs start by using [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customisable via subclasses) and evolve toward [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory), [**Prototype**](https://refactoring.guru/design-patterns/prototype), or [**Builder**](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
- [**Builder**](https://refactoring.guru/design-patterns/builder) focuses on constructing complex objects step by step. [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) specialises in creating families of related objects. _Abstract Factory_ returns the product immediately, whereas _Builder_ lets you run some additional construction steps before fetching the product.
- [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) classes are often based on a set of [**Factory Methods**](https://refactoring.guru/design-patterns/factory-method), but you can also use [**Prototype**](https://refactoring.guru/design-patterns/prototype) to compose the methods on these classes.
- [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) can serve as an alternative to [**Facade**](https://refactoring.guru/design-patterns/facade) when you only want to hide the way the subsystem objects are created from the client code.
- You can use [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) along with [**Bridge**](https://refactoring.guru/design-patterns/bridge). This pairing is useful when some abstractions defined by _Bridge_ can only work with specific implementations. In this case, _Abstract Factory_ can encapsulate these relations and hide the complexity from the client code.
- [**Abstract Factories**](https://refactoring.guru/design-patterns/abstract-factory), [**Builders**](https://refactoring.guru/design-patterns/builder) and [**Prototypes**](https://refactoring.guru/design-patterns/prototype) can all be implemented as [**Singletons**](https://refactoring.guru/design-patterns/singleton).

### Usage Examples

- [`**javax.xml.parsers.DocumentBuilderFactory#newInstance()**`](http://docs.oracle.com/javase/8/docs/api/javax/xml/parsers/DocumentBuilderFactory.html#newInstance--)
- [`**javax.xml.transform.TransformerFactory#newInstance()**`](http://docs.oracle.com/javase/8/docs/api/javax/xml/transform/TransformerFactory.html#newInstance--)
- [`**javax.xml.xpath.XPathFactory#newInstance()**`](http://docs.oracle.com/javase/8/docs/api/javax/xml/xpath/XPathFactory.html#newInstance--)

**Identification:** The pattern is easy to recognize by methods, which return a factory object. Then, the factory is used for creating specific sub-components.

## Builder

Lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

### **Director**

You can go further and extract a series of calls to the builder steps you use to construct a product into a separate class called _director_. The director class defines the order in which to execute the building steps, while the builder provides the implementation for those steps.

![[/Untitled 2 15.png|Untitled 2 15.png]]

### Implementation

```Java
public interface Builder {
    void setCarType(CarType type);
    void setSeats(int seats);
    void setEngine(Engine engine);
    void setTransmission(Transmission transmission);
    void setTripComputer(TripComputer tripComputer);
    void setGPSNavigator(GPSNavigator gpsNavigator);
}

public class CarBuilder implements Builder {
    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;
	// setters and getters
}

public class CarManualBuilder implements Builder{
    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;
	// setters and getters
}
```

```Java
public class Director {

    public void constructSportsCar(Builder builder) {
        builder.setCarType(CarType.SPORTS_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(3.0, 0));
        builder.setTransmission(Transmission.SEMI_AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructCityCar(Builder builder) {
        builder.setCarType(CarType.CITY_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(1.2, 0));
        builder.setTransmission(Transmission.AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructSUV(Builder builder) {
        builder.setCarType(CarType.SUV);
        builder.setSeats(4);
        builder.setEngine(new Engine(2.5, 0));
        builder.setTransmission(Transmission.MANUAL);
        builder.setGPSNavigator(new GPSNavigator());
    }
	public static void main(String[] args) {
	        Director director = new Director();

	        CarBuilder builder = new CarBuilder();
	        director.constructSportsCar(builder);

	        Car car = builder.getResult();
	        System.out.println("Car built:\n" + car.getCarType());
	
	
	        CarManualBuilder manualBuilder = new CarManualBuilder();
	
	        // Director may know several building recipes.
	        director.constructSportsCar(manualBuilder);
	        Manual carManual = manualBuilder.getResult();
	        System.out.println("\nCar manual built:\n" + carManual.print());
	    }
}
```

### **Applicability**

- Use the Builder pattern to get rid of a “telescoping constructor”.
- Use the Builder pattern when you want your code to be able to create different representations of some product (for example, stone and wooden houses).
- Use the Builder to construct [Composite](https://refactoring.guru/design-patterns/composite) trees or other complex objects.

### Pros

- You can construct objects step-by-step, defer construction steps or run steps recursively.
- You can reuse the same construction code when building various representations of products.
- _Single Responsibility Principle_. You can isolate complex construction code from the business logic of the product.

### Cons

- The overall complexity of the code increases since the pattern requires creating multiple new classes.

### **Relations with Other Patterns**

- Many designs start by using [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customisable via subclasses) and evolve toward [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory), [**Prototype**](https://refactoring.guru/design-patterns/prototype), or [**Builder**](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
- [**Builder**](https://refactoring.guru/design-patterns/builder) focuses on constructing complex objects step by step. [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) specialises in creating families of related objects. _Abstract Factory_ returns the product immediately, whereas _Builder_ lets you run some additional construction steps before fetching the product.
- You can use [**Builder**](https://refactoring.guru/design-patterns/builder) when creating complex [**Composite**](https://refactoring.guru/design-patterns/composite) trees because you can program its construction steps to work recursively.
- You can combine [**Builder**](https://refactoring.guru/design-patterns/builder) with [**Bridge**](https://refactoring.guru/design-patterns/bridge): the director class plays the role of the abstraction, while different builders act as implementations.
- [**Abstract Factories**](https://refactoring.guru/design-patterns/abstract-factory), [**Builders**](https://refactoring.guru/design-patterns/builder) and [**Prototypes**](https://refactoring.guru/design-patterns/prototype) can all be implemented as [**Singletons**](https://refactoring.guru/design-patterns/singleton).

### Usage examples

- [`**java.lang.StringBuilder#append()**`](http://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html#append-boolean-) (`unsynchronized`)
- [`**java.lang.StringBuffer#append()**`](http://docs.oracle.com/javase/8/docs/api/java/lang/StringBuffer.html#append-boolean-) (`synchronized`)
- [`**java.nio.ByteBuffer#put()**`](http://docs.oracle.com/javase/8/docs/api/java/nio/ByteBuffer.html#put-byte-) (also in [`**CharBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/CharBuffer.html#put-char-), [`**ShortBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/ShortBuffer.html#put-short-), [`**IntBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/IntBuffer.html#put-int-), [`**LongBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/LongBuffer.html#put-long-), [`**FloatBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/FloatBuffer.html#put-float-) and [`**DoubleBuffer**`](http://docs.oracle.com/javase/8/docs/api/java/nio/DoubleBuffer.html#put-double-))
- [`**javax.swing.GroupLayout.Group#addComponent()**`](http://docs.oracle.com/javase/8/docs/api/javax/swing/GroupLayout.Group.html#addComponent-java.awt.Component-)
- All implementations [`**java.lang.Appendable**`](http://docs.oracle.com/javase/8/docs/api/java/lang/Appendable.html)

**Identification:** The Builder pattern can be recognized in a class, which has a single creation method and several methods to configure the resulting object. Builder methods often support chaining (for example, `someBuilder.setValueA(1).setValueB(2).create()`).

## Prototype

Lets you copy existing objects without making your code dependent on their classes.

The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface for all objects that support cloning. This interface lets you clone an object without coupling your code to the class of that object. Usually, such an interface contains just a single `clone` method.

![[/Untitled 3 15.png|Untitled 3 15.png]]

The **Prototype Registry** provides an easy way to access frequently-used prototypes. It stores a set of pre-built objects that are ready to be copied. The simplest prototype registry is a `name → prototype` hash map

![[/Untitled 4 11.png|Untitled 4 11.png]]

### Implementation

```Java
public abstract class Shape {
    public int x;
    public int y;
    public String color;

    public Shape() {
    }

    public Shape(Shape target) {
        if (target != null) {
            this.x = target.x;
            this.y = target.y;
            this.color = target.color;
        }
    }

    public abstract Shape clone();

    @Override
    public boolean equals(Object object2) {
        if (!(object2 instanceof Shape)) return false;
        Shape shape2 = (Shape) object2;
        return shape2.x == x && shape2.y == y && Objects.equals(shape2.color, color);
    }
}

public class Circle extends Shape {
    public int radius;

    public Circle() {
    }

    public Circle(Circle target) {
        super(target);
        if (target != null) {
            this.radius = target.radius;
        }
    }

    @Override
    public Shape clone() {
        return new Circle(this);
    }

    @Override
    public boolean equals(Object object2) {
        if (!(object2 instanceof Circle) || !super.equals(object2)) return false;
        Circle shape2 = (Circle) object2;
        return shape2.radius == radius;
    }
}
```

  

**Prototype registry**

```Java
public class BundledShapeCache {
    private Map<String, Shape> cache = new HashMap<>();

    public BundledShapeCache() {
        Circle circle = new Circle();
        circle.x = 5;
        circle.y = 7;
        circle.radius = 45;
        circle.color = "Green";

        Rectangle rectangle = new Rectangle();
        rectangle.x = 6;
        rectangle.y = 9;
        rectangle.width = 8;
        rectangle.height = 10;
        rectangle.color = "Blue";

        cache.put("Big green circle", circle);
        cache.put("Medium blue rectangle", rectangle);
    }

    public Shape put(String key, Shape shape) {
        cache.put(key, shape);
        return shape;
    }

    public Shape get(String key) {
        return cache.get(key).clone();
    }
}
```

### **Applicability**

- Use the Prototype pattern when your code shouldn’t depend on the concrete classes of objects that you need to copy.
- Use the Prototype pattern when your code shouldn’t depend on the concrete classes of objects that you need to copy.

### Pros

- You can clone objects without coupling to their concrete classes.
- You can get rid of repeated initialization code in favor of cloning pre-built prototypes.
- You can produce complex objects more conveniently.
- You get an alternative to inheritance when dealing with configuration presets for complex objects.

### Cons

- Cloning complex objects that have circular references might be very tricky.

### **Relations with Other Patterns**

- Many designs start by using [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) (less complicated and more customizable via subclasses) and evolve toward [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory), [**Prototype**](https://refactoring.guru/design-patterns/prototype), or [**Builder**](https://refactoring.guru/design-patterns/builder) (more flexible, but more complicated).
- [**Abstract Factory**](https://refactoring.guru/design-patterns/abstract-factory) classes are often based on a set of [**Factory Methods**](https://refactoring.guru/design-patterns/factory-method), but you can also use [**Prototype**](https://refactoring.guru/design-patterns/prototype) to compose the methods on these classes.
- [**Prototype**](https://refactoring.guru/design-patterns/prototype) can help when you need to save copies of [**Commands**](https://refactoring.guru/design-patterns/command) into history.
- Designs that make heavy use of [**Composite**](https://refactoring.guru/design-patterns/composite) and [**Decorator**](https://refactoring.guru/design-patterns/decorator) can often benefit from using [**Prototype**](https://refactoring.guru/design-patterns/prototype). Applying the pattern lets you clone complex structures instead of re-constructing them from scratch.
- [**Prototype**](https://refactoring.guru/design-patterns/prototype) isn’t based on inheritance, so it doesn’t have its drawbacks. On the other hand, _Prototype_ requires a complicated initialisation of the cloned object. [**Factory Method**](https://refactoring.guru/design-patterns/factory-method) is based on inheritance but doesn’t require an initialisation step.
- Sometimes [**Prototype**](https://refactoring.guru/design-patterns/prototype) can be a simpler alternative to [**Memento**](https://refactoring.guru/design-patterns/memento). This works if the object, the state of which you want to store in the history, is fairly straightforward and doesn’t have links to external resources, or the links are easy to re-establish.
- [**Abstract Factories**](https://refactoring.guru/design-patterns/abstract-factory), [**Builders**](https://refactoring.guru/design-patterns/builder) and [**Prototypes**](https://refactoring.guru/design-patterns/prototype) can all be implemented as [**Singletons**](https://refactoring.guru/design-patterns/singleton).

### **Usage examples**

- [`**java.lang.Object#clone()**`](http://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#clone--) (class should implement the [`**java.lang.Cloneable**`](http://docs.oracle.com/javase/8/docs/api/java/lang/Cloneable.html) interface)

**Identification:** The prototype can be easily recognized by a `clone` or `copy` methods, etc.

## Singleton

Lets you ensure that a class has only one instance, while providing a global access point to this instance.

All implementations of the Singleton have these two steps in common:

- Make the default constructor private, to prevent other objects from using the `new` operator with the Singleton class.
- Create a static creation method that acts as a constructor. Under the hood, this method calls the private constructor to create an object and saves it in a static field. All following calls to this method return the cached object.

### Implementation

Eager

```Java

public class EagerInitializedSingleton {

    private static final EagerInitializedSingleton instance = new EagerInitializedSingleton();

    private EagerInitializedSingleton(){}

    public static EagerInitializedSingleton getInstance() {
        return instance;
    }
}
```

SingleThreaded Lazy

```Java
public final class Singleton {
    private static Singleton instance;
    public String value;

    private Singleton(String value) {
        this.value = value;
    }

    public static Singleton getInstance(String value) {
        if (instance == null) {
            instance = new Singleton(value);
        }
        return instance;
    }
}
```

MultiThreaded

```Java
public final class Singleton 
    private static volatile Singleton instance;

    public String value;

    private Singleton(String value) {
        this.value = value;
    }

    public static Singleton getInstance(String value) {
        Singleton result = instance;
        if (result != null) {
            return result;
        }
        synchronized(Singleton.class) {
            if (instance == null) {
                instance = new Singleton(value);
            }
            return instance;
        }
    }
}
```

Inner static class

```Java
public class BillPughSingleton {

    private BillPughSingleton(){}

    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

Enum

```Java
public enum EnumSingleton {

    INSTANCE;

    public static void doSomething() {
        // do something
    }
}
```

### Applicability

- Use the Singleton pattern when a class in your program should have just a single instance available to all clients; for example, a single database object shared by different parts of the program.
- Use the Singleton pattern when you need stricter control over global variables.

### Pros

- You can be sure that a class has only a single instance.
- You gain a global access point to that instance.
- The singleton object is initialised only when it’s requested for the first time.

### Cons

- Violates the _Single Responsibility Principle_. The pattern solves two problems at the time.(it is responsible for maintaining its sole instance and it is also responsible for the main role of the class)
- The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.
- The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.
- It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.

### **Relations with Other Patterns**

- A [**Facade**](https://refactoring.guru/design-patterns/facade) class can often be transformed into a [**Singleton**](https://refactoring.guru/design-patterns/singleton) since a single facade object is sufficient in most cases.
- [**Flyweight**](https://refactoring.guru/design-patterns/flyweight) would resemble [**Singleton**](https://refactoring.guru/design-patterns/singleton) if you somehow managed to reduce all shared states of the objects to just one flyweight object. But there are two fundamental differences between these patterns:
    1. There should be only one Singleton instance, whereas a _Flyweight_ class can have multiple instances with different intrinsic states.
    2. The _Singleton_ object can be mutable. Flyweight objects are immutable.
- [**Abstract Factories**](https://refactoring.guru/design-patterns/abstract-factory), [**Builders**](https://refactoring.guru/design-patterns/builder) and [**Prototypes**](https://refactoring.guru/design-patterns/prototype) can all be implemented as [**Singletons**](https://refactoring.guru/design-patterns/singleton).

### **Usage examples**

- [`**java.lang.Runtime#getRuntime()**`](http://docs.oracle.com/javase/8/docs/api/java/lang/Runtime.html#getRuntime--)
- [`**java.awt.Desktop#getDesktop()**`](http://docs.oracle.com/javase/8/docs/api/java/awt/Desktop.html#getDesktop--)
- [`**java.lang.System#getSecurityManager()**`](http://docs.oracle.com/javase/8/docs/api/java/lang/System.html#getSecurityManager--)

**Identification:** Singleton can be recognised by a static creation method, which returns the same cached object.

  

# Structural patterns

## Adapter

Allows objects with incompatible interfaces to collaborate.

You can create an _adapter_. This is a special object that converts the interface of one object so that another object can understand it.

An adapter wraps one of the objects to hide the complexity of conversion happening behind the scenes. The wrapped object isn’t even aware of the adapter. For example, you can wrap an object that operates in meters and kilometers with an adapter that converts all of the data to imperial units such as feet and miles.

![[/Untitled 5 11.png|Untitled 5 11.png]]

### Implementation

```Java
public class RoundHole {
    private double radius;

    public RoundHole(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public boolean fits(RoundPeg peg) {
        boolean result;
        result = (this.getRadius() >= peg.getRadius());
        return result;
    }
}

public class RoundPeg {
    private double radius;

    public RoundPeg() {}

    public RoundPeg(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }
}

public class SquarePeg {
    private double width;

    public SquarePeg(double width) {
        this.width = width;
    }

    public double getWidth() {
        return width;
    }

    public double getSquare() {
        double result;
        result = Math.pow(this.width, 2);
        return result;
    }
}

public class SquarePegAdapter extends RoundPeg {
    private SquarePeg peg;

    public SquarePegAdapter(SquarePeg peg) {
        this.peg = peg;
    }

    @Override
    public double getRadius() {
        double result;
        // Calculate a minimum circle radius, which can fit this peg.
        result = (Math.sqrt(Math.pow((peg.getWidth() / 2), 2) * 2));
        return result;
    }
}
```

## Bridge

Lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other

![[/Untitled 6 11.png|Untitled 6 11.png]]

### Implementation

```Java
public interface Device {
    boolean isEnabled();

    void enable();

    void disable();

    int getVolume();

    void setVolume(int percent);

    int getChannel();

    void setChannel(int channel);

    void printStatus();
}

public class Radio implements Device {
    private boolean on = false;
    private int volume = 30;
    private int channel = 1;

    @Override
    public boolean isEnabled() {
        return on;
    }

    @Override
    public void enable() {
        on = true;
    }

    @Override
    public void disable() {
        on = false;
    }

    @Override
    public int getVolume() {
        return volume;
    }

    @Override
    public void setVolume(int volume) {
        if (volume > 100) {
            this.volume = 100;
        } else if (volume < 0) {
            this.volume = 0;
        } else {
            this.volume = volume;
        }
    }

    @Override
    public int getChannel() {
        return channel;
    }

    @Override
    public void setChannel(int channel) {
        this.channel = channel;
    }

    @Override
    public void printStatus() {
        System.out.println("------------------------------------");
        System.out.println("| I'm radio.");
        System.out.println("| I'm " + (on ? "enabled" : "disabled"));
        System.out.println("| Current volume is " + volume + "%");
        System.out.println("| Current channel is " + channel);
        System.out.println("------------------------------------\n");
    }
}

public class Tv implements Device {
    private boolean on = false;
    private int volume = 30;
    private int channel = 1;

    @Override
    public boolean isEnabled() {
        return on;
    }

    @Override
    public void enable() {
        on = true;
    }

    @Override
    public void disable() {
        on = false;
    }

    @Override
    public int getVolume() {
        return volume;
    }

    @Override
    public void setVolume(int volume) {
        if (volume > 100) {
            this.volume = 100;
        } else if (volume < 0) {
            this.volume = 0;
        } else {
            this.volume = volume;
        }
    }

    @Override
    public int getChannel() {
        return channel;
    }

    @Override
    public void setChannel(int channel) {
        this.channel = channel;
    }

    @Override
    public void printStatus() {
        System.out.println("------------------------------------");
        System.out.println("| I'm TV set.");
        System.out.println("| I'm " + (on ? "enabled" : "disabled"));
        System.out.println("| Current volume is " + volume + "%");
        System.out.println("| Current channel is " + channel);
        System.out.println("------------------------------------\n");
    }
}
```

```Java
public interface Remote {
    void power();

    void volumeDown();

    void volumeUp();

    void channelDown();

    void channelUp();
}

public class BasicRemote implements Remote {
    protected Device device;

    public BasicRemote() {}

    public BasicRemote(Device device) {
        this.device = device;
    }

    @Override
    public void power() {
        System.out.println("Remote: power toggle");
        if (device.isEnabled()) {
            device.disable();
        } else {
            device.enable();
        }
    }

    @Override
    public void volumeDown() {
        System.out.println("Remote: volume down");
        device.setVolume(device.getVolume() - 10);
    }

    @Override
    public void volumeUp() {
        System.out.println("Remote: volume up");
        device.setVolume(device.getVolume() + 10);
    }

    @Override
    public void channelDown() {
        System.out.println("Remote: channel down");
        device.setChannel(device.getChannel() - 1);
    }

    @Override
    public void channelUp() {
        System.out.println("Remote: channel up");
        device.setChannel(device.getChannel() + 1);
    }
}

public class AdvancedRemote extends BasicRemote {

    public AdvancedRemote(Device device) {
        super.device = device;
    }

    public void mute() {
        System.out.println("Remote: mute");
        device.setVolume(0);
    }
}
```

## Composite

Lets you compose objects into tree structures and then work with these structures as if they were individual objects.

![[/Untitled 7 9.png|Untitled 7 9.png]]

### Implementation

```Java
public interface Shape {
    int getX();
    int getY();
    int getWidth();
    int getHeight();
    void move(int x, int y);
    boolean isInsideBounds(int x, int y);
    void select();
    void unSelect();
    boolean isSelected();
    void paint(Graphics graphics);
}

abstract class BaseShape implements Shape {
    public int x;
    public int y;
    public Color color;
    private boolean selected = false;

    BaseShape(int x, int y, Color color) {
        this.x = x;
        this.y = y;
        this.color = color;
    }

    @Override
    public int getX() {
        return x;
    }

    @Override
    public int getY() {
        return y;
    }

    @Override
    public int getWidth() {
        return 0;
    }

    @Override
    public int getHeight() {
        return 0;
    }

    @Override
    public void move(int x, int y) {
        this.x += x;
        this.y += y;
    }

    @Override
    public boolean isInsideBounds(int x, int y) {
        return x > getX() && x < (getX() + getWidth()) &&
                y > getY() && y < (getY() + getHeight());
    }

    @Override
    public void select() {
        selected = true;
    }

    @Override
    public void unSelect() {
        selected = false;
    }

    @Override
    public boolean isSelected() {
        return selected;
    }

    void enableSelectionStyle(Graphics graphics) {
        graphics.setColor(Color.LIGHT_GRAY);

        Graphics2D g2 = (Graphics2D) graphics;
        float[] dash1 = {2.0f};
        g2.setStroke(new BasicStroke(1.0f,
                BasicStroke.CAP_BUTT,
                BasicStroke.JOIN_MITER,
                2.0f, dash1, 0.0f));
    }

    void disableSelectionStyle(Graphics graphics) {
        graphics.setColor(color);
        Graphics2D g2 = (Graphics2D) graphics;
        g2.setStroke(new BasicStroke());
    }


    @Override
    public void paint(Graphics graphics) {
        if (isSelected()) {
            enableSelectionStyle(graphics);
        }
        else {
            disableSelectionStyle(graphics);
        }

        // ...
    }
}

public class Dot extends BaseShape {
    private final int DOT_SIZE = 3;

    public Dot(int x, int y, Color color) {
        super(x, y, color);
    }

    @Override
    public int getWidth() {
        return DOT_SIZE;
    }

    @Override
    public int getHeight() {
        return DOT_SIZE;
    }

    @Override
    public void paint(Graphics graphics) {
        super.paint(graphics);
        graphics.fillRect(x - 1, y - 1, getWidth(), getHeight());
    }
}

public class Circle extends BaseShape {
    public int radius;

    public Circle(int x, int y, int radius, Color color) {
        super(x, y, color);
        this.radius = radius;
    }

    @Override
    public int getWidth() {
        return radius * 2;
    }

    @Override
    public int getHeight() {
        return radius * 2;
    }

    public void paint(Graphics graphics) {
        super.paint(graphics);
        graphics.drawOval(x, y, getWidth() - 1, getHeight() - 1);
    }
}

public class Rectangle extends BaseShape {
    public int width;
    public int height;

    public Rectangle(int x, int y, int width, int height, Color color) {
        super(x, y, color);
        this.width = width;
        this.height = height;
    }

    @Override
    public int getWidth() {
        return width;
    }

    @Override
    public int getHeight() {
        return height;
    }

    @Override
    public void paint(Graphics graphics) {
        super.paint(graphics);
        graphics.drawRect(x, y, getWidth() - 1, getHeight() - 1);
    }
}

public class CompoundShape extends BaseShape {
    protected List<Shape> children = new ArrayList<>();

    public CompoundShape(Shape... components) {
        super(0, 0, Color.BLACK);
        add(components);
    }

    public void add(Shape component) {
        children.add(component);
    }

    public void add(Shape... components) {
        children.addAll(Arrays.asList(components));
    }

    public void remove(Shape child) {
        children.remove(child);
    }

    public void remove(Shape... components) {
        children.removeAll(Arrays.asList(components));
    }

    public void clear() {
        children.clear();
    }

    @Override
    public int getX() {
        if (children.size() == 0) {
            return 0;
        }
        int x = children.get(0).getX();
        for (Shape child : children) {
            if (child.getX() < x) {
                x = child.getX();
            }
        }
        return x;
    }

    @Override
    public int getY() {
        if (children.size() == 0) {
            return 0;
        }
        int y = children.get(0).getY();
        for (Shape child : children) {
            if (child.getY() < y) {
                y = child.getY();
            }
        }
        return y;
    }

    @Override
    public int getWidth() {
        int maxWidth = 0;
        int x = getX();
        for (Shape child : children) {
            int childsRelativeX = child.getX() - x;
            int childWidth = childsRelativeX + child.getWidth();
            if (childWidth > maxWidth) {
                maxWidth = childWidth;
            }
        }
        return maxWidth;
    }

    @Override
    public int getHeight() {
        int maxHeight = 0;
        int y = getY();
        for (Shape child : children) {
            int childsRelativeY = child.getY() - y;
            int childHeight = childsRelativeY + child.getHeight();
            if (childHeight > maxHeight) {
                maxHeight = childHeight;
            }
        }
        return maxHeight;
    }

    @Override
    public void move(int x, int y) {
        for (Shape child : children) {
            child.move(x, y);
        }
    }

    @Override
    public boolean isInsideBounds(int x, int y) {
        for (Shape child : children) {
            if (child.isInsideBounds(x, y)) {
                return true;
            }
        }
        return false;
    }

    @Override
    public void unSelect() {
        super.unSelect();
        for (Shape child : children) {
            child.unSelect();
        }
    }

    public boolean selectChildAt(int x, int y) {
        for (Shape child : children) {
            if (child.isInsideBounds(x, y)) {
                child.select();
                return true;
            }
        }
        return false;
    }

    @Override
    public void paint(Graphics graphics) {
        if (isSelected()) {
            enableSelectionStyle(graphics);
            graphics.drawRect(getX() - 1, getY() - 1, getWidth() + 1, getHeight() + 1);
            disableSelectionStyle(graphics);
        }

        for (Shape child : children) {
            child.paint(graphics);
        }
    }
}

public class ImageEditor {
    private EditorCanvas canvas;
    private CompoundShape allShapes = new CompoundShape();

    public ImageEditor() {
        canvas = new EditorCanvas();
    }

    public void loadShapes(Shape... shapes) {
        allShapes.clear();
        allShapes.add(shapes);
        canvas.refresh();
    }

    private class EditorCanvas extends Canvas {
        JFrame frame;

        private static final int PADDING = 10;

        EditorCanvas() {
            createFrame();
            refresh();
            addMouseListener(new MouseAdapter() {
                @Override
                public void mousePressed(MouseEvent e) {
                    allShapes.unSelect();
                    allShapes.selectChildAt(e.getX(), e.getY());
                    e.getComponent().repaint();
                }
            });
        }

        void createFrame() {
            frame = new JFrame();
            frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
            frame.setLocationRelativeTo(null);

            JPanel contentPanel = new JPanel();
            Border padding = BorderFactory.createEmptyBorder(PADDING, PADDING, PADDING, PADDING);
            contentPanel.setBorder(padding);
            frame.setContentPane(contentPanel);

            frame.add(this);
            frame.setVisible(true);
            frame.getContentPane().setBackground(Color.LIGHT_GRAY);
        }

        public int getWidth() {
            return allShapes.getX() + allShapes.getWidth() + PADDING;
        }

        public int getHeight() {
            return allShapes.getY() + allShapes.getHeight() + PADDING;
        }

        void refresh() {
            this.setSize(getWidth(), getHeight());
            frame.pack();
        }

        public void paint(Graphics graphics) {
            allShapes.paint(graphics);
        }
    }
}
```

## Decorator

Lets you attach new behaviours to objects by placing these objects inside special wrapper objects that contain the behaviours.

“Wrapper” is the alternative nickname for the Decorator pattern that clearly expresses the main idea of the pattern. A _wrapper_ is an object that can be linked with some _target_ object. The wrapper contains the same set of methods as the target and delegates to it all requests it receives. However, the wrapper may alter the result by doing something either before or after it passes the request to the target.

![[/Untitled 8 9.png|Untitled 8 9.png]]

### Implementation

```Java
public interface DataSource {
    void writeData(String data);

    String readData();
}

public class FileDataSource implements DataSource {
    private String name;

    public FileDataSource(String name) {
        this.name = name;
    }

    @Override
    public void writeData(String data) {
        //...
    }

    @Override
    public String readData() {
        //...
    }
}
```

```Java
public class DataSourceDecorator implements DataSource {
    private DataSource wrappee;

    DataSourceDecorator(DataSource source) {
        this.wrappee = source;
    }

    @Override
    public void writeData(String data) {
        wrappee.writeData(data);
    }

    @Override
    public String readData() {
        return wrappee.readData();
    }
}

public class EncryptionDecorator extends DataSourceDecorator {

    public EncryptionDecorator(DataSource source) {
        super(source);
    }

    @Override
    public void writeData(String data) {
        super.writeData(encode(data));
    }

    @Override
    public String readData() {
        return decode(super.readData());
    }

    private String encode(String data) {
        //...
    }

    private String decode(String data) {
       //...
    }
}

public class CompressionDecorator extends DataSourceDecorator {
    private int compLevel = 6;

    public CompressionDecorator(DataSource source) {
        super(source);
    }

    public int getCompressionLevel() {
        return compLevel;
    }

    public void setCompressionLevel(int value) {
        compLevel = value;
    }

    @Override
    public void writeData(String data) {
        super.writeData(compress(data));
    }

    @Override
    public String readData() {
        return decompress(super.readData());
    }

    private String compress(String stringData) {
        //...
    }

    private String decompress(String stringData) {
        //...
    }
}
```

  

## Facade

Provides a simplified interface to a library, a framework, or any other complex set of classes.

A facade is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.

![[/Untitled 9 9.png|Untitled 9 9.png]]

### Implementaton

```Java
public class VideoFile {
    private String name;
    private String codecType;

    public VideoFile(String name) {
        this.name = name;
        this.codecType = name.substring(name.indexOf(".") + 1);
    }

    public String getCodecType() {
        return codecType;
    }

    public String getName() {
        return name;
    }
}

public interface Codec {
}

public class MPEG4CompressionCodec implements Codec {
    public String type = "mp4";
}

public class OggCompressionCodec implements Codec {
    public String type = "ogg";
}

public class CodecFactory {
    public static Codec extract(VideoFile file) {
        String type = file.getCodecType();
        if (type.equals("mp4")) {
            System.out.println("CodecFactory: extracting mpeg audio...");
            return new MPEG4CompressionCodec();
        } else {
            System.out.println("CodecFactory: extracting ogg audio...");
            return new OggCompressionCodec();
        }
    }
}

public class VideoConversionFacade {
    public File convertVideo(String fileName, String format) {
        System.out.println("VideoConversionFacade: conversion started.");
        VideoFile file = new VideoFile(fileName);
        Codec sourceCodec = CodecFactory.extract(file);
        Codec destinationCodec;
        if (format.equals("mp4")) {
            destinationCodec = new MPEG4CompressionCodec();
        } else {
            destinationCodec = new OggCompressionCodec();
        }
        VideoFile buffer = BitrateReader.read(file, sourceCodec);
        VideoFile intermediateResult = BitrateReader.convert(buffer, destinationCodec);
        File result = (new AudioMixer()).fix(intermediateResult);
        System.out.println("VideoConversionFacade: conversion completed.");
        return result;
    }
}
```

## FlyWeight

Lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

![[/Untitled 10 9.png|Untitled 10 9.png]]

![[/Untitled 11 9.png|Untitled 11 9.png]]

```Java
	public class Tree {
    private int x;
    private int y;
    private TreeType type;

    public Tree(int x, int y, TreeType type) {
        this.x = x;
        this.y = y;
        this.type = type;
    }

    public void draw(Graphics g) {
        type.draw(g, x, y);
    }
}

public class TreeType {
    private String name;
    private Color color;
    private String otherTreeData;

    public TreeType(String name, Color color, String otherTreeData) {
        this.name = name;
        this.color = color;
        this.otherTreeData = otherTreeData;
    }

    public void draw(Graphics g, int x, int y) {
        g.setColor(Color.BLACK);
        g.fillRect(x - 1, y, 3, 5);
        g.setColor(color);
        g.fillOval(x - 5, y - 10, 10, 10);
    }
}

public class TreeFactory {
    static Map<String, TreeType> treeTypes = new HashMap<>();

    public static TreeType getTreeType(String name, Color color, String otherTreeData) {
        TreeType result = treeTypes.get(name);
        if (result == null) {
            result = new TreeType(name, color, otherTreeData);
            treeTypes.put(name, result);
        }
        return result;
    }
}

public class Forest extends JFrame {
    private List<Tree> trees = new ArrayList<>();

    public void plantTree(int x, int y, String name, Color color, String otherTreeData) {
        TreeType type = TreeFactory.getTreeType(name, color, otherTreeData);
        Tree tree = new Tree(x, y, type);
        trees.add(tree);
    }

    @Override
    public void paint(Graphics graphics) {
        for (Tree tree : trees) {
            tree.draw(graphics);
        }
    }
}
```

## Proxy

Lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

![[/Untitled 12 9.png|Untitled 12 9.png]]

### Implementation

```Java
public interface ThirdPartyYouTubeLib {
    HashMap<String, Video> popularVideos();

    Video getVideo(String videoId);
}

public class ThirdPartyYouTubeClass implements ThirdPartyYouTubeLib {

    @Override
    public HashMap<String, Video> popularVideos() {
        connectToServer("http://www.youtube.com");
        return getRandomVideos();
    }

    @Override
    public Video getVideo(String videoId) {
        connectToServer("http://www.youtube.com/" + videoId);
        return getSomeVideo(videoId);
    }

    private int random(int min, int max) {
        return min + (int) (Math.random() * ((max - min) + 1));
    }

    private void experienceNetworkLatency() {
        // ...
    }

    private void connectToServer(String server) {
        // ...
    }

    private HashMap<String, Video> getRandomVideos() {
        // ...
        return hmap;
    }

    private Video getSomeVideo(String videoId) {
        System.out.print("Downloading video... ");

        experienceNetworkLatency();
        Video video = new Video(videoId, "Some video title");

        System.out.print("Done!" + "\n");
        return video;
    }

}

public class Video {
    public String id;
    public String title;
    public String data;

    Video(String id, String title) {
        this.id = id;
        this.title = title;
        this.data = "Random video.";
    }
}

public class YouTubeCacheProxy implements ThirdPartyYouTubeLib {
    private ThirdPartyYouTubeLib youtubeService;
    private HashMap<String, Video> cachePopular = new HashMap<String, Video>();
    private HashMap<String, Video> cacheAll = new HashMap<String, Video>();

    public YouTubeCacheProxy() {
        this.youtubeService = new ThirdPartyYouTubeClass();
    }

    @Override
    public HashMap<String, Video> popularVideos() {
        if (cachePopular.isEmpty()) {
            cachePopular = youtubeService.popularVideos();
        } else {
            System.out.println("Retrieved list from cache.");
        }
        return cachePopular;
    }

    @Override
    public Video getVideo(String videoId) {
        Video video = cacheAll.get(videoId);
        if (video == null) {
            video = youtubeService.getVideo(videoId);
            cacheAll.put(videoId, video);
        } else {
            System.out.println("Retrieved video '" + videoId + "' from cache.");
        }
        return video;
    }

    public void reset() {
        cachePopular.clear();
        cacheAll.clear();
    }
}

public class YouTubeDownloader {
    private ThirdPartyYouTubeLib api;

    public YouTubeDownloader(ThirdPartyYouTubeLib api) {
        this.api = api;
    }

    public void renderVideoPage(String videoId) {
        Video video = api.getVideo(videoId);
        System.out.println("\n-------------------------------");
        System.out.println("Video page (imagine fancy HTML)");
        System.out.println("ID: " + video.id);
        System.out.println("Title: " + video.title);
        System.out.println("Video: " + video.data);
        System.out.println("-------------------------------\n");
    }

    public void renderPopularVideos() {
        HashMap<String, Video> list = api.popularVideos();
        System.out.println("\n-------------------------------");
        System.out.println("Most popular videos on YouTube (imagine fancy HTML)");
        for (Video video : list.values()) {
            System.out.println("ID: " + video.id + " / Title: " + video.title);
        }
        System.out.println("-------------------------------\n");
    }
}
```

# Behavioral patterns

## Chain of resposibility

**Chain of Responsibility** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain. Like many other behavioral design patterns, the **Chain of Responsibility** relies on transforming particular behaviors into stand-alone objects called _handlers_.

Each linked handler has a field for storing a reference to the next handler in the chain. In addition to processing a request, handlers pass the request further along the chain. The request travels along the chain until all handlers have had a chance to process it.

Here’s the best part: a handler can decide not to pass the request further down the chain and effectively stop any further processing.

![[/Untitled 13 9.png|Untitled 13 9.png]]

However, there’s a slightly different approach (and it’s a bit more canonical) in which, upon receiving a request, a handler decides whether it can process it. If it can, it doesn’t pass the request any further. So it’s either only one handler that processes the request or none at all. This approach is very common when dealing with events in stacks of elements within a graphical user interface.

![[/Untitled 14 9.png|Untitled 14 9.png]]

### Implementation

```Java
public abstract class Middleware {
    private Middleware next;

    /**
     * Builds chains of middleware objects.
     */
    public static Middleware link(Middleware first, Middleware... chain) {
        Middleware head = first;
        for (Middleware nextInChain: chain) {
            head.next = nextInChain;
            head = nextInChain;
        }
        return first;
    }

    public abstract boolean check(String email, String password);

    protected boolean checkNext(String email, String password) {
        if (next == null) {
            return true;
        }
        return next.check(email, password);
    }
}
```

```Java
public class UserExistsMiddleware extends Middleware {
    private Server server;

    public UserExistsMiddleware(Server server) {
        this.server = server;
    }

    public boolean check(String email, String password) {
        if (!server.hasEmail(email)) {
            System.out.println("This email is not registered!");
            return false;
        }
        if (!server.isValidPassword(email, password)) {
            System.out.println("Wrong password!");
            return false;
        }
        return checkNext(email, password);
    }
}
```

```Java
public class RoleCheckMiddleware extends Middleware {
    public boolean check(String email, String password) {
        if (email.equals("admin@example.com")) {
            System.out.println("Hello, admin!");
            return true;
        }
        System.out.println("Hello, user!");
        return checkNext(email, password);
    }
}
```

## Command

**Command** is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations

![[/Untitled 15 8.png|Untitled 15 8.png]]

![[/Untitled 16 8.png|Untitled 16 8.png]]

### Implementation

```Java
public abstract class Command {
    public Editor editor;
    private String backup;

    Command(Editor editor) {
        this.editor = editor;
    }

    void backup() {
        backup = editor.textField.getText();
    }

    public void undo() {
        editor.textField.setText(backup);
    }

    public abstract boolean execute();
}
```

```Java
public class CopyCommand extends Command {

    public CopyCommand(Editor editor) {
        super(editor);
    }

    @Override
    public boolean execute() {
        editor.clipboard = editor.textField.getSelectedText();
        return false;
    }
}
```

```Java
public class PasteCommand extends Command {

    public PasteCommand(Editor editor) {
        super(editor);
    }

    @Override
    public boolean execute() {
        if (editor.clipboard == null || editor.clipboard.isEmpty()) return false;

        backup();
        editor.textField.insert(editor.clipboard, editor.textField.getCaretPosition());
        return true;
    }
}
```

```Java
JButton ctrlC = new JButton("Ctrl+C");
JButton ctrlX = new JButton("Ctrl+X");
Editor editor = this;

ctrlC.addActionListener( e -> executeCommand(new CopyCommand(editor)));
ctrlC.addActionListener( e -> executeCommand(new CutCommand(editor)));
```

## Iterator

**Iterator** is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

![[/Untitled 17 5.png|Untitled 17 5.png]]

### Implementation

```Java
public interface ProfileIterator {
    boolean hasNext();

    Profile getNext();

    void reset();
}
```

```Java
public class FacebookIterator implements ProfileIterator {
    private Facebook facebook;
    private String type;
    private String email;
    private int currentPosition = 0;
    private List<String> emails = new ArrayList<>();
    private List<Profile> profiles = new ArrayList<>();

    public FacebookIterator(Facebook facebook, String type, String email) {
        this.facebook = facebook;
        this.type = type;
        this.email = email;
    }

    private void lazyLoad() {
        if (emails.size() == 0) {
		       ...
        }
    }

    @Override
    public boolean hasNext() {
        lazyLoad();
        return currentPosition < emails.size();
    }

    @Override
    public Profile getNext() {
					...
        return friendProfile;
    }

    @Override
    public void reset() {
        currentPosition = 0;
    }
}
```

```Java
public interface SocialNetwork {
    ProfileIterator createFriendsIterator(String profileEmail);

    ProfileIterator createCoworkersIterator(String profileEmail);
}
```

```Java
public class Facebook implements SocialNetwork {
	private List<Profile> profiles;
	
	@Override
  public ProfileIterator createFriendsIterator(String profileEmail) {
      return new FacebookIterator(this, "friends", profileEmail);
  }

  @Override
  public ProfileIterator createCoworkersIterator(String profileEmail) {
      return new FacebookIterator(this, "coworkers", profileEmail);
  }
}
```

```Java
public void sendSpamToFriends(String profileEmail, String message) {
    System.out.println("\nIterating over friends...\n");
    iterator = network.createFriendsIterator(profileEmail);
    while (iterator.hasNext()) {
        Profile profile = iterator.getNext();
        sendMessage(profile.getEmail(), message);
    }
}

public void sendSpamToCoworkers(String profileEmail, String message) {
    System.out.println("\nIterating over coworkers...\n");
    iterator = network.createCoworkersIterator(profileEmail);
    while (iterator.hasNext()) {
        Profile profile = iterator.getNext();
        sendMessage(profile.getEmail(), message);
    }
}
```

## Mediator

**Mediator** is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

The Mediator pattern suggests that you should cease all direct communication between the components which you want to make independent of each other. Instead, these components must collaborate indirectly, by calling a special mediator object that redirects the calls to appropriate components. As a result, the components depend only on a single mediator class instead of being coupled to dozens of their colleagues.

![[/Untitled 18 5.png|Untitled 18 5.png]]

### Implementation

```Java
public interface Component {
    void setMediator(Mediator mediator);
    String getName();
}
```

```Java
public class AddButton extends JButton implements Component {
    private Mediator mediator;

    @Override
    public void setMediator(Mediator mediator) {
        this.mediator = mediator;
    }

    @Override
    protected void fireActionPerformed(ActionEvent actionEvent) {
        mediator.addNewNote(new Note());
    }
}
```

```Java
public class DeleteButton extends JButton  implements Component {
    private Mediator mediator;

    @Override
    public void setMediator(Mediator mediator) {
        this.mediator = mediator;
    }

    @Override
    protected void fireActionPerformed(ActionEvent actionEvent) {
        mediator.deleteNote();
    }
}
```

```Java
public interface Mediator {
    void addNewNote(Note note);
    void deleteNote();
}
```

```Java
public class Editor implements Mediator {
		private List list;
		
		@Override
    public void registerComponent(Component component) {
        component.setMediator(this);
        switch (component.getName()) {
            case "AddButton":
                add = (AddButton)component;
                break;
            case "DelButton":
                del = (DeleteButton)component;
                break;
		}

		@Override
    public void addNewNote(Note note) {
        title.setText("");
        textBox.setText("");
        list.addElement(note);
    }

    @Override
    public void deleteNote() {
        list.deleteElement();
    }
		
		public static void main(String[] args) {
        Mediator mediator = new Editor();

        mediator.registerComponent(new AddButton());
        mediator.registerComponent(new DeleteButton());
    }
}
```

## Memento

**Memento** is a behavioural design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation. The Memento pattern delegates creating the state snapshots to the actual owner of that state, the _originator_ object. Hence, instead of other objects trying to copy the editor’s state from the “outside,” the editor class itself can make the snapshot since it has full access to its own state.

The pattern suggests storing the copy of the object’s state in a special object called _memento_. The contents of the memento aren’t accessible to any other object except the one that produced it. Other objects must communicate with mementos using a limited interface which may allow fetching the snapshot’s metadata (creation time, the name of the performed operation, etc.), but not the original object’s state contained in the snapshot.

![[/Untitled 19 5.png|Untitled 19 5.png]]

Such a restrictive policy lets you store mementos inside other objects, usually called _caretakers_. Since the caretaker works with the memento only via the limited interface, it’s not able to tamper with the state stored inside the memento. At the same time, the originator has access to all fields inside the memento, allowing it to restore its previous state at will.

### Implementation

```Java
public class Editor extends JComponent {
    private Canvas canvas;
    private CompoundShape allShapes = new CompoundShape();
    private History history;

		public String backup() {
        try {
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(baos);
            oos.writeObject(this.allShapes);
            oos.close();
            return Base64.getEncoder().encodeToString(baos.toByteArray());
        } catch (IOException e) {
            return "";
        }
    }

    public void restore(String state) {
        try {
            byte[] data = Base64.getDecoder().decode(state);
            ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(data));
            this.allShapes = (CompoundShape) ois.readObject();
            ois.close();
        } catch (ClassNotFoundException e) {
            System.out.print("ClassNotFoundException occurred.");
        } catch (IOException e) {
            System.out.print("IOException occurred.");
        }
    }
}
```

```Java
public class History {
    private List<Pair> history = new ArrayList<Pair>();
    private int virtualSize = 0;

    private class Pair {
        Command command;
        Memento memento;
        Pair(Command c, Memento m) {
            command = c;
            memento = m;
        }

        private Command getCommand() {
            return command;
        }

        private Memento getMemento() {
            return memento;
        }
    }
}
```

```Java

public interface Command {
    String getName();
    void execute();
}

public class ColorCommand implements Command {
    private Editor editor;
    private Color color;
}

public class MoveCommand implements Command {
    private Editor editor;
    private int startX, startY;
    private int endX, endY;
}
```

```Java
public class Memento {
    private String backup;
    private Editor editor;

    public Memento(Editor editor) {
        this.editor = editor;
        this.backup = editor.backup();
    }

    public void restore() {
        editor.restore(backup);
    }
}
```

## Observer

**Observer** is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

The Observer pattern suggests that you add a subscription mechanism to the publisher class so individual objects can subscribe to or unsubscribe from a stream of events coming from that publisher. Fear not! Everything isn’t as complicated as it sounds. In reality, this mechanism consists of 1) an array field for storing a list of references to subscriber objects and 2) several public methods which allow adding subscribers to and removing them from that list.

![[/Untitled 20 4.png|Untitled 20 4.png]]

### Implementation

```Java
public class EventManager {
    Map<String, List<EventListener>> listeners = new HashMap<>();

		public EventManager(String... operations) {
        for (String operation : operations) {
            this.listeners.put(operation, new ArrayList<>());
        }
    }

		public void subscribe(String eventType, EventListener listener) {
        List<EventListener> users = listeners.get(eventType);
        users.add(listener);
    }

    public void unsubscribe(String eventType, EventListener listener) {
        List<EventListener> users = listeners.get(eventType);
        users.remove(listener);
    }

    public void notify(String eventType, File file) {
        List<EventListener> users = listeners.get(eventType);
        for (EventListener listener : users) {
            listener.update(eventType, file);
        }
    }
}
```

```Java
public class Editor {
    public EventManager events;
    private File file;

    public Editor() {
        this.events = new EventManager("open", "save");
    }

    public void openFile(String filePath) {
        this.file = new File(filePath);
        events.notify("open", file);
    }

    public void saveFile() throws Exception {
        if (this.file != null) {
            events.notify("save", file);
        } else {
            throw new Exception("Please open a file first.");
        }
    }
}
```

```Java
public interface EventListener {
    void update(String eventType, File file);
}

public class EmailNotificationListener implements EventListener {
    private String email;

    public EmailNotificationListener(String email) {
        this.email = email;
    }

    @Override
    public void update(String eventType, File file) {
        System.out.println("Email to " + email + ": Someone has performed " + eventType + " operation with the following file: " + file.getName());
    }
}

public class LogOpenListener implements EventListener {
    private File log;

    public LogOpenListener(String fileName) {
        this.log = new File(fileName);
    }

    @Override
    public void update(String eventType, File file) {
        System.out.println("Save to log " + log + ": Someone has performed " + eventType + " operation with the following file: " + file.getName());
    }
}
```

## State

**State** is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

The main idea is that, at any given moment, there’s a _finite_ number of _states_ which a program can be in. Within any unique state, the program behaves differently, and the program can be switched from one state to another instantaneously. However, depending on a current state, the program may or may not switch to certain other states. These switching rules, called _transitions_, are also finite and predetermined.

![[/Untitled 21 4.png|Untitled 21 4.png]]

This structure may look similar to the [**Strategy**](https://refactoring.guru/design-patterns/strategy) pattern, but there’s one key difference. In the State pattern, the particular states may be aware of each other and initiate transitions from one state to another, whereas strategies almost never know about each other.

### Implementation

```Java
*/
public abstract class State {
    Player player;

    /**
     * Context passes itself through the state constructor. This may help a
     * state to fetch some useful context data if needed.
     */
    State(Player player) {
        this.player = player;
    }

    public abstract String onLock();
    public abstract String onPlay();
    public abstract String onNext();
    public abstract String onPrevious();
}
```

```Java
public class LockedState extends State {

    LockedState(Player player) {
        super(player);
        player.setPlaying(false);
    }

    @Override
    public String onLock() {
        if (player.isPlaying()) {
            player.changeState(new ReadyState(player));
            return "Stop playing";
        } else {
            return "Locked...";
        }
    }

    @Override
    public String onPlay() {
        player.changeState(new ReadyState(player));
        return "Ready";
    }

    @Override
    public String onNext() {
        return "Locked...";
    }

    @Override
    public String onPrevious() {
        return "Locked...";
    }
}
```

```Java
public class ReadyState extends State {

    public ReadyState(Player player) {
        super(player);
    }

    @Override
    public String onLock() {
        player.changeState(new LockedState(player));
        return "Locked...";
    }

    @Override
    public String onPlay() {
        String action = player.startPlayback();
        player.changeState(new PlayingState(player));
        return action;
    }

    @Override
    public String onNext() {
        return "Locked...";
    }

    @Override
    public String onPrevious() {
        return "Locked...";
    }
}
```

```Java
public class Player {
    private State state;
    private boolean playing = false;
    private List<String> playlist = new ArrayList<>();
    private int currentTrack = 0;

    public Player() {
        this.state = new ReadyState(this);
        setPlaying(true);
        for (int i = 1; i <= 12; i++) {
            playlist.add("Track " + i);
        }
    }

    public void changeState(State state) {
        this.state = state;
    }

    public State getState() {
        return state;
    }
}
```

```Java
JButton play = new JButton("Play");
play.addActionListener(e -> textField.setText(player.getState().onPlay()));
JButton stop = new JButton("Stop");
stop.addActionListener(e -> textField.setText(player.getState().onLock()));
JButton next = new JButton("Next");
next.addActionListener(e -> textField.setText(player.getState().onNext()));
JButton prev = new JButton("Prev");
prev.addActionListener(e -> textField.setText(player.getState().onPrevious()));
```

## Strategy

**Strategy** is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

The Strategy pattern suggests that you take a class that does something specific in a lot of different ways and extract all of these algorithms into separate classes called _strategies_.

The original class, called _context_, must have a field for storing a reference to one of the strategies. The context delegates the work to a linked strategy object instead of executing it on its own.

The context isn’t responsible for selecting an appropriate algorithm for the job. Instead, the client passes the desired strategy to the context. In fact, the context doesn’t know much about strategies. It works with all strategies through the same generic interface, which only exposes a single method for triggering the algorithm encapsulated within the selected strategy.

This way the context becomes independent of concrete strategies, so you can add new algorithms or modify existing ones without changing the code of the context or other strategies.

![[/Untitled 22 4.png|Untitled 22 4.png]]

[**Command**](https://refactoring.guru/design-patterns/command) and [**Strategy**](https://refactoring.guru/design-patterns/strategy) may look similar because you can use both to parameterize an object with some action. However, they have very different intents.

- You can use _Command_ to convert any operation into an object. The operation’s parameters become fields of that object. The conversion lets you defer execution of the operation, queue it, store the history of commands, send commands to remote services, etc.
- On the other hand, _Strategy_ usually describes different ways of doing the same thing, letting you swap these algorithms within a single context class.

### Implementation

```Java
public interface PayStrategy {
    boolean pay(int paymentAmount);
    void collectPaymentDetails();
}
```

```Java
public class PayByPayPal implements PayStrategy {
    private static final Map<String, String> DATA_BASE = new HashMap<>();
    private final BufferedReader READER = new BufferedReader(new InputStreamReader(System.in));
    private String email;
    private String password;
    private boolean signedIn;
		
		@Override
    public void collectPaymentDetails() {
        ...
    }

    private boolean verify() {
        setSignedIn(email.equals(DATA_BASE.get(password)));
        return signedIn;
    }

    @Override
    public boolean pay(int paymentAmount) {
       ...
    }
```

```Java
public class PayByCreditCard implements PayStrategy {
    private final BufferedReader READER = new BufferedReader(new InputStreamReader(System.in));
    private CreditCard card;
		
		@Override
    public void collectPaymentDetails() {
        ...
    }

    /**
     * After card validation we can charge customer's credit card.
     */
    @Override
    public boolean pay(int paymentAmount) {
        ...
    }

    private boolean cardIsPresent() {
        return card != null;
    }
}
```

```Java
public class Order {
    private int totalCost = 0;
    private boolean isClosed = false;

    public void processOrder(PayStrategy strategy) {
        strategy.collectPaymentDetails();
        // Here we could collect and store payment data from the strategy.
    }
		
		public static void main(String[] args) throws IOException {
						if (paymentMethod.equals("1")) {
                    strategy = new PayByPayPal();
                } else {
                    strategy = new PayByCreditCard();
                }
            }
            order.processOrder(strategy);
            strategy.pay(order.getTotalCost())
		}
}
```

## Template method

**Template Method** is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

The Template Method pattern suggests that you break down an algorithm into a series of steps, turn these steps into methods, and put a series of calls to these methods inside a single _template method._ The steps may either be `abstract`, or have some default implementation. To use the algorithm, the client is supposed to provide its own subclass, implement all abstract steps, and override some of the optional ones if needed (but not the template method itself).

![[/Untitled 23 4.png|Untitled 23 4.png]]

### Implementation

```Java
public abstract class Network {
    String userName;
    String password;

    Network() {}

    /**
     * Publish the data to whatever network.
     */
    public boolean post(String message) {
        // Authenticate before posting. Every network uses a different
        // authentication method.
        if (logIn(this.userName, this.password)) {
            // Send the post data.
            boolean result =  sendData(message.getBytes());
            logOut();
            return result;
        }
        return false;
    }

    abstract boolean logIn(String userName, String password);
    abstract boolean sendData(byte[] data);
    abstract void logOut();
}
```

```Java
public class Facebook extends Network {
    public Facebook(String userName, String password) {
        this.userName = userName;
        this.password = password;
    }

    public boolean logIn(String userName, String password) {
        ...
    }

    public boolean sendData(byte[] data) {
        ...
    }

    public void logOut() {
				...
    }
}
```

```Java
Network network = null;
// Create proper network object and send the message.
if (choice == 1) {
    network = new Facebook(userName, password);
} else if (choice == 2) {
    network = new Twitter(userName, password);
}
network.post(message);
```

## Visitor

**Visitor** is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

The Visitor pattern suggests that you place the new behavior into a separate class called _visitor_, instead of trying to integrate it into existing classes. The original object that had to perform the behavior is now passed to one of the visitor’s methods as an argument, providing the method access to all necessary data contained within the object.

![[/Untitled 24 4.png|Untitled 24 4.png]]

```Java
public interface Shape {
    void move(int x, int y);
    void draw();
    String accept(Visitor visitor);
}
```

```Java
public class Dot implements Shape {
    private int id;
    private int x;
    private int y;
		
		@Override
    public String accept(Visitor visitor) {
        return visitor.visitDot(this);
    }
}

public class Circle extends Dot {
    private int radius;
		
		@Override
    public String accept(Visitor visitor) {
        return visitor.visitCircle(this);
    }
}

public class Rectangle implements Shape {
    private int id;
    private int x;
    private int y;
    private int width;
    private int height;

		@Override
    public String accept(Visitor visitor) {
        return visitor.visitRectangle(this);
    }
}
```

```Java
public interface Visitor {
    String visitDot(Dot dot);

    String visitCircle(Circle circle);

    String visitRectangle(Rectangle rectangle);

    String visitCompoundGraphic(CompoundShape cg);
}
```

```Java
public class XMLExportVisitor implements Visitor {

    public String export(Shape... args) {
        StringBuilder sb = new StringBuilder();
        sb.append("<?xml version=\"1.0\" encoding=\"utf-8\"?>" + "\n");
        for (Shape shape : args) {
            sb.append(shape.accept(this)).append("\n");
        }
        return sb.toString();
    }

    public String visitDot(Dot d) {
        return "<dot>" + "\n" +
                "    <id>" + d.getId() + "</id>" + "\n" +
                "    <x>" + d.getX() + "</x>" + "\n" +
                "    <y>" + d.getY() + "</y>" + "\n" +
                "</dot>";
    }

    public String visitCircle(Circle c) {
        return "<circle>" + "\n" +
                "    <id>" + c.getId() + "</id>" + "\n" +
                "    <x>" + c.getX() + "</x>" + "\n" +
                "    <y>" + c.getY() + "</y>" + "\n" +
                "    <radius>" + c.getRadius() + "</radius>" + "\n" +
                "</circle>";
    }
...
}
```

[Patterns in Spring](https://www.notion.so/Patterns-in-Spring-91b687b403ca4f5e8b28223c330d6436?pvs=21)

# Design Patterns In Spring

## **Singleton pattern**

The singleton pattern is a mechanism that ensures only one instance of an object exists per applocation.

- **Singleton bean scope**

## **Factory Method pattern**

The factory method pattern entails a factory class with an abstract method for creating the desired object.

- **Application Context** - Spring treats a bean container as a factory that produces beans. Each of the getBean methods is considered a factory method,
- **External Configuration we can completely change the application's behavior based on external configuration. If we wish to change the implementation of the autowired objects in the application, we can adjust the ApplicationContext implementation we use.**

## **Proxy pattern**

The proxy pattern is a technique that allows one object — the proxy — to control access to another object — the subject or service.

- Transactions - Spring creates a proxy that wraps our BookRepository bean and instruments our bean to execute our create method atomically.

## **Template pattern**

The template method pattern is a technique that defines the steps required for some action, implementing the boilerplate steps, and leaving the customizable steps as abstract. Subclasses can then implement this abstract class and provide a concrete implementation for the missing steps.

- JdbcTemplate

  

  

![[/Untitled 25 4.png|Untitled 25 4.png]]

# **SOLID**

## **Смысл принципа**

Набор принципов необходимый для создания легко поддерживаемой системы

## **Расшифровка**

**Single responsibility principle**

Каждый объект должен иметь одну ответственность и эта ответственность должна быть полностью инкапсулирована в класс. Пример нарушения -

класс содержит логику работы с бд и бизнес логику

**Open–closed principle(Принцип открытости/закрытости)**

Программные сущности должны быть открыты для расширения, но закрыты для модификации. Сущность может быть расширена путем создания новых типов сущностей, но расширение поведения не должно влечь за собой изменение кода, который использует эту сущность. Пример нарушения - в методе проверяется тип класса и в зависимости от него выбирается действие

**Liskov substitution principle(Принцип подстановки Лисков)**

Функции, которые используют базовый тип, должны иметь возможность использовать подтипы базового типа, не зная об этом. Пример нарушения - в методе проверяется тип класс

**Interface segregation principle (Принцип разделения интерфейса)**

Много интерфейсов, специально предназначенных для клиентов, лучше, чем один интерфейс общего назначения. Пример нарушения - класс имеет пустые реализации методов интерфейса

**Dependency Inversion (Принцип инверсии зависимостей)**

Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.

  

Пример нарушения - высокоуровневые компоненты зависят от низкоуровневыхclass PasswordReminder {

private mySQLConnection dbConnection;

}

MySQLConnection является низкоуровневым модулем, PasswordReminder - высокоуровневый. Но в соответствии с определением принципа, гласящим разделять абстракции от реализации, этот фрагмент его нарушает, т.к. класс PasswordReminder зависит от класса MySQLConnection. Верный вариант:

class PasswordReminder {

private dbConnectionInterface dbConnection;

}

## **Схожие термины**

**YAGNI**

**You aren't gonna need it -** Процесс и принцип проектирования ПО, при котором в качестве основной цели и/или ценности декларируется отказ от избыточной функциональности, — то есть отказ добавления функциональности, в которой нет непосредственной надобности.(Не надо делать на всякий случай)

**KISS**

Большинство систем работают лучше всего, если они остаются простыми, а не усложняются. Поэтому в области проектирования простота должна быть одной из ключевых целей

**DRY**

Принцип разработки программного обеспечения, нацеленный на снижение повторения информации различного рода

**Правило бойскаута**

Всегда оставляйте код после себя чище, чем он был

# **Паттерны**

Повторимая архитектурная конструкция, представляющая собой решение проблемы проектирования в рамках некоторого часто возникающего контекста.

## **Поведенческие**

### **Итератор**

Дает возможность последовательно обходить элементы составных объектов, не раскрывая их внутреннего представления.

### **Цепочка обязанностей**

Позволяет передавать запросы последовательно по цепочке обработчиков. Каждый последующий обработчик решает, может ли он обработать запрос сам и стоит ли передавать запрос дальше по цепи.

### **Снимок**

Позволяет сохранять и восстанавливать прошлые состояния объектов, не раскрывая подробностей их реализации.(Выполнять сохранение своего состояния должен сам объект, например .clone() Serializable)

### Observer

Создает механизм подписки, позволяющий одним объектам следить и реагировать на события, происходящие в других объектах.

[![](https://lh3.googleusercontent.com/32pra7orL16BNNBO0r7BGtO8dZZ4W549RSR8EDr-BIR6mHjfQ18-cXImWSzQv9y_Kql6OH9D9p6VOu23GiRNgeMD-p_fxFpRUaKibtEkYFGoA869BINRTjLZQMndhB-c6f4I9rpHr5vjLG-fYPeic0v1GyoaO6ZqWRQ5fXr3g1o-Z7z16aBD7fz0wyX5)](https://lh3.googleusercontent.com/32pra7orL16BNNBO0r7BGtO8dZZ4W549RSR8EDr-BIR6mHjfQ18-cXImWSzQv9y_Kql6OH9D9p6VOu23GiRNgeMD-p_fxFpRUaKibtEkYFGoA869BINRTjLZQMndhB-c6f4I9rpHr5vjLG-fYPeic0v1GyoaO6ZqWRQ5fXr3g1o-Z7z16aBD7fz0wyX5)

![[/Untitled 26 3.png|Untitled 26 3.png]]

![[/Untitled 27 3.png|Untitled 27 3.png]]

![[/Untitled 28 3.png|Untitled 28 3.png]]

### **Команда**

Преобразует запросы в объекты и передает их в методы.(лямбды)

![[/Untitled 29 3.png|Untitled 29 3.png]]

![[/Untitled 30 3.png|Untitled 30 3.png]]

![[/Untitled 31 3.png|Untitled 31 3.png]]

![[/Untitled 32 3.png|Untitled 32 3.png]]

  

### **Посредник**

Уменьшает связность объектов, путем создания объекта посредника, который знает, каким объектам передавать запросы.(DispatcherServlet)

## Mediator

![[/Untitled 33 3.png|Untitled 33 3.png]]

![[/Untitled 34 3.png|Untitled 34 3.png]]

![[/Untitled 35 3.png|Untitled 35 3.png]]

### **Состояние**

Позволяет изменять поведение объекта в зависимости от его состояния

### **Стратегия**

Позволяет выделить семейство алгоритмов в отдельные классы и в рантайме подставлять нужный(Сортированные коллекции TreeSet - по умолчанию и с заданным компаратором)

### Template method Pattern

Определяет скелет алгоритма, перекладывая особенности его реализации на подклассы. Позволяет подклассам менять шаги, не меняя его общей структуры(Реализованные методы InputStream, OutputStream)

![[/Untitled 36 3.png|Untitled 36 3.png]]

  

![[/Untitled 37 3.png|Untitled 37 3.png]]

  

![[/Untitled 38 3.png|Untitled 38 3.png]]

![[/Untitled 39 3.png|Untitled 39 3.png]]

![[/Untitled 40 3.png|Untitled 40 3.png]]

![[/Untitled 41 3.png|Untitled 41 3.png]]

### **Посетитель**

Размещает новое для классов поведение в отдельный класс(FileVisitor)

## **Структурные**

### Adapter

Позволяет объектам с несовместимыми интерфейсами взаимодействовать.(InputStreamReader)

![[/Untitled 42 3.png|Untitled 42 3.png]]

![[/Untitled 43 3.png|Untitled 43 3.png]]

![[/Untitled 44 3.png|Untitled 44 3.png]]

![[/Untitled 45 2.png|Untitled 45 2.png]]

![[/Untitled 46 2.png|Untitled 46 2.png]]

### **Фасад**

Скрытие сложности системы, путем предоставления единого объекта, управляющего доступом к разным частям системы(ServletContext)

### **Компоновщик**

Собирает множество объектов в один, в виде древовидной структуры.

### **Декоратор**

Позволяет добавлять объектам новую функциональность в рантайме. Класс оборачивает целевой и добавляет ему новый функционал(Collections.unmodifieble)

![[/Untitled 47 2.png|Untitled 47 2.png]]

![[/Untitled 48 2.png|Untitled 48 2.png]]

**Reasons to Favour Composition over Inheritance in Java and OOP:**

1. The fact that Java does not support multiple inheritances is one reason for favoring composition over inheritance in Java. Since you can only extend one class in Java, but if you need multiple features, such as reading and writing character data into a file, you need Reader and Writer functionality. It makes your job simple to have them as private members, and this is called Composition.
2. Composition offers better test-ability of a class than Inheritance. If one class consists of another class, you can easily construct a Mock Object representing a composed class for the sake of testing. This privilege is not given by inheritance.
3. Although both Composition and Inheritance allow you to reuse code, one of Inheritance’s disadvantages is that it breaks encapsulation. If the subclass depends on the action of the superclass for its function, it suddenly becomes fragile. When super-class behavior changes, sub-class functionality can be broken without any modification on its part.
4. In the timeless classic Design Patterns, several object-oriented design patterns listed by Gang of Four: Elements of Reusable Object-Oriented Software, favor Composition over Inheritance. Strategy design pattern, where composition and delegation are used to modify the behavior of Context without touching context code, is a classical example of this. Instead of getting it by inheritance, because Context uses composition to carry strategy, it is simple to have a new implementation of strategy at run-time.
5. Another reason why composition is preferred over inheritance is flexibility. If you use Composition, you are flexible enough to replace the better and updated version of the Composed class implementation. One example is the use of the comparator class, which provides features for comparison.

Inheritance

```Plain
class Person {
 String title;
 String name;
 int age;
}

class Employee extends Person {
 int salary;
 String title;
}
```

Composition

```Plain
class Person {
 String title;
 String name;
 int age;

 public Person(String title, String name, String age) {
    this.title = title;
    this.name = name;
    this.age = age;
 }

}

class Employee {
 int salary;
 private Person person;

 public Employee(Person p, int salary) {
     this.person = p;
     this.salary = salary;
 }
}
```

![[/Untitled 49 2.png|Untitled 49 2.png]]

![[/Untitled 50 2.png|Untitled 50 2.png]]

![[/Untitled 51 2.png|Untitled 51 2.png]]

![[/Untitled 52 2.png|Untitled 52 2.png]]

### **Легковес**

Позволяет вместить больше объектов в меньший объем памяти(String Pool)

### **Мост**

Разделяет классы на несколько отдельных иерархий, позволяя уменьшить их связанность

  

### Proxy

Подставляет вместо реальных объектов объекты-заместители, которые перехватывают вызов к оригинальному объекту.

![[/Untitled 53 2.png|Untitled 53 2.png]]

![[/Untitled 54 2.png|Untitled 54 2.png]]

![[/Untitled 55 2.png|Untitled 55 2.png]]

![[/Untitled 56 2.png|Untitled 56 2.png]]

![[/Untitled 57 2.png|Untitled 57 2.png]]

## Interpreter

![[/Untitled 58 2.png|Untitled 58 2.png]]

![[/Untitled 59 2.png|Untitled 59 2.png]]

![[/Untitled 60 2.png|Untitled 60 2.png]]

## **Порождающие**

### Factor

Метод, который в зависимости от требования создает объект подкласса интерфейса(интерфейс Game, подклассы Chess и тд)

![[/Untitled 61 2.png|Untitled 61 2.png]]

![[/Untitled 62 2.png|Untitled 62 2.png]]

![[/Untitled 63 2.png|Untitled 63 2.png]]

![[Untitled 64.png]]

![[Untitled 65.png]]

![[Untitled 66.png]]

### **Абстрактная фабрика**

создание семейства связанных объектов, не привязанных к конкретным реализациям. Есть общая абстрактная фабрика, от которой реализуются конкретные(GameFactory и реализации ChessGameFactory и тд)

### **Прототип**

Копирование объектов не вдаваясь в подробности их реализации. Копирование поручается самим копируемым объектам(Clonable)

![[Untitled 67.png]]

![[Untitled 68.png]]

![[Untitled 69.png]]

### Builder

Разделяет создание сложного объекта на несколько этапов. Создается соответствующий класс билдер, с помощью которого создается объект.(Lombok builder)

![[Untitled 70.png]]

![[Untitled 71.png]]

![[Untitled 72.png]]

### Singleton

Гарантирует, что у класса есть только один экземпляр(Scope singletone)

Минусы

Наличие синглтона понижает тестируемость приложения в целом и классов, которые используют синглтон,

![[Untitled 73.png]]

![[Untitled 74.png]]

![[Untitled 75.png]]

![[Untitled 76.png]]

  

# **GRASP**

**General responsibility assignment software patterns**  — общие шаблоны распределения ответственностей.

### **Информационный эксперт**

Ответственность должна быть назначена тому, кто обладает максимум необходимой информации

### **Создатель**

Создавать объекты нужно только в тот момент, когда они нужны и только там, где они используются

### **Controller**

Отвечает за обработку запросов и решает кому должен делегировать запросы на выполнение.

### **Слабая связанность**

Необходимо поддерживать более низкую связность между объектами

### **Высокое зацепление**

Класс старается выполнить как можно меньше не специфичных для него задач

### **Чистая выдумка**

Искусственная сущность для достижения слабой связанности и высокого зацепления

### **Посредник**

Снижает связность между объектами путем добавления промежуточного. Пример

Контроллер соединяет модель и отображение

### **Полиморфизм**

Ответственность за определение вариации поведения должна возлагаться на тип, для которого это изменение происходит

### **Устойчивость к изменениям**

Защищает элементы от изменений других элементов

  

## Repository pattern

![[Untitled 77.png]]

![[Untitled 78.png]]

![[Untitled 79.png]]

![[Untitled 80.png]]

![[Untitled 81.png]]

## Modell-View-Controller

![[Untitled 82.png]]

![[Untitled 83.png]]

![[Untitled 84.png]]

## Inversion of control

![[Untitled 85.png]]

![[Untitled 86.png]]

![[Untitled 87.png]]

![[Untitled 88.png]]

![[Untitled 89.png]]