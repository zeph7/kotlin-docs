# Conditionals

## if / else

* `if` is called an `expression` because it returns a value

```kotlin
//program for greater of two number
fun main(args: Array<String>){
    var a = 2
    var b = 5
    var maxValue: Int = if(a > b){
        println("a is greater")
        a
    }else{
        println("b is greater")
        b
    }
    //in a multiple lines of code under if or else, last statement will be returned
    println(maxValue)
}
```

## When

* `when` in Kotlin is like <-> `switch-case` in Java

```kotlin
fun main(args: Array<String>){
    val x = 4
    when(x){                            //means when x == 1 print "x is 1"
        1 -> println("x is 1")          //or when x == 2 print "x is 2"
        2 -> {println("x is 2")}        //can use codeblock for multiple lines of code
        3,4,5 -> println("x is 3 or 4 or 5")    //when x==3 or x==4 or x==5 print "x is 3 or 4 or 5"
        in 6..10 -> println("x lies in 6 to 10")    //when x in 6 to 10 print "x lies in 6 to 10"
        !in 11..20 -> println("x not lies in 11 to 20") ////when x not in 11 to 20 print "x not lies in 11 to 20"
        else -> println("x is unknown")         //default value
    }
}
```

## When as Expression

* `when` is called an `expression` as it also returns a value

```kotlin
fun main(args: Array<String>){
    val x = 1
    var str: String = when(x){
        1 -> "x is 1"
        2 -> "x is 2"
        else -> "x is an alien"
    }
    print(str)
}
```


