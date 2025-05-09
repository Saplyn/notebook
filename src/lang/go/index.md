# Go

- [Language Homepage](https://go.dev/)
- [Standard Library](https://pkg.go.dev/std)

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

## Types

```go
// declare a variable
var a int
var b int = 10 // with initialization
var c = 10     // type inferred
d := 10        // short hand

// declare multiple variables
var (
    e int
    f int = 10
    g = 10
    h := 10
)

// declare constants
const i int = 10
const j = 10
const (
    k int = 10
    l = 10
)

// array (fixed size)
var m [5]int
n := [5]int{1, 2, 3, 4, 5}
o := [...]int{1, 2, 3, 4, 5}  // size inferred

// slice (dynamic size)
p := []int{1, 2, 3, 4, 5}
q := n[1:3] // slice of n from index 1 to 3
r := n[:3]  // slice of n from index 0 to 3
s := make([]int, 5) // make a slice with length 5

// map (key-value)
t := map[string]int{
    "a": 1,
    "b": 2,
}
for k, v := range t {
    fmt.Println(k, v)
}

// types
int, int8, int16, int32, int64
uint, uint8, uint16, uint32, uint64, uintptr
float32, float64
complex64, complex128
byte // uint8
rune // (int32) a Unicode code point
string

// type conversion
i := 42
f := float64(i)
u := uint(f)
```

## Operators

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `<=`, `>`, `>=`
- Logical: `&&`, `||`, `!`
- Bitwise: `&`, `|`, `^`, `<<`, `>>`
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

## Flow Control

```go
// if-else
if a > 10 {
    fmt.Println("a is greater than 10")
} else if a < 10 {
    fmt.Println("a is less than 10")
} else {
    fmt.Println("a is equal to 10")
}

// switch
switch a {
case 1:
    fmt.Println("a is 1")
case 2:
    fmt.Println("a is 2")
default:
    fmt.Println("a is neither 1 nor 2")
}
switch {  // switch true for long-chain if-else
case t.Hour() < 12:
    fmt.Println("Good morning!")
case t.Hour() < 17:
    fmt.Println("Good afternoon.")
default:
    fmt.Println("Good evening.")
}

// for (while-style)
x := 0
for x < 5 {
    fmt.Println(x)
    x++
}

// for (C-style)
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// for (range)
y := []int{1, 2, 3, 4, 5}
for i, v := range y {
    fmt.Println(i, v)
}
for _, v := range y {  // ignore index
    fmt.Println(v)
}

// infinite loop
for {
    fmt.Println("infinite loop")
}

break    // exit loop
continue // skip to next iteration

// defer
func main() {
    defer fmt.Println("world")
    fmt.Println("hello")
} // output: hello world

// defer stack
func main() {
    for i := 0; i < 5; i++ {
        defer fmt.Println(i)
    }
} // output: 4 3 2 1 0
```

## Pointers

```go
// declaration
var a int = 10
var b *int = &a // b is a pointer to a
var nothing *int // `nil` pointer

// dereference
fmt.Println(*b) // 10

// pass by reference (pointer's value)
func change(a *int) {
    *a = 20
}
change(&a)     // `&` address of
fmt.Println(a) // 20
```

## Structs

```go
// declaration
type person struct {
    name string
    age  int
}

// initialization
p := person{"Alice", 20}
q := person{
    name: "Bob",
    age:  30,
}
fmt.Println(p.name) // access fields

// receiver function (method)
func (p person) greet() {
    fmt.Println("Hello, my name is", p.name)
}
p.greet() // call method

// pointer receiver function
func (p *person) grow() {
    p.age++  // auto dereferenced
}
p.grow()
```

## Interfaces

```go
// declaration
type drawable interface {
    info() string
    draw()
}

// implementation
type circle struct {
    radius int
}
func (c circle) info() string {
    return "Circle"
}
func (c circle) draw() {
    fmt.Println("Drawing circle")
}

// polymorphism
func drawShape(d drawable) {
    fmt.Println(d.info())
    d.draw()
}
drawShape(circle{10})

// empty interface
func describe(i interface{}) {  // any type
    fmt.Printf("(%v, %T)\n", i, i)
}
describe(42)
describe("hello")
```
