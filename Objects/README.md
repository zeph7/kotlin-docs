# Objects and companion objects

## Object declarations

* If you need a <i>singleton</i> - a class that only has got one instance - you can declare the class in the usual way, but use the 
`object` keyword instead of `class`:

```kotlin
object CarFactory {
    val cars = mutableListOf<Car>()
    
    fun makeCar(horsepowers: Int): Car {
        val car = Car(horsepowers)
        cars.add(car)
        return car
    }
}
```

* There will only ever be one instance of this class, and the instance (which is created the first time it is accessed, in a 
thread-safe manner) has got the same name as the class:

```kotlin
val car = CarFactory.makeCar(150)
println(CarFactory.cars.size)
```

## Companion objects

* If you need a function or a property to be tied to a class rather than to instances of it (similar to `@staticmethod` in Python), 
you can declare it inside a <i>companion object</i>:

```kotlin
class Car(val horsepowers: Int) {
    companion object Factory {
        val cars = mutableListOf<Car>()

        fun makeCar(horsepowers: Int): Car {
            val car = Car(horsepowers)
            cars.add(car)
            return car
        }
    }
}
```

* The companion object is a singleton, and its members can be accessed directly via the name of the containing class (although you 
can also insert the name of the companion object if you want to be explicit about accessing the companion object):

```kotlin
val car = Car.makeCar(150)
println(Car.cars.size)
```
Or

```kotlin
val car = Car.makeCar(150)
println(Car.Factory.cars.size)
```

* In spite of this syntactical convenience, the companion object is a proper object on its own, and can have its own supertypes - 
and you can assign it to a variable and pass it around
* A companion object is initialized when the class is loaded (typically the first time it's referenced by other code that is being 
executed), in a thread-safe manner
* You can omit the name, in which case the name defaults to `Companion`
* A class can only have one companion object, and companion objects can not be nested
* Companion objects and their members can only be accessed via the containing class name, not via instances of the containing class
* If you try to redeclare a companion object in a subclass, you'll just shadow the one from the base class




