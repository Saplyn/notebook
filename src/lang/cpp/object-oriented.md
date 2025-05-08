# Object Oriented

## Class as Object

Like `struct` in C, `class` is a user-defined data type that can hold data
members. Unlike `struct`, `class` can also hold member functions, known as
method.

```cpp
class Point {
// access specifier: only accessible within the class
private:
  double x;
  double y;

// access specifier: accessible outside the class
public:
  void set(double x, double y) {
    // `this` pointer points to the caller object
    this->x = x;
    this->y = y;
  }

  // const method promises not to modify the object
  double dist(const Point& that) const {
    return sqrt(
      // `this` is optional
      (x - that.x) * (x - that.x) +
      (y - that.y) * (y - that.y)
    );
  }
};  // don't forget the semicolon

Point p;   // create an object of class Point
p.dist();  // access the member function
```

## Constructor & Destructor

A constructor is a special member function that is called when an object is
created, used to initialize the object.

A destructor on the other hand is called when an object is destroyed, used
to release allocated resources.

```cpp
class Vector {
private:
  double* elem;
  int sz;

public:
  // default constructor
  Vector() {
    elem = new double[10];
    sz = s;
  }

  // constructor with parameter
  Vector(int s) : // `:` is the initializer list
    elem{new double[s]},
    sz{s}
  {
    for (int i = 0; i < s; ++i) { elem[i] = 0; }
  }

  // destructor
  ~Vector() { delete[] elem; }

  // copy constructor
  Vector(const Vector& a) : elem{new double[a.sz]}, sz{a.sz} {
    for (int i = 0; i < sz; ++i) { elem[i] = a.elem[i]; }
  }

  // move constructor
  Vector(Vector&& a) : elem{a.elem}, sz{a.sz} {
    a.elem = nullptr;
    a.sz = 0;
  }
};

class Point {
private:
  double x;
  double y;

public:
  Point() = default;      // default constructor (compiler generated)
  Point(const Point&) = delete; // no copy constructor (prevent copy)
  Point(Point&&) = delete;    // no move constructor (prevent move)
};
```

## Sharing

`friend` keyword grants access to private members of a class to another
class or function.

```cpp
// forward declaration
class List;
class Node;

class Node {
  friend class List; // List can access private members of Node
private:
  int data;
  Node* next;
};

class List {
private:
  Node* head;
  Node* tail;
};
```

`static` keyword makes a member shared by all instances of the class.

```cpp
class Counter {
private:
  static int count; // shared by all instances of Counter
public:
  Counter() { ++count; }
  ~Counter() { --count; }
  static int getCount() { return count; }
};

int Counter::count = 0; // initialize the static member
Counter a;
Counter::getCount(); // => 1
Counter b, c;
Counter::getCount(); // => 3
```

## Inheritance

```cpp
// base class
class Animal {
// access specifier: accessible within the class and derived class
protected:
  int age;
public:
  void type() { std::cout << "I'm an animal!" << std::endl; }
  void eat() { std::cout << "yum yum~" << std::endl; }
};

// derived class
class Cat : public Animal {
public:
  // override the base class method
  void type() { std::cout << "I'm a cat!" << std::endl; }
  // implement a new method
  void meow() {
    std::cout << "mew mew~ I'm "
              << age  // inherited from Animal
              << " years old!"
              << std::endl;
  }
};

Cat pet;
pet.eat();  // inherited from Animal
pet.meow(); // defined in Cat
```

Sometimes multiple classes share a common base class, and they need to access
the common base class members. In this case, virtual inheritance is used, to
avoid ambiguity (the common base class is inherited only once).

```cpp
class Animal { /* ... */ };
class Dog : virtual public Animal { /* ... */ };
class Cat : virtual public Animal { /* ... */ };
class SiameseCat : public Dog, public Cat { /* ... */ };
```

When defining a base class, we can use `virtual` keyword to make a method
overridable by derived classes; `override` keyword to ensure that the
method is indeed overriding a base class method; and `final` keyword to
mark a method as not overridable.

It's also possible to define a pure virtual method, which has no implementation
and must be overridden by derived classes.

```cpp
class Animal {
public:
  // virtual method (with default implementation)
  virtual void info() { std::cout << "I'm an animal!" << std::endl; }
  // pure virtual method (no implementation, must be overridden)
  virtual void eat() = 0;
};

class Feline : public Animal {
public:
  // override the base class method
  void info() override { std::cout << "I'm a feline!" << std::endl; }
  // override the pure virtual method
  void eat() override { std::cout << "I eat meat!" << std::endl; }
};

class Cat : public Feline {
public:
  // make final overrides
  void info() override final { std::cout << "I'm a cat!" << std::endl; }
  void eat() override final { std::cout << "I eat fish!" << std::endl; }
};
```

## Polymorphism

Polymorphism is the ability to treat objects of different derived classes
through a common interface (base class).

```cpp
// dynamic dispatch (runtime polymorphism)
Cat saplyn;
Animal* ani_ptr = &saplyn;
Animal& ani_ref = saplyn;

ani_ptr->type(); // calls Cat::type()
ani_ref.type();  // calls Cat::type()

// however, the following will result in slicing
Animal ani = saplyn; // only the base part of saplyn is copied
ani.type();      // calls Animal::type()
```

## Operator Overloading

The operators that can be overloaded are listed below:

- Binary Operators: `+`, `-`, `*`, `/`, `%`, `^`, `&`, `|`, `=`, `<<`, `>>`, `,`
  , `->*`, `->`, `[]`
- Comparison Operators: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Short-Circuit Operators: `&&`, `||`
- Unary Operators: `~`, `!`
- Pre/Post Unary Operators: `++`, `--`
- Function Call Operator: `()`

Note that `.` (dot operator) and `? :` (ternary operator) cannot be overloaded.
Additionally, the behavior of some operators (like the short-circuit behavior
of logical operators `&&` and `||`) cannot be changed.

```cpp
class Point {
private:
  double x;
  double y;
public:
  Point() = default;
  Point(double x = 0, double y = 0) : x(x), y(y) {}

  double get_x() const { return x; }
  double get_y() const { return y; }
};

// overload the `+` operator
Point operator+(const Point& a, const Point& b) {
  return Point{a.get_x() + b.get_x(), a.get_y() + b.get_y()};
}

// overload the `<<` operator
std::ostream& operator<<(std::ostream& os, const Point& p) {
  return os << '(' << p.get_x() << ", " << p.get_y() << ')';
}
```

In C++, some operators have multiple forms (pre/post-incr, etc.), they are
overloaded with different signatures.

```cpp
// pre-increment (++a)
Type& operator++();
// post-increment (a++)
Type operator++(int);

// l-value []
Type& operator[](int i);
// r-value []
Type operator[](int i) const;
```
