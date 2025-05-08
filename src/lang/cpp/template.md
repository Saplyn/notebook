# Template

## Generics

In C++, templates are a way to make functions and classes generic, so that they
can handle any data type. This happens at compile time (static dispatch).

```cpp
// `T` is a type parameter, it can be any type
template <typename T>
void swap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}
int a = 1, b = 2;
swap<int>(a, b);
double c = 1.1, d = 2.2;
swap<double>(c, d);
```

```cpp
// multiple type parameters also possible
template <typename T, typename U>
class Pair { /* ... */ };
Pair<int, double> p;
Pair<int, int> q;
```

It's possible to create specialized versions of a template for specific types.

```cpp
// generic version
template <typename T>
void print(const T& value) { /* ... */ }

// specialized version for `std::string`
template <>
void print<std::string>(const std::string& value) { /* ... */ }
```

## Variadic Templates

Variadic templates allow a function or class to accept any number of arguments of
any type.

```cpp
// base case
void print() { std::cout << std::endl; }
// recursive case
template <typename T, typename... Args>
void print(const T& first_arg, const Args&... args) {
    std::cout << first_arg << ' ';
    print(args...);
}
print(1, 2.2, "three");
```

```cpp
// `sizeof...(args)` gives the number of arguments
template <typename... Args>
void count_args(const Args&... args) {
    std::cout << sizeof...(args) << std::endl;
}
count_args(1, 2.2, "three"); // => 3
```

## Meta-programming

`constexpr` is a keyword used to declare a constant expression. It indicates
that a variable or function can be evaluated at compile-time and its value
can be used in other compile-time expressions.

```cpp
// constexpr variables
constexpr int length = 10;
constexpr int width = 4 + 6;

// constexpr functions
constexpr int factorial(int n) {
    return n <= 1 ? 1 : (n * factorial(n - 1));
}
```

We can use meta-programming techniques to perform calculations and generate
compile-time constants or data structures to optimize code or providing
compile-time configuration options.

```cpp
// compile-time Fibonacci sequence
template <int N>
struct Fibonacci {
    static const int value = Fibonacci<N - 1>::value + Fibonacci<N - 2>::value;
};
template <> struct Fibonacci<0> { static const int value = 0; };
template <> struct Fibonacci<1> { static const int value = 1; };

// usage
int fib = Fibonacci<10>::value; // => 55
```
