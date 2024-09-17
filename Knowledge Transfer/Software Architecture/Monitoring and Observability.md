---
Interview graded: true
Last edited time: 2023-07-16T20:26
Needs Rework: false
Status: Not started
Topic:
  - "[[Web Services Architecture]]"
---
### **System health monitoring**

Monitoring overall system health is important to ensure that your system is performing well.

**Monitoring metrics:**

- Average response time
- A number of live HTTP sessions - The number of live HTTP sessions reflects the concurrent usage of your site. The more concurrent live sessions, the more memory is required
- Database and connection pool size
- Java virtual memory (JVM) - Use JVM metrics to understand the JVM heap dynamics, including the frequency of garbage collection. This data can help set the optimal heap size. In addition, use the metric to identify potential memory leaks.
- CPU and I/O usage

### **System Observability**

Ability to measure the internal state of a system only by its external outputs. It is invaluable for investigating situations where a system starts to deviate from its intended state

- logs - lines of text that an application generates at discrete points
- metrics - values represented as counts or measures that we calculate or aggregate over a time period.
- traces - representation of distributed events as the request flows through a distributed system.

### **Observability vs. Monitoring**

Monitoring basically refers to the practice of watching a system's state through a predefined set of metrics and logs. We can only effectively monitor a system if it's observable

**OpenTracing** is a project that provides vendor-neutral APIs and instrumentation for distributed tracing