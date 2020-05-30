# Functions

## `Function` as `expression`

* `function` can be used as an `expression` to directly return a value without return statement

```kotlin
fun main(args: Array<String>) {
    var largeValue = max(4, 6)
    println("greater no.= $largeValue")
}

fun max(a: Int, b: Int): Int = if (a > b) {
    println("$a is greater")
    a       //returns the last statement from a codeblock
} else {
    println("$b is greater")
    b       //returns the last statement from a codeblock
}
```

## Default Argument

* Java does not support default function, so there's a problem in interoperability, i.e. in calling a default from a Kotlin file to Java file
* So you pass `@JvmOverloads` notation
* This will make a Kotlin function compatible with Java file.
* Used just above the function in a Kotlin File

```kotlin
fun main(args: Array<String>){
    var result: Int
    result = findVolume(2,3)
    println(result)     // Output - 60
    result = findVolume(2,3,5)
    println(result)     // Output - 30
}

// height is a default argument with default value 10

@JvmOverloads
fun findVolume(length: Int, breadth: Int, height: Int = 10): Int{
    return length * breadth * height
}
```

## Named Parameter

* Pure Kotlin Functionality, not supported in Java, so not Interoperable
* Kotlin include a feature of naming the parameters of the function
* So that their position can be interchanged, but that doesn't change anything

```kotlin
fun main(args: Array<String>){
    findVol(length = 2, breadth = 3, height = 30)
    findVol(height = 30, length = 2, breadth = 3)
}

fun findVol(length: Int, breadth: Int, height: Int){
    println("length = $length")
    println("breadth = $breadth")
    println("height = $height")
}
```

## Extension Function

* To add a new function to the existing /predefined class without declaring it inside the class
* The new function behaves just like static function in case of java

```kotlin
fun main(args: Array<String>){
    var student = Student()
    println("Pass status: " + student.hasPassed(57))
    println("Scholarship status: " + student.isScholar(57))
}

// Extension Function (which is extended inside the class Student)
fun Student.isScholar(marks: Int): Boolean{
    return marks > 95
}


class Student{
    fun hasPassed(marks: Int): Boolean{
        return marks > 33
    }
}

//This is actually not the practical usage of Extension Function
```

* Since `String` class has no function for concatenation of strings
* And we are going to add a function for "adding strings" to the `String` Class

```kotlin
fun main(args: Array<String>){

    var str1: String = "John"
    var str2: String = "Snow"
    var str3: String = "Hey"

    println(str3.add(str1, str2))

    // calling add function from String Class by the use of str3 which is an object of String class
}

// now this extension function add() is a part of String Class
fun String.add(s1: String, s2: String): String{

    return this+s1+s2
}

// this keyword is actually referring to str3
```


