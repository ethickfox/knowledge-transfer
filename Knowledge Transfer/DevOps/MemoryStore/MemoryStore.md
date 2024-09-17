---
Assignee: Nikita Katyshev
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Not started
Topic:
  - "[[Google Cloud]]"
---
Memorystore for Redis provides a fully-managed service that is powered by the Redis in-memory data store to build application caches that provide sub-millisecond data access.

[![](https://lh6.googleusercontent.com/bv5bs5jb7KIf3WG1gl33PaQ6bOgh_UufSZoq5ixsa_wvIgANWMZTXdSYYqO75ZMRYvDqhtp5gHJKsEuwu6sV13gA2rTyUIFAgoz9n0QRuHYcFXvU3iIilBOJ8nvfPvOUUazmAiYQGDslcUYjdSeAG3BeB1k98-v1rbiIqZPGAyL2cT2lFtdsFMaqGu7_AA)](https://lh6.googleusercontent.com/bv5bs5jb7KIf3WG1gl33PaQ6bOgh_UufSZoq5ixsa_wvIgANWMZTXdSYYqO75ZMRYvDqhtp5gHJKsEuwu6sV13gA2rTyUIFAgoz9n0QRuHYcFXvU3iIilBOJ8nvfPvOUUazmAiYQGDslcUYjdSeAG3BeB1k98-v1rbiIqZPGAyL2cT2lFtdsFMaqGu7_AA)

Memorystore for Redis is compatible with any client library for Redis, for example Jedis  
.  

```XML
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>4.3.1</version>
</dependency>
```