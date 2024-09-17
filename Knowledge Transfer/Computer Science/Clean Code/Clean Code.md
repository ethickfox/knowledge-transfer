---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Planned
Topic:
  - "[[Knowledge Transfer/Computer Science/Development/Clean Code\\|Clean Code]]"
---
# **Code quality**

Identifying and estimating code quality requires you to consider two aspects of code: what it **does** and how it **looks**.

- **Functional Code Quality  
      
    **Functional code quality means following or meeting functional requirements. It’s about what the code does. Activities that ensure functional code quality include unit testing and functional testing.
- **Structural Code Quality  
      
    **Structural code quality means adhering to project-specific guidelines, minimizing unnecessary details, and maintaining clean code. It’s about how the code looks.  
    Activities that ensure structural code quality include static code analysis and code review.  
    

**Charaсteristics of Structural Code Quality**

- **Clear  
      
    **Code isn’t only for the person who creates it. Other developers on your team will need to read or edit the code. If you are the code author, it can be beneficial to ask a fellow developer to take a brief look at your code and answer the question, “Is my code easy to understand and edit?” While your code might seem understandable to you, it’s important to keep other contributors in mind to ensure the project goes smoothly. Maintaining and extending code is simpler when it is easy to understand.
- **Easy to Maintain  
      
    **Anyone who works with your team’s code needs to understand it in order to make changes. When a single modification requires a lot of time because of the way the code was initially developed, that’s typically a sign that the code has not been created with maintainability in mind. When code is overly complicated, simple changes can result in a total rework of the code. High quality code is simple and clean, which makes it easy for others to edit and maintain it.
- **Follows Project guidelines  
      
    **Project guidelines help ensure consistency between multiple developers, making edits or updates much easier—especially if one developer needs to work with another developer’s code. Keeping everyone on the same page saves time and resources for any project. These guidelines should be created at the start of the project and kept in a place where your team can easily find and use them.
- **Testable  
      
    **Testable code is code that's written so that it can be verified independently. Developers should be able to write automated tests that demonstrate the behaviors they want to verify. Such code should consist of small, functionally independent behaviors that make up a system. Otherwise, a task to cover code with readable tests may become difficult or even impossible.

![[/Untitled 40.png|Untitled 40.png]]

**Establishing Coding Standards**

By incorporating coding standards, your team takes steps to:

- Enhance code clarity
- Increase code readability and consistency
- Support better maintainability and control
- Reduce software development risks
- Reduce complexity

Coding standards usually consist of three major elements: style guides, code principles, and project-specific code conventions. Each of these three parts includes key pieces of information that impact the success of a project.

- **Style Guide**  
    A style guide communicates a visual representation of the source code and depends on the programming language. It includes:  
    - The layout of the source code
    - Indentation
    - Use of white space around operators and keywords
    - Capitalization of keywords and variable names
    - Style and spelling of user-defined identifiers, such as function, procedure, and variable names
    - Style of comments
- **Coding principles**  
    Coding principles establish source code structure to ensure code’s readability and modifiability. They include:  
    - Language constructions usage (e.g. exception handling, goto/break usage, etc.)
    - Logical code structure (Size of the method, number of parameters, naming principles, etc.)
    - Design principles (**SOLID**, Patterns, **KISS**, etc.)
- **Project specific code conventions  
      
    **Project specific code conventions extend—and sometimes override—well-known styles and practices. They explain project-specific implementation details that act as educational materials for project onboarding. They include:
    - Extended global style guides and coding principles
    - Guidelines about how to implement project specific features (e.g., implement new service, transactions rules, logging approach, etc.)
    - Naming patterns for certain units, modules, and classes
    - Any other FAQs or DOs and DON'Ts that helps developers write code in a consistent way

**Implementing Automated Code Analysis**

Automated code analysis is a tool for achieving structural code quality and enforcing coding standards. It analyzes the program code against a predefined set of rules and best practices via a fully automated process.

SonarQube (formerly Sonar) is an open source platform for continuous inspection of code quality that delivers one of the best static code analyzers on the market for Java, C#, C/C++, and many other languages.

SonarQube provides very deep code analysis, allowing it to calculate and manage the value of technical debt. In addition to SonarQube, there are other useful tools for automated code analysis.

  

**Code Quality Metrics**

Сode quality metrics are a set of software metrics that help developers better understand the code they're writing. You can use code metrics to figure out issues in code promptly. Throughout the development process, you and your team can detect potential hazards, assess the present state of the project, and track progress. The following quality metrics will help with this:

- **Cyclomatic complexity  
      
    **Cyclomatic complexity is a quality metric that measures the structural complexity of code. It refers to the number of decisions in the source code. The higher the cyclomatic complexity, the more complex the code
- Class coupling  
      
    **Class coupling** measures how many classes a single class uses. The lower the value, the better. Good software design requires low coupling; designs with high coupling are difficult to reuse and maintain.
- **Class hierarchy, or a depth of inheritance tree (DIT)**  
    Class hierarchy, or a depth of inheritance tree (DIT), measures the complexity of the class hierarchy in object-oriented programming. It shows how object classes are derived from other classes. The higher the DIT, the more complex the software.  
    
- **Code duplication  
      
    **Code duplication refers to a sequence of source code that appears more than once, either within a program or across different programs. Duplicate code is considered undesirable as it is more difficult to maintain. If such code requires updating, there is a risk that one copy of the code will be modified without checking for other instances of the same code sequence.
- **Method cohesion**  
    Method cohesion has to do with methods written within a class. A method should explicitly state why it is written. Otherwise, it lacks cohesion. High cohesion means that methods of the class are codependent and form a logical whole, whereas low cohesion results in huge classes that are difficult to understand and maintain.  
    

## **Naming Coding Elements**

**Length Rules**

- **Methods and Classes, Short Names  
      
    **The longer the scope of a function, the shorter its name should be. The longest function names should be given to those functions that are called from just one place. Public API functions or classes usually have long scopes so we should have short and well-defined names assigned to them.
- **Methods and Classes, Long Names  
      
    **When functions have a shorter scope or if it is only used once, using long descriptive names will help readability. Functions that are called locally from a few nearby places should have long descriptive names, and the longest function names should be given to those functions that are called from just one place. Private functions and Private classes have shorter scopes, so we want to have long and descriptive names used for them.
- **Variables and Parameters, Long Names  
      
    **When it comes to naming variables and parameters it’s the inverse of the scope law of methods and classes. The length of a name should correspond to the size of its scope. For example, global variables should have long and self-descriptive names.  
    In case of Instance variables or even for constants, since they have longer scopes, we should use descriptive names as shown below.  
    
- **Variables and Parameters, Short Names  
      
    **Local variables of a short method or small block should have short names. Names can be as short as single letters; however, these should only be used in certain circumstances:
    - Counter variables for simple for loops
    - Exception instances in Catch blocks
    - Arguments of very short functions

**Grammar Rules**

- **Class**
    - Use noun phrase names
    - Don’t use verbs
- **Methods**
    - Use verb phrase names
- **Solution domain**
    
    Use programming terms such as:
    
    - Computer Science terms
    - Algorithm names
    - Design Pattern names
- **Problem Domains**
    - Use simple terms that clearly identify the problem
    - Avoid programmer language

  

  

  

  

- Имя должно отвечать на вопрос, почему данная конструкция существует, что она делает, и как используется
- Если имя требует комментария, значит оно не подходит
- Не используйте названия, которые могут иметь несколько значений
- Разные вещи должны иметь значимые различия в названиях
- Используйте легко произносимые имена
- Используйте имена, которые можно легко найти
- Имя класса - существительное
- Имя метода - глагол
- Имя метода или класса - чем чаще используется, тем короче название

**Методы**

**Общее**

- Методы должны быть небольшими(20 строк) и извлекаться до тех пор пока это возможно
- Никаких вложенных структур, внутри if, else, while
- У метода должна быть только одна задача
- Не более одного уровня абстракции
- Никаких побочных эффектов, метод не должен обещать одно, а делать другое
- Методы либо должны что-то делать - изменять состояние(команда), либо отвечать на что-то(запрос), но он не может делать все вместе. Они либо изменяют состояние объекта, либо возвращают некоторую информацию об объекте, но не то и другое вместе.

**Параметры**

- В идеале два параметра, максимум три
- Нет логических параметров
- Не передавайте в аргументы null. Если необходимо, имя метода должно показывать это
- Избегайте выходных аргументов

**Побочные эффекты**

- Побочный эффект присутствует, если при вызове метода изменяется состояние, и возможно получение разных результатов после каждого вызова
- Обычно функции с побочными эффектами имеют парную функцию. - Начать и зафиксировать - Открыть и закрыть - Начать процесс и присоединиться или завершить - Создать и удалить
- Мы контролируем временную привязку, инкапсулируя эти операции в одном методе.

**Общие принципы**

- Обычно комментарии - плохо
- Никаких версионных комментариев
- KISS
- DRY
- Лучше использовать непроверенные исключения
- Используйте исключения, а не коды возврата
- Ошибки лучше обрабатывать там, где они возникают
- Использование анализаторов кода

PMD

Checkstyle

Метрики качества кода

|   |   |   |   |
|---|---|---|---|
|Название|Определение|Метрика|Tool|
|Цикломатическая сложность|Число линейно независимых путей в секции кода.|>10 - refactor|SonarQube|
|Отсутствие согласованности в методах|Мера связанности между методами и переменными экземпляра класса: количество методов, которые не обращаются к определенному полю данных / всем полям данных в классе.|Высокий cohesion (и соответственно низкий LCOM) - хорошая организация класса||
|Связь между объектами|Число классов, имеющих связь с данным.|Меньше - лучше|MetricsReloaded IntelliJ plugin|
|Code coverage|Процент кода вызываемого в тестоах|Больше - лучше|SonarQube|
|Unit test success percentage|Процент успешно пройденных юнит-тестов.|Должно быть 100%|SonarQube|
|Duplicate code percentage|Процент задвоения кода во всём коде приложения.|Меньше - лучше|SonarQube|
|Technical debt|Места, которые реализованы в крткосрочной перспективе, но требуют реализации с помощью лучшего решения|Меньше - лучше|SonarQube|
|Comment density|строки комментариев / (строки кода + строки комментариев) * 100.|Меньше - лучше|SonarQube|
|Static code rules compliance|Число, которое показывает, в какой степени используются практики чистого кода|Больше - лучше|SonarQube|

## **Code smells**

**Дублирование кода** - использование одинаковых структур кода в разных местах. Решение

- Выделение метода
- Подъем поля
- Формирование шаблона метода - если код похож, но не совпадает полностью
- Выделение класса

**Большой класс** - класс реализует слишком большую функциональность.

**Длинный метод** - если есть необходимость что-то прокомментировать - это нужно вынести в отдельный метод.

**Длинный список параметров** - можно инкапсулировать часть параметров в одном объекте

**Завистливые данные** - метод обращается к данным другого объекта чаще, чем к своим. Чтобы исправить, надо хранить данные  и методы использующие эти данные в одном месте.

**Цепочка вызовов** - появляется тогда, когда клиент запрашивает у одного объекта другой объект, другой объект запрашивает ещё один объект и т. д. Такие последовательности вызовов означают, что клиент связан с навигацией по структуре классов. Любые изменения промежуточных связей означают необходимость модификации клиента.

**Комментарии -** в большинстве случаев они нужны чтобы скрыть плохой код.

## **Ревью кода**

Анализ (инспекция) кода с целью выявить ошибки, недочеты, расхождения в стиле написания кода, в соответствии написанного кода и поставленной задачи.

**Зачем нужен?**

- Делится ответственность за задачу
- Передача знаний - не только тот, кто пишет код знает, как он работает
- Поиск багов
- Изучение новых подходов к программированию
- Проверка соответствия архитектуре
- Проверка соблюдения правил принятых в команде
- Корректность решения
- Тестируемость кода и наличие тестов

**Как проводится?**

- Определение требуемой задачи
- Проверка соответствия задачи и решения
- Проверка читаемости кода
- Проверка поддерживаемости кода
- Проверка доступных для улучшения точек кода
- Проверка наличия юнит тестов
- Проверка соответствия юнит тестов логике требуемой задачи и реализованного функционала