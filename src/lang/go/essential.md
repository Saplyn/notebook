# Essential

## Error Handling

Go programs express error state with `error` values, a built-in interface.

```go
// https://go.dev/tour/methods/19
// the error type
type error interface {
    Error() string
}

// failable & handling error
import "fmt"
i, err := strconv.Atoi("42")
if err != nil {
    fmt.Printf("couldn't convert number: %v\n", err)
    return
}
fmt.Println("Converted integer:", i)

// custom error
import "time"
type MyError struct {
    When time.Time
    What string
}
func (e *MyError) Error() string {
    return fmt.Sprintf("at %v, %s", e.When, e.What)
}
func run() error {
    return &MyError{ time.Now(), "it didn't work", }
}
```

## Generics

Go functions can be written to work on multiple types using type parameters.
The type parameters of a function appear between brackets, before the
function's arguments.

```go
// https://go.dev/tour/generics/1
// generic function
func Index[T comparable](s []T, x T) int {
    for i, v := range s {
        // v and x are type T, which is `comparable`
        if v == x {
            return i
        }
    }
    return -1
}

// generic type
type Stack[T any] struct {
    data []T
}
```

## Goroutines

A goroutine is a lightweight thread managed by the Go runtime.

```go
// https://go.dev/tour/concurrency/1
func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}
// `go` starts a new goroutine
func main() {
    go say("world")
    say("hello")
}
```

## Channels

Channels are a typed conduit through which we can send and receive
values with the channel operator `<-`. By default, sends and
receives block until the other side is ready.

```go
// https://go.dev/tour/concurrency/2
func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // send sum to c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    // make a channel of int
    c := make(chan int)
    // start two goroutines
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c // receive from c

    fmt.Println(x, y, x+y)
}
```

A sender can close a channel to indicate that no more values will be sent.
Receivers can test whether a channel has been closed by assigning a second
parameter to the receive expression.

```go
// https://go.dev/tour/concurrency/4
func fibonacci(n int, c chan int) {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        c <- x
        x, y = y, x+y
    }
    close(c)  // close the channel
    // can be tested by `v, ok := <-c`
}

func main() {
    // make a buffered channel
    c := make(chan int, 10)
    go fibonacci(cap(c), c)
    // receive from the channel until it's closed
    for i := range c {
        fmt.Println(i)
    }
}
```

## Select

The `select` statement lets a goroutine wait on multiple communication
operations. A `select` blocks until one of its cases can run, then it
executes that case. It chooses one at random if multiple are ready.

```go
// https://go.dev/tour/concurrency/6
func main() {
    tick := time.Tick(100 * time.Millisecond)
    boom := time.After(500 * time.Millisecond)
    for {
        select {
        case <-tick:
            fmt.Println("tick.")
        case <-boom:
            fmt.Println("BOOM!")
            return
        default:
            fmt.Println("    .")
            time.Sleep(50 * time.Millisecond)
        }
    }
}
```
