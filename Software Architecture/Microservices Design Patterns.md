---
Interview graded: false
Last edited time: 2023-08-23T11:26
Last recall: 2023-07-22
Needs Rework: true
Status: Not started
Topic:
  - "[[Web Services Architecture]]"
---
![[Untitled 90.png|Untitled 90.png]]

# Decomposition Patterns

## By Business Capability

(By Business Process Domains)

![[Untitled 1 21.png|Untitled 1 21.png]]

![[Untitled 2 16.png|Untitled 2 16.png]]

![[Untitled 3 16.png|Untitled 3 16.png]]

## By SubDomain

Divide by data domains starting with model

![[Untitled 4 12.png|Untitled 4 12.png]]

![[Untitled 5 12.png|Untitled 5 12.png]]

![[Untitled 6 12.png|Untitled 6 12.png]]

## By Transaction

## Decomposition Strategies

## Strangler Pattern

Moving parts of monolith to different services

![[Untitled 7 10.png|Untitled 7 10.png]]

![[Untitled 8 10.png|Untitled 8 10.png]]

![[Untitled 9 10.png|Untitled 9 10.png]]

![[Untitled 10 10.png|Untitled 10 10.png]]

## Sidecar Pattern

Removing repetetive code

![[Untitled 11 10.png|Untitled 11 10.png]]

![[Untitled 12 10.png|Untitled 12 10.png]]

# Integration Patterns

## Gateway Patterns

It is a proxy or facade

![[Untitled 13 10.png|Untitled 13 10.png]]

![[Untitled 14 10.png|Untitled 14 10.png]]

![[Untitled 15 9.png|Untitled 15 9.png]]

![[Untitled 16 9.png|Untitled 16 9.png]]

## Process Aggregator

For cases when several business processes should be called, aggregates the call and calls each process

![[Untitled 17 6.png|Untitled 17 6.png]]

![[Untitled 18 6.png|Untitled 18 6.png]]

![[Untitled 19 6.png|Untitled 19 6.png]]

![[Untitled 20 5.png|Untitled 20 5.png]]

## Edge Pattern

![[Untitled 21 5.png|Untitled 21 5.png]]

![[Untitled 22 5.png|Untitled 22 5.png]]

![[Untitled 23 5.png|Untitled 23 5.png]]

  

# Database Patterns

## Database Per Service (Single Service, Single Database)

![[Untitled 24 5.png|Untitled 24 5.png]]

![[Untitled 25 5.png|Untitled 25 5.png]]

## Shared Database Per Service

![[Untitled 26 4.png|Untitled 26 4.png]]

## Command Query Responsibility Segregation (CQRS)

![[Untitled 27 4.png|Untitled 27 4.png]]

![[Untitled 28 4.png|Untitled 28 4.png]]

## Asynchronous eventing

![[Untitled 29 4.png|Untitled 29 4.png]]

# Operational Patterns

# Observability Patterns

## Log Aggregation pattern

![[Untitled 30 4.png|Untitled 30 4.png]]

![[Untitled 31 4.png|Untitled 31 4.png]]

  

## Metrics aggregation pattern

![[Untitled 32 4.png|Untitled 32 4.png]]

![[Untitled 33 4.png|Untitled 33 4.png]]

  

## Tracing Pattern

![[Untitled 34 4.png|Untitled 34 4.png]]

![[Untitled 35 4.png|Untitled 35 4.png]]

# Cross Cutting Concerns Patterns

## External Configuration Pattern

![[Untitled 36 4.png|Untitled 36 4.png]]

![[Untitled 37 4.png|Untitled 37 4.png]]

## Service Discovery Patterns

![[Untitled 38 4.png|Untitled 38 4.png]]