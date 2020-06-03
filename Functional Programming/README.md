# Functional programming

## Function types

* Functions in Kotlin are first-class values - they can be assigned to variables and passed around as parameters
* The type a function is a <i>function type</i>, which is indicated with a parenthesized parameter type list and an arrow to the return type
 
 For example, consider this function:
 ```kotlin
 fun safeDivide(numerator: Int, denominator: Int) =
    if (denominator == 0) 0.0 else numerator.toDouble() / denominator
 ```
 
* It takes two `Int` parameters and returns a `Double`, so its type is `(Int, Int) -> Double`

We can reference the function itself by prefixing its name with `::`, and we can assign it to a variable (whose type would normally be 
inferred, but we show the type signature for demonstration):

```kotlin
val f: (Int, Int) -> Double = ::safeDivide
```

When you have a variable or parameter of function type (or say, function reference), you can call it as if it were an 
ordinary function, and that will cause the referenced function to be called:

```kotlin
val quotient = f(3, 0)
```

It is possible for a class to implement a function type as if it were an interface. It must then supply an operator function called 
invoke with the given signature, and instances of that class may then be assigned to a variable of that function type:

```kotlin
class Divider : (Int, Int) -> Double {
    override fun invoke(numerator: Int, denominator: Int): Double = ...
}
```

## Function literals: lambda expressions and anonymous functions

* <i>lambda expressions</i>: unnamed function declarations with a very compact syntax, which evaluate to callable function objects
* In Kotlin, lambdas can contain multiple statements, so it can do more complex task
* The last statement must be an expression, whose result will become the return value of the lambda

A lambda expression is enclosed in curly braces, and begins by listing its parameter names and possibly their types (unless the 
types can be inferred from context):

```kotlin
val safeDivide = { numerator: Int, denominator: Int ->
    if (denominator == 0) 0.0 else numerator.toDouble() / denominator
}
```

### Note

* The type of `safeDivide` is `(Int, Int) -> Double`. Unlike function type declarations, parameter list of lambda expressions 
must not be enclosed in parentheses
* The return type of a lambda expression is inferred from the type of the last expression inside it (or from the function type 
of the variable/parameter that the lambda expression is assigned to)

If a lambda expression is assigned to a variable with a declared type or passed as a function parameter (which is the ordinary use), 
Kotlin can infer the parameter types too, and you only need to specify their names:

```kotlin
val safeDivide: (Int, Int) -> Double = { numerator, denominator ->
    if (denominator == 0) 0.0 else numerator.toDouble() / denominator
}
```

Or

```kotlin
fun callAndPrint(function: (Int, Int) -> Double) {
    println(function(2, 0))
}

callAndPrint({ numerator, denominator ->
    if (denominator == 0) 0.0 else numerator.toDouble() / denominator
})
```

A parameterless lambda does not need the arrow. A one-parameter lambda can choose to omit the parameter name and the arrow, in which 
case the parameter will be called `it`:

```kotlin
val square: (Double) -> Double = { it * it }
```

If the type of the last parameter to a function is a function type and you want to supply a lambda expression, you can place 
the lambda expression outside of the parameter parentheses. If the lambda expression is the only parameter, you can omit the 
parentheses entirely. This is very useful for constructing DSLs.

```kotlin
fun callWithPi(function: (Double) -> Double) {
    println(function(3.14))
}

callWithPi { it * it }
```

### Underscore for unused variables (since 1.1)

If the lambda parameter is unused, you can place an underscore instead of its name:

```kotlin
map.forEach { _, value -> println("$value!") }
```

### Anonymous Functions

If you want to be more explicit about the fact that you're creating a function, you can make an <i>anonymous function</i>, which is still an 
expression rather than a declaration:

```kotlin
callWithPi(fun(x: Double): Double { return x * x })
```

Or

```kotlin
callWithPi(fun(x: Double) = x * x)
```

Lambda expressions and anonymous functions are collectively called function literals.


## Nice utility functions

### run(), let(), and with()

### run()

* `?.` is nice if you want to call a function on something that might be null. But what if you want to call a function that takes a 
non-null parameter, but the value you want to pass for that parameter might be null?
* Try `run()`, which is an extension function on `Any?` that takes a lambda with receiver as a parameter and invokes it on the value 
that it's called on, and use `?.` to call `run()` only if the object is non-null:

```kotlin
val result = maybeNull?.run { functionThatCanNotHandleNull(this) }
```

* If `maybeNull` is null, the function won't be called, and `result` will be null; otherwise, it will be the return value 
of `functionThatCanNotHandleNull(this)`, where `this` refers to `maybeNull`
* You can chain `run()` calls with `?.` - each one will be called on the previous result if it's not null:

```kotlin
val result = maybeNull
    ?.run { firstFunction(this) }
    ?.run { secondFunction(this) }
```

* The first `this` refers to `maybeNull`, the second one refers to the result of `firstFunction()`, and `result` will be the result of 
`secondFunction()` (or null if `maybeNull` or any of the intermediate results were null)

### let()

* A syntactic variation of `run()` is `let()`, which takes an ordinary function type instead of a function type with receiver, so 
the expression that might be null will be referred to as `it` instead of this
* Both `run()` and `let()` are also useful if you've got an expression that you need to use multiple times, but you don't care to come up 
with a variable name for it and make a null check:

```kotlin
val result = someExpression?.let {
   firstFunction(it)
   it.memberFunction() + it.memberProperty
}
```

### with()

* Yet another version is `with()`, which you can also use to avoid coming up with a variable name for an expression, but only if you 
know that its result will be non-null:

```kotlin
val result = with(someExpression) {
   firstFunction(this)
   memberFunction() + memberProperty
}
```

* In the last line, there's an implicit `this.` in front of both `memberFunction()` and `memberProperty` (if these exist on the type of 
someExpression). The return value is that of the last expression.

## apply() and also()

### apply()

* If you don't care about the return value from the function, but you want to make one or more calls involving something that might be 
null and then keep on using that value, try `apply()`, which returns the value it's called on
* This is particularly useful if you want to work with many members of the object in question:

```kotlin
maybeNull?.apply {
    firstFunction(this)
    secondFunction(this)
    memberPropertyA = memberPropertyB + memberFunctionA()
}?.memberFunctionB()
```

* Inside the apply block, this refers to `maybeNull`. There's an implicit this in front of `memberPropertyA`, `memberPropertyB`, and 
`memberFunctionA` (unless these don't exist on `maybeNull`, in which case they'll be looked for in the containing scopes). Afterwards, 
`memberFunctionB()` is also invoked on `maybeNull`

### also()

* If you find the this syntax to be confusing, you can use `also` instead, which takes ordinary lambdas:

```kotlin
maybeNull?.also {
    firstFunction(it)
    secondFunction(it)
    it.memberPropertyA = it.memberPropertyB + it.memberFunctionA()
}?.memberFunctionB()
```

### takeIf() and takeUnless()

* If you want to use a value only if it satisfies a certain condition, try `takeIf()`, which returns the value it's called on if it 
satisfies the given predicate, and null otherwise
* There's also` takeUnless()`, which inverts the logic
* You can follow this with a `?.` to perform an operation on the value only if it satisfies the predicate
* Below, we compute the square of some expression, but only if the expression value is at least 42:

```kotlin
val result = someExpression.takeIf { it >= 42 } ?.let { it * it }
```







