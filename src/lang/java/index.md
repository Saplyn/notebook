# Java

- [SDKMAN - Software Development Kit Manager](https://sdkman.io/)

```java
// hello world in Java
// `HelloWorld.java`
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello world!");
    }
}
```

## Types

- Primitives: `int`, `char`, `float`, `double`
- Constants: `final`
- Array: `int[] arr = new int[5];`

## Operators

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `<=`, `>`, `>=`
- Logical: `&&`, `||`, `!`
- Bitwise: `&`, `|`, `^`, `<<`, `>>`, `>>>` (unsigned right shift)
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

## Control Flow

```java
// if statement
if (condition) {
    // ...
} else if (condition) {
    // ...
} else {
    // ...
}

// switch statement
switch (expression) {
    case value1:
        // ...
        break;
    case value2:
        // ...
        break;
    default:
        // ...
}

// while loop
int i = 0;
while (i < 10) {
    System.out.println(i);
}

// do-while loop
int i = 0;
do {
    System.out.println(i);
} while (i < 10);

// for loop
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// for each loop
int[] numbers = {1, 2, 3, 4, 5};
for (int number : numbers) {
    System.out.println(number);
}

break;    // exits the loop
continue; // skips the current iteration.

// label jump
outerLoop:
for (int i = 0; i < 5; i++) {
    innerLoop:
    for (int j = 0; j < 5; j++) {
        if (i == 1 && j == 1) {
            // continue with the next iteration of the inner loop
            continue innerLoop;
        }
        if (i == 2 && j == 2) {
            // break out of the outer loop
            break outerLoop;
        }
        System.out.println(i);
    }
}
```
