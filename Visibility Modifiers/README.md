# Visibility modifiers 

* Kotlin allows you to enforce symbol visibility via <i>visibility modifiers</i>, which can be placed on symbol declarations. If you 
don't supply a visibility modifier, you get the default visibility level, which is `public`
* The meaning of a visibility modifier depends on whether it's applied to a top-level declaration or to a declaration inside a class

<b>For top-level declarations</b>:

* `public` (or omitted): this symbol is visible throughout the entire codebase
* `internal`: this symbol is only visible inside files that belong to the same <i>module</i> (a source code grouping which is 
defined by your IDE or build tool) as the file where this symbol is declared
* `private`: this symbol is only visible inside the file where this symbol is declared

For example, `private` could be used to define a property or helper function that is needed by several functions in one file, or a 
complex type returned by one of your private functions, without leaking them to the rest of the codebase:

```kotlin
private class ReturnType(val a: Int, val b: Double, val c: String)

private fun secretHelper(x: Int) = x * x

private const val secretValue = 3.14
```

<b>For a symbol that is declared inside a class:</b>

* `public` (or omitted): this symbol is visible to any code that can see the containing class
* `internal`: this symbol is only visible to code that exists inside a file that belongs to the same module as the file where this 
symbol is declared, and that can also see the containing class
* `protected`: this symbol is only visible inside the containing class and all of its subclasses, no matter where they are declared 
(so if your class is public and open, anyone can subclass it and thus get to see and use the protected members). If you have used 
Java: this does not also grant access from the rest of the package.
* `private`: this symbol is only visible inside the containing class

### Note

* Visibility modifiers can't be placed on local variables, since their visibility is always limited to the containing block
* The type of a property, and the types that are used for the parameters and the return type of a function, must be "at least as 
visible" as the property/function itself. For example, a public function can't take a private type as a parameter
* The visibility level only affects the <i>lexical visibility</i> of the symbol
* When you subclass a class, its private members are also inherited by the subclass, but are not directly accessible there - however, 
if you call an inherited public function that happens to access a private member, that's fine.




