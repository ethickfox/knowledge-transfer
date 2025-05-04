---
Interview graded: true
Last edited time: 2023-07-16T20:26
Needs Rework: false
Status: Not started
Topic:
  - "[Test Automation](Test%20Automation)"
---
## **Mockito**

Фреймворк для работы с заглушками. Для создания mock объекта можно использовать аннотацию @Mock либо Mockito.mock(DataService.class)

**Управление поведением**

Когда на mock объект как-то воздействуют, он должен отреагировать определенным образом.

- Основная реализация использует when, этот метод принимает вызов переопределенного метода и возвращает объект типа OngoingStubbing.позволяющий использовать методы семейства then.

Mockito.when(dataService.getAllData()).thenReturn(data);

- Альтернативная - Mockito.do. Эти методы позволяют задать поведение начиная с результата вызова и возвращают объект класса Stubber, уже при помощи которого можно задать условие

Mockito.doReturn(data).when(dataService).getData(). Данный метод не гарантирует правильность вызываемого метода на стадии компиляции

- Проверить что тестируемый **метод вызывается нужное количество раз**  - verify. Можно указать ожидаемый результат, количество вызовов и метод, который должен вызываться

### **Stub**

Переопределенный класс, в котором уже сразу зашит определенный ответ

### **Mock**

Объекты,которые необходимо настроить перед их использованием, то есть сказать, при выполнении каких методов какой будет результат. Изначально написанные методы тут не вызываются. По умолчанию не переопределенные методы возвращают 0 или null, а для коллекция - пустую коллекцию

### **MockBean**

### **Spy**

Объект-обёртка над каким-то другим реальным объектом, который переопределяет методы, от которых нужна особая функциональность, но те методы, которые не переопределены все равно отрабатывают, как в обычном объекте. При создании spy на основе класса, если его тип — интерфейс, будет создан обычный mock-объект, а если тип — класс, то Mockito попытается создать экземпляр при помощи конструктора по умолчанию (без параметров). Вызов метода spy на состояние изначального экземпляра не повлияет

### **Captor**

Класс, который позволяет захватить аргумент, который был передан в метод и проанализировать его

### **Коэффициент покрытия тестами**

Величина, показывающая нам процент выполненного исходного кода, во время тестирования.

В Idea есть плагин для проверки коэффициента покрытия тестами, он может работать с разными библиотеками. По умолчанию там библиотека IntelliJ IDEA Code Coverage Agent, но можно использовать Jacoco

**Виды**

**Hamcrest matchers**

### **PowerMockito**

PowerMockito is PowerMock's extension API to support Mockito. It provides capabilities to work with the Java Reflection API in a simple way to overcome the problems of Mockito, such as the lack of ability to mock final, static or private methods.

### MockMVC

MockMVc is built on Servlet API mock implementations from the `spring-test`module and does not rely on a running container. Therefore, there are some differences when compared to full end-to-end integration tests with an actual client and a live server running.

The easiest way to think about this is by starting with a blank `MockHttpServletRequest`. Whatever you add to it is what the request becomes. Things that may catch you by surprise are that there is no context path by default; no `jsessionid` cookie; no forwarding, error, or async dispatches; and, therefore, no actual JSP rendering. Instead, “forwarded” and “redirected” URLs are saved in the `MockHttpServletResponse` and can be asserted with expectations.

This means that, if you use JSPs, you can verify the JSP page to which the request was forwarded, but no HTML is rendered. In other words, the JSP is not invoked. Note, however, that all other rendering technologies that do not rely on forwarding, such as Thymeleaf and Freemarker, render HTML to the response body as expected. The same is true for rendering JSON, XML, and other formats through `@ResponseBody` methods.

Alternatively, you may consider the full end-to-end integration testing support from Spring Boot with `@SpringBootTest`. See the [Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing).

There are pros and cons for each approach. The options provided in Spring MVC Test are different stops on the scale from classic unit testing to full integration testing. To be certain, none of the options in Spring MVC Test fall under the category of classic unit testing, but they are a little closer to it. For example, you can isolate the web layer by injecting mocked services into controllers, in which case you are testing the web layer only through the `DispatcherServlet` but with actual Spring configuration, as you might test the data access layer in isolation from the layers above it. Also, you can use the stand-alone setup, focusing on one controller at a time and manually providing the configuration required to make it work.

Another important distinction when using Spring MVC Test is that, conceptually, such tests are the server-side, so you can check what handler was used, if an exception was handled with a HandlerExceptionResolver, what the content of the model is, what binding errors there were, and other details. That means that it is easier to write expectations, since the server is not an opaque box, as it is when testing it through an actual HTTP client. This is generally an advantage of classic unit testing: It is easier to write, reason about, and debug but does not replace the need for full integration tests. At the same time, it is important not to lose sight of the fact that the response is the most important thing to check. In short, there is room here for multiple styles and strategies of testing even within the same project.