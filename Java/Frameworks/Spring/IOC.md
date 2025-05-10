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

```java
@Component("someBean")
public class SomeBean implements InitializingBean, DisposableBean {


	@Autowired
	private InjectedBean innerBean;

}
```

  

**Setter Injection**

Would be used in cases when there are some circular dependencies

```java
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

```java
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

```java
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
## **Dependency Lookup**

Object knows whom it should ask for the dependencies

## ApplicationContext

Spring framework provides two implementations of IOC container in the form of [differencebetweenbeanfactoryvsa](differencebetweenbeanfactoryvsa)

- **BeanFactory** - The root interface for accessing a Spring bean container.
- **ApplicationContext** - main interface, extends BeanFactory. Provides more advanced features, such as Event Publishing,
    - **AnnotationConfigApplicationContext** - creates beans marked with annotations @Configuration, @Component. For Java and Annotation based
    - **XmlWebApplicationContext** - for XML based configurations web.xml files

Create container

```java
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

