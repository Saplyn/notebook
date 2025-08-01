# Kotlin

- [Official Website](https://kotlinlang.org/)
- [Standard Library](https://kotlinlang.org/api/core/kotlin-stdlib/)

```kotlin
import kotlin.random.Random

fun generateSecretNumber(min: Int, max: Int): Int = Random.nextInt(min, max + 1)

fun main() {
    println("Guess the number!")

    val secretNumber = generateSecretNumber(1, 100)

    while (true) {
        println("Please input your guess.")

        val input = readLine()
        if (input.isNullOrBlank()) continue

        val guess = input.trim().toIntOrNull() ?: continue

        println("You guessed: $guess")

        when {
            guess < secretNumber -> println("Too small!")
            guess > secretNumber -> println("Too big!")
            else -> {
                println("You win!")
                break
            }
        }
    }
}
```

## Variables

- Variables:
  - `val`: define read-only variable
  - `var`: define mutable variable
- Basic types:
  - `Byte`, `Short`, `Int`, `Long`
  - `UByte`, `UShort`, `UInt`, `ULong`
  - `Float`, `Double`
  - `Boolean`, `Char`, `String`
- Null safety:
  - Nullable: `String?`, `Int?` (use `?` to allow null values)
  - Null check: `if (variable != null) {}`
  - Safe call: `variable?.length` (returns `null` if variable is `null`)
  - Elvis operator: `variable ?: defaultValue` (returns `defaultValue` if `null`)

## Operators

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `<=`, `>`, `>=`
- Logical: `&&`, `||`, `!`
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

## Branching

```kotlin
// if-else expression
if (condition) { /* ... */ }
else { /* ... */ }

// when expression
when (value) {
    1 -> println("One")
    2 -> println("Two")
    else -> println("Something else")
}

// when expression (without a subject)
when {
    condition1 -> println("Condition 1 is true")
    condition2 -> println("Condition 2 is true")
    else -> println("Neither condition is true")
}
```

## Looping

```kotlin
// range
1..5        // 1, 2, 3, 4, 5
1..<5       // 1, 2, 3, 4
5 downTo 1  // 5, 4, 3, 2, 1
1..5 step 2 // 1, 3, 5

// for loop
for (i in 1..5) { println(i) }
for (cake in listOf("carrot", "cheese", "chocolate")) {
    println("$cake cake")
}

// while loop
while (condition) { /* ... */ }

// do-while loop
do { /* ... */ } while (condition)

```

## Collections

### Lists

```kotlin
// Read only list
val readOnlyShapes = listOf("triangle", "square", "circle")
println(shapes)  // [triangle, square, circle]

// Mutable list with explicit type declaration
val shapes: MutableList<String> = mutableListOf("triangle", "square", "circle")

// Create a read-only view of a mutable list by assigning it to a List
val shapesLocked: List<String> = shapes

// Accessing items in a list by indexing
println("The first item in the list is: ${readOnlyShapes[0]}")

// Checking if an item exists in a list
println("circle" in readOnlyShapes)
```

### Sets

```kotlin
// Read-only set
val readOnlyFruit = setOf("apple", "banana", "cherry", "cherry")

// Mutable set with explicit type declaration
val fruit: MutableSet<String> = mutableSetOf("apple", "banana", "cherry", "cherry")

// Create a read-only view of a mutable set by assigning it to a Set
val fruitLocked: Set<String> = fruit

// Adding & removing items from a mutable set
fruit.add("orange")
fruit.remove("banana")

// Checking if an item exists in a set
println("apple" in readOnlyFruit)
```

### Maps

```kotlin
// Read-only map
val readOnlyJuiceMenu = mapOf("apple" to 100, "kiwi" to 190, "orange" to 100)
println(readOnlyJuiceMenu) // {apple=100, kiwi=190, orange=100}

// Mutable map with explicit type declaration
val juiceMenu: MutableMap<String, Int> = mutableMapOf("apple" to 100, "kiwi" to 190, "orange" to 100)

// Create a read-only view of a mutable map by assigning it to a Map
val juiceMenuLocked: Map<String, Int> = juiceMenu

// Accessing items in a map by key
println("The value of apple juice is: ${readOnlyJuiceMenu["apple"]}") // 100
println("The value of pineapple juice is: ${readOnlyJuiceMenu["pineapple"]}") // null

// Adding & removing items from a mutable map
juiceMenu["coconut"] = 150 // Add key "coconut" with value 150 to the map
juiceMenu.remove("orange") // Remove key "orange" from the map
```
