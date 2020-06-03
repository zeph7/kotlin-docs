# Null safety 

## Working with nulls

* A variable that doesn't refer to anything refers to `null`
* `null` is not an object - it's just a <i>keyword</i> that is used to make a variable refer to nothing or to check if it does
* A variable cannot actually be null unless it's been declared to allow for null, which you do by suffixing the type name with `?`

For example:

```kotlin
fun test(a: String, b: String?) {
}
```

* Compiler will allow this function to be called as -> `test("a", "b")` or `test("a", null)`, but not as `test(null, "b")` or `test(null, null)`
* Inside of test, the compiler will not allow you to do anything with b that would result in an exception if b should happen to be 
null - so you can do `a.length`, but not `b.length`
* However, once you're inside a conditional where you have checked that b is not null, you can do it:

```kotlin
if (b != null) {
    println(b.length)
}
```

Or

```kotlin
if (b == null) {
    // Can't use members of b in here
} else {
    println(b.length)
}
```

Making frequent null checks is annoying, so if you have to allow for the possibility of nulls, there are several very useful 
operators in Kotlin to ease working with values that might be null, as described below

## Safe call operator

* `x?.y` evaluates `x`, and if it is not null, it evaluates `x.y` (without reevaluating `x`), whose result becomes the result of the 
expression - otherwise, you get null
* This also works for functions, and it can be chained - for example, `x?.y()?.z?.w()` will return null if any of `x, x.y()`, or `x.y().z` 
produce null; otherwise, it will return the result of `x.y().z.w()`

## Elvis operator

* `x ?: y` evaluates `x`, which becomes the result of the expression unless it's null, in which case you'll get `y` instead. 
You can even use it to perform an early return in case of null:

```kotlin
val z = x ?: return y
```

* This will assign `x` to `z` if `x` is non-null, but if it is null, the entire function that contains this expression will stop and return 
`y`

## Not-null assertion operator

* Sometimes, you're in a situation where you have a value `x` that you know is not null, but the compiler doesn't realize it
* This can legitimately happen when you're interacting with Java code, but if it happens because your code's logic is more complicated 
than the compiler's ability to reason about it, you should probably restructure your code
* If you can't convince the compiler, you can resort to saying `x!!` to form an expression that produces the value of `x`, but whose 
type is non-nullable:

```kotlin
val x: String? = javaFunctionThatYouKnowReturnsNonNull()
val y: String = x!!
```

* It can of course be done as a single expression: `val x = javaFunctionThatYouKnowReturnsNonNull()!!`
* `!!` will raise a `NullPointerException` if the value actually is null
* So you could also use it if you really need to call a particular function and would rather have an exception if there's no object 
to call it on (`maybeNull!!.importantFunction()`), although a better solution (because an NPE isn't very informational) is this:

```kotlin
val y: String = x ?: throw SpecificException("Useful message")
y.importantFunction()
```







