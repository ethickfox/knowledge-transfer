# Generics
 Special Java language tools for implementing generalized programming: a specific approach to describing data and algorithms, allowing you to work with different types of data without changing their description. Generic type parameter is when a _type_ can be used as a parameter in a class, method or interface declaration.
- As generics did not exist before Java 5, it is possible not to use them at all. For example, generics were retrofitted to most of the standard Java classes such as collections.
	- Despite being able to compile, it’s still likely that there will be a warning from the compiler. This is because we are losing the extra compile-time check that we get from using generics.
	- The point to remember is that **while backward compatibility and type erasure make it possible to omit generic types, it is bad practice.**
- One advantage of using generics is avoiding casts and provide type safety. This is particularly useful when working with collections. By using generics we have a compile type check which prevents _ClassCastExceptions_ and removes the need for casting.
- The other advantage is to avoid code duplication. Without generics, we have to copy and paste the same code but for different types. With generics, we do not have to do this. We can even implement algorithms that apply to generic types.
# Type Erasure
It’s important to realise that generic type information is only available to the compiler, not the JVM. In other words**,** type erasure means that **generic type information is not available to the JVM at runtime**, only compile time.
Types can be divided into:
- Reifiable-type is a type that is fully available at runtime.
	- A primitive type (such as int)
	- A nonparameterized class or interface type (such as Number, String, or Runnable)
	- A parameterized type in which all type arguments are unbounded wildcards (such as List`<?>, ArrayList<?>, or Map<?, ?>`)
	- A raw type (such as List, ArrayList, or Map)
	- An array whose component type is reifiable(such as `int[], Number[], List<?>[], List[], or int[][]`)
- Non-reifiable Types are types whose information is erased and becomes unavailable during execution (Generics) The use of this type of arrays is prohibited they are aware of the type used in runtime it is to ensure type security then in Object[] you could put the` List<String>[]` and Heap polution would occur.
	- A type variable(such as T)
	- A parameterised type with actual parameters (such as `List<Number>`, `ArrayList<String>, or Map<String, Integer>`)
	- A parameterised type with a bound (such as `List<? extends Number> or Comparable<? super String>`)

The reasoning behind major implementation choice is simple – preserving backward compatibility with older versions of Java. When a generic code is compiled into bytecode, it will be as if the generic type never existed. This means that the compilation will:
1. Replace generic types with objects
2. Replace bounded types with the first bound class
3. Insert the equivalent of casts when retrieving generic objects.
4. генерирует Bridge методы для сохранения полиморфизма

|                                                      |                         |
| ---------------------------------------------------- | ----------------------- |
| T (Тип)                                              | \|T\| (Затирание типа)  |
| `List<Integer>, List< String>, List< List< String>>` | _List_                  |
| `List<Integer>[]`                                    | _List[]_                |
| _List_                                               | _List_                  |
| _int_                                                | _int_                   |
| _Integer_                                            | _Integer_               |
| `<T extends Comparable<T>>`                          | _Comparable_            |
| `<T extends Object & Comparable<? super T>>`         | _Object_                |
| `LinkedCollection<E>.Node`                           | _LinkedCollection.Node_ |
Bridge методы
Создадим класс с наследованием от дженерика
```java
public class ErasureTest implements Comparable<ErasureTest> {
	@Override
	public int compareTo(ErasureTest o) {
		return 0;
	}
	
	public int compareTo(Object o) {
		return 0;
	}
}
```
Возникает compile-time error создается метод compareTo(Object o), а он уже есть.
Оба метода останутся

There is one situation where a generic type is available at runtime. This is when a generic type is part of the class signature like so:

```java
public class CatCage implements Cage<Cat>

(Class<T>)((ParameterizedType)getClass().getGenericSuperclass()).getActualTypeArguments()[0];
```

## Wildcard
Implementation of polymorphism for generics. Because if B is the successor of A, `Collection<B>` is not the successor of `Collection<A>`. `Collection<?>` Can accept any of the types. **A wildcard type represents an unknown** _**type**_.
### Upper Bounded wildcards
**An upper bounded wildcard is when a wildcard type inherits from a concrete type**. Wildcard with the specified upper border (`List<? extends Number>`) Allows you to read from the structure:
```Java
void printListSum(List<? extends Number> list) {
	for (Number number : list)
		System.out.println(number.intValue());
}

printListSum(new ArrayList<Integer>());
printListSum(new ArrayList<Double>());
printListSum(new ArrayList<String>()); // --
printListSum(new ArrayList<Object>()); // --
printListSum(new ArrayList<ArrayList<String>>()); // --
```

![](Java/Core/_img/Pasted%20image%2020250520210730.png)

Wildcard с указанной верхней границей  (`List<? extends Number>)` Позволяет читать из структуры:

```Java
void printListSum(List<? extends Number> list) {
	for (Number number : list) {
	    System.out.println(number.intValue());
	}
}

printListSum(new ArrayList<Integer>());
printListSum(new ArrayList<Double>());
printListSum(new ArrayList<String>()); // --printListSum(new ArrayList<Object>()); // --
printListSum(new ArrayList<ArrayList<String>>()); // --
```
### Lower Bounded wildcards
Lower bounded wildcard means we are forcing the type to be a superclass of our bounded type.Wildcard with the specified lower border (`List<? supper Number>`) Allows you to write in structures:

```Java
void insertNumbers(List<? super Number> list){
	for (int i = 0; i < 10; i++) {
		list.add(i);
		list.add("test"); // error
	}
}

insertNumbers(new ArrayList<String>()); // --
insertNumbers(new ArrayList<Double>()); // --
insertNumbers(new ArrayList<Number>());
insertNumbers(new ArrayList<Object>());
```
![](Java/Core/_img/Pasted%20image%2020250520211006.png)
### Bounded parameters
**When we use bounded parameters, we are restricting the types that can be used as generic type arguments.**
As an example, let’s say we want to force our generic type always to be a subclass of animal:
```java
public abstract class Cage<T extends Animal> {
	abstract void addAnimal(T animal)
}
```
###  Unbounded Wildcard
An unbounded wildcard is a wildcard with no upper or lower bound, that can represent any type.
It is not the same as the Object, because there is no inheritance for generics
```Java
List<?> wildcardList = new ArrayList<String>(); 
List<Object> objectList = new ArrayList<String>(); // Compilation error
```
### Multiple bounded Wildcard
```java
public abstract class Cage<T extends Animal & Comparable>
```
**It’s also worth remembering that if one of the upper bounds is a class, it must be the first argument.**

Multiple restrictions. It is written via the character "&", that is, we say that the type represented by a variable of type T must be bounded at the top by the Object class and the Comparable interface.

```java
<T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll)
```
### PECS
- **Producer extends**(extend - supplier) - if you need to extract items. The collection can contain any subtype of the specified and each element behaves as specified. Accordingly, we cannot add any element because at runtime we do not know which specific type is in the collection
- **Consumer super**(super - user) **-** if you add elements. As in the collection can always contain a thing, you can definitely add it
The final thing to consider is what to do if a collection is **both a consumer and a producer**. An example of this might be a collection where elements are both added and removed. In this case, an **unbounded wildcard** should be used.
### Covariance, Contravariance and Invariance
**Covariance -** List can be assigned to a variable of type List (as if it is a successor of List). Arrays are covariant: Object[] can be set to String[].

`List<Integer>` можно присвоить в переменную типа `List<? extends Number> `(как будто он наследник` List<Number>`). Массивы ковариантны: в переменную Object[] можно присвоить значение типа String[].

```Java
Object[] strings = new Object[5];
strings = new String[3];
```

**Contravariance -** As a parameter of `List<Number>.#sort of Comparator<? super Number>` can be passed to `Comparator<Object> `(as if it were the parent of Comparator)
В качестве параметра метода `List<Number>\#sort` типа `Comparator<? super Number>` может быть передан `Comparator<Object>` (как будто он родитель `Comparator<Number>`)

**Invariance -** Lack of covariance and counter-variant properties. Generics without wildcards are invariant: List cannot be placed in a List or List variable.
Отсутствие свойств ковариантности и контрвариантности. Дженерики без вайлдкардов инвариантны: `List<Number>` нельзя положить ни в переменную типа `List<Double>`, ни в `List<Object>`.

Type inference is when the compiler can look at the type of a method argument to infer a generic type. For example, if we passed in _T_ to a method which returns _T,_ then the compiler can figure out the return type.
```Java
Integer inferredInteger = returnType(1);
String inferredString = returnType("String");
```
## **Wildcard Capture**
```Java
public static void reverse(List<?> list) {
	List<Object> tmp = new ArrayList<Object>(list);
	for (int i = 0; i < list.size(); i++) {
		list.set(i, tmp.get(list.size()-i-1)); // compile-time error
	}
}
```

Ошибка компиляции возникла, потому что в методе reverse в качестве аргумента принимается список с неограниченным символом подстановки `<?>` .

`<?>`означает то же что и `<? extends Object>`. Следовательно, согласно принципу PECS, list – это producer. А producer только продюсирует элементы. А мы в цикле for вызываем метод set(), т.е. пытаемся записать в list. И поэтому упираемся в защиту Java, что не позволяет установить какое-то значение по индексу.

Нам поможет паттерн Wildcard Capture. Здесь мы создаем обобщенный метод rev. Он объявлен с переменной типа T. Этот метод принимает список типов T, и мы можем сделать сет.

```Java
public static void reverse(List<?> list) {
	rev(list);
}

private static <T> void rev(List<T> list) {
	List<T> tmp = new ArrayList<T>(list);
	for (int i = 0; i < list.size(); i++) {
		list.set(i, tmp.get(list.size()-i-1));
	}
}
```
## Heap Pollution
The situation that occurs when a parameterized variable of type points to an object that is not of this parameterized type. So the data in the pile isn’t the type we’re expecting.
```java
List<A> listOfAs = new ArrayList<>();

List<B> listOfBs = (List<B>)(Object)listOfAs; //points to a list of As
```
Heap pollution может произойти в двух случаях: при использовании массивов дженериков и при смешивании параметризованных и raw-типов.

Может привести к ClassCastExceptions или ArrayStoreException, поэтому во время компиляции будет выдано предупреждение. Можно сапресить через @SaveVarargs.
https://habr.com/ru/post/207360
# Pattern matching
Added in Java 16
Реализован через обновленный instanceof
```java
// Old
if(obj instanceof String) {
	String str = (String) obj;
	System.out.println(str.toLowerCase());
}

// New
if (obj instanceof String str) {
	System.out.println(str.toLowerCase());
}
```