# Generics

## Generic type parameters

* Generics allows a class or function to contain members whose types vary with each usage
* They allow you to specify a "placeholder" type in a class or function that must be filled in whenever the class or function is used
* For example, a node in a linked list needs to contain data of some type that is not known when we write the class, so we introduce 
a <i>generic type parameter</i> `T` (they are conventionally given single-letter names):

```kotlin
class TreeNode<T>(val value: T?, val next: TreeNode<T>? = null)
```

* Whenever you create an instance of this class, you must specify an actual type in place of T, unless the compiler can infer it 
from the constructor parameters: `TreeNode("foo")` or `TreeNode<String>(null)`
* Every use of this instance will act as if it were an instance of a class that looks like this:

```kotlin
class TreeNode<String>(val value: String?, val next: TreeNode<String>? = null)
```

* To make functions that take more generic parameters than the class does, and to make generic functions inside nongeneric classes, 
and to make generic top-level functions, there's different placement of the generic type parameter in generic function declarations:

```kotlin
fun <T> makeLinkedList(vararg elements: T): TreeNode<T>? {
    var node: TreeNode<T>? = null
    for (element in elements.reversed()) {
        node = TreeNode(element, node)
    }
    return node
}
```

## Constraints

* You can restrict the types that can be used for a generic type parameter, by specifying that it must be an instance of a 
specific type or of a subclass thereof
* If you've got a class or interface called `Vehicle`, you can do:

```kotlin
class TreeNode<T : Vehicle>
```

* Now, you may not create a `TreeNode` of a type that is not a subclass/implementor of `Vehicle`
* If you want to impose additional constraints, you must use a separate `where` clause, in which case the type parameter must be a 
subclass of the given class (if you specify a class, and you can specify at most one) and implement all the given interfaces
* You may then access all the public members of all the given types whenever you've got a value of type `T`:

```kotlin
class TreeNode<T> where T : Vehicle, T : HasWheels
```

## Variance

### Introduction

* Pop quiz: if `Apple` is a subtype of `Fruit`, and `Bowl` is a generic container class, is `Bowl<Apple>` a subtype of `Bowl<Fruit>`? 
The answer is - perhaps surprisingly - <i>no</i>
* The reason is that if it were a subtype, we would be able to break the type system like this:

```kotlin
fun add(bowl: Bowl<Fruit>, fruit: Fruit) = bowl.add(fruit)

val bowl = Bowl<Apple>()
add(bowl, Pear()) // Doesn't actually compile!
val apple = bowl.get() // Boom!
```

* If the second-to-last line compiled, it would allow us to put a pear into what is ostensibly a bowl of only apples, and your 
code would explode when it tried to extract the "apple" from the bowl
* However, it's frequently useful to be able to let the type hierarchy of a generic type parameter "flow" to the generic class
* As we saw above, though, some care must be taken - the solution is to restrict the direction in which you can move data in 
and out of the generic object





