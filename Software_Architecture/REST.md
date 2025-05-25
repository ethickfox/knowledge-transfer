## **REST**

**Rest(Representational State Transfer)** -  это архитектурный стиль взаимодействия компонентов распределенного приложения в сети. И имеет следующие требования

- **Client–server architecture** - отделение интерфейса клиента и сервера, работающего с данными
- **Statelessness** - в период между запросами клиента никакая информация о состоянии клиента на сервере не хранится
- **Cacheability** ответы с сервера должны иметь явную метку - кэшированные/нет для предотвращения получения устаревшей информации
- **Uniform interface** - наличие унифицированного интерфейса для общения клиента и сервера, отвечающего следующим правилам:
    - **Resource identificatio** - в рест ресурсом является все, чему можно дать имя(пользователь,изображение и тд) Идентификатором является URI
    - **Resource manipulation through representations** - Представление в REST используется для выполнения действий над ресурсами. Представление ресурса представляет собой текущее или желаемое состояние ресурса.
    - **Self-descriptive messages**запрос и ответ должны хранить в себе всю необходимую информацию для их обработки
    - **HATEOAS** (hypermedia as the engine of application state) - s a constraint of the REST application architecture. HATEOAS keeps the REST style architecture unique from most other network application architectures. The term “hypermedia” refers to any content that contains links to other forms of media such as images, movies, and text. REST architectural style lets us use the hypermedia links in the API response contents. It allows the client to dynamically navigate to the appropriate resources by traversing the hypermedia links. Navigating hypermedia links is conceptually the same as browsing through web pages by clicking the relevant hyperlinks to achieve a final goal.
- **Layered System** - допускается разделить систему на иерархию слоев но с условием, что каждый компонент может видеть компоненты только непосредственно следующего слоя.
- **Code-On-Demand** - позволяется загрузка и выполнение кода или программы на стороне клиента.

Idempotent requests

They can be retried or executed multiple times without causing unintended side effects.

Safe requests

A safe method does not change the value that is returned, it reads – but it never writes.

![Untitled](Software_Architecture/Communication/_img/Untitled.jpeg)

## **Rest api versioning**

Versioning is effective communication around changes to your API, so consumers know what to expect from it. With APIs, something as simple as changing a property name from productId to productID can break things for consumers.

### **Why needed**

Breaking Changes

- Changing the request/response format (e.g. from XML to JSON)
- Changing a property name (e.g. from name to productName) or data type on a property (e.g. from an integer to a float)
- Adding a required field on the request (e.g. a new required header or property in a request body)
- Removing a property on the response (e.g. removing description from a product)
- Adding new validation

### **Change scope**

- **Leaf** A change to an isolated endpoint with no relationship to other endpoints
- **Branch** A change to a group of endpoints or a resource accessed through several endpoints
- **Trunk** An application-level change, warranting a version change on most or all endpoints
- **Root** A change affecting access to all API resources of all versions

### **Types of API Versioning**

- **URI Path -** **This strategy involves putting the version number in the path of the URI, and is often done with the prefix "v". (Trunk)**
- **Query Params** - This type of versioning adds a query param to the request that indicates the version. Very flexible in terms of requesting the version of the resource (Leaf)
- **Versioning through content negotiation** **GET /users/1234 HTTP/1.1**
    
    Accept: application/vnd.ploeh.user+json; version=2
    
- **Header** **- The header approach is one that provides more granularity in serving up the requested version of any given resource.**
    - Custom header
    - Accept header

### **Maturity model**

Categories are based on how much the web services are REST compliant. Richardson used three main factors to decide the maturity of a service. These factors are

- URI,
- HTTP Methods,
- HATEOAS (Hypermedia)

### **Level 0**

Level zero of maturity does not make use of any of URI, HTTP Methods, and HATEOAS capabilities. The services at zero maturity level have a single URI and use a single HTTP method (typically POST).

For example, most SOAP Web Services use a single URI to identify an endpoint, and HTTP POST to transfer SOAP-based payloads, effectively ignoring the rest of the HTTP verbs.

### **Level 1**

Level one of maturity makes use of URIs, but does not use the HTTP Methods, and HATEOAS.

### **Level 2**

Level two of maturity makes use of URIs and HTTP Methods, but does not use the HATEOAS.

### **Level 3**

Level three of maturity makes use of all three, i.e. URIs and HTTP, and HATEOAS.

  

### REST vs SOAP

The main _difference between SOAP and REST_ is that the former provides a standard of communication between client, server, and other parties and has restricted a set of rules and format, while REST leverages the ubiquity of HTTP protocol, in both client and servers, to allow them to communicate with each other regardless of their implementation. In short, getting data from a [top5restfulwebserviceswithspringcour](top5restfulwebserviceswithspringcour)requires less headache than getting data from a SOAP web service.

In SOAP, you need to understand lengthy WSDL documents to find out the right methods and the right way to call them.

SOAP provides the Messaging Protocol layer of a [web services protocol stack](https://en.wikipedia.org/wiki/Web_services_protocol_stack) for web services. It is an XML-based protocol consisting of three parts:

- an envelope, which defines the message structure and how to process it
- a set of encoding rules for expressing instances of application-defined datatypes
- a convention for representing procedure calls and responses

SOAP has three major characteristics:

1. _extensibility_ (security and [WS-Addressing](https://en.wikipedia.org/wiki/WS-Addressing) are among the extensions under development)
2. _neutrality_ (SOAP can operate over any protocol such as [HTTP](https://en.wikipedia.org/wiki/HTTP), [SMTP](https://en.wikipedia.org/wiki/SMTP), [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol), [UDP](https://en.wikipedia.org/wiki/SOAP-over-UDP))
3. _independence_ (SOAP allows for any [programming model](https://en.wikipedia.org/wiki/Programming_model))

In short, **RESTfull web services** are much simpler, flexible, and expressive than SOAP web services in Java.

**1. Short Form**

REST stands for REpresntational State Transfer (REST) while SOAP Stands for Simple Object Access Protocol (SOAP).

**2. Architecture style vs Protocol**

REST is an architectural style, on which RESTFul web services are built while SOAP is a standard devised to streamline communication between client and server in terms of format, structure, and method.

**3. Use of HTTP Protocol**

REST takes full advantage of the HTTP protocol, including methods e.g. [GET, POST, PUT, and DELETE](http://javarevisited.blogspot.sg/2012/03/get-post-method-in-http-and-https.html) to represent action like from an application which provides data related to books, GET request can be used to retrieve books, POST can be used to upload data of a new book, and DELETE can be used to remove a book from the library. On the other hand, SOAP uses XML messages to communicate with the server, so it supports only POST(mostly).

**4. Supported Format**

RESTful web service can return the response in various formats like JSON, XML, and HTML while using SOAP web service you tie your response with XML because the actual response is bundled inside a SOAP message which is always in XML format.

**5. Speed**

Processing a RESTful web service request is much faster than processing a SOAP message because you need to do less parsing. Because of this reason RESTful, web services are _**faster**_ than SOAP web services.

**6. Bandwidth**

SOAP messages consume more bandwidth than RESTFul messages for the same type of an operation because [howto](howto) is more verbose than [p](p), the standard way to send RESTFul messages, and SOAP has an additional header for every message while RESTFul services utilize HTTP header.

**7. Transport Independence**

Since SOAP messages are wrapped in a SOAP envelope they can be sent over to any transport mechanism like TCP, FTP, SMTP, or any other protocol. On the other hand, _RESTful Web services_ are heavily dependent upon HTTP protocol.They used HTTP commands in their operation and depends upon HTTP for transmitting content to the server. Though in the real-world, SOAP is mostly over HTTP so this advantage of transport independence is not really utilised.

**8. Resource Identification**

RESTful web services utilize URLs to identify the desired resources to be accessed while SOAP uses XML messages to identify the desired web procedure or resource to be invoked.

**9. Security**

Security in RESTful web services can be implemented using standard and traditional solutions for authorized access to certain web resources. While implementing security in SOAP-based web services you need additional infrastructure on the web to enable message or transport-level security concerns.

**10. Caching**

RESTful web service takes full advantage of the web caching mechanism because they are basically [URL-based](http://java67.blogspot.sg/2013/01/difference-between-url-uri-and-urn.html). On the other hand, SOAP web services totally ignore the web caching mechanism.

**11. Approach**

In REST-based web services, every entity is centered around resources, while in the case of SOAP web services, every entity is centered around interfaces and messages.

**12. An Example**

In the first paragraph, we have seen one example of requesting the same web service using both SOAP and RESTFul style, you can see that the REST web service is easy to understand, can be cached, and required little effort to understand as compared to SOAP.