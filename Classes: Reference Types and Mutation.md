# Classes: Reference Types and Mutation

Unlike structs and enums, **Classes** in Swift are **reference types**. This fundamental difference changes how they handle mutation entirely.

Here is the most important rule to remember: You generally *do NOT use the* `mutating` **keyword in classes.**

## 1. Why Classes Don't Need `mutating`

When you assign a class instance to a variable, you are storing a **reference** (a pointer) to where that object lives in memory.

When you modify a property of a class, you are changing the data sitting at that memory address. You are not replacing the instance itself or changing the pointer. Therefore, Swift allows methods to modify properties without any special keywords.

**Example:**
````swift
class Car {
    var speed: Int = 0
    
    // ✅ No 'mutating' keyword is needed here!
    func accelerate() {
        speed += 10
    }
}

let myCar = Car()
myCar.accelerate() 
print(myCar.speed) // Output: 10
````

## 2. The `let` Constant Surprise
This is often the most confusing part for developers transitioning to Swift. You can modify the properties of a class instance assigned to a `let` constant.

- **Struct** (`let`): The value is frozen. You cannot change properties.
- **Class** (`let`): The reference is frozen. You cannot make the variable point to a different car, but you can change the internal data of the car it points to.

````swift
class User {
    var name: String
    init(name: String) { self.name = name }
}

// 'user' is a constant
let user = User(name: "Alice")

// ✅ ALLOWED: We are changing the data inside the object
user.name = "Bob" 

// ❌ ERROR: We cannot change the reference itself
// user = User(name: "Charlie") // Cannot assign to value: 'user' is a 'let' constant
````

## 3. Summary: Struct vs. Class Mutation

Here is a quick comparison to visualize the differences:

| Feature | Struct / Enum | Class |
| :--- | :--- | :--- |
| **Type** | Value Type (Copies data) | Reference Type (Shares data) |
| **Mutation Keyword** | **Required** (`mutating`) | **Not Used** |
| `let` **Behavior** | Instance is immutable. | Reference is immutable; properties are mutable. |
| **Copying** | Copying creates a unique instance. Modifying one does not affect the other. | Copying creates a shared reference. Modifying one **updates all**. |

## 4. Shared State (The Risk)

Because classes share data, if you pass a class instance to different parts of your app, they all point to the same object. If one part of your app changes a property, it changes for **everyone**.

````swift
let carA = Car()
let carB = carA // carB points to the same object as carA

carB.speed = 100
print(carA.speed) // Output: 100 (carA was affected!)
````

This is why Swift encourages using Structs by default: they are safer because they don't have these "spooky action at a distance" side effects unless you explicitly ask for them using `inout`.