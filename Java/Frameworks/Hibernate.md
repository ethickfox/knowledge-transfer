# **Hibernate**

Фреймворк (реализация jpa), упрощающий взаимодействие с бд посредствам ORM.

**ORM**

Технология для преобразования объекта в строку в таблице и наоборот.

**JPA**

спецификация, которая описывает систему управления и взаимодействия Java объектов с объектами из бд.

**EntityManager** - управляет сессией(связывает entity и бд)

**Java Standard Bean Validation Api** - спецификация правил валидации

**Hibernate Validator** - ее реализация

- @Size(min=, max=) - проверка длины строки
- @NotNull, @NotEmpty @NotBlank
- @Min, @Max - ограничение числовых значений
- @Pattern(regex=””) - проверка на регекс

**JPA Criteria API**

A simple query language for a REST API

**Specification**

Lets us pass in our own constraint to construct the actual query

**JpaSpecificationExecutor<T>**

Interface for enabling JPA Criteria for jpa repository

**Наследование**

[**https://www.baeldung.com/hibernate-inheritance**](https://www.baeldung.com/hibernate-inheritance)

- _MappedSuperclass_ – the parent classes, can't be entities
- Single Table – The entities from different classes with a common ancestor are placed in a single table.
- Joined Table – Each class has its table, and querying a subclass entity requires joining the tables.
- Table per Class – All the properties of a class are in its table, so no join is required.

**DAO** - вспомогательный объект для работы с бд

**ConnectionPool** - хранит подключения активными и закрывает, когда они становятся ненужными

Generation strategy

Entity Graph

## **Жизненный цикл объекта**

- **New или Transient (временный)** - не связан с EntityManagerCategory cat = new Category();cat.setTitle("new category");
- **Managed или Persistent** объект привязан к EntityManager и бдentityManager.persist(cat);
- **Commit** транзакция завершена, все сущности в контексте detachedentityManager.getTransaction().begin();entityManager.getTransaction().commit();
- Сущность изымаем из контекста, она становится **detached** - объекта нет в EntityManager, но он все еще существуетentityManager.detach(cat);
- Сущность из detached можно снова сделать **managed**
    
    Category managed = entityManager.merge(cat);
    
- И можно сделать **Removed**. Интересно, что cat всё равно detachedentityManager.remove(managed);

## **Уровни кэширования**

инвалидация кэша

горячие и холодные данные

[![](https://lh4.googleusercontent.com/LU77T68fhJOjkuPsAXKj4NW-LN8hqRrkWA2Gom2W2wdJGa77oZNoShsVIfX7cmmM3dhj05wCiVHhL8ahMESrl5yzSLnjT49IN-P-feVi-TKUW35Wyej9CVLayk1MYW1ECOfJ-EPMsETLMLli-i0NKDHIsgaGjJFfIWd52DzzAR1zu7m0OPiDzvRQG5xT)](https://lh4.googleusercontent.com/LU77T68fhJOjkuPsAXKj4NW-LN8hqRrkWA2Gom2W2wdJGa77oZNoShsVIfX7cmmM3dhj05wCiVHhL8ahMESrl5yzSLnjT49IN-P-feVi-TKUW35Wyej9CVLayk1MYW1ECOfJ-EPMsETLMLli-i0NKDHIsgaGjJFfIWd52DzzAR1zu7m0OPiDzvRQG5xT)

**Связи**

@OneToOne - необходимо писать, по какой колонке идет связь (@JoinColumn)

@OneTo Many - mapped by(с какой таблицей связан)

@ManyToOne - идут вместе, можно через них друг с другом связывать

@ManyToMany - указывается @JoinTable, тк связь идет уже через промежуточную таблицу в нем указываются joinColumn(колонка соответствующая данной таблице)  и inverseJoinColumn(колонска соответствующая связываемой таблице)

**Сессия** - используется для получения физического соединения с БД. Обычно, сессия создаётся при необходимости, а после этого закрывается. Это связано с тем, что эти объекты крайне легковесны. Чтобы понять, что это такое, можно сказать, что создание, чтение, изменение и удаление объектов происходит через объект Session.

SessionFactory - обычно создаётся в единственном экземпляре, при запуске приложения.

Transaction - объект представляет собой рабочую единицу работы с БД. В Hibernate транзакции обрабатываются менеджером транзакций.

## **N+1**

Hibernate N+1 problem occurs when you use FetchType.LAZY for your entity associations. If you perform a query to select n-entities and if you try to call any access method of your entity's lazy association, Hibernate will perform n-additional queries to load lazily fetched objects.

Default fetch type - lazy

## **Solving**

### **join fetch**

The first solution is to use **join fetch**:.createQuery("select a from Author a left join fetch a.books"

Join

When using `JOIN` against an entity associations, JPA will generate a JOIN between the parent entity and the child entity tables in the generated SQL statement.

So, taking your example, when executing this JPQL query:

```Plain
FROM Employee emp
JOIN emp.department dep
```

Hibernate is going to generate the following SQL statement:

```Plain
SELECT emp.*
FROM employee emp
JOIN department dep ON emp.department_id = dep.id
```

> Note that the SQL SELECT clause contains only the employee table columns, and not the department ones. To fetch the departmenttable columns, we need to use JOIN FETCH instead of JOIN.

join fetch

So, compared to `JOIN`, the `JOIN FETCH` allows you to project the joining table columns in the `SELECT` clause of the generated SQL statement.

So, in your example, when executing this JPQL query:

```Plain
FROM Employee emp
JOIN FETCH emp.department dep
```

Hibernate is going to generate the following SQL statement:

```Plain
SELECT emp.*, dept.*
FROM employee emp
JOIN department dep ON emp.department_id = dep.id
```

> Note that, this time, the department table columns are selected as well, not just the ones associated with the entity listed in the FROMJPQL clause.

Also, `JOIN FETCH` is a great way to address the [`LazyInitializationException`](https://vladmihalcea.com/the-best-way-to-handle-the-lazyinitializationexception/) when using Hibernate as you can initialize entity associations using the `FetchType.LAZY` fetching strategy along with the main entity you are fetching.

  

**FetchMode** : It defines `**how**` hibernate (using which strategy, e.g. Join, SubQuery etc) will fetch data from database.

**FetchType** : It defines `**whether**` hibernate will fetch the data or not.

`FetchMode` isn't only applicable with `FetchType.EAGER`. The rules are as follows: a) if you don't specify `FetchMode`, the default is `JOIN` and `FetchType` works normal, b) if you specify `FetchMode.JOIN` explicitly, `FetchType` is ignored and a query is always eager, c) if you specify `FetchMode.SELECT` or `FetchMode.SUBSELECT`, `FetchType.Type` works normal

### BatchSize

Another way is to use **@BatchSize** on the lazy association:@OneToMany(fetch = FetchType.LAZY, mappedBy = "author")

@BatchSize(size = 10)

### SubQuery

The third way is to use a **sub query** returning a list of author identifiers@Fetch(FetchMode.SUBSELECT)

### **NamedEntityGraph**

Using **NamedEntityGraph** Annotation

- Annotate the entity class with JPA’s @NamedEntityGraph annotation
- Attach the @EntityGraph annotation to the repository method, with the name of the graph.

  

  

First, we can use JPA's _@NamedEntityGraph_ annotation directly on our _Item_ entity:

```Plain
@Entity
@NamedEntityGraph(name = "Item.characteristics",
    attributeNodes = @NamedAttributeNode("characteristics")
)
publicclassItem {
	//...
}
```

And then, we can attach the _@EntityGraph_ annotation to one of our repository methods:

```Plain
publicinterfaceItemRepositoryextendsJpaRepository<Item, Long> {

    @EntityGraph(value = "Item.characteristics")
    ItemfindByName(String name);
}
```

As the code shows, we've passed the name of the entity graph, **which we've created earlier on the** _**Item**_ **entity,** to the _@EntityGraph_ annotation. When we call the method, that's the query Spring Data will use.

**The default value of the type argument of the** _**@EntityGraph**_ **annotation is** _**EntityGraphType.FETCH**_. When we use this, the Spring Data module will apply the _FetchType.EAGER_ strategy on the specified attribute nodes. And for others, the _FetchType.LAZY_ strategy will be applied.

So in our case, the _characteristics_ property will be loaded eagerly, even though the default fetch strategy of the _@OneToMany_ annotation is lazy.

One catch here is that **if the defined** _**fetch**_ **strategy is** _**EAGER,**_ **then we cannot change its behavior to** _**LAZY**__._ This is by design since the subsequent operations may need the eagerly fetched data at a later point during the execution.

### **Adhoc Entity Graph**

**Adhoc Entity Graph** on Repository interface - define an ad-hoc EntityGraph, using attributePaths, without using NamedEntityGraph annotation on the entity. AttributePaths should include the names of the entities to be fetched eagerly.

**Or, we can define an ad-hoc entity graph, too,** with _attributePaths._

Let's add an ad-hoc entity graph to our _CharacteristicsRepository_ that eagerly loads its _Item_ parent:

```Plain
public interface CharacteristicsRepository
extends JpaRepository<Characteristic, Long> {

@EntityGraph(attributePaths = {"item"})
Characteristic findByType(String type);
}
```

### _LazyInitializationException_

Hibernate throws the _LazyInitializationException_ when it needs to initialize a lazily fetched association to another entity without an active session context. That’s usually the case if you try to use an uninitialized association in your client application or web layer.

Here you can see a test case with a simplified example.

|   |   |
|---|---|
|123456789101112131415|`EntityManager em = emf.createEntityManager();``em.getTransaction().begin();` `TypedQuery<Author> q = em.createQuery(`        `"SELECT a FROM Author a"``,`        `Author.``**class**``);``List<Author> authors = q.getResultList();``em.getTransaction().commit();``em.close();` `**for**` `(Author author : authors) {`    `List<Book> books = author.getBooks();`    `log.info(``"... the next line will throw LazyInitializationException ..."``);`    `books.size();``}`|

The database query returns an _Author_ entity with a lazily fetched association to the books this author has written. Hibernate initializes the _books_ attributes with its own _List_ implementation, which handles the lazy loading. When you try to access an element in that _List_ or call a method that operates on its elements, Hibernate’s _List_ implementation recognizes that no active session is available and throws a _LazyInitializationException_.