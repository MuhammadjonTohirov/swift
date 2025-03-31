---
description: >-
  Protocol-Oriented Programming (POP) is a programming paradigm that Swift
  embraces as an alternative to traditional object-oriented programming. Let me
  explain it in a way that's clear for someone new
---

# Protocol-Oriented Programming

### The Basics

At its core, protocol-oriented programming uses protocols (similar to interfaces in other languages) as the fundamental building blocks for code organization, rather than classes or inheritance.

```swift
protocol Animal {
    var name: String { get }
    func makeSound()
}
```

### Key Concepts

#### 1. Protocol Extensions

One of Swift's most powerful features is the ability to extend protocols with default implementations:

```swift
extension Animal {
    func introduce() {
        print("Hi, I'm \(name)")
        makeSound()
    }
}
```

Now any type conforming to `Animal` automatically gets the `introduce()` method without having to implement it.

#### 2. Protocol Composition

You can combine protocols to build more complex behaviors:

```swift
protocol Flyable {
    func fly()
}

// A bird is both an Animal and Flyable
struct Bird: Animal, Flyable {
    let name: String
    
    func makeSound() {
        print("Tweet!")
    }
    
    func fly() {
        print("Soaring through the sky")
    }
}
```

#### 3. Value Types Over Reference Types

POP encourages using structs and enums (value types) rather than classes (reference types):

```swift
swiftCopystruct Dog: Animal {
    let name: String
    
    func makeSound() {
        print("Woof!")
    }
}
```

### Advantages Over Traditional OOP

1. **Avoiding the Inheritance Trap**: Inheritance can lead to deep, fragile hierarchies. POP favors composition.
2. **Better Reusability**: Protocol extensions allow you to share behavior across unrelated types.
3. **Value Semantics**: Using structs by default gives you predictable value semantics, avoiding unexpected sharing of state.

### Practical Example

Here's how you might use POP in a real Swift app:

```swift
protocol Persistable {
    var id: String { get }
    func save() throws
    static func load(id: String) throws -> Self
}

extension Persistable {
    func save() throws {
        // Default implementation that saves to UserDefaults
        let encoder = JSONEncoder()
        let data = try encoder.encode(self)
        UserDefaults.standard.set(data, forKey: id)
    }
}

struct User: Persistable, Codable {
    let id: String
    var name: String
    var email: String
    
    // We don't need to implement save() since we get it from the protocol extension
    
    static func load(id: String) throws -> User {
        guard let data = UserDefaults.standard.data(forKey: id) else {
            throw NSError(domain: "UserError", code: 1, userInfo: nil)
        }
        
        let decoder = JSONDecoder()
        return try decoder.decode(User.self, from: data)
    }
}
```

### When to Use POP

Protocol-oriented programming works best when:

* You need to share behavior across different types
* You want to avoid deep class hierarchies
* You need to conform to multiple "interfaces"
* You're working with the Swift standard library (which is itself built on POP)
