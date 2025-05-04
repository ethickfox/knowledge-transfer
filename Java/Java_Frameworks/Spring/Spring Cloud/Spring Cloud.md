---
Interview graded: false
Last edited time: 2023-11-29T17:43
Needs Rework: false
Status: Not started
Topic:
  - "[[../Spring]]"
---
![[Untitled 103.png|Untitled 103.png]]

![[Untitled 1 30.png|Untitled 1 30.png]]

  

[https://microservices.io/patterns/reliability/circuit-breaker.html](https://microservices.io/patterns/reliability/circuit-breaker.html)

## Externalised Configuration

![[Untitled 2 22.png|Untitled 2 22.png]]

![[Untitled 3 20.png|Untitled 3 20.png]]

  

## Service Discovery

![[Untitled 4 15.png|Untitled 4 15.png]]

![[Untitled 5 15.png|Untitled 5 15.png]]

### Eureka

The annotation @EnableEurekaServer exposes the Eureka API and web console for a Spring Boot application.

  

The annotation @EnableDiscoveryClient is used to expose the service to Eureka based on default or overridden configuration.

![[Untitled 6 15.png|Untitled 6 15.png]]

The @LoadBalanced annotation is used to load balance service calls across all instances discovered from Eureka.

### Feign Client

OpenFeign dependency

The [spring.application.name](http://spring.application.name/) is what is exposed via Eureka that the feign client consumes.

![[Untitled 7 13.png|Untitled 7 13.png]]

## Circuit Breaking

![[Untitled 8 13.png|Untitled 8 13.png]]

![[Untitled 9 13.png|Untitled 9 13.png]]

![[Untitled 10 13.png|Untitled 10 13.png]]

![[Untitled 11 13.png|Untitled 11 13.png]]

![[Untitled 12 12.png|Untitled 12 12.png]]

![[Untitled 13 12.png|Untitled 13 12.png]]

### API Gateway

![[Untitled 14 12.png|Untitled 14 12.png]]

![[Untitled 15 11.png|Untitled 15 11.png]]

![[Untitled 16 11.png|Untitled 16 11.png]]

![[Untitled 17 8.png|Untitled 17 8.png]]

![[Untitled 18 8.png|Untitled 18 8.png]]

![[Untitled 19 8.png|Untitled 19 8.png]]

### Telemetry

![[Untitled 20 7.png|Untitled 20 7.png]]

![[Untitled 21 7.png|Untitled 21 7.png]]