# 🧬 Handling Mutating Functions in Swift Enums (Enumerations)

**Enums** in Swift are also **value types**, just like structs, and they handle mutation in a very similar way: you must use the `mutating` keyword if a method needs to change the current case of the enum or any of its associated values.

## 1. Why `mutating` is Needed for Enums

An enum instance holds a single value representing one of its defined cases. When an instance method needs to switch the enum from one case to another, or modify an associated value within the current case, it is performing a **mutation** on the value type.

**Example: Traffic Light State**

A traffic light can change its state (color). To allow a method to transition the enum from one state to the next, it must be marked `mutating`.

````swift
enum TrafficLight {
    case red
    case yellow
    case green

    // This method changes the enum's 'self' value to a new case.
    mutating func nextState() {
        switch self {
        case .red:
            self = .green
        case .green:
            self = .yellow
        case .yellow:
            self = .red
        }
    }
}
````

## 2. Using mutating with Associated Values

Mutating methods are also necessary if you need to modify the **associated values** of the current enum case.

````swift
enum TaskStatus {
    case pending
    case inProgress(progressPercentage: Int)
    case completed
    
    // This method modifies the associated value of the 'inProgress' case.
    mutating func updateProgress(to newPercentage: Int) {
        if case .inProgress(let oldPercentage) = self {
            // Replace the current case with a new instance of the same case,
            // updating only the associated value.
            self = .inProgress(progressPercentage: newPercentage)
            print("Updated from \(oldPercentage)% to \(newPercentage)%.")
        } else {
            print("Task is not currently in progress.")
        }
    }
}
````

## 3. Usage Constraint
Just like with structs, you can only call a mutating method on an enum instance that has been declared with the `var` keyword.

````swift
var task = TaskStatus.inProgress(progressPercentage: 50)
task.updateProgress(to: 75) // Calls the mutating method successfully

// Let's change the enum case using the nextState example:
var light = TrafficLight.red
light.nextState() // light is now .green

// ❌ The following would cause a compiler error:
// let constantTask = TaskStatus.pending
// constantTask.updateProgress(to: 100) // Error: Cannot call mutating method on a 'let' constant
````