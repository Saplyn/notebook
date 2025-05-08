# C++

- [Standard Library](https://en.cppreference.com/w/cpp/header)

```cpp
#include <ctime>
#include <iostream>
#include <random>

int generate_secret_number(int min, int max) {
    std::mt19937 rng(time(0));
    return (rng() % (max - min + 1)) + min;
}

int main() {
    std::cout << "Guess the number!" << std::endl;

    int secret_number = generate_secret_number(1, 100);

    while (true) {
        std::cout << "Please input your guess: ";

        int guess;
        std::cin >> guess;

        std::cout << "You guessed: " << guess << std::endl;

        if (guess < secret_number) {
            std::cout << "Too small!" << std::endl;
        } else if (guess > secret_number) {
            std::cout << "Too big!" << std::endl;
        } else {
            std::cout << "You win!" << std::endl;
        break;
      }
    }

    return 0;
}
```

> Refer to [C](../c/index.md) for language basics.

## Types

- New primitive: `bool`
- Auto typing: `auto x = 5;`
- Copy typing: `decltype(x) y = x;`
- Type alias: `using Distance = double;`
- Null pointer: `int *p = nullptr;`
- Reference: `int &r = x;`

## Namespaces

```cpp
namespace lyn {
    int number;  // number is a member of namespace lyn (lyn::number)
}

int main() { lyn::number = 32; }
```

## Lambda Functions

```cpp
auto add = [/* capture */](int a, int b) { return a + b; };
add(1, 2); // => 3
```

## Dynamic Memory

```cpp
// new and delete
int* p = new int; // allocate memory for an int
*p = 1919;
delete p; // release memory

// new[] and delete[]
int* p = new int[5]; // allocate memory for 5 integers
p[0] = 1919;
delete[] p; // release memory
```
