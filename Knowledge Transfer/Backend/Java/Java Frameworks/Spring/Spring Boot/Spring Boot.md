---
Interview graded: true
Last edited time: 2023-08-21T16:39
Last recall: 2023-07-22
Needs Rework: false
Status: Not started
Topic:
  - "[[Spring]]"
---
# **Spring Boot**

Framework that simplifies creation of Spring application. While Spring Framework focuses on providing flexibility, Spring Boot seeks to reduce code length and simplify web application development. By leveraging annotation and boilerplate configuration, Spring Boot reduces the time it takes to develop applications. This capability helps you create standalone applications with less or almost no configuration overhead.

Spring Boot supports embedded containers:

- _Tomcat (default)_
- _Jetty_
- _Undertow_

![[/Untitled 115.png|Untitled 115.png]]

Below are some key points which spring boot offers but spring doesn’t:

- Starter POM.
- Version Management.
- Auto Configuration.
- Component Scanning.
- Embedded server.
- InMemory DB.
- Actuators

Features:

- **Ease of Dependency Management** - To speed up the dependency management process, Spring Boot implicitly packages the required third-party dependencies for each type of Spring-based application and provides them through so-called starter packages (spring-boot-starter-web, spring-boot-starter-data-jpa, etc.). Starter packages are collections of handy dependency descriptors that you can include in your application.
- **Automatic Сonfiguration** Spring Boot will try to automatically configure your Spring application based on the jar dependencies you added. For example, if you add Spring-boot-starter-web, Spring Boot will automatically configure registered beans such as DispatcherServlet, ResourceHandlers, and MessageSource. If you are using spring-boot-starter-jdbc, Spring Boot automatically registers the DataSource, EntityManagerFactory, and TransactionManager beans and reads the database connection information from the application.properties file. If you are not going to use a database and do not provide any details about connecting manually, Spring Boot will automatically configure the database in the memory without any additional configuration on your part (if you have H2 or HSQL libraries).
- **Native Support for Application Server** includes an embedded web server. Developers no longer have to worry about setting up a servlet container and deploying an application to it
- Fast and easy development of Spring-based applications;
- No need for the deployment of war files;
- The ability to create standalone applications;
- Helping to directly embed Tomcat, Jetty, or Undertow into an application;
- No need for XML configuration;
- Reduced amounts of source code;
- Additional out-of-the-box functionality

**Disadvantages**

- Lack of control
- The complex and time-consuming process of converting a legacy or an existing Spring project to a Spring Boot application;
- Not suitable for large-scale projects. Although it’s great for working with microservices, many developers claim that Spring Boot is not suitable for building monolithic applications.
- Possible problems with dependencies
- Spring boot may unnecessarily increase the deployment binary size with unused dependencies.

### **@SpringBootApplication**

Аннотация приложения, которая содержит

- @SpringBootConfiguration - stereotype of [_@Configuration_](https://www.baeldung.com/spring-bean-annotations) annotation. enable registration of extra beans in the context or the import of additional configuration classes. The main difference is that _@SpringBootConfiguration_ allows configuration to be automatically located. This can be especially useful for unit or integration tests.
- @EnableAutoConfiguration - Сканирует пакеты и добавляет бины, созданные нами и jar из classpath
- @ComponentScan - Scans packages and adds beans

## **Starter**

Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop shop for all the Spring and related technologies that you need without having to hunt through sample code and copy-paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for database access, include the `spring-boot-starter-data-jpa` dependency in your project.

The starters contain a lot of the dependencies that you need to get a project up and running quickly and with a consistent, supported set of managed transitive dependencies.

All **official** starters follow a similar naming pattern; `spring-boot-starter-*`, where `*` is a particular type of application.

Spring Boot also includes the following starters that can be used if you want to exclude or swap specific technical facets

|   |   |
|---|---|
|Name|Description|
|`spring-boot-starter-jetty`|Starter for using Jetty as the embedded servlet container. An alternative to [`spring-boot-starter-tomcat`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#spring-boot-starter-tomcat)|
|`spring-boot-starter-log4j2`|Starter for using Log4j2 for logging. An alternative to [`spring-boot-starter-logging`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#spring-boot-starter-logging)|
|`spring-boot-starter-logging`|Starter for logging using Logback. Default logging starter|
|`spring-boot-starter-reactor-netty`|Starter for using Reactor Netty as the embedded reactive HTTP server.|
|`spring-boot-starter-tomcat`|Starter for using Tomcat as the embedded servlet container. Default servlet container starter used by [`spring-boot-starter-web`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#spring-boot-starter-web)|
|`spring-boot-starter-undertow`|Starter for using Undertow as the embedded servlet container. An alternative to [`spring-boot-starter-tomcat`](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#spring-boot-starter-tomcat)|

@**ConditionalOn…**

Anywhere we define a Spring bean, we can optionally add a condition. Only if this condition is satisfied will the bean be added to the application context.

- @**ConditionalOnProperty**
- @**ConditionalOnExpression**
- @**ConditionalOnBean**
- @**ConditionalOnMissingBean**
- @**ConditionalOnResource**

## Client

### **WebClient**

WebClient is an interface representing the main entry point for performing web requests. This solution offers support for both synchronous and asynchronous operations, making it suitable also for applications running on a Servlet Stack.

It was created as part of the Spring Web Reactive module and will be replacing the classic RestTemplate in these scenarios. In addition, the new client is a reactive, non-blocking solution that works over the HTTP/1.1 protocol.

The _WebClient_ is part of the [Spring WebFlux](https://www.baeldung.com/spring-webflux) library. It's **a non-blocking solution provided by the Spring Reactive Framework to address the performance bottlenecks of synchronous implementations like Feign clients**.

```Java
@GetMapping(value = "/products-non-blocking", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<Product> getProductsNonBlocking() {
    log.info("Starting NON-BLOCKING Controller!");

    Flux<Product> productFlux = WebClient.create()
      .get()
      .uri(getSlowServiceBaseUri() + SLOW_SERVICE_PRODUCTS_ENDPOINT_NAME)
      .retrieve()
      .bodyToFlux(Product.class);

    productFlux.subscribe(product -> log.info(product.toString()));

    log.info("Exiting NON-BLOCKING Controller!");
    return productFlux;
}
```

### _**@FeignClient**_

The Feign client is a declarative REST client that makes writing web clients easier. When using Feign, the developer has only to define the interfaces and annotate them accordingly. The actual web client implementation is then provided by Spring at runtime. Behind the scenes, **interfaces annotated with** _**@FeignClient**_ **generate a synchronous implementation based on a thread-per-request model**. So, for each request, the assigned thread blocks until it receives the response. The disadvantage of keeping multiple threads alive is that each open thread occupies memory and CPU cycles.

```Java
@FeignClient(value = "productsBlocking", url = "http://localhost:8080")
public interface ProductsFeignClient {

    @RequestMapping(method = RequestMethod.GET, value = "/slow-service-products", produces = "application/json")
    List<Product> getProductsBlocking(URI baseUrl);
}

@GetMapping("/products-blocking")
public List<Product> getProductsBlocking() {
    log.info("Starting BLOCKING Controller!");
    final URI uri = URI.create(getSlowServiceBaseUri());

    List<Product> result = productsFeignClient.getProductsBlocking(uri);
    result.forEach(product -> log.info(product.toString()));

    log.info("Exiting BLOCKING Controller!");
    return result;
}
```

  

# **Actuator**

Starter for using Spring Boot’s Actuator which provides production ready features to help you monitor and manage your application

[![](https://lh3.googleusercontent.com/yXNPbefXPyYCAdtEq6XP5um7lYlRyOmQEpWRTSkvON8Sgdg35A-qNOm3uUPUpH6c_Kdy8jPaJdBXk6fRqw74Lwrg4r51lFIzljhpvXu_4BJiqyIszpVZ2dojkuoR1rheCeMXfvatu-oyYcr-F0KqsEpmL3rN5LuithLLyWyBrgd_K4bJLmRHLpunH0uZ)](https://lh3.googleusercontent.com/yXNPbefXPyYCAdtEq6XP5um7lYlRyOmQEpWRTSkvON8Sgdg35A-qNOm3uUPUpH6c_Kdy8jPaJdBXk6fRqw74Lwrg4r51lFIzljhpvXu_4BJiqyIszpVZ2dojkuoR1rheCeMXfvatu-oyYcr-F0KqsEpmL3rN5LuithLLyWyBrgd_K4bJLmRHLpunH0uZ)

## **Managing Dependencies**

### **Dependency Management Plugin**

When you apply the [`io.spring.dependency-management`](https://github.com/spring-gradle-plugins/dependency-management-plugin) plugin, Spring Boot’s plugin will automatically [import the](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#reacting-to-other-plugins.dependency-management) [`spring-boot-dependencies`](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#reacting-to-other-plugins.dependency-management) [bom](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/htmlsingle/#reacting-to-other-plugins.dependency-management) from the version of Spring Boot that you are using. This provides a similar dependency management experience to the one that’s enjoyed by Maven users. For example, it allows you to omit version numbers when declaring dependencies that are managed in the bom. To make use of this functionality, declare dependencies in the usual way but omit the version number

**Workarounds and Fixes**

- Explicitly manage the Reactor dependency in the app:
    
    ```XML
    <dependencyManagement>
    		<dependencies>
    			<dependency>
    				<groupId>io.projectreactor</groupId>
    				<artifactId>reactor-core</artifactId>
    				<version>2.5.0.BUILD-SNAPSHOT</version>
    			</dependency>
    		</dependencies>
    	</dependencyManagement>
    ```
    
    This is ugly and lots of lines of XML. Note that it doesn't help to use a BOM that has this code in it with Maven 3.3 (but does with Maven 3.4, see below).
    
    ```XML
    $ cd app-1
    $ ../mvn dependency:tree
    ...
    [INFO] com.example:example-app-1:jar:0.0.1-SNAPSHOT
    [INFO] \- com.example:example-library:jar:0.0.1-SNAPSHOT:compile
    [INFO]    \- io.projectreactor:reactor-core:jar:2.5.0.BUILD-SNAPSHOT:compile
    [INFO]       \- org.reactivestreams:reactive-streams:jar:1.0.0:compile
    ...
    ```
    
      
    
    ## Spring Observability
    
    ## Spring Boot CLI