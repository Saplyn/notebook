# Object Oriented

## Classes

```kotlin
// Class definition
class Contact(val id: Int, var email: String = "example@gmail.com") // header (primary constructor)
{
    val category: String = "work" // property with default value

    fun printId() { // method
        println(id)
    }
}

fun main() {
    val contact = Contact(1, "mary@gmail.com") // instantiating
    println(contact.email)

    contact.email = "jane@gmail.com"
    println(contact.email)

    contact.printId()
}
```

## Data Classes

Data classes are used to hold data. They automatically generate useful methods
like `equals()` (`==`), `toString()`, and `copy()`.

```kotlin
data class User(val id: Int, val name: String)

fun main() {
    val user1 = User(1, "Alice")
    val user2 = User(1, "Alice")
    println(user1 == user2) // true, compares data
    println(user1) // User(id=1, name=Alice)

    val user3 = user1.copy(name = "Bob") // creates a copy with modified name
    println(user3) // User(id=1, name=Bob)
}
```
