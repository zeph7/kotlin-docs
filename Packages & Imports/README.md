# Packages and imports

## Packages

* Every Kotlin file should belong to a <i>package</i>
* The package declaration must go on the top of the file:

```kotlin
package content.exercises
```

* If a file doesn't declare a package, it belongs to the nameless <i>default package</i>. This should be avoided, as it will make it hard to 
reference the symbols from that file in case of naming conflicts (you can't explicitly import the empty package)

## Imports

* In order to use something from a package, it is sufficient to use the package name to fully qualify the name of the symbol at the 
place where you use the symbol:

```kotlin
val exercise = content.exercises.Exercise()
```

* This quickly gets unwieldy, so you will typically <i>import</i> the symbols you need. You can import a specific symbol:

```kotlin
import content.exercises.Exercise
```

* Or an entire package, which will bring in all the symbols from that package:

```kotlin
import content.exercises.*
```

* With either version of the import, you can now simply do:

```kotlin
val exercise = Exercise()
```

* If there is a naming conflict, you should usually import just one of the symbols and fully qualify the usages of the other. 
If both are heavily used, you can rename the symbol at import time:

```kotlin
import content.exercises.Exercise as Ex
```

* In Kotlin, importing is a compile-time concept - importing something does not actually cause any code to run 

