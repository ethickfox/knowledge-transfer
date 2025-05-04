---
Interview graded: true
Last edited time: 2024-04-21T17:10
Last recall: 2023-08-03
Needs Rework: false
Status: Not started
Topic:
  - "[Spring](Spring)"
---
**Spring** is an [application framework](https://en.wikipedia.org/wiki/Application_framework) and [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control) [container](https://en.wikipedia.org/wiki/Servlet_container) for the [Java platform](https://en.wikipedia.org/wiki/Java_platform).

**Framework** - Platform that defines application structure
1. [AOP](AOP.md)
2. [Boot](Boot.md)
3. [Cloud](Cloud.md)
4. [Data](Data.md)
5. [JMS](JMS.md)
6. [MVC](MVC.md)
7. [Security](Security.md)
8. [Test](Test.md)
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

## **Inversion Of Control**

Abstract principle that passes responsibility of creation and managing of objects for some external entity - IOC Container. Spring framework provides two implementations of IOC container in the form of [differencebetweenbeanfactoryvsa](differencebetweenbeanfactoryvsa)

[![](https://lh6.googleusercontent.com/H8KA3rx6kzfF8Sh9DDpHZq4dDjamGY4Cz9_1w0sQMNwQg4LdZIaw7NMZ7_hRDJwOSX9JuIv3Pd85CDMzw5_eG-d-xi9GGeeWe4wyiUW50UxjRcDBZzmMS0IVu_UwmsGrSR0FuJniLz2lGaMZfTyaDLfTwNpYsf41D_tMw01E5k8_i-bZZwzlLPyR7Uo2)](https://lh6.googleusercontent.com/H8KA3rx6kzfF8Sh9DDpHZq4dDjamGY4Cz9_1w0sQMNwQg4LdZIaw7NMZ7_hRDJwOSX9JuIv3Pd85CDMzw5_eG-d-xi9GGeeWe4wyiUW50UxjRcDBZzmMS0IVu_UwmsGrSR0FuJniLz2lGaMZfTyaDLfTwNpYsf41D_tMw01E5k8_i-bZZwzlLPyR7Uo2)

## **Dependency Injection**

Object configuration style, which passes responsibility for object fields definition to another, external entity. It lowers coupling between objects. Object is being provided of all necessary dependencies when created.

- **Reduce coupling -** Both constructor and setter dependency injection reduce coupling.
- **Improves testability -** Dependency Injection allows to replace the actual object with a mock object which improves testability by writing simple JUnit tests which uses a mock object.
- **Flexibility -** This is another advantage which comes as a side benefit of reduced coupling, because of DI you can replace non-performance implementation with a better one later.

**Field Injection**

Not recommended because harder to test

- Тк тогда есть жесткая связь с Di контейнером
- Не может быть инициализирован объект без рефлексии(например для тестов)
- Явные зависимости класса скрыты для внешних пользователей
- В целом ухудшает тестируемость

```Java
@Component("someBean")
public class SomeBean implements InitializingBean, DisposableBean {


	@Autowired
	private InjectedBean innerBean;

}
```

  

**Setter Injection**

Would be used in cases when there are some circular dependencies

```Java
@Component("someBean")
public class SomeBean implements InitializingBean, DisposableBean {

	private String text;

	public void setInnerBean(InjectedBean innerBean) {
		this.innerBean = innerBean;
	}

}
```

**Constructor Injection**

Recommended type

```Java
@Component("someBean")
public class SomeBean implements InitializingBean, DisposableBean {

	private InjectedBean innerBean;

	public SomeBean(InjectedBean innerBean) {
		this.innerBean = innerBean;
	}
}
```

If there are more then one constructor, one of them should be marked @Autowired to work with Spring

**Method Injection**

If you need Singleton bean to return prototype object. Spring  dynamically redefines method to return a bean from context

```Java
@Component
public abstract class SingletonBean {

	public SingletonBean(){
		System.out.println("Singleton Bean Instantiated !!");
	}

	@Lookup
	public abstract PrototypeBean getPrototypeBean();
	
}
```

**Injecting the prototype into a singleton.**

When we inject the prototype into a singleton, this object would be created only once. To change this behavior prototype bean’s proxy mode may be changed or the @Lookup method created.

## **Dependency Lookup**

Object knows whom it should ask for the dependencies

## ApplicationContext

Spring framework provides two implementations of IOC container in the form of [differencebetweenbeanfactoryvsa](differencebetweenbeanfactoryvsa)

- **BeanFactory** - The root interface for accessing a Spring bean container.
- **ApplicationContext** - main interface, extends BeanFactory. Provides more advanced features, such as Event Publishing,
    - **AnnotationConfigApplicationContext** - creates beans marked with annotations @Configuration, @Component. For Java and Annotation based
    - **XmlWebApplicationContext** - for XML based configurations web.xml files

Create container

```Java
ClassPathXmlApplicationContext context = 
		new ClassPathXmlApplicationContext("appContext.xml");

//get from Container
Pet pet = context.getBean(Pet.class);
```

### **BeanFactory**

Highest level interface of IOC Container in Spring. Defines  getBean, containsBean...

[![](https://lh5.googleusercontent.com/Pqq-gfIygbEXAgULgmdBl23r_47GImR_l3H-oYgUA1f-8uK4Xz_lEmtZ-vORN0Zq1g6pAHOsYtdyFGs3rzcTNzVd2mDRffgUyBiE3Gm78yw2P3sJeRJoIv-86tWWvdpSHTNywBcUjtpOqXoqrbkPgWuAa0pLuqslzYcnox8aPMjAt_9u3eyd-zc6OOas)](https://lh5.googleusercontent.com/Pqq-gfIygbEXAgULgmdBl23r_47GImR_l3H-oYgUA1f-8uK4Xz_lEmtZ-vORN0Zq1g6pAHOsYtdyFGs3rzcTNzVd2mDRffgUyBiE3Gm78yw2P3sJeRJoIv-86tWWvdpSHTNywBcUjtpOqXoqrbkPgWuAa0pLuqslzYcnox8aPMjAt_9u3eyd-zc6OOas)

### Context usage

- **XML**
- **Annotations**
- **Java code** - класс с конфигурацией. На него при помощи CGLib накручивается proxy и каждый раз, когда нужен бин, происходит делегация в тот метод, который мы пропишем в Java-сonfig.

### **Context Initialization**

1. Parse Config and create BeanDefinitions
2. Настройка созданных BeanDefinition - у нас есть возможность повлиять на то, какими будут наши бины еще до их фактического создания, иначе говоря мы имеем доступ к метаданным класса. Для этого существует специальный интерфейс BeanFactoryPostProcessor. For ex @Value("${host}") -> @Value("127.0.0.1")
3. Create custom FactoryBean
4. Create Bean Instances
5. Setup created beans

### Context Closing

- Standalone Non-Web Applications
    - Register Shutdown hook by calling ConfigurableApplicationContext\#registerShutdownHook
    - Call ConfigurableApplicationContext\#close
- Web Application
    - ContextLoaderListener will automatically close context when web container will stop the web application
- SpringBoot
    - Application Context will be automatically closed
    - Shutdown hook will be automatically registered
    - ContextLoaderListener applies to Spring Boot Web Applications as well

### Factory bean

There are two kinds of beans in the Spring bean container: ordinary beans and factory beans. Spring uses the former directly, whereas latter can produce objects themselves, which are managed by the framework.

Let's look at the _FactoryBean_ interface first:

```Java
public interface FactoryBean {
    T getObject()throws Exception;
    Class<?> getObjectType();
		boolean isSingleton();
}
```

Let's discuss the three methods:

- _getObject()_ – returns an object produced by the factory, and this is the object that will be used by Spring container
- _getObjectType()_ – returns the type of object that this _FactoryBean_ produces
- _isSingleton()_ – denotes if the object produced by this _FactoryBean_ is a singleton

**BeanDefinitionMap -** Для еще не созданных бинов есть Map<BeanName, BeanDefinition>, а для созданных - Map<BeanName, Bean>

## Dependency Resolution

- ApplicationContext initialised by metadata from configurations
- Each BeanDefinitions are defined by beans name and dependencies, which are it’s fields or constructor parameters
- Each bean configuration is validated(Except of Lazy)
- Created Singleton Beans
- Other Beans created when they are required
    - When BeanDefinition is created, dependencies graph is also created
    - Spring instantiates beans as late as possible. Because of that after successful container creation, exception might appear when object is created.
- If there are no cyclic dependencies
    - Dependency bean is created, than required bean created. Dependency bean injected into required bean
- If there are cyclic dependencies, solutions:
    - Both beans created, then injected via setters. In this case constructor injection doesn’t work.
    - Code redesign
    - Lazy initialisation - proxy object is created instead of required bean. When someone reaches this object, real bean instead of proxy is created.
    - Using PostConstruct, set manually
    - Using ApplicationContextAware + InitializingBean, set manually

## **EJB(Enterprise java beans)**

POJO with special annotation . These classes are called beans.

- @MessageDriven
- @Entity — определены в спецификации JPA (Java Persistence API) и используются для хранения данных;
- Session Beans
    - @Stateless
    - @Stateful
    - @Singleton

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

### Bean Injection

- **@Autowired**
- **@Inject** - same as @Autowired but from Jakarta EE. With following execution paths(Same as Autowired)
    - Match by Type
    - Match by Qualifier
    - Match by Name
- **@Resource** - same as @Autowired но это стандартная аннотация, а та спринговая. With following execution paths
    - Match by Name
    - Match by Type
    - Match by Qualifier

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
          
        
    
    ```Java
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

```Java
<bean id="someCat" class="com.ethickeep.entities.Cat"/>
```

### Annotations

**@Qualifier**

Qualifies which bean should be injected

```Java
@Qualifier("dog")

public Person(@Qualifier("dog") Pet pet)
```

**@Primary**

Указание, что использовать данный бин, в случае, если есть несколько кандидатов

**@Value**

Указание конкретного значения для внедрения

```Java
@Value("${person.age}")
private Double age;
```

**@Profile**

Maps the bean to that particular profile

```Java
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

```Java
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

```Java
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

# **Spring resources**

Spring дает вам возможность доступа к ресурсам с помощью приятного небольшого синтаксиса. Интерфейс ресурса имеет несколько интересных методов:

- boolean exists();
- String getFilename();
- File getFile() throws IOException;
- InputStream getInputStream() throws IOException;

Использование

- Resource aClasspathTemplate = ctx.getResource("classpath:somePackage/application.properties");
- Resource aFileTemplate = ctx.getResource("file:///someDirectory/application.properties");
- Resource anHttpTemplate = ctx.getResource("[https://marcobehler.com/application.properties](https://marcobehler.com/application.properties)");
- Resource depends = ctx.getResource("myhost.com/resource/path/myTemplate.txt");
- Resource s3Resources = ctx.getResource("s3://myBucket/myFile.txt");

**@PropertySources**

Установка путей к свойствам

```Java
@PropertySources({
		@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties"),
		@PropertySource("file://myFolder/app-production.properties")
})
```