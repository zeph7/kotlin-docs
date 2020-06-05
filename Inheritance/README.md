# Inheritance

## Subclassing

* Kotlin supports single-parent class inheritance - so each class (except the root class `Any`) has got exactly one parent class, called 
a <i>superclass</i>
* Kotlin wants you to think through your class design to make sure that it's actually safe to subclass it, so classes are <i>closed</i> 
by default and can't be inherited from unless you explicitly declare the class to be <i>open</i> or <i>abstract</i>

You can then subclass from that class by declaring a new class which mentions its parent class after a colon:

```kotlin
open class MotorVehicle
class Car : MotorVehicle()
```

* Classes that don't declare a superclass implicitly inherit from `Any`

The subclass must invoke one of the constructors of the base class, passing either parameters from its own constructor or 
constant values:

```kotlin
open class MotorVehicle(val maxSpeed: Double, val horsepowers: Int)
class Car(
    val seatCount: Int,
    maxSpeed: Double
) : MotorVehicle(maxSpeed, 100)
```

* The subclass <i>inherits</i> all members that exist in its superclass - both those that are directly defined in the superclass and 
the ones that the superclass itself has inherited

In this example, Car contains the following members:


* `seatCount`, which is `Car`'s own property
* `maxSpeed` and `horsepowers`, which are inherited from `MotorVehicle`
* `toString()`, `equals()`, and `hashCode()`, which are inherited from `Any`

<b>Note</b> - that we do not use `val` in front of `maxSpeed` in `Car` - doing so would have introduced a distinct property in `Car` 
that would have shadowed the one inherited from `MotorVehicle`. As written, it's just a constructor parameter that we pass on to the 
superconstructor.

* `private` members (and `internal` members from superclasses in other modules) are also inherited, but are not directly 
accessible: if the superclass contains a private property `foo` that is referenced by a public function `bar()`, instances of 
the subclass will contain a `foo`; they can't use it directly, but they are allowed to call `bar()`.

## Overriding

* If a member function or property is declared as `open`, subclasses may <i>override</i> it by providing a new implementation

Let's say that MotorVehicle declares this function:
```kotlin
open fun drive() =
    "$horsepowers HP motor vehicle driving at $maxSpeed MPH"
```

If `Car` does nothing, it will inherit this function as-is, and it will return a message with the car's horsepowers and max speed. 
If we want a car-specific message, `Car` can override the function by redeclaring it with the `override` keyword:

```kotlin
override fun drive() =
   "$seatCount-seat car driving at $maxSpeed MPH"
```

* The signature of the overriding function must exactly match the overridden one, except that the return type in the overriding 
function may be a subtype of the return type of the overridden function

If what the overriding function wants to do is an extension of what the overridden function did, you can call the overridden 
function via `super` (either before, after, or between other code):

```kotlin
override fun drive() =
    super.drive() + " with $seatCount seats"
```

## Interfaces

* The single-parent rule often becomes too limiting, as you'll often find commonalities between classes in different branches 
of a class hierarchy. These commonalities can be expressed in <i>interfaces</i>
* An <i>interface</i> is essentially a contract that a class may choose to sign; if it does, the class is obliged to provide 
implementations of the properties and functions of the interface
* However, an interface may (but typically doesn't) provide a default implementation of some or all of its properties and functions
* If a property or function has a default implementation, the class may choose to override it, but it doesn't have to

Here's an interface without any default implementations:

```kotlin
interface Driveable {
    val maxSpeed: Double
    fun drive(): String
}
```

We can choose to let `MotorVehicle` implement that interface, since it's got the required members - but now we need to mark 
those members with `override`, and we can remove `open` since an overridden function is implicitly open:

```kotlin
open class MotorVehicle(
    override val maxSpeed: Double,
    val wheelCount: Int
) : Driveable {
    override fun drive() = "Wroom!"
}
```

* Subclasses of a class that implements an interface (in this case, `Car`) are also considered to be implementing the interface
* A symbol that is declared inside an interface normally should be public. The only other legal visibility modifier is `private`, 
which can only be used if the function body is supplied - that function may then be called by each class that implements the 
interface, but not by anyone else

## Abstract classes

* Some superclasses are very useful as a grouping mechanism for related classes and for providing shared functions, but are so 
general that they're not useful on their own
* `MotorVehicle` seems to fit this description. Such a class should be declared abstract, which will prevent the class from 
being instantiated directly:

```kotlin
abstract class MotorVehicle(val maxSpeed: Double, val wheelCount: Int)
```

* Now, you can no longer say val `mv = MotorVehicle(100, 4)`
* Abstract classes are implicitly open, since they are useless if they don't have any concrete subclasses

When an abstract class implements one or more interfaces, it is not required to provide definitions of the members of its 
interfaces (but it can if it wants to). It must still declare such members, using `abstract override` and not providing any body 
for the function or property:

```kotlin
abstract override val foo: String
abstract override fun bar(): Int
```

Being abstract is the only way to "escape" from having to implement the members of your interfaces, by offloading the work onto 
your subclasses - if a subclass wants to be concrete, it must implement all the "missing" members

## Polymorphism

* Polymorphism is the ability to treat objects with similar traits in a common way
* In Kotlin, polymorphism is achieved via the class hierarchy, in such a way that it is impossible to run into a situation where 
a property or function is missing
* The basic rule is that a variable/property/parameter whose declared type is `A` may refer to an instance of a class `B` if and only 
if `B` is a subtype of `A`. This means that either, `A` must be a class and `B` must be a subclass of `A`, or that `A` must be an 
interface and `B` must be a class that implements that interface or be a subclass of a class that does

With our classes and interfaces from the previous sections, we can define these functions:

```kotlin
fun boast(mv: MotorVehicle) =
    "My ${mv.wheelCount} wheel vehicle can drive at ${mv.maxSpeed} MPH!"

fun ride(d: Driveable) =
    "I'm riding my ${d.drive()}"
```

and call them like this:

```kotlin
val car = Car(4, 120)
boast(car)
ride(car)
```

* We're allowed to pass a `Car` to `boast()` because `Car` is a subclass of `MotorVehicle`. We're allowed to pass a `Car` to 
`ride()` because `Car` implements `Driveable` (thanks to being a subclass `MotorVehicle`)
* Inside `boast()`, we're only allowed to access the members of the declared parameter type `MotorVehicle`, even if we're in a 
situation where we know that it's really a `Car` (because there could be other callers that pass a non-`Car`)
* Inside `ride()`, we're only allowed to access the members of the declared parameter type `Driveable`
* This ensures that every member lookup is safe - the compiler only allows you to pass objects that are guaranteed to have the 
necessary members. The downside is that you will sometimes be forced to declare "unnecessary" interfaces or wrapper classes in 
order to make a function accept instances of different classes

## Casting and type testing

* When you take an interface or an open class as a parameter, you generally don't know the real type of the parameter at runtime, 
since it could be an instance of a subclass or of any class that implements the interface
* You can use is to check if the real type of an object is a particular class or a subclass thereof (or an implementor of an interface)

When this is used as the condition in an if, the compiler will let you perform type-specific operations on the object inside the if body:

```kotlin
fun foo(x: Any) {
    if (x is Person) {
        println("${x.name}") // This wouldn't compile outside the if
    }
}
```

* If you want to check for <i>not</i> being an instance of a type, use `!is`
* Note that `null` is never an instance of any non-nullable type, but it is always an "instance" of any nullable type (even though it 
technically isn't an instance, but an absence of any instance)
* The compiler will not let you perform checks that can't possibly succeed because the declared type of the variable is a class 
that is on an unrelated branch of the class hierarchy from the class you're checking against - if the declared type of `x` is 
`MotorVehicle`, you can't check if `x` is a `Person`
* If the right-hand side of `is` is an interface, Kotlin will allow the type of the LHS to be any interface or open class, 
because it could be that some subclass thereof implements the interface

If your code is too clever for the compiler, and you know without the help of `is` that `x` is an instance of `Person` but the compiler 
doesn't, you can cast your value with `as`:

```kotlin
val p = x as Person
```

* This will raise a `ClassCastException` if the object is not actually an instance of `Person` or any of its subclasses

If you're not sure what `x` is, but you're happy to get null if it's not a `Person`, you can use `as?`, which will return null 
if the cast fails. Note that the resulting type is `Person?`:

```kotlin
val p = x as? Person
```

You can also use `as` to cast to a nullable type. The difference between this and the previous `as?` cast is that this one will fail 
if `x` is a non-null instance of another type than `Person`:

```kotlin
val p = x as Person?
```

## Delegation

If you find that an interface that you want a class to implement is already implemented by one of the properties of the class, you 
can <i>delegate</i> the implementation of that interface to that property with `by`:

```kotlin
interface PowerSource {
    val horsepowers: Int
}

class Engine(override val horsepowers: Int) : PowerSource

open class MotorVehicle(val engine: Engine): PowerSource by engine
```

This will automatically implement all the interface members of `PowerSource` in `MotorVehicle` by invoking the same member on `engine`. 
This only works for properties that are declared in the constructor.







