
# Data Types

```kotlin
fun main(args: Array<String>){
    var age: Int = 10   // or you can simply write // var age  = 10

    var b: Boolean = true
    var f: Float = 97.4F    // F or f is necessary to denote for Float Data Type
    var d: Double = 98.47

    println(age)
    println(b)
    println(f)
    print(d)
}
```

* Why I in Int and B in Boolean capital?
* Because Kotlin has no primitive data types All Data Types in Kotlin are `Objects Actually everything in Kotlin is an `Object`


### Integer Type

* `Long` 	Bits = 64 , Min value =  -9223372036854775808 ,	Max value = 9223372036854775807
* `Int` 	Bits = 32 , Min value = -2147483648 ,	Max value = 2147483647
* `Short` Bits = 16 , Min value = -32768 ,	Max value = 32767
* `Byte` 	Bits = 8 , Min value =  -128 ,	Max value = 127

### Note

* An `integer` literal has the type `Int` if its value fits in an `Int`, or `Long` otherwise
* `Long` literals should be suffixed by `L` for clarity, which will also let you make a Long with a "small" value
* There are no literal suffixes for Short or Byte, so such values need an explicit type declaration or the use of an explicit conversion function.

```kotlin
val anInt = 3
val anotherInt = 2147483647
val aLong = 2147483648
val aBetterLong = 2147483649L
val aSmallLong = 3L
val aShort: Short = 32767
val anotherShort = 1024.toShort()
val aByte: Byte = 65
val anotherByte = -32.toByte()
```

### Floating-point and other Types

* `Double` 	Bits = 64 , Notes -> 16-17 significant digits (same as float in Python)
* `Float` 	Bits = 32 , Notes -> 6-7 significant digits
* `Char`    Bits = 16 , Notes -> UTF-16 code unit
* `Boolean` 	Bits = 8 , Notes -> true or false

### Note 

* Beware that dividing an integer by an integer produces an integer (like in Python 2, but unlike Python 3)
* If you want a floating-point result, at least one of the operands needs to be a floating-point number

```kotlin
println(7 / 3)            // Prints 2
println(7 / 3.0)          // Prints 2.3333333333333335
val x = 3
println(7 / x)            // Prints 2
println(7 / x.toDouble()) // Prints 2.3333333333333335
```

### String Type

```kotlin
val c = 'x' // Char
val message = "Hello" // String
val m = message[0] // Char
```


