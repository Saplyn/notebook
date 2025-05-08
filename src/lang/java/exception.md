# Exception

## Try, Catch & Finally

```java
BufferedReader reader = null;

try {  // Code that might throw an exception
    reader = new BufferedReader(new FileReader("example.txt"));
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (FileNotFoundException e) {  // Handle the exception
    System.out.println("File not found: " + e.getMessage());
} catch (IOException e) {  // Handle the exception
    System.out.println("Error reading file: " + e.getMessage());
} finally {  // This code will always run
    try {
        if (reader != null) {
            reader.close();
        }
    } catch (IOException e) {
        System.out.println("Error closing file: " + e.getMessage());
    }
}
```

## Throw & Throws Marker

The `throw` statement is used to explicitly throw an exception. And the
`throws` tells the compiler that a method may throw an exception.

```java
public void checkNumber(int num) throws NegativeNumberException {
    if (num < 0) {
        throw new NegativeNumberException("Number is negative: " + num);
    }
}
```

Their are two types of exceptions in Java: checked and unchecked. The checked
exceptions are checked at compile-time, while the unchecked exceptions are not.

```java
try {
    FileInputStream file = new FileInputStream("myFile.txt");
} catch (FileNotFoundException e) {
    // Handle the checked exception (e.g., provide an alternative file)
}

// ArithmeticException, an unchecked exception
int result = 10 / 0;
```

It's possible to define a custom exception by extending the `Exception` class.

```java
class CustomCheckedException extends Exception {
    public CustomCheckedException(String message) {
        super(message);
    }
}

class MyUncheckedException extends RuntimeException {
    // Constructor and additional methods
}
```
