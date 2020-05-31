# OOP Basic

* `Object-oriented programming` (OOP) is a programming paradigm based on the concept of `objects`, which can contain data, in the form 
of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods)
* In OOP, computer programs are designed by making them out of objects that interact with one another

## Classes

* The definitions for the data format and available procedures for a given type or class of object; may also contain data and procedures 
(known as class methods) themselves, i.e. classes contain the `data members` and `member functions`
* A class is a `blueprint` of a runtime `entity` and `object` is its `state`, which includes both its behavior and state
* `Objects` are instances of the classes
* `Classes` are characterised by `Properties` and `Methods`

Example ->

```
Take an example of `House` Class ->

Properties : -Price, -Type, -YearBuilt, -Owner
Methods : -nunberOfBathrooms, -getYearBuilt, -getHousePrice
```


### Declaration and instantiation

* Classes are declared with the class keyword. A basic class without any properties or functions of its own looks like this:
```kotlin
class Empty
```

* You can then create an instance of this class as:
```kotlin
val object = Empty()
```
* Class names should use `UpperCamelCase`

### Inherited built-in functions

Every class that doesn't explicitly declare a parent class inherits from `Any`, which is the root of the class hierarchy
 Via `Any`, every class automatically has the following functions:
 - `toString()` returns a string representation of the object
 - `equals(x)` checks if this object is equal to some other object `x` of any class (by default, this just checks if this object is 
 the same object as `x`
 - `hashCode()` returns an integer that can be used by hash tables and for shortcutting complex equality comparisons (objects that are 
 equal according to equals() must have the same hash code, so if two objects' hash codes are different, the objects cannot be equal)
 
### Properties

```kotlin
class Person {
    var name = "Anne"
    var age = 32
}
```
* Every instance of `Person` will have its own `name` and `age` property, which can be modified independently of the other

```kotlin
val a = Person()
val b = Person()
println("${a.age} ${b.age}") // Prints "32 32"
a.age = 42
println("${a.age} ${b.age}") // Prints "42 32"
```

* It's not possible to add new properties to an object or to a class at runtime
* Property names should use `lowerCamelCase`

### Constructors and initializer blocks

* Kotlin `constructors` and `initializer` blocks run automatically whenever an instance of an `object is created`
* A Kotlin class may have one `Primary Constructor`, whose parameters are supplied after the class name
* The primary constructor parameters are available when you initialize properties in the class body, and also in the optional 
`initializer` block, which can contain complex initialization logic (a property can be declared without an initial value, in which 
case it must be initialized in init)

```kotlin
class Person(firstName: String, lastName: String, yearOfBirth: Int) {
    val fullName = "$firstName $lastName"
    var age: Int
    
    init {
        age = 2018 - yearOfBirth
    }
}
```

* If all you want to do with a constructor parameter value is to assign it to a property with the same name, you can declare the 
property in the primary constructor parameter list (the oneliner below is sufficient for both declaring the properties, declaring 
the constructor parameters, and initializing the properties with the parameters):
```kotlin
class Person(val name: String, var age: Int)
```

* If you need multiple ways to initialize a class, you can create `Secondary Constructors`, each of which looks like a function 
whose name is constructor
* Every secondary constructor must invoke another (primary or secondary) constructor by using the `this` keyword as if it were a 
function (so that every instance construction eventually calls the primary constructor)

```kotlin
class Person(val name: String, var age: Int) {
    constructor(name: String) : this(name, 0)
    constructor(yearOfBirth: Int, name: String)
        : this(name, 2018 - yearOfBirth)
}
```

* A secondary constructor can also have a body in curly braces if needs to do more than what the primary constructor does
* The constructors are distinguished from each other through the types of their parameters, like in ordinary function overloading

We can now create a Person in three different ways:

```kotlin
val a = Person("Jaime", 35)   // name = "Jamie", age = 35
val b = Person("Jack")        // name = "Jack", age = 0
val c = Person(1995, "Lynne") // name = "Lynne", age = 23 (2018-1993)
```

* <b>Note:</b> If a class has got a primary constructor, it is no longer possible to create an instance of it without supplying any 
parameters (unless one of the secondary constructors is parameterless).

## Setters and getters

### Setter

* A property is really a `backing field` (kind of a hidden variable inside the object) and two `accessor functions`: one that gets 
the value of the variable and one that sets the value
* You can override one or both of the accessors 
* Inside an accessor, you can reference the `backing field` with `field`
* The setter accessor must take a parameter `value`, which is the value that is being assigned to the property
* A `getter body` could either be a `one-line expression` preceded by `=` or a more complex body enclosed in curly braces, while a `setter` 
body typically includes an assignment and must therefore be enclosed in curly braces
* The compiler can tell which accessors belong to which properties because the only legal place for an accessor is immediately after the 
property declaration (and there can be at most one getter and one setter) - so you can't split the property declaration and the accessor 
declarations (the order of the accessors doesn't matter)

```kotlin
class Person(age: Int) {
    var age = 0
        set(value) {
            if (value < 0) throw IllegalArgumentException(
                    "Age cannot be negative")
            field = value
        }

    init {
        this.age = age
    }
}
```

* Annoyingly, the setter logic is not invoked by the initialization, which instead sets the backing field directly - that's why we 
have to use an initializer block in this example in order to verify that newly-created persons also don't get a negative age

### Getter

* If for some reason you want to store a different value in the backing field than the value that is being assigned to the property, 
you're free to do that, but then you will probably want a getter to give the calling code back what they expect: if you say 
`field = value * 2` in the setter and `this.age = age * 2` in the initializer block, you should also have `get() = field / 2`

You can also create properties that don't actually have a backing field, but just reference another property:

```kotlin
val isNewborn
    get() = age == 0
```

## Member functions

* A function declared inside a class is called a `member function` of that class
* Every invocation of a member function must be performed on an instance of the class, and the instance will be available 
during the execution of the function
* Every member function can use the keyword `this` to <i>reference</i> the <i>current instance</i>, without declaring it
* As long as there is no name conflict with an identically-named parameter or local variable, `this` can be omitted

If we do this inside a `Person` class with a `name` property:

```kotlin
fun present() {
    println("Hello, I'm $name!")
}
```

We can then do this:

```kotlin
val p = Person("Claire")
p.present() // Prints "Hello, I'm Claire!"
```

You could have said `${this.name}`, but that's redundant and generally discouraged. Oneliner functions can be declared with an `=`:

```kotlin
fun greet(other: Person) = println("Hello, ${other.name}, I'm $name!")
```

* Member functions are declared at compile-time, so can't assign a new member function at runtime
* Member function names should use `lowerCamelCase`


## Lateinit

* Kotlin requires that every member property is initialized during instance construction 
* Sometimes, a class is intended to be used in such a way that the constructor doesn't have enough information to initialize all 
properties (such as when making a builder class or when using property-based dependency injection) 

In order to not have to make those properties <i>nullable</i>, you can use a <i>late-initialized property</i>:

```kotlin
lateinit var name: String
```

* Kotlin will allow you to declare this property without initializing it, and you can set the property value at some point after 
construction (either directly or via a function)
* You should initialize the property before it is to be read, otherwise you will get `UninitializedPropertyAccessException`

Inside the class that declares a lateinit property, you can check if it has been initialized:

```kotlin
if (::name.isInitialized) println(name)
```

* `lateinit` can only be used with `var`, not with `val`, and the type must be non-primitive and non-nullable

## Infix functions

* You can designate a one-parameter member function or extension function for use as an infix operator, which can be useful if 
you're designing a DSL
* The left operand will become `this`, and the right operand will become the parameter

If you do this inside a `Person` class that has got a `name` property:

```kotlin
infix fun marry(spouse: Person) {
    println("$name and ${spouse.name} are getting married!")
}
```

We can now do this (but it's still possible to call the function the normal way):

```kotlin
val lisa = Person("Lisa")
val anne = Person("Anne")
lisa marry anne // Prints "Lisa and Anne are getting married!"
```

* All infix functions have the same precedence (which is shared with all the built-in infix functions, such as the bitwise functions 
`and`, `or`, `inv`, etc.): lower than the arithmetic operators and the `..` range operator, but higher than the Elvis operator `?:`, 
comparisons, logic operators, and assignments


## Operators

* Most of the operators that are recognized by Kotlin's syntax have predefined textual names and are available for implementation 
in your classes

For example, the binary + operator is called plus. Similarly to the infix example, if you do this inside a Person class that has got a 
name property:

```kotlin
operator fun plus(spouse: Person) {
    println("$name and ${spouse.name} are getting married!")
}
```

With `lisa` and `anne` from the infix example, you can now do:

```kotlin
lisa + anne // Prints "Lisa and Anne are getting married!"
```

## Enum classes

* Whenever you want a variable that can only take on a limited number of values where the only feature of each value is that it's 
distinct from all the other values, you can create an `enum` class:

```kotlin
enum class ContentKind {
    TOPIC,
    ARTICLE,
    EXERCISE,
    VIDEO,
}
```

* There are exactly four instances of this class, named `ContentKind.TOPIC`, and so on. Instances of this class can be compared to 
each other with `==` and `!=`, and you can get all the allowable values with `ContentKind.values()`

You can also tack on more information to each instance if you need:

```kotlin
enum class ContentKind(val kind: String) {
    TOPIC("Topic"),
    ARTICLE("Article"),
    EXERCISE("Exercise"),
    VIDEO("Video"),
    ;

    override fun toString(): String {
        return kind
    }
}
```

* Null safety is enforced as usual, so a variable of type `ContentKind` can not be null, unlike in Java

## Data classes

* Frequently - especially if you want a complex return type from a function or a complex key for a map - you'll want a quick and 
dirty class which only contains some properties, but is still comparable for equality and is usable as a map key 
* If you create a `data class`, you'll get automatic implementations of the following functions: `toString()` (which will produce a 
string containing all the property names and values), `equals()` (which will do a per-property equals()), `hashCode()` (which will hash 
the individual properties and combine the hashes), and the functions that are required to enable Kotlin to destructure an instance 
of the class into a declaration (`component1()`, `component2()`, etc.):

```kotlin
data class ContentDescriptor(val kind: ContentKind, val id: String) {
    override fun toString(): String {
        return kind.toString() + ":" + id
    }
}
```






