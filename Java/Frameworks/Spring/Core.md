# Spring Core

**Spring** is an application framework and [inversion of control] container for the Java platform

**Framework** - Platform that defines application structure

Core part consists of:
1. [AOP](AOP.md)
2. [IOC](IOC.md)
3. [Resources](Resources.md)


There are also other parts, such as:
1. [Boot](Boot.md)
2. [Cloud](Cloud.md)
3. [Data](Data.md)
4. [JMS](JMS.md)
5. [MVC](MVC.md)
6. [Security](Security.md)
7. [Test](Test.md)

## Advantages:

- **Light Weight:** Spring is a lightweight framework because of its POJO implementation. It does not force the programmer to inherit any class and implement any interface. With the help of Spring, we can enable powerful, scalable applications using POJOs (Plain Old Java Object).
- **Flexible:** It provides flexible libraries trusted by developers all over the world. The developer can choose either XML, Java-based or annotations for configuration options. The IoC and DI features provide the foundation for a wide-ranging set of features and functionality. It makes the job simpler.
- **Loose Coupling:** Spring applications are loosely coupled because of dependency injection. It handles injecting dependent components without a component knowing where they came from
- **Declarative Support:** It provides declarative support for caching, validation, transaction, and formatting.
- **Cross-cutting behavior:** Resource management is a cross-cutting concern, easy to copy and paste everywhere(AOP)
- **Configuration:** It provides a consistent way of configuring everything, separate configuration from application logic, varying configuration.
- **Lifecycle:** Responsible for managing all your application components, particularly those in middle-tier container sees components through well-defined lifecycle: init(), destroy().
- **Dependency Injection:** The use of dependency injection makes the easy development of JavaEE.
- **Easier Testing:** The use of dependency injection makes the testing easy. The spring framework does not require a server while the EJB and Struts application requires a server.
- **Fast:** The team of Spring engineers deeply cares about the performance. Its fast startup, fast shutdown, and optimized execution maintain performance make it fast.
- **Secure:** It monitors third-party dependencies closely. The regular update is issued that make our data and application secure. We can make our application secure by using the Spring Security framework. It provides industry-standard security schemes and delivers a trustworthy solution that is secure by default.

## Disadvantages

- **Complexity:** Working with Spring is more complex. It requires a lot of expertise. If you have not used Spring before, first you will have to learn. The learning curve is also difficult, so if you have not a lot of development experience, it is difficult to learn.
- **Parallel Mechanism:** It provides multiple options to developers. These options create confusion to developers that which feature to use and which to not and wrong decisions may lead to significant delays. (XML, Java-based, Annotations configuration)
- **Lots of XML:** Developing a Spring application requires lots of XML in old versions

## [IOC](IOC.md)

## **Bean**
POJO, who’s lifecycle is driven by container.
Можно создать абстрактный бин - как шаблон для других бинов
Соответственно есть наследование бинов

  

**Beans Thread safety**

Beans are not thread safe, regardless their scopes. To make bean safe, you have to make your object thread safe:

- Design your beans **immutable**
- **Design your beans stateless:**
- Design your beans with locks
- Design your beans [**persistent**](http://en.wikipedia.org/wiki/Persistent_data_structure)

### **Bean scope**
- singleton(default) - one object for all application. Created when Context instantiated
- prototype - new object each time. Created, when object requested.
- Web App Scopes
    - request
    - session
    - application - ServletContext lifecycle
    - websocket - WebSocket session

**@Lazy** - позволяет создавать бин только во время запроса

### Bean Creation
- **@ComponentScan** - When working with Spring, we can annotate our classes in order to make them into Spring beans. Furthermore, **we can tell Spring where to search for these annotated classes,** as not all of them must become beans in this particular run.  
    We use the   
    _@ComponentScan_ annotation along with the _@Configuration_ annotation to specify the packages that we want to be scanned. _@ComponentScan_ without arguments tells Spring to scan the current package and all of its sub-packages.
- **@Configuration** - указывает, что данный класс содержит методы, создающие бины. _@Configuration_ is also a _@Component_. A class annotated with `@Configuration` cannot be final because Spring will use CGLIB to create a proxy for `@Configuration` class.
    
    - **@Bean** - defines that this method returns bean. It couldn’t be final, because, Spring needs to override methods from parent class for the proxy to work correctly, however, final method cannot be overridden, having such a method will make CGLIB fail.  
        When using  
        `@Bean` without specifying name or alias, the default bean ID will be created based on the name of the method which was annotated with `@Bean` annotation.  
        You can override this behaviour by specifying name or aliases for the bean.  
        **Alias** is always the **second name** of the bean. The **first name** will become the **ID**.  
          
        
    
    ```java
    @Bean(name={"firstName","name1"}) //ID = firstName, Alias = name1
    public SpringBean springBean(){...}
    ```
    
- **@Component** - указывает, что данный класс является бином
    - @Controller
    - @Service
    - @Repository

### **Bean lifecycle**

1. Object Instantiation
2. **Setters are called** - propperties from configuration set and dependencies injected
3. Вызов методов ***aware** интерфейсов для настройки инфраструктуры(Реализация паттерна observer. Во время инициализации спринга идет проверка бинов имплементирующих один из этих интерфейсов, если такой найден, у него вызывается set метод). Spring предлагает широкий спектр Aware интерфейсов обратного вызова, которые позволяют bean-компонентам указывать контейнеру, что им требуется определенная зависимость от инфраструктуры. Как правило, имя указывает тип зависимости.
    1. setBeanName интерфейса BeanNameAware - для плолучения имени бина в BeanFactory
    2. setBeanFactory интерфейса BeanFactoryAware получения BeanFactory
    3. и тд

Еще раз обратите внимание, что использование этих интерфейсов связывает ваш код с Spring API и не соответствует стилю Inversion of Control. В результате рекомендуется использовать их для компонентов инфраструктуры, которые требуют программного доступа к контейнеру.

1. **PreInitialization** - BeanPostProcessor postProcessBeforeInitialization. Allows to interject into process of bean setup, before they are in a container.
2. **Initialization**
    1. InitializingBeans метод afterProppertiesSet
    2. @PostConstruct
    3. Кастомный init метод - @Bean(initMethod="init")
3. **PostInitialization** метод postProcessAfterInitialization() интерфейса BeanPostProcessor,двухфазовый конструктор. Если внутри конструктора мы манипулируем пропертями бина, которые являются объектами, мы получим NPE. То есть нужно две фазы конструктора – из инит-метода можем  инициализировать поля созданного бина. Allows to interject into process of bean setup, before they are in a container. Proxy should be created here.
4. **Использование бина**
5. **Закрытие контейнера**
6. **Уничтожение**
    1. Метод destroy() интерфейса DisposableBean.
    2. Метод с аннотацией @PreDestroy;
    3. кастомный destroy-method или @Bean(destroyMethod="destroy")
        1. Если scope singleton, то вызовется после закрытия контекста
        2. Если scope prototype, то не вызовется

### **BeanDefenitionReader**

Интерфейс, объявляющий методы для чтения бинов из ресурсов или по пути:

- AbstractBeanDefinitionReader
- GroovyBeanDefinitionReader
- PropertiesBeanDefinitionReader
- XmlBeanDefinitionReader

### **BeanFactoryPostProcessor**

Это интерфейс, который позволяет настраивать BeanDefinitions до того, как создаются бины. Он работает на этапе, когда кроме BeanDefinitions еще ничего нету (когда никаких бинов еще нет, ничего нет) и может что-то “подкрутить” в BeanDefinitions. Он содержит единственный метод postProcessorBeanFactory()(который в качестве параметра принимает ConfigurableListableBeanFactory — о ней написано ниже) у которого есть доступ к BeanFactory, т.е. мы можем как-то повлиять на BeanFactory до того, как он начнет работать и повлиять на BeanDefinitions до того, как BeanFactory из них начнет создавать бины.

### **BeanDefinition**

Интерфейс, который описывает бин, его свойства, аргументы конструктора и другую метаинформацию.

**Объявление бина**

```java
<bean id="someCat" class="com.ethickeep.entities.Cat"/>
```

### Annotations

**@Qualifier**

Qualifies which bean should be injected

```java
@Qualifier("dog")

public Person(@Qualifier("dog") Pet pet)
```

**@Primary**

Указание, что использовать данный бин, в случае, если есть несколько кандидатов

**@Value**

Указание конкретного значения для внедрения

```java
@Value("${person.age}")
private Double age;
```

**@Profile**

Maps the bean to that particular profile

```java
// -Dspring.profiles.active=dev

@Component
@Profile("dev")
public class DevDatasourceConfig

@Component
@Profile("!dev")
public class ProdDatasourceConfig
```

**@Order**

Implies a specific order, in which the beans will be loaded or prioritized by Spring.

Lower numbers indicate a higher priority. The feature may be used to add beans in a specific order into a collection (ie via `@Autowired`), among other things. Due to its influence on injection precedence, it may seem like it might influence the singleton startup order also. But in contrast, the dependency relationships and _@DependsOn_ declarations determine the singleton startup order.

**@DependsOn**

The _@DependsOn_ annotation can force the Spring IoC container to initialize one or more beans before the bean which is annotated by _@DependsOn_ annotation.

The _@DependsOn_ annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with [**@Bean**](https://www.javaguides.net/2018/09/spring-bean-annotation-with-example.html).

```java
@Configuration
public class AppConfig {

    @Bean("firstBean")
    @DependsOn(value = {
        "secondBean",
        "thirdBean"
    })
    public FirstBean firstBean() {
        return new FirstBean();
    }

    @Bean("secondBean")
    public SecondBean secondBean() {
        return new SecondBean();
    }

    @Bean("thirdBean")
    public ThirdBean thirdBean() {
        return new ThirdBean();
    }
}
```

  

**@ConfigurationProperties**

Externalized configuration and easy access to properties defined in properties files.

```java
@Configuration
@ConfigurationProperties(prefix = "mail")
public class ConfigProperties {
    
    private String hostName;
    private int port;
    private String from;

    // standard getters and setters
}


//mail.hostname=host@mail.com
//mail.port=9000
//mail.from=mailer@mail.com
```

## **Event**

Спринг позволяет создавать ивенты, которые можно вызывать из контекста

@**EventListener -** Класс, который следит за ивентом, и в случае его вызова выполняет какой-либо метод.

### **ApplicationListener**

Он умеет слушать контекст Spring, все “events”, которые с ним происходят. Работает на этапе, когда все уже создано. Также он имеет дженерики <>, в которых мы можем указать что конкретно мы хотим слушать. Обозначается аннотацией @EventListener.

- contextStartedEvent — контекст начал свое построение (не построился, а только начал)
- contextRefreshedEvent — когда контекст заканчивает свое построение, он всегда делает refresh.
- contextStoppedEvent
- contextClosedEvent

# Patterns
**Singleton pattern**
**Singleton Beans** - Spring’s approach differs from the strict definition of a singleton since an application can have more than one Spring container. Therefore, **multiple objects of the same class can exist in a single application if we have multiple containers.**
**Factory Method Pattern**
The factory method pattern entails a factory class with an abstract method for creating the desired object.

**Application Context** - Spring treats a bean container as a factory that produces beans.
1. Spring defines the _BeanFactory_ interface as an abstraction of a bean container
2. Each of the _getBean_ methods is considered a factory method, which returns a bean matching the criteria supplied to the method

**External Configuration**
If we wish to change the implementation of the autowired objects in the application, we can adjust the _ApplicationContext_ implementation we use. For example, we can change the _AnnotationConfigApplicationContext_ to an XML-based configuration class, such as ClassPathXmlApplicationContext

**Proxy**
The proxy pattern is a technique that allows one object — the proxy — to control access to another object — the subject or service.
**Transactions** - In Spring, beans are proxied to control access to the underlying bean. We see this approach when using transactions. This annotation instructs Spring to atomically execute our _create_ method. Without a proxy, Spring wouldn’t be able to control access to our bean and ensure its transactional consistency.

**Template Method Pattern**
In many frameworks, a significant portion of the code is boilerplate code.
**Templates & Callbacks**
When executing a query on a database, the same series of steps must be completed:
1. Establish a connection
2. Execute query
3. Perform cleanup
4. Close the connection
These steps are an ideal scenario for the template method pattern.
We can provide the missing step by supplying a callback method.

A callback method is a method that allows the subject to signal to the client that some desired action has completed.
