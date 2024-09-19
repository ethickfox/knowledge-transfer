---
Interview graded: true
Last edited time: 2024-04-03T19:11
Last recall: 2024-04-03
Needs Rework: false
Status: Not started
Topic:
  - "[[Knowledge Transfer/Backend/Java/Java Core/Java Core\\|Java Core]]"
---
## **JAR and WAR**

**Java Archive**

Содержит файлы классов, ресурсы, зависимые библиотеки, и другие необходимые для приложения файлы. Может содержать точку входа, и использоваться как цель для исполнения команды java

**Web Archive**

Технически имеет ту же структуру, но другую роль – архив JavaEE web-компонента. Обычно содержит jar-ы с реализацией, JSP, статические файлы фронт-энда, и мета-информацию для сервлет-контейнера (web.xml). В основном используется как деплоймент web-приложения в сервлет-контейнер

# **JVM**

## **JDK**

Комплект разработчика приложений, включает в себя Jre и Java Development Tools(компилятор, архиватор и тд)

## **JRE**

Пакет всего необходимого для запуска скомпилированной java программы(байткода). Включает в себя Java Class Library и JVM

## **JVM**

Спецификация программы, предназначенная для выполнения байт-кода Java.

Состоит из

- **Загрузчика классов**
    - **Bootstrap ClassLoader** - встроенная в JVM нативная реализация, родитель для всех остальных загрузчиков. Загружает часть стандартных классов java.*
    - **Platform ClassLoader(Extentions)** - отвечает за загрузку стандартных классов Java-рантайма. Гарантируется, что ему будут видны (но не факт что загружены непосредственно им) все стандартные классы Java SE и JDK.
    - **Application ClassLoader -** загружает классы из classpath конкретного приложения;
- **Сборщика мусора**
- **Интерпретатора**
- **Jit компилятора**

**Hotspot -** JVM выпускаемая Oracle, написана на c++

## **JIT**

Обычно в приложении только небольшие части кода выполняются достаточно часто и производительность приложения в основном зависит от скорости выполнения именно этих частей. Эти части кода называются горячими точками (hot spots), их и компилирует JIT компилятор. Существует 2 компилятора:

- С1(клиентский) - компилирует быстрее, но код менее оптимален
- С2(серверный) - компилирует медленнее, но код более оптимален

## **Рефлексия**

Механизм исследования данных о программе во время её выполнения. Рефлексия позволяет исследовать информацию о полях, методах и конструкторах классов.

## **Enum**

## **Ссылки**

В java все объекты хранятся в куче, а в переменных лишь ссылки на эти области памяти. Вся передача происходит по значению. Для примитивов - копия текущего значения, для ссылок на объекты - копия ссылки

**Виды**

### **Strong**

Обычные ссылки, всякий раз, когда на объект ссылается цепочка сильных ссылочных объектов, он не может быть собран мусором.

StringBuilder builder = new StringBuilder();

### **Soft**

Если на объект ссылается только ссылка такого типа, то он доступен для сборки мусора GC только когда jvm не начнет не хватать памяти и не возникнет риск OutOfMemoryError (в этом случае он может быть удален).Используется для кэша, например, чтобы предотвратить удаление из кэша делаем сильные ссылки, а если нет в нем нужды, то мягкие

SoftReference<StringBuilder> reference1 = new SoftReference<>(builder);

### **Weak**

Если на объект ссылается только ссылка такого типа, то он доступен для сборки мусора GC.Используются для хранения вспомогательной информации об объекте(например в WeakHashmap), если объект уничтожается, то и связанные данные пропадут

WeakReference<StringBuilder> weakBuilder = new WeakReference<StringBuilder>(builder);

weakBuilder.get() - возвращает объект, если он не был убран гц, иначе null

### **Phantom**

Если на объект ссылается только ссылка такого типа, то он доступен для сборки мусора GC. Мы не можем получить объект из такой ссылки, и для PhantomReference нужна очередь со ссылками Мы можем проверять в этой очереди, не был ли удален объект, на который ссылается PhantomReference и вмеcто finalize

ReferenceQueue<StringBuilder> referenceQueue = new ReferenceQueue<>();

PhantomReference<StringBuilder> reference1 = new PhantomReference<>(builder, referenceQueue);

# **GC/Memory**

## **Pojo**

Plain old java object - Обычный java объект

### **Lifecycle**

- создан - new
- используется
- невидимый - существуют сильные ссылки но к ним нет доступа (ссылка внутри for а мы снаружи)
- недостижимый - не осталось сильных ссылок на данный объект
- собранный - помечен для сборки
- финализированный - был вызван finalize метод и объект ожидает сборки
- деалоцированный - объект уничтожен

### Generation hypothesis

The probability of death as a function of age decreases very quickly. The vast majority of objects live extremely short. The longer you live, the more likely you are to live.

## **Виды и принцип работы GC**

### **Способы работы с мусором**

**Поиск**

- **Reference counting** - подсчет ссылок, если два объекта ссылаются друг на друга, то они не будут удалены
- **Tracing** - достижимость объекта из корневых точек(поток, класс и тд)

**Очистка**

- Mark and sweep - проходим по графу достижимости и помечаем объекты. Все что не помечено удаляем
- Copying collectors - делим память на две части - в одну переносим все живые объекты, а старые объекты оставляем в другой и считаем ее чистой

## **Виды GC**

- **Serial** замораживает потоки при очистке. minor gc - использует copy collectors для очищения эдема, и mark sweep для уплотнения old gen
- **Parallel** все то же, но в несколько потоков (**default Java 8**)
- **CMS** останавливает для пометки объектов доступных напрямую из корней. Параллельно с работой помечает все остальные. Приостанавливает снова, чтобы найти что не отметил в прошлый раз. Собирает мусор параллельно с работой
- **G1** - много областей памяти, все помечены либо эдем, либо survivor, либо tenured. Есть малая сборка останавливает приложение и чистит только те области, которые успеет за отведенное время. Смешанная сборка - пометка(составляет список живых объектов), initial mark - отметка корней, Concurrent marking - остановка приложения и пометка объектов в нес потоков, remark - доп поиск неучтенных объектов. Cleanup - очистка (**default Java 9**)
- **ZGC**

## **Организация памяти**

### Object Header

**mark word**  - хранит вспомогательную информацию

**identity_hashcode** - хэшкод объекта

**age** - количество пережитых сборок мусора. Когда age достигает числа max-tenuring-threshold,объект перемещается в область хипа old generation.

biased_lock - включение/выключение(Biased Locking - ускоряет повторный захват объекта тем же потоком)

**lock** - состояние блокировки объекта

- 00 — Lightweight Locked - время захвата данного объекта разными потоками не пересекается вообще или пересекается незначительно
- 01 — Unlocked or Biased - объектом преимущественно владеет какой-то конкретный поток
- 10 — Heavyweight Locked - время захвата данного объекта разными потоками пересекается значительно
- 11 — Marked for Garbage Collection.

**сlass word** - хранится указатель на сам класс, то есть, на то место, где лежит информация о данном типе данных: методы, аннотации, наследование и другое

[![](https://lh3.googleusercontent.com/SJvtj-ngsfOpz4ryoQSj-xD8QY9o5rB2v9etk9SQwXrKyJB7880MANJfnfCWu-75kIxYpTe5A6U8ps_Lng962ZaGlfNcwm-gUdK-NT_8PoaOgo_3UB7k0YWBCHm5aEt_dfDiknalktCEuVcbGg-x9pIyJJ-sZxDweUwxMbx-xvGssvoISwhIIuSvghAI)](https://lh3.googleusercontent.com/SJvtj-ngsfOpz4ryoQSj-xD8QY9o5rB2v9etk9SQwXrKyJB7880MANJfnfCWu-75kIxYpTe5A6U8ps_Lng962ZaGlfNcwm-gUdK-NT_8PoaOgo_3UB7k0YWBCHm5aEt_dfDiknalktCEuVcbGg-x9pIyJJ-sZxDweUwxMbx-xvGssvoISwhIIuSvghAI)

[![](https://lh4.googleusercontent.com/GxgQmKE0fD_Z-pfSumb5dcAimEzEBpt9PdlmLfSyjmWO9qhLgzvuj7rIANMK93uecpUnyqZ2bcmKPT8BtzrFgBoVTqOSQNJGqO-ErvhMqlQnQ1Pik44TIz2N2IQNdWDBsKLfVLecPJthU8BWmhd1Ds1q_Ab87JtlfPI2-8-MlFDgFp6nJm7FCQc-oshb)](https://lh4.googleusercontent.com/GxgQmKE0fD_Z-pfSumb5dcAimEzEBpt9PdlmLfSyjmWO9qhLgzvuj7rIANMK93uecpUnyqZ2bcmKPT8BtzrFgBoVTqOSQNJGqO-ErvhMqlQnQ1Pik44TIz2N2IQNdWDBsKLfVLecPJthU8BWmhd1Ds1q_Ab87JtlfPI2-8-MlFDgFp6nJm7FCQc-oshb)

[![](https://lh4.googleusercontent.com/iiaHhUGcSKoEeD0vjYcFqXDBFXvyTYqmiwlz6RSayGDot8e348w3FP69426PD-kNn_-pam4DRR3li5u2Ci1f34vmGc1aBg8sEvPdWSfUff4jNGXyzPo59s1nquRar5YBHs2X8Vn9VzbkbX3vDnIeYmWFe1XY1rPDgJYzE98zdWvxFJcfeVq0QWBlMSgB)](https://lh4.googleusercontent.com/iiaHhUGcSKoEeD0vjYcFqXDBFXvyTYqmiwlz6RSayGDot8e348w3FP69426PD-kNn_-pam4DRR3li5u2Ci1f34vmGc1aBg8sEvPdWSfUff4jNGXyzPo59s1nquRar5YBHs2X8Vn9VzbkbX3vDnIeYmWFe1XY1rPDgJYzE98zdWvxFJcfeVq0QWBlMSgB)

[![](https://lh6.googleusercontent.com/gWUL7uqmAOHq0aJFdp0j2G0JTldv7Z61_Y47Mtu3dwajqUtEsY3ES11an-x8xM6h1ou8bmQUKbLHlsFimwqIDkkNw-pV8LlQMH6I05awxVm8ZufzEoM_M45SIYvURx2BkVUd_VandS2w-eCC0XWYnqj9IGxeEltHUbW87N8Xa9dYJ-wuLRmFH-d9rvcY)](https://lh6.googleusercontent.com/gWUL7uqmAOHq0aJFdp0j2G0JTldv7Z61_Y47Mtu3dwajqUtEsY3ES11an-x8xM6h1ou8bmQUKbLHlsFimwqIDkkNw-pV8LlQMH6I05awxVm8ZufzEoM_M45SIYvURx2BkVUd_VandS2w-eCC0XWYnqj9IGxeEltHUbW87N8Xa9dYJ-wuLRmFH-d9rvcY)

[![](https://lh4.googleusercontent.com/l2AZy_pMY21moVYSV3_lgjFU6KK6Hht30rdihlFRqCHY6RybLzEjshTphoCb0UqxOm35--53DviCvTq4abY1zZMomEuzhOIzd5JzEnvegJoTw-N54-x0JyRKfY10rjfGVKnoG4RY-4f7Ewf0Hn_bu1ugM4u_gAfURwD76RucR1VzoTOA7LHJzuVQxVG9)](https://lh4.googleusercontent.com/l2AZy_pMY21moVYSV3_lgjFU6KK6Hht30rdihlFRqCHY6RybLzEjshTphoCb0UqxOm35--53DviCvTq4abY1zZMomEuzhOIzd5JzEnvegJoTw-N54-x0JyRKfY10rjfGVKnoG4RY-4f7Ewf0Hn_bu1ugM4u_gAfURwD76RucR1VzoTOA7LHJzuVQxVG9)

**GC roots** - корни сборщика мусора, из которых идет поиск живых объектов

**Количество сборок объекта** GC хранится в хедере объекта

In Java 1.8 or earlier, JDK holds back the memory while in JDK 11+ Java releases memory to the OS.

  

[[Constant Pool]]

## **Проблемы**

**Дефрагментация CMS**

**Дефрагментация** - процесс перераспределения фрагментов файлов и логических структур файловых систем на дисках для обеспечения непрерывной последовательности кластеров.

Дефрагментация требует полной остановки программы(Stop the world)

### **Stop the world**

Пауза необходимая сборщику мусора для обхода размеченных объектов.Тк граф динамичен. Во время его обхода ранее размеченные объекты могут стать недоступными, и сборщик мусора может не удалить то, что только что стало мусором.

**Latency**

Задержка - сколько времени требуется на одну уборку мусора

Пропускная способность - сколько мусора можно убрать за одну уборку

  

[https://www.digitalocean.com/community/tutorials/java-jvm-memory-model-memory-management-in-java#](https://www.digitalocean.com/community/tutorials/java-jvm-memory-model-memory-management-in-java#)

### Questions

- Difference between Heap and Stack Memory in java. And how java utilizes this.
    
    The stack is a part of memory that contains information about nested method calls down to the current position in the program. It also contains all local variables and references to objects on the heap defined in currently executing methods.
    
    This structure allows the runtime to return from the method knowing the address whence it was called, and also clear all local variables after exiting the method. Every thread has its own stack.
    
    The heap is a large bulk of memory intended for allocation of objects. When you create an object with the _new_ keyword, it gets allocated on the heap. However, the reference to this object lives on the stack.
    
    Stack memory is the portion of memory that was assigned to every individual program. And it was fixed. On the other hand, Heap memory is the portion that was not allocated to the java program but it will be available for use by the java program when it is required, mostly during the runtime of the program.
    
    **Java Utilizes this memory as -**
    
    - When we write a java program then all the variables, methods, etc are stored in the stack memory.
    - And when we create any object in the java program then that object was created in the heap memory. And it was referenced from the stack memory.
- Tell us something about JIT compiler.
    
    - JIT stands for Just-In-Time and it is used for improving the performance during run time. It does the task of compiling parts of byte code having similar functionality at the same time thereby reducing the amount of compilation time for the code to run.
    - The compiler is nothing but a translator of source code to machine-executable code. But what is special about the JIT compiler? Let us see how it works:
        - First, the Java source code (.java) conversion to byte code (.class) occurs with the help of the javac compiler.
        - Then, the .class files are loaded at run time by JVM and with the help of an interpreter, these are converted to machine understandable code.
        - JIT compiler is a part of JVM. When the JIT compiler is enabled, the JVM analyzes the method calls in the .class files and compiles them to get more efficient and native code. It also ensures that the prioritized method calls are optimized.
        - Once the above step is done, the JVM executes the optimized code directly instead of interpreting the code again. This increases the performance and speed of the execution.
    
    ![[Untitled 62.png|Untitled 62.png]]
    
- What is the importance of reflection in Java?
    
    - The term `reflection` is used for describing the inspection capability of a code on other code either of itself or of its system and modify it during runtime.
    - Consider an example where we have an object of unknown type and we have a method ‘fooBar()’ which we need to call on the object. The static typing system of Java doesn't allow this method invocation unless the type of the object is known beforehand. This can be achieved using reflection which allows the code to scan the object and identify if it has any method called “fooBar()” and only then call the method if needed.
    
    ```Java
    Method methodOfFoo = fooObject.getClass().getMethod("fooBar",null);
    methodOfFoo.invoke(fooObject,null);
    ```
    
    - Using reflection has its own cons:
        - Speed — Method invocations due to reflection are about three times slower than the direct method calls.
        - Type safety — When a method is invoked via its reference wrongly using reflection, invocation fails at runtime as it is not detected at compile/load time.
        - Traceability — Whenever a reflective method fails, it is very difficult to find the root cause of this failure due to a huge stack trace. One has to deep dive into the invoke() and proxy() method logs to identify the root cause.
    - Hence, it is advisable to follow solutions that don't involve reflection and use this method as a last resort.
- Is exceeding the memory limit possible in a program despite having a garbage collector?
    
    Yes, it is possible for the program to go out of memory in spite of the presence of a garbage collector. Garbage collection assists in recognizing and eliminating those objects which are not required in the program anymore, in order to free up the resources used by them.
    
    In a program, if an object is unreachable, then the execution of garbage collection takes place with respect to that object. If the amount of memory required for creating a new object is not sufficient, then memory is released for those objects which are no longer in the scope with the help of a garbage collector. The memory limit is exceeded for the program when the memory released is not enough for creating new objects.
    
    Moreover, exhaustion of the heap memory takes place if objects are created in such a manner that they remain in the scope and consume memory. The developer should make sure to dereference the object after its work is accomplished. Although the garbage collector endeavors its level best to reclaim memory as much as possible, memory limits can still be exceeded.
    
- What are the possible ways of making object eligible for garbage collection (GC) in Java?
    
    **First Approach:** Set the object references to null once the object creation purpose is served.
    
    **Second Approach:** Point the reference variable to another object. Doing this, the object which the reference variable was referencing before becomes eligible for GC.
    
    **Third Approach:** Island of Isolation Approach: When 2 reference variables pointing to instances of the same class, and these variables refer to only each other and the objects pointed by these 2 variables don't have any other references, then it is said to have formed an “Island of Isolation” and these 2 objects are eligible for GC.
    
    ```Java
    public class IBGarbageCollect {
       IBGarbageCollect ib;    
       public static void main(String [] str){
           IBGarbageCollect ibgc1 = new IBGarbageCollect();
           IBGarbageCollect ibgc2 = new IBGarbageCollect();
           ibgc1.ib = ibgc2; //ibgc1 points to ibgc2
           ibgc2.ib = ibgc1; //ibgc2 points to ibgc1
           ibgc1 = null;
           ibgc2 = null;
           /* 
           * We see that ibgc1 and ibgc2 objects refer 
           * to only each other and have no valid 
           * references- these 2 objects for island of isolcation - eligible for GC
           */
       }
    }
    ```
    
- What Is Garbage Collection and What Are Its Advantages?
    
    Garbage collection is the process of looking at heap memory, identifying which objects are in use and which are not, and deleting the unused objects.
    
    An in-use object, or a referenced object, means that some part of your program still maintains a pointer to that object. An unused object, or unreferenced object, is no longer referenced by any part of your program. So the memory used by an unreferenced object can be reclaimed.
    
    The biggest advantage of garbage collection is that it removes the burden of manual memory allocation/deallocation from us so that we can focus on solving the problem at hand.
    
- Are There Any Disadvantages of Garbage Collection?
    
    Yes. Whenever the garbage collector runs, it has an effect on the application’s performance. This is because all other threads in the application have to be stopped to allow the garbage collector thread to effectively do its work.
    
    Depending on the requirements of the application, this can be a real problem that is unacceptable by the client. However, this problem can be greatly reduced or even eliminated through skillful optimization and garbage collector tuning and using different GC algorithms.
    
- What Is the Meaning of the Term “Stop-The-World”?
    
    When the garbage collector thread is running, other threads are stopped, meaning the application is stopped momentarily.
    
    Depending on the needs of an application, “stop the world” garbage collection can cause an unacceptable freeze. This is why it is important to do garbage collector tuning and JVM optimization so that the freeze encountered is at least acceptable.
    
- What JVM consists of?
    - **Загрузчика классов**
        - **Bootstrap ClassLoader** - встроенная в JVM нативная реализация, родитель для всех остальных загрузчиков. Загружает часть стандартных классов java.*
        - **Platform ClassLoader(Extentions)** - отвечает за загрузку стандартных классов Java-рантайма. Гарантируется, что ему будут видны (но не факт что загружены непосредственно им) все стандартные классы Java SE и JDK.
        - **Application ClassLoader -** загружает классы из classpath конкретного приложения;
    - **Сборщика мусора**
    - **Интерпретатора**
    - **Jit компилятора**
- What is JIT Compiler?
    
    Typically, only some paricular pieces of code are executed frequently enough in an application and the performance of the application depends mainly on the speed of execution of these parts. These code parts are called hot spots (hot spots), they are and compiles the JIT compiler. There are 2 compilers:
    
    - C1(client) - compiles faster, but the code is less optimal
    - C2 (server) - compiles more slowly, but the code is more optimal
- What is reflection?
    
    The mechanism of studying information about the program at the time of its execution. Reflection allows to study information about fields, methods and constructors of classes.
    
- What is Reference?
    
    In Java, all objects are stored in a heap, and in variables only references to these memory areas. All transmission occurs by value. For primitives - copy of current value, for object references - copy of link
    
- Describe Strong, Weak, Soft and Phantom References and Their Role in Garbage Collection.
    - **Strong** - Normal links, whenever a chain of strong reference objects refers to an object, it cannot be collected by garbage**.  
          
        **`StringBuilder builder = new StringBuilder();`  
        The story only changes when we nullify   
        `builder` like this:  
          
        `builder = null;`  
        After calling the above line, the object will then be eligible for collection.  
        
    - **Soft -** If this type of reference is the only reference to the object. It will become eligible for gc collection when memory becomes tight and the JVM is on the brink of throwing an _OutOfMemory_ error. In other words, objects with only soft references are collected as a last resort to recover memory. Used for cache, for example, to prevent deletion from cache make strong links, and if not in it soft  
          
        `SoftReference<StringBuilder> reference1 = new SoftReference<>(builder);`
    - **Weak -** when object only has a weak reference, the JVM’s garbage collector will have absolutely no compromise and immediately collect the object at the very next cycle. Used to store additional information about the object (for example in WeakHashmap), if the object is destroyed, the associated data will be lost  
          
        `WeakReference<StringBuilder> weakBuilder = new WeakReference<StringBuilder>(builder);`  
          
        `weakBuilder.get()` - возвращает объект, если он не был убран гц, иначе null
    - **Phantom -** is similar to a weak reference and an object with only phantom references will be collected without waiting. However, phantom references are enqueued as soon as their objects are collected. We can poll the reference queue to know exactly when the object was collected.  
          
        `ReferenceQueue<StringBuilder> referenceQueue = new ReferenceQueue<>(); PhantomReference<StringBuilder> reference1 = new PhantomReference<>(builder, referenceQueue);`
- Pojo lifecycle?
    - created - new
    - in use
    - invisible - there are strong links but no access (link inside for and we outside)
    - unattainable - no strong references to this object
    - assembled - marked for assembly
    - finalised - final method was called and the object is waiting for assembly
    - De-alocated - the facility was destroyed
- What are the ways of Garbage Collection?
    
    **Search**
    
    - **Reference counting** - if two objects refer to each other, they will not be deleted
    - **Tracing** - reachability of an object from root points (stream, class, etc.)
    
    **Cleaning**
    
    - **Mark and sweep** - go through the reachability graph and mark objects. Anything that is not marked is removed
    - **Copying collectors** - divide memory into two parts - in one we transfer all living objects, and leave old objects in another and consider it clean
- Garbage Collectors types
    
      
    
    - **Serial -** uses the simple **mark-sweep-compact** approach for young and old generations garbage collection i.e Minor and Major GC.Serial GC is useful in client machines such as our simple stand-alone applications and machines with smaller CPU. It is good for small applications with low memory footprint.
    - **Parallel** - is same as Serial GC except that is spawns N threads for young generation garbage collection where N is the number of CPU cores in the system. We can control the number of threads using `-XX:ParallelGCThreads=n` JVM option. Parallel Garbage Collector is also called throughput collector because it uses multiple CPUs to speed up the GC performance. Parallel GC uses a single thread for Old Generation garbage collection. It can be set using **-XX:+UseParallelGC**
    - **CMS** also referred as concurrent low pause collector. It does the garbage collection for the Old generation. CMS collector tries to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently with the application threads. CMS collector on the young generation uses the same algorithm as that of the parallel collector. This garbage collector is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using `-XX:ParallelCMSThreads=n` JVM option.
    - **G1** - many areas of memory, all labeled either Eden, survivor, or tenured. There is a small build stops the app and cleans only those areas that will have time. Mixed assembly - annotation (makes a list of living objects), initial mark - a mark of roots, Concurrent marking - stopping the application and marking objects in bore threads, remark - in addition to searching for unaccounted objects. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector. Garbage First Collector doesn’t work like other collectors and there is no concept of Young and Old generation space. It divides the heap space into multiple equal-sized heap regions. When a garbage collection is invoked, it first collects the region with lesser live data, hence “Garbage First”.
    - **ZGC**
- What is Generation hypothesis?
    
    The probability of death as a function of age decreases very quickly. The vast majority of objects live extremely short. The longer you live, the more likely you are to live.
    
- What Is Generational Garbage Collection?
    
    Generational garbage collection can be loosely defined as the strategy used by the garbage collector where the heap is divided into a number of sections called generations, each of which will hold objects according to their “age” on the heap.
    
    Whenever the garbage collector is running, the first step in the process is called marking. This is where the garbage collector identifies which pieces of memory are in use and which are not. This can be a very time-consuming process if all objects in a system must be scanned.
    
    With generational garbage collection, objects are grouped according to their “age” in terms of how many garbage collection cycles they have survived. This way, the bulk of the work spread across various minor and major collection cycles.
    
- Describe in Detail How Generational Garbage Collection Works
    
    The heap is divided up into smaller spaces or generations. These spaces are Young Generation, Old or Tenured Generation, and Metaspace
    
    [![](https://lh4.googleusercontent.com/l2AZy_pMY21moVYSV3_lgjFU6KK6Hht30rdihlFRqCHY6RybLzEjshTphoCb0UqxOm35--53DviCvTq4abY1zZMomEuzhOIzd5JzEnvegJoTw-N54-x0JyRKfY10rjfGVKnoG4RY-4f7Ewf0Hn_bu1ugM4u_gAfURwD76RucR1VzoTOA7LHJzuVQxVG9)](https://lh4.googleusercontent.com/l2AZy_pMY21moVYSV3_lgjFU6KK6Hht30rdihlFRqCHY6RybLzEjshTphoCb0UqxOm35--53DviCvTq4abY1zZMomEuzhOIzd5JzEnvegJoTw-N54-x0JyRKfY10rjfGVKnoG4RY-4f7Ewf0Hn_bu1ugM4u_gAfURwD76RucR1VzoTOA7LHJzuVQxVG9)
    
    The **young generation** hosts most of the newly created objects. It is is further divided into three spaces: an Eden space and two survivor spaces such as Survivor 1 (s1) and Survivor 2 (s2).
    
    The **old generation hosts objects that** **have lived in memory longer than a certain “age”**. The objects that survived garbage collection from the young generation are promoted to this space. It is generally larger than the young generation. As it is bigger in size, the garbage collection is more expensive and occurs less frequently than in the young generation.
    
    One of the basic ways of garbage collection involves three steps:
    
    1. **Marking**: This is the first step where garbage collector identifies which objects are in use and which ones are not in use.
    2. **Normal Deletion**: Garbage Collector removes the unused objects and reclaim the free space to be allocated to other objects.
    3. **Deletion with Compacting**: For better performance, after deleting unused objects, all the survived objects can be moved to be together. This will increase the performance of allocation of memory to newer objects.
    
    Process:
    
    1. First, **any new objects are allocated to the Eden space**. Both survivor spaces start out empty. When the Eden space fills up, a minor garbage collection is triggered. Referenced objects are moved to the first survivor space. Unreferenced objects are deleted.
    2. During the next minor GC, the same thing happens to the Eden space. Unreferenced objects are deleted and referenced objects are moved to a survivor space. However, in this case, they are moved to the second survivor space (S2).
    3. In addition, objects from the last minor GC in the first survivor space (S1) have their age incremented and are moved to S2. Once all surviving objects have been moved to S2, both S1 and Eden space are cleared. At this point, S2 contains objects with different ages.
    4. Eventually, a major garbage collection will be performed on the old generation which cleans up and compacts that space. For each major GC, there are several minor GCs.
- What is MetaSpace?
    
    [![](https://lh4.googleusercontent.com/GxgQmKE0fD_Z-pfSumb5dcAimEzEBpt9PdlmLfSyjmWO9qhLgzvuj7rIANMK93uecpUnyqZ2bcmKPT8BtzrFgBoVTqOSQNJGqO-ErvhMqlQnQ1Pik44TIz2N2IQNdWDBsKLfVLecPJthU8BWmhd1Ds1q_Ab87JtlfPI2-8-MlFDgFp6nJm7FCQc-oshb)](https://lh4.googleusercontent.com/GxgQmKE0fD_Z-pfSumb5dcAimEzEBpt9PdlmLfSyjmWO9qhLgzvuj7rIANMK93uecpUnyqZ2bcmKPT8BtzrFgBoVTqOSQNJGqO-ErvhMqlQnQ1Pik44TIz2N2IQNdWDBsKLfVLecPJthU8BWmhd1Ds1q_Ab87JtlfPI2-8-MlFDgFp6nJm7FCQc-oshb)
    
    Metaspace is a new memory space – starting from the Java 8 version; **it has replaced the older PermGen memory space**. The most significant difference is how it handles memory allocation.
    
    Specifically, **this native memory region grows automatically by default**.
    
    Additionally, the garbage collection process also gains some benefits from this change. The garbage collector now automatically triggers the cleaning of the dead classes once the class metadata usage reaches its maximum metaspace size.
    
    Therefore, **with this improvement, JVM reduces the chance to get the** _**OutOfMemory**_ **error**.
    
- How Do You Trigger Garbage Collection from Java Code?
    
    **You, as Java programmer, can not force garbage collection in Java**; it will only trigger if JVM thinks it needs a garbage collection based on Java heap size.
    
    There are methods like System.gc() and Runtime.gc() which is used to send request of Garbage collection to JVM but it’s not guaranteed that garbage collection will happen.
    
- What Happens When There Is Not Enough Heap Space to Accommodate Storage of New Objects?
    
    If there is no memory space for creating a new object in Heap, Java Virtual Machine throws _OutOfMemoryError_ or more specifically _**java.lang.OutOfMemoryError**_ **heap space.**
    
- Is It Possible to «Resurrect» an Object That Became Eligible for Garbage Collection?
    
    When an object becomes eligible for garbage collection, the GC has to run the _finalize_ method on it. The _finalize_ method is guaranteed to run only once, thus the GC flags the object as finalized and gives it a rest until the next cycle.
    
    In the _finalize_ method you can technically “resurrect” an object, for example, by assigning it to a _static_ field. The object would become alive again and non-eligible for garbage collection, so the GC would not collect it during the next cycle.
    
    The object, however, would be marked as finalized, so when it would become eligible again, the finalize method would not be called.
    
- Suppose We Have a Circular Reference (Two Objects That Reference Each Other). Could Such Pair of Objects Become Eligible for Garbage Collection and Why?
    
    Yes, a pair of objects with a circular reference can become eligible for garbage collection. This is because of how Java’s garbage collector handles circular references. It considers objects live not when they have any reference to them, but when they are reachable by navigating the object graph starting from some garbage collection root (a local variable of a live thread or a static field). If a pair of objects with a circular reference is not reachable from any root, it is considered eligible for garbage collection.
    
- How Are Strings Represented in Memory?
    
    A _String_ instance in Java is an object with two fields: a _char[] value_ field and an _int hash_ field. The _value_ field is an array of chars representing the string itself, and the _hash_ field contains the _hashCode_ of a string which is initialized with zero, calculated during the first _hashCode()_ call and cached ever since. Important thing is that a _String_ instance is immutable: you can’t get or modify the underlying _char[]_ array. Another feature of strings is that the static constant strings are loaded and cached in a string pool. If you have multiple identical _String_ objects in your source code, they are all represented by a single instance at runtime.
    
    A special space in which the strings created by "" are stored, it is in a heap.
    
    intern() - this method can return a reference to the corresponding string in the string pool
    
- Describe the process of memory allocation
    
    The Stack Memory allocation in java is used for static memory and thread execution. The values contained in this memory are temporary and limited to specific methods as they keep getting referenced in Last-In-First-Out fashion. In static memory allocation, we have to declare the variables before executing the program. Static memory is allocated during the compile time.
    
    Mainly used by java runtime, Java Heap Space comes into play every time an object is created and allocated in it. The discrete function, like Garbage Collection, keeps flushing the memory used by the previous objects that hold no reference. For an object created in the Heap Space can have free access across the application. Dynamic memory allocation in Java means that memory is allocated to Java objects during the run time or the execution time. It is contrary to static memory allocation. The dynamic memory allocation takes place in the heap space. The heap space is where new objects are always created and their references are stored in the stack memory.
    
- What is a stack overflow?
    
    specific type of runtime error that occurs when a program's call stack exceeds its allocated memory for the stack.
    
    The call stack is a data structure used by the program to keep track of function calls and local variables. When a function is called, its local variables and the return address of the calling function are pushed onto the stack. When the function finishes executing, the local variables and return address are popped off the stack, and the program returns to the calling function.
    
    However, if a program has deep or infinite recursion (a function calling itself repeatedly), or if it uses too much stack space with a large number of nested function calls and local variables, it can exhaust the available stack memory. When this happens, the program cannot allocate more space on the stack for new function calls, resulting in a stack overflow.
    
- Java Memory switches
    
    |   |   |
    |---|---|
    |**VM Switch**|**VM Switch Description**|
    |-Xms|For setting the initial heap size when JVM starts|
    |-Xmx|For setting the maximum heap size.|
    |-Xmn|For setting the size of the Young Generation, rest of the space goes for Old Generation.|
    |-XX:PermGen|For setting the initial size of the Permanent Generation memory|
    |-XX:MaxPermGen|For setting the maximum size of Perm Gen|
    |-XX:SurvivorRatio|For providing ratio of Eden space and Survivor Space, for example if Young Generation size is 10m and VM switch is -XX:SurvivorRatio=2 then 5m will be reserved for Eden Space and 2.5m each for both the Survivor spaces. The default value is 8.|
    |-XX:NewRatio|For providing ratio of old/new generation sizes. The default value is 2.|
    
- How to monitor Java Memory?
    
    **jstat**
    
    We can use `jstat` command line tool to monitor the JVM memory and garbage collection activities.
    
    The last argument for jstat is the time interval between each output, so it will print memory and garbage collection data every 1 second.
    
    - **S0C and S1C**: This column shows the current size of the Survivor0 and Survivor1 areas in KB.
    - **S0U and S1U**: This column shows the current usage of the Survivor0 and Survivor1 areas in KB. Notice that one of the survivor areas are empty all the time.
    - **EC and EU**: These columns show the current size and usage of Eden space in KB. Note that EU size is increasing and as soon as it crosses the EC, Minor GC is called and EU size is decreased.
    - **OC and OU**: These columns show the current size and current usage of Old generation in KB.
    - **PC and PU**: These columns show the current size and current usage of Perm Gen in KB.
    - **YGC and YGCT**: YGC column displays the number of GC event occurred in young generation. YGCT column displays the accumulated time for GC operations for Young generation. Notice that both of them are increased in the same row where EU value is dropped because of minor GC.
    - **FGC and FGCT**: FGC column displays the number of Full GC event occurred. FGCT column displays the accumulated time for Full GC operations. Notice that Full GC time is too high when compared to young generation GC timings.
    - **GCT**: This column displays the total accumulated time for GC operations. Notice that it’s sum of YGCT and FGCT column values.
    
    **Java VisualVM with Visual GC**
    
      
    
- Java GC Tuning
    
    **Java Garbage Collection Tuning** should be the last option you should use for increasing the throughput of your application and only when you see a drop in performance because of longer GC timings causing application timeout. If you see `java.lang.OutOfMemoryError: PermGen space` errors in logs, then try to monitor and increase the Perm Gen memory space using -XX:PermGen and -XX:MaxPermGen JVM options. You might also try using `-XX:+CMSClassUnloadingEnabled` and check how it’s performing with CMS Garbage collector. If you see a lot of Full GC operations, then you should try increasing Old generation memory space. Overall garbage collection tuning takes a lot of effort and time and there is no hard and fast rule for that. You would need to try different options and compare them to find out the best one suitable for your application. That’s all for Java Memory Model, Memory Management in Java and Garbage Collection, I hope it helps you in understanding JVM memory and garbage collection process.
    
- **What is the Java Classloader? List and explain the purpose of the three types of class loaders.**
    
    The [Java Classloader](https://docs.oracle.com/javase/7/docs/api/java/lang/ClassLoader.html) is the part of the Java runtime environment that loads classes on demand (lazy loading) into the JVM (Java Virtual Machine). Classes may be loaded from the local file system, a remote file system, or even the web.
    
    When the JVM is started, three class loaders are used:
    
    1. **Bootstrap Classloader:** Loads core java API file rt.jar from folder.
    
    2. **Extension Classloader:** Loads jar files from folder.
    
    3. **System/Application Classloader:** Loads jar files from path specified in the CLASSPATH environment variable.