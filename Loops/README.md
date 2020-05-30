# Loops

## for Loop

* Kotlin's loops are similar to Python's. for iterates over anything that is iterable (anything that has an iterator() function that provides an Iterator object), or anything that is itself an iterator:

```kotlin
val names = listOf("Anne", "Peter", "Jeff")
for (name in names) {
    println(name)
}
```

```kotlin
fun main(args: Array<String>){
    var i: Int
    for (i in 1..10) {
        println(i)
    }
}
```

## while Loop

```kotlin
fun main(args: Array<String>){
    var i: Int = 1
    while(i <= 10){
        println(i)
        i++
    }
}
```

## do while Loop

* always iterates the loop atleast once

```kotlin
fun main(args: Array<String>){
    var i: Int = 1
    do{
        println(i)
        i++
    }while(i <= 10)
}
```


## break Statement

* In Java you can break out of inner loop simply but can't break out of outer loop so easily
* But Kotlin includes an easy way to break out of outer loop
* You can anytime break out of any loop labelled by any user defined name like myloop

```kotlin
fun main(args: Array<String>){
    var i: Int
    var j: Int
    myloop@ for (i in 1..3){        //labelling this for loop as myloop
        for (j in 1..3){
            println("$i $j")
            if(i == 2 && j == 2)
                break@myloop        //break's out of loop which is labelled as myloop
        }
    }
}
```

## continue Statement

* In Java you can continue to inner loop simply but can't continue to outer loop so easily
* But Kotlin includes an easy way to continue to outer loop
* You can anytime continue to any loop labelled by any user defined name like myloop

```kotlin
fun main(args: Array<String>){
    var i: Int
    var j: Int
    myloop@ for (i in 1..3){        //labelling this for loop as myloop
        for (j in 1..3){
            if(i == 2 && j == 2)
                continue@myloop        //continue to loop which is labelled as myloop
            println("$i $j")
        }
    }
}
```
