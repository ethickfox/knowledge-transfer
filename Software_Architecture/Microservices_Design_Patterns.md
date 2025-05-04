---
Interview graded: false
Last edited time: 2023-08-23T11:26
Last recall: 2023-07-22
Needs Rework: true
Status: Not started
Topic:
  - "[Web Services Architecture](Web%20Services%20Architecture)"
---
![Untitled 90.png](_img/Untitled%2090.png)

# Decomposition Patterns

## By Business Capability

(By Business Process Domains)

![Untitled 1 21.png](_img/Untitled%201%2021.png)

![Untitled 2 16.png](_img/Untitled%202%2016.png)

![Untitled 3 16.png](_img/Untitled%203%2016.png)

## By SubDomain

Divide by data domains starting with model

![Untitled 4 12.png](_img/Untitled%204%2012.png)

![Untitled 5 12.png](_img/Untitled%205%2012.png)

![Untitled 6 12.png](_img/Untitled%206%2012.png)

## By Transaction

## Decomposition Strategies

## Strangler Pattern

Moving parts of monolith to different services

![Untitled 7 10.png](_img/Untitled%207%2010.png)

![Untitled 8 10.png](../Computer_Science/_img/Untitled%208%2010.png)

![Untitled 9 10.png](_img/Untitled%209%2010.png)

![Untitled 10 10.png](_img/Untitled%2010%2010.png)

## Sidecar Pattern

Removing repetetive code

![Untitled 11 10.png](_img/Untitled%2011%2010.png)

![Untitled 12 10.png](_img/Untitled%2012%2010.png)

# Integration Patterns

## Gateway Patterns

It is a proxy or facade

![Untitled 13 10.png](_img/Untitled%2013%2010.png)

![Untitled 14 10.png](_img/Untitled%2014%2010.png)

![Untitled 15 9.png](_img/Untitled%2015%209.png)

![Untitled 16 9.png](Untitled%2016%209.png)

## Process Aggregator

For cases when several business processes should be called, aggregates the call and calls each process

![Untitled 17 6.png](_img/Untitled%2017%206.png)

![Untitled 18 6.png](_img/Untitled%2018%206.png)

![Untitled 19 6.png](_img/Untitled%2019%206.png)

![Untitled 20 5.png](_img/Untitled%2020%205.png)

## Edge Pattern

![Untitled 21 5.png](_img/Untitled%2021%205.png)

![Untitled 22 5.png](_img/Untitled%2022%205.png)

![Untitled 23 5.png](_img/Untitled%2023%205.png)

  

# Database Patterns

## Database Per Service (Single Service, Single Database)

![Untitled 24 5.png](_img/Untitled%2024%205.png)

![Untitled 25 5.png](_img/Untitled%2025%205.png)

## Shared Database Per Service

![Untitled 26 4.png](Untitled%2026%204.png)

## Command Query Responsibility Segregation (CQRS)

![Untitled 27 4.png](_img/Untitled%2027%204.png)

![Untitled 28 4.png](_img/Untitled%2028%204.png)

## Asynchronous eventing

![Untitled 29 4.png](_img/Untitled%2029%204.png)

# Operational Patterns

# Observability Patterns

## Log Aggregation pattern

![Untitled 30 4.png](_img/Untitled%2030%204.png)

![Untitled 31 4.png](_img/Untitled%2031%204.png)

  

## Metrics aggregation pattern

![Untitled 32 4.png](_img/Untitled%2032%204.png)

![Untitled 33 4.png](_img/Untitled%2033%204.png)

  

## Tracing Pattern

![Untitled 34 4.png](_img/Untitled%2034%204.png)

![Untitled 35 4.png](_img/Untitled%2035%204.png)

# Cross Cutting Concerns Patterns

## External Configuration Pattern

![Untitled 36 4.png](_img/Untitled%2036%204.png)

![Untitled 37 4.png](_img/Untitled%2037%204.png)

## Service Discovery Patterns

![Untitled 38 4.png](_img/Untitled%2038%204.png)