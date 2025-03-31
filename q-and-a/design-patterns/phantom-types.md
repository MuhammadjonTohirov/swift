---
description: Phantom Types in Swift
---

# Phantom types

Phantom types are a type system technique where a type parameter is used solely to create type distinctions but never appears in the data structure itself. In Swift, this typically involves generic parameters that don't appear in the actual implementation.

### Basic Concept

A phantom type parameter appears in the type declaration but not in any of the fields of the data structure:

```swift
swiftCopystruct Identifier<T> {
    let value: String
    
    init(_ value: String) {
        self.value = value
    }
}
```

Here, `T` is a phantom type parameter. The `Identifier` structure only contains a `String`, but the type parameter `T` creates distinct types at compile time.

### Real-World Use Cases

#### 1. Type-Safe Identifiers

```swift
struct User {
    let id: Identifier<User>
    let name: String
}

struct Product {
    let id: Identifier<Product>
    let title: String
}

func fetchUser(id: Identifier<User>) -> User? {
    // Implementation
}

// Usage
let userId = Identifier<User>("user-123")
let productId = Identifier<Product>("product-456")

// This works
let user = fetchUser(id: userId)

// This won't compile - type mismatch
// let user = fetchUser(id: productId)
```

This prevents accidentally passing a product ID to a function expecting a user ID, despite both being strings underneath.

#### 2. Units of Measurement

```swift
struct Distance<Unit> {
    let value: Double
}

enum Miles {}
enum Kilometers {}

extension Distance where Unit == Miles {
    func toKilometers() -> Distance<Kilometers> {
        return Distance<Kilometers>(value: value * 1.60934)
    }
}

// Usage
let driveDistance = Distance<Miles>(value: 10)
let hikeDistance = driveDistance.toKilometers()
```

This creates type safety for unit conversions.

#### 3. State Validation

```swift
struct Email<ValidationState> {
    let rawValue: String
}

enum Unvalidated {}
enum Validated {}

extension Email where ValidationState == Unvalidated {
    func validate() -> Email<Validated>? {
        if rawValue.contains("@") {
            return Email<Validated>(rawValue: rawValue)
        }
        return nil
    }
}

func sendEmail(_ email: Email<Validated>, content: String) {
    // Only validated emails can be passed here
}

// Usage
let unvalidatedEmail = Email<Unvalidated>(rawValue: "user@example.com")
if let validatedEmail = unvalidatedEmail.validate() {
    sendEmail(validatedEmail, content: "Hello")
}
```

This approach ensures that only validated emails can be passed to sensitive functions.

#### 4. Access Control Permissions

```swift
struct DatabaseConnection<Permission> {
    let connection: SQLiteConnection
    
    func query(_ sql: String) -> [Any] {
        // Execute query
        return []
    }
}

enum ReadPermission {}
enum WritePermission {}

extension DatabaseConnection where Permission == WritePermission {
    func execute(_ sql: String) {
        // Execute update statement
    }
}

// Usage
let readOnlyConnection = DatabaseConnection<ReadPermission>(connection: createConnection())
let adminConnection = DatabaseConnection<WritePermission>(connection: createConnection())

// This works for both
let results = readOnlyConnection.query("SELECT * FROM users")
let moreResults = adminConnection.query("SELECT * FROM users")

// This only works for the admin connection
adminConnection.execute("DELETE FROM users")

// This won't compile
// readOnlyConnection.execute("DELETE FROM users")
```

This pattern enforces compile-time permission checks.

### Benefits of Phantom Types

1. **Type Safety**: Prevent mixing of semantically different values that have the same underlying representation
2. **Self-Documenting Code**: Types clearly communicate intent and relationships
3. **Compile-Time Validation**: Errors are caught at compile time rather than runtime
4. **Zero Runtime Overhead**: Phantom types are erased during compilation, adding no runtime cost

### When to Consider Using Phantom Types

* When you need to distinguish between values of the same underlying type
* For creating API boundaries where type safety is important
* When modeling state transitions that should be validated at compile time
* For enforcing units of measurement or other dimensional analysis
* When implementing type-level state machines

Phantom types are particularly useful in larger codebases where maintaining strict type safety helps prevent bugs and clarifies intent across team members.
