# Mutating Functions in Swift Structs

In Swift, `structs` (structures) are **value types**, meaning that when they are copied, the copy is completely independent of the original. By default, the properties of a value type **cannot be modified** from within an instance method, because doing so would modify the value itself.
To allow an instance method to modify the properties of a struct, you must mark that method with the `mutating` keyword.

## 1. The Need for mutating
Since structs are value types, an instance method is normally unable to change the struct's properties. Swift enforces this to ensure **predictable data flow** and prevent accidental side effects when working with copies of the struct.

**Example without `mutating` (will result in a compiler error):**

```swift
struct Point {
    var x: Int
    var y: Int

    // ❌ Error: Cannot assign to property 'self' is immutable
    func moveX(by deltaX: Int) {
        x += deltaX // This line is an error!
    }
}
```

## 2. Using the `mutating` Keyword
By adding the `mutating` keyword to the method definition, you explicitly signal to the compiler that this method is allowed to modify the struct's properties.

````swift
struct Point {
    var x: Int
    var y: Int

    // ✅ Add the 'mutating' keyword
    mutating func moveX(by deltaX: Int) {
        x += deltaX // Now allowed!
    }
}

// ⚠️ Important: You can only call mutating methods
// on variables (var), not constants (let).

var p = Point(x: 10, y: 5)
print("Before move: \(p.x)") // Output: Before move: 10

p.moveX(by: 5) // The 'moveX' method mutates 'p'

print("After move: \(p.x)")  // Output: After move: 15
````

## 3. Mutating and the self Property
A mutating method can also assign a completely new instance to the implicit `self` property, replacing the original instance entirely. This is useful for entirely redefining the value of the struct.

````swift
struct Point {
    var x: Int
    var y: Int

    mutating func resetToOrigin() {
        // Assigning a brand new 'Point' instance to 'self'
        self = Point(x: 0, y: 0)
    }
}

var p = Point(x: 100, y: 200)
p.resetToOrigin()

print("New point: (\(p.x), \(p.y))") // Output: New point: (0, 0)
````

## 4. Constraints on Mutating Methods
- **Variables Only:** You can only call a mutating method on an instance that was declared as a variable (`var`). You cannot call a mutating method on an instance declared as a constant (`let`).

- **Protocol Requirement:** If a struct adopts a protocol that requires a method to be mutating, the protocol's requirement must also be marked with `mutating`.