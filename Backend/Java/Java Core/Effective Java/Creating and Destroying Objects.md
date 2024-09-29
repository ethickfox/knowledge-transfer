## Consider static factory method instead of constructor
Static factory method(not the same as Factory method pattern)\
``` Java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE
}
```
###### Pros
1.  Static method has name, constructor - don't
	1. Easier to express what we are creating
	2. Ability to create constructors with the same signature
2.  They are not required to create new object on invocation
	1. Immutable classes could use preconstructed instances, even avoid creating new objects(this is used by [[Flyweight]] pattern)
	3. Creation of instance-controlled classes
3. They could return any subtype of this class
	1. Api could return objects without making their classes public(interface based frameworks)
	2. We could create companion classes to group and avoid exposing api(like Collections)
4. Class of returning parameter may vary from call to call because of different parameters
5. Class of returning object may not exist at the moment of writing method
	1. We could plug implementation later like JDBC drivers
	2. Such methods form basis of [[Service provider framework]]
###### Cons