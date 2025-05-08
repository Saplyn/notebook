# Object Oriented

## Class as Object

In Java, every item has an access specifier:

- `public`: accessible from anywhere
- `protected`: accessible within the package and by subclass
- (default): accessible within the package
- `private`: accessible within the class

```java
public class Point {
    // data members
    private double x;
    private double y;

    // method
    public void set(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double dist(Point that) {
        return Math.sqrt(
            (x - that.x) * (x - that.x) +
            (y - that.y) * (y - that.y)
        );
    }
}

Point p = new Point();
p.set(3.0, 4.0);
```

Classes may have `static` members, which are shared by all objects (instances)
of the same class (type).

```java
public class Student {
    // fields
    String name;
    int age;

    // constructor
    Student(String name, int age) {
        /* ... */
    }

    // method
    void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
    }
}
```

## Copy & Reference

In Java, everything that is not a primitive type is a reference type. When an
object is assigned to another object, the reference is copied, not the object
itself.

```java
// shallow copy
// `obj1` and `obj2` now share the same memory address (reference)
Object obj1 = new Object();
Object obj2 = obj1;

// deep copy constructor
public class MyClass {
    private int number;
    private String text;

    // deep copy constructor
    public MyClass(MyClass original) {
        this.number = original.getNumber();
        this.text = original.getText();
    }
}
```

## Finalize Method

Java is garbage-collected, so there is no need to explicitly release resources.
We can, however, `@Override` the `finalize` method to release resources before
the object is garbage-collected.

```java
public class Student {
    /* ... */

    @Override
    protected void finalize() throws Throwable {
        // Perform cleanup actions before object is garbage collected
        super.finalize();  // call the finalize method of the superclass
        System.out.println("Finalizing Student object: " + name);
    }
}
```

## Inheritance

Every class may `extend` another class, inheriting its members and making it
its superclass. A subclass may `@Override` a method of its superclass. A subclass
may access the members of its superclass via `super` keyword.

```java
// Animal.java
public class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }

    public void makeSound() {
        System.out.println("The animal makes a sound.");
    }
}

// Dog.java
public class Dog extends Animal { // extends superclass
    public Dog(String name) {
        super(name);  // call superclass constructor
    }

    // override superclass method
    @Override
    public void makeSound() {
        super.makeSound();  // access superclass member
        System.out.println("Woof woof!");
        System.out.println("My name is " + super.name + "!");
    }
}
```

They can be `abstract` classes with `abstract` methods, which cannot be
instantiated. They serve as blueprints for other classes.

```java
// Animal.java
// abstract class, cannot be instantiated
public abstract class Animal {
    // abstract method, must be overridden by subclass
    public abstract void makeSound();

    // concrete method as default implementation
    public void sleep() {
        System.out.println("The animal is sleeping.");
    }
}
```

the `final` keyword prevents a class or method from being overridden.

```java
// final class, cannot be extended
public final class NotExtendable {
    /* ... */
}

public class SomeClass {
    // final method, cannot be overridden
    public final void norOverridable() {
        System.out.println("Not overridable.");
    }
}
```

## Interface

Interface is a contract defined for a class. It defines a set of abstract
methods that a class can `implement`. A class may implement multiple
interfaces.

```java
// interface
interface Stringify {
    // method blueprint
    String toString();
}
interface Comparable {
    // default implementation
    default int compareTo(Object obj) {
        return 0;
    }
}

// class implementing the interface
class MyClass implements Stringify, Comparable {
    @Override
    public String toString() {
        return "MyClass";
    }

    @Override
    public int compareTo(Object obj) {
        /* ... */
    }
}

Stringify obj = new MyClass();
System.out.println(obj.toString());
```

Interfaces may also `extend` other interfaces.

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

interface Walkable {
    void walk();
}

interface Amphibious extends Flyable, Swimmable, Walkable {
    void land();
}
```

## Polymorphism

```java
public class Animal {
    public void makeSound() { System.out.println("Animal is making a sound"); }
}
public class Dog extends Animal {
    @Override
    public void makeSound() { System.out.println("Dog is barking"); }
}
public class Cat extends Animal {
    @Override
    public void makeSound() { System.out.println("Cat is meowing"); }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal();
        Dog dog = new Dog();
        Cat cat = new Cat();

        System.out.println(animal instanceof Animal); // true
        System.out.println(dog instanceof Animal); // true
        System.out.println(cat instanceof Animal); // true
        System.out.println(dog instanceof Dog); // true
        System.out.println(cat instanceof Dog); // false


        Animal animal1 = new Animal();
        Animal animal2 = new Dog();
        Animal animal3 = new Cat();

        animal1.makeSound();  // Output: Animal is making a sound
        animal2.makeSound();  // Output: Dog is barking
        animal3.makeSound();  // Output: Cat is meowing
    }
}
```
