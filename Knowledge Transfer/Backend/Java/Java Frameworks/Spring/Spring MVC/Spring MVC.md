---
Interview graded: true
Last edited time: 2023-08-04T15:19
Last recall: 2023-08-04
Needs Rework: false
Status: Not started
Topic:
  - "[[Spring]]"
---
# **Spring MVC**

  

Web framework that allows to create web applications and RESTful services

## **MVC в Spring**

MVC is popular as it isolates the application logic from the user interface layer and supports separation of concerns. Here the Controller receives all requests for the application and then works with the Model to prepare any data needed by the View. The View then uses the data prepared by the Controller to generate a final presentable response.

- Model – The lowest level of the pattern which is responsible for maintaining data. Однако данные полностью независимы от вашего HTML, это простые объекты Java (например, объекты пользователя), из которых состоит ваше приложение.
- View – This is responsible for displaying all or a portion of the data to the user.
- Controller – Software Code that controls the interactions between the Model and View.отвечает на HTTP-запрос и знает, как преобразовать HTTP-запрос в объекты Java, а также ваши объекты Java в ответ HTML

### Component Stereotypes

- **@Controller  
      
    **Реализация контроллера из MVC. стереотип @Component, который предназначен для классов с контроллерами используется с RequestMapping. Класс, который отвечает за обработку запросов.
    - **@RestController** - тот же контроллер, но оборачивает методы анотацией @ResponseBody. Он может возвращать либо ModelAndView либо Response Entity.
- **@Service  
      
    **Cтереотип @Component, который предназначен для бизнес логики. (В иерархии является соединителем репозитория и контроллера)
- **@Repository  
      
    **Cтереотип @Component, который предназначен для работы с бд. Содержит PersistenceExceptionTranslationPostProcessor, который превращает все sql ошибки в spring unified unchecked exceptions.
    - CrudRepository - основные CRUD операции.
    - PagingAndSortingRepository - добавляет пагинацию
    - JpaRepository - добавляет JPA-специфические методы,такие как flushing the persistence context and deleting records in a batch.

### Controller

Allows next parameters:

- `ServletRequest` / `HttpServletRequest`
- HttpSession
- WebRequest
- Locale
- `java.io.InputStream` / `java.io.Reader`
- `java.io.OutputStream` / `java.io.Writer`
- @PathVariabe
- @RequestParam
- @RequestHeader
- @RequestBody
- `java.util.Map` / `org.springframework.ui.Model` / `org.springframework.ui.ModelMap`
- `org.springframework.validation.Errors` / `org.springframework.validation.BindingResult`
- `org.springframework.web.bind.support.SessionStatus`

Allows next return types:

- ModelAndView/Model
- Map
- View
- String
- void
- If the method is annotated with `@ResponseBody`, the return type is written to the response HTTP body. The return value will be converted to the declared method argument type using `HttpMessageConverter`

**@RequestMapping**

The annotation is used to map web requests to Spring Controller methods.

```Java
@RequestMapping(
            path = "/value",                  //endpoint
            params = {"id", "second"},        //?id="123"&second="456"
            headers = {"api-type!=new"},      //accepts only requests where no api-type=new in header
            consumes = "application/json",    //accepts only requests where type application/json
            produces = "application/json",
            method = RequestMethod.GET)
    public String getValue(String id, String second) {
        return "Params: " + id + " " + second;
    }
```

Spring Framework 4.3 introduced [a few new](https://www.baeldung.com/spring-new-requestmapping-shortcuts) HTTP mapping annotations, all based on _@RequestMapping_:

- _@GetMapping - read_
- _@PostMapping - create_
- _@PutMapping - update_
- _@DeleteMapping - delete_
- _@PatchMapping -_ instead of sending the _entire_ document(PUT), we just send a representation of the changes we made: a patch document.

```Java
@GetMapping("/{id}")
public ResponseEntity<?> getBazz(@PathVariable String id){
    return new ResponseEntity<>(new Bazz(id, "Bazz"+id), HttpStatus.OK);
}

@PostMapping
public ResponseEntity<?> newBazz(@RequestParam("name") String name){
    return new ResponseEntity<>(new Bazz("5", name), HttpStatus.OK);
}

@PutMapping("/{id}")
public ResponseEntity<?> updateBazz(
  @PathVariable String id,
  @RequestParam("name") String name) {
    return new ResponseEntity<>(new Bazz(id, name), HttpStatus.OK);
}

@DeleteMapping("/{id}")
public ResponseEntity<?> deleteBazz(@PathVariable String id){
    return new ResponseEntity<>(new Bazz(id), HttpStatus.OK);
}
```

_**@PathVariable**_

Extracts part of uri

```Java
@RequestMapping(value = "/ex/foos/{id}", method = GET)
@ResponseBody
public String getFoosBySimplePathWithPathVariable(@PathVariable("id") long id) {
    return "Get a specific Foo with id=" + id;
}

//aslo regexes are available too
@RequestMapping(value = "/ex/bars/{numericId:[\\d]+}", method = GET)
@ResponseBody
public String getBarsBySimplePathWithPathVariable(
  @PathVariable long numericId) {
    return "Get a specific Bar with id=" + numericId;
}
```

**@RequestParam**

Извлекает параметры запроса из запроса

```Java
// http://localhost:8080/api/foos?id=abc
public String getFoos(@RequestParam String id)
```

**@RequestHeader**

Извлекает содержимое заголовка

```Java
String greeting(@RequestHeader("accept-language") String language)
```

**@ResponseStatus**

If we want to specify the **response status of a controller method**, we can mark that method with _@ResponseStatus._ It has two interchangeable arguments for the desired response status: _code,_ and _value._

```Java
@ResponseStatus(HttpStatus.BAD_REQUEST, reason = "Some parameters are invalid")
void onIllegalArgumentException(IllegalArgumentException exception) {}
```

We have three ways to use _@ResponseStatus_ to convert an _Exception_ to an HTTP response status:

- using _@ExceptionHandler_
- using _@ControllerAdvice_
- marking the _Exception_ class

```Java
@ResponseStatus(code = HttpStatus.BAD_REQUEST)
class CustomException extends RuntimeException {}
```

  

**@ModelAttribute**

Позволяет передать модель в вид

```Java
@ModelAttribute
public void addAttributes(Model model) {
	model.addAttribute("msg", "Welcome to the Netherlands!");
}
```

**Model**

Spring автоматически добавляет этот класс в качестве параметра в контроллер. В нем можно указать все данные, которые необходимо отобразить в представлении.

```Java
@GetMapping("/account/{userId}")

public String account(Model model, @PathVariable Integer userId) {
	model.addAttribute("user", userDao.findById(userId));
	return "templates/account"; // (3)
}
```

  

**Сontent negotiation**

Существует множество способов, с помощью которых клиент может указать Spring MVC, какой формат ответа ожидается при выполнении запроса.

- Указав заголовок Accept, такой как «Accept: application/json» или «Accept: application/xml». Это работает "из коробки"
- Добавив расширение URL к вашему пути запроса, например /health.json или /health.xml. Это требует настройки на стороне Spring MVC для работы.
- Добавив параметр запроса в путь запроса, например /health?Format = json. Это требует настройки на стороне Spring MVC для работы.

The code snippet below will not result in ambiguous mapping error because both methods return different content types:

```Java
@GetMapping(value = "foos/duplicate", produces = MediaType.APPLICATION_XML_VALUE)
public String duplicateXml() {
    return "<message>Duplicate</message>";
}
    
@GetMapping(value = "foos/duplicate", produces = MediaType.APPLICATION_JSON_VALUE)
public String duplicateJson() {
    return "{\"message\":\"Duplicate\"}";
}
```

**WebSocket**

  

**Rest отображение**

Сгенерировать json или xml можно с помощью соответствующей библиотеки. По умолчанию это Jackon. Нужно добавить аннотацию **@ResponseBody** к одному из маппингов или @RestController классу контроллеру.

```Java
@GetMapping("/health")
@ResponseBody // (1)
public HealthStatus health() {
	return new HealthStatus(); // (2)
}
```

  

@_**ExceptionHandler**_

Spring configuration will detect this annotation and register the method as an exception handler for the **argument exception class and its subclasses**.

```Java
@Controller
public class PageController {

  @RequestMapping(value = "/{id}", method = RequestMethod.GET)
  public ModelAndView getPage(@PathVariable("id") String id) throws Exception {
      throw new RecordNotException("Page not found for id : " + id);
  }

  @ExceptionHandler(RecordNotException.class)
  public ModelAndView handleException(RecordNotException ex) {
    ...
    return modelAndView;
  }
}
```

Handler methods that are annotated with this annotation are allowed to have very flexible signatures. They can accept arguments of different types. For example, an exception argument, request and/or response objects, session object, locale object and model object etc.

Similar to arguments, return types can be of different types. For example, _ModelAndView_ object, _Model_ object, _View_ object, view name as String etc. We can mark the method to `void` also if the method handles the response itself by writing the response content directly to `HttpServletResponse`.

We may combine the `ExceptionHandler` annotation with `@ResponseStatus` for a specific HTTP error status.

```Java
@ExceptionHandler({NullPointerException.class, ArrayIndexOutOfBoundsException.class, IOException.class})
@ResponseBody
public ErrorResponse handleException(AjaxException ex, HttpServletResponse response) {
    ...
}
```

_**@ControllerAdvice**_

If we want to centralize the exception-handling logic to one class that is capable of handling exceptions thrown from any handler class/ controller class – then we can use `@ControllerAdvice` annotation.

By default, the methods in an [_@ControllerAdvice_](https://howtodoinjava.com/spring-rest/exception-handling-example/) apply globally to all controllers. We can create a class and add _@ControllerAdvice_ annotation on top. Then add _@ExceptionHandler_ methods for each type of specific exception class in it.

Notice we extended the exception handler class with _**ResponseEntityExceptionHandler**_. It is a convenient base class for `@ControllerAdvice` classes that wish to provide centralized exception handling across all `@RequestMapping` methods through `@ExceptionHandler` methods.

```Java
@ControllerAdvice
public class CustomExceptionHandler extends ResponseEntityExceptionHandler
{
  @ExceptionHandler(Exception.class)
  public final ResponseEntity<ErrorResponse> handleAllExceptions(Exception ex, WebRequest request) {
    List<String> details = new ArrayList<>();
    details.add(ex.getLocalizedMessage());
    ErrorResponse error = new ErrorResponse(ApplicationConstants.SERVER_ERROR, details);
    return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
  }
  @ExceptionHandler(RecordNotFoundException.class)
  public final ResponseEntity<ErrorResponse> handleUserNotFoundException(RecordNotFoundException ex,                       WebRequest request) {
    List<String> details = new ArrayList<>();
    details.add(ex.getLocalizedMessage());
    ErrorResponse error = new ErrorResponse(ApplicationConstants.RECORD_NOT_FOUND, details);
    return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
  }
}
```

## Http Request processing

Web container like Tomcat is responsible for creating Servlet and Filter instances and invoking their various life-cycle methods like [[]], service(), destroy(). In the case of an HTTP request, HttpServlet handles that, and depending upon the HTTP request method various doXXX() method is invoked by container like doGet() to process GET request and doPost() to process POST request.

### **DispatcherServlet**

The DispatcherServlet is the main controller of the Spring MVC Application. All incoming web request passes through DispatcherServlet before processed by individual Spring controllers i.e classes annotated using @Controller annotation. A web application can define any number of _DispatcherServlet_ instances. Each instance will operate in its own namespace, loading its own application context with mappings, handlers, etc. Only the root application context is loaded by [_ContextLoaderListener_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/context/ContextLoaderListener.html), if any, will be shared. In most cases, applications have only a single _DispatcherServlet_ with the context-root URL`(/)`, that is, all requests coming to that domain will be handled by it.

Он реализует паттерн Front controller. https://en.m.wikipedia.org/wiki/Front_controller

![[/Untitled 57.png|Untitled 57.png]]

1. После получения HTTP-запроса DispatcherServlet обращается к интерфейсу HandlerMapping, который определяет, какой Контроллер должен быть вызван, после чего, отправляет запрос в нужный Контроллер
2. After receiving HTTP request, Application Server (Standalone or Embedded) searches for Servlet that can handle request, `DispatcherServlet` is selected based on Servlet Registration and url-pattern.
3. `DispatcherServlet` uses `HandlerMapping` classes to get request mapping information and `HandlerAdapter`.
4. `DispatcherServlet` uses `HandlerAdapter` to execute controller method that will handle request.
5. `DispatcherServlet` interprets results of method execution and renders `View` with help of `ViewResolver` classes

```XML
<web-app>
	<!-- The front controller of this Spring Web application, responsible 
	for handling all application requests -->
	<servlet>
	  <servlet-name>Spring MVC Dispatcher Servlet</servlet-name>
	  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
		   <param-name>contextConfigLocation</param-name>
		   <param-value>/WEB-INF/config/web-application-config.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
	   <servlet-name>example</servlet-name>
	   <url-pattern>*</url-pattern>
	</servlet-mapping>
</web-app>
```

### ViewResolver

The **InternalResourceViewResolver** is an implementation of the ViewResolver interface in the [[top10springm]] which resolves logical view names like "hello" to internal physical resources like Servlet and JSP files like jsp files placed under the WEB-INF folder.

It's also a  best practice to put all JSP files inside the WEB-INF directory, to hide them from direct access (e.g. via a manually entered URL). If we put them inside the WEB-INF directory then only controllers will be able to access them.

There are next types of view resolvers:

- InternalViewResolver - for HTML
- XmlViewResolver
- JSONViewResolver

## Client

**RestTemplate**

Spring REST Api Client