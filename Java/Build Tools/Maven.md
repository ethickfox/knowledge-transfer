---
Interview graded: true
Last edited time: 2024-02-19T11:18
Needs Rework: false
Status: Not started
Topic:
  - "[[Java Build Tools]]"
---
## **Maven**

Фреймворк для автоматизации сборки проектов на основе описания их структуры в файлах на языке POM,

**Зависимости**

В maven есть 2 типа зависимостей:

- Прямые - зависимости прописанные в pom.xml в теге <dependencies/>
- Транзитивные зависимости - зависимости, которые наобходимы для зависимостей (A --> B --> C).

### **POM**

Project Object Model - основной файл конфигурации в Maven. Он содержит всю необходимую для сборки проекта информацию.

Все pom файлы можно разделить на 3 типа:

**Super POM**

Основной файл, от которого наследуются все pom файлы(По аналогии с object в Java). Он задает дефолтные значения для таких элементов конфигурации как:

- Репозитории зависимостей
- Репозитории плагинов
- Сборка - имя директории, финального артефакта и тд.
- Профили - (с каким профилем, какие плагины, какой билд и тд)

**Simplest POM**

Самый простой пом, который можно создать. Он должен состоять из версии мавена, groupId, artifactId, версии проекта

**Effective POM**

Является сочетанием дефолтных настроек из super POM и конфигурации заданной для приложения. Можно получить через mvn help:effective-pom

**Жизненный цикл**

- Валидация - проверка корректности необходимой для сборки информации(в том числе загрузка зависимостей)
- Компиляция кода
- Компиляция тестов
- Прогон тестов
- упаковка кода
- Интеграционные тесты
- установка пакета в локальный репозиторий
- копирование в удаленный репозиторий

### BOM

BOM stands for Bill Of Materials. **A BOM is a special kind of POM that is used to control the versions of a project’s dependencies and provide a central place to define and update those versions.**

BOM provides the flexibility to add a dependency to our module without worrying about the version that we should depend on.

**Артефакт**

Результирующая программа, полученная сборкой с помощью maven. Хранится в репозитории

**Репозиторий**

Хранилище всех зависимостей(в порядке поиска зависимостей)

- Local - директория на на компьютере, создается при первом запуске maven
- Central - основной репозиторий, обеспечиваемый сообществом Maven
- Remote - кастомный удаленный репозиторий, который можно использовать, если нет зависимости в Central

**Scope**

- test - нужен только в тестах
- **compile** только этап компиляции, включается в сборку, дефолтный
- provided - используется только на этапе компиляции, в сборку не включается
- runtime - не нужен для компиляции, будет использоваться только во время работы приложения
- system - как provided, но путь указывается явно

**Archetype**

Шаблон вашего будущего проекта или, цитируя официальную документацию: «архетип есть модель по которой делаются все остальные вещи такого рода

**Dependency hell**

Антипаттерн управления зависимостями на проекте, при котором усложняется граф зависимостей библиотек и возникает ситуация, когда библиотеки требуют разные версии одной и той же зависимости. Или одна библиотека может косвенно потребовать несколько версий одной и той же зависимости.

The conflict here comes when 2 dependencies refer to different versions of a specific artifact. Which one will be included by Maven?

**The answer here is the “nearest definition”. This means that the version used will be the closest one to our project in the tree of dependencies. This is called dependency mediation.**

Let's see the following example to clarify the dependency mediation:

```Plain
A -> B -> C -> D 1.4  and  A -> E -> D 1.0
```

This example shows that project _A_ depends on _B_ and _E._ _B_ and _E_ have their own dependencies which encounter different versions of the _D_ artifact. Artifact _D_ 1.0 will be used in the build of _A_ project because the path through _E_ is shorter.

There are different techniques to determine which version of the artifacts should be included:

- We can always guarantee a version by declaring it explicitly in our project's POM. For instance, to guarantee that _D_ 1.4 is used, we should add it explicitly as a dependency in the _pom.xml_ file.
- We can use the _Dependency Management_ section to control artifact versions as we will explain later in this article.

Как исправить?

- Удаление дублирующихся зависимостей mvn dependency:analyze-duplicate
- Удаление неиспользуемых записанных зависимостей mvn dependency:analyze
- Добавление используемых не записанных зависимостей mvn dependency:analyze
- Нахождение нескольких версий одной библиотеки, поиск мест, где используются эти версии и обновление их до одной
- Дублирующиеся классы - в разных библиотеках есть классы со схожей реализацией tattletale-maven

**DependencyManagement**

Simply put, Dependency Management is a mechanism to centralize the dependency information.

When we have a set of projects that inherit a common parent, we can put all dependency information in a shared POM file called BOM.

```XML
<project ...>
	
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Baeldung-BOM</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>BaelDung-BOM</name>
    <description>parent pom</description>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>test</groupId>
                <artifactId>a</artifactId>
                <version>1.2</version>
            </dependency>
            <dependency>
                <groupId>test</groupId>
                <artifactId>b</artifactId>
                <version>1.0</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>test</groupId>
                <artifactId>c</artifactId>
                <version>1.0</version>
                <scope>compile</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

As we can see, the BOM is a normal POM file with a _dependencyManagement_ section where we can include all an artifact's information and versions.

There are 2 ways to use the previous BOM file in our project and then we will be ready to declare our dependencies without having to worry about version numbers.

We can inherit from the parent:

```XML
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>Test</name>
    <parent>
        <groupId>baeldung</groupId>
        <artifactId>Baeldung-BOM</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
</project>
```

We can also import the BOM.

```XML
<project ...>
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>Test</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>Test</name>
    
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>baeldung</groupId>
                <artifactId>Baeldung-BOM</artifactId>
                <version>0.0.1-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

The order of precedence of the artifact's version is:

1. The version of the artifact's direct declaration in our project pom
2. The version of the artifact in the parent project
3. The version in the imported pom, taking into consideration the order of importing files
4. dependency mediation

- We can overwrite the artifact's version by explicitly defining the artifact in our project's pom with the desired version
- If the same artifact is defined with different versions in 2 imported BOMs, then the version in the BOM file that was declared first will win

  

**Dependency vs** **DependencyManagement**

- Артефакты указанные в dependencies будут включены в качестве зависимости дочерних модулей
- Артефакты указанные в dependencyManagement будут включены в дочерние модули только в случае если они будут прописаны в dependencies дочернего модуля. Это помогает унифицировать версии для всех модулей