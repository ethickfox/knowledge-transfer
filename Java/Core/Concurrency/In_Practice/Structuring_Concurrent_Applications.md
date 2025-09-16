# Task Execution
## Executing tasks in threads
- Executing tasks sequentially - Single thread
- Explicitly creating threads for tasks - create a new thread for servicing each request
  - Disadvantages of unbounded thread creation
    - Thread lifecycle overhead - If requests are frequent and lightweight, as in most server applications, creating a new thread for each request can consume signiﬁcant computing resources.
    - Resource consumption. Active threads consume system resources, especially memory. When there are more runnable threads than available processors, threads sit idle.Having many idle threads can tie up a lot of memory, putting pressure on the garbage collector,
    - Stability. There is a limit on how many threads can be created. The limit varies by platform and is affected by factors including JVM invocation parameters, the requested stack size in the Thread constructor, and limits on threads placed by the underlying operating system.2 When you hit this limit, the most likely result is an OutOfMemoryError.
