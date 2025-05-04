---
Interview graded: true
Last edited time: 2023-08-08T09:12
Last recall: 2023-08-03
Needs Rework: false
Status: Not started
Topic:
  - "[[../Spring]]"
---
# **Spring AOP**

Идея АОП заключается в выделении сквозной функциональности в аспекты. Парадигма программирования, основанная на идее разделения основного и служебного функционала. Служебный функционал записывается в Aspect классы.

Spring AOP aims to provide a simple AOP implementation across Spring IoC to solve the most common problems that programmers face. **It is not intended as a complete AOP solution** – it can only be applied to beans that are managed by a Spring container.

Включение АОП - **@EnableAspectJAutoProxy**

## **Сущности**

**Сквозные задачи** - это задачи распределенные по различным модулям программы, запутывающие тем самым код

[![](https://lh5.googleusercontent.com/nAyKfui3cu-tGssl6-WWcnnJbkYKTLbuv5LspG353yhqfH-idgdhXC0KtO6TZQjb5tBeSsVp0BgxeVbkMXccqsraksuOuhroLWPDZq1BlJ1qnyOvnJsy_QqB8FDCZaVNuBkgp5g0Q1-rzzZRr6sbsFiuzdRUO_JPzaS0q8XMuWfYlLxNtySYPw5Xybjj)](https://lh5.googleusercontent.com/nAyKfui3cu-tGssl6-WWcnnJbkYKTLbuv5LspG353yhqfH-idgdhXC0KtO6TZQjb5tBeSsVp0BgxeVbkMXccqsraksuOuhroLWPDZq1BlJ1qnyOvnJsy_QqB8FDCZaVNuBkgp5g0Q1-rzzZRr6sbsFiuzdRUO_JPzaS0q8XMuWfYlLxNtySYPw5Xybjj)

**Аспект** - класс отвечающий за сквозную функциональность(cross-cutting logic) Например:

- Логирование
- Проверка прав
- Обработка транзакция
- Обработка исключений

**Pointcut** - выражение, описывающее, где должен быть вызван Advice(AspectJ Excpression Language)

**Pointcut designator** - execution(..)

**Joinpoint** - точка, когда применяется Advice

**Advice** - метод, который находится в Аспекте и содержит сквозную логику

```Java
@Component
@Aspect
public class LoggingAspect { //Aspect

	@Before("execution(public String getBook())") //Pointcut
	public String log(){     //Advice
		...
	}
}

public class Library {

	public String getBook(){    //Joinpoint
		...
	}
}
```

### Pointcut

Syntax:

```Java
execution(modifier return-type declaring-type method-name(parameters) throws)

execution(public String com.ethickeep.Library.getBook(String))

execution( * get*(..) || * set*(..)) - wildcard и комбинирование
```

- Before — перед вызовом метода
    
    ```Java
    @Before("execution(public String getBook())")
    ```
    
- After returning — после выполнения метода, но до возврата результата
    
    ```Java
    @AfterReturnung(pointcut = "execution(public String getBooks())", returning = “books”)
    public void advice(List<Book> books)
    ```
    
- After throwing — после выполнения метода,в случае exception
    
    ```Java
    @AfterThrowing(pointcut = "execution(public String getBook())", throwing = “ex”)
    public void advice(Throwable ex)
    ```
    
- After/After finally — после завершения метода, не получить доступ к результату и исключению
    
    ```Java
    @After(pointcut = "execution(public String getBook())")
    public void advice()
    ```
    
- Around — оборачивает метод(можно перед, после, вместо). Нужно вызывать метод вручную
    
    ```Java
    @Around(pointcut = "execution(public String getBook())")
    public Object advice(ProceedingJoinPoint jp){  //jp - getBook method
    	Object res = jp.proceed();
    	return res;
    }
    ```
    

[![](https://lh3.googleusercontent.com/yiUTJaXmEecVAkwzxo8vVnX_oQIdxK5EgFwc7gmbqsN0K-wV63nBWHX2RQuNX562p0hwPqAC6t6lBlr9WT9dMBXE6PLUJZNKLk__MtrY3_vyzgE7uEk3bFxCG_P6urxHW2Yu51ce4vqqa9m3baJ2YeTou-iJi1HViF7ozHhd4uEI_L-sImlDI-2D3HtZ)](https://lh3.googleusercontent.com/yiUTJaXmEecVAkwzxo8vVnX_oQIdxK5EgFwc7gmbqsN0K-wV63nBWHX2RQuNX562p0hwPqAC6t6lBlr9WT9dMBXE6PLUJZNKLk__MtrY3_vyzgE7uEk3bFxCG_P6urxHW2Yu51ce4vqqa9m3baJ2YeTou-iJi1HViF7ozHhd4uEI_L-sImlDI-2D3HtZ)

## Proxy

Cтруктурный паттерн проектирования, который позволяет подставлять вместо реальных объектов специальные объекты-заменители. Эти объекты перехватывают вызовы к оригинальному объекту, позволяя сделать что-то до или после передачи вызова оригиналу.

Если Spring аспекту нужно сделать proxy на какой-то объект, то он вначале смотрит на его интерфейсы, и если они есть, то использует Dynamic proxy(Internal Java Implementation), а если их нету, то CGLib. (С 3.2 версии CGLib идет вместе с Spring, поведение можно изменить, явно задав флаг proxy-target-class)

Spring может реализовывать АОП с помощью прокси либо через AspectJ, он в свою очередь меняет байткод, а не добавляет реализацию на лету.

**Only public interface method calls for JDK Proxy and public methods for CGLIB**.

### **DynamicProxy**

Встроенная реализация JDK которая позволяет осуществлять перехватывать вызов метода и добавлять функциональность. Для работы необходимо, чтобы целевой класс наследовался от интерфейса с данным методом. И реализовать свой InvocationHandler с методом invoke. И передать в него объект целевого класса и создать новый целевой объект через Proxy.newProxyInstance. Used when proxying interfaces

### **CGLib**

Реализует проксирование через создание классов наследников проксируемого объекта. Используем класс Enhancer. Тут через setSuperclass мы указываем проксируемый объект и через setCallback устанавливаем обертку вызова метода. Тк обе эти библиотеки используют наследование, не поддерживаются final классы и методы. В нем еще есть миксины(технология, позволяющая комбинировать множество объектов в один) Used when proxying classes

![[Untitled 116.png|Untitled 116.png]]

### **AspectJ**

**AspectJ is the original AOP technology which aims to provide complete AOP solution.** It is more robust but also significantly more complicated than Spring AOP. It's also worth noting that AspectJ can be applied across all domain objects.

|   |   |
|---|---|
|Spring AOP|AspectJ|
|Implemented in pure Java|Implemented using extensions of Java programming language|
|No need for separate compilation process|Needs AspectJ compiler (ajc) unless LTW is set up|
|Only runtime weaving is available|Runtime weaving is not available. Supports compile-time, post-compile, and load-time Weaving|
|Less Powerful – only supports method level weaving|More Powerful – can weave fields, methods, constructors, static initializers, final class/methods, etc…|
|Can only be implemented on beans managed by Spring container|Can be implemented on all domain objects|
|Supports only method execution pointcuts|Support all pointcuts|
|Proxies are created of targeted objects, and aspects are applied on these proxies|Aspects are weaved directly into code before application is executed (before runtime)|
|Much slower than AspectJ|Better Performance|
|Easy to learn and apply|Comparatively more complicated than Spring AOP|

### **Weaving**

Both AspectJ and Spring AOP uses the different type of weaving which affects their behavior regarding performance and ease of use

- AspectJ
    - **Compile-time weaving**: The AspectJ compiler takes as input both the source code of our aspect and our application and produces a woven class files as output
    - **Post-compile weaving**: This is also known as binary weaving. It is used to weave existing class files and JAR files with our aspects
    - **Load-time weaving**: This is exactly like the former binary weaving, with a difference that weaving is postponed until a class loader loads the class files to the JVM
- Spring AOP makes use of
    - **Runtime weaving** - the aspects are woven during the execution of the application using proxies of the targeted object