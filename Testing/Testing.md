1. [Metrics](Testing/Metrics.md)
2. [Quality Assurance](Testing/Quality_Assurance.md)
3. [Automated](Testing/Automated.md)
4. [Testcase Management](Testing/Testcase_Management.md)
5. [Testing NonFunctional Requirements](Testing/Testing_NonFunctional_Requirements.md)
6. [Unit Testing](Testing/Unit_Testing/Unit_Testing.md)
## **Testing pyramid**
Visual mythaphor for understanding that more isolated tests are faster and "cheaper", so they should be used more than slover services tests which should be used more than even mslower UI tests.

[![](https://lh5.googleusercontent.com/hbuKkHKntbWkUd39AFTSVV6nTtvN2sE9IxO3xwjxvIVPgwOBJ-qZkcgAb0ojCLVHgt4JAVfzMgZibHsuLMBzjNITawFsUkJYKTP7asV7BA50qh1Ec-fKTlumT6zr6vnoj3ZTdMjSUJu33RVnCl3EJ0A77KhxwSmZ5L1WnZpJfbRso2Om6nh5EPblGeLn)](https://lh5.googleusercontent.com/hbuKkHKntbWkUd39AFTSVV6nTtvN2sE9IxO3xwjxvIVPgwOBJ-qZkcgAb0ojCLVHgt4JAVfzMgZibHsuLMBzjNITawFsUkJYKTP7asV7BA50qh1Ec-fKTlumT6zr6vnoj3ZTdMjSUJu33RVnCl3EJ0A77KhxwSmZ5L1WnZpJfbRso2Om6nh5EPblGeLn)

Оригинальная пирамида тестов состоит из трёх уровней:

- [Unit_Testing](Unit_Testing/Unit_Testing.md)
- [Integrational Testing](Integrational%20Testing)
- [Functional Testing](Functional%20Testing)

Из этой пирамиды главное запомнить два принципа:

- Писать тесты разной детализации.
- Чем выше уровень, тем меньше тестов.

## **Юнит тесты**

Тестируют отдельные модули или компоненты программного обеспечения(тестирование белого ящика) Должны составлять основную часть авто тестов. Цель состоит в том, чтобы проверить, что каждая единица программного кода работает должным образом. В идеале должны проверять каждый возможный блок кода, со всеми ветвлениями и тд. Фреймворки - Junit, Mockito.

## **Интеграционные тесты**

Тестирование группы взаимодействующих модулей(тестирование черного ящика) - mockMvc

### **Types of integration testing**

There are several different types and approaches for integration testing:

- **Top-down testing**: As the name says, you need to start with the highest priority module and go to the lower module in the testing.
- **Bottom-up testing**: When lower modules depend on the higher module, a reverse approach is preferable.
- **Big bang testing**: Here, all the modules and components of the software system are integrated and tested as a whole.
- **Sandwich testing**: It’s a combination of a top-down approach and a bottom-up approach. The testing starts from both ends and moves toward the middle-level components.
- **Agile testing**: This is a common approach when the product is already out. The testing is performed throughout the development process. Usually, the developer runs the test cases as part of the regression testing step to ensure the new code doesn’t break the existing code.
- **Continuous integration testing**: Also known as continuous testing, it’s a method where you execute the integration tests as part of the Continuous Integration/Continuous Deployment (CI/CD) pipeline.

The integration test suite can check various things:

- Testing of the integration points between different modules or components.
- Data flow verification in all the modules.
- Testing of the interactions between different systems (such as databases, servers, and third-party APIs)
- Validation of interfaces and APIs.
- Error handling and fault tolerance mechanisms.
- Performance testing (load testing) to find out bottlenecks.

## **Функциональные тесты**

Тестирование системы в целом.(Selenium) должны размещаться на вершине пирамиды. Большая часть вашего кода и бизнес-логики должна быть уже протестирована до этого уровня;

API tests include testing of functionality, reliability, performance, and security of APIs. Depending on the approach, you can carry out API testing at various stages and in different ways:

- Manual testing – QA specialist uses an API testing tool to manually check the API endpoints’ functionality.
- Automated testing – Developer writes and runs the API tests during development and runs them locally.
- Continuous testing – API tests are executed automatically in a CI/CD pipeline.

### **Methods of API testing**

API testing has various methods. Here are some of the top methods:

- **Functional testing**: You check that each API endpoint produces the expected response based on a given request.
- **Load testing**: Applicable when there are a huge number of API requests. Load testing ensures that the app is stable even if there are a significant amount of queries.
- **Fuzz testing**: In fuzz testing, a random fuzz (invalid or random data) is given to all the APIs to test the error handling and uncover any security issues.
- **Security testing**: Authentication and authorization mechanisms should work properly. Security testing ensures the same.
- **Regression testing**: It’s the process of testing the APIs every time there is a new change. Testers and developers use it when the app is in production.

## **Юнит тесты vs Интеграционные**

Интеграционные проверяют взаимодействие компонентов, а юнит - компоненты  по отдельности

  

### Integration vs API

Even though they are not the same thing, API testing is a type of integration testing.

However, it is easy to confuse them as the core working of both tests is quite similar. Both tests ensure that different components integrate with the software and function properly.

But here are some key differences.

|   |   |
|---|---|
|**API Testing**|**Integration Testing**|
|Tests the functionality and performance of APIs.|Tests the interaction between different components of an application.|
|Automated testing performed early in the development cycle.|Testing performed after unit testing and before system testing.|
|Focuses on the behavior of the API for a given input.|Focuses on how different modules or services interact with each other.|
|Typically tests individual APIs in isolation.|Tests the parts of the system. It can check API, database, business logic, and integration with external resources.|
|Only usable for testing APIs that are part of a larger system.|Can be used for testing a wide range of applications, including desktop, web, and mobile applications.|

## **Нагрузочное тестирование**

Позволяет проверить такие нефункциональные требования к системе, как производительность, стабильность, масштабируемость, стрессо- и отказоустойчивость.

## **Ручное функциональное тестирование**

Разобраться в документации и функциональности тестируемого продукта, составление и выполнение тестовых сценариев

## **Сервисное тестирование**

## **Смоук Тесты**

Smoke Testing is done whenever the new functionalities of software are developed and integrated with existing build that is deployed in QA/staging environment. It ensures that all critical functionalities are working correctly or not.

  

|   |   |
|---|---|
|Smoke Testing|Regression Testing|
|Smoke Testing Is Used To Check If A Build Of The Software Is Stable Enough For It To Be Tested.|Regression Testing Is Used To Verify If Any Recent Changes Have Impacted The Existing Functionality.|
|It Is Employed By Both Software Testers And Developers.|It Is Predominantly Used By Software Testers Alone.|
|It Is A Surface-Level Type Of Testing As It Is Focused Only On Certain Modules And Performed On Initial & Unverified Builds.|It Is A More In-Depth Type Of Testing As It Has An End-To-End Approach And Is Performed Only On Stable Builds.|
|Smoke Testing Is Always Done Before Regression Testing.|Regression Testing Is Performed During Various Phases Of Testing.|
|It Doesn’t Require Much Time Or Resources To Be Done.|It Requires A Lot Of Time, Resources, And Effort To Be Done.|
|It Is Purely An Acceptance Type Of Testing As It Validates The Build And Prevents Further Testing If It Doesn’t Pass.|Though Functionalities Are Tested, It Is Not A Purely Acceptance-Based Type Of Testing As Any Issues Found In This Phase Don’t Prevent The Application From Being Further Tested.|
|Smoke Testing Definitely Requires Documentation|Regression Testing May Or May Not Require Documentation Based On The Need.|

### Sanity testing

The main purpose of Sanity testing is to verify that the changes or the proposed functionality are working according to plan. Suppose there are minor changes to be made to the code, the sanity test further checks if the end-to-end testing of the build can be performed seamlessly. However, if the test fails, the testing team rejects the software build, thereby saving both time and money.

- Smoke test is done to make sure that the critical functionalities of the program are working fine, whereas sanity testing is done to check that newly added functionalities, bugs, etc., have been fixed.
- The software build may be either stable or unstable during smoke testing. The software build is relatively stable at the time of sanity testing.

### Functional testing

Functional testing is **a type of testing that seeks to establish whether each application feature works as per the software requirements**. Each function is compared to the corresponding requirement to ascertain whether its output is consistent with the end user's expectations.

### End-to-end (E2E)

Testing is a [software testing](https://www.techtarget.com/whatis/definition/software-testing) methodology that verifies the working order of a software product in a start-to-finish process. End-to-end testing verifies that all components of a system can run under real-world scenarios.

End-to-end testing is typically performed by quality assurance ([QA](https://www.techtarget.com/searchsoftwarequality/definition/quality-assurance)) teams, and are executed in dedicated test environments. This normally takes place after [functional](https://www.techtarget.com/searchsoftwarequality/definition/functional-testing) and [system testing](https://www.techtarget.com/searchsoftwarequality/definition/system-testing). End-to-end testing starts from the user's perspective, simulating typical operations the application can perform.

There are two types of end-to-end testing: horizontal and vertical.

Horizontal end-to-end testing is the most used and well-known approach. Horizontal testing can build confidence in a system by assuming the perspective of a user. Horizontal testing confirms whether a user can navigate through a system, if it works as expected and if there are any unexpected [bugs](https://www.techtarget.com/searchsoftwarequality/definition/bug) or exceptions.

Vertical end-to-end testing refers to testing in layers, meaning tests happen in a sequential, hierarchal order. Each component of a system or product is tested from start to finish ensuring quality. Vertical testing is frequently used to test critical components of a complex computing system that do not typically involve users or interfaces. A layer without a user interface ([UI](https://www.techtarget.com/searchapparchitecture/definition/user-interface-UI)) would benefit from vertical end-to-end testing over horizontal.

## **FIRST**

- **Fast**
- **Isolated/Independent** - тесты не должны зависить друг от друга
- **Repeatable** - каждый раз при выполнении теста с одними и теми же данными, результат должен быть один и тот же
- **Self-validating** - тест сам должен давать ответ, на то, прошел ли он корректно или нет(а не выводить какую-то информацию в sout, чтобы ее потом искали)
- **T**
    - **Thorough**(тщательный) - должна покрываться вся кодовая база, должен охватываться каждый возможный случай
    - **Timely** - Since tests help us to design testable production code, we cannot wait for our code to be ready for production before writing them.

## **Test double**

Test Double is a generic term for any case where you replace a production object for testing purposes. There are various kinds of double:

- **Dummy** objects are passed around but never actually used. Usually, they are just used to fill parameter lists.
- **Fake** objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an InMemoryTestDatabase is a good example).
- **Stubs** provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
- **Spies** are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.

**Mocks**

are pre-programmed with expectations which form a specification of the calls they are expected to receive. They can throw an exception if they receive a call they don't expect and are checked during verification to ensure they got all the calls they were expecting.
