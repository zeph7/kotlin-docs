# Extension functions/properties

* Since you can't modify built-in or third-party classes, you can't directly add functions or properties to them
* If you can achieve what you want by only using the public members of a class, you can of course just write a function that takes 
an instance of the class as a parameter - but sometimes, you'd really like to be able to say `x.foo(y)` instead of `foo(x, y)`, 
especially if you want to make a chain of such calls or property lookups: `x.foo(y).bar().baz` instead of `getBaz(bar(foo(x, y)))`
* There is a nice piece of syntactic sugar that lets you do this: <i>extension functions</i> and <i>extension properties</i>
* They look like regular member functions/properties, but they are defined outside of any class - yet they reference the class name 
and can use `this`
* However, they can only use visible members of the class (typically just the public ones)
* Behind the scenes, they get compiled down to regular functions that take the target instance as a parameter

For example, if you work a lot with bytes, you might want to easily get an unsigned byte in the range 0 through 255 instead 
of the default -128 through 127 (the result will have to be in the form of a `Short`/`Int`/`Long`, though). `Byte` is a built-in class 
that you can't modify, but you can define this extension function:

```kotlin
fun Byte.toUnsigned(): Int {
    return if (this < 0) this + 256 else this.toInt()
}
```

Now, you can do:

```kotlin
val x: Byte = -1
println(x.toUnsigned()) // Prints 255
```

If you'd rather do `x.unsigned`, you can define an <i>extension property</i>:

```kotlin
val Byte.unsigned: Int
    get() = if (this < 0) this + 256 else this.toInt()
```

* Keep in mind that this is just syntactic sugar - you're not actually modifying the class or its instances
* Therefore, you have to import an extension function/property wherever you want to use it (since it isn't 
carried along with the instances of the class)
* For the same reason, you can not override extension members - you can reimplement them for subtypes, but the resolution happens 
at compile-time based on the static type of the expression you're invoking it on
* There are a lot of built-in extension functions/properties in Kotlin - for example, `map()`, `filter()`, and the rest of the 
framework for processing collections in a functional manner is built using extension functions



