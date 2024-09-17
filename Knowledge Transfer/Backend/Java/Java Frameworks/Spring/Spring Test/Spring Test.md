---
Interview graded: false
Last edited time: 2023-11-29T17:30
Needs Rework: false
Status: Not started
Topic:
  - "[[Spring]]"
---
Put [`@Transactional`](https://docs.spring.io/spring/docs/5.2.4.RELEASE/javadoc-api/org/springframework/transaction/annotation/Transactional.html) on your test class or methods and _test-managed transactions_ will automatically be rollbacked.

Since it's a test, the transaction should be by default rolledback thanks to `TransactionalTestExecutionListener`. Now you are asking why something is committed anyway, it can be because of inner transactions. `@Transactional` spans the transaction for entire test method, so if you use some dao (like in your case) that transaction will be rolledback also, however if some method uses transaction propagation type other than `REQUIRED`, for example `REQUIRED_NEW`, call to db can be performed anyway, because `REQUIRED_NEW` suspends current transaction from the test and creates a new one. That new one will be committed. There is also flush time issue, due hibernate caching - he makes a call to db at the end of transaction, usually at the end of the method, so fetching elements directly after persisting them may fail. Both things can be tricky.

[https://relentlesscoding.com/posts/automatic-rollback-of-transactions-in-spring-tests/](https://relentlesscoding.com/posts/automatic-rollback-of-transactions-in-spring-tests/)

Is it a good practice to use @Transactional on test methods?

Sure, but not to test transaction boundaries. Remember that you can use also `@DataJpaTest`, which places `@Transactional` for every test method.

  

  

![[/Untitled 122.png|Untitled 122.png]]

![[/Untitled 1 43.png|Untitled 1 43.png]]

![[/Untitled 2 31.png|Untitled 2 31.png]]

![[/Untitled 3 26.png|Untitled 3 26.png]]