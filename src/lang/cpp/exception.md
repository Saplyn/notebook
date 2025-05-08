# Exception

## Exception Handling

When executing a program, error occurs. It's important to handle the error properly.
In C++, exception is the idiomatic way to handle error.

```cpp
// `try` tries to execute the block of code
// and hands over to `catch` if an exception is thrown
try {
    int numerator = 10;
    int denominator = 0;
    int result = numerator / denominator;  // Potential division by zero

    // This line won't be executed if an exception is thrown above
    std::cout << "Result: " << result << std::endl;
} catch (const std::exception& e) {
    // handle the exception
    std::cout << "An exception occurred: " << e.what() << std::endl;
}
```

```cpp
// `catch` can be chained to handle specific exceptions
try {
    std::string s = "hello";
    s[10] = 'x';  // Accessing an invalid index throws an out_of_range exception
} catch (const std::out_of_range& e) {
    std::cout << "Out of range exception: " << e.what() << std::endl;
} catch (const std::exception& e) {
    std::cout << "An exception occurred: " << e.what() << std::endl;
} catch (...) {
    std::cout << "An unknown exception occurred." << std::endl;
}
```

```cpp
// `throw` can be used to throw an exception manually
void func() {
    // when exception is thrown, the control is transferred to the nearest `catch` block
    throw std::runtime_error("An error occurred in func()");
    // unreachable code
    std::cout << "This line won't be executed" << std::endl;
}
try {
    func();
} catch (const std::exception& e) {
    std::cout << "An exception occurred: " << e.what() << std::endl;
}
```

## `noexcept` Guarantee

The `noexcept` keyword is used to declare that a function does not throw any
exceptions. If a function marked with `noexcept` does throw an exception, the
`std::terminate` function is called, terminating the program.

```cpp
// won't fail guarantee
void func() noexcept { /* ... */ }

// specialized won't fail
template <> void func<int>() noexcept { /* ... */ }
template <> void func() noexcept(sizeof(T) == 4) { /* ... */ }
```

## Custom Exception

We can define custom exception by inheriting from `std::exception`.

```cpp
#include <stdexcept>

class MyException : public exception {
private:
    std::string m_message;
public:
    MyException(const std::string& msg) : m_message(msg) {}

    std::string what() override { return m_message; }
};

try {
    throw MyException("An error occurred in func()");
} catch (const std::exception& e) {
    std::cout << "An exception occurred: " << e.what() << std::endl;
}
```

```cpp
// specialized exception
#include <stdexcept>

class RunOutOfMemory : public std::runtime_error {
public:
    RunOutOfMemory(const std::string& msg) : std::runtime_error(msg) {}
};
```
