---
Interview graded: true
Last edited time: 2023-12-04T15:28
Needs Rework: false
Status: Not started
Topic:
  - "[[Web Services Architecture]]"
---
![[Untitled 41.png|Untitled 41.png]]

# **Microservices**

Microservices is an architectural style that structures an application as a collection of services that are loosely coupled, highly maintainable, pluggable, and owned by small teams based on services.

Set of software applications designed for a limited scope that work with each other to form a bigger solution. Each service has minimal capabilities for the sake of creating a very modularized overall architecture.

**Микросервисы**

Небольшие автономные сервисы, которые работают совместно

Хотя для микросервисов нет единого принятого определения, имеется несколько важных характеристик:

- REST — построен вокруг RESTful ресурсов. Связь между сервисами может быть на основе событий или протокола HTTP.
- Небольшие хорошо выбранные развертываемые блоки — ограниченный контекст
- Облачные возможности — динамическое масштабирование

[![](https://lh4.googleusercontent.com/sytCkQDZlwYJXX-dLCm-t3d8RhBuV7OIQbHunO560AuFyU-ckv4Vs15UU374OXUqUqm-4feXmdYKQK7ri4NXlRZ6PO4RAJIdCXkONkaBgBlCRx5BjifmXdWhHc_H1Cpj-4Kn4hiL6kJleA0Z8E3519gwUHkuKEmy6djKspBw-J1WpF4FaFcgR_CGvmhR)](https://lh4.googleusercontent.com/sytCkQDZlwYJXX-dLCm-t3d8RhBuV7OIQbHunO560AuFyU-ckv4Vs15UU374OXUqUqm-4feXmdYKQK7ri4NXlRZ6PO4RAJIdCXkONkaBgBlCRx5BjifmXdWhHc_H1Cpj-4Kn4hiL6kJleA0Z8E3519gwUHkuKEmy6djKspBw-J1WpF4FaFcgR_CGvmhR)

**Преимущества:**

- Использование новых технологий и процессов адаптация становится проще. Вы можете попробовать новые технологии с новыми микросервисами, которые мы создим.
- Faster and easier deployment
- Easier to scale
- Better fault tolerance
- Ability to mix different technologies and languages

**Benefits**

This solution has a number of benefits:

- Enables the continuous delivery and deployment of large, complex applications.
    - Improved maintainability - each service is relatively small and so is easier to understand and change
    - Better testability - services are smaller and faster to test
    - Better deployability - services can be deployed independently
    - It enables you to organize the development effort around multiple, autonomous teams. Each (so called two pizza) team owns and is responsible for one or more services. Each team can develop, test, deploy and scale their services independently of all of the other teams.
- Each microservice is relatively small:
    - Easier for a developer to understand
    - The IDE is faster making developers more productive
    - The application starts faster, which makes developers more productive, and speeds up deployments
- Improved fault isolation. For example, if there is a memory leak in one service then only that service will be affected. The other services will continue to handle requests. In comparison, one misbehaving component of a monolithic architecture can bring down the entire system.
- Eliminates any long-term commitment to a technology stack. When developing a new service you can pick a new technology stack. Similarly, when making major changes to an existing service you can rewrite it using a new technology stack.

**Drawbacks**

This solution has a number of drawbacks:

- Developers must deal with the additional complexity of creating a distributed system:
    - Developers must implement the inter-service communication mechanism and deal with partial failure
    - Implementing requests that span multiple services is more difficult
    - Testing the interactions between services is more difficult
    - Implementing requests that span multiple services requires careful coordination between the teams
    - Developer tools/IDEs are oriented on building monolithic applications and don’t provide explicit support for developing distributed applications.
- Deployment complexity. In production, there is also the operational complexity of deploying and managing a system comprised of many different services.
- Increased memory consumption. The microservice architecture replaces N monolithic application instances with NxM services instances. If each service runs in its own JVM (or equivalent), which is usually necessary to isolate the instances, then there is the overhead of M times as many JVM runtimes. Moreover, if each service runs on its own VM (e.g. EC2 instance), as is the case at Netflix, the overhead is even higher.
- Higher latency because of calls to remote services

**Проблемы микросервисных архитектур**

- Автоматизация: поскольку вместо монолита есть ряд более мелких компонентов, вам необходимо автоматизировать все — сборки, развертывание, мониторинг и т. Д.
- **Видимость**: теперь у вас есть несколько небольших компонентов для развертывания и обслуживания. Может быть, 100 или 1000 компонентов. Вы должны быть в состоянии отслеживать и определять проблемы автоматически. Вам нужна отличная видимость вокруг всех компонентов.
- **Ограниченный контекст**: определение границ микросервиса — непростая задача. Ограниченный контекст из доменного дизайна — хорошая отправная точка. Ваше понимание домена развивается в течение определенного периода времени. Вы должны убедиться, что границы микросервиса развиваются.
- **Управление конфигурациями:** вам необходимо поддерживать конфигурации для сотен компонентов в разных средах. Вам потребуется решение для управления конфигурацией
- **Колода карт**: Если микросервис в нижней части цепочки вызовов выходит из строя, он может повлиять на все остальные микросервисы. Микросервисы должны быть отказоустойчивыми.
- **Отладка:** Когда возникает проблема, требующая изучения, вам может понадобиться изучить несколько служб в разных компонентах. Централизованное ведение журнала и инструментальные панели необходимы для облегчения отладки проблем.
- **Согласованность:** у вас не может быть широкого спектра инструментов, решающих одну и ту же проблему. Хотя важно стимулировать инновации, важно также иметь децентрализованное управление языками, платформами, технологиями и инструментами, используемыми для реализации / развертывания / мониторинга микросервисов.

**Балансировки нагрузки**

- На стороне сервера и на стороне клиента.
- Ribbon обеспечивает балансировку нагрузки на стороне клиента, указанные вами — балансировку на стороне сервера.

## **Microservices patterns**

- **Database per server**
- **Shared database**
- **Domain event** - publish an event whenever data changes
- **Messaging** - use asynchronous messaging for inter-service communication
- **API gateway** - a service that provides each client with unified interface to services
- **Backend for front-end** - a separate API gateway for each kind of client
- **Distributed tracing** - instrument services with code that assigns each external request an unique identifier that is passed between services. Record information (e.g. start time, end time) about the work (e.g. service requests) performed when handling the external request in a centralized service
- **Exception tracking** report all exceptions to a centralized exception tracking service that aggregates and tracks exceptions and notifies developers.
- **Health check API** service API (e.g. HTTP endpoint) that returns the health of the service and is intended to be pinged, for example, by a monitoring service

**When to use the microservice architecture?**

One challenge with using this approach is deciding when it makes sense to use it. When developing the first version of an application, you often do not have the problems that this approach solves. Moreover, using an elaborate, distributed architecture will slow down development. This can be a major problem for startups whose biggest challenge is often how to rapidly evolve the business model and accompanying application. Using Y-axis splits might make it much more difficult to iterate rapidly. Later on, however, when the challenge is how to scale and you need to use functional decomposition, the tangled dependencies might make it difficult to decompose your monolithic application into a set of services.

**How to decompose the application into services?**

Another challenge is deciding how to partition the system into microservices. This is very much an art, but there are a number of strategies that can help:

Decompose by business capability and define services corresponding to business capabilities.

Decompose by domain-driven design subdomain.

Decompose by verb or use case and define services that are responsible for particular actions. e.g. a Shipping Service that’s responsible for shipping complete orders.

Decompose by by nouns or resources by defining a service that is responsible for all operations on entities/resources of a given type. e.g. an Account Service that is responsible for managing user accounts.

Ideally, each service should have only a small set of responsibilities. (Uncle) Bob Martin talks about designing classes using the Single Responsibility Principle (SRP). The SRP defines a responsibility of a class as a reason to change, and states that a class should only have one reason to change. It make sense to apply the SRP to service design as well.

Another analogy that helps with service design is the design of Unix utilities. Unix provides a large number of utilities such as grep, cat and find. Each utility does exactly one thing, often exceptionally well, and is intended to be combined with other utilities using a shell script to perform complex tasks.

**How to maintain data consistency?**

In order to ensure loose coupling, each service has its own database. Maintaining data consistency between services is a challenge because 2 phase-commit/distributed transactions is not an option for many applications. An application must instead use the Saga pattern. A service publishes an event when its data changes. Other services consume that event and update their data. There are several ways of reliably updating data and publishing events including Event Sourcing and Transaction Log Tailing.

**How to implement queries?**

Another challenge is implementing queries that need to retrieve data owned by multiple services.

The API Composition and Command Query Responsibility Segregation (CQRS) patterns.

**Patterns**

There are many patterns related to the microservices pattern. The Monolithic architecture is an alternative to the microservice architecture. The other patterns address issues that you will encounter when applying the microservice architecture.

- Decomposition patterns
    - [Decompose by business capability](https://microservices.io/patterns/decomposition/decompose-by-business-capability.html)
    - [Decompose by subdomain](https://microservices.io/patterns/decomposition/decompose-by-subdomain.html)
- The [Database per Service pattern](https://microservices.io/patterns/data/database-per-service.html) describes how each service has its own database in order to ensure loose coupling.
- The [API Gateway pattern](https://microservices.io/patterns/apigateway.html) defines how clients access the services in a microservice architecture.
- The [Client-side Discovery](https://microservices.io/patterns/client-side-discovery.html) and [Server-side Discovery](https://microservices.io/patterns/server-side-discovery.html) patterns are used to route requests for a client to an available service instance in a microservice architecture.
- The Messaging and Remote Procedure Invocation patterns are two different ways that services can communicate.
- The [Single Service per Host](https://microservices.io/patterns/deployment/single-service-per-host.html) and [Multiple Services per Host](https://microservices.io/patterns/deployment/multiple-services-per-host.html) patterns are two different deployment strategies.
- Cross-cutting concerns patterns: [Microservice chassis pattern](https://microservices.io/patterns/microservice-chassis.html) and [Externalized configuration](https://microservices.io/patterns/externalized-configuration.html)
- Testing patterns: [Service Component Test](https://microservices.io/patterns/testing/service-component-test.html) and [Service Integration Contract Test](https://microservices.io/patterns/testing/service-integration-contract-test.html)
- [Circuit Breaker](https://microservices.io/patterns/reliability/circuit-breaker.html)
- [Access Token](https://microservices.io/patterns/security/access-token.html)
- Observability patterns:
    - [Log aggregation](https://microservices.io/patterns/observability/application-logging.html)
    - [Application metrics](https://microservices.io/patterns/observability/application-metrics.html)
    - [Audit logging](https://microservices.io/patterns/observability/audit-logging.html)
    - [Distributed tracing](https://microservices.io/patterns/observability/distributed-tracing.html)
    - [Exception tracking](https://microservices.io/patterns/observability/exception-tracking.html)
    - [Health check API](https://microservices.io/patterns/observability/health-check-api.html)
    - [Log deployments and changes](https://microservices.io/patterns/observability/log-deployments-and-changes.html)
- UI patterns:
    - [[s]]
    - [Client-side UI composition](https://microservices.io/patterns/ui/client-side-ui-composition.html)

  

Minuses

- Complexity(more artifacts to deploy)
- Additional calls(more latency)
- Reduction to reliability

![[Untitled 1 6.png|Untitled 1 6.png]]

![[Untitled 2 4.png|Untitled 2 4.png]]

![[Untitled 3 4.png|Untitled 3 4.png]]

  

![[Untitled 4 3.png|Untitled 4 3.png]]

![[Untitled 5 3.png|Untitled 5 3.png]]

![[Untitled 6 3.png|Untitled 6 3.png]]

![[Untitled 7 3.png|Untitled 7 3.png]]

![[Untitled 8 3.png|Untitled 8 3.png]]

![[Untitled 9 3.png|Untitled 9 3.png]]

![[Untitled 10 3.png|Untitled 10 3.png]]

Instead of ACID

  

API Layer - proxy

Data Layer

Circuit breaker

![[Untitled 11 3.png|Untitled 11 3.png]]

![[Untitled 12 3.png|Untitled 12 3.png]]

![[Untitled 13 3.png|Untitled 13 3.png]]

![[Untitled 14 3.png|Untitled 14 3.png]]

  

### Atomic Transaction Microservices

![[Untitled 15 3.png|Untitled 15 3.png]]

![[Untitled 16 3.png|Untitled 16 3.png]]