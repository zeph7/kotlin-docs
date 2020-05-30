# Basics

## Hello World Program

* Kotlin is a JVM(Java Virtual Machine) Language, so it need JDK(Java Development Kit)
* In Kotlin we don't need to declare class explictly(unlike Java)
* So you can directly create a function by keyword 'fun'
* Kotlin is a JVM Language so it should demand class Files like Java, why it doesn't ?
* Thats because Kotlin compiler internally creates a Class File which is loaded into memory for execution in runtime (when converted into class files)

```kotlin
fun main(args: Array<String>){      //function defined as main function with arguments taken by it in Array of <String>,as denoted by this line
    println("Hello World!!")
    println(10 / 2)
    println(10.0 / 2)
    print(true)
}
```

* `fun` determine, its a function
* `main` its a main function or say method
* `args` variable name of type -> Array of `<String>`
* `println` prints the inside statement with a newline char added '\n'
* `print` function prints everything inside it
* if int value inside print -> gives integer o/p, else if float or boolean-> gives float/boolean o/p

## Basic Syntax

```kotlin
fun main(args: Array<String>){
    var myString = "Hello World!" 
    var num = 99
    var decimal = 99.5
    var myChar = 'a'
    var bool = false
    
    val myAnotherStr = "My constant string value"
}
```
* `var` is to declare a `variable`
* `val` is to declare a `constant variable` - if its value is tried to change, will result in an error

## Function Basic

```kotlin
fun main(args: Array<String>){
    var name: String     //name is declared as String type
    name = "Zephyr"
    display(name)       //here name is parameter passed
}

fun display(name: String){      //here name is argument and declares as String type
    print("My name is: " + name)
}
```

## Basic String Interpolation

```kotlin
fun main(args: Array<String>){
    var obj = Person2()     //object obj of class Person is made
    obj.name = "Zephyr"         //name variable is referenced by the help of obj

    println("The name of the person is " + obj.name)
        //Or use string interpolation as by ${}
    print("The name of the person is ${obj.name}")
}

class Person{
    var name = ""
}
```

## Class Basic

```kotlin
fun main(args: Array<String>){
    var obj = Person()          //object obj of class Person is made
    obj.name = "Zephyr"         //name variable is referenced by the help of obj
    obj.display(obj.name)       //display function is referenced by the help of obj
}

class Person {
    var name = ""
    fun display(name: String) {      //here name is argument and declares as String type
        print("My name is: " + name)
    }
}
```

## Import Class from Package

```kotlin
package com.example

class Person {
    var name = ""
}
```

```kotlin
import com.example.Person

fun main(args: Array<String>){
    var obj = Person()     //object obj of class Person is made, which is called from inside package com.example
    obj.name = "Zephyr"    //name variable is referenced by the help of obj
    print("The name of the person is ${obj.name}")   //string interpolation is used
}
```

## Import Constructor from Package

```kotlin
package com.example

class Person4(var name: String) {
    fun display() {
        print("The name of the person is ${name}")
    }
}
```

```kotlin
import com.example.Person

fun main(args: Array<String>){

    var obj = Person("Zephyr")     //object obj of constructor class Person is made, which is called from inside package com.example
    obj.display()

}
```

