---
Assignee: Mikhail Krasikov
Interview graded: true
Last edited time: 2024-02-17T11:34
Last recall: 2023-11-26
Needs Rework: false
Status: In progress
Topic:
  - "[[../Core\\|Java Core]]"
---
## Basic terms

==**Process**== - object, which created by operating system, when user starts the application.

**==Concurrency==** - the way of solving multiple tasks together.

**==Parallelism==** - the method of execution different parts of same program.

==**Thread**== - for a single process, the operating system created one main thread, which completes commands from the central processor one by one.  
To create new thread we can use class  
**Tread** or class **ExecutorService**.  
If an exception occurs in the thread that no one catches then it will terminate.  

### **Приоритет**

Потоку можно проставить приоритет - некоторое число, которое чем больше, тем больше приоритет. Такие потоки будут выполняться в первую очередь и иметь большее процессорное время.

**Mutex**

Флаг объекта, который может принимать состояние - свободен, занят

Intrinsic locks в Java выступают в качестве mutex (mutual exclusion locks), что означает, что максимум один поток может войти в лок. Mutex - это не класс и не интерфейс в Java, а просто общее понятие.возможны только два состояния — «свободен» и «занят».

**Monitor**

Реализован в java через synchronized. блочит выполнение метода объекта, если он кем-то уже выполняется

Дополнительная «надстройка» над мьютексом.. Также этим термином называют многопоточную конструкцию для контроля совместного доступа к общему ресурсу («монитор объекта»).Например synchronized. Защитный механизм создает именно монитор! Компилятор преобразует слово synchronized в несколько специальных кусков кода.

Предполагает условную переменную и две операции - ждать наступления условия и сигнализировать наступление условия. Когда условие выполнено, только одна задача из пула ждущих наступления этого условия задач начинает выполняться.

## States of thread

- ==**NEW**== - when a new thread is created, it is in the new state. The thread has not yet started to run when the thread is in this state.
- ==**RUNNABLE**== - a thread that is ready to run is moved to a runnable state. In this state, a thread might actually be running or it might be ready to run at any instant of time.
- ==**WAITING**== - when a thread is temporarily inactive, then it’s in one of the following states: blocked or waiting.
- ==**BLOCKED**== - when a thread is temporarily inactive, then it’s in one of the following states: blocked or waiting.
- ==**TIMED WAITING**== - a thread lies in a timed waiting state when it calls a method with a time-out parameter. A thread lies in this state until the timeout is completed or until a notification is received.
- ==**TERMINATED**== - a thread terminates because of either of the following reasons:  
    - Because it exits normally. This happens when the code of thread has been entirely executed by the program.  
    - Because there occured some unusual erroneous event, like segmentation fault or an unhandled exception.  
    

## Thread API

==**Thread**== - is an API for low-level threads managed by the JVM and the operating system.

- Thread.==**sleep**==**(**long millis**)** - method waits for some time, requires exception handling.
- Thread.==**yield**==() - forces the processor to switch to processing other threads.
- ==**wait**==**()** - method waits for a notification from another object. We can call method wait(long ms) with a time limit. It can be called ONLY in synchronized block, otherwise it throws an ==**IllegalMonitorStateException**==.
- **==setDaemon==****(**boolean on**)** - mark as thread that will be forcibly terminated when all normal thread has finished.
- ==**interrupt**==() - flag thread as thread that we want to complete.
- ==**join**==() - makes the current thread to wait for the one thread that has this method called.  
      
    

**==Runnable==** - is an interface that has one not implemented method ==**run**==(). The interface is used by Thread class to perform a task in a separate thread.  
  

==**Callable**== - the same as Runnable, but there is a generic for returning a result of a certain type.  
  

**Producer Consumer Problem with Wait and Notify**

[[producerconsume]]

## Concurrency package

### Executors

- **Thread Pool**
    
    Cоздает тред потоков фиксированного размера. И при запуске 4 потоков, будут переиспользоваться два.
    
    **ThreadPool** - позволяет создать очередь из потоков, с помощью которой можно сэкономить ресурсы на переключении контекстов
    
    - newFixedThreadPool - пул из нескольких потоков
    
    newSingleThreadExecutor - пул из одного потока
    
    A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing. Since the thread is already existing when the request arrives, the delay introduced by thread creation is eliminated, making the application more responsive.
    
    ### **Risks in using Thread Pools**
    
    - While deadlock can occur in any multi-threaded program, thread pools introduce another case of deadlock, one in which all the executing threads are waiting for the results from the blocked threads waiting in the queue due to the unavailability of threads for execution.
    - Thread Leakage occurs if a thread is removed from the pool to execute a task but not returned to it when the task completed. As an example, if the thread throws an exception and pool class does not catch this exception, then the thread will simply exit, reducing the size of the thread pool by one. If this repeats many times, then the pool would eventually become empty and no threads would be available to execute other requests.
    - Resource Thrashing :If the thread pool size is very large then time is wasted in context switching between threads. Having more threads than the optimal number may cause starvation problem leading to resource thrashing as explained.
    
    ### **Points**
    
    - Don’t queue tasks that concurrently wait for results from other tasks. This can lead to a situation of deadlock as described above.
    - Be careful while using threads for a long lived operation. It might result in the thread waiting forever and would eventually lead to resource leakage.
    - The Thread Pool has to be ended explicitly at the end. If this is not done, then the program goes on executing and never ends. Call shutdown() on the pool to end the executor. If you try to send another task to the executor after shutdown, it will throw a RejectedExecutionException.
    - One needs to understand the tasks to effectively tune the thread pool. If the tasks are very contrasting then it makes sense to use different thread pools for different types of tasks so as to tune them properly.
    - You can restrict maximum number of threads that can run in JVM, reducing chances of JVM running out of memory.
    - If you need to implement your loop to create new threads for processing, using ThreadPool will help to process faster, as ThreadPool does not create new Threads after it reached it’s max limit.
    - After completion of Thread Processing, ThreadPool can use the same Thread to do another process(so saving the time and resources to create another Thread.)
    
    ### **ThreadPoolExecutor**
    
    ThreadPoolExecutor is an extensible thread pool implementation with lots of parameters and hooks for fine-tuning. It uses BlockingQueue for new tasks and HashSet for workers, that pull new tasks from queue, while running their’s run method.
    
      
    
    [![](https://lh4.googleusercontent.com/R3qqpE-zH-Bm_F9sdE1uq0c_t5ugoAMCIIug-JPw_DlcmvjLBYFXu3vf-qzWDvCr7NPmH-eFf4FdYzXVl-YuV-CT-BQbGmDsKFqTYTBN0mLUKVSK6d6b7gaxyufqRVVZ7xrHLO3rkOHCQI7b8AWfCmDXR34ZHfUgwtwAMFQD8g-VOePIlaRpxvCXSRlj)](https://lh4.googleusercontent.com/R3qqpE-zH-Bm_F9sdE1uq0c_t5ugoAMCIIug-JPw_DlcmvjLBYFXu3vf-qzWDvCr7NPmH-eFf4FdYzXVl-YuV-CT-BQbGmDsKFqTYTBN0mLUKVSK6d6b7gaxyufqRVVZ7xrHLO3rkOHCQI7b8AWfCmDXR34ZHfUgwtwAMFQD8g-VOePIlaRpxvCXSRlj)
    
    The corePoolSize parameter is the number of core threads that will be instantiated and kept in the pool. When a new task comes in, if all core threads are busy and the internal queue is full, the pool is allowed to grow up to maximumPoolSize.
    
    The keepAliveTime parameter is the interval of time for which the excessive threads (instantiated in excess of the corePoolSize) are allowed to exist in the idle state. By default, the ThreadPoolExecutor only considers non-core threads for removal.
    
    ### **newFixedThreadPool**
    
    newFixedThreadPool method creates a ThreadPoolExecutor with equal corePoolSize and maximumPoolSize parameter values and a zero keepAliveTime. This means that the number of threads in this thread pool is always the same
    
    ### **newCachedThreadPool**
    
    newCachedThreadPool() method. This method does not receive a number of threads at all. We set the corePoolSize to 0 and set the maximumPoolSize to Integer.MAX_VALUE. Finally, the keepAliveTime is 60 seconds:
    
    A typical use case is when we have a lot of short-living tasks in our application.
    
    ### **newSingleThreadExecutor**
    
    newSingleThreadExecutor() API creates another typical form of ThreadPoolExecutor containing a single thread. The single thread executor is ideal for creating an event loop. The corePoolSize and maximumPoolSize parameters are equal to 1, and the keepAliveTime is 0.
    
    ### **ScheduledThreadPoolExecutor**
    
    - schedule method allows us to run a task once after a specified delay.
    - scheduleAtFixedRate method allows us to run a task after a specified initial delay and then run it repeatedly with a certain period.
    - scheduleWithFixedDelay method is similar to scheduleAtFixedRate in that it repeatedly runs the given task, but the specified delay is measured between the end of the previous task and the start of the next.
    
    ### **ForkJoinPool**
    
    ForkJoinPool is the central part of the fork/join framework introduced in Java 7. It solves a common problem of spawning multiple tasks in recursive algorithms. We'll run out of threads quickly by using a simple ThreadPoolExecutor, as every task or subtask requires its own thread to run.
    
    In a fork/join framework, any task can spawn (fork) a number of subtasks and wait for their completion using the join method. The benefit of the fork/join framework is that it does not create a new thread for each task or subtask, instead implementing the work-stealing algorithm. The work-stealing algorithm optimizes resource utilization by allowing idle threads to steal tasks from busy ones.
    
    ```Java
    public static class CountingTask extends RecursiveTask<Integer> {
    	private final TreeNode node;
    	
    	public CountingTask(TreeNode node) {
    		this.node = node;
    	}
    	
    	@Override
    	protected Integer compute() {
    		return node.value 
    						+ node.children.stream()
    								.map(childNode -> new CountingTask(childNode).fork())
    								.collect(Collectors.summingInt(ForkJoinTask::join));
    	}
    }
    
    TreeNode tree = new TreeNode(5, new TreeNode(3), 
    	new TreeNode(2,new TreeNode(2), new TreeNode(8)));
    	
    ForkJoinPool forkJoinPool = ForkJoinPool.commonPool();
    int sum = forkJoinPool.invoke(new CountingTask(tree));
    ```
    
- **Executors**
    
    ==**Executor**== - is an interface that has a single execute method to submit Runnable instances for execution.
    
    ```Java
    Executor executor = anExecutor;
    executor.execute(new RunnableTask1());
    executor.execute(new RunnableTask2());
    ```
    
      
      
    ==**ExecutorService**== - execution service, that serves as an alternative to class Thread which used to manage threads. This service is based on interface Executor in which only one method ==**execute**==(Runnable command) is defined.When method execute is called, the thread is executed. That is, the execute method starts the specified thread for execution.
    
    The ExecutorService interface contains a large number of methods to control the progress of the tasks and manage the termination of the service. Using this interface, we can submit the tasks for execution and also control their execution using the returned Future instance.  
    We can create ExecutorService based on Executors:  
    
    ```Java
    ExecutorService executor = Executors.newFixedThreadPool(2);
    ```
    
    In this case, pool of threads will be created
    
    [![](https://lh5.googleusercontent.com/BZZvgpK7Ybz3PHJrS3b4eWHIa3Fb1dMqj4VbpSOf_NHO3HD1gUdC-MpH7a9V8qt-_vvUte6rnNL1bAowg-fiYSYeXMD1fqEjLo4-hPzs8oQAfqJaA9myzSDyDRe_oAfLlhFut87QOCAiFKIqBNrQP3xF8NYoMP3AeVLkXn5Ec-hjLxwfsBj40t4fkoDB)](https://lh5.googleusercontent.com/BZZvgpK7Ybz3PHJrS3b4eWHIa3Fb1dMqj4VbpSOf_NHO3HD1gUdC-MpH7a9V8qt-_vvUte6rnNL1bAowg-fiYSYeXMD1fqEjLo4-hPzs8oQAfqJaA9myzSDyDRe_oAfLlhFut87QOCAiFKIqBNrQP3xF8NYoMP3AeVLkXn5Ec-hjLxwfsBj40t4fkoDB)
    
    [![](https://lh6.googleusercontent.com/oWdx5IIPp7mX6psfRh5OH95vdwRLriGvLII5uwcMHY7OJJn03nBp46jfjPr96ZiWt_ttJMhfffVlPBR7mbV5qolN5oDd0H0gF53OXdNQkb6IE2EYRn7XiDhycf-zx9dN5X5rgN1gS7aqoh-txUfnwApXy3CGo775RUWxP4Ml4Rv1anZuflC_y2Eu5Jp-)](https://lh6.googleusercontent.com/oWdx5IIPp7mX6psfRh5OH95vdwRLriGvLII5uwcMHY7OJJn03nBp46jfjPr96ZiWt_ttJMhfffVlPBR7mbV5qolN5oDd0H0gF53OXdNQkb6IE2EYRn7XiDhycf-zx9dN5X5rgN1gS7aqoh-txUfnwApXy3CGo775RUWxP4Ml4Rv1anZuflC_y2Eu5Jp-)
    
- **Future**
    
    **Future** - Методы интерфейса можно использовать для проверки завершения работы потока, ожидания завершения и получения результата
    
    **FutureTask**  - обертка - реализация Future, которая позволяет работать с callable или runnable, но при этом возвращать результат
    
    **Delayed** - интерфейс для маркировки объектов, которые должны будут быть обработаны после определенной задержки
    
    **ScheduledFuture** - отсроченное действие с результатом, которое можно отменить
    
    [![](https://lh5.googleusercontent.com/dYiPmSYUpjUh9hZjkSl_ecUevKXAJDv_-gCz_I6xg7Fu-jPhFXj7Xgjt3JOO9v3RpNid_k_ZIXInBsWC5wKcX_JOKrHuLjxymITtdhheQ04CvHM86luHI4bX_4KVXLEkB3yFsoePJ7EAdPG2Ad7275J9ATh0hmYq8k-CLraRrtoG9YbRSk1X6MPJ5ILE)](https://lh5.googleusercontent.com/dYiPmSYUpjUh9hZjkSl_ecUevKXAJDv_-gCz_I6xg7Fu-jPhFXj7Xgjt3JOO9v3RpNid_k_ZIXInBsWC5wKcX_JOKrHuLjxymITtdhheQ04CvHM86luHI4bX_4KVXLEkB3yFsoePJ7EAdPG2Ad7275J9ATh0hmYq8k-CLraRrtoG9YbRSk1X6MPJ5ILE)
    
    [![](https://lh4.googleusercontent.com/SQHTcGKt6Zd-qyUU41N2rmSisvzu-24AzRDHy3FNjUCV26fsslMPF92MnsLxnLix8RaaK9j88WWQKlvr44-XU4oInGTDD__x5Pv_AUn_fg5GOEFtW55e2eObCPr-BGzPQKp1SARHwy5eCFiotL4gCc_1EQlZ-eDNC1uq4oKDx_wPhEB0nEFhBhqWi2xD)](https://lh4.googleusercontent.com/SQHTcGKt6Zd-qyUU41N2rmSisvzu-24AzRDHy3FNjUCV26fsslMPF92MnsLxnLix8RaaK9j88WWQKlvr44-XU4oInGTDD__x5Pv_AUn_fg5GOEFtW55e2eObCPr-BGzPQKp1SARHwy5eCFiotL4gCc_1EQlZ-eDNC1uq4oKDx_wPhEB0nEFhBhqWi2xD)
    
    [![](https://lh4.googleusercontent.com/zA777JDVPaTXRiktoc6PKWnykTmLd8IlZdvQCEBCPgOKQ_rF6YblwGb7AA8NyeAsUAmJ9xNsCnX1TH4lT-8EV3QJfli41IdqvwW1Ko80m3luRGSPUT_YXQaMrCeVm_4tnGV4Ta0WBdxi-JHrOfQv4vpYCWsi6j40tTAVp6moGroH5iTGFHFWM2mocf9l)](https://lh4.googleusercontent.com/zA777JDVPaTXRiktoc6PKWnykTmLd8IlZdvQCEBCPgOKQ_rF6YblwGb7AA8NyeAsUAmJ9xNsCnX1TH4lT-8EV3QJfli41IdqvwW1Ko80m3luRGSPUT_YXQaMrCeVm_4tnGV4Ta0WBdxi-JHrOfQv4vpYCWsi6j40tTAVp6moGroH5iTGFHFWM2mocf9l)
    
    [![](https://lh4.googleusercontent.com/McTdCIYBzIDvaUM6v9T_q-a5yr0hm_zCwwz2ecilD_N8Bkycqiisx1xwh0mVaHzBYBoH5E4eLd0DQMwaz4jybpfLe93mRYBHqERFcXy0PHNXalyCUpeRFAmKG5By4OpvbPpBW6enKMFzdhLLlixy_sXgGeiWRgSKTfymaAark4wSjZoBI13fFhWcoEwC)](https://lh4.googleusercontent.com/McTdCIYBzIDvaUM6v9T_q-a5yr0hm_zCwwz2ecilD_N8Bkycqiisx1xwh0mVaHzBYBoH5E4eLd0DQMwaz4jybpfLe93mRYBHqERFcXy0PHNXalyCUpeRFAmKG5By4OpvbPpBW6enKMFzdhLLlixy_sXgGeiWRgSKTfymaAark4wSjZoBI13fFhWcoEwC)
    
- **ForkJoinPool**  
    
    **ForkJoinPool**  - специальный вид ExecutorService, который предназначен для выполнения рекурсивных задач.
    
    [![](https://lh6.googleusercontent.com/gi3XBsax6QAEwMJmOY7kPTciDZNlXWVqDfm76mhduyIktnrQslGR0RwcATDBUQ0rcuVdh9MajX0P5Zu1_bbSPWaNdE1DJkBuUUZapqgtOQ3rn_-kiQTTRN4IrCBrIw1fX7438g1HZqdA1ge8-ZD8DZZXBdlFxUfPWN0inpEMyvrclquUEAB6zvnKYvtv)](https://lh6.googleusercontent.com/gi3XBsax6QAEwMJmOY7kPTciDZNlXWVqDfm76mhduyIktnrQslGR0RwcATDBUQ0rcuVdh9MajX0P5Zu1_bbSPWaNdE1DJkBuUUZapqgtOQ3rn_-kiQTTRN4IrCBrIw1fX7438g1HZqdA1ge8-ZD8DZZXBdlFxUfPWN0inpEMyvrclquUEAB6zvnKYvtv)
    
    [![](https://lh4.googleusercontent.com/5JQh6gX4O7z9Y92xiUhyT0AuCGK-pwFSFUhGfpKYnTwgK3vE68S8TtScRPs8V8PtZcK_M0yq8tCAlaUb1qvFwgJBkjFv7bZLzov2b3poU5K2ZCy9RSrxl7ijedUgv4dadvd8QTadkKuGT0-BNM2xAWjCSdTSUYLTm9uyuu37yjU8K96bdQCJXghhr7S1)](https://lh4.googleusercontent.com/5JQh6gX4O7z9Y92xiUhyT0AuCGK-pwFSFUhGfpKYnTwgK3vE68S8TtScRPs8V8PtZcK_M0yq8tCAlaUb1qvFwgJBkjFv7bZLzov2b3poU5K2ZCy9RSrxl7ijedUgv4dadvd8QTadkKuGT0-BNM2xAWjCSdTSUYLTm9uyuu37yjU8K96bdQCJXghhr7S1)
    
    [![](https://lh3.googleusercontent.com/ovSvOGrYrVl94ercrJ3O4EFOgmRyKIJ78GUrd3JG_MXbLN8Svr-xlkzrtqsE8EIc68Zkqm8gvUGhUO_WKRg1EbLul0-C2XubIbSj2w1LS2JiDkwaTKm8YuTGNjJlucSqo09RcTnGkJdbsNcvb5yJbVeyjLr4NMTkvYuEP8IOkedmxQ6qXndhL1mD3blE)](https://lh3.googleusercontent.com/ovSvOGrYrVl94ercrJ3O4EFOgmRyKIJ78GUrd3JG_MXbLN8Svr-xlkzrtqsE8EIc68Zkqm8gvUGhUO_WKRg1EbLul0-C2XubIbSj2w1LS2JiDkwaTKm8YuTGNjJlucSqo09RcTnGkJdbsNcvb5yJbVeyjLr4NMTkvYuEP8IOkedmxQ6qXndhL1mD3blE)
    

### Atomics

### **Atomics (AtomicReference)**

Потокобезопасные переменные, которые поддержиивают атомарные операции. Атомарность поддерживается с помощью Compare and Swap. То есть при установку значения происходит проверка, соответствует ли новое значение ожидаемому. Если оно не соответствует, то операция не выполнена и она повторяется снова.

[![](https://lh6.googleusercontent.com/RUjorlHk3msjzQY8rNgVD0BU16wXPpLxO3X17TtzPCLSZ2yQlirsjtmgqaHlydUXDHVcszkjkU5OQw-JPB-ON4qZgICw-pmAJ63WgvWyzFSYxGXApAC0UnozmYoN9deXC4uV745QY-MKJycdRfDkgnbON9pPhw1M1h3ZphrfU_hGwzraxujJh-ushwVV)](https://lh6.googleusercontent.com/RUjorlHk3msjzQY8rNgVD0BU16wXPpLxO3X17TtzPCLSZ2yQlirsjtmgqaHlydUXDHVcszkjkU5OQw-JPB-ON4qZgICw-pmAJ63WgvWyzFSYxGXApAC0UnozmYoN9deXC4uV745QY-MKJycdRfDkgnbON9pPhw1M1h3ZphrfU_hGwzraxujJh-ushwVV)

### Synchronizers

Утилиты для синхронизации потоков, которые дают возможность разработчику регулировать и/или ограничивать работу потоков

- **Semaphore**
    
    **Semaphore -** это механизм (класс в Java), который позволяет контролировать доступ к одному или нескольким единицам ресурсов. Был предложен Эдсгаром Дийкстрой в 1962 году. Механизм основан на счётчике и двух методах  - wait() (в реализации Java - acquire()) и signal() (в реализации Java - release()).
    
    Имеет внутри себя переменную-счётчик, которая хранит в себе количество ресурсов, которые можно использовать, и две атомарные операции для увеличения или уменьшения значения переменной на единицу. Если значение переменной больше нуля, поток может войти в код. Если значение ноль, поток блокируется (вываливается исключение).
    
    То есть то же, что и lock, но может позволить себя захватить более чем одному потоку (например, чтобы ограничить количество cpu / io / ram - интенсивных задач, исполняемых одновременно). Binary semaphore - фактически mutex.
    
    Поток, который запрашивает ресурс, для которого нет свободных слотов, блокируется (либо прерывается, либо истекает время ожидания) до того момента, пока слот не освободится.
    
    **Semaphore** - ограничивает количество одновременно выполняемых потоков  
      
    
    [![](https://hsto.org/files/9da/48f/85b/9da48f85b5874362bc2279f181613c0e.gif)](https://hsto.org/files/9da/48f/85b/9da48f85b5874362bc2279f181613c0e.gif)
    
- **CountDownLatch**
    
    Это механизм синхронизации, который может отсрочить прогресс одного или более потока до наступления терминального состояния синхронизатора. Действует как ворота: поток ждёт у закрытых ворот, пока они не откроются (пока Latch не достигнет терминального состояния). Открытый latch остаётся открытым навсегда.
    
    Позволяет удостовериться, что определённые действия будут начаты только после завершения каких-то других одиночных действий. Например, вычисления не начнутся, пока какой-то ресурс не будет инициализирован. Или игра не начнётся, пока все участники многопользовательской игры не будут в состоянии готовности.
    
    Реализуется через счётчик, который инициализируется количеством событий, которые должны произойти до открытия ворот. Метод countDown() уменьшает счётчик на единицу, указывая, что одно из событий произошло. Метод await() усыпляет вызывающий поток до момента, когда счётчик достигнет нуля.
    
    **CountDownLatch** - предоставляет возможность любому количеству потоков в блоке кода ожидать до тех пор, пока не завершится определенное количество операций, выполняющихся в других потоках, перед тем как они будут «отпущены»
    
    [![](https://habrastorage.org/files/46b/3ae/b41/46b3aeb417cf4fb4ba271b4c66b52436.gif)](https://habrastorage.org/files/46b/3ae/b41/46b3aeb417cf4fb4ba271b4c66b52436.gif)
    
- **CycleBarrier**
    
    Этот механизм позволяет нескольким потокам ждать друг друга «на точке встречи». В отличие от Latch, который остаётся открытым навсегда после изначального открытия, барьер может переиспользоваться, после того как ждущие потоки рилизятся. Когда задача приходит в место встречи, она должна вызвать метод await() и ждать прибытия остальных задач.
    
    Когда все задачи в месте встречи, барьер пробуждает их все для дальнейших действий (барьер пройден) и сбрасывается для дальнейшего использования. Каждый await() в этом случае возвращает число - порядок, в котором поток прибыл к месту встречи. Если истекает время ожидания вызова await() или один из ждущих потоков прерывается, барьер считается сломанным, и вызовы await() у остальных потоков завершаются с исключением BrokenBarrierException.
    
    Реализация инициируется числом потоков, которые работают над одной задачей.
    
    Сходство с Latch в том, что и тот, и другой блокируют потоки до наступления событий. Отличие от Latch в том, что в Latch потоки ждут наступления каких-либо внешних событий, а в Barrier - друг друга.
    
    **CycleBarrier** - как только нужное количество потоков достигло нужной точки, все потоки продолжают работу
    
    [![](https://habrastorage.org/files/89a/f0c/b71/89af0cb71aad4465bb9c934b8be91a67.gif)](https://habrastorage.org/files/89a/f0c/b71/89af0cb71aad4465bb9c934b8be91a67.gif)
    
- **Exchanger**
    
    **Exchanger** - поток, вызывающий у обменника метод exchange() блокируется и ждет другой поток. Когда другой поток вызовет тот же метод, произойдет обмен объектами: каждая из них получит аргумент другой в методе exchange()
    
    [![](https://habrastorage.org/files/947/ef3/f47/947ef3f47ff843a099059006b30ea54d.gif)](https://habrastorage.org/files/947/ef3/f47/947ef3f47ff843a099059006b30ea54d.gif)
    
    Вероятно, наиболее интересным с точки зрения синхронизации является класс Exchanger, предназначенный для упрощения процесса обмена данными между двумя потоками исполнения.
    
    Принцип действия класса Exchanger очень прост: он ожидает до тех пор, пока два отдельных потока исполнения не вызовут его метод exchange(). Как только это произойдет, он произведет обмен данными, предоставляемыми обоими потоками. Нетрудно представить, как воспользоваться классом Exchanger. Например, один поток исполнения подготавливает буфер для приема данных через сетевое соединение, а другой - заполняет этот буфер данными, поступающими через сетевое соединение. Оба потока исполнения действуют совместно, поэтому всякий раз, когда требуется новая буферизация, осуществляется обмен данными
    
- **Phaser**
    
    Это механизм синхронизации, позволяющий контролировать выполнение алгоритмов, которые конкурентным способом могут быть разделены на фазы. Если есть процесс с чётко выраженными фазами, так что необходимо закончить предыдущую фазу, прежде чем переходить к следующей, можно использовать этот класс для конкуретной версии процесса.
    
    **Phaser** - то же, что и CyclicBarrier, но позволяет синхронизировать потоки, представляющие отдельную фазу или стадию выполнения общего действия.
    
    [![](https://habrastorage.org/files/086/6a4/b7a/0866a4b7acdf416384d4e7372b49a34b.gif)](https://habrastorage.org/files/086/6a4/b7a/0866a4b7acdf416384d4e7372b49a34b.gif)
    
    Phaser должен знать количество задач, которые ему нужно контролировать. Java называет это «регистрация участников».
    
    Задача должна информировать Phaser, когда она заканчивает выполнение фазы. Phaser усыпит задачу до момента выполнения этой фазы всеми задачами.
    
    Внутри себя Phaser хранит целое число, отражающее количество смены фаз.
    
    Участник может выйти из процесса в любой момент. Java называет это отменой регистрации участника.
    
    Когда phaser меняет фазы, можно исполнить кастомный код.
    
    Можно контролировать завершение Phaser. Когда Phaser завершается, новые участники не принимаются и не ведётся синхронизация между задачами.
    
    Phaser предоставляет методы, чтобы узнать статус и число участников.
    
    Регистрация участников
    
    Через конструктор Phaser() - создаёт Phaser с нулём участников.
    
    Через конструктор Phaser(int parties) - создаёт Phaser с числом участников parties.
    
    Через метод bulkRegister(int parties) - добавляет parties участников.
    
    Через метод register() - добавляет одного участника.
    
    Когда один из участников завершает выполение задачи, ему нужно отменить свою регистрацию в Phaser, иначе тот будет вечно ждать следующего изменения фазы. Для этого нужно вызвать метод arriveAndDeregister().
    
- **Отличие Mutex, Lock и Semaphore**
    
    **Lock** позволяет одному потоку войти в залоченный ресурс и не шарится с другими процессами.
    
    **Mutex –** то же, что lock, но может быть system-wide, то есть может шариться с другими процессами. Реализован в операционной системе. Используется для resource sharing.
    
    **Semaphore –** используется для signaling от одной задачи к другой (а НЕ ДЛЯ защиты нескольких эквивалентных ресурсов, как некоторые ошибочно полагают).
    

**Основные характеристики класса:**

### Collections

- **BlockingQueue**
    
    Java BlockingQueue implementations are thread-safe. All queuing methods are atomic in nature and use internal locks or other forms of concurrency control.
    
    Java BlockingQueue interface is part of java collections framework and it’s primarily used for implementing producer consumer problem. We don’t need to worry about waiting for the space to be available for producer or object to be available for consumer in BlockingQueue because it’s handled by implementation classes of BlockingQueue.
    
- **Collections.synchronizedCollection()**
    
    Возвращает потокобезопасную коллекцию, на основе переданной. Достигает потокобезопасности через блокировку монитора.
    
- **Concurrent collections**
    
    Отдельные коллекции, например ConcurrentHashMap. Достигают синхронизации с помощью деления коллекции на сегменты. То есть несколько потоков могут обратиться к map одновременно, но только к разным ее блокам. CopyOnWrite коллекции удобно использовать, когда write операции довольно редки, например при реализации механизма подписки listeners и прохода по ним.
    
- **CopyOnWrite**
    
    Все операции по изменению коллекции (add, set, remove) приводят к созданию новой копии внутреннего массива. Тем самым гарантируется, что при проходе итератором по коллекции не кинется ConcurrentModificationException. Следует помнить, что при копировании массива копируются только референсы (ссылки) на объекты (shallow copy), т.ч. доступ к полям элементов не thread-safe.
    

### Locks

Lock is a more flexible and sophisticated thread synchronization mechanism than the standard synchronized block.

Представляет собой альтернативные и более гибкие механизмы синхронизации потоков по сравнению с базовыми synchronized, wait, notify, notifyAll. Using synchronized:

- It is not possible to interrupt a thread waiting to acquire a lock (lock Interruptibly).
- It is not possible to attempt to acquire a lock without being willing to wait for it forever (try lock).
- Cannot implement non-block-structured locking disciplines, as intrinsic locks must be released in the same block in which they are acquired.
- Faireless locking

- **Lock interface**
    
    Один из базовых механизмов синхронизации в Java - интерфейс Lock и реализующие его классы. Любой объект в Java может выступать в качестве lock для целей синхронизации. Такие встроенные локи называются intrinsic locks или monitor locks. Исполняющий поток автоматически занимает лок перед тем, как войти в код внутри синхронизированного блока, и автоматически освобождает его, когда выходит из этого блока (через нормальное исполнение кода или выброс исключения).
    
    Базовая реализация - ReentrantLock. Поток, который захватил, но не освободил ReentrantLock, владеет этим локом. Как и intrinsic лок, данный лок является reentrant. В общем случае, если поток запрашивает лок, который удерживается другим потоком, запрашивающий поток блокируется до освобождения лока. Однако если поток запрашивает лок, которым он уже владеет в данный момент, запрос удовлетворяется.
    
    Таким образом, reentrancy означает, что локи захватываются на основе per thread, а не per invocation. Реализуется ассоциацией с каждым локом счётчика захватов. Когда счётчик равен нулю, лок считается незанятым. Когда поток захватывает прежде не занятый лок, JVM записывает владельца лока и увеличивает счётчик на единицу. Если тот же поток захватывает лок снова, счётчик опять увеличивается, а когда освобождает - уменьшается. Когда счётчик становится равным нулю, лок освобождается.
    
    Let's take a look at the methods in the Lock interface:
    
    **void lock()** – acquire the lock if it's available; if the lock isn't available a thread gets blocked until the lock is released
    
    **void lockInterruptibly()** – this is similar to the lock(), but it allows the blocked thread to be interrupted and resume the execution through a thrown java.lang.InterruptedException
    
    **boolean tryLock()** – this is a non-blocking version of lock() method; it attempts to acquire the lock immediately, return true if locking succeeds
    
    **boolean tryLock(long timeout, TimeUnit timeUnit)** – this is similar to tryLock(), except it waits up the given timeout before giving up trying to acquire the Lock
    
    **void unlock()** – unlocks the Lock instance
    
    A locked instance should always be unlocked to avoid deadlock condition. A recommended code block to use the lock should contain a try/catch and finally block:
    
      
    
- **ReentrantLock**
    
    ReentrantLock class implements the Lock interface. It offers the same concurrency and memory semantics, as the implicit monitor lock accessed using synchronized methods and statements, with extended capabilities.
    
    We need to make sure that we are wrapping the lock() and the unlock() calls in the try-finally block to avoid the deadlock situations.
    
    Let's see how the tryLock() works:
    
    ```Java
    public void performTryLock(){
    	//...
    	boolean isLockAcquired = lock.tryLock(1, TimeUnit.SECONDS);
    	
    	if(isLockAcquired) {
    		try {
    			//Critical section here
    		} finally {
    			lock.unlock();
    		}
    	}
    	//...
    }
    ```
    
      
    
    In this case, the thread calling tryLock(), will wait for one second and will give up waiting if the lock isn't available.
    
    [![](https://lh6.googleusercontent.com/3M_KyJS72b9ox7CDpwwZT8qKxUECFlZfKdy4xMf-gB0rBO_v7vayGloPlQoNQjHjhf1j76-iTot7nRhvLi4c45v-nXW9YiYMVLpOc1mEe-7WWoOQrejMBikhEBqLqn8ZMyzsHdqqWLwRv0lykjMprC797wtzmH4ZINWn54ij31ptz65uWVSE-JfDagtG)](https://lh6.googleusercontent.com/3M_KyJS72b9ox7CDpwwZT8qKxUECFlZfKdy4xMf-gB0rBO_v7vayGloPlQoNQjHjhf1j76-iTot7nRhvLi4c45v-nXW9YiYMVLpOc1mEe-7WWoOQrejMBikhEBqLqn8ZMyzsHdqqWLwRv0lykjMprC797wtzmH4ZINWn54ij31ptz65uWVSE-JfDagtG)
    
- **ReadWriteLock**
    
    - поддерживает пару связанных блокировок: одну для операций только для чтения, а другую - для записи. Блокировка чтения может удерживаться одновременно несколькими потоками чтения, пока нет писателей. Блокировка записи является исключительной.
    
    [![](https://lh4.googleusercontent.com/i1mCU_Wanf0y7BNItmATsNYLVq-yc9je3-Sg2jd-cT1JolwcJ0q4yswe35XppcFRyGqw7e1R_rbHWxvKIeyKV_fxuCvRvgGuFMafwZSjBTsgxILMKHPH0gMzneGd8t6BvF5gfI-M9xEgcT3a8yqcR8FyAi8iZGCJ61QFoJCXatp19Pu5jKyhnu4TW5lt)](https://lh4.googleusercontent.com/i1mCU_Wanf0y7BNItmATsNYLVq-yc9je3-Sg2jd-cT1JolwcJ0q4yswe35XppcFRyGqw7e1R_rbHWxvKIeyKV_fxuCvRvgGuFMafwZSjBTsgxILMKHPH0gMzneGd8t6BvF5gfI-M9xEgcT3a8yqcR8FyAi8iZGCJ61QFoJCXatp19Pu5jKyhnu4TW5lt)
    
- **ReentrantReadWriteLock**
    
    **ReentrantReadWriteLock**
    
    ReentrantReadWriteLock class implements the ReadWriteLock interface.
    
    Let's see rules for acquiring the ReadLock or WriteLock by a thread:
    
    Read Lock – if no thread acquired the write lock or requested for it then multiple threads can acquire the read lock
    
    Write Lock – if no threads are reading or writing then only one thread can acquire the write lock
    
    ```Java
    Map<String,String> syncHashMap = new HashMap<>();
    ReadWriteLock lock = new ReentrantReadWriteLock();
    // ...
    Lock writeLock = lock.writeLock();
    
    public void put(String key, String value) {
    	try {
    		writeLock.lock();
    		syncHashMap.put(key, value);
    	} finally {
    		writeLock.unlock();
    	}
    }
    
    public String remove(String key){
    	try {
    		writeLock.lock();
    		return syncHashMap.remove(key);
    	} finally {
    		writeLock.unlock();
    	}
    }
    ```
    
- **StampedLock**
    
    ### **StampedLock**
    
    StampedLock is introduced in Java 8. It also supports both read and write locks. However, lock acquisition methods return a stamp that is used to release a lock or to check if the lock is still valid:
    
    ```Java
    Map<String,String> map = new HashMap<>();
    private StampedLock lock = new StampedLock();
    
    public void put(String key, String value){
    long stamp = lock.writeLock();
    try {
    map.put(key, value);
    } finally {
    lock.unlockWrite(stamp);
    }
    }
    
    public String get(String key) throws InterruptedException {
    long stamp = lock.readLock();
    try {
    return map.get(key);
    } finally {
    lock.unlockRead(stamp);
    }
    }
    ```
    
    Another feature provided by StampedLock is optimistic locking. Most of the time read operations don't need to wait for write operation completion and as a result of this, the full-fledged read lock isn't required.
    
    Instead, we can upgrade to read lock:
    
    ```Java
    public String readWithOptimisticLock(String key) {
    
    long stamp = lock.tryOptimisticRead();
    
    String value = map.get(key);
    
    if(!lock.validate(stamp)) {
    
    stamp = lock.readLock();
    
    try {
    
    return map.get(key);
    
    } finally {
    
    lock.unlock(stamp);
    
    }
    
    }
    
    return value;
    
    }
    ```
    
- **Condition**
    
    ### **Condition**
    
    The Condition class provides the ability for a thread to wait for some condition to occur while executing the critical section.
    
    This can occur when a thread acquires the access to the critical section but doesn't have the necessary condition to perform its operation. For example, a reader thread can get access to the lock of a shared queue, which still doesn't have any data to consume.
    
    Traditionally Java provides wait(), notify() and notifyAll() methods for thread intercommunication. Conditions have similar mechanisms, but in addition, we can specify multiple conditions:
    
    ```Java
    Stack<String> stack = new Stack<>();
    int CAPACITY = 5;
    ReentrantLock lock = new ReentrantLock();
    Condition stackEmptyCondition = lock.newCondition();
    Condition stackFullCondition = lock.newCondition();
    public void pushToStack(String item){
    try {
    lock.lock();
    
    while(stack.size() == CAPACITY) {
    
    stackFullCondition.await();
    
    }
    
    stack.push(item);
    
    stackEmptyCondition.signalAll();
    
    } finally {
    
    lock.unlock();
    
    }
    
    }
    
    public String popFromStack() {
    
    try {
    
    lock.lock();
    
    while(stack.size() == 0) {
    
    stackEmptyCondition.await();
    
    }
    
    return stack.pop();
    
    } finally {
    
    stackFullCondition.signalAll();
    
    lock.unlock();
    
    }
    
    }
    ```
    
- **Lock vs Synchronized Block**
    
    There are few differences between the use of synchronized block and using Lock API's:
    
    - A synchronized block is fully contained within a method – we can have Lock API's lock() and unlock() operation in separate methods
    - A synchronized block doesn't support the fairness, any thread can acquire the lock once released, no preference can be specified. We can achieve fairness within the Lock APIs by specifying the fairness property. It makes sure that longest waiting thread is given access to the lock
    - A thread gets blocked if it can't get an access to the synchronized block. The Lock API provides tryLock() method. The thread acquires lock only if it's available and not held by any other thread. This reduces blocking time of thread waiting for the lock
    - A thread which is in “waiting” state to acquire the access to synchronized block, can't be interrupted. The Lock API provides a method lockInterruptibly() which can be used to interrupt the thread when it's waiting for the lock
- **Issues**
    
    ### **Deadlock**
    
    Состояние, когда два потока заняли ресурсы, к которым хочет обратиться противоположный поток.
    
    [![](https://lh6.googleusercontent.com/DXsm3HSpw0DERaD-_P40kw4VKEckc0smXKGTBru1Q4YoXFHcZtAEW_GmTWworT2uVfvvNjYl4_9ETHlhVIkOoTxFdIEKt8hCPZ6sKKxz77Psi2TCF-TzDemLnh84eLSaCO9MBb9MbbWNjbGJAcuCRgW-19vo6u0lMOevwciR7BdT5xKNMcQ9tmFAEF3h)](https://lh6.googleusercontent.com/DXsm3HSpw0DERaD-_P40kw4VKEckc0smXKGTBru1Q4YoXFHcZtAEW_GmTWworT2uVfvvNjYl4_9ETHlhVIkOoTxFdIEKt8hCPZ6sKKxz77Psi2TCF-TzDemLnh84eLSaCO9MBb9MbbWNjbGJAcuCRgW-19vo6u0lMOevwciR7BdT5xKNMcQ9tmFAEF3h)
    
    **Deadlock Prevention**
    
    - **Lock Ordering -** **If you make sure that all locks are always taken in the same order by any thread, deadlocks cannot occur.**
    - **Lock Timeout** - put a timeout on lock attempts meaning a thread trying to obtain a lock will only try for so long before giving up. If a thread does not succeed in taking all necessary locks within the given timeout, it will backup, free all locks taken, wait for a random amount of time and then retry.
    - **Interruptible lock** acquisition allows locking to be used within cancellable activities.The lockInterruptibly method allows us to try and acquire a lock while being available for interruption.
    
    [![](https://lh4.googleusercontent.com/UP1izeTODqei8g9lRM-vXaJjaYXkI3pAEyLNIaicD0-Hjn0yI5S2Ca9urpuspvvn1WBhZqpHcR7sKGPt65A4BnAtN3CtIvIkZBdIpurJY8-bs8aMrEQ5WCeKucWENtjkb3yHZJnP8vRfPN2R1l60H9aB-x3m9RnfAtvQYimo75o1q2KldfhyjLT1tSR2)](https://lh4.googleusercontent.com/UP1izeTODqei8g9lRM-vXaJjaYXkI3pAEyLNIaicD0-Hjn0yI5S2Ca9urpuspvvn1WBhZqpHcR7sKGPt65A4BnAtN3CtIvIkZBdIpurJY8-bs8aMrEQ5WCeKucWENtjkb3yHZJnP8vRfPN2R1l60H9aB-x3m9RnfAtvQYimo75o1q2KldfhyjLT1tSR2)
    
    **Анализ объектов в дедлоке**
    
    JJVM allows you to diagnose deadlocks by displaying them in thread dumps. Such dumps include information on the state of the flow. If it is locked, the dump contains information about the monitor the stream is waiting to release. JVM looks at a graph of expected (busy) monitors before output of the dump threads, and if it finds a loop, it adds information about mutual locking, specifying the monitors and threads involved.
    
    [![](https://lh4.googleusercontent.com/tdEfKXhXZZbAiKG1APXMwRZkOcXxqnMu0zQspMUVsUu0Vo5l5RqW5y4Ob0qfsY_fapEVt_DZ7GTzrc6wOB0KxJR3-wgShq9kAo8O6tvD2tWnxdybpGg_Jzf2Pxk1es6z3wptQ1RmoNiQkHJt4ybA3wOtTpbhyX-6q-zRNlw12E3wkdR7cGqrHZqiPV9k)](https://lh4.googleusercontent.com/tdEfKXhXZZbAiKG1APXMwRZkOcXxqnMu0zQspMUVsUu0Vo5l5RqW5y4Ob0qfsY_fapEVt_DZ7GTzrc6wOB0KxJR3-wgShq9kAo8O6tvD2tWnxdybpGg_Jzf2Pxk1es6z3wptQ1RmoNiQkHJt4ybA3wOtTpbhyX-6q-zRNlw12E3wkdR7cGqrHZqiPV9k)
    

## JMM

Модель памяти Java описывает поведение потоков в среде исполнения Java. Модель памяти — часть семантики языка Java, и описывает, на что может и на что не должен рассчитывать программист, разрабатывающий ПО не для конкретной Java-машины, а для Java в целом.

**Visibility**

One thread may not see the changes made by another because the variables are saved to the registers and the local CPU cache

**Reordering**

Compiler can move operations to improve performance

**Happens before** - Пусть есть поток X и поток Y (не обязательно отличающийся от потока X). И пусть есть операции A (выполняющаяся в потоке X) и B (выполняющаяся в потоке Y).

В таком случае, A happens-before B означает, что все изменения, выполненные потоком X до момента операции A и изменения, которые повлекла эта операция, видны потоку Y в момент выполнения операции B и после выполнения этой операции.

Happens-before is a concept, a phenomenon, or simply a set of rules that define the basis for reordering of instructions by a compiler or CPU.

1. В рамках одного поток любая операция happens-before любой операцией следующей за ней в исходном коде
2. Освобождение лока (unlock) happens-before захват того же лока (lock)
3. Выход из synchronized блока/метода happens-before вход в synchronized блок/метод на том же мониторе
4. Запись volatile поля happens-before чтение того же самого volatile поля
5. Завершение метода run экземпляра класса Thread happens-before выход из метода join() или возвращение false методом isAlive() экземпляром того же треда
6. Вызов метода start() экземпляра класса Thread happens-before начало метода run() экземпляра того же треда
7. Завершение конструктора happens-before начало метода finalize() этого класса
8. Вызов метода interrupt() на потоке happens-before когда поток обнаружил, что данный метод был вызван либо путем выбрасывания исключения InterruptedException, либо с помощью методов isInterrupted() или interrupted()
9. An unlock on a monitor happens-before every subsequent lock on that monitor.
10. A write to a volatile field happens-before every subsequent read of that field.
11. A call to start() on a thread happens-before any actions in the started thread.
12. All actions in a thread happen-before any other thread successfully return from a join() on that thread.
13. The default initialization of any object happens-before any other actions (other than default-writes) of a program.
14. When a statement invokes Thread.start, every statement that has a happens-before relationship with that statement also has a happens-before relationship with every statement executed by the new thread. The effects of the code that led up to the creation of the new thread are visible to the new thread.
15. When a thread terminates and causes a Thread.join in another thread to return, then all the statements executed by the terminated thread have a happens-before relationship with all the statements following the successful join. The effects of the code in the thread are now visible to the thread that performed the join.

Write volatile field happens-before reading the same volatile field