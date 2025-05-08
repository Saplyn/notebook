# Generic

## Type Parameter

To work with generic object in Java, we define classes or interfaces with
`<T>`.

```java
public class Box<T> {
    private T value;

    public Box(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}

Box<int> intObj = new Box<>(42);
Box<String> strObj = new Box<>("Hello, Generics!");

int intValue = intObj.getData();
String strValue = strObj.getData();
```

We can also use generic with methods.

```java
public <T> T findMax(T[] arr) {
    if (arr == null || arr.length == 0) {
        return null;
    }

    T max = arr[0];
    for (T item : arr) {
        if (item.compareTo(max) > 0) {
            max = item;
        }
    }
    return max;
}
```
