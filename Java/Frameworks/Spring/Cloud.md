---
Interview graded: false
Last edited time: 2023-11-29T17:43
Needs Rework: false
Status: Not started
Topic:
  - "[../Spring](../Spring)"
---
![Untitled 103.png](Untitled%20103.png)

![Untitled 1 30.png](Untitled%201%2030.png)

  

[https://microservices.io/patterns/reliability/circuit-breaker.html](https://microservices.io/patterns/reliability/circuit-breaker.html)

## Externalised Configuration

![Untitled 2 22.png](../../../Software_Architecture/_img/Untitled%202%2022.png)

![Untitled 3 20.png](Untitled%203%2020.png)

  

## Service Discovery

![Untitled 4 15.png](../../../Software_Architecture/_img/Untitled%204%2015.png)

![Untitled 5 15.png](Untitled%205%2015.png)

### Eureka

The annotation @EnableEurekaServer exposes the Eureka API and web console for a Spring Boot application.

  

The annotation @EnableDiscoveryClient is used to expose the service to Eureka based on default or overridden configuration.

![Untitled 6 15.png](Untitled%206%2015.png)

The @LoadBalanced annotation is used to load balance service calls across all instances discovered from Eureka.

### Feign Client

OpenFeign dependency

The [spring.application.name](http://spring.application.name/) is what is exposed via Eureka that the feign client consumes.

![Untitled 7 13.png](Untitled%207%2013.png)

## Circuit Breaking

![Untitled 8 13.png](Untitled%208%2013.png)

![Untitled 9 13.png](Untitled%209%2013.png)

![Untitled 10 13.png](Untitled%2010%2013.png)

![Untitled 11 13.png](Untitled%2011%2013.png)

![Untitled 12 12.png](Untitled%2012%2012.png)

![Untitled 13 12.png](Untitled%2013%2012.png)

### API Gateway

![Untitled 14 12.png](Untitled%2014%2012.png)

![Untitled 15 11.png](Untitled%2015%2011.png)

![Untitled 16 11.png](Untitled%2016%2011.png)

![Untitled 17 8.png](Untitled%2017%208.png)

![Untitled 18 8.png](Untitled%2018%208.png)

![Untitled 19 8.png](Untitled%2019%208.png)

### Telemetry

![Untitled 20 7.png](Untitled%2020%207.png)

![Untitled 21 7.png](Untitled%2021%207.png)