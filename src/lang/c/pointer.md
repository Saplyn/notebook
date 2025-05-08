# Pointer

## Basic

Pointers are a special type of variable, it stores the memory address of another
variable, instead of the value of the variable itself.

```c
int a = 1;

// `&` is the address-of operator
int *p = &a; // `p` is a pointer to `a`

// `*` is the dereference operator
*p = 2; // `a` is now 2
```

Pointers support pointer arithmetic, which "shifts" the pointer to another
location in memory.

```c
int arr[3] = {1, 2, 3};
int *p = arr; // `p` points to `arr[0]`

p++; // now points to `arr[1]`
p++; // now `arr[2]`
```

## Void Pointer

Void pointers are generic pointers that can point to any data type. They are
declared using the `void` keyword.

```c
int a = 1;
float b = 2.0;

void *p;
p = &a; // `p` points to an `int`
p = &b; // `p` points to a `float`
```

## Function Pointer

Pointers can be used to point to functions, which allows for dynamic function
calls.

```c
int add(int a, int b) {
    return a + b;
}

// return type is `int`, takes two `int` parameters
int (*func_ptr)(int, int) = add; // `func_ptr` points to `add`

func_ptr(1, 2); // calls `add(1, 2)`
```
