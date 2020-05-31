# Exceptions

## Throwing and catching

Exceptions pretty much work like they do in Python. You <i>throw</i> (raise) one with `throw`:

```kotlin
throw IllegalArgumentException("Value must be positive")
```

You <i>catch</i> it with `try/catch`:

```kotlin
fun divideOrZero(numerator: Int, denominator: Int): Int {
    try {
        return numerator / denominator
    } catch (e: ArithmeticException) {
        return 0
    }
}
```

* The `catch` blocks are tried in order until an exception type is found that matches the thrown exception (it doesn't need to be an 
exact match; the thrown exception's class can be a subclass of the declared one), and at most one `catch` block will be executed
* If no match is found, the exception bubbles out of the `try/catch`
* The `finally` block (if any) is executed at the end, no matter what the outcome is: either after the `try` block completes 
successfully, or after a `catch` block is executed (even if another exception is thrown by the `catch` block), or if no matching catch is found

### `try/catch` as an <i>expression</i>

```kotlin
return try {
    numerator / denominator
} catch (e: ArithmeticException) {
    0
}
```
* The base exception class is `Throwable` (but it is more common to extend its subclass `Exception`), and there are a ton of built-in 
exception classes. If you don't find one that match your needs, you can create your own by inheriting from an existing exception class

## Nothing

* `throw` is also an <i>expression</i>, and its return type is the special class `Nothing`, which does not have any instances
* The compiler knows that an expression whose type is Nothing will never return normally
* If you make a function that always throws, or that starts an infinite loop, you could declare its return type to be `Nothing` in 
order to make the compiler aware of this
* One fun example of this is the built-in function `TODO`, which you can call in any expression (possibly supplying a string argument), 
and it raises a `NotImplementedError`
* The nullable version `Nothing?` will be used by the compiler when something is initialized with null and there is no other type 
information. In `val x = null`, the type of `x` will be `Nothing`?
* This type does not have the "never returns normally" semantics; instead, the compiler knows that the value will always be null




