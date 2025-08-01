# Function & Lambda

## Functions

```kotlin
// function with parameters and return type
fun sum(x: Int, y: Int): Int {
    return x + y
}
sum(3, 5)
sum(y = 5, x = 3) // named arguments

// default parameters
fun greet(name: String = "World") {
    println("Hello, $name!")
}
greet() // Hello, World!
greet("Saplyn") // Hello, Saplyn!

// single-expression function
fun multiply(x: Int, y: Int) = x * y
```

## Lambda Expressions

```kotlin
// lambda function
val square: (Int) -> Int = { x: Int -> x * x }
println(square(4)) // 16

// lambda invoke in place
println({ text: String -> text.uppercase() }("hello"))

// no parameters & "no" return type
val unitFunction: () -> Unit = { println("Hello, Kotlin!") }

// trailing lambda
listOf(1, 2, 3).fold(0, { x, item -> x + item }) // 6
listOf(1, 2, 3).fold(0) { x, item -> x + item }  // 6 (trailing lambda)
```

## Extension Functions

To add new functionality to existing classes without modifying their source code,
we can define extension functions. They are defined with a _receiver type_ followed
by a dot and then the _function name_.

```kotlin
fun String.bold(): String = "<b>$this</b>"
// `String` is the receiver type
// `bold` is the function name
// `this` refers to the receiver object

fun main() {
    println("hello".bold()) // <b>hello</b>
}
```

Extension-oriented programming is powerful and common in Kotlin:

```kotlin
class HttpClient {
    fun request(method: String, url: String, headers: Map<String, String>): HttpResponse {
        println("Requesting $method to $url with headers: $headers")
        return HttpResponse("Response from $url")
    }
}

fun HttpClient.get(url: String): HttpResponse = request("GET", url, emptyMap())

fun main() {
    val client = HttpClient()

    // Making a GET request using request() directly
    val getResponseWithMember = client.request("GET", "https://example.com", emptyMap())

    // Making a GET request using the get() extension function
    val getResponseWithExtension = client.get("https://example.com")
}
```
